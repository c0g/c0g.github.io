---
date: 2013-10-24
tags: python
layout: post
title: A pretty Python
---
I knocked this out and was quite happy:
```python
    def F500():
        with closing(urlopen("http://money.cnn.com/magazines/fortune/fortune500/2013/full_list/index.html")) as f:
            document = html.parse(f).getroot()
            names = [span.text for span in document.find_class("name")]
            symbols = {name: YQLTicker.symbols_from_name(name) for name in names}
            return symbols
```         
The function downloads a webpage containing a list of the Fortune 500 companies then uses Yahoo's APIs to get the stock symbols corresponding to each one. It returns a dictionary where the keys are the companies' names and the values are an array of the ticker symbols. 

It's rare that I manage to write code that needs more text to describe than compute.