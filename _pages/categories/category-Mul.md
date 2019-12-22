---
title: "Post about Multimedia Systems"
layout: archive
permalink: /categories/mul
author_profile: true
sidebar_main: true
---
> 멀티미디어 시스템에 대한 포스트들입니다.

{% assign posts = site.categories.ml | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
