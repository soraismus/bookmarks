[Skip to content](#content)

-   [![RSS](http://www.michaelnielsen.org/ddi/wp-content/themes/titan_pro/images/flw-rss.png)](http://www.michaelnielsen.org/ddi/feed/)

[DDI](http://www.michaelnielsen.org/ddi/)

Data-driven intelligence

-   [About](http://michaelnielsen.org/blog/michael-a-nielsen)
-   [Michael’s main blog](http://michaelnielsen.org/blog/)
-   [Data-driven intelligence blog](http://michaelnielsen.org/ddi/)
-   [Writing](http://michaelnielsen.org/blog/writing/)

Why Bloom filters work the way they do
======================================

by Michael Nielsen on September 26, 2012

Imagine you’re a programmer who is developing a new web browser. There
are many malicious sites on the web, and you want your browser to warn
users when they attempt to access dangerous sites. For example, suppose
the user attempts to access `http://domain/etc`. You’d like a way of
checking whether `domain` is known to be a malicious site. What’s a good
way of doing this?

An obvious naive way is for your browser to maintain a list or set data
structure containing all known malicious domains. A problem with this
approach is that it may consume a considerable amount of memory. If you
know of a million malicious domains, and domains need (say) an average
of 20 bytes to store, then you need 20 megabytes of storage. That’s
quite an overhead for a single feature in your web browser. Is there a
better way?

In this post I’ll describe a data structure which provides an excellent
way of solving this kind of problem. The data structure is known as a
[Bloom filter](http://en.wikipedia.org/wiki/Bloom_filter). Bloom filter
are much more memory efficient than the naive “store-everything”
approach, while remaining extremely fast. I’ll describe both how Bloom
filters work, and also some extensions of Bloom filters to solve more
general problems.

Most explanations of Bloom filters cut to the chase, quickly explaining
the detailed mechanics of how Bloom filters work. Such explanations are
informative, but I must admit that they made me uncomfortable when I was
first learning about Bloom filters. In particular, I didn’t feel that
they helped me understand *why* Bloom filters are put together the way
they are. I couldn’t fathom the mindset that would lead someone to
*invent* such a data structure. And that left me feeling that all I had
was a superficial, surface-level understanding of Bloom filters.

In this post I take an unusual approach to explaining Bloom filters. We
*won’t* begin with a full-blown explanation. Instead, I’ll gradually
build up to the full data structure in stages. My goal is to tell a
plausible story explaining how one could invent Bloom filters from
scratch, with each step along the way more or less “obvious”. Of course,
hindsight is 20-20, and such a story shouldn’t be taken too literally.
Rather, the benefit of developing Bloom filters in this way is that it
will deepen our understanding of why Bloom filters work in just the way
they do. We’ll explore some alternative directions that plausibly
*could* have been taken – and see why they don’t work as well as Bloom
filters ultimately turn out to work. At the end we’ll understand much
better why Bloom filters are constructed the way they are.

Of course, this means that if your goal is just to understand the
mechanics of Bloom filters, then this post isn’t for you. Instead, I’d
suggest looking at a more conventional introduction – the [Wikipedia
article](http://en.wikipedia.org/wiki/Bloom_filter), for example,
perhaps in conjunction with an interactive demo, like the nice one
[here](http://www.jasondavies.com/bloomfilter/). But if your goal is to
understand why Bloom filters work the way they do, then you may enjoy
the post.

**A stylistic note:** Most of my posts are code-oriented. This post is
much more focused on mathematical analysis and algebraic manipulation:
the point isn’t code, but rather how one could come to invent a
particular data structure. That is, it’s the story *behind* the code
that implements Bloom filters, and as such it requires rather more
attention to mathematical detail.

**General description of the problem:** Let’s begin by abstracting away
from the “safe web browsing” problem that began this post. We want a
data structure which represents a set
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
of objects. That data structure should enable two operations: (1) the
ability to `add` an extra object
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
to the set; and (2) a `test` to determine whether a given object
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is a member of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
Of course, there are many other operations we might imagine wanting –
for example, maybe we’d also like to be able to `delete` objects from
the set. But we’re going to start with just these two operations of
`add`ing and `test`ing. Later we’ll come back and ask whether operations
such as `delete`ing objects are also possible.

**Idea: store a set of hashed objects:** Okay, so how can we solve the
problem of representing
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
in a way that’s more memory efficient than just storing all the objects
in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")?
One idea is to store hashed versions of the objects in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S"),
instead of the full objects. If the hash function is well chosen, then
the hashed objects will take up much less memory, but there will be
little danger of making errors when `test`ing whether an object is an
element of the set or not.

Let’s be a little more explicit about how this would work. We have a set
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
of objects ![x\_0, x\_1, \\ldots,
x\_{|S|-1}](http://s.wordpress.com/latex.php?latex=x_0%2C%20x_1%2C%20%5Cldots%2C%20x_%7B%7CS%7C-1%7D&bg=ffffff&fg=000000&s=0 "x_0, x_1, \ldots, x_{|S|-1}"),
where
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
denotes the number of objects in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
For each object we compute an
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")-bit
hash function
![h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x_j)")
– i.e., a hash function which takes an arbitrary object as input, and
outputs
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")
bits – and the set
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
is represented by the set ![\\{ h(x\_0), h(x\_1), \\ldots, h(x\_{|S|-1})
\\}](http://s.wordpress.com/latex.php?latex=%5C%7B%20h%28x_0%29%2C%20h%28x_1%29%2C%20%5Cldots%2C%20h%28x_%7B%7CS%7C-1%7D%29%20%5C%7D&bg=ffffff&fg=000000&s=0 "\{ h(x_0), h(x_1), \ldots, h(x_{|S|-1}) \}").
We can `test` whether
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is an element of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
by checking whether
![h(x)](http://s.wordpress.com/latex.php?latex=h%28x%29&bg=ffffff&fg=000000&s=0 "h(x)")
is in the set of hashes. This basic hashing approach requires roughly
![m
|S|](http://s.wordpress.com/latex.php?latex=m%20%7CS%7C&bg=ffffff&fg=000000&s=0 "m |S|")
bits of memory.

(As an aside, in principle it’s possible to store the set of hashed
objects more efficiently, using just
![m|S|-\\log(|S|!)](http://s.wordpress.com/latex.php?latex=m%7CS%7C-%5Clog%28%7CS%7C%21%29&bg=ffffff&fg=000000&s=0 "m|S|-\log(|S|!)")
bits, where
![\\log](http://s.wordpress.com/latex.php?latex=%5Clog&bg=ffffff&fg=000000&s=0 "\log")
is to base two. The
![-\\log(|S|!)](http://s.wordpress.com/latex.php?latex=-%5Clog%28%7CS%7C%21%29&bg=ffffff&fg=000000&s=0 "-\log(|S|!)")
saving is possible because the ordering of the objects in a set is
redundant information, and so in principle can be eliminated using a
suitable encoding. However, I haven’t thought through what encodings
could be used to do this in practice. In any case, the saving is likely
to be minimal, since ![\\log(|S|!) \\approx |S| \\log
|S|,](http://s.wordpress.com/latex.php?latex=%5Clog%28%7CS%7C%21%29%20%20%5Capprox%20%7CS%7C%20%5Clog%20%7CS%7C%2C&bg=ffffff&fg=000000&s=0 "\log(|S|!)  \approx |S| \log |S|,")
and
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")
will usually be quite a bit bigger than ![\\log
|S|](http://s.wordpress.com/latex.php?latex=%5Clog%20%7CS%7C&bg=ffffff&fg=000000&s=0 "\log |S|")
– if that weren’t the case, then hash collisions would occur all the
time. So I’ll ignore the terms
![-\\log(|S|!)](http://s.wordpress.com/latex.php?latex=-%5Clog%28%7CS%7C%21%29&bg=ffffff&fg=000000&s=0 "-\log(|S|!)")
for the rest of this post. In fact, in general I’ll be pretty cavalier
in later analyses as well, omitting lower order terms without comment.)

A danger with this hash-based approach is that an object
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
outside the set
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
might have the same hash value as an object inside the set, i.e., ![h(x)
=
h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x%29%20%3D%20h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x) = h(x_j)")
for some
![j](http://s.wordpress.com/latex.php?latex=j&bg=ffffff&fg=000000&s=0 "j").
In this case, `test` will erroneously report that
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
That is, this data structure will give us a *false positive*.
Fortunately, by choosing a suitable value for
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m"),
the number of bits output by the hash function, we can reduce the
probability of a false positive as much as we want. To understand how
this works, notice first that the probability of `test` giving a false
positive is 1 minus the probability of `test` correctly reporting that
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is not in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
This occurs when ![h(x) \\neq
h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x%29%20%5Cneq%20h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x) \neq h(x_j)")
for all
![j](http://s.wordpress.com/latex.php?latex=j&bg=ffffff&fg=000000&s=0 "j").
If the hash function is well chosen, then the probability that ![h(x)
\\neq
h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x%29%20%5Cneq%20h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x) \neq h(x_j)")
is
![(1-1/2\^m)](http://s.wordpress.com/latex.php?latex=%281-1%2F2%5Em%29&bg=ffffff&fg=000000&s=0 "(1-1/2^m)")
for each
![j](http://s.wordpress.com/latex.php?latex=j&bg=ffffff&fg=000000&s=0 "j"),
and these are independent events. Thus the probability of `test` failing
is:

![ p = 1-(1-1/2\^m)\^{|S|}.
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%201-%281-1%2F2%5Em%29%5E%7B%7CS%7C%7D.%20&bg=ffffff&fg=000000&s=0 "   p = 1-(1-1/2^m)^{|S|}. ")

This expression involves three quantities: the probability
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
of `test` giving a false positive, the number
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")
of bits output by the hash function, and the number of elements in the
set,
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|").
It’s a nice expression, but it’s more enlightening when rewritten in a
slightly different form. What we’d really like to understand is how many
bits of memory are needed to represent a set of size
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|"),
with probability
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
of a `test` failing. To understand that we let
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#")
be the number of bits of memory used, and aim to express
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#")
as a function of
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
and
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|").
Observe that ![\\\# =
|S|m](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%20%7CS%7Cm&bg=ffffff&fg=000000&s=0 "\# = |S|m"),
and so we can substitute for ![m =
\\\#/|S|](http://s.wordpress.com/latex.php?latex=m%20%3D%20%5C%23%2F%7CS%7C&bg=ffffff&fg=000000&s=0 "m = \#/|S|")
to obtain

![ p = 1-\\left(1-1/2\^{\\\#/|S|}\\right)\^{|S|}.
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%201-%5Cleft%281-1%2F2%5E%7B%5C%23%2F%7CS%7C%7D%5Cright%29%5E%7B%7CS%7C%7D.%20&bg=ffffff&fg=000000&s=0 "   p = 1-\left(1-1/2^{\#/|S|}\right)^{|S|}. ")

This can be rearranged to express
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#")
in term of
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
and
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|"):

![ \\\# = |S|\\log \\frac{1}{1-(1-p)\^{1/|S|}}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%7CS%7C%5Clog%20%5Cfrac%7B1%7D%7B1-%281-p%29%5E%7B1%2F%7CS%7C%7D%7D.%20&bg=ffffff&fg=000000&s=0 "   \# = |S|\log \frac{1}{1-(1-p)^{1/|S|}}. ")

This expression answers the question we really want answered, telling us
how many bits are required to store a set of size
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
with a probability
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
of a `test` failing. Of course, in practice we’d like
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
to be small – say ![p =
0.01](http://s.wordpress.com/latex.php?latex=p%20%3D%200.01&bg=ffffff&fg=000000&s=0 "p = 0.01")
– and when this is the case the expression may be approximated by a more
transparent expression:

![ \\\# \\approx |S|\\log \\frac{|S|}{p}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%5Capprox%20%7CS%7C%5Clog%20%5Cfrac%7B%7CS%7C%7D%7Bp%7D.%20&bg=ffffff&fg=000000&s=0 "   \# \approx |S|\log \frac{|S|}{p}. ")

This makes intuitive sense: `test` failure occurs when
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is not in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S"),
but
![h(x)](http://s.wordpress.com/latex.php?latex=h%28x%29&bg=ffffff&fg=000000&s=0 "h(x)")
is in the hashed version of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
Because this happens with probability
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p"),
it must be that
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
occupies a fraction
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
of the total space of possible hash outputs. And so the size of the
space of all possible hash outputs must be about
![|S|/p](http://s.wordpress.com/latex.php?latex=%7CS%7C%2Fp&bg=ffffff&fg=000000&s=0 "|S|/p").
As a consequence we need
![\\log(|S|/p)](http://s.wordpress.com/latex.php?latex=%5Clog%28%7CS%7C%2Fp%29&bg=ffffff&fg=000000&s=0 "\log(|S|/p)")
bits to represent each hashed object, in agreement with the expression
above.

How memory efficient is this hash-based approach to representing
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")?
It’s obviously likely to be quite a bit better than storing full
representations of the objects in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
But we’ll see later that Bloom filters can be far more memory efficient
still.

The big drawback of this hash-based approach is the false positives.
Still, for many applications it’s fine to have a small probability of a
false positive. For example, false positives turn out to be okay for the
safe web browsing problem. You might worry that false positives would
cause some safe sites to erroneously be reported as unsafe, but the
browser can avoid this by maintaining a (small!) list of safe sites
which are false positives for `test`.

**Idea: use a bit array:** Suppose we want to represent some subset
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
of the integers ![0, 1, 2, \\ldots,
999](http://s.wordpress.com/latex.php?latex=0%2C%201%2C%202%2C%20%5Cldots%2C%20999&bg=ffffff&fg=000000&s=0 "0, 1, 2, \ldots, 999").
As an alternative to hashing or to storing
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
directly, we could represent
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
using an array of
![1000](http://s.wordpress.com/latex.php?latex=1000&bg=ffffff&fg=000000&s=0 "1000")
bits, numbered
![0](http://s.wordpress.com/latex.php?latex=0&bg=ffffff&fg=000000&s=0 "0")
through
![999](http://s.wordpress.com/latex.php?latex=999&bg=ffffff&fg=000000&s=0 "999").
We would set bits in the array to
![1](http://s.wordpress.com/latex.php?latex=1&bg=ffffff&fg=000000&s=0 "1")
if the corresponding number is in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S"),
and otherwise set them to
![0](http://s.wordpress.com/latex.php?latex=0&bg=ffffff&fg=000000&s=0 "0").
It’s obviously trivial to `add` objects to
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S"),
and to `test` whether a particular object is in
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
or not.

The memory cost to store
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
in this bit-array approach is
![1000](http://s.wordpress.com/latex.php?latex=1000&bg=ffffff&fg=000000&s=0 "1000")
bits, regardless of how big or small
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
is. Suppose, for comparison, that we stored
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
directly as a list of 32-bit integers. Then the cost would be ![32
|S|](http://s.wordpress.com/latex.php?latex=32%20%7CS%7C&bg=ffffff&fg=000000&s=0 "32 |S|")
bits. When
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
is very small, this approach would be more memory efficient than using a
bit array. But as
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
gets larger, storing
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
directly becomes much less memory efficient. We could ameliorate this
somewhat by storing elements of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
using only 10 bits, instead of 32 bits. But even if we did this, it
would still be more expensive to store the list once
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
got beyond one hundred elements. So a bit array really would be better
for modestly large subsets.

**Idea: use a bit array where the indices are given by hashes:** A
problem with the bit array example described above is that we needed a
way of numbering the possible elements of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S"),
![0,1,\\ldots,999](http://s.wordpress.com/latex.php?latex=0%2C1%2C%5Cldots%2C999&bg=ffffff&fg=000000&s=0 "0,1,\ldots,999").
In general the elements of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
may be complicated objects, not numbers in a small, well-defined range.

Fortunately, we can use hashing to number the elements of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S").
Suppose
![h(\\cdot)](http://s.wordpress.com/latex.php?latex=h%28%5Ccdot%29&bg=ffffff&fg=000000&s=0 "h(\cdot)")
is an
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")-bit
hash function. We’re going to represent a set ![S =
\\{x\_0,\\ldots,x\_{|S|-1}\\}](http://s.wordpress.com/latex.php?latex=S%20%3D%20%5C%7Bx_0%2C%5Cldots%2Cx_%7B%7CS%7C-1%7D%5C%7D&bg=ffffff&fg=000000&s=0 "S = \{x_0,\ldots,x_{|S|-1}\}")
using a bit array containing
![2\^m](http://s.wordpress.com/latex.php?latex=2%5Em&bg=ffffff&fg=000000&s=0 "2^m")
elements. In particular, for each
![x\_j](http://s.wordpress.com/latex.php?latex=x_j&bg=ffffff&fg=000000&s=0 "x_j")
we set the
![h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x_j)")th
element in the bit array, where we regard
![h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x_j)")
as a number in the range
![0,1,\\ldots,2\^m-1](http://s.wordpress.com/latex.php?latex=0%2C1%2C%5Cldots%2C2%5Em-1&bg=ffffff&fg=000000&s=0 "0,1,\ldots,2^m-1").
More explicitly, we can `add` an element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
to the set by setting bit number
![h(x)](http://s.wordpress.com/latex.php?latex=h%28x%29&bg=ffffff&fg=000000&s=0 "h(x)")
in the bit array. And we can `test` whether
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is an element of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
by checking whether bit number
![h(x)](http://s.wordpress.com/latex.php?latex=h%28x%29&bg=ffffff&fg=000000&s=0 "h(x)")
in the bit array is set.

This is a good scheme, but the `test` can fail to give the correct
result, which occurs whenever
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is not an element of
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S"),
yet ![h(x) =
h(x\_j)](http://s.wordpress.com/latex.php?latex=h%28x%29%20%3D%20h%28x_j%29&bg=ffffff&fg=000000&s=0 "h(x) = h(x_j)")
for some
![j](http://s.wordpress.com/latex.php?latex=j&bg=ffffff&fg=000000&s=0 "j").
This is exactly the same failure condition as for the basic hashing
scheme we described earlier. By exactly the same reasoning as used then,
the failure probability is

![ [\*] \\,\\,\\,\\, p = 1-(1-1/2\^m)\^{|S|}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5B%2A%5D%20%5C%2C%5C%2C%5C%2C%5C%2C%20p%20%3D%201-%281-1%2F2%5Em%29%5E%7B%7CS%7C%7D.%20&bg=ffffff&fg=000000&s=0 "   [*] \,\,\,\, p = 1-(1-1/2^m)^{|S|}. ")

As we did earlier, we’d like to re-express this in terms of the number
of bits of memory used,
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#").
This works differently than for the basic hashing scheme, since the
number of bits of memory consumed by the current approach is ![\\\# =
2\^m](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%202%5Em&bg=ffffff&fg=000000&s=0 "\# = 2^m"),
as compared to ![\\\# =
|S|m](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%20%7CS%7Cm&bg=ffffff&fg=000000&s=0 "\# = |S|m")
for the earlier scheme. Using ![\\\# =
2\^m](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%202%5Em&bg=ffffff&fg=000000&s=0 "\# = 2^m")
and substituting for
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")
in Equation [\*], we have:

![ p = 1-(1-1/\\\#)\^{|S|}.
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%201-%281-1%2F%5C%23%29%5E%7B%7CS%7C%7D.%20&bg=ffffff&fg=000000&s=0 "   p = 1-(1-1/\#)^{|S|}. ")

Rearranging this to express
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#")
in term of
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
and
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
we obtain:

![ \\\# = \\frac{1}{1-(1-p)\^{1/|S|}}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%5Cfrac%7B1%7D%7B1-%281-p%29%5E%7B1%2F%7CS%7C%7D%7D.%20&bg=ffffff&fg=000000&s=0 "   \# = \frac{1}{1-(1-p)^{1/|S|}}. ")

When
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
is small this can be approximated by

![ \\\# \\approx \\frac{|S|}{p}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%5Capprox%20%5Cfrac%7B%7CS%7C%7D%7Bp%7D.%20&bg=ffffff&fg=000000&s=0 "   \# \approx \frac{|S|}{p}. ")

This isn’t very memory efficient! We’d like the probability of failure
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
to be small, and that makes the
![1/p](http://s.wordpress.com/latex.php?latex=1%2Fp&bg=ffffff&fg=000000&s=0 "1/p")
dependence bad news when compared to the
![\\log(|S|/p)](http://s.wordpress.com/latex.php?latex=%5Clog%28%7CS%7C%2Fp%29&bg=ffffff&fg=000000&s=0 "\log(|S|/p)")
dependence of the basic hashing scheme described earlier. The only time
the current approach is better is when
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
is very, very large. To get some idea for just how large, if we want ![p
=
0.01](http://s.wordpress.com/latex.php?latex=p%20%3D%200.01&bg=ffffff&fg=000000&s=0 "p = 0.01"),
then
![1/p](http://s.wordpress.com/latex.php?latex=1%2Fp&bg=ffffff&fg=000000&s=0 "1/p")
is only better than
![\\log(|S|/p)](http://s.wordpress.com/latex.php?latex=%5Clog%28%7CS%7C%2Fp%29&bg=ffffff&fg=000000&s=0 "\log(|S|/p)")
when
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
gets to be more than about ![1.27 \*
10\^{28}](http://s.wordpress.com/latex.php?latex=1.27%20%2A%2010%5E%7B28%7D&bg=ffffff&fg=000000&s=0 "1.27 * 10^{28}").
That’s quite a set! In practice, the basic hashing scheme will be much
more memory efficient.

Intuitively, it’s not hard to see why this approach is so memory
inefficient compared to the basic hashing scheme. The problem is that
with an
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")-bit
hash function, the basic hashing scheme used
![m|S|](http://s.wordpress.com/latex.php?latex=m%7CS%7C&bg=ffffff&fg=000000&s=0 "m|S|")
bits of memory, while hashing into a bit array uses
![2\^m](http://s.wordpress.com/latex.php?latex=2%5Em&bg=ffffff&fg=000000&s=0 "2^m")
bits, but doesn’t change the probability of failure. That’s
exponentially more memory!

At this point, hashing into bit arrays looks like a bad idea. But it
turns out that by tweaking the idea just a little we can improve it a
lot. To carry out this tweaking, it helps to name the data structure
we’ve just described (where we hash into a bit array). We’ll call it a
*filter*, anticipating the fact that it’s a precursor to the Bloom
filter. I don’t know whether “filter” is a standard name, but in any
case it’ll be a useful working name.

**Idea: use multiple filters:** How can we make the basic filter just
described more memory efficient? One possibility is to try using
multiple filters, based on independent hash functions. More precisely,
the idea is to use
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
filters, each based on an (independent)
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")-bit
hash function, ![h\_0, h\_1, \\ldots,
h\_{k-1}](http://s.wordpress.com/latex.php?latex=h_0%2C%20h_1%2C%20%5Cldots%2C%20h_%7Bk-1%7D&bg=ffffff&fg=000000&s=0 "h_0, h_1, \ldots, h_{k-1}").
So our data structure will consist of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
separate bit arrays, each containing
![2\^m](http://s.wordpress.com/latex.php?latex=2%5Em&bg=ffffff&fg=000000&s=0 "2^m")
bits, for a grand total of ![\\\# = k
2\^m](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%20k%202%5Em&bg=ffffff&fg=000000&s=0 "\# = k 2^m")
bits. We can `add` an element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
by setting the
![h\_0(x)](http://s.wordpress.com/latex.php?latex=h_0%28x%29&bg=ffffff&fg=000000&s=0 "h_0(x)")th
bit in the first bit array (i.e., the first filter), the
![h\_1(x)](http://s.wordpress.com/latex.php?latex=h_1%28x%29&bg=ffffff&fg=000000&s=0 "h_1(x)")th
bit in the second filter, and so on. We can `test` whether a candidate
element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is in the set by simply checking whether all the appropriate bits are
set in each filter. For this to fail, each individual filter must fail.
Because the hash functions are independent of one another, the
probability of this is the
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")th
power of any single filter failing:

![ p = \\left(1-(1-1/2\^m)\^{|S|}\\right)\^k.
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%20%5Cleft%281-%281-1%2F2%5Em%29%5E%7B%7CS%7C%7D%5Cright%29%5Ek.%20&bg=ffffff&fg=000000&s=0 "   p = \left(1-(1-1/2^m)^{|S|}\right)^k. ")

The number of bits of memory used by this data structure is ![\\\# = k
2\^m](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%20k%202%5Em&bg=ffffff&fg=000000&s=0 "\# = k 2^m")
and so we can substitute ![2\^m =
\\\#/k](http://s.wordpress.com/latex.php?latex=2%5Em%20%3D%20%5C%23%2Fk&bg=ffffff&fg=000000&s=0 "2^m = \#/k")
and rearrange to get

![ [\*\*] \\,\\,\\,\\, \\\# = \\frac{k}{1-(1-p\^{1/k})\^{1/|S|}}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5B%2A%2A%5D%20%5C%2C%5C%2C%5C%2C%5C%2C%20%5C%23%20%3D%20%5Cfrac%7Bk%7D%7B1-%281-p%5E%7B1%2Fk%7D%29%5E%7B1%2F%7CS%7C%7D%7D.%20&bg=ffffff&fg=000000&s=0 "   [**] \,\,\,\, \# = \frac{k}{1-(1-p^{1/k})^{1/|S|}}. ")

Provided
![p\^{1/k}](http://s.wordpress.com/latex.php?latex=p%5E%7B1%2Fk%7D&bg=ffffff&fg=000000&s=0 "p^{1/k}")
is much smaller than
![1](http://s.wordpress.com/latex.php?latex=1&bg=ffffff&fg=000000&s=0 "1"),
this expression can be simplified to give

![ \\\# \\approx \\frac{k|S|}{p\^{1/k}}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%5Capprox%20%5Cfrac%7Bk%7CS%7C%7D%7Bp%5E%7B1%2Fk%7D%7D.%20&bg=ffffff&fg=000000&s=0 "   \# \approx \frac{k|S|}{p^{1/k}}. ")

Good news! This repetition strategy is much more memory efficient than a
single filter, at least for small values of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k").
For instance, moving from ![k =
1](http://s.wordpress.com/latex.php?latex=k%20%3D%201&bg=ffffff&fg=000000&s=0 "k = 1")
repetitions to ![k =
2](http://s.wordpress.com/latex.php?latex=k%20%3D%202&bg=ffffff&fg=000000&s=0 "k = 2")
repititions changes the denominator from
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
to
![\\sqrt{p}](http://s.wordpress.com/latex.php?latex=%5Csqrt%7Bp%7D&bg=ffffff&fg=000000&s=0 "\sqrt{p}")
– typically, a huge improvement, since
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
is very small. And the only price paid is doubling the numerator. So
this is a big win.

Intuitively, and in retrospect, this result is not so surprising.
Putting multiple filters in a row, the probability of error drops
exponentially with the number of filters. By contrast, in the single
filter scheme, the drop in the probability of error is roughly linear
with the number of bits. (This follows from considering Equation [\*] in
the limit where
![1/2\^m](http://s.wordpress.com/latex.php?latex=1%2F2%5Em&bg=ffffff&fg=000000&s=0 "1/2^m")
is small.) So using multiple filters is a good strategy.

Of course, a caveat to the last paragraph is that this analysis requires
that ![p\^{1/k} \\ll
1](http://s.wordpress.com/latex.php?latex=p%5E%7B1%2Fk%7D%20%5Cll%201&bg=ffffff&fg=000000&s=0 "p^{1/k} \ll 1"),
which means that
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
can’t be too large before the analysis breaks down. For larger values of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
the analysis is somewhat more complicated. In order to find the optimal
value of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
we’d need to figure out what value of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
minimizes the exact expression [\*\*] for
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#").
We won’t bother – at best it’d be tedious, and, as we’ll see shortly,
there is in any case a better approach.

**Overlapping filters:** This is a variation on the idea of repeating
filters. Instead of having
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
separate bit arrays, we use just a single array of
![2\^m](http://s.wordpress.com/latex.php?latex=2%5Em&bg=ffffff&fg=000000&s=0 "2^m")
bits. When `add`ing an object
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x"),
we simply set all the bits ![h\_0(x), h\_1(x),\\ldots,
h\_{k-1}(x)](http://s.wordpress.com/latex.php?latex=h_0%28x%29%2C%20h_1%28x%29%2C%5Cldots%2C%20h_%7Bk-1%7D%28x%29&bg=ffffff&fg=000000&s=0 "h_0(x), h_1(x),\ldots, h_{k-1}(x)")
in the same bit array. To `test` whether an element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is in the set, we simply check whether all the bits ![h\_0(x),
h\_1(x),\\ldots,
h\_{k-1}(x)](http://s.wordpress.com/latex.php?latex=h_0%28x%29%2C%20h_1%28x%29%2C%5Cldots%2C%20h_%7Bk-1%7D%28x%29&bg=ffffff&fg=000000&s=0 "h_0(x), h_1(x),\ldots, h_{k-1}(x)")
are set or not.

What’s the probability of the `test` failing? Suppose ![x \\not \\in
S](http://s.wordpress.com/latex.php?latex=x%20%5Cnot%20%5Cin%20S&bg=ffffff&fg=000000&s=0 "x \not \in S").
Failure occurs when ![h\_0(x) =
h\_{i\_0}(x\_{j\_0})](http://s.wordpress.com/latex.php?latex=h_0%28x%29%20%3D%20h_%7Bi_0%7D%28x_%7Bj_0%7D%29&bg=ffffff&fg=000000&s=0 "h_0(x) = h_{i_0}(x_{j_0})")
for some
![i\_0](http://s.wordpress.com/latex.php?latex=i_0&bg=ffffff&fg=000000&s=0 "i_0")
and
![j\_0](http://s.wordpress.com/latex.php?latex=j_0&bg=ffffff&fg=000000&s=0 "j_0"),
and also ![h\_1(x) =
h\_{i\_1}(x\_{j\_1})](http://s.wordpress.com/latex.php?latex=h_1%28x%29%20%3D%20h_%7Bi_1%7D%28x_%7Bj_1%7D%29&bg=ffffff&fg=000000&s=0 "h_1(x) = h_{i_1}(x_{j_1})")
for some
![i\_1](http://s.wordpress.com/latex.php?latex=i_1&bg=ffffff&fg=000000&s=0 "i_1")
and
![j\_1](http://s.wordpress.com/latex.php?latex=j_1&bg=ffffff&fg=000000&s=0 "j_1"),
and so on for all the remaining hash functions, ![h\_2, h\_3,\\ldots,
h\_{k-1}](http://s.wordpress.com/latex.php?latex=h_2%2C%20h_3%2C%5Cldots%2C%20h_%7Bk-1%7D&bg=ffffff&fg=000000&s=0 "h_2, h_3,\ldots, h_{k-1}").
These are independent events, and so the probability they all occur is
just the product of the probabilities of the individual events. A little
thought should convince you that each individual event will have the
same probability, and so we can just focus on computing the probability
that ![h\_0(x) =
h\_{i\_0}(x\_{j\_0})](http://s.wordpress.com/latex.php?latex=h_0%28x%29%20%3D%20h_%7Bi_0%7D%28x_%7Bj_0%7D%29&bg=ffffff&fg=000000&s=0 "h_0(x) = h_{i_0}(x_{j_0})")
for some
![i\_0](http://s.wordpress.com/latex.php?latex=i_0&bg=ffffff&fg=000000&s=0 "i_0")
and
![j\_0](http://s.wordpress.com/latex.php?latex=j_0&bg=ffffff&fg=000000&s=0 "j_0").
The overall probability
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p")
of failure will then be the
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")th
power of that probability, i.e.,

![ p = p(h\_0(x) = h\_{i\_0}(x\_{j\_0}) \\mbox{ for some } i\_0,j\_0)\^k
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%20p%28h_0%28x%29%20%3D%20h_%7Bi_0%7D%28x_%7Bj_0%7D%29%20%5Cmbox%7B%20for%20some%20%7D%20i_0%2Cj_0%29%5Ek%20&bg=ffffff&fg=000000&s=0 "   p = p(h_0(x) = h_{i_0}(x_{j_0}) \mbox{ for some } i_0,j_0)^k ")

The probability that ![h\_0(x) =
h\_{i\_0}(x\_{j\_0})](http://s.wordpress.com/latex.php?latex=h_0%28x%29%20%3D%20h_%7Bi_0%7D%28x_%7Bj_0%7D%29&bg=ffffff&fg=000000&s=0 "h_0(x) = h_{i_0}(x_{j_0})")
for some
![i\_0](http://s.wordpress.com/latex.php?latex=i_0&bg=ffffff&fg=000000&s=0 "i_0")
and
![j\_0](http://s.wordpress.com/latex.php?latex=j_0&bg=ffffff&fg=000000&s=0 "j_0")
is one minus the probability that ![h\_0(x) \\neq
h\_{i\_0}(x\_{j\_0})](http://s.wordpress.com/latex.php?latex=h_0%28x%29%20%5Cneq%20h_%7Bi_0%7D%28x_%7Bj_0%7D%29&bg=ffffff&fg=000000&s=0 "h_0(x) \neq h_{i_0}(x_{j_0})")
for all
![i\_0](http://s.wordpress.com/latex.php?latex=i_0&bg=ffffff&fg=000000&s=0 "i_0")
and
![j\_0](http://s.wordpress.com/latex.php?latex=j_0&bg=ffffff&fg=000000&s=0 "j_0").
These are independent events for the different possible values of
![i\_0](http://s.wordpress.com/latex.php?latex=i_0&bg=ffffff&fg=000000&s=0 "i_0")
and
![j\_0](http://s.wordpress.com/latex.php?latex=j_0&bg=ffffff&fg=000000&s=0 "j_0"),
each with probability
![1-1/2\^m](http://s.wordpress.com/latex.php?latex=1-1%2F2%5Em&bg=ffffff&fg=000000&s=0 "1-1/2^m"),
and so

![ p(h\_0(x) = h\_{i\_0}(x\_{j\_0}) \\mbox{ for some } i\_0,j\_0) =
1-(1-1/2\^m )\^{k|S|},
](http://s.wordpress.com/latex.php?latex=%20%20%20p%28h_0%28x%29%20%3D%20h_%7Bi_0%7D%28x_%7Bj_0%7D%29%20%5Cmbox%7B%20for%20some%20%7D%20i_0%2Cj_0%29%20%3D%201-%281-1%2F2%5Em%20%29%5E%7Bk%7CS%7C%7D%2C%20%20%20&bg=ffffff&fg=000000&s=0 "   p(h_0(x) = h_{i_0}(x_{j_0}) \mbox{ for some } i_0,j_0) = 1-(1-1/2^m )^{k|S|},   ")

since there are
![k|S|](http://s.wordpress.com/latex.php?latex=k%7CS%7C&bg=ffffff&fg=000000&s=0 "k|S|")
different pairs of possible values ![(i\_0,
j\_0)](http://s.wordpress.com/latex.php?latex=%28i_0%2C%20j_0%29&bg=ffffff&fg=000000&s=0 "(i_0, j_0)").
It follows that

![ p = \\left(1-(1-1/2\^m )\^{k|S|}\\right)\^k.
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%20%5Cleft%281-%281-1%2F2%5Em%20%29%5E%7Bk%7CS%7C%7D%5Cright%29%5Ek.%20&bg=ffffff&fg=000000&s=0 "   p = \left(1-(1-1/2^m )^{k|S|}\right)^k. ")

Substituting ![2\^m =
\\\#](http://s.wordpress.com/latex.php?latex=2%5Em%20%3D%20%5C%23&bg=ffffff&fg=000000&s=0 "2^m = \#")
we obtain

![ p = \\left(1-(1-1/\\\# )\^{k|S|}\\right)\^k
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%20%5Cleft%281-%281-1%2F%5C%23%20%29%5E%7Bk%7CS%7C%7D%5Cright%29%5Ek%20&bg=ffffff&fg=000000&s=0 "   p = \left(1-(1-1/\# )^{k|S|}\right)^k ")

which can be rearranged to obtain

![ \\\# = \\frac{1}{1-(1-p\^{1/k})\^{1/k|S|}}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%5Cfrac%7B1%7D%7B1-%281-p%5E%7B1%2Fk%7D%29%5E%7B1%2Fk%7CS%7C%7D%7D.%20&bg=ffffff&fg=000000&s=0 "   \# = \frac{1}{1-(1-p^{1/k})^{1/k|S|}}. ")

This is remarkably similar to the expression [\*\*] derived above for
repeating filters. In fact, provided
![p\^{1/k}](http://s.wordpress.com/latex.php?latex=p%5E%7B1%2Fk%7D&bg=ffffff&fg=000000&s=0 "p^{1/k}")
is much smaller than
![1](http://s.wordpress.com/latex.php?latex=1&bg=ffffff&fg=000000&s=0 "1"),
we get

![ \\\# \\approx \\frac{k|S|}{p\^{1/k}},
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%5Capprox%20%5Cfrac%7Bk%7CS%7C%7D%7Bp%5E%7B1%2Fk%7D%7D%2C%20&bg=ffffff&fg=000000&s=0 "   \# \approx \frac{k|S|}{p^{1/k}}, ")

which is exactly the same as [\*\*] when
![p\^{1/k}](http://s.wordpress.com/latex.php?latex=p%5E%7B1%2Fk%7D&bg=ffffff&fg=000000&s=0 "p^{1/k}")
is small. So this approach gives quite similar outcomes to the repeating
filter strategy.

Which approach is better, repeating or overlapping filters? In fact, it
can be shown that

![ \\frac{1}{1-(1-p\^{1/k})\^{1/k|S|}} \\leq
\\frac{k}{1-(1-p\^{1/k})\^{1/|S|}},
](http://s.wordpress.com/latex.php?latex=%20%20%20%5Cfrac%7B1%7D%7B1-%281-p%5E%7B1%2Fk%7D%29%5E%7B1%2Fk%7CS%7C%7D%7D%20%5Cleq%20%5Cfrac%7Bk%7D%7B1-%281-p%5E%7B1%2Fk%7D%29%5E%7B1%2F%7CS%7C%7D%7D%2C%20&bg=ffffff&fg=000000&s=0 "   \frac{1}{1-(1-p^{1/k})^{1/k|S|}} \leq \frac{k}{1-(1-p^{1/k})^{1/|S|}}, ")

and so the overlapping filter strategy is more memory efficient than the
repeating filter strategy. I won’t prove the inequality here – it’s a
straightforward (albeit tedious) exercise in calculus. The important
takeaway is that overlapping filters are the more memory-efficient
approach.

How do overlapping filters compare to our first approach, the basic
hashing strategy? I’ll defer a full answer until later, but we can get
some insight by choosing ![p =
0.0001](http://s.wordpress.com/latex.php?latex=p%20%3D%200.0001&bg=ffffff&fg=000000&s=0 "p = 0.0001")
and
![k=4](http://s.wordpress.com/latex.php?latex=k%3D4&bg=ffffff&fg=000000&s=0 "k=4").
Then for the overlapping filter we get ![\\\# \\approx
40|S|](http://s.wordpress.com/latex.php?latex=%5C%23%20%5Capprox%2040%7CS%7C&bg=ffffff&fg=000000&s=0 "\# \approx 40|S|"),
while the basic hashing strategy gives ![\\\# = |S| \\log( 10000
|S|)](http://s.wordpress.com/latex.php?latex=%5C%23%20%3D%20%7CS%7C%20%5Clog%28%2010000%20%7CS%7C%29&bg=ffffff&fg=000000&s=0 "\# = |S| \log( 10000 |S|)").
Basic hashing is worse whenever
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
is more than about 100 million – a big number, but also a big
improvement over the ![1.27 \*
10\^{28}](http://s.wordpress.com/latex.php?latex=1.27%20%2A%2010%5E%7B28%7D&bg=ffffff&fg=000000&s=0 "1.27 * 10^{28}")
required by a single filter. Given that we haven’t yet made any attempt
to optimize
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k"),
this ought to encourage us that we’re onto something.

### Problems for the author

-   I suspect that there’s a simple intuitive argument that would let us
    see upfront that overlapping filters will be more memory efficient
    than repeating filters. Can I find such an argument?

**Bloom filters:** We’re finally ready for Bloom filters. In fact, Bloom
filters involve only a few small changes to overlapping filters. In
describing overlapping filters we hashed into a bit array containing
![2\^m](http://s.wordpress.com/latex.php?latex=2%5Em&bg=ffffff&fg=000000&s=0 "2^m")
bits. We could, instead, have used hash functions with a range
![0,\\ldots,M-1](http://s.wordpress.com/latex.php?latex=0%2C%5Cldots%2CM-1&bg=ffffff&fg=000000&s=0 "0,\ldots,M-1")
and hashed into a bit array of
![M](http://s.wordpress.com/latex.php?latex=M&bg=ffffff&fg=000000&s=0 "M")
(instead of
![2\^m](http://s.wordpress.com/latex.php?latex=2%5Em&bg=ffffff&fg=000000&s=0 "2^m"))
bits. The analysis goes through unchanged if we do this, and we end up
with

![ p = \\left(1-(1-1/\\\# )\^{k|S|}\\right)\^k
](http://s.wordpress.com/latex.php?latex=%20%20%20p%20%3D%20%5Cleft%281-%281-1%2F%5C%23%20%29%5E%7Bk%7CS%7C%7D%5Cright%29%5Ek%20&bg=ffffff&fg=000000&s=0 "   p = \left(1-(1-1/\# )^{k|S|}\right)^k ")

and

![ \\\# = \\frac{1}{1-(1-p\^{1/k})\^{1/k|S|}},
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%5Cfrac%7B1%7D%7B1-%281-p%5E%7B1%2Fk%7D%29%5E%7B1%2Fk%7CS%7C%7D%7D%2C%20&bg=ffffff&fg=000000&s=0 "   \# = \frac{1}{1-(1-p^{1/k})^{1/k|S|}}, ")

exactly as before. The only reason I didn’t do this earlier is because
in deriving Equation [\*] above it was convenient to re-use the
reasoning from the basic hashing scheme, where
![m](http://s.wordpress.com/latex.php?latex=m&bg=ffffff&fg=000000&s=0 "m")
(not
![M](http://s.wordpress.com/latex.php?latex=M&bg=ffffff&fg=000000&s=0 "M"))
was the convenient parameter to use. But the exact same reasoning works.

What’s the best value of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
to choose? Put another way, what value of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
should we choose in order to minimize the number of bits,
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#"),
given a particular value for the probability of error,
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p"),
and a particular sizek
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")?
Equivalently, what value of
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
will minimize
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p"),
given
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#")
and
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")?
I won’t go through the full analysis here, but with calculus and some
algebra you can show that choosing

![ k \\approx \\frac{\\\#}{|S|} \\ln 2
](http://s.wordpress.com/latex.php?latex=%20%20%20k%20%5Capprox%20%5Cfrac%7B%5C%23%7D%7B%7CS%7C%7D%20%5Cln%202%20&bg=ffffff&fg=000000&s=0 "   k \approx \frac{\#}{|S|} \ln 2 ")

minimizes the probability
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p").
(Note that
![\\ln](http://s.wordpress.com/latex.php?latex=%5Cln&bg=ffffff&fg=000000&s=0 "\ln")
denotes the natural logarithm, not logarithms to base 2.) By choosing
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
in this way we get:

![ [\*\*\*] \\,\\,\\,\\, \\\# = \\frac{|S|}{\\ln 2} \\log \\frac{1}{p}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5B%2A%2A%2A%5D%20%5C%2C%5C%2C%5C%2C%5C%2C%20%5C%23%20%3D%20%5Cfrac%7B%7CS%7C%7D%7B%5Cln%202%7D%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D.%20&bg=ffffff&fg=000000&s=0 "   [***] \,\,\,\, \# = \frac{|S|}{\ln 2} \log \frac{1}{p}. ")

This really is good news! Not only is it better than a bit array, it’s
actually (usually) much better than the basic hashing scheme we began
with. In particular, it will be better whenever

![ \\frac{1}{\\ln 2} \\log \\frac{1}{p} \\leq \\log \\frac{|S|}{p},
](http://s.wordpress.com/latex.php?latex=%20%20%20%5Cfrac%7B1%7D%7B%5Cln%202%7D%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D%20%5Cleq%20%5Clog%20%5Cfrac%7B%7CS%7C%7D%7Bp%7D%2C%20&bg=ffffff&fg=000000&s=0 "   \frac{1}{\ln 2} \log \frac{1}{p} \leq \log \frac{|S|}{p}, ")

which is equivalent to requiring

![ |S| \\geq p\^{1-1/\\ln 2} \\approx \\frac{1}{p\^{0.44}}.
](http://s.wordpress.com/latex.php?latex=%20%20%20%7CS%7C%20%5Cgeq%20p%5E%7B1-1%2F%5Cln%202%7D%20%5Capprox%20%5Cfrac%7B1%7D%7Bp%5E%7B0.44%7D%7D.%20&bg=ffffff&fg=000000&s=0 "   |S| \geq p^{1-1/\ln 2} \approx \frac{1}{p^{0.44}}. ")

If we want (say)
![p=0.01](http://s.wordpress.com/latex.php?latex=p%3D0.01&bg=ffffff&fg=000000&s=0 "p=0.01")
this means that Bloom filter will be better whenever ![|S| \\geq
8](http://s.wordpress.com/latex.php?latex=%7CS%7C%20%5Cgeq%208&bg=ffffff&fg=000000&s=0 "|S| \geq 8"),
which is obviously an extremely modest set size.

Another way of interpreting [\*\*\*] is that a Bloom filter requires
![\\frac{1}{\\ln 2} \\log \\frac{1}{p} \\approx 1.44 \\log
\\frac{1}{p}](http://s.wordpress.com/latex.php?latex=%5Cfrac%7B1%7D%7B%5Cln%202%7D%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D%20%5Capprox%201.44%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D&bg=ffffff&fg=000000&s=0 "\frac{1}{\ln 2} \log \frac{1}{p} \approx 1.44 \log \frac{1}{p}")
bits per element of the set being represented. In fact, it’s possible to
prove that any data structure supporting the `add` and `test` operations
will require at least ![\\log
\\frac{1}{p}](http://s.wordpress.com/latex.php?latex=%5Clog%20%5Cfrac%7B1%7D%7Bp%7D&bg=ffffff&fg=000000&s=0 "\log \frac{1}{p}")
bits per element in the set. This means that Bloom filters are
near-optimal. Futher work has been done finding even more
memory-efficient data structures that actually meet the ![\\log
\\frac{1}{p}](http://s.wordpress.com/latex.php?latex=%5Clog%20%5Cfrac%7B1%7D%7Bp%7D&bg=ffffff&fg=000000&s=0 "\log \frac{1}{p}")
bound. See, for example, the paper by [Anna Pagh, Rasmus Pagh, and S.
Srinivasa
Rao](http://scholar.google.ca/scholar?cluster=13031359803369786500&hl=en&as_sdt=0,5).

### Problems for the author

-   Are the more memory-efficient algorithms practical? Should we be
    using them?

In actual applications of Bloom filters, we won’t know
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
in advance, nor
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|").
So the way we usually specify a Bloom filter is to specify the *maximum*
size
![n](http://s.wordpress.com/latex.php?latex=n&bg=ffffff&fg=000000&s=0 "n")
of set that we’d like to be able to represent, and the maximal
probability of error,
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p"),
that we’re willing to tolerate. Then we choose

![ \\\# = \\frac{n}{\\ln 2} \\log \\frac{1}{p}
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%5Cfrac%7Bn%7D%7B%5Cln%202%7D%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D%20&bg=ffffff&fg=000000&s=0 "   \# = \frac{n}{\ln 2} \log \frac{1}{p} ")

and

![ k = \\ln \\frac{1}{p}.
](http://s.wordpress.com/latex.php?latex=%20%20%20k%20%3D%20%5Cln%20%5Cfrac%7B1%7D%7Bp%7D.%20&bg=ffffff&fg=000000&s=0 "   k = \ln \frac{1}{p}. ")

This gives us a Bloom filter capable of representing any set up to size
![n](http://s.wordpress.com/latex.php?latex=n&bg=ffffff&fg=000000&s=0 "n"),
with probability of error guaranteed to be at most
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p").
The size
![n](http://s.wordpress.com/latex.php?latex=n&bg=ffffff&fg=000000&s=0 "n")
is called the *capacity* of the Bloom filter. Actually, these
expressions are slight simplifications, since the terms on the right may
not be integers – to be a little more pedantic, we choose

![ \\\# = \\lceil \\frac{n}{\\ln 2} \\log \\frac{1}{p} \\rceil
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%5Clceil%20%5Cfrac%7Bn%7D%7B%5Cln%202%7D%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D%20%5Crceil%20&bg=ffffff&fg=000000&s=0 "   \# = \lceil \frac{n}{\ln 2} \log \frac{1}{p} \rceil ")

and

![ k = \\lceil \\ln \\frac{1}{p} \\rceil.
](http://s.wordpress.com/latex.php?latex=%20%20%20k%20%3D%20%5Clceil%20%5Cln%20%5Cfrac%7B1%7D%7Bp%7D%20%5Crceil.%20&bg=ffffff&fg=000000&s=0 "   k = \lceil \ln \frac{1}{p} \rceil. ")

One thing that still bugs me about Bloom filters is the expression for
the optimal value for
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k").
I don’t have a good intuition for it – why is it logarithmic in
![1/p](http://s.wordpress.com/latex.php?latex=1%2Fp&bg=ffffff&fg=000000&s=0 "1/p"),
and why does it not depend on
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")?
There’s a tradeoff going on here that’s quite strange when you think
about it: bit arrays on their own aren’t very good, but if you repeat or
overlap them just the right number of times, then performance improves a
lot. And so you can think of Bloom filters as a kind of compromise
between an overlap strategy and a bit array strategy. But it’s really
not at all obvious (a) why choosing a compromise strategy is the best;
or (b) why the right point at which to compromise is where it is, i.e.,
why
![k](http://s.wordpress.com/latex.php?latex=k&bg=ffffff&fg=000000&s=0 "k")
has the form it does. I can’t quite answer these questions at this point
– I can’t see that far through Bloom filters. I suspect that
understanding the ![k =
2](http://s.wordpress.com/latex.php?latex=k%20%3D%202&bg=ffffff&fg=000000&s=0 "k = 2")
case really well would help, but haven’t put in the work. Anyone with
more insight is welcome to speak up!

**Summing up Bloom filters:** Let’s collect everything together. Suppose
we want a Bloom filter with capacity
![n](http://s.wordpress.com/latex.php?latex=n&bg=ffffff&fg=000000&s=0 "n"),
i.e., capable of representing any set
![S](http://s.wordpress.com/latex.php?latex=S&bg=ffffff&fg=000000&s=0 "S")
containing up to
![n](http://s.wordpress.com/latex.php?latex=n&bg=ffffff&fg=000000&s=0 "n")
elements, and such that `test` produces a false positive with
probability at most
![p](http://s.wordpress.com/latex.php?latex=p&bg=ffffff&fg=000000&s=0 "p").
Then we choose

![ k = \\lceil \\ln \\frac{1}{p} \\rceil
](http://s.wordpress.com/latex.php?latex=%20%20%20k%20%3D%20%5Clceil%20%5Cln%20%5Cfrac%7B1%7D%7Bp%7D%20%5Crceil%20&bg=ffffff&fg=000000&s=0 "   k = \lceil \ln \frac{1}{p} \rceil ")

independent hash functions, ![h\_0, h\_1, \\ldots,
h\_{k-1}](http://s.wordpress.com/latex.php?latex=h_0%2C%20h_1%2C%20%5Cldots%2C%20h_%7Bk-1%7D&bg=ffffff&fg=000000&s=0 "h_0, h_1, \ldots, h_{k-1}").
Each hash function has a range
![0,\\ldots,\\\#-1](http://s.wordpress.com/latex.php?latex=0%2C%5Cldots%2C%5C%23-1&bg=ffffff&fg=000000&s=0 "0,\ldots,\#-1"),
where
![\\\#](http://s.wordpress.com/latex.php?latex=%5C%23&bg=ffffff&fg=000000&s=0 "\#")
is the number of bits of memory our Bloom filter requires,

![ \\\# = \\lceil \\frac{n}{\\ln 2} \\log \\frac{1}{p} \\rceil.
](http://s.wordpress.com/latex.php?latex=%20%20%20%5C%23%20%3D%20%5Clceil%20%5Cfrac%7Bn%7D%7B%5Cln%202%7D%20%5Clog%20%5Cfrac%7B1%7D%7Bp%7D%20%5Crceil.%20&bg=ffffff&fg=000000&s=0 "   \# = \lceil \frac{n}{\ln 2} \log \frac{1}{p} \rceil. ")

We number the bits in our Bloom filter from
![0,\\ldots,\\\#-1](http://s.wordpress.com/latex.php?latex=0%2C%5Cldots%2C%5C%23-1&bg=ffffff&fg=000000&s=0 "0,\ldots,\#-1").
To `add` an element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
to our set we set the bits ![h\_0(x), h\_1(x), \\ldots,
h\_{\\\#-1}(x)](http://s.wordpress.com/latex.php?latex=h_0%28x%29%2C%20h_1%28x%29%2C%20%5Cldots%2C%20h_%7B%5C%23-1%7D%28x%29&bg=ffffff&fg=000000&s=0 "h_0(x), h_1(x), \ldots, h_{\#-1}(x)")
in the filter. And to `test` whether a given element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is in the set we simply check whether bits ![h\_0(x), h\_1(x), \\ldots,
h\_{\\\#-1}(x)](http://s.wordpress.com/latex.php?latex=h_0%28x%29%2C%20h_1%28x%29%2C%20%5Cldots%2C%20h_%7B%5C%23-1%7D%28x%29&bg=ffffff&fg=000000&s=0 "h_0(x), h_1(x), \ldots, h_{\#-1}(x)")
in the bit array are all set.

That’s all there is to the mechanics of how Bloom filters work! I won’t
give any sample code – I usually provide code samples in Python, but the
Python standard library lacks bit arrays, so nearly all of the code
would be concerned with defining a bit array class. That didn’t seem
like it’d be terribly illuminating. Of course, it’s not difficult to
find libraries implementing Bloom filters. For example, [Jason
Davies](http://www.jasondavies.com/) has written a javascript Bloom
filter which has a fun and informative [online interactive
visualisation](http://www.jasondavies.com/bloomfilter/). I recommend
checking it out. And I’ve personally used [Mike
Axiak](http://mike.axiak.net/)‘s fast C-based Python library
[pybloomfiltermmap](https://github.com/axiak/pybloomfiltermmap) – the
documentation is clear, it took just a few minutes to get up and
running, and I’ve generally had no problems.

### Problems

-   Suppose we have two Bloom filters, corresponding to sets
    ![S\_1](http://s.wordpress.com/latex.php?latex=S_1&bg=ffffff&fg=000000&s=0 "S_1")
    and
    ![S\_2](http://s.wordpress.com/latex.php?latex=S_2&bg=ffffff&fg=000000&s=0 "S_2").
    How can we construct the Bloom filters corresponding to the sets
    ![S\_1 \\cup
    S\_2](http://s.wordpress.com/latex.php?latex=S_1%20%5Ccup%20S_2&bg=ffffff&fg=000000&s=0 "S_1 \cup S_2")
    and ![S\_1 \\cap
    S\_2](http://s.wordpress.com/latex.php?latex=S_1%20%5Ccap%20S_2&bg=ffffff&fg=000000&s=0 "S_1 \cap S_2")?

**Applications of Bloom filters:** Bloom filters have been used to solve
many different problems. Here’s just a few examples to give the flavour
of how they can be used. An early idea was Manber and Wu’s 1994
[proposal](http://scholar.google.ca/scholar?cluster=2662095141699171069)
to use Bloom filters to store lists of weak passwords. Google’s
[BigTable](http://research.google.com/archive/bigtable.html) storage
system uses Bloom filters to speed up queries, by avoiding disk accesses
for rows or columns that don’t exist. Google Chrome uses Bloom filters
to do [safe web
browsing](http://src.chromium.org/viewvc/chrome/trunk/src/chrome/browser/safe_browsing/bloom_filter.h?view=markup)
– the opening example in this post was quite real! More generally, it’s
useful to consider using Bloom filters whenever a large collection of
objects needs to be stored. They’re not appropriate for all purposes,
but at the least it’s worth thinking about whether or not a Bloom filter
can be applied.

**Extensions of Bloom filters:** There’s many clever ways of extending
Bloom filters. I’ll briefly describe one, just to give you the flavour,
and provide links to several more.

**A delete operation:** It’s possible to modify Bloom filters so they
support a `delete` operation that lets you remove an element from the
set. You can’t do this with a standard Bloom filter: it would require
unsetting one or more of the bits ![h\_0(x), h\_1(x),
\\ldots](http://s.wordpress.com/latex.php?latex=h_0%28x%29%2C%20h_1%28x%29%2C%20%5Cldots&bg=ffffff&fg=000000&s=0 "h_0(x), h_1(x), \ldots")
in the bit array. This could easily lead us to accidentally `delete`
*other* elements in the set as well.

Instead, the `delete` operation is implemented using an idea known as a
*counting Bloom filter*. The basic idea is to take a standard Bloom
filter, and replace each bit in the bit array by a bucket containing
several bits (usually 3 or 4 bits). We’re going to treat those buckets
as counters, initially set to
![0](http://s.wordpress.com/latex.php?latex=0&bg=ffffff&fg=000000&s=0 "0").
We `add` an element
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
to the counting Bloom filter by *incrementing* each of the buckets
numbered ![h\_0(x), h\_1(x),
\\ldots](http://s.wordpress.com/latex.php?latex=h_0%28x%29%2C%20h_1%28x%29%2C%20%5Cldots&bg=ffffff&fg=000000&s=0 "h_0(x), h_1(x), \ldots").
We `test` whether
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is in the counting Bloom filter by looking to see whether each of the
corresponding buckets are non-zero. And we `delete`
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
by decrementing each bucket.

This strategy avoids the accidental deletion problem, because when two
elements of the set
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
and
![y](http://s.wordpress.com/latex.php?latex=y&bg=ffffff&fg=000000&s=0 "y")
hash into the same bucket, the count in that bucket will be at least
![2](http://s.wordpress.com/latex.php?latex=2&bg=ffffff&fg=000000&s=0 "2").
`delete`ing one of the elements, say
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x"),
will still leave the count for the bucket at least
![1](http://s.wordpress.com/latex.php?latex=1&bg=ffffff&fg=000000&s=0 "1"),
so
![y](http://s.wordpress.com/latex.php?latex=y&bg=ffffff&fg=000000&s=0 "y")
won’t be accidentally deleted. Of course, you could worry that this will
lead us to erroneously conclude that
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
is still in the set after it’s been deleted. But that can only happen if
other elements in the set hash into every single bucket that
![x](http://s.wordpress.com/latex.php?latex=x&bg=ffffff&fg=000000&s=0 "x")
hashes into. That will only happen if
![|S|](http://s.wordpress.com/latex.php?latex=%7CS%7C&bg=ffffff&fg=000000&s=0 "|S|")
is very large.

Of course, that’s just the basic idea behind counting Bloom filters. A
full analysis requires us to understand issues such as bucket overflow
(when a counter gets incremented too many times), the optimal size for
buckets, the probability of errors, and so on. I won’t get into that,
but you there’s details in the further reading, below.

**Other variations and further reading:** There are many more variations
on Bloom filters. Just to give you the flavour of a few applications:
(1) they can be modified to be used as lookup dictionaries, associating
a value with each element `add`ed to the filter; (2) they can be
modified so that the capacity scales up dynamically; and (3) they can be
used to quickly approximate the number of elements in a set. There are
many more variations as well: Bloom filters have turned out to be a very
generative idea! This is part of why it’s useful to understand them
deeply, since even if a standard Bloom filter can’t solve the particular
problem you’re considering, it may be possible to come up with a
variation which does. You can get some idea of the scope of the known
variations by looking at the [Wikipedia
article](http://en.wikipedia.org/wiki/Bloom_filter). I also like the
[survey
article](http://scholar.google.ca/scholar?cluster=7837240630058449829&hl=en&as_sdt=0,5)
by [Andrei Broder](http://en.wikipedia.org/wiki/Andrei_Broder) and
[Michael Mitzenmacher](http://mybiasedcoin.blogspot.ca/). It’s a little
more dated (2004) than the Wikipedia article, but nicely written and a
good introduction. For a shorter introduction to some variations,
there’s also a recent [blog
post](http://matthias.vallentin.net/blog/2011/06/a-garden-variety-of-bloom-filters/)
by [Matthias Vallentin](http://matthias.vallentin.net/). You can get the
flavour of current research by looking at some of the papers citing
Bloom filters
[here](http://academic.research.microsoft.com/Publication/772630/space-time-trade-offs-in-hash-coding-with-allowable-errors).
Finally, you may enjoy reading the [original paper on Bloom
filters](http://scholar.google.ca/scholar?cluster=11454588508174765009&hl=en&as_sdt=0,5),
as well as the [original paper on counting Bloom
filters](http://scholar.google.ca/scholar?cluster=18066790496670563714&hl=en&as_sdt=0,5).

**Understanding data structures:** I wrote this post because I recently
realized that I didn’t understand any complex data structure in any sort
of depth. There are, of course, a huge number of striking data
structures in computer science – just look at [Wikipedia’s amazing
list](http://en.wikipedia.org/wiki/List_of_data_structures)! And while
I’m familiar with many of the simpler data structures, I’m ignorant of
most complex data structures. There’s nothing wrong with that – unless
one is a specialist in data structures there’s no need to master a long
laundry list. But what bothered me is that I hadn’t *thoroughly*
mastered even a single complex data structure. In some sense, I didn’t
know what it means to understand a complex data structure, at least
beyond surface mechanics. By trying to reinvent Bloom filters, I’ve
found that I’ve deepened my own understanding and, I hope, written
something of interest to others.

*Interested in more? Please [subscribe to this
blog](http://www.michaelnielsen.org/ddi/feed/), or [follow me on
Twitter](http://twitter.com/\#!/michael_nielsen). You may also enjoy
reading my new book about open science, [Reinventing
Discovery](http://www.amazon.com/Reinventing-Discovery-New-Networked-Science/dp/product-description/0691148902).*

From →
[Uncategorized](http://www.michaelnielsen.org/ddi/category/uncategorized/ "View all posts in Uncategorized")

17 Comments [Leave one →](#respond "Leave one →")

1.  ![image](http://0.gravatar.com/avatar/afaffb3cc93fe942bb2d4aa3076c062d?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    [David Meyer](http://math.ucsd.edu/~dmeyer)
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-1974)

    Very nice post about a topic in which I didn’t know I was
    interested! I find I am interested because of some analogies with
    physics: Storing a list of hashes is like representing a
    multiparticle state in first quantized form, e.g., a list of |S|
    positions. Storing them as |S| 1s in a bit array indexed by hashes—a
    filter—is like representing the state in second quantized form,
    i.e., as the occupation numbers of a list of positions. Moving to
    multiple (k) filters is what one would do if the particles live in k
    dimensions.

    So what’s the particle (physics) analogy for overlapping the
    filters? It is the inverse of the possibly more familiar idea of
    taking k particles in 1 dimension and representing their state by a
    single “particle” in k dimensions with coordinates the set of 1
    dimensional coordinates. Bloom filters optimize the choice of the
    number of dimensions in which the “particle” lives.

    Finally, handling the delete operation by moving to counting Bloom
    filters has the physics analogy of removing the exclusion principle
    on the second quantized representation, i.e., switching from
    fermions to bosons (or at least partly removing it in the case where
    the buckets have some finite maximum size).

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=1974#respond)

    -   ![image](http://0.gravatar.com/avatar/e5cbb32cf54a237e409a3608fc2f88d1?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

        [Michael Nielsen](http://michaelnielsen.org/blog)
        [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-2160)

        David – Interesting way of thinking. I wonder what exactly is
        being optimized in this analogy? In what sense is this best?
        Relatedly, what do multiple hash collisions mean?

        [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=2160#respond)

    -   ![image](http://1.gravatar.com/avatar/7b4707f974af261f71943e1f2046c9ee?s=40&d=http%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

        [SonOfLilit](http://iconcurandfurthermore.tumblr.com/)
        [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-3921)

        This completely blew my mind. Thanks.

        [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=3921#respond)

2.  ![image](http://0.gravatar.com/avatar/a791ba7f399022c60bfd462f9fdba5da?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    [Andrew](http://andrewartajos.wordpress.com)
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-1986)

    There’s a code kata for this. I enjoyed implementing every bit of
    it.\

    [http://codekata.pragprog.com/2007/01/kata\_five\_bloom.html](http://codekata.pragprog.com/2007/01/kata_five_bloom.html)

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=1986#respond)

    -   ![image](http://0.gravatar.com/avatar/e5cbb32cf54a237e409a3608fc2f88d1?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

        [Michael Nielsen](http://michaelnielsen.org/blog)
        [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-2161)

        That’s a great kata! Spell checking is a fun application for
        this kind of thing, and I’ve read that it was a genuine early
        application of Bloom filters.

        [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=2161#respond)

        -   ![image](http://0.gravatar.com/avatar/49d1543d004a2c48717533ec95de6448?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

            Someone
            [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-6219)

            Well, the link doesn’t work anymore.

            [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=6219#respond)

3.  ![image](http://1.gravatar.com/avatar/fc9f0c2010542aaa9edeb219365f71bd?s=40&d=http%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    Honkmaster
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-1991)

    Oh cool, you lost me with your mathematic notation nonsense. You
    should have stayed with the web browser example.

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=1991#respond)

    -   ![image](http://0.gravatar.com/avatar/e5cbb32cf54a237e409a3608fc2f88d1?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

        [Michael Nielsen](http://michaelnielsen.org/blog)
        [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-2162)

        I genuinely doubt that would have helped more than it hindered.
        Sometimes, “mathematic notation nonsense” is necessary to
        understand something well.

        [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=2162#respond)

        -   ![image](http://0.gravatar.com/avatar/607914b5055a9462c24108b8f7542a5c?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

            Scott
            [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-6221)

            I agree that sometimes the mathematical notation is
            necessary – but a lot of people discovered a love for math
            and algorithmics later in life. I had horrible math teachers
            growing up that could not explain these things and didn’t
            care if it was as muddy as pea soup.

            But from a self-taught perspective – the explanation of
            “this operation will take n\*n\*n number of iterations in a
            list” is less terse but easier to explain than the
            notational representation.

            [http://bigocheatsheet.com/](http://bigocheatsheet.com/)

            Good info about understanding the “notational nonsense”

            [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=6221#respond)

4.  ![image](http://0.gravatar.com/avatar/630c001f9566f323959b7e94e87e708b?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    Bill
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-2189)

    You wrote:\
     ” I suspect that there’s a simple intuitive argument that would let
    us see upfront that overlapping filters will be more memory
    efficient than repeating filters. Can I find such an argument? ”

    I’ll take a quick stab at this one.

    I think the best way to tackle this is to start by visualizing the
    extreme cases: What happens when the bit map is all ones? What does
    it signify if the bit map is all zeros? Then back off from the
    extremes just a tad: Almost all ones, or almost all zeros?

    Once that is done it is easy to rationalize that either extreme is
    inefficient in terms of memory. Almost all ones means the bit map is
    too small for a given set, has very little information density
    (large p), and is thus inefficient. Almost all zeros means the bit
    map is too large for a given p and thus we are wasting memory.
    Intuitively we can grasp that optimum memory efficiency is somewhere
    between the two extremes. Wikipedia says max efficiency occurs at
    \~50% ones, but I would think optimum would be a smidgen over 50%

    So if we set our goal to be \~ 50% ones, we can evaluate the two
    choices: overlapping vs. repeating in that light.

    The next step is a bit harder to do without resorting to math. Let’s
    ask the question does p linearly track bit size? Intuitively I do
    not think so because I see from my study of the extremes that p will
    move very slowly near the all zeros condition, but will increase
    very quickly as the bit map becomes saturated near the all ones
    condition. If p went up linearly then overlapping vs. repeating
    algorithms would yield approximately the same p for a given memory
    size. But since it is not linear, just a small increase in the size
    of the bit map will cause a large change in p as we near the extreme
    all ones case. The overlapping filter option allows a small increase
    in bit map size thus dropping p quickly vs. the repeating filter
    option which stair steps the memory at k\*M (hope I wrote that
    right)

    Putting it together. We want to be somewhere near 50% ones. The
    repeating filter option is k times as large memory wise than the
    overlapping filter option. Since p does linearly track with bit map
    size we can always create a more efficient bit map using the
    overlapping filters than we can with the repeating filters. The
    repeating method will always have more zeros than ones for a given p
    and will thus not have the same degree of information density for a
    given memory size.

    Hope my thinking is clear and accurate. Even if not I had a lot of
    fun reading your article. This one post got me to subscribe. Thanks.

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=2189#respond)

5.  ![image](http://1.gravatar.com/avatar/70e68e456577d15a045e3699079a6236?s=40&d=http%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    Titus Brown
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-3599)

    Hi Michael, just finished reading your book on Reinventing Discovery
    (fantastic!) and thought I’d look you up on twitter, and then ran
    across this exposition on Bloom filters. I’d seen it before but
    hadn’t connected the dots to you!

    Anyway, thought you might be interested in our work in applying
    Bloom filters to DNA sequence assembly —
    [http://www.pnas.org/content/early/2012/07/25/1121464109.full.pdf](http://www.pnas.org/content/early/2012/07/25/1121464109.full.pdf).
    You can also look at rayan chaikhi’s follow up work on actually
    building an assembler on Bloom filters, too.

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=3599#respond)

6.  ![image](http://0.gravatar.com/avatar/abc2d1fd69e261fb86016bc32c140726?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    Tobin Baker
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-3608)

    I think another way to think about it is that you can improve the
    resolution of a simple bitvector by hashing to subsets of cells
    rather than just single cells. Of course, then analysis of
    collisions becomes more complicated…

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=3608#respond)

7.  ![image](http://1.gravatar.com/avatar/7b4707f974af261f71943e1f2046c9ee?s=40&d=http%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    [SonOfLilit](http://iconcurandfurthermore.tumblr.com/)
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-3920)

    My own take on the first author’s problem, as seen here:
    [http://iconcurandfurthermore.tumblr.com/post/47807349687/why-bloom-filters-work-the-way-they-do-ddi](http://iconcurandfurthermore.tumblr.com/post/47807349687/why-bloom-filters-work-the-way-they-do-ddi):

    Overlapping k filters of size |S| memory size \# with Probability of
    Collision p each, in the worst case where the set bits of each
    filter never overlap, would be like increasing |S| by a factor of k.
    This would yield for each PoC \~ k|S|/\# = kp.

    Increasing those filters’ memory sizes to k\# would improve their
    specific PoCs to |S|/k\# = p/k.

    Therefore, doing both would leave each filter’s PoC at \~ kp/k = p,
    which means , k filters of memory size \# and k overlapping filters
    of memory size k\# perform the same in the same budget of k\#
    memory.

    In practice there are collisions between the filters’ set bits,
    making the effect of overlap less significant than the factor of k
    without changing any other calculation we did. Therefore,
    overlapping filters are (at least a bit) better.

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=3920#respond)

8.  ![image](http://1.gravatar.com/avatar/317bb6b3e9692ae8e8ad093e18402ac3?s=40&d=http%3A%2F%2F1.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    Phil Goetz
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-5986)

    I could’ve used this for my database of a billion protein accessions
    to check for annotations. I worked around the problem with table
    precomputations, which was probably better in the long run, but it
    is difficult to keep all the data sets in sync with each other now.

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=5986#respond)

9.  ![image](http://0.gravatar.com/avatar/24e08a9ea84deb17ae121074d0f17125?s=40&d=http%3A%2F%2F0.gravatar.com%2Favatar%2Fad516503a11cd5ca435acc9bb6523536%3Fs%3D40&r=G)

    [Mathias Bynens](http://mathiasbynens.be/)
    [permalink](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-6218)

    Note that Chromium uses a prefix set nowadays rather than a Bloom
    filter:
    [http://src.chromium.org/viewvc/chrome/trunk/src/chrome/browser/safe\_browsing/safe\_browsing\_store\_file.h](http://src.chromium.org/viewvc/chrome/trunk/src/chrome/browser/safe_browsing/safe_browsing_store_file.h)

    [Reply](/ddi/why-bloom-filters-work-the-way-they-do/?replytocom=6218#respond)

### Trackbacks and Pingbacks

1.  [Why Bloom filters work the way they do «
    thoughts…](http://irrlab.com/2012/09/27/why-bloom-filters-work-the-way-they-do/)
2.  [Structured procrastination, 2 Dec 2012 | Code for
    Life](http://sciblogs.co.nz/code-for-life/2012/12/03/structured-procrastination-2-dec-2012/)

### Leave a Reply [Cancel reply](/ddi/why-bloom-filters-work-the-way-they-do/#respond)

Name (required)

Email (required, will not be published)

Website

Comment

**Note:** HTML is allowed. Your email address will not be published.

[Subscribe to this comment feed via
RSS](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/feed/)

Follow Michael
--------------

-   [![image](http://michaelnielsen.org/blog/wp-content/themes/titan_pro/images/flw-rss.png)
    Subscribe to this blog](http://www.michaelnielsen.org/ddi/feed/)
-   [Michael's main blog](http://michaelnielsen.org/blog)
-   [![image](http://michaelnielsen.org/blog/wp-content/themes/titan_pro/images/flw-twitter.png)
    Follow Michael on Twitter](http://twitter.com/#!/michael_nielsen)

Other places on the web
-----------------------

-   [GitHub](https://github.com/mnielsen)
-   [Delicious](http://delicious.com/nielsen)
-   [Delicious research links for book
    project](http://delicious.com/nielsen/nnftd)
-   [Delicious research links for last
    book](http://delicious.com/nielsen/tfos)
-   [Google Plus](https://plus.google.com/116973436260300431929/posts)

Books
-----

[![image](http://ws.assoc-amazon.com/widgets/q?_encoding=UTF8&Format=_SL110_&ASIN=0691148902&MarketPlace=US&ID=AsinImage&WS=1&tag=michaniels-20&ServiceVersion=20070822)](http://www.amazon.com/gp/product/0691148902/ref=as_li_tf_il?ie=UTF8&tag=michaniels-20&linkCode=as2&camp=217145&creative=399373&creativeASIN=0691148902)![image](http://www.assoc-amazon.com/e/ir?t=michaniels-20&l=as2&o=1&a=0691148902&camp=217145&creative=399373)
\
 [Reinventing Discovery: The New Era of Networked
Science](http://www.amazon.com/gp/product/0691148902/ref=as_li_tf_tl?ie=UTF8&tag=michaniels-20&linkCode=as2&camp=217145&creative=399373&creativeASIN=0691148902)![image](http://www.assoc-amazon.com/e/ir?t=michaniels-20&l=as2&o=1&a=0691148902&camp=217145&creative=399373)
[(Errata)](http://michaelnielsen.org/blog/errata-for-reinventing-discovery/)
\
 \

[![image](http://ws.assoc-amazon.com/widgets/q?_encoding=UTF8&ASIN=1107002176&Format=_SL110_&ID=AsinImage&MarketPlace=US&ServiceVersion=20070822&WS=1&tag=michaniels-20)](http://www.amazon.com/gp/product/1107002176/ref=as_li_tf_il?ie=UTF8&camp=1789&creative=9325&creativeASIN=1107002176&linkCode=as2&tag=michaniels-20)![image](http://www.assoc-amazon.com/e/ir?t=michaniels-20&l=as2&o=1&a=1107002176)
\
 [Quantum Computation and Quantum
Information](http://www.amazon.com/gp/product/1107002176/ref=as_li_tf_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1107002176&linkCode=as2&tag=michaniels-20)![image](http://www.assoc-amazon.com/e/ir?t=michaniels-20&l=as2&o=1&a=1107002176)

Other projects
--------------

-   [Essays](http://michaelnielsen.org/blog/essays/)
-   [TEDxWaterloo video about open
    science](http://www.youtube.com/watch?v=DnWocYKqvhw&feature=player_embedded)
-   [Quantum computing for the
    determined](http://michaelnielsen.org/blog/quantum-computing-for-the-determined/)
-   [Polymath
    wiki](http://michaelnielsen.org/polymath1/index.php?title=Main_Page)

Recent Posts
------------

-   [Reinventing
    Explanation](http://www.michaelnielsen.org/ddi/reinventing-explanation/)
-   [How the Bitcoin protocol actually
    works](http://www.michaelnielsen.org/ddi/how-the-bitcoin-protocol-actually-works/)
-   [Neural Networks and Deep Learning: first chapter now
    live](http://www.michaelnielsen.org/ddi/neural-networks-and-deep-learning-first-chapter-now-live/)
-   [Why Bloom filters work the way they
    do](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/)
-   [How to crawl a quarter billion webpages in 40
    hours](http://www.michaelnielsen.org/ddi/how-to-crawl-a-quarter-billion-webpages-in-40-hours/)

Recent Comments
---------------

-   Scott on [Why Bloom filters work the way they
    do](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-6221)
-   Someone on [Why Bloom filters work the way they
    do](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-6219)
-   [Mathias Bynens](http://mathiasbynens.be/) on [Why Bloom filters
    work the way they
    do](http://www.michaelnielsen.org/ddi/why-bloom-filters-work-the-way-they-do/#comment-6218)
-   Michael Nielsen on [How the Bitcoin protocol actually
    works](http://www.michaelnielsen.org/ddi/how-the-bitcoin-protocol-actually-works/#comment-6216)
-   Dave on [How the Bitcoin protocol actually
    works](http://www.michaelnielsen.org/ddi/how-the-bitcoin-protocol-actually-works/#comment-6215)

[![RSS](http://www.michaelnielsen.org/ddi/wp-includes/images/rss.png)](http://feeds.delicious.com/v2/rss/nielsen/linklog?count=5 "Syndicate this content") [Linklog](http://previous.delicious.com/nielsen/linklog "bookmarks tagged linklog by nielsen")
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   [Internet of Dreams -
    o<](http://internet-of-dreams.tumblr.com/post/50644305113/o "Good interview with an online troll: "I am lucky to have not had people who liked me for who I was when I was truly awful"")
    Good interview with an online troll: "I am lucky to have not had
    people who liked me for who I was when I was truly awful"
-   [Algorithmic Rape Jokes in the Library of Babel | Quiet
    Babylon](http://quietbabylon.com/2013/algorithmic-rape-jokes-in-the-library-of-babel/ "On the problems caused when you create tshirts using a data mining algorithm.   As Kevin Slavin has noted, this is how our culture is increasingly being created.")
    On the problems caused when you create tshirts using a data mining
    algorithm. As Kevin Slavin has noted, this is how our culture is
    increasingly being created. […]
-   [Dan Pallotta: The way we think about charity is dead wrong | Video
    on
    TED.com](http://www.ted.com/talks/dan_pallotta_the_way_we_think_about_charity_is_dead_wrong.html?utm_source=newsletter_weekly_2013-03-16&utm_campaign=newsletter_weekly&utm_medium=email&utm_content=talk_of_the_week_button "An extremely insightful talk about the way our society thinks about not-for-profit organizations.  He highlights many of the systematic ways we hobble the not-for-profit sector.")
    An extremely insightful talk about the way our society thinks about
    not-for-profit organizations. He highlights many of the systematic
    ways we hobble the not-for-profit sector. […]
-   [Agnotology: The Making and Unmaking of Ignorance - Edited by Robert
    N. Proctor and Londa
    Schiebinger](http://www.sup.org/book.cgi?id=11232 "Linked mostly for the title, which is the useful term "agnotology" - the study of ignorance.")
    Linked mostly for the title, which is the useful term "agnotology" -
    the study of ignorance.
-   [How do I create a book
    index?](http://ask.metafilter.com/4835/How-do-I-create-a-book-index "Fun discussion of the minutiae of creating a book index.")
    Fun discussion of the minutiae of creating a book index.

[![RSS](http://www.michaelnielsen.org/ddi/wp-includes/images/rss.png)](http://feeds.delicious.com/v2/rss/nielsen/documentary?count=5 "Syndicate this content") [Documentaries](http://previous.delicious.com/nielsen/documentary "bookmarks tagged documentary by nielsen")
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

-   [Visual Storytelling: The Digital Video
    Documentary](http://documentarystudies.duke.edu/uploads/media_items/visual-storytelling-the-digital-video-documentary.original.pdf "A brief skim suggests that this is a very good basic guide to making documentaries.")
    A brief skim suggests that this is a very good basic guide to making
    documentaries.
-   [The Great Ecstasy of the Woodcarver Steiner - Werner
    Herzog](http://www.youtube.com/watch?feature=player_embedded&v=liYnvIBLMBQ "Documentary of Wolfgang Steiner, one of the world's top ski-jumpers in the 1970s.  The spine of the documentary is a sequence of extraordinary shots of Steiner's jumps, taken with a pair of high-speed cameras.")
    Documentary of Wolfgang Steiner, one of the world's top ski-jumpers
    in the 1970s. The spine of the documentary is a sequence of
    extraordinary shots of Steiner's jumps, taken with a pair of
    high-speed cameras. […]
-   [Inge Druckrey: Teaching to
    See](http://vimeo.com/45232468 "A reminder of the diversity of life: the passion and learning and insight people pour into calligraphy, font design, and design more generally.")
    A reminder of the diversity of life: the passion and learning and
    insight people pour into calligraphy, font design, and design more
    generally.
-   [Indie Game: The Movie (2012) -
    IMDb](http://www.imdb.com/title/tt1942884/ "Striking for the level of emotional commitment and uncertainty (and turmoil) on the part of the creators!")
    Striking for the level of emotional commitment and uncertainty (and
    turmoil) on the part of the creators!
-   [Step Into Liquid
    (2003)](http://www.imdb.com/title/tt0308508/ "Remarkable survey of the cutting edge of surfing.  We see the origins of tow-rope surfing (where surfers are pulled by jet skis into waves that are too big to paddle out to), the use of hydrofoil designs that put the board a foot or two _above_ the wave, and even the use of weather stations to monitor conditions out in mid-ocean, looking for giant waves.")
    Remarkable survey of the cutting edge of surfing. We see the origins
    of tow-rope surfing (where surfers are pulled by jet skis into waves
    that are too big to paddle out to), the use of hydrofoil designs
    that put the board a foot or two \_above\_ the wave, and even the
    use of weather stations to monitor conditions out in mid-ocean,
    looking for giant waves. […]

Search
------

Archives
--------

-   [January 2014](http://www.michaelnielsen.org/ddi/2014/01/)
-   [December 2013](http://www.michaelnielsen.org/ddi/2013/12/)
-   [November 2013](http://www.michaelnielsen.org/ddi/2013/11/)
-   [September 2012](http://www.michaelnielsen.org/ddi/2012/09/)
-   [August 2012](http://www.michaelnielsen.org/ddi/2012/08/)
-   [June 2012](http://www.michaelnielsen.org/ddi/2012/06/)
-   [April 2012](http://www.michaelnielsen.org/ddi/2012/04/)
-   [March 2012](http://www.michaelnielsen.org/ddi/2012/03/)
-   [February 2012](http://www.michaelnielsen.org/ddi/2012/02/)
-   [January 2012](http://www.michaelnielsen.org/ddi/2012/01/)
-   [July 2011](http://www.michaelnielsen.org/ddi/2011/07/)
-   [June 2011](http://www.michaelnielsen.org/ddi/2011/06/)

