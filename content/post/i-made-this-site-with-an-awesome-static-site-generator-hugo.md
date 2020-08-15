---
title: "I made this site with an awesome static site generator Hugo!"
thumbnailImagePosition: left
thumbnailImage: /images/gohugo.png
metaAlignment: left
coverMeta: out
date: 2020-08-15T16:38:31+0900
showActions: false
categories:
- blog
tags:
- Hugo
- This site
---

This site is built with [Hugo](https://gohugo.io/). Hugo is written in Go and is one of the most popular open-source static site generators. Hugo allows you to create a rich static site easily. You don't have to be a professional front-end web developer or an experienced Go programmer. All you have to do is edit a toml file and write your posts as a markdown format.
<!--more-->

## Why did I chose Hugo?
### Comparing with WordPress
[WordPress](https://wordpress.com/) is used to create websites that contain dynamic contents. When I decided to start my own blog, I was going to use WordPress as I did in the past. But using WordPress may lead to some problems. My biggest concern was its cost. If you use WordPress, you have to pay for a hosting service like Bluehost, SiteGround, and so on. When I was running my blog with WordPress in the past, it costed about 500 yen (which almost equal to $5) per month. Although the service I used was relatively cheap, I couldn't stand the fact that I had to continue paying even when I stopped posting for a while.

Static sites don't need any databases or web application servers. Instead, it just needs a simple web server that just returns simple  files. You can host your website for free with a free static site hosting provider like [Github Pages](https://pages.github.com/), [Netlify](https://www.netlify.com/), and so on. And I realized that all the functionality I wanted for my site could be provided by a static site. That's why I left WordPress and chose a static site.

### Comparing with other static site generators
There are a bunch of other static site generators as listed in [this site](https://www.staticgen.com/). I didn't try all of them, but I did try [Gatsby](https://www.gatsbyjs.com/). As a result of my experimental use of Gatsby, I felt it was difficult for non-front-end web developers like me to handle. Gatsby is based on [React](https://reactjs.org/) and depends on a bunch of npm modules. When I tried to build the site with a little bit of configuration, I encountered an error related to those modules repeatedly. It seems Gatsby has some pros that Hugo doesn't have, but I quitted using Gatsby for that reason.

Hugo, on the other hand, is pretty easy to use. I have never needed any Go knowledge while using Hugo. In addition, Hugo has some advantages over Gatsby as below.
- Hugo build a site faster
- Hugo offers more themes

## Using Hugo
Basically you can build your first site with Hugo easily by following this [Quick Start](https://gohugo.io/getting-started/quick-start/). You can select a theme from [here](https://themes.gohugo.io/). Mine is this awesome [Tranquilpeak](https://themes.gohugo.io/hugo-tranquilpeak-theme/) theme! Deploying a site with Hugo is also easy. By using [Netlify](https://www.netlify.com/), the site will be automatically deployed after you push to master. You can also use [Github Pages](https://pages.github.com/) as a hosting provider like me.

## Conclusion
I explained why I chose Hugo and introduced Hugo. I intend to write a post about the configuration I did with Tranquilpeak theme in the future.

I leave the repos related to this site here.
- Build: https://github.com/naotoman/hugo-mywebsite
- Deploy: https://github.com/naotoman/naotoman.github.io
- Theme: https://github.com/naotoman/hugo-tranquilpeak-theme