---
title: "The Senior Engineer's Guide: Document Your Intent - An Anecdote"
excerpt: "Holy crap, dude, that's not what I meant!"
classes: wide
tags:
  - software
  - career
  - senior-engineers-guide
---

In the fall of 2015, Peloton's tax partner, Avalara experience an outage on Black Friday, during which their tax calculation API was entirely down. Peloton used this API to determine that amount of tax due for an order in a given postal code for a given basket of goods, which is more complicated than it sounds because it considers a combination of state and local taxes, which includes the type of products in the order (clothes and exercise equipment may be taxed at different rates, for example), and it tracks the aggregate amount of tax the company owes to each state in which it operates.

Peloton, for reasons I won't get into here, had spun up its own homegrown e-commerce platform instead of going with something more out-of-the-box like Magento, and the software wasn't perfect. During this outage, the checkout software essentially prevented customers from placing orders because we couldn't retrieve a tax calculation.

After that Black Friday outage I was tasked with writing what we called a "kill switch" to disable tax calculation, setting it to $0 and marking orders with a flag that said there was a tax outage. I was specifically told to write this manual switch instead of an automatic circuit breaker because business wanted to be the ultimate decision maker, and with this flag we hoped to possibly collect the outstanding tax from Avalara.

This seemed like a real long shot to me, and it wasn't ever possible in the end because Avalara, like all smart API providers, don't hold themselves liable for losses. I didn't want to do it that way, but the CTO and my boss both acknowledged the technical shortcomings after I raised my concerns, and we went forward with the manual switch.

It wasn't an ideal solution, but, to quote Kurt Vonnegut: "So it goes."

### Architecture

I made a simple table with four fields:

- A string for the name of the feature in question
- A boolean to indicate whether the feature was active or inactive
- A UUID of the last user to update the flag
- A timestamp of the last time the flag was updated

We made this accessible in our CMS UI, which included a permissioning system, and restricted access to superusers only.

This functionality was essentially a permanent feature flag. The difference between this kill switch and a feature flag is that feature flags are typically intended to be temporary and are often used to do controlled launches. Once the new code is determined to be stable, the feature flag is removed, and the old code is discarded.

### Fast forward

Peloton didn't really have an established back end feature flag framework. The front end teams used something like Optimizely, which was much more feature-rich and allowed for things like A/B tests, but the back end teams didn't really need that kind of power.

However after maybe a year or two, and unbeknownst to me, one team did want feature flag functionality. and one of the younger engineers went digging and found this kill switch feature.

They decided to use it to launch one of their new features, which had nothing at all to do with e-commerce and was not intended to be a permanent flag. They must have also told their colleagues, because other teams started to use the kill switch feature as an ethereal feature flag, and that's essentially what it became.

### In which I smack my head

The real problem for this kind of usage was that our kill switch UI displayed nothing aside from a set of hard-coded switches in a specially-privileged CMS page that these teams didn't know about, either because they weren't permissioned to view it or they didn't dig deep enough to find out where the data was used. So they built a custom UI that displayed all the rows in the table and allowed CMS users to toggle them on and off.

The issue here is that the way they built their own less-privileged CMS page was such that it didn't differentiate between kill switches and feature flags, so any CMS user could have easily made a mistake and disabled the Avalara tax calculation, even if they didn't have the permission to access the kill switch UI.

### This is your life now

I explained the intent of the kill switch functionality to these teams after I found out what they'd been doing with it, but by then it was too late. They weren't going to build separate functionality to handle their use case, rightly or wrongly, and my business sponsors didn't want to pay for a tech debt project to extend the functionality to differentiate properly between kill switches and feature flags.

At the time of my departure kill switches and feature flags were still coupled, but we'd largely moved away from kill switches in favor of circuit breakers and smarter default error handling.

Fortunately, we hadn't had any issues with mistaken toggles of critical functions.

### Retrospective

I could have made the intent of the kill switch much clearer both in code and in our Confluence documentation. Why did we build it? As critical and highly-permissioned functionality, I should have outlined who gets permission, the shortcomings of the solution we'd put together so quickly, and a list of potential enhancements, and a vision for what it *should* look like.

That doesn't mean other teams wouldn't have built their feature flags on top of that functionality anyway, but at least they'd be more informed about the scope of feature and about the potential to create a security issue.

Documenting this stuff in the code, or at least inserting a comment to the full documentation would have been a good idea to make it more likely that a curious engineer would find it, but there are no guarantees.

I was miffed that people didn't just ask me about the feature since the guy who usurped it knew me quite well, but sometimes we just have to let go and accept our new reality. The best we can do is to write sufficient documentation, even if it isn't perfect (it doesn't even have to be good, just good enough), and we have to trust that our colleagues will find it and use it to make informed decisions, even if we might disagree with them.

The best we could have hoped for was that no issues arose from this change in use case, and we were fortunate that things played out that way.
