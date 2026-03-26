---
layout: default
title: Elsewhere
permalink: /elsewhere/
footer_note: "The internet is large. I am in several corners of it, maintaining varying levels of composure."
description: "Where to find me around the internet."
---

<div class="post-epigraph">
  <p>"Now more than ever do I realize that I will never be content with a sedentary life, that I will always be haunted by thoughts of a sun-drenched elsewhere."</p>
  <cite>— Isabelle Eberhardt · <em>The Nomad: Diaries of Isabelle Eberhardt</em></cite>
</div>

<div class="rule-ornament" aria-hidden="true"><span>❧</span></div>
<h2 class="section-label">where to find me</h2>

<div class="elsewhere-list" role="list">
  {% for link in site.links %}
  <div class="elsewhere-list__item" role="listitem">
    <div class="elsewhere-list__icon" aria-hidden="true">
      {% include icons/{{ link.icon }}.html %}
    </div>
    <div class="elsewhere-list__body">
      <p class="elsewhere-list__platform">{{ link.platform }}</p>
      <p class="elsewhere__handle">
        <a href="{{ link.url }}" rel="me noopener" target="_blank"
           aria-label="{{ link.platform }}{% if link.handle %}: {{ link.handle }}{% endif %}">
          {{ link.handle | default: link.url }}
        </a>
      </p>
      {% if link.note %}
      <p class="elsewhere__note">{{ link.note }}</p>
      {% endif %}
    </div>
  </div>
  {% endfor %}
</div>
