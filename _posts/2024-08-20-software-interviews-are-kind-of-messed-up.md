---
title: "Software interviews are kind of messed up"
excerpt: "Everyone in the tech industry knows that software engineering interviews are awful and very few companies have done something about it."
tags:
  - software
  - interviewing
  - career
  - peloton
  - leetcode
---

Everyone in the tech industry knows that software engineering interviews are awful and very few companies have done something about it. Now that I've been on the job market for several months, I've had an updated view of what it's like from the candidate's perspective. At the outset of my job search, I was grinding Leetcode questions because that's what you do. And I got totally slaughtered! My 18 years of professional experience had little bearing on the outcome of this kind of interview!

Phone screens are usually a data structures/algorithms question that seeks to weed candidates out. I'm guilty of running this kind of interview myself for years. I own that and I will no doubt do penance in the afterlife for my sins. But during that time I also began to feel like there was a better way.

When I first started at Peloton in 2015, we ran candidates through a gauntlet of Leetcode- and Cracking-the-Coding-Interview-style questions just like everyone else did. And it was...serviceable. People got jobs. We filled roles. Some coworkers were great collaborators/mentors/technologists and some weren't. Some were good cultural fits and others weren't. But then we started to question why we were asking the questions we were asking. Data structures and algorithms questions are important if you're doing a lot of algorithm-heavy optimizations, but since we were building an e-commerce platform those questions didn't really represent the kinds of challenges candidates would face on the job. Could we glean better information during the interview experience with more-relevant questions? What would those questions look like?

### What do I look for in a candidate?

At first, I continued asking DSA questions, but with far less emphasis on the actual DSA. I absolutely could not care less if you can balance a binary tree or find the median of the last n elements of a stream of numbers. You can look those algorithms up on your own time and I trust the long memory of the internet to have plenty of resources.

In conversations about work experience, I care about how well you can tell me about the work you've done. Pick a project. Tell me about the problem you were trying to solve. I want to know that you know why you were solving it, that you understand and can clearly explain the context to someone who's unfamiliar with the company/domain.

In coding interviews, I need to see at least a little code. Obviously. Nothing crazy, though, I swear! I was still asking questions that required knowledge of data structures and algorithms, but I tried to make it clear that I didn't really care if candidates needed help with those. We could use some abstractions if we had to or I could help candidates write the operations they needed. It was a pair programming exercise! I offered a problem and told candidates that I expected to chat as we approached the problem, clarified details, and ultimately laid out the approach.

Candidates definitely needed to write test cases. Or at least explain what cases they would want to include in a test suite. I'm not strict on whether tests should come before implementation or after. We all write tests on the job, so this is probably the most practical part. I wanted to know that candidates could understand a problem deeply enough to spot the edge cases and think about different scenarios.

Candidates should also be able to walk me through their code line by line with a couple example test cases. It's disconcerting when candidates write code that they can't explain. At the most basic level we all need to be able to understand what we've implemented and we need to be able to explain it to others. We need to be able to justify our choices and explain our thought process, especially if we're collaborating on code long-term and mentoring other engineers.

I felt like I had a lot of good results from that, but it still didn't cover as wide a range of the actual job as I would've liked. Fortunately, the larger interviewing team came together to come up with a brand new (to us) paradigm that included new question formats.

### Solve a problem that isn't completely insane

I don't think anyone cares if you can write a flood-fill algorithm or two sum or rotate a linked list. I do care that you can understand a problem with some constraints, write working code, then build off of your solution.

To that end, Peloton asked a question that basically asked to implement a tennis scoring system. The first part detailed a scoring progression of 0 -> 15 -> 30 -> 40 -> Win, which was pretty straightforward.

The second part was to modify this to include Deuce and Advantage, so that a player basically had to win by two points.

There was a third part that I can't remember because barely anyone ever got to it. Usually people got through part II before it was time to allow the candidate to ask questions.

### Modify an existing code base

Hardly anyone ever works on a brand new code base, so it made sense to ask a question that drops candidates into the unfamiliar.

We used [Emily Bache's Supermarket Refactoring Kata](https://github.com/emilybache/SupermarketReceipt-Refactoring-Kata) such that we had two distinct versions of the code base: one for a refactoring exercise and one for a new feature (discounted bundles).

These two exercises were presented in different rounds of interview, with the new feature beginning with a refactored version of the code so that candidates aren't working with trash at the outset.

The code wasn't so overwhelming that most candidates couldn't wrap their heads around it relatively quickly, but there were times that people just balked. This occurred, I think, far less often than with the Leetcode-style questions, and we often received compliments on how refreshing it was to work on a problem that leveraged someone's actual work experience.

### Good interviewing takes work

It's difficult to write questions that mimic the on-the-job experience in the span of an hour-long interview. I don't think I'm a huge fan of take-home projects because, as an interviewer, you don't get to see the thought process. And as a candidate, you don't generally get the benefit of a conversation with an interviewer (this is not always the case). And sometimes, take-home projects require a ton of time that isn't compensated[^1].

I would argue that putting forth the effort, especially if you're starting with something like Emily Bache's code katas, is well worth the cost in terms of both the relevance of the tests and the humaneness of the interviewing process. It's unreasonable to ask lazy questions and expect great results. I'm sure it's possible to find great matches using only Leetcode, but if interviewers don't think about why they ask those questions, they're going to hire candidates who are good at Leetcode instead of hiring candidates who are a better fit for the role itself. It's all the randomness with fewer possibilities.

[^1]: And yes, I'm saying it should be, especially if it takes more than an hour or two, otherwise you, the company, have no skin in the game, and you should.