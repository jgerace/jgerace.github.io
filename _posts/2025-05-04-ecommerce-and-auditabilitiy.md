---
title: "The Senior Engineer's Guide: Software Degredation - Part I - E-commerce and Auditability, An Anecdote"
excerpt: "Software rots over time like those veggies in the back of your fridge."
classes: wide
tags:
  - software
  - career
  - senior-engineers-guide
---

### Why does good software go bad?

All software is vulnerable to errors. But the most insidious, sneaky types of errors are the ones we can't anticipate, the ones that make our software break gradually without our even noticing.

I'm thinking, in particular, about user stories changing as the business changes.

At Peloton, I spent a lot of time investigating quirks in the order lifecycle. Sometimes it was difficult to determine programatically or manually through our CMS what happened to an order such that it resulted in its current state. Usually, this sort of question arose because our workflows weren't that robust. Did the user make a mistake? Was there a bug in the code? What was the intent behind this data?

### Automating workflows fixed a lot of things!

Our RMA/Exchange/Refund functionality in our CMS came from a need to automate workflows as our customer base grew so large that support agents couldn't handle the glut of support tickets, even if those requests were a tiny percentage of order volume. We initially allowed for a lot of flexibility for things like customer support operations so that agents could assist customers more quickly and with more agility to increase customer satisfaction.

It actually worked pretty well, and from what I remember, our Net Promoter Score, which was the C-suite's north star at the time, was still well into the 90s.

### It was good while it lasted

The biggest source of data grief came from customer support agents and the manual workflows we provided for them through our CMS for our RMA/Exchange/Refund functionality. In order to make the support process as painless as possible for customers, and as quick as possible for agents (so they can close tickets more quickly), we were asked to give them quite a bit of leeway in terms of what they could do, i.e. financially. Just give an irate customer a refund. Toss money at the problem!

Flexibility, however, doesn't make it easy to tell the story of what happened and why, if that's what you need to do. As a younger company, we hadn't really needed a super detailed auditing framework that recorded exact changes to orders during the RMA/Exchange/Refund process.

We had a decent amount of logging, which was only available to, and readable by, engineers. That normally worked well, and engineers were able to debug issues just fine, or else we'd add more logging and wait for another occurrence of the issue. As one might expect, this occasionally resulted in a few upset customers, but emergencies really didn't happen that often.

Post-IPO, these workflows caused issues precisely because of that flexibility. If a support agent was refunding money for a few different reasons (concessions for bad service, product returns/exchanges, purchase of new products with a refund, etc) they could do all of that in a single transaction. However, the finance teams were required to account for these scenarios differently, and since the data was all mixed up, they couldn't tell what was what. And if they were desperate enough to ask me, I wouldn't have been able to tell either without a serious amount of digging.

As I understand the accounting requirements, there's a certain amount of error that can be recorded on the books before things get legally dicey, so that was something we needed to lock down before it got out of hand.

The IPO process required us to build more of that auditability framework. Our user stories changed with a single decision. Certainly, if we'd built it sooner it would have been useful for debugging purposes and it might have saved some time by eliminating the need for engineering intervention.  But we hadn't and what we did have was sufficient. There might have been a benefit, but no concrete user story until the IPO process.

At that point, we were able to examine the specific needs based on the most up-to-date user stories and devise a solution that could be extensible given the new auditability requirements.

### Pie in the sky

A good auding framework would answer five questions for business stakeholders:

- What action occurred?
- Who performed the action?
- When did it occur?
- Why did it happen?
- What data changed?

Taking each event in sequence would effectively allow anyone to reconstruct every order state from creation to the end of its lifecycle.

Good software would also provide logging so that engineers can debug intermediate steps in these state changes in the event of an error.


I do have anecdotes about software degredation as a result of engineering sacrifices and broken promises, and I intend to write about them in future installments. Stay tuned!
### Sorry support agents...

One of the first things we did was to make our customer support workflows a little more directed. Instead of allowing agents to perform multiple actions in a single runthrough of our RMA/Exchange/Refund workflow, we required them to perform multiple runthroughs so we could attach reasons for each transaction. We even broke out the accessory exchange workflow into its own space to ensure that they could only perform like-for-like exchanges instead of allowing customers to return, for example, a pair of shoes and, with the refund, purchase a heart rate monitor. The latter would be considered a brand new order for accounting purposes, and not an exchange.

This, understandably, made support agents' lives more difficult, but in a post-IPO world, legal requirements win over things like customer satisfaction and employees' blood pressure.

### Events, finally

We took the opportunity to add event streaming.

Each event corresponded to one of the RMA/Exchange/Refund actions, which we surfaced on the order profile in our CMS, so business stakeholders could easily see what happened without asking engineers. These events were then sync'd automatically to our Netsuite instance, where they were ingested into the accounting software in real time as opposed to our older practice of sending batched data.

The problem with event streams at Peloton was the engineering resource limitation of the internal Netsuite engineering team. They just didn't have enough time to ingest all the events we could have sent through, so we had to continue to batch some of the data, which occasionally created race conditions that the Netsuite engineers had to untangle. That, however, was their engineering problem to deal with when they felt the ROI made sense.

### The real issue is that it wasn't an issue

Everything was fine until Peloton decided to IPO, which is to say that the user stories at the time didn't lead engineers to think ahead about what might happen a few years in the future. The requirement to give more flexibility to support agents implicitly prevented us from parsing the agent's exact intent and the business need was greater than the data need, so we didn't spend the extra time on it. We were all totally on board with that!

To that end, the software was perfectly fine until it wasn't, which is basically the story of software evolution.

Engineers could have pushed back against the product team to make the support workflow more precise when we were building these CMS workflows, but there's no way we would have come up with a compelling argument for it without knowing we were ever going to IPO.

Just because a feature is somehow better doesn't mean that it's necessary.

And in a world in which you want to maximize your return on investment, you need to be as efficient as possible.

User stories can change either suddenly or gradually and everything is fine until it's not. It's sometimes easy, in other scenarios, to trace a path from one engineering sacrifice to another until we arrive at software that doesn't even fulfill its intended purpose, but software degredation doesn't always occur like that. Sometimes the nature of a business changes, and nobody's done anything wrong.

I do have anecdotes about software degredation as a result of engineering sacrifices and broken promises, and I intend to write about them in future installments. Stay tuned!
