---
title: "Automating the deployment of this blog with github actions"
thumbnailImagePosition: left
thumbnailImage: /images/githubaction.png
metaAlignment: left
coverMeta: out
date: 2020-08-30T22:43:27+0900
showActions: false
categories:
- blog
tags:
- Hugo
- This site
- CI/CD
- github actions
- github pages
---

I automated the deploy process of this blog by using github actions. This site is built with Hugo and hosted by github pages. At first I did the deploy process manually, but now this site is automatically updated when I write a new post and push it to master.
<!--more-->

# Background
In Hugo, you can make a directory called `public` in which all the contents that are necessary to the site are constructed by Running the command `hugo`. You can host the website by placing these contents under the web server's root directory. If you use github pages as an hosting service, There are several options. In my case, I made a repository called `naotoman.github.io` (naotoman is my github username) and stored the contents under its root directory. 

I use the repository called `hugo-mywebsite` for pre-rendered contents of this site.
Therefore, I had to do these following processes in order to deploy the site after I wrote a post and pushed the change to master.
1. clean up unnecessary files under the `public` directory to remove deleted posts
1. run `hugo`
1. change the current directory to `public/`
1. commit and push the change to the master branch of `naotoman.github.io` repository.

# Automation
I automated the above process by using github actions. I wrote the following yaml file.

```yaml
name: rebuild-website

on:
  push:
    branches:
    - master

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: install-hugo
      working-directory: /tmp
      run: |
        wget https://github.com/gohugoio/hugo/releases/download/v0.74.3/hugo_0.74.3_Linux-64bit.deb
        sudo apt install ./hugo_0.74.3_Linux-64bit.deb
    - uses: actions/checkout@v2
      with:
        submodules: true
    - uses: actions/checkout@v2
      with:
        repository: naotoman/naotoman.github.io
        path: public
        persist-credentials: false
    - name: clean-public
      working-directory: ./public
      run: ls -A | grep -v -E '^.git$|^.gitignore$|^CNAME$' | xargs rm -rf
    - name: run-hugo
      run: hugo
    - name: deploy
      working-directory: ./public
      env:
        EMAIL: ${{ secrets.EMAIL }}
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
      run: |
        git config user.name "Naoto"
        git config user.email "$EMAIL"
        git add .
        git commit -m "Rebuild site $(date '+%Y/%m/%d %H:%M:%S')"
        git push https://naotoman:${ACCESS_TOKEN}@github.com/naotoman/naotoman.github.io.git
```

I will explain important parts of this config file.

To push to a different repository, `persist-credentials` should be set to `false` when you checkout that repository. In this case I wanted to push to `naotoman.github.io` repository. All the files and directories except `.git`, `.gitignore`, and `CNAME` have to be deleted before running `hugo`. You have to set an access token in order to let the CD runner to push to the repository.

# Related repos
- Build: https://github.com/naotoman/hugo-mywebsite
- Deploy: https://github.com/naotoman/naotoman.github.io