---
layout: index
---

<ul class="posts">
  {% for post in site.posts %}
    <li class="post">
      <a class="post-link" href="{{ post.url }}">
        <h2 class="post-title">{{ post.title }}</h2>
        <span class="meta">{{ post.date | date_to_string }}</span>
      </a>
    </li>
  {% endfor %}
</ul>
