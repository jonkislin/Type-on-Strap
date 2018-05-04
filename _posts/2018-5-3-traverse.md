---
layout: post
title: "Traversing the Trump Twitterverse"
excerpt_separator: "<!--more-->"
published: true
---

For my final project at Metis in NYC, I decided to collect as many tweets as I could referencing @realDonaldTrump on twitter. I wanted to find out how users interacting with the president interacted with each other, and model groups of these users into [ideologically disparate communities](http://senseable.mit.edu/community_detection/) by way of community detection and topic modeling. Most importantly, I wanted to find out which users were talking the most across community lines, [minimizing the path distance between disparate groups](https://www.geeksforgeeks.org/betweenness-centrality-centrality-measure/).

<!--more-->

### Why do this?
Political polarization is... a bit intense these days:

<a>
<img src="http://assets.pewresearch.org/wp-content/uploads/sites/5/2014/06/PP-2014-06-12-polarization-1-02.png"
alt="IMAGE ALT TEXT HERE" width="830" border="10" />
</a>

Take a look at a graph fellow Metis Alum Tim Martin made of [twitter users talking about climate change](https://zeromh.github.io/climate_change_conversations/). The small blob on the right is a community of climate change deniers:


<a>
<img src="https://zeromh.github.io/images/network_deniers_circled.png"
alt="IMAGE ALT TEXT HERE" width="830" border="10" />
</a>
