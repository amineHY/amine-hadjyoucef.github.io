---
title: Education
subtitle: Summary of education
permalink: /articles/
layout: "single"
icon: fa-book
author_profile: true
header:
  image: assets/images/banner_publications.jpg
toc: true
---



I love books! Here are some I'm reading now:

1. Robert Burton: *The Anatomy of Melancholy*
2. Robert Musil: *The Man Without Qualities*
8. William Thackeray: *Pendennis*
9. Karl Marx: *Capital*
10. James Woodforde: *The Diary of A Country Parson*

source: [The Guardian](https://www.theguardian.com/books/booksblog/2011/jan/04/best-boring-books)




{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
