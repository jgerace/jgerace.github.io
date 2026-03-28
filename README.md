# Marginalia

A Jekyll theme for personal sites with a literary, dry-wit sensibility. Built around an inline margin note system — notes sit beside the text they annotate on desktop, and become tap-to-reveal toggles on mobile.

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
├── _config.yml           # Site settings — edit this first
├── _includes/
│   ├── masthead.html
│   ├── footer.html
│   ├── social-links.html
│   ├── mn.html           # ← margin note include (no number)
│   ├── sn.html           # ← sidenote include (auto-numbered)
│   ├── margin-note.html  # alias → mn.html
│   ├── sidenote.html     # alias → sn.html
│   ├── youtube.html      # ← responsive YouTube embed
│   └── icons/            # SVG icon partials (one file per platform)
├── _layouts/
│   ├── default.html
│   ├── home.html
│   └── post.html
├── _posts/               # Blog posts (.md)
├── _projects/            # Project entries (.md)
├── _sass/
│   ├── _variables.scss   # All colours, fonts, sizes — edit here first
│   ├── _base.scss
│   ├── _masthead.scss
│   ├── _layout.scss
│   ├── _post.scss        # Margin note system lives here
│   ├── _projects.scss
│   ├── _syntax.scss      # Code syntax highlighting
│   └── _tags.scss
├── assets/css/main.scss
├── writing/
│   └── index.html        # Paginated writing archive
├── tags/
│   └── index.html        # Tag index page
├── index.html            # Homepage (must be .html for jekyll-paginate)
├── projects.md
├── elsewhere.md
├── contact.md
├── colophon.md
├── 404.md
├── robots.txt
└── humans.txt
```

---

## Writing posts

### Front matter

```yaml
---
title: 'Your post title'       # use single quotes if title contains Markdown
date: 2026-01-01
tags: [typography, process]
mood: "wistful, decisive"
mood_colour: red               # red | blue | green | amber
margin_notes: true             # set to true if post uses margin notes; omit for centred layout
wide: true                     # full-width mode — expands to fill site width, collapses margin notes
                               # Good for technical posts with large code blocks
description: "Used for SEO meta description and social sharing previews.
              Falls back to lede, then excerpt if not set."
lede: "A sentence below the title."
byline_note: "optional parenthetical after your name"
read_time: 6              # optional — auto-calculated from content if omitted
epigraph: "A quote that opens the post."
epigraph_source: "— Who Said It"
footer_note: "A custom footer note for this post."
excerpt: "Used in the post list."
redirect_from: /old-url-slug/  # for URL migration
---
```

### Margin notes

Notes are written **inline** inside normal Markdown paragraphs — no wrapper divs, no per-paragraph includes.

**Margin note** (no number, † toggle on mobile and in wide mode):

```markdown
Some text{% include mn.html id="unique-id" note="Your aside." %} continues normally.
```

**Sidenote** (auto-numbered via CSS counter, number toggle on mobile and in wide mode):

```markdown
Some text{% include sn.html id="unique-id" note="Your numbered aside." %} continues.
```

**Rules:**
- Each `id` must be unique within the page — use a short descriptive slug
- Notes can contain basic inline HTML (`<em>`, `<a>`, `<code>`) but not block elements
- Multiple notes in one paragraph are fine — they stack in the margin with `clear: right`
- Margin notes do not work inside Markdown files that mix HTML blocks — use pure Markdown or pure HTML

### Kramdown footnotes

Standard Kramdown footnote syntax works alongside margin notes:

```markdown
Some text with a footnote.[^1]

[^1]: The footnote text, which appears at the bottom of the page.
```

### Embedded YouTube videos

```markdown
{% include youtube.html id="VIDEOID" %}
{% include youtube.html id="VIDEOID" width="640" height="360" %}
{% include youtube.html id="VIDEOID" controls="0" start="90" %}
```

Defaults to 1280×720 (16:9) if width/height are omitted.

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
| `$margin-col` | `220px` | Width of the margin note column |
| `$margin-gap` | `44px` | Gap between text and margin |
| `$max-width` | `980px` | Maximum site width |
| `$bp-medium` | `720px` | Breakpoint where margin collapses |

The hire block on the homepage is toggled by `hiring: true/false` in `_config.yml`.

The number of recent posts shown on the homepage is set by `home_post_count` (default: 5). The full paginated archive lives at `/writing/`.

---

## Contact form

The contact page at `/contact/` submits to Formspree. To change the endpoint, update the `action` attribute in `contact.md`:

```html
<form class="contact-form" action="https://formspree.io/f/yourcode" method="POST">
```
