---
title: "Post about etc..."
layout: archive
permalink: /categories/etc
author_profile: true
sidebar_main: true
---
> 기타 전공관련 자료입니다.

{% assign posts = site.categories.etc | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
