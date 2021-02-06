---
layout: page
permalink: /publications/
title: Publications
description: Sorted in reverse chronological order.
years: [2021, 2020]
nav: true
---
For the most up-to-date list, please see my <a href="https://scholar.google.com/citations?hl=en&user=0eAmyqkAAAAJ">Google scholar page.</a>
<div class="publications">

{% bibliography -f papers -q @ %}

{% for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}
</div>
