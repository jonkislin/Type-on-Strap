---
layout: post
title: "Traversing the Trump Twitterverse"
excerpt_separator: "<!--more-->"
published: true
---
*Note: Traversing the Trump Twitterverse was heavily inspired by Tim Martin's awesome [Promoting Positive Climate Change Discussions on Twitter](https://zeromh.github.io/climate_change_conversations/).*

In March 2018, I spent most of my waking time collecting, organizing, and modeling 3.4 million tweets directed at @realDonaldTrump.

### ...Why?
Well, I wanted to investigate whether the users generating these tweets fell into [ideologically disparate communities](http://senseable.mit.edu/community_detection/).

### How'd you do that?
I used community detection and topic modeling algorithms. I was particularly interested in identifying the users that were talking the most across community lines, [minimizing the path distance between disparate groups](https://www.geeksforgeeks.org/betweenness-centrality-centrality-measure/).

<!--more-->

### Interesting. What inspired you to do this?
Political polarization is... a bit intense these days: <br><br>

<a>
<img src="http://assets.pewresearch.org/wp-content/uploads/sites/5/2014/06/PP-2014-06-12-polarization-1-02.png"
alt="IMAGE ALT TEXT HERE" width="1000" border="10" />
</a>

Take a look at a graph fellow Metis Alum Tim Martin made of [twitter users talking about climate change](https://zeromh.github.io/climate_change_conversations/). The small blob on the right is a community of climate change deniers: <br><br>

<a>
<img src="https://zeromh.github.io/images/network_deniers_circled.png"
alt="IMAGE ALT TEXT HERE" width="1000" border="10" />
</a>

This one from an [MIT study](https://news.vice.com/en_us/article/d3xamx/journalists-and-trump-voters-live-in-separate-online-bubbles-mit-analysis-shows) is particularly elucidating (though as we'll see, a lot of this may be bot-noise): <br><br>

<a>
<img src="https://vice-prod-news-assets.s3.amazonaws.com/uploads/2016/12/TwitterData1-01.png"
alt="IMAGE ALT TEXT HERE" width="1000" border="10" />
</a>

So, there's definitely a lot out there on polarization and twitter. Given Donald Trump's notoriety for using Twitter as a platform, I wanted to see what the landscape of users interacting with the President - and more importantly, each other - would look like.

## Method
### Tweet collection
Some basics to figure out:
- Should we use the Twitter REST API or the Twitter Streaming API?
- What should our sample of tweets look like? What should our query to the API be?

<a>
<img src="/assets/img/tmux_setup.png"
alt="Remotely connected to an EC2 c5.9xlarge instance on the left. Local machine prompt on the right." width="10000" border="5" />
<em>Remotely connected to an EC2 c5.9xlarge instance on the left.<br> Local machine prompt on the right.</em>
</a>

#### Which API to use?
I had wanted to capture tweets from specific events that might elicit mixed responses from both liberals and conservatives. It would have been great, for example, to see tweets during the immediate time period when James Comey or Rex Tillerson were fired. Unfortunately, access to large quantities of historical tweets is pricey, and third party APIs that seek to bypass twitter's restrictions are sketchy at best. Due to the volume of tweets I wanted (around 1 million), I chose the streaming API and let it run until I got to 3.5 million tweets - about two business weeks of runtime, including some random interruptions.
<!-- (Did I need to focus on events? There are so many what ifs here!) -->

#### Tweet Sample
First, I toyed with the idea of using two queries - one that would capture people using politically
meaningful hashtags (map ideology), and one that would capture tweeting about musical artists and see which artists would minimize the path distance between people on other sides of the ideological spectrum. Honestly, that sounds like a good project - if you're interested in bringing people
together, find the celebrities who have the power to do so. But this isn't the project I ended up doing.

That proved a little too complicated for the time constraints, so I stuck with a simpler idea: let's just get as many tweets as we can referencing @realDonaldTrump. For inclusion in our query, the tweet would only have to include that 'mention.'

The query to capture the sample was as simple as this:
```python
## Query, placed at top of script for easy modification:
## (See final code block)
QUERY = ['@realDonaldTrump'] # list of strings to track
# boolean logic works within strings
```
For the full script: [A Simple Twitter API Script](/2018/05/05/streaming_script.html)

### Constructing the Network
- Each user will be a "node" in our graph. What will be the edges between them?

#### Edges (the links between users)
The edges, instead of being just retweets, just mentions, or just quotes, I decided, would be any interaction between users. By the nature of our query ("@realDonaldTrump"), everyone would have an interaction with the President's twitter account. However, very often people include other mentions in their tweets. This is how, later, we'll build our network. So, to sum things up:
- Each user is represented by a *node* (point, dot, vertex). A node for a user is generated as soon as that user tweets, or has been mentioned by another user. There are no duplicate nodes.
- Each "mention", "retweet", or "quote tweet" embedded within one tweet, or across multiple tweets, is an *edge*, connecting the mentioning user to the user being mentioned.

<a>
<img src="/assets/img/nodes_and_edges.png"
alt="Each circle is a user. Each line is an interaction (mention, retweet, quote-tweet, etc.)" width="1000" border="10" />
<em> <br>Each circle is a user. Each line is an interaction (mention, retweet, quote-tweet, etc.) </em>
</a>

### Visualization?
Before diving into Network metrics, I really wanted to get a visual grasp of the network that could be generated by my tweets. Tweets, as user-generated text data, are notoriously messy. From 3.36 million tweets, about 2 million were discarded.
Take a look at [my preprocessing notebook](https://github.com/jonkislin/traverse/blob/master/code/2-tweet_preprocessing.ipynb) for details (heavily adapted from Tim Martin's analogous notebook).

LINK TO TWEET PREPROCESSING NOTEBOOK
LINK TO NETWORK ANALYSIS NOTEBOOK
LINK TO node.js SCRIPT

### Analyzing the Network
Analyzing the net
#### Community detection
#### Topic Modeling tweets
#### Network Metrics

## POST UNDER CONSTRUCTION - TO DO:

### Getting down to it
- ~~Tweet collection, storage, preprocessing~~
- ~~Network building~~
- Visualization
- Network metrics - finding the moderators
- Topic Modeling
- Why LDA?

### What I learned
- twitter can be pretty nasty

### Where to next
- The Electome
