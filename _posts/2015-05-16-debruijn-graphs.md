---
layout: post
title: "de Bruijn graphs and genomic assembly"
excerpt: "Post about gibbs sampling method."
tags: [machine learning, data science, mathematics, statistics]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---

Current genome sequencing technology (especially NGS) can sequence only short fragments of DNA. Generally, many copies of a region are sequenced to reduce the sequencing errors. The date from such sequencers, which is in form of short DNA sequence, needs to be assembled to obtain the complete genome. One of the modern methods to assemble the reads data is to use de Bruijn graphs.

**Definition:** Given a sequence of characters, it’s <strong>k−dimensional de Bruijn graph</strong> is defined as follows :
- The nodes are all the subsequences of length k, called <em>k-mers</em>.
- An edge between two nodes is the subsequence of length k + 1 which has the first node as prefix and the second as suffix.
<br><br>
e.g. For S = AGATAC, the 3-de Bruijn of S will have

- nodes - AGA,GAT,ATA,TAC
- edges - AGAT,GATA,ATAC

<strong>Observation:</strong> There exist $latex n^k$ k-mers in an alphabet containing n characters.

This implies that for English alphabet, we have 17576 3-mers. For DNA sequences the alphabet consists of 4 nucleotides A,G,T,C and thus there are 64 possible 3-mers.
A k-mer can appear more than one times in a de Bruijn graph with edges to different nodes.

A <strong>Hamiltonian path</strong> in a graph is a path that travels to every nodes exactly once.In the above example, a Hamiltonian path is given by
AGA -&gt; GAT -&gt; ATA -&gt; TAC.

It is easy to convince yourself that a Hamiltonian path in a de-Brujn graph constructed from the reads data will be a candidate for genome assembly because it visits each detected k-mer. Moreover, it will be the minimum length assembly as it travels to each node exactly one.
Unfortunately, there is no efficient algorithm to compute the Hamiltonian path in a large graph. In fact,

<strong>Theorem:</strong> Finding a Hamiltonian path in a graph is NP-Complete.

An <strong>Eulerian path</strong> in a graph is a path that visits each edge exactly once. In the above example, an Eulerian path is given by
AGAT -&gt; GATA -&gt; ATAC

An Eulerian path in a de Bruijn graph constructed on the sequence reads will also give a reasonable genome assembly as it visits each detected k + 1-mer. On the question of existence of an Eulerian path, we have following theorem of Euler -

<strong>Theorem:</strong> A directed connected graph (one there exists a path between any two nodes) has an Eulerian cycle if and only if the indegree (number of edges leading into the node) of each node is equal to its outdegree (number of edges leaving the node).

<strong>Lemma:</strong> de Bruijn graph has an Eulerian cycle.
<strong>proof:</strong> For any node, both the indegree and the outdegree are equal to the number of times the k-mer assigned node occurs in the sequence.

Having established that the de Bruijn graph has an Eulerian cycle, the next question is whether we can compute it efficiently. There are algorithms to compute the Eulerian cycle of a graph which are linear in number of edges.

<strong>Algorithm to assmeble genome:</strong>
￼• Choose the size of k-mers.
• Generate all k-mers from the reads data.
• Build a de Bruijn graph with k-mers as nodes and connected $latex k + 1$-mers as edges.
• Compute the Eulerian path of the de Bruijn graph. This will be a candidate assmebly.

Below is an example of genome assembly using de Bruijn graphs

-

<img class="alignnone  wp-image-340" src="https://januverma.files.wordpress.com/2014/11/nbt-2023-f3.gif?w=300" alt="nbt.2023-F3" width="547" height="372" />

Most of the times, sequencing doesn’t give us all the data. We have a lot of missing data which represent gaps in the genome and in that case, it would be impossible to obtain a whole genome assembly. So we build many de Bruijn graphs and obtain Eulerian cycles. Such cycles represent contiguous sequences of nucleotides, called <strong>contigs</strong>. Sometimes we join contigs separated by gaps to obtain <strong>scaffolds</strong>.