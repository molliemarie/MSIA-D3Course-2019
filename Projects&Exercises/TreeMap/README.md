# Nested Data and Our First Tree Map

## The Data

As William S. Cleveland puts it in [Visualizing Data](https://books.google.com/books/about/Visualizing_Data.html?id=V-dQAAAAMAAJ):

 > In the early 1930s, agronomists in Minnesota ran a field trial to study the crop barley. At six sites in Minnesota, ten varieties of Barley were grown in each of two years. The data are the yields for all combinations of site, variety and year, so there are 6 x 10 x 2 =120 observations.

Open the data file `barley.tsv` ('tsv' stands for tab-separated values); look over the data and familiarize yourself with what we're about to plot.

You're finished product will look like this:


## Hierarchical Data and `d3.nest`

TKTK fill in with stuffs

## Steps:

1) First, we're going to use [`d3.nest`](https://github.com/d3/d3-collection#nests) to structure our data hierarchically, passing in site as the key. Our code will look like this:

```
    var nestedData = d3.nest()
        .key(function(d) { return d.site; })
        .entries(data);
```

so that you can take a look at what this looks like, add the following line below:

```
    console.log('nestedData', nestedData)
```
and then take a look at the console. You'll see the original data is already being printed in the console. Expand each and compare the structures.

2. Now, let's define a hierarchy for the data using [`d3.hierarchy`](https://github.com/d3/d3-hierarchy/blob/master/README.md#hierarchy) and print the resultant data to the console, like so:

```
    var root = d3.hierarchy({
        values: nestedData
    }, function(d) {
        return d.values;
    });

    console.log('root', root)
```

Go to your console and expand the data to take a look at the new structure.

3. Now, we're going to create a treemap function that will compute your layout given your  data structure using [`d3.treemap()`](https://github.com/d3/d3-hierarchy/blob/master/README.md#treemap), which "lays out the specified root hierarchy, assigning the following properties on root and its descendants:

- node.x0 - the left edge of the rectangle
- node.y0 - the top edge of the rectangle
- node.x1 - the right edge of the rectangle
- node.y1 - the bottom edge of the rectangle"

We'll start out by assigning [`treemap.size`](https://github.com/d3/d3-hierarchy/blob/master/README.md#treemap_size), which "sets this treemap layout’s size to the specified two-element array of numbers [width, height] and returns this treemap layout. If size is not specified, returns the current size, which defaults to [1, 1]" and [`treemap.tile`](https://github.com/d3/d3-hierarchy/blob/master/README.md#treemap_tile) which "sets the [tiling method](https://github.com/d3/d3-hierarchy/blob/master/README.md#treemap-tiling) to the specified function and returns this treemap layout. If tile is not specified, returns the current tiling method, which defaults to [d3.treemapSquarify](https://github.com/d3/d3-hierarchy/blob/master/README.md#treemapSquarify) with the golden ratio." We'll be assigning [`d3.treemapResquarify`](https://github.com/d3/d3-hierarchy/blob/master/README.md#treemapResquarify) because "this tiling method is good for animating changes to treemaps because it only changes node sizes and not their relative positions, thus avoiding distracting shuffling and occlusion."

Our code will look like this: 

```
    var treemap = d3.treemap() // function that returns a function!
        .size([width, height]) // set size: scaling will be done internally            
        .tile(d3.treemapResquarify)
        .padding(0); //padding between groups
```

4. Now, let's create a function that will reset the treemap. As you may have noticed in the `d3.treemap` documentation, "you must call [`root.sum`](https://github.com/d3/d3-hierarchy/blob/master/README.md#node_sum) before passing the hierarchy to the treemap layout."

```
    var setTreemap = function() {
        // Redefine which value you want to visualize in your data by using the `.sum()` method
        root.sum(function(d) {
            return +d['yield_' + year];
        });

        // (Re)build your treemap data structure by passing your `root` to your `treemap` function
        treemap(root);
    }
    
    // Now, call your set Treemap function
    setTreemap()    
```

5. Now, let's create an ordinal color scale. To do this we'll need to:

- get a list of sites from our nested data object. 
- Use `d3.scaleOrdinal` to create an ordinal scale. Your domain will be your site names and your range should be the colors stored in the variable `d3.schemeCategory10` (or you can choose from one of the other [d3 chromatic scales](https://github.com/d3/d3-scale-chromatic/blob/master/README.md#schemeCategory10)).

So, first, use `map` to get a list of sites. Since we're going to be passing in our nested data, we'll be returning `d.key` to the list. Like so:

```
    var sites = nestedData.map(function(d) {
        return d.key;
    });
```

Now, set an ordinal scale for colors:

```
    var colorScale = d3.scaleOrdinal().domain(sites).range(d3.schemeCategory10);
```

6. Ok, one more big step before we start seeing something on the page. We need to bind our data to a selection fo elements. Let's use the class `node` for this. The data that you want to join is array of elements returned by [`root.leaves()`](https://github.com/d3/d3-hierarchy/blob/master/README.md#node_leaves) (which returns the array of leaf nodes in traversal order; leaves are nodes with no children).

First, let's start by selecting all elements with class `node` and binding the data `root.leaves()`. Let's also print them to the console so that we can take a look:

```
    var nodes = div.selectAll(".node").data(root.leaves());

    console.log('nodes', nodes)
```

As you'll remember from earlier (and can see for yourself if you expand the `root` data or `nodes` in the console), each node has been assigned an `x0`, `x1`, `y0`, and `y1` value. (See step 3 to remind yourself what they are.)

Now, enter and append the elements and position them using the appropriate styles. We'll first append a `div`, assign the class `node`, and add text corresponding to the barley variety. Then use styles to assign `left`, `top`, `width`, and `height` - using our `x0`, `x1`, `y0`, and `y1` values - to assign the location and size of each rectangle. Lastly, we'll assign the `background` using the colorscale. 

```
    nodes.enter()
        .append("div")
        .attr('class', 'node')
        .text(function(d) {
            return d.data.variety;
        })
        .style("left", function(d, i) {
            return d.x0 + "px";
        })
        .style("top", function(d) {
            return d.y0 + "px";
        })
        .style('width', function(d) {
            return d.x1 - d.x0 + 'px';
        })
        .style("height", function(d) {
            return d.y1 - d.y0 + "px";
        })
        .style("background", function(d, i) {
            return colorScale(d.data.site);
        });
```
