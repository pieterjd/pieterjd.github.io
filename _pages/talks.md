---
layout: archive
title: Talks
permalink: /talks/
---
Past talks will be added over time, future talks will be posted once the date has been confirmed.


{% for talk  in site.talks %}
  <section id="{{ talk.title }}" class="taxonomy__section">
    <a href="{{ talk.url }}"><h2 class="archive__subtitle">{{ talk.title }}</h2></a>
    <a href="#{{page-title}}" class="back-to-top">{{ site.data.ui-text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
  </section>
{% endfor %}
