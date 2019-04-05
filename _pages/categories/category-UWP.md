---
title: "Post about UWP"
layout: archive
permalink: /categories/uwp
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.uwp | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
