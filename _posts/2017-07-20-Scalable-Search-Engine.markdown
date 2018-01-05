---
layout: post
title:  "Create your own Scalable Search Engine"
date:   2017-07-20 00:00:00 +0530
categories: projects
author: "abhishek shah"
---

## A simple and scalable search engine

I had been working on Apache Kafka, Apache Storm, Apache Solr and Apache Flume. Created few apps with these and then I thought what could we achieve more out of this combination?

I have experience writing a web crawler in Python without using any frame work, learnt this at a course from Udacity Computer Science 101. When I was in college I wrote a web scraping module for a junior in Python using Beautiful Soup.

So things that I have with me.
* Streaming platform - [Apache Kafka](https://kafka.apache.org/http://storm.apache.org/)
* Real Time Analysis tool - [Apache Storm](http://storm.apache.org/)
* Data aggregator - [Apache Flume](https://flume.apache.org/)
* Indexing and Searching tool - [Apache Solr](http://lucene.apache.org/solr/)
* little experience in [Python](https://www.python.org/) 2 & 3.
* Web Parsing - [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/)
* And I know what I'm doing.

I can make a scalable search engine for the web. I know it will take a lot of time to index the whole web. But what's the hurry?

If you are not familiar with any of the above technology - then there's a good news, I'll be writing articles on each one of those as much as we are able to start working on this project. I believe I would take a year to build the things from bottom up. If you know or have learnt any one of the technology and would like to contribute, please make a pull request on github. More on the contribution later,  most probably at the home page of the project at github.

## Step One - web scraping
Kafka is a streaming platform and we will use it during web crawling. One of the Kafka topic will have only the URLs to be crawled.

Let's name this Topic - `TO_BE_SEARCHED`. We will start with a good seed page. A good seed page is web page which has lot of unique external URLs. So, our Kafka topic will have the URL of the seed page.
Our Storm topology will have bolts and spouts. A Kafka spout to be specific, because it will read the URLs from the Kafka topic. Spout reads the URL and will pass it to the bolts. Right now I can foresee, we need minimum two bolts, one will scrap the web page at the URL passed from the Kafka spout. After scraping the html document at the URL, the bolt will pass the unique URLs found at this page to the other bolt. This bolt B will put all the URL into Kafka topic `TO_BE_SEARCHED`. This will act as a kind of stack.

**Now something very important, we want to crawl the URLs in breadth first fashion**. Because a seed page may have too much of internal links, we don't want to keep ourself too much busy with indexing one page, we want to cover the entire web. But definetly we will cover all the internal URLs of the seed page, though later.
So, inserting in the Kafka topic will be in accordance to breadth first search fashion.

So, by now we have a Kafka Spout which reads the URLs from Kafka topic. URL is passesd to Bolt A - which parses the html document. Bolt A passes unique URLs to Bolt B - which puts back in Kafka topic.

## Step Two - Integrations



Repository created at github already. It can be found [here](https://github.com/ashah2012/scalable-search-engine).
