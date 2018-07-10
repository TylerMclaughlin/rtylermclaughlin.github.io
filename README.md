# Hello!

My Name is Tyler McLaughlin and I'm a PhD Scientist living in Houston.  My scientific career as a biologist began with researching systems biology in Pittsburgh, PA and Farmington, CT.  I had a math and molecular biology double major in undergrad and so this focus felt natural.  During my PhD in Systems, Synthetic, and Physical Biology at Rice University, my research involved human cell biophysics and systems biology, at the experimental level and with extensive amounts of image-based and statistical data analysis.   I am now applying to be a data scientist or computational biologist in industry.  You can learn more about me [here](www.linkedin.com/in/r-tyler-mclaughlin-phd).

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




[md](_posts/2018-04-29-jazz-scale-networks.md)
[html](_posts/2018-04-29-jazz-scale-networks.html)



### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/TylerMclaughlin/tylermclaughlin.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
