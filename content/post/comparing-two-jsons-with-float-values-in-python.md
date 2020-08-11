---
title: "Comparing two JSONs with float values in Python"
thumbnailImagePosition: left
# thumbnailImage: //d1u9biwaxjngwg.cloudfront.net/cover-image-showcase/city-750.jpg
metaAlignment: left
coverMeta: out
date: 2020-08-10T18:15:27+09:00
showActions:    false
categories:
- blog
tags:
- Python
- Algorithm
---

JSON objects are often used while we write programs. 
I will introduce the way to compare two JSONs with float values for approximate equality in Python.
<!--more-->

## Background
I encountered the situation where I have to compare two json objects for equality in Python.
But there was a problem.
Each json objects may include float values and I wanted to assume two JSONs are equal if there is only a slight difference between those float values.
As below, it's cannot be done with Python's builtin `==` operator.

```python
js1 = {"a": 1.0000000000}
js2 = {"a": 1.0000000001}
assert js1 == js2  # False
```

Python also has builtin `json` module, but there seems to be no functions that can be used for this purpose.
So, I decided to create a function for my own.

## Definition of (approximate) equality
Before we go, we have to define (approximate) equality of float values.
So in this time, two float values are assumed to be approximately equal if these are strictly equal when being rounded to a certain number of significant figures.
For example, if we round to three significant figures, We get the following results.

|A|B|A equals B|
|:-:|:-:|:-:|
|1.001|1.002|False|
|1001.0|1002.0|False|
|1.0001|1.0002|True|
|1.0001|1.0005|False|
|10001.0|10005.0|False|
|0.000001|0.000002|False|

## Solution
I created this function to achieve those goals.

```python
from typing import Optional, Union

JsonType = Optional[Union[dict, list, str, float, int]]


def are_jsons_approx_equal(js1: JsonType, js2: JsonType, precision: int) -> bool:
    """Compares two json objects and returns True if these are approximately equal.

    Float values are rounded to the significant figures specified as `precision`
    when being compared.

    Args:
        js1: json object to compare
        js2: json object to compare
        precision: significant figures applied to float values

    Returns:
        True if two json objects are approximately equal.

    Raises:
        TypeError: if `js1` or `js2` is not a valid json object.
    """

    def _float_to_str_in_json(js):
        if type(js) in (str, int, bool, type(None)):
            return js
        if type(js) is float:
            return f"{js:.{precision}e}"
        if type(js) is list:
            return [_float_to_str_in_json(x) for x in js]
        if type(js) is dict:
            if any([type(key) is not str for key in js.keys()]):
                raise TypeError("One of key types is not 'str'.")
            return {key: _float_to_str_in_json(val) for key, val in js.items()}
        raise TypeError(f"Value of type '{type(js)}' is not a valid json object.")

    new_js1 = _float_to_str_in_json(js1)
    new_js2 = _float_to_str_in_json(js2)

    return new_js1 == new_js2
```

This function returns `True` if `js` and `js2` are approximately equal.
If Neither of them includes float values, if just returns the result of `js1 == js2`.
You can specify significant figures as `precision`.

In addition, this function raises `TypeError` if one of `js1` or `js2` is not a valid json object.
For example, if the object has values of type `numpy.int64`, `set`, or keys of type `int`, it raises exception.

More about Json 
- https://www.json.org/json-en.html
- https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON
