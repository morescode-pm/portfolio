---
layout: page
title: Portfolio Index
---

<details open>
  <summary><strong>List of Projects by Name</strong></summary>
    <ul>
    {% for post in site.posts %}
        <li>
        <a href="{{ post.url }}">{{ post.title }}</a>
        <br>
        <small style="font-style: italic">Tags: {{ post.tags | join: ', ' }}</small>
        </li>
    {% endfor %}
    </ul>
</details>


<details open>
  <summary><strong>List of Projects by Tag</strong></summary>
  <div style="line-height:1.3;">
    <ul>
    {% for tag in site.tags %}
      <li style="margin-bottom: 0.2em;">
        <span style="font-style: italic">{{ tag[0] }}:</span>
        {% for post in tag[1] %}
          <small><a href="{{ post.url }}">{{ post.title }}</a>{% unless forloop.last %}, {% endunless %}</small>
        {% endfor %}
      </li>
    {% endfor %}
    </ul>
  </div>
</details>