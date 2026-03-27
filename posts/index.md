---
layout: single
title: Posts
tags: [posts]
classes: wide
share: true
author_profile: true
---

<div class="posts-list">
{% assign visiblePosts = site.posts | where_exp: "post", "post.hidden != true" %}
{% assign postsByYear = visiblePosts | group_by_exp: 'post', 'post.date | date: "%Y"' %}
{% for year in postsByYear %}
  <section id="{{ year.name }}" class="taxonomy__section">
    <h1 class="archive__subtitle">{{ year.name }}</h1>
    <div class="entries-{{ page.entries_layout | default: 'list' }}">
      {% for post in year.items %}
        {% include post-single.html type=page.entries_layout %}
      {% endfor %}
    </div>
    <a href="#page-title" class="back-to-top">{{ site.data.ui-text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
  </section>
{% endfor %}
</div>
