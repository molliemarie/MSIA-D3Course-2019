# Nested Data and Our First Tree Map

## The Data

As William S. Cleveland puts it in [Visualizing Data](https://books.google.com/books/about/Visualizing_Data.html?id=V-dQAAAAMAAJ):

 > In the early 1930s, agronomists in Minnesota ran a field trial to study the crop barley. At six sites in Minnesota, ten varieties of Barley were grown in each of two years. The data are the yields for all combinations of site, variety and year, so there are 6 x 10 x 2 =120 observations.

Open the data file `barley.tsv` ('tsv' stands for tab-separated values); look over the data and familiarize yourself with what we're about to plot.

You're finished product will look like this:


## Hierarchical Data and `d3.nest()`

TKTK fill in with stuffs

## Steps:

1) First, we're going to use [`d3.nest()`](https://github.com/d3/d3-collection#nests) to structure our data hierarchically, passing in site as the key. Our code will look like this:

```
    var nestedData = d3.nest()
        .key(function(d) { return d.site; })
        .entries(data);
```

so that you can take a look at what this looks like, add the following line below:

```
    console.log('nestedData', nestedData)
```
and then take a look at the console.
