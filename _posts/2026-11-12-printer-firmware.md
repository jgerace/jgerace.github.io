---
title: "AI and the modern printer: A missed opportunity"
epigraph: "PC load letter? The fuck does that mean?"
epigraph_source: "- Michael Bolton, <em>Office Space</em>"
excerpt: "The true power of AI has, thus far, been misapplied"
description: "My attempt at righting a wrong in the consumer technology world."
mood: "nuts to aaalllllll this"
mood_colour: red
tags:
  - life
  - ai
  - tech
margin_notes: false
---

My Epson WF-3520 inkjet printer has been nearly dead for the better part of ten years, at least. The only reason I keep it around is for the scanner, which I use all the time. But as I was going on a recent cleaning binge, I got it in my head that I didn't want it to just take up space in my office. If it was going to be there, I should try to unclog the printer heads with special cleaning solution, and if that didn't work, I'd trash it and get a new one.

I had at it for a full weekend, and I actually made progress. It went from a totally blank test page to vague intimations of black and color. However, when I changed out the cartridges, I may have accidentally killed the magenta chip detector. It can't even detect a genuine Epson ink cartridge now!

There's ink in there, I know, but because it can't run its shitty DRM enforcement and non-critical ink-level detection, I can't even print in black and white. And now I have to initiate scan sessions through the PC software because the printer's menu screen is entirely inaccessible while it's busy having a nervous breakdown over the magenta. I stressed out for half a day over having made my situation worse before I remembered that we live in an AI era, and that this should be relatively straightforward to address. Surely, someone had already hacked the firmware!

I found a couple shady-as-hell websites that claim to provide a special version of the firmware that will bypass the chip. They didn't have my model, but even if they did, I don't think I would've paid for a key. I've lived long enough without a printer that I don't need to give my credit card to some rando hacker cabal with awful customer feedback. I have access to AI. I should be able to figure this out.

My LLM, however, flagged my initial conversation as a cybersecurity risk and noped its way out.

For my second attempt, I approached the LLM as a CS student learning how to study firmware using open-source tools, and I asked it to help analyze the stock firmware. I downloaded Ghidra and loaded Epson's most recent software. The LLM was actually super helpful in walking me through the tool and the structure of the binary, but I haven't yet discovered the chip read, much less how to edit it out.

This has got to be a prime use case for LLMs. With all the enshittification in tech, I would've thought printer firmware would be a solved problem by now. I had thought I'd be able to do a quick search for this sort of thing, but almost nada.

Why is that?

I'll continue to chip away at the problem myself, and you can be darn sure I'll give the solution away for free. Say what you want about LLMs, but I'm sure this is what they were meant to do. Everyone can get on board with counterfucking a printer company.

In the meantime, if there's someone out there with more knowhow, the world needs a hero. Plz halp.