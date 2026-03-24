---
layout: default
title: Elsewhere
permalink: /elsewhere/
footer_note: "The internet is large. I am in several corners of it, maintaining varying levels of composure."
description: "Where to find me around the internet."
---

<div class="post-epigraph">
  <p>"You can find me in several places. Whether this constitutes a presence is a matter of interpretation."</p>
  <cite>— Me · <em>written just now, for this page</em></cite>
</div>

<div class="rule-ornament" aria-hidden="true"><span>❧</span></div>
<h2 class="section-label">where to find me</h2>

<div class="post-list" role="list">
  {% for link in site.links %}
  <div class="post-list__item" role="listitem">
    <div class="post-list__meta">
      <span class="post-list__tag">{{ link.platform }}</span>
    </div>
    <div>
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
