# ngraph.centrality [![Build Status](https://travis-ci.org/anvaka/ngraph.centrality.svg)](https://travis-ci.org/anvaka/ngraph.centrality)

Library computes centrality for entire graph and returns object, where keys are
nodes' identifiers and values are centrality values:

``` javascript
{
  node_1: centrality_value_for_node_1,
  node_2: centrality_value_for_node_2
  // ...
}
```

# usage

## [Degree centrality](https://en.wikipedia.org/wiki/Centrality#Degree_centrality)

``` javascript
var centrality = require('ngraph.centrality');
var g = require('ngraph.graph')();

// Let's build a simple graph:
g.addLink('fortran', 'c');
g.addLink('c', 'c++');
g.addLink('c++', 'perl');
g.addLink('c', 'javascript');

// this will consider graph as undirected:
var degreeCentrality = centrality.degree(g);

/*
degreeCentrality is:
{
  "fortran": 1,
  "c": 3,
  "c++": 2,
  "perl": 1,
  "javascript": 1
}
*/

// This will compute in-centrality:
var inCentrality = centrality.degree(g, 'in');
/* inCentrality is 
{
  "fortran": 0,
  "c": 1,
  "c++": 1,
  "perl": 1,
  "javascript": 1
}
*/

// out-centrality:
var outCentrality = centrality.degree(g, 'out');
/* outCentrality is
{
  "fortran": 1,
  "c": 2,
  "c++": 1,
  "perl": 0,
  "javascript": 0
}
*/

// You can also pass 'inout' or 'both' to get same results
// as `degreeCentrality`
var sameAsDegreeCentrality = centrality.degree(g, 'inout');
```

Performance of degree centrality calculation is:

* **inout**: `O(n)`, where `n` is number of nodes
* **in** or **out**: `O(n * a)`, where `a` is the average number of edges per
node


## [Betweenness centrality](https://en.wikipedia.org/wiki/Betweenness_centrality)

``` javascript
var centrality = require('ngraph.centrality');
var g = require('ngraph.graph')();
// Let's use the same graph as before:


// this will consider graph as undirected:
var betweenness = centrality.betweenness(g);
/* betweenness centrality is:

{
  "fortran": 0,
  "c": 5,
  "c++": 3,
  "perl": 0,
  "javascript": 0
}
*/

// this will consider graph as directed:
var directedBetweenness = centrality.betweenness(g, true);
/* directedBetweenness is:
{
  "fortran": 0,
  "c": 3,
  "c++": 2,
  "perl": 0,
  "javascript": 0
}
*/
```

Performance of betweenness calculation is `O(n * e)` time, and `O(n + e)` space
where `n` is number of nodes and `e` is number of edges.

This library implements Brandes's algorithm published in [A Faster Algorithm for Betweenness Centrality](http://www.inf.uni-konstanz.de/algo/publications/b-fabc-01.pdf)
and further discussed in [On Variants of Shortest-Path Betweenness
Centrality and their Generic Computation](http://www.inf.uni-konstanz.de/algo/publications/b-vspbc-08.pdf).

# install

With [npm](https://npmjs.org) do:

```
npm install ngraph.centrality
```

# license

MIT

# todo

It would be nice to have asynchronous version for each centrality calculator.
