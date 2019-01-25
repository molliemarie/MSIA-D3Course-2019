# Using D3.js to explore a site

D3 is not intended just for data visualization; like jQuery, it's useful for DOM selection and manipulation, or simply for creating structured HTML pages. It's also public-facing on more web sites than you think. 

Let's explore some of its features on the [Illinois Traffic Stops site](https://illinoistrafficstops.com/).

1. Open developer tools. This is different for different browser. In chrome you select view --> Developer --> Developer tools
2. Type these into your console and explore the results:

 ```
 d3.select('.rateBubbleCircle')
 d3.selectAll('.rateBubbleCircle')
 
```

3. Let's add a stroke to all the elements with the class `rateBubbleCircle`. 

```
d3.selectAll('.rateBubbleCircle').style('stroke', 'black')
```

4. Let's change the color of all the elements with class `rateBubbleCircle` and class `black`. 

Hint, selecting items using two classes looks something like this:

```
d3.selectAll('.class1.class2')
```

So, to do this, you'll need to type:

```
d3.selectAll('.rateBubbleCircle.black').style('fill', 'yellow')
```

5. select all circles and change the radius.

```
d3.selectAll('.rateBubbleCircle').attr('r', '5')
```

Feel free to experiment with different numbers.

6. Let's append an `h1` tag to the div with the id `all-stop-section`. (The new h1 tag will be added below the first waffle plot.)

```
d3.select('#all-stop-section').append('h1').attr('id', 'newh1')
```
Inspect the page and find the new h1 tag we added. Now let's add text to the new h1 tag and see it appear on the page.

```
d3.select('#newh1').append('text').text('NEW TEXT')
```

7. Let's practice expecting elements. Inspect one of the bars in the bar plot by right clicking on the bar and selecting "inspect". What classes does that bar have?

8. Try selecting all bars, or a subset of the bars (your choice), and changing something about them. You can start with what we've already experimented with, but feel free to experiment.
