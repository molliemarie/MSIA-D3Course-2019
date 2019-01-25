## Lab 2
This is an exercise to practice manipulating the DOM with D3. The final example should look like this (you can also see the `complete.html` file for answers):

![simple html rectangles](imgs/complete.png)

You'll want to open up your `index.html` file in a text editor, and then perform the following steps (instructions are also in the file). All steps will be completed using d3.js, and should be completed in the `<script>` section at the bottom of the `index.html` file.

- Select your `body` and append a `div` element in which you'll render your content. To do this, you'll use the `d3.select()` method, and then the `.append()` method to append your element to your selection.
- Append a new `p` element to the `div` you just created, and use the `.text()` method to set the text to "My Bar Chart"
- Append a container `svg` to your `div` element in which you'll place your rectangles 
- Set your svg's `width` to 300, and `height` to `400` 
- Append 3 `rect` elements inside of your `<svg>` (one at a time), setting the properties for each one. We'll improve on this process later: 
    - `x`: How far to move the rectangle in the `x` direction (right). Should be `0` for all rectangles. 
    - `y`: How for to move the rect in the `y` direction (down from the top). Should be `10`, `40` `70` 
    - `width`: How far to draw the rectangle to the right. Should be `100`,`200`, `300` 
    - `height`: The vertical height of each rectangle. Should be `20` for all rectangles 
