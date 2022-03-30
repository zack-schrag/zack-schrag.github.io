---
layout: default
title: With Great Abstraction Comes Great Responsibility
categories: architecture
excerpt_separator: <!--more-->
---

In our house, my wife and I have a deal when it comes to chores. I hate doing laundry and she doesn't mind it, so she does all of our laundry. She hates doing dishes and I don't mind it, so I do all of our dishes. This deal works great for us, we each have full ownership over one chore and don't have to worry about the other chore.

<!--more-->

In order for this to work well, there are a couple things that need to happen. When it comes to laundry, I need to put my clothes in the laundry basket, and then I know that at some point those clothes will be cleaned and make their way back to our closet. If I don't put the clothes in the basket, there's no guarantee they will get cleaned and put away.

When it comes to the dishes, my wife puts the dishes in the sink, and she knows at some point they will be cleaned and put back into the cabinets. If the dishes don't make it to the sink, there's no guarantee they will get washed and put in the cabinets.

What does any of this have to do with abstraction? By now, you're probably starting to guess where I'm heading with this. What my wife and I have set up are two *abstractions*. The abstraction I interact with is the laundry basket. I input my clothes in the basket, and the outputs are clean clothes in our closet. The implementation details of cleaning laundry is abstracted away from me. I don't concern myself with how many loads she does, what type of detergent she uses, specific washer settings, etc.

The abstraction my wife interacts with is the sink. She inputs dishes into the sink, and the outputs are clean dishes in the cabinet. The implementation details of doing the dishes are abstracted away from her. She doesn't concern herself with the type of scrubber I use, the type of dish soap, the temperature of the water, etc.

## Unintended Consequences of Abstractions
**When creating abstractions, you (the developer) bring responsibility onto yourself to maintain the abstraction, whether you want it or not.** I've seen many cases (including things I've built myself) where an abstraction is built so that another developer (perhaps a new hire or a junior developer) doesn't have to think about the details of what's happening underneath. The intent of this is pure - save everybody's time by abstracting some tedious or tricky part of the code away into a tidy little wrapper.

The problem with this is more often than not, the developer does not want the responsibility of maintaining that abstraction, they just want to hack it together and hand it off. My guess is that many of you can relate. Have you ever created a little script and passed it around to other members of your team? Did anyone ever ask you questions about it on how to get it working for their use case? Or maybe they even asked you to add some feature to it to do something different? And in your head you think to yourself "Why did I ever create this stupid script?!? This was supposed to be a quick fix for one person, but now I have people asking me to add new features to it??"

The script is a very minor example, but let's take it a step further. As engineers, we *love* to create custom frameworks, don't we? We have testing frameworks, DevOps frameworks, framework for our frameworks, the list goes on and on. Sometimes, these frameworks can be excellent and meet a need. However, in other times, if we aren't thoughtful about our approach, we can end up frustrated like in the script example. Frameworks are one big abstraction, and if you aren't ready to take on the responsibility of maintaining it, answering questions, tweaking things endlessly, then do not build it! With great abstraction comes great responsibility.

## Handling Leaky Abstractions
Something worth noting is the "Law of Leaky Abstractions", coined by Joel Spolsky. You can read about it [here](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/). The TLDR is that all abstractions are inherently "leaky". I.e. they are not perfect and therefore require the consumer of the abstraction to be concerned about details that were intended to be abstracted away. For example, let's say I really drop the ball doing the dishes, and I put away all of our pots and pans without actually washing them. They have spaghetti sauce cooked into them, some of them have grease caked onto them. It's a real mess. To make matters worse, I just left for a business trip for two weeks. Let's imagine what that phone call might go like.

>
> **Wife:** How's your trip going?  
> **Me:** It's not too bad, how are things at home?  
> **Wife:** The dog is being really funny, I'll have to send you a video. Oh and I'm getting ready to cook dinner but I noticed all of the pans are dirty?  
> **Me:** Oh yeah sorry about that! I was pretty stressed when I did the dishes last so I must have put them away without washing them.  
> **Wife:** Ahh ok, I'll just clean them real quick. Where do you keep the sponge?  
> **Me:** Under the sink in the left side of the cabinet.  
> **Wife:** Perfect, do you like the city you're in?  
> **Me:** Yeah I like it a lot! Blah blah...

Seems reasonable, right? Luckily, she has done dishes before, so she will simply wash them herself and whip up a delicious stir fry for dinner. The implementation details of the sink abstraction have "leaked", but my wife was able to easily deal with it.

Now, let's consider a slightly different scenario. Let's say my wife had *never* done dishes before in her life. As far as she is concerned, it's all a bunch of magic that gets dirty dishes cleaned and put away. If we have the same situation above (I'm gone and the dishes are a mess), now it becomes a real crisis. Let's imagine what that phone call might look like:

>
> **Wife:** Hi, where do you keep the sponge?  
> **Me:** \*insert sarcastic tone\* Well hello to you to. Under the sink in the cabinet.  
**Wife:** There's 3 sponges. How am I supposed to pick the right one? You should just have one sponge. This is ridiculous. I called all of our friends and they all only have one sponge.  
> **Me:** Use any of them, I typically use the blue one because it can scrub a little better, but it really doesn't matter. Sorry that I like to have backups.  
> **Wife:** \*\*whispers under her breath\*\* *it's so dumb there are 3 sponges, how could anyone have ever know to pick the right one*. Ok I'll use that one. Do we own a dishwasher?  
> **Me:** No we don't. I do all the dishes by hand.  
> **Wife:** What?!? Why don't we have a dishwasher? Don't all modern houses have a dishwasher?   
> **Me:** Yeah most do, but we don't.  
> **Wife:** Why not?  
> **Me:** It hasn't been a financial priority lately.  
> **Wife:** Hmmpf, fine. I'll do them by hand I guess. We really need to figure out our budget so we can get a dishwasher.  
> **Me**: I mean, sure, we can talk about that. But even if we ordered one today it probably won't get installed until I'm back anyways.  
> **Wife**: Ugh, this is too complicated. How could anyone ever figure this out unless they've worked as a dishwasher at a restaurant. I'm going to order delivery.  
> **Wife:** And by the way, I can't beleive you left the dishes in such a mess. You really need to figure out how to not let this happen again.  
> **Me:** Well sorrrryyyyy! I'M NOT PERFECT! How have you NEVER done dishes before?  
> **Wife:** Oh I'MMM incompetent?!? YOU'RE the incompetent one! You can't even clean the dishes right and that's your job! The laundry gets done exactly right every week and you can't even wash a stupid pan!  
> **Me**: Well what about that one time where you stained my favorite shirt! Blah blah...

Two very different phone calls. One is quick, reasonable, and with one simple question answered, my wife can do the dishes. The other call, each step is painful and she questions everything about the process I have in place for doing dishes. Sound familiar? How many times has your team been fighting fires before a big deadline and there is the disgruntled voice questioning every decision ever made (this has been me *plenty* of times)? "Why did we do it this way? This is stupid. If only we did \*insert hot new technology here\* we wouldn't have this problem."

What is the root problem in the second phone call? It's simple, I created an abstraction (the sink), but I wasn't prepared to accept FULL responsibility of that abstraction. Sure, it's a little strange my wife had never done the dishes before, but by deciding to own the chore of doing the dishes, I implicitly took responsibility of the abstraction. Which, since my wife had never done dishes, meant I should have been prepared for a situation where the abstraction "leaked".

## Concealed vs. Convenience Abstractions
What's the point of all of this? There are two kinds of abstractions that we create, and it's important to be cognizant of which one(s) you are creating:  
- An abstraction where the consumer is not expected to know how things work underneath the abstraction. E.g. an electrician, your average homeowner doesn't know how wire things up but the electrician does. I like to call these **concealed abstractions**.  
- An abstraction intended to make the consumer's job *easier*, but they are still expected to know how things work underneath the abstraction. E.g. a power drill, you could still use a regular screwdriver because you understand how it works, but the power drill makes life much easier. I like to call these **convenience abstractions**.  

Dysfunction arises when expectations are mismatched for the type of abstraction. Perhaps you created an abstraction intending for it to be a convenience abstraction, but the consumer expected a concealed abstraction. In this situation, the consumer will be frustrated when the abstraction leaks, and you (the developer) will be frustrated they couldn't debug on their own. However, if it is understood from the beginning that it was intended as a concealed abstraction, then your reaction will be to fix the leak as fast as possible. 

Remember the script example from above? You intended for it to be a *convenience abstraction* but others treated it as a *concealed abstraction*. Had expectations been clearly set, you may have been able to avoid some frustration.

>
> **Note:** It's possible that one abstraction could be both. For some consumers, it might be concealed because they've never worked with the underlying technology. For others, it might be a convenience because they have worked extensively with the underlying technology.

Are you thinking of creating a new abstraction? Maybe a framework or a tool? Have you considered if you want it to be a convenience or a concealed abstraction? What happens when the abstraction leaks? Are you OK with being the point person? These are all questions that are good to consider before moving forward.

---
Abstractions are a fantastic tool for making life easier, but often they can be a catalyst for frustration and dysfunction in teams. In order to avoid this, it's important to **understand who you are creating the abstraction for, and be prepared for how to handle a situation when the abstraction leaks.** If we do that, we can help make life easier for the teams and organizations that we work in.
