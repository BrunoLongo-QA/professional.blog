---
layout: page
title:  "Artigos Em Portugues"
permalink:  /br-articles/
tags: categories
---
<div class="tags-expo">
  <hr />
  <div class="tags-expo-section">
    {% for tag in site.categories %}
    <ul class="tags-expo-posts">
      {% for post in tag[1] %}
        {% if tag[0] == "BR-Articles" %}
        <a class="post-title" href="{{ site.baseurl }}{{ post.url }}">
            <li>
            <small class="post-link">{{ post.date | date_to_string }} - {{ post.title }}</small> 
            </li>
        </a>
      {% endif %}
      {% endfor %}
    </ul>
    {% endfor %}
  </div>
</div>