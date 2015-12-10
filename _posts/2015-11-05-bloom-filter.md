---
layout: post
title: "Bloom Filter"
excerpt: "Quick introduction to bloom filter data structure."
tags: [mathematics, computer science, data structures]
comments: true
image:
  feature: sample-image-4.jpg
  credit: WeGraphics
  creditlink: http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/
---
A <strong>Bloom filter</strong> is a compact data structure which is used to test membership of an element in a set. It is built by constructing a probablistic representation of the set, which can be queried for membership. A BF is designed for speed and effciency. The trade off for this efficiency is that a Bloom filter is probabilistic, in the sense that it tells us that the element is either definitely not in the set or may be in the set. Thus we can have some false positives i.e. the elements which are not in the set but the BF computes postive probability of it being in the set.
<br><br>
A bloom filter supports two operations

- <strong>Add</strong>: adds an element \\( x \\) to the set \\( S \\).
- <strong>Test </strong>: checks whether a given element \\( x \\) is in the set \\( S \\) or not. Test returns a boolean value as follows 

$$ Test = \begin{cases}
		false, &amp; \text{then x is definitely not in S} \\
		true, &amp; \text{then x is probably in the set }
	\end{cases}
$$
<br><br>
A BF should also compute the <em>false positive rate</em>.

<br><br>
<strong>Construction of a Bloom filter:</strong>
<br>
Consider a set \\( S = {a_1, a_2, \ldots, a_n} \\). Bloom filters describe membership information of \\( S \\) using a bit array \\( V \\) of length \\( m \\). It consists of an array of \\( m \\) bits, each initialized to \\( 0 \\). To add an item \\( \theta \\) to the bloom filter, \\( k \\) independent hash functions \\( H\_{1}(\theta), H\_{2}(\theta), \ldots, H\_{k}(\theta) \\) are calculated. Each maps \\( \theta \\) to an integer in \\( [0, m) \\) and the corresponding \\( h \\) array bits are set to 1.
<br><br>
To test if an item \\( \alpha \\) is a member, the same hash functions are applied to 
\\( \alpha \\) and the corresponding values in the bitarray are checked. \\( \alpha \\) is a member if all corresponding bits are set to 1.
<br><br>
By construction, a bloom filter correctly identifies whether a element is added to to it. But there is a false positive rate as well. A false positive occurs if all the bits corresponding to an element \\( \gamma \\) are set to 1 by coincidence because of the items besides \\( \gamma \\) that were added previously.

<br><br>
<strong>Example: </strong>
<br>
I borrowed the following example from <a href="http://kellabyte.com/2013/01/24/using-a-bloom-filter-to-reduce-expensive-operations-like-disk-io/">kellabyte</a>

A bloom starts by initializing an empty bitarray : 
<figure>
<a href="https://januverma.files.wordpress.com/2015/06/bloomfilter_empty_thumb1.jpg"><img src="https://januverma.files.wordpress.com/2015/06/bloomfilter_empty_thumb1.jpg?w=300" alt="bloomfilter_empty_thumb1" width="300" height="42" class="alignnone size-medium wp-image-883" /></a>
<fugure>

When a new element is added, the hashes are computed and the corresponding bits are turned on :
<figure>  
<a href="https://januverma.files.wordpress.com/2015/06/bloomfilter_adding_thumb1.jpg"><img src="https://januverma.files.wordpress.com/2015/06/bloomfilter_adding_thumb1.jpg?w=300" alt="bloomfilter_adding_thumb1" width="300" height="129" class="alignnone size-medium wp-image-882" /></a>
<figure>

When we query for an element, the same hashes are evaluated : 
<figure> 
<a href="https://januverma.files.wordpress.com/2015/06/bloomfilter_querying_thumb1.jpg"><img src="https://januverma.files.wordpress.com/2015/06/bloomfilter_querying_thumb1.jpg?w=300" alt="bloomfilter_querying_thumb1" width="300" height="196" class="alignnone size-medium wp-image-881" /></a>
<figure>

<br><br>
<strong>Optimizing a bloom filter:</strong>
<br>
There is a tradeoff between the size of the bitarray and the false positive rate. 
Note that after inserting \\( n \\) entries into a bloom filter of size \\( m \\) with \\( k \\) hash functions, the probability that a particular bit is still 0 is given by -
<br>
$$ P\_0 = (1 - \frac{1}{m})^{kn} \simeq 1 - e^{-\frac{kn}{m}} $$
<br>
Hence the probability of a false positive i.e. the probability that all k-bits have been previously set is - 
<br>
$$ P\_{err} = (1 - P\_0)^{k} \simeq (1 - e^{-\frac{kn}{m}})^{k} $$
<br>
Given \\( n \\) and \\( m \\), the optimal number of hash functions that minimizes the false positive rate is -
<br>
$$ k\_{optimal} \simeq \frac{m}{n} \ln 2 $$
<br>
In practice, only a small number of hash functions are used to reduce the computational overhead each additional hash function. 
<br><br>
Usually we have a fair idea of how many elements will be added to the bloom filter, in such a case, we have choose the size of the bitarray keeping the number of hash functions reasonable. 
<br><br>
e.g choosing \\( m = 8n \\) i.e. 1 byte per entry, and \\( k=5 \\), gives a false positive rate of \\( 2.16\% \\)
<br><br>
<strong>Implementation:</strong>
<br>
Let's try to implement a bloom filter in python following the above construction procedure. We will use python libraries <em>bitarray</em> and <em>hashlib</em>.
{% highlight python%}
import hashlib
from bitarray import bitarray
{% endhighlight %}

We will write a class called <em>BloomFilter</em>. This takes two arguments - size of the bitarray $latex m$ and number of hashes \\( k \\). 
{% highlight python %}
class BloomFilter():
	"""
	Implements a bloom filter.
	"""
	def __init__(self, m ,k):
		self.m = m
		self.k = k
		self.bfArray = bitarray(m)
		self.length = 0
{% endhighlight %}
<br>
As we discussed above a bloom filter computes \\( k \\) hashes for each element. Let's create these hash functions using python library <em>hashlib</em> which comes with base package. 

{% highlight python %}
def hash_functions(self, key):
	"""
	Build k hash functions.
	Map key to an integer in [0,m).
	"""
	h = hashlib.new('md5')
	h.update(str(key))
	x = long(h.hexdigest(),16)
	for i in xrange(self.k):
		if (x < self.m):
			h.update('.')
			x = long(h.hexdigest(),16)
		x,y = divmod(x, self.m)
		yield y
{% endhighlight %}
<br>
Now we are ready to add elements to the bloom filter. 
{% highlight python %}
def add(self, element):
	"""
	Add an element.
	"""
	self.length += 1
	for i in self.hash_functions(element):
		self.bfArray[i] = 1
	return None
{% endhighlight %}
<br>
The following method will check for membership.
{% highlight python %}
def contains(self, element):
	"""
	Checks if an element is present in the bloom filter.
	"""
	return all(self.bfArray[i] for i in self.hash_functions(element))
{% endhighlight %}
<br>
This completes our implementation of a bloom filter. Surely one can add more methods e.g. to get the number of elements added into the bloom filter or to compute the false positive rate. I'll leave that to the reader. 

<br><br>
<strong>Additional resources: </strong>

- <a href="http://billmill.org/bloomfilter-tutorial/">Bloom filter examples</a>
- <a href="http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/">Why bloom filters work</a>
- <a href="http://gsd.di.uminho.pt/members/cbm/ps/dbloom.pdf">Scalable bloom filters</a>
- <a href="https://github.com/jaybaird/python-bloomfilter">python-bloomfilter</a>