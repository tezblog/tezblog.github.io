---
layout: default
permalink: /tag/test
title: leetcode
---

<div id="tag-title">
  <h1>{{ page.title }} 태그 포스팅들</h1>
</div>
<div id="tag-content">
    {% assign subs = "[array#stack|node|design|math" | split: "|" %}
    {% assign subs_desc = "String, Array:|Linked List, Tree, Graph:|Back Tracking, Dynamic Programming, Divide and Conquer, Greedy:|Mathematics:" | split: "|" %}
    {% assign to = subs.size | minus: 1 %}
    {% assign posts = site.tags[title] %}
    
    {% for i in (0..to) %}
        <div class="sub-tag">
            <h2>{{ subs_desc[i] }}</h2>
        </div>
        {% for post in posts reversed %}
            {% for x in subs[i] | split: "#" %}
                {% if post.tags contains x %}
                    <div class="post-list">
                        <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
                        <div class="post-desc">Last updated: {{ post.updated }}</div>
                    </div>
                    {% break %}
                {% endif %}
            {% endfor %}
        {% endfor %}
    {% endfor %}
</div>
