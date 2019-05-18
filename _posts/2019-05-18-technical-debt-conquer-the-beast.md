---
layout: post
title:  "Technical debt: conquering the beast"
date:   2019-05-18 23:00:00 +0700
categories: Engineering,Culture,Lead
permalink: "/technical-debt-conquer-the-beast"
---
"The project is messy". "Implementing this feature will require a massive change". "Can't do X without breaking Y". Strange bugs appear in features no one has touched in a while. Team members start talking about rewriting the entire thing. Familiar? That's too much tech debt. Your team probably needs to get refactoring — and it may be easier than you think...
<!--more-->

# So what is technical debt anyway?
I won't try to define it here. Martin Fowler [put it](https://martinfowler.com/bliki/TechnicalDebt.html) better than I could:
> Technical Debt is a wonderful metaphor developed by Ward Cunningham[...]. In this metaphor, doing things the quick and dirty way sets us up with a technical debt, which is similar to a financial debt. Like a financial debt, the technical debt incurs interest payments, which come in the form of the extra effort that we have to do in future development because of the quick and dirty design choice

Now like financial debt, technical debt is not the problem. Just like there's a [healthy level of financial debt](https://www.business.com/articles/healthy-business-debt/), there's a healthy level of technical debt.

The problem is not about _having_ tech debt: it's about having _unmanaged_ tech debt. This is what we're going to explore next.

# What causes too much tech debt?
Almost every software engineer with some experience has been through this. The codebase is getting bigger, more messy, harder to maintain. New features start taking longer and longer. Even small changes can some times cause a major headache. 

I've been in a fair number of projects like that. When given the opportunity to lead the mobile team at Mozio, I knew I had to do better. So the first thing I did was research the cause of the problem. My findings:

## Too much focus on _delivering_ or _shipping_
"We need to deliver this product quickly". "Our company needs this feature shipped very fast". Especially in a startup that is burning cash fast and needs to find its market, it's common to focus every effort into delivering products or features. The very survival of the company depends on it.

But having 100% focus on delivering new features/products leaves 0% time to do housekeeping.

## "Business people need results, not a clean codebase"
That's one I heard quite a lot. Hey, we're not here to create an "us vs them" situation. Business faces pressure from a variety of directions: investors in need of results, competition, etc. It's up to the technical lead to sell the fact that a manageable codebase is just as important as delivering new features. Decisions tend to be biased to the business side of things simply because there is a power imbalance in play: the business areas tend to hold more power than the engineering areas.

## Not embracing technical debt
A tech team needs to have the right engineering culture to work smoothly. And embracing tech debt is an important — but often neglected — part of it. Here's what "embracing" tech debt means to me: it may sound counter intuitive, but it's important that the team knows it's **fine to create tech debt**. If tech debt is viewed as a problem, engineers may try to design something that is perfect from the start. Not only this will make things slower, it will also create tech debt disguised as flexibility.

Remember: tech debt we don't know about is the worst kind of tech debt. More on that later.

Now we understand the causes, let's talk about steps your team can take to improve...

# Conquering technical debt

## How technical debt accumulates
More often than not, excess technical debt is a self-inflicted wound. Teams won't dedicate enough time to clean up and improve simply because they set an unrealistic expectation of productivity. This works well in the beginning: building things from scratch, there is nothing to get in the way. The team delivers fast, and cares little about refactoring and improving. Business is happy — why change?

Stretching the housekeeping analogy a bit: a restaurant promises to deliver food in up to 25 minutes or it's free. Cooking anything on their menu takes no more than 10 minutes. Easy, right? So they assign everyone in the kitchen to cooking. Initial customers all get their food ahead of time. But no one is doing the dishes. There will come a point where cooking will require to first find the adequate pan in a pile of dirty pans, wash it, and _then_ start cooking. A bottleneck was created: no one can cook until they wash a pan. But there's not enough space for many to be washed simultaneously. That restaurant will be out of business soon.

Simply assigning a certain number of people to cleaning the pans would have saved that restaurant, of course. Software is no different. The main difference is how visible those problems are. Of course no one would run a restaurant like that. Yet many software teams are run exactly like that.

## First, a small change of mindset may be needed...
If it's present, **completely remove the notion that cleanups and refactors are wasted time**. They are just as important, or arguably more so, than new code. Everyone involved (engineers, business people...) needs to understand that. The initiative doesn't have to come from a tech lead — any individual engineer can start pushing for that. It will need to be adopted by the lead eventually, of course.

## ... Then, expectations need to be adjusted
Now that cleanups and refactors are acceptable and expected, the productivity expectation needs to be corrected. As projects grow, they become more complex. But when the team is set up to spend enough time improving, refactoring and cleaning up, that complexity can be managed. **Teams need to budget time for refactors and improvements**. This is the only way those will ever happen.

As one can imagine, this will mean less _features_ are being worked on, because the team is also working on improvements. And that is fine — expected. Make sure this new productivity level is communicated and understood. Things will not be as fast as in the beginning. But they will keep moving, and with enough refactoring, not so many new features will be delayed unexpectedly because they hit ugliness left behind.

## And finally, embrace technical debt
Remember how "tech debt we don't know about is the worst kind of tech debt"? That is because it tends to show up only when it has become a problem. Had it been paid earlier, it would have had less negative impact.

So how do we make sure tech debt is accounted for? By embracing it. **Accept that tech debt is inevitable, let it happen and document it**. That achieves two results:
- By documenting tech debt as it is created, engineers don't need to carry the burden. They can take that shortcut if it's reasonable for the current task, but document it so it's not forgotten. Create an issue in the source control platform. Add an entry to the project management tool. Anything. Just make sure a detailed description of the tech debt is present and easy to find when planning work. 
- Documented tech debt can be paid at any time, not only when it becomes a problem. Remember how the engineer took that shortcut and left something not so great? No? Neither did I, but it's documented. So let's improve it now that this important feature has been delivered.

When the team embraces technical debt, it can move faster. It's OK to create some less-than-optimal solutions, because by documenting it and budgeting time, we can rest assured knowing it will be improved soon.

# Last thoughts
Most of the time, excessive technical debt, messy code and unmaintainable projects are self-inflicted. With the hope of delivering fast, little care is dedicated to cleanups and refactors, which causes many issues along the way. Things start taking longer, engineers start to become unhappy, etc. But with a little planning and discipline, tech debt can be conquered and become an important ally. Embrace it!

# Note
Those observations and suggestions are, of course, based on my experience. Your mileage may vary. Along with real world experience, much of this came directly or indirectly from books. One that comes to mind is [The Phoenix Project, by Gene Kim](https://www.goodreads.com/book/show/17255186-the-phoenix-project), which I strongly recommend. I have no connection with the authors.