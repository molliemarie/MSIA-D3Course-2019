# Welcome to Day Five! :D

## Agenda:

- Announcements
- General Update Pattern Review
- Hierarchy review
- Other things to be aware of
- Debugging reveiw
- Resources to keep in mind


## Accouncement: Data Visualization Society Launch!

[Elijah Meeks](https://twitter.com/Elijah_Meeks), [Amy Cesal](https://twitter.com/AmyCesal), and I launched the [Data Visualization Society](https://www.datavisualizationsociety.com/) on Wednesday. Here's [an article introducing the society](https://medium.com/data-visualization-society/introducing-the-data-visualization-society-d13d42ab0bec) and explaining why we felt it important to create this organization. If this is something you're interested in joining, you can do so [here](https://www.datavisualizationsociety.com/join).

At the time I wrote this (Thursday at 6pm), we had over 1,500 people already signed up. Very exciting!

## Review General Update Pattern

Let's take a look at the finished line/scatter plot with the general update pattern applied. 

## Hierarchy

Let's discuss hierarchy, when it applies, and some examples of d3 plots for which you need to format your data in a hierarchical way (using [`d3.nest`](https://github.com/d3/d3-collection#nests)). We'll start by looking at [this doc from last class](/Projects&Exercises/TreeMap/).

Some other examples:

- [tree map](https://blockbuilder.org/mbostock/8fe6fa6ed1fa976e5dd76cfa4d816fec)
- [sunburst](https://blockbuilder.org/EE2dev/4153ee8eafb5a27d32588b12877a0ea7)
- [circle packing](https://blockbuilder.org/mbostock/4063530)
- [zoomable circle packing](https://blockbuilder.org/mbostock/7607535)
- [tree of life](https://blockbuilder.org/mbostock/c034d66572fd6bd6815a)
- [radial cluster dendogram](https://blockbuilder.org/mbostock/4339607)
- [crazy tree map thing](https://blockbuilder.org/mbostock/4341134)
- [force-directed graph](https://blockbuilder.org/mbostock/4062045)
- [draggable force-directed graph](https://blockbuilder.org/mbostock/4557698)

## Other things to be aware of
- [`d3-queue`](https://github.com/d3/d3-queue)
  - allows you to load in multiple files at once. Delays loading of plots until all are loaded.
  - [example](https://blockbuilder.org/mbostock/1696080)
- [`d3-zoom`](https://github.com/d3/d3-zoom)
  - Allows you zoom in on a plot and pan by clicking and dragging
  - [example](https://blockbuilder.org/mbostock/d1f7b58631e71fbf9c568345ee04a60e)
  - [example with automatic transitions](https://blockbuilder.org/mbostock/b783fbb2e673561d214e09c7fb5cedee)
- [`d3-brush`](https://github.com/d3/d3-brush)
  - Brushing allows you to select a region using a pointing gesture, such as by clicking and dragging the mouse. 
  - Often used to select discrete elements, such as dots in a scatterplot or files on a desktop. 
  - It can also be used to zoom-in to a region of interest.
  - [brushable network](https://blockbuilder.org/mbostock/4560481)
  - [re-centering brush](https://blockbuilder.org/mbostock/6498000)
  - [Brush and Zoom 1](https://blockbuilder.org/mbostock/34f08d5e11952a80609169b7917d4172)
  - [Brush and Zoom2](https://blockbuilder.org/mbostock/f48fcdb929a620ed97877e4678ab15e6)
- legends
  - [example](https://blockbuilder.org/mbostock/3887051)
  - [example 2](https://blockbuilder.org/mbostock/1341679)
- [x-value mouseover example](https://bl.ocks.org/mbostock/3902569)

## Debugging

## Resources to keep in mind

- [blocksbuilder](https://blockbuilder.org/search) - great tool for finding examples!
- [Interactive Data Visualization for the Web](https://www.amazon.com/Interactive-Data-Visualization-Web-Introduction/dp/1491921285/ref=pd_lpo_sbs_14_t_0?_encoding=UTF8&psc=1&refRID=E4SJ53PCAA5KA87ARSH5) by Scott Murray. Best book I've seen on d3.
- [DashingD3](https://www.dashingd3js.com/) has a number of free tutorials. (Although some are only accessible with a membership.)
- [Jim Vallandingham](https://vallandingham.me/) has some great tutorials.
- [Nadieh Bremer](https://www.visualcinnamon.com/blog/) has created some several blogs on some of her fancier techniques. 
- Additional resources [here](resources.md)

## Class Project time!

Get in your class project groups. Let's see what you got! :)



