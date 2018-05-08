---
layout: default
title: Blog
navigable: true
---

# Hello!

My Name is Tyler McLaughlin and I'm a PhD Scientist living in Houston.  My scientific career  as a biologist started off researching computational systems biology in Pittsburgh, PA and Farmington, CT.  In undergrad I loved math and molecular biology equally and so research in this area felt very natural.  During my PhD in Systems, Synthetic, and Physical Biology, my research involved human cell biophysics and systems biology, mostly at the experimental level but with extensive amounts of image-based and statistical data analysis.   I am now applying to be a computational biologist at many companies on the West Coast.  You can learn more about me [here](www.linkedin.com/in/r-tyler-mclaughlin-phd).

# Posts

<ul style="padding-left:0px;">
  {% for post in site.categories.blog %}

      <h2>
        <a href="{{ post.url | prepend: site.baseurl }}">{{ post.title }}</a>
      </h2>

      <span class="text-warning">{{ post.date | date: "%b %-d, %Y" }}</span>
      <p>{{ post.content | strip_html | truncatewords:75}}</p>
      <a href="{{ post.url | prepend: site.baseurl }}">Read more...</a><br>

  {% endfor %}
</ul>

