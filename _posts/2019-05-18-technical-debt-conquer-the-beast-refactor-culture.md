---
layout: post
title:  "Technical debt: conquer the beast"
date:   2019-05-18 18:00:00 +0700
categories: Engineering,Culture,Lead
permalink: "/technical-debt-conquer-the-beast"
---
"The project is messy". "Implementing this feature will require a massive change". "Can't do X without breaking Y". Strange bugs appear in features no one has touched in a while. Team members start talking about rewriting the entire thing. Familiar? That's too much tech debt. Your team probably needs to get refactoring - and it may be easier than you think...
<!--more-->

# So what is technical debt anyway?
I won't try to define it here. Martin Fowler [put it](https://martinfowler.com/bliki/TechnicalDebt.html) better than I could:
> Technical Debt is a wonderful metaphor developed by Ward Cunningham[...]. In this metaphor, doing things the quick and dirty way sets us up with a technical debt, which is similar to a financial debt. Like a financial debt, the technical debt incurs interest payments, which come in the form of the extra effort that we have to do in future development because of the quick and dirty design choice

Now like financial debt, technical debt is not the problem. Just like there's a [healthy level of financial debt](https://www.business.com/articles/healthy-business-debt/), there's a healhy level of technical debt.

The problem is not about _having_ tech debt: it's about having _too much_ tech debt. This is what we're going to explore next.

# What causes too much tech debt?
Almost every software engineer with some experience has been through this. The codebase is getting bigger, more messy, harder to maintain. New features start taking longer and longer. Even small changes can some times cause a major headache. 

I've been in a fair number of projets like that. When given the opportunty to lead the mobile team at Mozio, I knew I had to do better. So the first thing I did was research the cause of the problem. My findings:

## Too much focus on _delivering_ or _shipping_
"We need to deliver this product quickly". "Our company needs this feature shipped very fast". Especially in a startup that is burning cash fast and needs to find its market, it's common to focus every effort into delivering products or features. The very survival of the company depends on it.

But having 100% focus on delivering new features/products leaves 0% time to do housekeeping.

Let's stretch the housekeeping analogy a bit: boiling an egg takes about 10 minutes. But if you never do your dishes, there will come a point where boiling an egg will require you to first climb through a pile of dirty pans and plates. You now have to: find the adequate pan in the pile, reach it (potentially breaking something else in the process), wash it, dry it, and _then_ boil your egg. A 10-minute task now takes maybe 30min. Sure, you could maybe live with that. A restaurant could not.

## "Business people need results, not a clean codebase"
That's one I heard quite a lot. Hey, we're not here to create an "us vs them" situation. Business faces pressure from a variety of directions: investors in need of results, competition, etc. It's up to the technical lead to sell the fact that a manageable codebase is just as important as delivering new features. Decisions tend to be biased to the business side of things simply because there is a power imbalance in play: the business areas tend to hold more power than the engineering areas.

## Not embracing technical debt
A tech team needs to have the right engineering culture to work smoothly. And embracing tech debt is an important - but often neglected - part of it. Here's what "embracing" tech debt means to me: it may sound counterintuitive, but it's important that the team knows it's **fine to create tech debt**. If tech debt is viewed as a problem, engineers may try to design something that is perfect from the start. Not only this will make things slower, it will also create tech debt disguised as flexibility.

Remember: tech debt we don't know about is the worst kind of tech debt.

Now we understand the causes, let's talk about steps your team can take to improve...

# Conquering techical debt
Let's talk about the causes one by one: 

## Too much focus on delivering
More often than not, this is a self-inflicted wound. A lot of the time, teams won't dedicate enough time to clean up simply because they set an unrealistic expectation of productivity. This works well in the beginning: building things from scratch, there is no tech debt to get in the way. The team delivers fast, and cares little about refactoring and improving. Business is happy - why change?

But then it catches up. Engineers were too focused on deliverying new things and spent little to no time cleaning up. Didn't pay much attention to the business and the actual product aside from the imediate goals. The project started to accumulate workarounds on top of workarounds. How did this happen!? We were so careful with the code we wrote...