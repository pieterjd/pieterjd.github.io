---
title: Talks
permalink: /talks/
layout: archive
---
cdcdcd
talks: {{site.talks.size}}


{% for talk  in site.talks %}
  <section id="{{ talk.title }}" class="taxonomy__section">
    <h2 class="archive__subtitle">{{ talk.title }}</h2>
    <a href="#page-title" class="back-to-top">{{ site.data.ui-text[site.locale].back_to_top | default: 'Back to Top' }} &uarr;</a>
  </section>
{% endfor %}
