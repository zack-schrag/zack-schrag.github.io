---
layout: default
title: Why Software Development is Just Like Fantasy Football
categories: software
tags: software decision what-if risk
excerpt_separator: <!--more-->
---

Matthew Berry, one of fantasy football's most prominent voices, wrote this in his [Draft Day Manifesto article](https://www.espn.com/fantasy/football/story/_/page/draftdaymanifesto19/fantasy-football-draft-day-manifesto-how-draft-your-fantasy-football-team-matthew-berry-way) in 2019:

<!--more-->

> At a fundamental level, fantasy football is entirely about minimizing risk and giving yourself the best odds to win on a weekly basis.

He goes on to talk about how unpredictable fantasy football is, and lists a handful of examples that highlight it. And then another gem: 

> Since you won't know what is definitely going to happen, all we can do is try to predict what's most likely to happen.

Anyone who has spent much time at all playing fantasy football will agree with these quotes. Using these principles, I can try to make the best decision for my team based on what's most likely to happen. And if the unlikely event does happen, it doesn't mean I made the wrong decision. It just means I couldn't predict the future. **I made the best decision I could with the information I had at the moment.** 

In my first year playing fantasy football, I thought I would be clever and draft "nobodies" that I hoped would end up being superstars. Unsurprisingly, I got last place that year. Why? Because I was gambling and hoping I'd hit the jackpot, not consider what's most likely to happen.

What does this have to do with software development? Let me change Matthew Berry's first quote slightly:

> At a fundamental level, decision-making in software is entirely about minimizing risk and giving the business the best odds to consistently win in the marketplace.

And I don't even need to change the second quote:
> Since you won't know what is definitely going to happen, all we can do is try to predict what's most likely to happen.

Fits pretty nicely, right? Let me elaborate with an example. In one of my jobs, we (the engineers) were having a meeting with a Product Manager, discussing requirements towards the end of the project. He was typing acceptance criteria for a Jira ticket, and added a bullet point that seemed insignificant at the surface, but would drastically change the scope of the project. The PM's reasoning for wanting this requirement was essentially "what if the business needs it later?" Internally, my brain looked something like this: 🚩🚩🚩🚩🚩😬😬😬🚩😬🚩

And then this:  
![Michael Scott No](https://media.giphy.com/media/12XMGIWtrHBl5e/giphy.gif)

So, what's most likely to happen? Considering no discussions happened yet with the business, they probably won't need that functionality. And, even if they did, it probably will take a different shape. We explained why this requirement was significant work, and we ended up decided to delay building that piece of functionality. I left the company prior to finding out if it was truly "needed" later. Either way, we made the best decision we could with the information we had available to us at the time.

In technology, we love asking "what if (insert some unlikely scenario here)?". Sometimes those are great questions worth asking, but other times they can just be distracting. 

At another job, I was in charge of building a service to integrate with an AWS service for our system prior to our product launch. When I was demoing, I was asked "what if the AWS service goes down?". I responded with "how often does that service go down?". I was trying to convey that the cost of implementing a solution that could handle an AWS outage was not worth it compared to a simpler, faster solution that didn't. I argued, if we find the AWS service goes down more frequently than we like, we can build for that solution later. I believe that original solution is still in place today, and as far as I know, there have been no outages for that AWS service (years later). 

In fantasy football, the equivalent would be starting Alexander Mattison over Derrick Henry** because "what if he scores more points?". It *might* happen, but it's not the *most likely* thing to happen.

> ** If you don't know NFL players, insert "mediocre player you'll probably be disappointed with" and "superstar player who can win you a championship" for "Alexander Mattison" and "Derrick Henry", respectively.

---
Don't let irrelevant "what ifs" derail your project. Remember your fantasy football principles! Ask yourself: 
- How can we minimize risk?
- What's most likely to happen?  

I'm convinced that asking those 2 questions constantly will provide immense value and focus to any software team, just like it will help you win a fantasy football championship.
