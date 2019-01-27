# Your first D3.js scatter plot!

## Day One:

For this first project, we're going to use data from [The National UFO Reporting Center](http://www.nuforc.org/), a website where people can report UFO sightings.

We'll make a fairly basic plot with no interactions to start that will end up looking something like this:

![static scatter](imgs/staticScatter.png)

**Note:** Since Mike doesn't technically make valid HTML pages, we're not going to either — here's how we'll start our own empty HTML pages from here on out:

  ```
  <!DOCTYPE html>
  <meta charset="utf-8">

  <style type="text/css">
    /*css to go here*/
  </style>

  <body></body>

 <script src="https://d3js.org/d3.v4.min.js"></script>
 
  <script>
    //JS to go here
  </script>

  ```
  
  **Steps:**
  
1. First, open the file `scatterStarter.html` and fork it. You'll notice the code is set up like above, plus I've added styling that will add a pink border around any svg we might draw.

  ```
  svg {
    border: 1px solid #f0f;
  }
  ```
  
2. think about the format we want our data in.
3. Add an SVG element of width 720 and height 400.
4. Using a data join, add a `circle` for every element of our array. Give it a radius 5 and a class, `ufoCircle'. Inspect it in the console. (You can pull up a Javascript console by clicking console in the bottom left corner of the pen)
 5. Position the circles based on their `cx` and `cy` attributes. How does SVG interpret these positions?
 6. We'll need to add a [scale](https://github.com/d3/d3-scale/blob/master/README.md).
 7. Let's label the count of each using text. At this point, we'll talk about how we can avoid this second data join and make our code a bit more efficient.
 8. Redo the original join, using groups first, then appending circles and text. Note SVG [transformation](http://www.w3.org/TR/SVG/coords.html) documentation, which is not that fun. 
 9. Maybe [axes](https://github.com/d3/d3-axis/blob/master/README.md) are in order?  
 10. We might need to move our axes around. We'll go through the math. Also, maybe add some styles?
 11. The margins are a problem, and they will always be. Here's [a great trick](https://bl.ocks.org/mbostock/3019563) we'll use on every chart we make from here on out.
 
Alright, we've done a lot! What we have should look pretty similar now to the plot above. Unfortunately, though, our chart isn't really better than what we get with the free tools. (Paste your data into [Chartbuilder](http://quartz.github.io/Chartbuilder/) and feel free to weep.) What makes D3 different is its ability to create **dynamic** and **custom** visualizations and things that tools aren't designed to create. To demonstrate this, tomorrow (and today if time) we will add some interactions to the plot to make it a bit more interesting!
 
 ## Day Two: 
 
Last class, we finished our first scatter pot! Let's start where we left off, adding some stylings and interactions! Open `class1Final.html` for our starting point today.

12. Right now, the data is provided for us in the file. Let's change this so that we're loading in the data from an outside file. D3 let's us do this easily with [d3-request](https://github.com/d3/d3-request)! The data we have is in csv (comma separated value) form, so we'll be using the `d3.csv()` module.

To load and parse a csv file: 

```
d3.csv("/path/to/file.csv", function(error, data) {
  if (error) throw error;
  console.log(data); // [{"Hello": "world"}, …]
});
```

What it's doing is loading the csv file, and then when the file is fully loaded, running the rest of the code. Additionally, rather than inserting an unnamed function into the module, I often name it for easier legibility. In our case, let's put our code into a function `ready()` and then call that function in the `d3.csv()` module:

```
d3.csv("data/ufo.csv", ready)

function ready(error, data) {

  if (error) return console.warn(error);
  
  ... rest of code (the stuff we've already written) here ...
  
}

```
 
13. Now that we've loaded in all of the data, all of the data is showing up! For starters, let's filter out everything that isn't 2018.   We'll need an easy way to check for this, so first let's create a year value in our data formatting loop using `getYear`.
 
  `d.year = d.date.getYear() + 1900`

   Curious as to why we add 1900 to the value? Read about the method [here](https://www.tutorialspoint.com/javascript/date_getyear.htm). 
 
   Next, let's create a new veriable `startData` that only includes 2018 data, using Javascript's [`filter`](https://alligator.io/js/filter-array-method/) method.
 
 `var startData = data.filter(function(d) { return d.year == 2018; })`
 
 You'll then need to replace data thoughout the file with `startData` for it to update.
 
14. The axis is still referring to month values. Let's change this so that it's reading the values in as dates. Use [`d3.timeparse()`]() to format the date. (You'll need to create a variable `parseTime` and then call that within the formatting for loop to create `d.date`.
 
 Then, we'll want to change our x axis from a linear scale to a time scale using [`d3.scaleTime()`](https://github.com/d3/d3-scale#scaleTime). 
 
15. You might have noticed that this broke the code. Our new time scale will no longer understand our originally set domain. This is a good opportunity to make the domain calculation a bit more dynamic. We'll  use [`d3.extent()`](https://github.com/d3/d3-array#extent) to obtain the min and max values of the data. We will end up with something like this:
 
  ```
  var xScale = d3.scaleTime()
    // .domain([1,12])
    .domain(d3.extent(startData, function(d) { return d.date; }))
    .range([0, width]);
  ```

16. We might as well apply `d3.extent()` to the `yScale` domain as well. Do that now. 

17. Let's style the chart to match the image above, starting with gridlines. Things like [`tickSize()`](https://github.com/d3/d3-axis/blob/master/README.md#axis_tickSize) might help. For example, adding a gridline to the x axis would look like this:

```
var xAxis = d3.axisBottom(xScale)
  .tickSize(-height);
```

If you don't understand why I'm inputting the variable `height`, try inputting different numbers to see what happens.

18. Now, add gridlines to the y axis. Think about what variable would be the correct input here.

19. Now, let's add some further styling, starting with the styling of the gridlines. Inside the `<style>` tag, add the following stylings. Refresh your page after each addition to see what it's doing. **What does line, path, and text refer to here?**

```
.axis line {
  stroke-width:1px;
  stroke: #ccc;
  stroke-dasharray: 2px 2px;
}

.axis text {
  font-size: 12px;
  fill: #777;
}

.axis path {
  display: none;
}
```

20. Let's style the circle label text by also adding the folling into the `<script>` tag.

```
.ufoGroup text {
  fill: #aaa; /*grey out text*/
  font-size: 11px;
}

```

21. In addition to adding styling using css (in the `<script>` tag), you can also add styling in javascript using d3. For example, let's change the color of the circles using d3. In the code where you added your circles (towards the bottom of the page), add the line `.style('fill', '[color of choice]'). Here's an example:

```
ufoGroup.append('circle')
  .attr('class', 'ufoCircle')
  .style('fill', 'limegreen') // you can also use hashcodes instead of a color name
  .attr('r', 10);
```

**When might we want to use d3 to do this rather than css?**

22. Now, Let's add a title! First, let's add a div with the id `titleDiv` within the `<body>` tags. 

```
<body>
  <div id='titleDiv'></div>
</body>

```

Then, add the below d3 code anywhere within the script to add a title. 

```
d3.select('#titleDiv')
  .append('h1')
  .text('UFO Sightings in 2018')
```
 
 We could technically add this title without using D3. **What are reasons why adding a title using d3 might come in handy?**
 
 ### Adding interactions
 
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
1. Let's start writing the dataSwap function. This function will take in a year and filter the full data to return just the data from that data year. Put this function outside of / above the `ready` function. 

```
function dataSwap(datasetGroup, xScale, yScale) {

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



