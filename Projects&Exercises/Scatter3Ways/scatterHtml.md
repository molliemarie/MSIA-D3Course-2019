# Creating a "scatter plot" using html

This is an exercise to practice basic HTML, SVG, and CSS syntax. It is also designed to demonstrate that something that _looks like_ a chart on a website is actually just a collection of simple HTML elements. The final example should look like this (you can also see a completed example [here](https://codepen.io/molliemarie/pen/RqmyaW).)

![simple html circles](imgs/scatterHtml.png)

You'll want to open up your `d3Scatter1.html` file in a text editor, and then perform the following steps (instructions are also in the file):

- Make a container `<div>` in which you'll render your content 
- Create a `<p>` element in which you write "My HTML Scatter" 
- Make a container `<svg>` element in which you'll place your circles 
- Set your svg's `width` to 300, and `height` to `400` 
- Put 3 `<circle>` elements inside of your `<svg>`, setting the properties for each one: 
    - `cx`: How far to move the circle in the `x` direction (right). Should be 100, 150, and 200. 
    - `cy`: How for to move the circle in the `y` direction (down from the top). Should be 100, 150, and 200. 
    - `r`: circle radius. Should be 10, 15, and 20. 

- In the `<style>` section, set `circle` elements to have a "fill" of whatever color you like.
