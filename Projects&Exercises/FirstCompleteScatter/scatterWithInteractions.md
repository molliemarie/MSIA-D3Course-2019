# Adding interactions to our scatter!
 
Ok, we've finished created a plot that resembles the image at the top of the page! Now, let's go about adding some interactions!
 
First, we will add some hover interactions. Let's make the following three things happen on hover:

- text appear (first, we'll need to remove the text, so that it only appears on hover)
- circle size increase 
- dull circles not being hovered on

#### Hover text
There are a couple ways we could do this. We could either remove the text completely, and then generate the text upon hover OR we could make the text invisible by default, make it visible only upon hover. Let's go the latter route. 

1. First, let's make the opacity of all text 0, by chaining `.attr('opacity', 0)` onto the text code, like so:

```
ufoGroup.append('text')
  .attr('class', 'ufoText')
  .attr('dx', 10)
  .attr('dy', -10)
  .text(function(d) { return d.count})
  .attr('opacity', 0);
```

2. Now, let's make the text visible upon hover. To do this, we will chain a [`mousenter`](https://developer.mozilla.org/en-US/docs/Web/Events/mouseenter) event to the  ufoGroup creation code:

```
  var ufoGroup = svg.selectAll('.ufoGroup')
    .data(data).enter().append('g')
    .attr('class', 'ufoGroup')
    .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})
    .on('mouseenter', function(d) {
    
      // add code here to make text visible
    
    })
  ```

This is a good time to talk about the Javascript keyword [`this`](https://www.w3schools.com/js/js_this.asp), which refers to the object it belongs to. For example, insert the following code

```
    .on('mouseenter', function(d) {

      var thisThis = d3.select(this)

      console.log(thisThis)

    })
```
Then, open your console and start hoving over bubbles. You'll notice that each time you hover, a json is printed in the console. If you expand the outputs, you'll notice they are describing the bubble over which you just hovered. Therefore, by selecting `this`, we are basically grabbing that bubble so that we can make changes to just specific bubble.

Now, let's select `this` and make the text reappear!

```
    .on('mouseenter', function(d) {

      d3.select(this)
        .select('text')
        .style('opacity', 1)

    })
```

You'll notice the text will now appear when a bubble is hovered over. Let's go one step further and utilize the [`d3-transition`](https://github.com/d3/d3-transition). A transition is a selection-like interface for animating changes to the DOM. Instead of applying changes instantaneously, transitions smoothly interpolate the DOM from its current state to the desired target state over a given duration. It's pretty great!  Try adding the following:

```
    .on('mouseenter', function(d) {

      d3.select(this)
        .select('text')
        .transition()
        .style('opacity', 1)

    })
```

Nice, right!? 

Ok, now we've got the text appearing, but it's not dissapearing! We'll need to add a `mouseleave` event.

3. Make text dissapear on `mouseleave`. We will do this by adjusting the text's [`opacity`](https://www.w3schools.com/cssref/css3_pr_opacity.asp) property, which ranges from 0 (invisible) to fulling opaque (1). Like so:

```

  var ufoGroup = svg.selectAll('.ufoGroup')
    .data(data).enter().append('g')
    .attr('class', 'ufoGroup')
    .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})
    .on('mouseenter', function(d) {

      d3.select(this)
        .select('text')
        .transition()
        .style('opacity', 1)

    })
    .on('mouseleave', function(d) {
    
    // NEW CODE HERE:

      d3.select(this)
        .select('text')
        .transition()
        .style('opacity', 0)
        
    });
```

### Increase circle size on hover
4. Increasing hte circle size on hover will look similar. Let's double the size of the circle on hover by adding the following code into the `mouseenter` event function:

```
      d3.select(this)
        .select('circle')
        .transition()
        .attr('r', radius*2)
```

Additionally, if you'd like the transition to take a specific amount of time, you can use `.duration()` to assign this, where one second equals a value of 1000. Try chaining [`.duration`](https://github.com/d3/d3-transition#transition_duration) into the above code. Experiment with different values! Like so:

```
      d3.select(this)
        .select('circle')
        .transition()
        .duration(1000) // can experiment with different values
        .attr('r', radius*2)
```

**Note: Moving forward, I encourage you to create the variable transitionTime at the top of the script, calling that value in `duration` from now on. **

Ok, let's add one more thing to this section before moving on - [`.ease`](https://github.com/d3/d3-transition#transition_ease), which specifies the transition easing function. Below is an example, but I encourage you to experiment with various easing functions. 

```
      d3.select(this)
        .select('circle')
        .transition()
        .ease(d3.easeElastic)
        .duration(transitionTime) //make sure to define this variable at top of script with other variables
        .attr('r', radius*2)
```

5. The circles now increase in size upon hover, but we need to make them go back to normal after. Try adding code into the `mouseleave` event that will do this. **hint:** It will look similar to the above code.

#### Adjust opacity of circles on hover

Now, let's make it so that when a circle is hovered  upon, all other circles are partly transparent. We do this by adjusting the `opacity` property. 

We could do this by either 1) selecting all circles besides the one being hovered upon and lowering their opacities or 2) lowering all circle opacities, and then specifying that the hovered upon circle is set to 1. The second is a bit easier, so let's do that.

6. First, set all circles to half opacity (value of 0.5) by adding the following code to the `mouseenter` event:

```
      d3.selectAll('circle')
        .style('opacity', 0.5)
```

7. Then, to make the selected circle fully opaque, simply chain one line (`.style('opacity', 1)`) to the already existing circle transition code, like so:

```
      d3.select(this)
        .select('circle')
        .transition()
        .ease(d3.easeElastic)
        .duration(1000)
        .attr('r', radius*2)
        .style('opacity', 1); //THIS LINE
```

8. Now, we probably want to make it so that when no bubble is being hovered upon, all bubbles are fully opaque. **What code would we add to the `mouseleave` event to make this happen?**

## Updating the Data

We've now successfully added some hover events! This already make our plot smoother, but let's go even further and make it possible to select different years and have the circles update. 

Goal: 
- click on button w/title and see data for just that group. For example, click on button that says group 1, and see just group 1 data plotted.

We'll need:
- buttons
- a data swap function that will:
  - take a value (like 2018)
  - transition the existing dots to the new locations
  - update the title
  
  
#### Start dataswap function
1. Let's start writing the dataSwap function. This function will take in a year and filter the full data to return just the data from that data year. For now, put this function inside the `ready` function at the top, below where you defined the variable `transitionTime` (since we'll want to use that). **Note:** It would be more proper for this function, as well as many variable definitions, to be outside of the `ready` function, but we'll worry about that later. For now, let's leave it all inside.

```
function dataSwap(datasetGroup) {

  //filters data using new input year
  var thisDataGroup = data.filter(function(d) { return d.group == datasetGroup})
  
}
```

#### create buttons

Let's create buttons with each year on it. To do this we'll need to:

- create an array of years included in the data
  - will first need to create a year variable within the data
- create a div with id `buttonsDiv`
- Use the array of years to create data buttons within `buttonsDiv`

2. Before we create an array of years included in the data, let's create a year variable within the data. To do this, we will use [`d3.timeParse()`](https://github.com/d3/d3-time-format):

```
  var parseTime = d3.timeParse("%m/%Y"); //CONVERTS STRING TO DATE

  data.forEach(function(d) {
    d.count = +d.count;
      d.month = d.date.split('/')[0];
      d.date = parseTime(d.date);
      d.year = d.date.getYear() + 1900 //CREATES YEAR VARIABLE WITHIN DATA
  });
```

Curious as to why we add 1900 to the year? Read more about `getYear` [here](https://www.tutorialspoint.com/javascript/date_getyear.htm) to find out.

3. Create an array of unique years. We'll do this using [`d3.set()`](https://github.com/d3/d3-collection#set) and [`map`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map). 

Before doing this, let's do some experimenting in the console. Copy and paste each of these lines into the cosole and see what it spits out: 

```
data.map(function(d) { return d.year })

d3.set(data.map(function(d) { return d.year }))

d3.set(data.map(function(d) { return d.year })).values();
```

**What's going on here? What is each part of this line (`map`, `set`, and `.values()`) doing?**

Now, let's create our yearList:

```
var yearList = d3.set(data.map(function(d) { return d.year })).values();
```

4. Let's add a div with the id `buttonsDiv` to the `body` so that we have somewhere specific to add the buttons.

```
<body>
  <div id='titleDiv'></div>
  <div id="buttonsDiv"></div> //FOR BUTTONS
</body>
```

5. Use our `yearsList` to add buttons for each year to the appropriate div.

```
  d3.select('#buttonsDiv') //select div with id buttonsDiv
    .selectAll('button') //look for existing buttons
    .data(yearList) //bind data (year list)
    .enter().append('button') //append buttons for each data point
    .text(function(d) { return d; }) //append text to each button that corresponds to the data point (Which is a year)
```

And we now have buttons!! But, they don't do anything... Let's add a click event to test them out.

```
  d3.select('#buttonsDiv')
    .selectAll('button')
    .data(yearList)
    .enter().append('button')
    .text(function(d) { return d; })
    //ADD THIS:
    .on('click', function(d) {
      console.log(d) //<-- test buttons. Should print out the year associated with pressed button
    })
```

Now when you click the buttons, you should see the year appear in the console.

### Finish and call dataSwap function

Now that we have functioning buttons, we can make the dataSwap function work!

6. We'll start by finishing the function. We need the function to move the ufoGroups to their new locations when a button is pressed. We'll do this by selecting all elements with the class `.ufoGroup`, binding the new data, and defining a new `transform, translate` location attribute.

```
function dataSwap(datasetGroup) {

  var thisDataGroup = data.filter(function(d) { return d.group == datasetGroup})
  
  //NEW CODE:
  svg.selectAll('.ufoGroup')
    .data(thisDataGroup)
    .transition()
    .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})

}
```

7. You'll notice the buttons still don't work. This is because we have not yet called the function. We'll do this by adding an `on click` event to the buttons, like so:

```
  d3.select('#buttonsDiv')
    .selectAll('button')
    .data(yearList)
    .enter().append('button')
    .text(function(d) { return d; })
    // ON CLICK EVENT, CALLS dataSwap:
    .on('click', function(d) {
      dataSwap(d) 
    })
```

8. Ok, so it still doesn't quite work. Before reading on, **do you have any idea why the bubbles are dissapearing?**

Earlier on, we based the domain of the scales on the startData for both x and y. So, in the dataswap function, let's add these lines to define the scales based on the new data.

```
  xScale
    .domain(d3.extent(thisDataGroup, function(d) { return d.date; }));

  yScale
    .domain(d3.extent(thisDataGroup, function(d) { return d.count; }));
```

9. You'll notice that the bubbles now move to the right location, but the axes are not updating. This is because we still need to update the axes.

```
    // attaches new scales to the axes
    xAxis.scale(xScale);

    yAxis.scale(yScale);
    
    // Updates the axis groups, and therefore the labels
    xAxisGroup.call(xAxis);

    yAxisGroup.call(yAxis);
```

10. We still need to change the title to match the new data. You can do this by adding the following code at the end of the `dataSwap` function:

```
    d3.select('#titleText')
      .text('UFO Sightings in ' + datasetGroup);
```

11. The axes are now update correctly, but they're very abrupt. Let's a add some transitions, defining durations and easing. I encourage you to experiment with these, but here's the complete `dataSwap` function showing an example of what could be done with `transition`, `duration`, and `easing`:

```
function ready(error, data) {

  if (error) return console.warn(error);

  function dataSwap(datasetGroup) {

    var thisDataGroup = data.filter(function(d) { return d.year == datasetGroup});

    console.log(thisDataGroup);

    xScale
      .domain(d3.extent(thisDataGroup, function(d) { return d.date; }));

    yScale
      .domain(d3.extent(thisDataGroup, function(d) { return d.count; }));

    xAxis.scale(xScale);

    yAxis.scale(yScale);

    xAxisGroup
      .call(xAxis);

    yAxisGroup
      .transition()
      .duration(transitionTime)
      .call(yAxis);
    
    svg.selectAll('.ufoGroup')
      .data(thisDataGroup)
      .transition()
      .ease(d3.easeElastic)
      .duration(transitionTime)
      .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})

    d3.select('#titleText')
      .text('UFO Sightings in ' + datasetGroup);
  }

```

12. Last, it's actually best to move your `dataSwap` function as well as variables that do not require data outside of the `ready` function. (**Note:** If you only move the `dataSwap` function out and no other variables, you'll notice you'll need to pass in many variables to get the function to work. Intead, it's easist to keep everything unnecessary to the function outside of the function.)

I urge you to walk through the completed example of this plot and take note of what's been moved. You can do this by opening `scatterCompleteExample.html` (recommended) or this [markdown file](finalScript.md)

**What was moved? What definitions were separated into sections? Why do you think that is?
