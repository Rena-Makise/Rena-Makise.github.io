---
title: "Post about Cloud"
layout: archive
permalink: /categories/cloud
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.cloud | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
