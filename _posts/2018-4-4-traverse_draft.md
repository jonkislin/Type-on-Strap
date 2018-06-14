---
layout: post
thumbnail: "assets/img/left-right.png"
feature-img: "assets/img/trump_scc.png"
title: "Traversing the Trump Twitterverse"
excerpt_separator: <!--more-->
published: false
---
Final project at Metis Data Science Bootcamp. Blog post in progress.  
[Click here](https://github.com/jonkislin/traverse) for a link to my project repo.

This project was heavily inspired by Tim Martin's excellent [Promoting Positive Climate Change Discussions on Twitter](https://zeromh.github.io/climate_change_conversations/).

Visualizations made possible by [Andrei Kashcha](https://github.com/anvaka/pm).

<!--more-->
### Motivation:
Some thoughts from [Dan Hopkins at 538](https://fivethirtyeight.com/features/political-twitter-is-no-place-for-moderates):  
> ....if someone tweets about politics, they are likely to label themselves as on the edges of the American ideological distribution. People who deem themselves “very conservative” or “very liberal” seem to be telling us as much about their level of political engagement as about their policy positions.

Some more thoughts:   

>We asked a [representative sample] of American adults about their Twitter usage and found that as of November/December 2016, just 12 percent of our respondents reported using the social media platform. So Twitter posters are already a distinct minority, and even among those who do tweet, users who routinely tweet about politics are an even smaller and more atypical minority. Political tweeters are even more polarized than the nation as a whole.

Can we find the moderates on twitter?
Where would we find them?

### Goals
Going into this project, I really wanted to make a visualization that showed ideology-based groupings
of twitter users, and be able to point out members that stand in between clashing groups. I didn't really
have a good idea of how to do that - I knew the possibility was there, but I wasn't sure how graph networks worked or how layout-generation algorithms or community detection worked.
Being able to follow along with Tim's notebooks (see link above) was a godsend, but there were still a few learning curves to climb:

- mongoDB (tweet storage)
- twitterAPI scripting (tweet collection)
- Docker, Amazon EC2, and PySpark (modeling 3.5 million tweets would have been infeasible with just a MacBook Pro)
- SQL (Tim opted for SQL rather than Pandas for most of his Exploratory Data Analysis, and I wanted to brush up, so I stuck with this. Check out https://pgexercises.com for a great collection of SQL practice exercises)
- basic JavaScript and node package manager (needed this to run my data through Andrei's awesome PM visualization - link also above)


### Tweet collection
Some basics to figure out:
- Should we use the Twitter REST API or the Twitter Streaming API?
- What should our sample of tweets look like? What should our query to the API be?
- Each user will be a "node" in our graph. What will be the edges between them?

#### Which API to use?
I had wanted to capture tweets from specific events that might elicit mixed responses from both liberals and conservatives. It would have been great, for example, to see tweets during the immediate time period when James Comey or Rex Tillerson were fired. Unfortunately, access to large quantities of historical tweets is pricey, and third party APIs that seek to bypass twitter's restrictions are sketchy at best. Due to the volume of tweets I wanted (around 1 million), I chose the streaming API and let it run until I got to 3.5 million tweets - about two business weeks of runtime, including some random interruptions.

<!-- (Did I need to focus on events? There are so many what ifs here!) -->

#### Tweet Sample
First, I toyed with the idea of using two queries - one that would capture people using politically
meaningful hashtags (map ideology), and one that would capture tweeting about musical artists and see which artists would minimize the path distance between people on other sides of the ideological spectrum. Honestly, that sounds like a good project - if you're interested in bringing people
together, find the celebrities who have the power to do so. But this isn't the project I ended up doing.

That proved a little too complicated for the time constraints, so I stuck with a simpler idea: let's get as many tweets as we can referencing @realDonaldTrump. For inclusion in our query, the tweet would only have to include that 'mention.'

Finally,

<!-- can twitter queries be more than just text searches? -->

### Analysis
- How will we model this social network?
- How will we categorize users into "communities"?
- By what metrics will we identify "moderators" and "influencers"?

### Conclusions


### Assumptions?
