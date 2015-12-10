---
layout: post
title: "Community Detection in networks"
excerpt: "Discussion on the Girvan-Newman method for extracting communities in a network graph."
tags: [mathematics, graph theory, theoretical physics, mathematical physics]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---
An important question about a social, biological or economical network is to identify *communities*. A **community** is a subset of nodes which are 'more' connected to each other than the members of the other subsets. There are various methods to do this, we will use <a href="http://en.wikipedia.org/wiki/Girvan%E2%80%93Newman_algorithm">**Girvan-Newman algorithm**. </a>
<br><br>
Consider the following graph G (forgive poor visuals!)
<figure>
<a href="https://januverma.files.wordpress.com/2014/08/graph.png"><img class="alignnone  wp-image-259" src="http://januverma.files.wordpress.com/2014/08/graph.png?w=300" alt="graph" width="448" height="340" /></a>
</figure>
G has 7 nodes labeled A,B,C,D,E,F,G and there are edges between some of them. We want to extract communities in the graph.
<br><br>
GN algorithm works by finding the edges which are 'between' communities and then progressively removing these edges from the original graph.
<br>
How do we decide which edges are most likely to be between communities ?
<br>
Girvan and Newman defined the <strong>edge betweenness</strong> of an edge \\( (u,v) \\) as the number of shortest paths between pairs of vertices that run along it i.e number of nodes \\( x \\) and \\( y \\) such that the edge \\( (u,v) \\) lies on the shortest path between \\( x \\) and \\( y \\).
<br>
This extends the notion of node betweenness to edges.
<br><br>
If there are more than one shortest path between \\( x \\) and \\( y \\), then the contribution of \\( (x,y) \\) to the betweenness of \\( (u,v) \\) is equal to the fraction of the shortest paths that include \\( (u,v) \\).
<br><br>
If a network contains communities that are only loosely connected by a few inter-community edges, then all shortest paths between different communities must go along one of these few edges. Thus, the edges connecting communities will have high edge betweenness. By removing these edges, we separate groups from one another and so reveal the underlying community structure of the graph.
<br><br>
The algorithm's steps for community detection are -

1. The betweenness of all existing edges in the network is calculated first.
2. The edge with the highest betweenness is removed.
3. The betweenness of all edges affected by the removal is recalculated.
4. Steps 2 and 3 are repeated until some stopping criterion is fulfilled.

<br><br>
Let's apply GN algorithm on our graph. For our graph \(( G \\), we can calculate the betweenness for all the edges -

- AB: 5
- AC: 1
- BC: 5
- BD: 12
- DE: 4.5
- DG: 4.5
- EF: 1.5
- DF: 4
- GF: 1.5

<br><br>
As we can see in the graph that there are two shortest path between E and G, one going through D and the other through F. Thus each of the edges DE, EF, DG, GF are credited with half a shortest path.
<br>
Clearly the edge BD has the highest betweenness. So removing leaves us with two communities.

<a href="https://januverma.files.wordpress.com/2014/08/graph1.png"><img class="alignnone  wp-image-267" src="http://januverma.files.wordpress.com/2014/08/graph1.png?w=300" alt="graph1" width="484" height="368" /></a>
<br><br>
If we interpret the graph as a friendship network in a class, then A,B,C and D,E,F,G are two groups of friends. BD is the only interlocutor!
<br>
We can go further and re-calculate the betweenness for all the edges in the graph and remove edges accordingly.
<br><br>
An implementation of GN algorithm in python can be found <a href="https://github.com/Jverma/TextGraphics/blob/master/TextGraphics/Analysis/communityDetection.py">here</a>.