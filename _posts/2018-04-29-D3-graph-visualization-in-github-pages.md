---
layout: post
title: "D3 Graph Visualization in GitHub Pages"
author: "R. Tyler McLaughlin"
date: "April 29th, 2018"
categories: blog
---
<script src="//code.jquery.com/jquery.js"></script>
<style>

.node {
  stroke: #fff;
  stroke-width: 1.5px;
}

.link {
  stroke: #999;
  stroke-opacity: .6;
}

</style>

The Javascript library [D3](https://d3js.org/) is known for making extremely nice looking, interactive data visualization apps that run in your web browser.  Some impressive examples include Mike Barry and Brian Card's visualizations of the [Boston subway system data](http://mbtaviz.github.io) and [an interactive neural network](https://playground.tensorflow.org) courtesy of Tensorflow.

Even though D3 has a serious learning curve, it is possible to make use of its nice visualizations without really knowing anything about Javascript.
This post will walk you through how to visualize a graph aka network in D3 with your own data.

It is really not that hard to get your own data
For this post I attempted to visualize my own network data using D3.  I followed a lot of steps by 
For this post, I wanted to make a simple D3 visualization to test whether or not there would be any hiccups to overcome in order to host my own D3 work with a Jekyll Blog and GitHub Pages. Javascript is high on my list of things to learn and I think picking up D3 along the way would be fun way to do that. I do not, however, want the presence of D3 on this site to indicate to anyone that I know what I'm doing. I do not.

<div id='d3div'></div>

You can click-and-drag the nodes around, although I'm not sure why you'd want to except that it is unreasonably fun. I think I read somewhere the edges of this graph are meant to behave like springs, but [Mike Bostock](https://en.wikipedia.org/wiki/Mike_Bostock) provides more information in the link given below. 

## The Code

The visualization above was taken from [this](http://bl.ocks.org/mbostock/4062045) entry of the [D3 gallery](https://github.com/mbostock/d3/wiki/Gallery). First, I create a `style` tag for the node and link attributes of the plot.



Finally the actual Javascript to create the SVG. I wanted the SVG to resize based on the width of the parent container. This way it should work properly when viewed on mobile or on a skinny web broswer window. To do this I changed the width variable to grab the width of the d3div tag. When you resize the window you'll have to refresh your browser to get the SVG to change. There almost certainly exists a more clever approach, but I didn't take the time to work it out.



<script src="//d3js.org/d3.v3.min.js"></script>
<script>

var width = $("#d3div").width(),
    height = 400;

var color = d3.scale.category20();

var force = d3.layout.force()
    .charge(-62)
    .linkDistance(80)
    .size([width, height]);

var svg = d3.select("#d3div").append("svg")
    .attr("width", width)
    .attr("height", height);

d3.json("../../../../scripts/jazz_scales_network_minCTs6.json", function(error, graph) {
  if (error) throw error;

  force
      .nodes(graph.nodes)
      .links(graph.links)
      .start();

  var link = svg.selectAll(".link")
      .data(graph.links)
    .enter().append("line")
      .attr("class", "link")
      .style("stroke-width", function(d) { return Math.sqrt(d.value); });

  var node = svg.selectAll(".node")
      .data(graph.nodes)
    .enter().append("circle")
      .attr("class", "node")
      .attr("r", 5)
      .style("fill", function(d) { return color(d.group); })
      .call(force.drag);

  node.append("title")
      .text(function(d) { return d.name; });

  force.on("tick", function() {
    link.attr("x1", function(d) { return d.source.x; })
        .attr("y1", function(d) { return d.source.y; })
        .attr("x2", function(d) { return d.target.x; })
        .attr("y2", function(d) { return d.target.y; });

    node.attr("cx", function(d) { return d.x; })
        .attr("cy", function(d) { return d.y; });
  });
});

</script>

## Conclusion

I'd like to thank the following bloggers for their helpful posts: [Andrew Mehrmann](http://dkmehrmann.github.io/blog/2016/05/01/d3.html), [Eric Bickel](https://ehbick01.github.io/2017/05/09/embedding-d3-visuals-in-rmarkdown/), and [Tyler Clavelle](https://tclavelle.github.io/blog/blogdown_github/)
