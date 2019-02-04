# Creating our first line plot

We've already created a scatter plot, so let's piggy back off that code, adding a line to this already created plot.

You can start with your most recent save of the scatter plot, or with the file `scatterCompleteExample.html`. 

1. Before worrying about transitions, let's add the line to our initial plot. We'll be using [`d3.line()`](https://github.com/d3/d3-shape#lines) to construct a new line generator. In the `ready` function, below all the scale and axes definitions, create your line like so:

```
  var lineGenerator = d3.line()
    .x(function(d) { return xScale(d.date)})
    .y(function(d) { return yScale(d.count)})

  svg.append('path')
    .attr('class', 'ufoLine')
    .attr('d', lineGenerator(startData));
```

Let's look into what's going on here. Below where you define `lineGenerator`, add the line 

`console.log(lineGenerator(startData));` 

and then check out the console in developer tools. What you see is the svg path mini-language consisting of "M"s and "L"s to indicate where the line starts and pivots. What d3.line is doing is converting your x and y values for you into this mini-language so that you don't have to! Read more on it [here](https://www.dashingd3js.com/svg-paths-and-d3js).

2. You'll notice that although you've created an outline of your line, it's also filled in with black, which is not quite what we want. To fix this, we'll need to do some css styling. In the css section (in `<style>` tag), add the following:

```
.ufoLine {
  fill: none;
}
```

Additionall, while we're here, let's style the line a bit also:

```
.ufoLine {
  /*removes black fill*/
  fill: none;
  /*styles line */
  stroke: darkgray;
  stroke-width: 3   ;
}
```

Feel free to experiment with these values.

3. The line is a bit rigid. Let's specify a curve using `d3.curve`(https://github.com/d3/d3-shape) to make the line flow a bit better:

```
  var lineGenerator = d3.line()
    .x(function(d) { return xScale(d.date)})
    .y(function(d) { return yScale(d.count)})
    .curve(d3.curveCardinal); //makes line 'curvy'
```

4. Now, we're ready to update the line by adding this code into the `dataSwap` function:

```
  svg.selectAll('.ufoLine')
    .transition()
    .ease(d3.easeElastic) //if you use the same ease here as you do with the circles, the line and circles will move together.
    .duration(transitionTime)
    .attr('d', lineGenerator(thisDataGroup));
```

Try using different ease functions, or commenting out the ease line to see what happens. Also, experiment with different transition times. 

Ok, let's test this out by refreshing and clicking on some buttons!

Uh oh! It breaks. **Any idea why? Check the console. What's the error message??**

5. If you look in the console, you'll see the error message `ReferenceError: lineGenerator is not defined`. This is an example of when we need to separate out a definition, defining the part of it that doesn't not require data outside of the ready function. First, let's definie `lineGenerator` outside of the `ready` function, like so:

```
var lineGenerator = d3.line()
    .curve(d3.curveCardinal);
```

Then, within the ready function, let's change our existing code to look like so:

```
  lineGenerator
    .x(function(d) { return xScale(d.date)})
    .y(function(d) { return yScale(d.count)})
```

And there we go - you've got yourself an updating line plot!

6. Post to [bl.ocks](https://bl.ocks.org/)
