---
title: Marginalia
description: "A Jekyll theme built around the margin-notes layout. This site, essentially, turned inside out and made into something other people can use. Documented as I go, which is the only honest way to document anything."
year: 2025
year_end: present
status: active
stack: [jekyll, sass, liquid]
feeling: "obsessed, slightly precious about it, unreasonably invested in the kerning"
project_url: https://github.com/yourusername/marginalia
---

{% include margin-note.html
   body="<p>This project began as a personal website and became a theme when I realised the margin note system was the most interesting thing about it and I wanted other people to be able to use it without copying my entire repository.</p>"
   mark="—"
   note="This is a common origin story for open-source projects: someone builds a thing for themselves and then feels vaguely guilty keeping it." %}

{% include margin-note.html
   body="<p>The core of it is a CSS grid trick. Each paragraph and its margin note live in the same grid row, which means the note always sits flush beside its anchor without JavaScript or absolute positioning. The rest of the theme — the masthead, the colours, the Garamond — is just furniture around that one idea.</p>"
   pull="true"
   note="The margin note system was the reason for building this in the first place. Everything else followed from it." %}
