---
title: Better Choice
layout: archive
permalink: categories/betterchoice
author_profile: true
sidebar_main: true
---



{% assign posts = site.categories['Better Choice'] %}

{% for post in posts %} {% include archive-single.html type=page.entries_layout %}{% endfor %}
