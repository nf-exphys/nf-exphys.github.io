---
layout: page
permalink: /publications/
title: Publications
description: A summary of my peer-reviewed publications powered by jekyll-scholar. Use the HTML links to visit the journal page or the PDF button to download the article. 
years: [2022, 2021]
nav: true
nav_order: 1
---
<!-- _pages/publications.md -->
<div class="publications">

{%- for y in page.years %}
  <h2 class="year">{{y}}</h2>
  {% bibliography -f papers -q @*[year={{y}}]* %}
{% endfor %}

</div>
