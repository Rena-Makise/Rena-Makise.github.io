---
title: "Post about UWP Example"
layout: archive
permalink: /categories/uwpex
author_profile: true
sidebar_main: true
---
> UWP 앱 실전 코드들에 대한 포스트들입니다.

{% assign posts = site.categories.uwpex | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
