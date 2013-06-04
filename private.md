---
layout: default
title: Private
---
<h1>Private area</h1>
<p>Either not ready for general consumption or personal notes</p>
<div class="row-fluid">
    <div class="span6 filler">
        <h4>Personal</h4>
        <ul>
        {% for post in site.tags.phd %}
        {% if post.private %}
        <li><a href="{{ post.url }}">{{ post.title }} - {{ post.date | date: "%d %b %Y" }}</a></li>
        {% endif %}
        {% endfor %}
        </ul>
    </div>
    <div class="span6">
        <h4>Drafts</h4>
        <ul>
        {% for post in site.posts %}
        {% if post.status.draft %}
        <li><a href="{{ post.url }}">{{ post.title }}-  {{ post.date | date: "%d %b %Y" }}</i></a></li>
        {% endif %}
        {% endfor %}
    </div>
</div>