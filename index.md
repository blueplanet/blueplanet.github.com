---
layout: page
title: 蒼い惑星
---
{% include JB/setup %}

{% for post in site.posts limit: 1 %}
  <h2>
    <span>{{ post.date | date: "%Y/%m/%d" }}</span> &raquo; 
    <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  </h2>
  <div>
    {{ post.content }}
  </div>
{% endfor %}

