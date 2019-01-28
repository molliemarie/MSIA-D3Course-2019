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

Then, add the below d3 code anywhere within the script to add a title with an id `titleText` attached to the `h1` tag. 

```
d3.select('#titleDiv')
  .append('h1')
  .attr('id', 'titleText')
  .text('UFO Sightings in 2018')
```
 
 We could technically add this title without using D3. **What are reasons when adding a title using d3 might come in handy?**
 
# Moving on

Ok, we've finished created a plot that resembles the image at the top of the page! Now, let's go about adding some interactions! See new instructions [here](scatterWithInteractions.md)







