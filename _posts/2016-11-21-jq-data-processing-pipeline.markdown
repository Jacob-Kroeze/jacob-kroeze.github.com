---
published: false
title: Jq Data Processing Pipeline
layout: post
tags: [jq, linux]
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

I'm curious about graduates in computer science, software development, and related fields in each country now and over the past 10 years?

https://stats.oecd.org seems reliable.

Here's a useful (oecd api) [http://stats.oecd.org/sdmx-json/data/RGRADSTY/AUS+AUT+BEL+CAN+CZE+DNK+FIN+FRA+DEU+GRC+HUN+ISL+IRL+ITA+JPN+KOR+LUX+MEX+NLD+NZL+NOR+POL+PRT+SVK+ESP+SWE+CHE+TUR+GBR+USA.905160.900000.900000.900000.900000.90/all?startTime=1998&endTime=2012]

