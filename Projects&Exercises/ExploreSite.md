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
d3.selectAll('.rateBubbleCircle').style('stroke', 'black')
```

