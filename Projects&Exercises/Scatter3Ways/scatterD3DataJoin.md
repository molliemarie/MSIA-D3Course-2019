## Lab 3
This is an exercise to practice manipulating the DOM D3 using the **data-join**. The final example should look like this (you can also see the `complete.html` file for answers):

![simple html rectangles](imgs/complete.png)

You'll want to open up your `index.html` file in a text editor, and then perform the following steps (instructions are also in the file). All steps will be completed using d3.js, and should be completed in the `<script>` section at the bottom of the `index.html` file. Preliminary steps have been completed for you.

- Append 3 `rect` elements inside of your `<svg>` **using the data join**. To do this, you'll use the following syntax:

```js
// Select all rects in the svg and bind your data to the selection
var rects = svg.selectAll('rect')

// Determine what's new to the screen using `.enter()` and for each new element, append a rectangle
// Then, use the data provided to set the desired attributes
rects.enter()
    .append('rect')
    .attr('x', 0)
    .attr('height', 20)
    .attr('y', function(d,i){return d.y}) // use y attribute to drive layout
    .attr('width', function(d,i){return d.width}); // use width attribute to drive layout
    

```
    - `x`: How far to move the rectangle in the `x` direction (right). Should be `0` for all rectangles. 
    - `y`: How for to move the rect in the `y` direction (down from the top). Should be `10`, `40` `70` 
    - `width`: How far to draw the rectangle to the right. Should be `100`,`200`, `300` 
    - `height`: The vertical height of each rectangle. Should be `20` for all rectangles 
