# Creating our first line plot!

We've already created a scatter plot, so let's piggy back off that code, adding a line to this already created plot.

You can start with your most recent save of the scatter plot, or with the file `scatterCompleteExample.html`. 

1. Before worrying about transitions, let's add the line to our initial plot. We'll be using [`d3.line()`](https://github.com/d3/d3-shape#lines) to construct a new line generator. In the `ready` function, below all the scale and axes definitions, create your line like so:

```
  var lineGenerator = d3.line()
    .x(function(d) { return xScale(d.date)})
    .y(function(d) { return yScale(d.count)})

  svg.append('path')
    .attr('class', 'line')
    .attr('d', lineGenerator(startData));
```

Let's look into what's going on here. Below where you define `lineGenerator`, add the line `console.log(lineGenerator(startData));` and then check out the console in developer tools. What you see is the svg path mini-language consisting of "M"s and "L"s to indicate where the line starts and pivots. What d3.line is doing is converting your x and y values for you into this mini-language so that you don't have to! Read more on it [here](https://www.dashingd3js.com/svg-paths-and-d3js).

2. You'll notice that although you've created an outline of your line, it's also filled in with black, which is not quite what we want. 






However, let's start by defining lineGenerator outside of the `ready` function so that it can be easily used in other functions, like so:

`var lineGenerator = d3.line()`

and then with the ready function, below all the scale and axes definitions, finish creating your line like so:

```
  lineGenerator
    .x(function(d) { return xScale(d.date)})
    .y(function(d) { return yScale(d.count)})

  svg.append('path')
    .attr('class', 'line')
    .attr('d', lineGenerator(startData));
```
