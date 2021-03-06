---
layout: post
title: Visualizing Union Find Algorithm in d3.js
---

<p data-height="570" data-theme-id="6323" data-slug-hash="Lgmnw" data-default-tab="result" data-user="SundeepB" class='codepen'>See the Pen <a href='http://codepen.io/SundeepB/pen/Lgmnw/'>Connected nodes</a> by Sundeep (<a href='http://codepen.io/SundeepB'>@SundeepB</a>) on <a href='http://codepen.io'>CodePen</a>.</p>
<script async src="//assets.codepen.io/assets/embed/ei.js"></script>

Read about the Union Find algorithm [here](http://algs4.cs.princeton.edu/15uf/). The algorithm is also explained in Coursera's [Algorithms course by Robert Sedgewick and Kevin Wayne](https://www.coursera.org/course/algs4partI).

See the full code on [code pen](http://codepen.io/SundeepB/pen/Lgmnw)

Its implemented in the awesome [D3 JS](http://d3js.org/)

We start by defining an svg element in the HTML

```html
<svg class="dot_world"></svg>
```

That is all the HTML we'll require apart from including the d3 js library. The rest of it is accomplished using D3.

We select the svg element and set the height and width by

```javascript
var dot_world = d3.select(".dot_world")
                  .attr("height",height)
                  .attr("width",width);
```

In the visualization, each node is a dot. For n nodes, each node is labeled from 0 to n-1.

```javascript
var data = [];
for (var i = 0;i < total_dots; i++) {
    data.push(i);
}
```

In our visualization, the nodes will be positioned in 2D space, so, we'll need to define functions to return the x and y co-ordinates of each node depending on the position of the node.

```javascript
var cx = function(d) {
  return ((d%dots_on_x)*(dot_span))+dot_spacing;
}
```

`d%dots_on_x` gives the column the dot will be present. With this we can calculate the x position of the node.

```javascript
var cy = function(d) {
  return ((Math.floor(d/dots_on_x))*dot_span)+dot_spacing;
}
```

`Math.floor(d/dots_on_x)` gives the row the dot should be placed in. With this we can calculate the y position of the node.

A circle svg element looks like the following

```html
<circle style="fill: rgb(218, 113, 26);" r="3" cy="394" cx="170"></circle>
```

While style is optional, r, cx and cy are required. Using D3

```javascript
var dot = dot_world.selectAll("circle")
                    .data(data)
                    .enter()
                    .append("circle")
                    .attr("cx",function(d,i){return cx(i);})
                    .attr("cy",function(d,i){return cy(i);})
                    .attr("r",dot_radius)
```

The `enter()` function will select all the data that is not attached to any element. At this point, it will be all the elements in the `data` array. `append(circle)` function will append a `<circle>` element for every data element. We'll add `cx`, `cy` and `r` attributes for each element using the functions we constructed earlier. Only because of d3, we were able to establish all this in well structured code.

At this point we'll have all the nodes drawn on the svg canvas. In an ideal situation, any node should be able to connect to any other node. But, if we do the same in the visualization, it'll make the node a mess and hard to understand. For the sake of simplicity and simple visualization, we will allow each node to connect only to its immediate neighbors(excluding diagonal connections). So, each node effectively has at least 2 neighbors and a maximum of 4 neighbors. In an entirely connected network of n nodes, we'll have `2n-(x+y)` connections where `n` is the total number of nodes and `x` and `y` are the number of columns and rows. As we'll want to randomize each connection, we'll take a chance at each connection whether to connect or not. This will make sure some dots are not connected, however, it is not impossible that the entire node comes connected, though I have not seen it occur even once.

An svg line looks like

```html
<line x1="458" y1="42" x2="474" y2="42" style="stroke:#000000;stroke-width=10"></line>
```

We'll add lines as

```javascript
var line = dot_world.append("line")
                    .attr("x1",cxa)
                    .attr("y1",cya)
                    .attr("x2",cxa)
                    .attr("y2",cya)
                    .attr("style","stroke:#000000;stroke-width=10");
```

Notice that at this point the line length is zero. This is because we want to animate the line to show that we are connecting the nodes. The line transition is defined as

```javascript
line.transition()
    .attr("x2",cxb)
    .delay(2000)
    .duration(2000)
    .attr("y2",cyb)
    .duration(2000);
```

The animation will begin with a `delay()` of 2 seconds so that the animation is not too sudden and the `duration()` of animation is also 2 seconds, short enough not to make the user bored and lengthy enough to enjoy the transition.

The `data` array is where we maintain our node connections and where the heart of the algorithm is. As explained, we'll go through each node, taking chance with all of its neighbors to make a connection. We'll randomly decide whether the node should connect with its neigbor(like flipping a coin), using

```javascript
if(Math.random() >= 0.5)
    return;
```

The actual algorithm is quite simple as follows:

```javascript
//Store a and b's association
  if(rootOf(a)!=rootOf(b)) {
    //console.log(a + " : " + b);
    if(data[a]!=a) {
      if(data[b]==b) {
        data[b] = rootOf(data[a]);
      }else {
        //This is a second chance
        return;
      }
    }else {
      data[a] = rootOf(data[b]);
    }
  }
```

The rootOf function finds the root iteratively

```javascript
var rootOf = function(a) {
  while(data[a]!=a) a = data[a];
  return a;
}
```

While the implementation is over simplification of the algorithm, we can see it working quite well. However, for practical purposes, the algorithm can still be optimized depending on the needs.
