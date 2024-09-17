---
layout: page
title: "Bash In Depth"
permalink: /bash-in-depth/
---

This page contains chapters on various topics in Bash.

<ul>
  {% for chapter in site.bash-in-depth %}
    <li><a href="{{ chapter.url }}">{{ chapter.title }}</a></li>
  {% endfor %}
</ul>
