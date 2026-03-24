---
layout: default
title: Projects
permalink: /projects/
footer_note: "If you have made it this far down the projects page: thank you. Most people do not. This note was written for you specifically."
---

<div class="post-epigraph">
  <p>"The ratio of time spent making to time spent explaining what you made is, in my experience, approximately 1:4."</p>
  <cite>— Me · <em>unpublished draft, 2023</em></cite>
</div>

<div class="rule-ornament" aria-hidden="true"><span>❧</span></div>
<h2 class="section-label">things I have made, with varying degrees of follow-through</h2>

<div class="project-list">
  {% assign sorted_projects = site.projects | sort: "year" | reverse %}
  {% for project in sorted_projects %}
  <article class="project-list__item{% if project.status == 'abandoned' %} project-list__item--abandoned{% endif %}">
    <div class="project-list__meta">
      <span class="project-list__year">
        {{ project.year }}{% if project.year_end %} – {{ project.year_end }}{% endif %}
      </span>
      {% if project.status %}
      <span class="mood-chip
        {% case project.status %}
          {% when 'active' %}chip-green
          {% when 'resting' %}chip-amber
          {% when 'abandoned' %}chip-red
        {% endcase %}
        " style="display:inline-block; margin-top:6px">
        {% case project.status %}
          {% when 'active' %}● active
          {% when 'resting' %}○ resting
          {% when 'abandoned' %}✕ abandoned
        {% endcase %}
      </span>
      {% endif %}
    </div>
    <div>
      <h2 class="project-list__title">
        {% if project.project_url and project.status != 'abandoned' %}
        <a href="{{ project.project_url }}" target="_blank" rel="noopener">{{ project.title }}</a>
        {% else %}
        {{ project.title }}
        {% endif %}
      </h2>
      <p class="project-list__desc">{{ project.description }}</p>
      {% if project.stack %}
      <div class="project-list__stack">
        {% for tech in project.stack %}
        <span class="project-list__tech">{{ tech }}</span>
        {% endfor %}
      </div>
      {% endif %}
      {% if project.feeling %}
      <p class="project-list__feeling"><span>disposition — </span>{{ project.feeling }}</p>
      {% endif %}
    </div>
  </article>
  {% endfor %}
</div>

<div class="ornament" aria-hidden="true">⁂</div>

<p class="colophon-note">All projects listed here were made by one person, evenings and weekends, without a brief, a budget, or anyone to answer to. This is either the ideal creative condition or a description of a problem. Probably both.</p>
