---
layout: sqlschool-lesson
categories: analytics-training
title: "About Analytics Training"
date: 2014-07-02 00:00:59
seo-title:
description: "Hone your analytical skills by solving real-world cases from Analytics teams at data-driven companies."
---

This section is meant to help those with solid SQL knowledge sharpen their analytical thinking ability. We've interviewed Analytics managers and re-created some of the problems they shared with us using fake data. These problems will force you to think critically about not just SQL syntax, but about the meaning behind what you're measuring.

You should have a strong handle on SQL before attempting these problems. If you need a refresher, <a href="/the-basics/introduction.html">start here</a>.

###Analytics Cases
This section is formatted into sections by company. For company, there is an overview explaining a bit about the business and the types of problems they typically solve&mdash;context that you will need in order to tackle the individual problems. For each company, there are several "cases"&mdash;scenarios that will test your analytical ability. The complete list is below:

{% for case in site.case-list %}
<h4 class="category" id="{{ case }}">
  {{ case | replace:'-', ' ' }}
</h4>
<ul class="table-of-contents">
  {% for post in site.posts %}
    {% if post.categories contains case and post.categories contains "analytics-training" %}
      {% assign answer = false %}
      {% if post.categories contains "answers" %}
        {% assign answer = true %}
      {% endif %} 
      {% if answer ==  false %}
        <li class="list-question">
          <a href="{{ post.url }}"> {{ post.title }}</a>
        </li>
      {% endif %}
    {% endif %}
  {% endfor %}
</ul>
{% endfor %}