---
published: false
title: Jq Data Processing Pipeline
layout: post
---
Often, when I read about _data processing_, data _pipelines_, _business intelligence_, and _big data_ I think many problems can be solved simply.

Here's how.

First, I split my problem into distinct responsibilities:

* get the data
* transform it
* store it

Most big _business intelligence_ problems fit into these steps. Sometimes I need to split the problem into multiple sections, but at some point this is what I have.

I can also think of this as a function with IO. Functions in ruby look like this 

```
def transform(get-date, where-to-store-it)
  (get-data)
  (do stuff)
  (where-to-store-it)
end
```

Since IO is my biggest performance constraint, I've tried to push the work down to tools written with perform in mind.

* Get the data: curl
* Transform: jq
* Store: linux pipe

I'm curious how many women are in computer science, software development, and related fields in each country now and over the past 10 years?


