---
title: 练习说明
permalink: index.html
layout: home
---

# GitHub Copilot 练习

以下快速入门练习旨在为你提供实践学习体验，帮助你探索 GitHub Copilot 的各项功能。 每个练习都包含一系列任务，你可以在自己的实验室环境中完成这些任务。

## 快速入门练习
<hr>

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}

{% for activity in labs  %}

### [{{ activity.lab.title }}]({{ site.github.url }}{{ activity.url }})
{{ activity.lab.description }}
<hr>
{% endfor %}
