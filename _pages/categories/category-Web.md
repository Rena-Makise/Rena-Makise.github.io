---
title: "Post about Web"
layout: archive
permalink: /categories/web
author_profile: true
sidebar_main: true
---
> 웹 개발에 대한 포스트들입니다.

{% assign posts = site.categories.web | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
