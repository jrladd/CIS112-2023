% Network Analysis
% DA 101, Dr. Ladd
% Week 12

# What are Networks?

## Networks are made up of...

- Entities (entity = node/vertex/actor)
- Relationships (relationship = edge/link/tie)
- We'll use "nodes" and "edges"

![](network_img/node_edge.png){height=400px}

## Nodes and Edges have Attributes

## Node Attributes

- numerical (size)
- categorical (color)

![](network_img/node_attributes.png){height=400px}

## Edge Attributes

## Directed and Undirected Edges

![](network_img/directed_edge.png){height=400px}

## Weighted and Unweighted Edges

![](network_img/weighted_edge.png){height=400px}

## Edge Types

![](network_img/type_edge.png){height=400px}

# Multiple Edges "in a row" Make a Path

## Path & Diameter

![](network_img/path.png){height=400px}

(& Average Shortest Path Length)

# Some special kinds of nodes

## Isolates

![](network_img/isolates.png){height=400px}

## Hubs

![](network_img/hubs.png){height=400px}

## Bridges

![](network_img/bridges.png){height=400px}

# Measuring a node's "importance" with centrality

## Degree

![](network_img/degree.png){height=400px}

## Strength

![](network_img/strength.png){height=400px}

## Betweenness

![](network_img/betweenness.png){height=400px}

# Different kinds of entities or nodes

## Unipartite/unimodal

![](network_img/unipartite.png){height=400px}

## Bipartite/bimodal

![](network_img/bipartite1.png){height=400px}

## Bipartite (cont.)

![](network_img/bipartite2.png){height=400px}

## Multipartite/k-partite/multimodal

![](network_img/multipartite.png){height=400px}

# Groups of nodes within a network

## Connected components

![](network_img/components.png){height=400px}

## Cliques and clustering

![](network_img/cliques.png){height=400px}

## Clustering Coefficient

![Image from [Wikipedia](https://en.wikipedia.org/wiki/Clustering_coefficient)](network_img/clustering_coefficient.svg){height=400px}

## Communities and community detection

![](network_img/communities.png){height=400px}

# Density

## A Sparse Network

![](network_img/sparse.png){height=400px}

## A Dense Network

![](network_img/dense.png){height=400px}

# There are many ways to visualize a network

## Adjacency Matrix

![Tilman Piesk, via Wikipedia](network_img/adjacency_matrix.svg){height=400px}

## Adjacency List

![](network_img/adjacency_list.png){height=300px}

- A adjacent to B,C
- B adjacent to A,C
- C adjacent to A,B

## Node-Link Diagram

![](network_img/node_edge.png){height=400px}

# Other Important Concepts

## Triadic Closure

![](network_img/triadic_closure.png){height=400px}

## Assortative mixing/Homophily

![](network_img/homophily.png){height=400px}

## Preferential Attachment

![](network_img/preferential_attachment.png){height=400px}

## Weak Ties

![](network_img/weak_ties.png){height=400px}

## Small World Network

<div style="width:50%;float:left;">
![](network_img/bacon.jpg){height=400px}
</div>
<div style="width:50%;float:left;">
- low average path length
- low clustering coefficients
- degree distribution follows power law (a few large hubs)
- low diameter (usually around "six degrees")
</div>

# Working with Networks in R

## Let's start with an example.

![](network_img/gameofthrones.jpg)

## We'll need two new libraries.

```r
library(igraph) # Network tools and metrics

library(networkD3) # Interactive network visualizations
```

## We can download the data and load it into a "network object" with iGraph.

First, download [got-edges.csv](../data/got-edges.csv). What does this data look like?

```r
# Read in edgelist CSV
edges <- read_csv("got-edges.csv")

# Create igraph object from edgelist
G <- graph_from_data_frame(d = edges, directed = FALSE)
E(G)$weight <- E(G)$Weight #Make sure weight is labeled correctly
```

## Let's make two versions of a node-link diagram.

```r
# The default R version
plot(G)

# A better D3 version
simpleNetwork(edges, opacity=1, zoom=TRUE)
```

## We can use iGraph to calculate different metrics.

```r
# Calculate degree and add to network data
V(G)$degree <- degree(G)

# Calculate betweenness centrality and add to network data
V(G)$betweenness <- betweenness(G, normalized=TRUE)

# Calculate the ratio of betweenness to degree
# (i.e. This is the node that functions most as a "bridge")
V(G)$betweenness_over_degree <- betweenness(G)/degree(G)
```

## You can convert node/vertex data to a dataframe.

```r
nodes <- igraph::as_data_frame(G, what="vertices")
```

What happens if you try:

```r
edges <- igraph::as_data_frame(G, what="edges")
```

## With a dataframe, you can easily see degree and betweenness distributions.

```r
# Degree distribution
ggplot(nodes, aes(degree)) +
     geom_histogram()
```

How would you make a graph of the betweenness distribution?

## Finally, let's make a better node-link diagram.

```r
# Convert data to networkD3-friendly format & calculate modularity clusters
G_d3 <- igraph_to_networkD3(G, membership(cluster_fast_greedy(G)))

# Add degree to node data and weight to link data
G_d3$nodes$degree <- V(G)$degree
G_d3$links$weight <- E(G)$weight

# Create force network with nodes sized by degree and colored by modularity
forceNetwork(Links=G_d3$links, Nodes = G_d3$nodes,
             Source = 'source', Target = 'target',
             Value= 'weight', NodeID = 'name',
             Group = 'group', Nodesize='degree',
             zoom = TRUE, opacity = 1)
```
