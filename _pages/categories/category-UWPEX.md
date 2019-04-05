---
title: "Post about UWP Example"
layout: archive
permalink: /categories/uwpex
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.uwpex | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
