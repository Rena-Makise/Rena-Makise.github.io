---
title: "Post about Cloud"
layout: archive
permalink: /categories/cloud
author_profile: true
sidebar_main: true
---
> 클라우드 시스템에 대한 포스트들입니다.

{% assign posts = site.categories.cloud | sort:"date" %}

{% for post in posts %}
  {% include archive-single.html type=page.entries_layout %}
{% endfor %}
