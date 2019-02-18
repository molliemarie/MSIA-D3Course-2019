
# Adding General Update Pattern to Scatter Line Plot

Now that we have an understanding of d3's general update pattern, let's put it into practice! We'll be doing this by editing our scatter / line plot we made last month. Go to the `Projects&Exercises/generalUpdatePattern/` folder and open `UpdatingScatterLineStarter.html` to start.

## Steps

First of all, let's try start by trying to apply the basic structure of the general update pattern. 

1. Right now, if we look in our dataSwap function, we see the following:

```
  svg.selectAll('.ufoGroup')
    .data(thisDataGroup)
    .transition()
    .ease(d3.easeElastic)
    .duration(transitionTime)
    .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})
```

Let's start by splitting this up into update, enter, and exit sections. 

```
  // binding our new data
  var groups = svg.selectAll('.ufoGroup')
    .data(thisDataGroup)

  // grab our enter selection, attatch new groups
  groups
    .enter().append('g')

  // 
  //Merge update applies changes to entering AND updating groups
  groups
    .merge(groups)
    .transition()
    .ease(d3.easeElastic)
    .duration(transitionTime)
    .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})


// remove exit selection
  groups.exit().remove()
```

2. Ok, so it's not working right yet, but that's ok. Let's notice what's wrong. 1) The dots aren't coming back when you select a year after selecting "2019". 2) the dot seems to travel from January to December. 

The second issue is an easy fix, so let's start there. We'll fix this by assigning the data a key as we bind it. See [`selection.data()`](https://github.com/d3/d3-selection#selection_data), which says: 

"If a key function is not specified, then the first datum in data is assigned to the first selected element, the second datum to the second selected element, and so on. A key function may be specified to control which datum is assigned to which element, replacing the default join-by-index, by computing a string identifier for each datum and element. This key function is evaluated for each selected element, in order, being passed the current datum (d), the current index (i), and the current group (nodes), with this as the current DOM element (nodes[i]); the returned string is the element’s key. The key function is then also evaluated for each new datum in data, being passed the current datum (d), the current index (i), and the group’s new data, with this as the group’s parent DOM element; the returned string is the datum’s key. The datum for a given key is assigned to the element with the matching key. If multiple elements have the same key, the duplicate elements are put into the exit selection; if multiple data have the same key, the duplicate data are put into the enter selection."

So, let's assign a key when binding our data by swapping out our .data() line in the dataSwap function with the following:

```
.data(thisDataGroup, function(d) { return d.month })
```

In doing this, we've made sure that a circle assigned to the month of January will still be assigned to the month of January when the new data is added, and same for all other months. 

Now you'll notice that when you click the "2019" button, and then click another button, the bubble is at least staying in January. 

3. Now, when we click to "2019" and then back to "2018, why are the other bubbles not showing up?

Simple - we haven't defined them! The way we had this set up originally, we didn't have to re-create any bubbles because we weren't removing them. Now that we're removing bubbles (whe selecting "2019" for instance), we're going to have to recreate the bubbles like so:

```
  enterGroups = groups
    .enter().append('g')
    // will need to assign class and location to incoming data groups
    .attr('class', 'ufoGroup')
    .attr('transform', function(d) { return 'translate(' + xScale(d.date) + ',' + yScale(d.count) + ')'})
    
  // add new circles
  enterGroups.append('circle')
    .attr('class', 'ufoCircle')
    .style('fill', 'limegreen')
    .attr('r', radius)

  // add new text
  enterGroups.append('text')
    .attr('class', 'ufoText')
    .attr('dx', radius)
    .attr('dy', -radius)
    .text(function(d) { return d.count})
    .style('opacity', 0)
```

4. Do you see anything missing?

We're creating new bubbles now, but we didn't keep any of the interactions that we defined when we originally created the bubbles. For now, copy and paste the `mouseenter` and `mouseleave` interactions and apply them to your newly created groups. (See `steps3.html` if you need help in doing so.)

5. Ok, so we have this working well with the update pattern, but now we've created some redundancies, haven't we? 

What we'll want to do now is create a drawCircles function that will draw and move the circles. Then, we can call that function from both places! We'll want to use the code from the `dataSwap` function, as it applies the general update pattern.

If you need help in doing this, look at code in `steps4.html` for guidance.

6. You'll likely now notice that there are a few other redundancies in our code, the most obvious of which is that we currently set our scales and our axes in two different locations - within the `ready` function and within the `dataSwap` function. Let's create functions `setScales` and `setAxes`, and then call them from the two locations. 

If you need help in doing this, look at code in `steps4.html` for guidance.


## Further thoughts:

Congratulations! You've successfully utilized d3's general update pattern! :D

Now, I want you to look at a [similar example of this plot](https://bl.ocks.org/molliemarie/3cba938e0485b105fdde92992a169f83). It uses the same data, but you'll notice it has one major difference - the y axis doesn't change. Things to think about:

- When might you want to have the full axis visible at all times? What comparisons would a static axis stress?
- When is it better to update the axis? What comparisons would a static axis stress?

Write your answers down. We will either discuss this near the end of class, or at the start of next class.
