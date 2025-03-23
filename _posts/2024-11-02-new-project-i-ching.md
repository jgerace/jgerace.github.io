---
title: "New Project: I Ching - The Book of Change"
excerpt_separator: "<!--more-->"
tags:
  - software
  - project
---

![Diagram of the I Ching by Unknown author - Perkins, Franklin. Leibniz and China: A Commerce of Light. Cambridge: Cambridge UP, 2004. 117. Print., Public Domain, https://commons.wikimedia.org/w/index.php?curid=36231359]({{ site.url }}{{ site.baseurl }}/assets/images/i-ching-diagram.jpeg){: .align-center}

* [Consult the I Ching!](https://www.johngerace.com/iching/)
* [View the source on Github!](https://github.com/jgerace/iching)

I recently digitized the core text of the public domain [Richard Wilhelm translation of the I Ching](http://www2.unipr.it/~deyoung/I_Ching_Wilhelm_Translation.html) and made a small web app for users to throw stalks (or coins) and get a reading.

This kind of thing exists online[^1], but the sites I've seen don't have a meditative quality to them at all. You click a bunch of buttons and get an immediate result. It's too fast. I wanted to make something that was easy to use, but gives more opportunity for reflection.

To that end, I used vanilla JS and CSS to walk a user through the steps and stretch the process out with simple timed element fading. It begins by prompting the user to throw sticks 6 times instead of automatically picking them at random at a single click. The idea is typically for the user to focus on their concern while they throw the stalks or coins, which can take a couple minutes to organize, interpret, and look up. The least I could do was to extend the timing a couple seconds after each digital throw to encourage a longer period of meditative focus before viewing their readings.

I purposfully omitted Wilhelm's commentary on the hexagrams because I prefer the experience of reading the core text and encouraging more interpretation, but I wouldn't be opposed to including that elsewhere, or on the hexagram reading itself, but hidden by default.

As a career backend engineer, I enjoyed playing around with JS/CSS. It was a fun UI exercise, and I'm definitely going to continue on it to clean up the code and add more interesting features if I can. I want to add more references and context for those who want to read further, and I can add more of the peripheral info about the trigrams and hexagrams, like familial relationships, animals, obtained images, data browsing, etc.

I used [Chota CSS](https://github.com/jenil/chota) for its simple styling and row/col tags, but I don't know that I love it. I ran into some weird problems where I couldn't hide a button, so I want to understasnd more about what it's doing under the hood, but I've also wanted to write my own tiny CSS framework for my own sites, so this might be a good opportunity to get the basics down.

[^1]: A few: [ichingonline.net](https://www.ichingonline.net), [Eclectic Energies](https://www.eclecticenergies.com/iching/virtualcoins), [AI Ching](https://aiching.app/consult-the-i-ching/)