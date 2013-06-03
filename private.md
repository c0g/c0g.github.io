---
layout: default
title: Private
---
# Private
Either not ready for general consumption or personal notes

<div class="filler">
    <ul>
    {% for post in site.posts %}
    {% if post.private %}
    <li><a href="{{ post.url }}">{{ post.title }} - {{ post.date | date: "%d %b %Y" }}</a></li>
    {% endif %}
    {% endfor %}
    </ul>
</div>