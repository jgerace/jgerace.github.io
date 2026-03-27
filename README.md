# Marginalia

A Jekyll theme for personal sites with a literary sensibility. Built around an inline margin note system — notes sit beside the text they annotate on desktop, and become tap-to-reveal toggles on mobile.

---

## Setup

**Requirements:** Ruby, Bundler, Jekyll 4+

```bash
bundle install
bundle exec jekyll serve
```

Site runs at `http://localhost:4000`.

---

## File structure

```
marginalia/
├── _config.yml           # Site settings, journal volume, hire block, home_post_count
├── _data/
│   └── links.yml         # Social / elsewhere links
├── _includes/
│   ├── masthead.html
│   ├── footer.html
│   ├── social-links.html
│   ├── mn.html             # ← margin note include (no number)
│   ├── sn.html             # ← sidenote include (auto-numbered)
│   ├── margin-note.html    # alias → mn.html
│   ├── sidenote.html       # alias → sn.html
│   └── icons/            # SVG icon partials
├── _layouts/
│   ├── default.html
│   ├── home.html
│   ├── post.html
│   ├── project.html
│   └── tag.html
├── _posts/               # Blog posts (.md)
├── _projects/            # Project entries (.md)
├── _sass/
│   ├── _variables.scss   # All colours, fonts, sizes — edit here first
│   ├── _base.scss
│   ├── _masthead.scss
│   ├── _layout.scss
│   ├── _post.scss        # Margin note system lives here
│   ├── _projects.scss
│   ├── _syntax.scss
│   └── _tags.scss
├── assets/css/main.scss
├── tags/index.html
├── writing/index.html
├── index.html            # Homepage (must be .html for jekyll-paginate)
├── projects.md
├── elsewhere.md
├── colophon.md
├── contact.md
├── 404.md
└── humans.txt
```

---

## Writing posts

### Front matter

```yaml
---
title: 'Your post title'          # use single quotes if title contains Markdown
# title: "Your post title"        # double quotes will escape * and _ characters
date: 2026-01-01
tags: [typography, process]
mood: "wistful, decisive"
mood_colour: red          # red | blue | green | amber
margin_notes: true        # set to true if post uses margin notes; omit or false for centred layout
description: 'Used for SEO meta description and social sharing previews.
                          # Falls back to lede, then excerpt if not set.'
wide: true                # full-width mode — expands to fill site width, collapses margin notes inline.
                          # Good for technical posts with large code blocks.
lede: "A sentence below the title."
byline_note: "optional parenthetical after your name"
read_time: 6
epigraph: "A quote that opens the post."
epigraph_source: "— Who Said It"
footer_note: "A custom footer note for this post."
excerpt: "Used in the post list."
redirect_from: /old-url-slug/   # for URL migration
---
```

### Margin notes

Notes are written **inline** inside normal Markdown paragraphs — no wrapper divs, no per-paragraph includes.

**Margin note** (no number, † toggle on mobile):

```markdown
Some text{% include mn.html id="unique-id" note="Your aside." %} continues normally.
```

**Sidenote** (auto-numbered via CSS counter, number toggle on mobile):

```markdown
Some text{% include sn.html id="unique-id" note="Your numbered aside." %} continues.
```

**Rules:**
- Each `id` must be unique within the page — use a short descriptive slug
- Notes can contain basic inline HTML (`<em>`, `<a>`, `<code>`) but not block elements
- Multiple notes in one paragraph are fine — they stack in the margin with `clear: right`

### Kramdown footnotes

You can also use standard Kramdown footnote syntax alongside margin notes:

```markdown
Some text with a footnote.[^1]

[^1]: The footnote text, which appears at the bottom of the page.
```

---

## Adding projects

Create a file in `_projects/`. The project title links to `project_url` if provided:

```yaml
---
title: Your Project
description: "Shown in the project list."
year: 2025
year_end: present
status: active            # active | resting | abandoned
stack: [ruby, jekyll]
feeling: "proud of it, uncertain what to do with it"
project_url: https://github.com/you/project  # omit for projects with no public URL
---
```

---

## Adding social links

Edit the `links:` section in `_config.yml`. Add `handle` and `note` fields for the `/elsewhere/` page:

```yaml
links:
  - platform: github
    url: https://github.com/yourusername
    handle: "@yourusername"
    note: "code, mostly half-finished"
    icon: github
```

To add a new platform, create `_includes/icons/yourplatform.html` with an inline SVG (`viewBox="0 0 24 24"`), then add an entry to the `links:` list in `_config.yml`.

---

## URL migration from old site

Add `redirect_from` to any post whose URL is changing:

```yaml
redirect_from: /old-post-slug/
```

Multiple old URLs:

```yaml
redirect_from:
  - /old-slug/
  - /2019/old-slug/
```

---

## Tags

Tags require no setup. A single `/tags/` page lists all tags and their posts automatically using `site.tags`, which Jekyll builds from your post front matter at compile time. No scripts, no generated files, no plugins needed.

Both front matter formats work:

```yaml
tags: [typography, process]
```

```yaml
tags:
  - typography
  - process
```

Tag links in post footers jump to the relevant section on `/tags/` via anchor links.

---

## Customising

All design tokens are in `_sass/_variables.scss`:

| Variable | Default | Purpose |
|---|---|---|
| `$pop-red` | `#c84b2f` | Drop caps, note marks, hover states |
| `$paper` | `#f6f1e7` | Page background |
| `$paper-dark` | `#ede4d0` | Surfaces, code blocks |
| `$margin-col` | `210px` | Width of the margin note column |
| `$margin-gap` | `2.5rem` | Gap between text and margin |
| `$max-width` | `940px` | Maximum site width |
| `$bp-medium` | `720px` | Breakpoint where margin collapses |
