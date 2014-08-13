[Burak Kanber, Engineer](/blog/)
================================

### The Blog

[Follow @bkanber](https://twitter.com/bkanber)

[Machine Learning: k-Means Clustering Algorithm in Javascript](http://burakkanber.com/blog/machine-learning-k-means-clustering-in-javascript-part-1/)
-----------------------------------------------------------------------------------------------------------------------------------------------------

On October 15, 2012

This article is part of the [Machine Learning in
Javascript](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/ "Machine Learning in Javascript: Introduction")
series. The series covers some of the essential machine learning
algorithms and assumes little background knowledge. There’s also a
mailing list at the bottom of the page if you want to know about new
articles; you can also [follow me on twitter:
@bkanber](https://twitter.com/bkanber).

Are you just looking for the code example? [Scroll down!](#fiddle)

Introduction and Motivation
---------------------------

Machine learning helps us navigate and process large volumes of data. We
can ask all sorts of questions about our data, and hope that ML can
answer them: what is this data point most similar to? Does the data come
in patterns? Can we predict what will happen in the future, given past
trends? These questions are applicable to all fields of study.

Today we’re going to figure out how to find clusters of data points.
Let’s say you work at a medical imaging devices company. Imagine you
already have a way to identify malignant cells from an image scan, but
it would be great to automatically identify the centers of clusters of
cells as well. Then a robot could go in with surgical precision and
remove the problem!

What we’re looking for is a clustering algorithm; today we’re going to
talk specifically about the k-means algorithm.

Clustering
----------

Clustering algorithms, in general, find groups of similar pieces of
data. If you run an online store you might use a clustering algorithm to
identify different shopper types. You may find that you have one type of
visitor that just window shops through 3-5 pages of products and leaves.
Another group might make meticulous purchasing decisions by looking
through 15 pages of products and reviews and end up making only one,
high-value purchase. And you may also identify the impulse buyer, who
makes numerous small purchases without browsing too deeply. Once you’ve
identified your e-shopper demographics, you’re better able to optimize
your site to increase sales. You can release features that appeal to
your impulse buyers, because now you know that you *have*impulse buyers!

And while that’s just one practical example of k-means, you’ll find this
algorithm used in multiple fields. Sometimes it’s just image processing
in 2 dimensions, other times it’s processing huge data across dozens of
dimensions and parameters. Like our k-nearest-neighbor algorithm,
k-means is versatile, simple to understand and implement, and sneakily
powerful.

k-means
-------

Like k-nearest-neighbor, the “k” in k-means gives away that there’s
going to be some number that we’re going to have to feed to our
algorithm. Specifically, “k” is the number of clusters we’re going to
find in our data. Unfortunately, it’s rarely possible to know the number
of clusters before you solve the problem, so k-means is usually
supplemented by another algorithm that first helps you find the best
value of *k*.

The issue is this: the k-means algorithm will partition your data into
“k” distinct clusters, but it does not tell you if that’s the
*correct*number of clusters. Your data might naturally have 5 different
clusters in it, but if you feed k-means the number 3 you’ll get 3
clusters back. Those clusters will be bigger, looser and more awkwardly
shaped than if you had told it to find 5 clusters.

The long and short of it is this: in order to use k-means you either
need to know how many clusters you’re looking for at the outset, *or*you
have to use a second algorithm to also guess the number of clusters.
K-means just organizes your points into clusters; you need to do
something else to figure out the right number of clusters.

For today we’ll contrive a situation and use three clusters from the
outset. Next time (in k-means part 2) we’ll look at a technique you can
use to automatically guess the value of “k”. Most often, these
algorithms rely on some kind of error analysis and multiple passes of
the k-means algorithm in order to optimize for the solution with the
smallest error value.

The Procedure
-------------

The k-means algorithm is simple but can become very powerful if you’re
using it on a dataset with many dimensions. Today we’re going to work in
2 dimensions, but next time we’ll do something more complicated. Here’s
what the algorithm looks like:

1.  Plot your data points
2.  Create “k” *additional* points, placing them randomly on your graph.
    These points are the “cluster centroids” — or the candidates for the
    centers of your clusters.
3.  Repeat the following:
    1.  “Assign” each data point to the cluster centroid closest to it
    2.  Move the centroid to the average position of all the data points
        that belong to it
    3.  If any of the centroids moved in the last step, repeat. If
        nothing moved, exit.

It’s that simple! As you can see, this is an iterative process. It may
take 2 or 3 or dozens of iterations, but eventually your cluster
centroids should converge to their solutions and stop moving. You then
take the final tally of the assignments and then you have your clusters.

Committee of Machines
---------------------

This algorithm, like many of the one we’ll play with in this series, is
susceptible to local optima. If you run the example below a few times
you’ll see that the clusters can end up in one of a few different
configurations. These are various local optima that the solution gets
stuck in. Algorithms that start with some sort of random seed state
(like GAs or k-means) are particularly susceptible to local optima,
because you never really know how the algorithm will start off and which
path the solution will end up following. Will this seed state lead to a
local or global optima? There’s no way of knowing!

Like in genetic algorithms, one way to shake out of local optima is to
give the solution a little bit of mutation. In our k-means example, we
could add a rule that gives a centroid a nudge in a random direction if
it doesn’t move after an iteration. It might settle back into its last
resting place, or it may find a new solution. The nudge shouldn’t be big
enough that it restarts the solution from the beginning, but just enough
to kick a centroid out of a local valley, if it’s in one.

Another technique we could use is called the “committee of machines”,
which works well if you’re running an algorithm that finishes pretty
quickly, or if you have parallel computing capabilities. It’s simple: we
run the k-means algorithm 3 or 5 or 51 or 10,000 times, and choose the
solution that it returned the most often. The term “committee of
machines” alludes to the fact that some people choose to actually run
parallel algorithms on different pieces of hardware, and a literal
committee of machines votes on the solution.

The Code
--------

Let’s dive in. Unlike my other examples thus far, I’m going to forego an
object-oriented implementation and just go straight procedural. There
are many ways to skin a cat. I love OOP but it’s important not to get
too comfortable in habits!

Additionally, while we’re only working with 2 dimensional data in this
example, I’d like to write this algorithm out to handle any number of
dimensions (except for the canvas drawing functions).

Let’s take a look at the data we’re using — a simple array of “points”,
which are just represented by 2-element arrays (for X and Y values):

~~~~ {.code}
var data = [
    [1, 2],
    [2, 1],
    [2, 4], 
    [1, 3],
    [2, 2],
    [3, 1],
    [1, 1],

    [7, 3],
    [8, 2],
    [6, 4],
    [7, 4],
    [8, 1],
    [9, 2],

    [10, 8],
    [9, 10],
    [7, 8],
    [7, 9],
    [8, 11],
    [9, 9],
];
~~~~

Next, we define two functions that are helpful to us, but not essential.
Given a list of points, I’d like to know what the max and min values for
each dimension are, and what the range of each dimension is. I want to
know “X ranges from 1 to 11, and Y ranges from 3 to 7″. Knowing these
figures helps us draw the graph on the canvas, and also helps when we
initialize our random cluster centers (we’d like them to be within the
range of the data points when we start them out).

Keeping in mind that we’re writing this to be generic with regards to
the number of dimensions they can handle:

~~~~ {.code}
function getDataRanges(extremes) {
    var ranges = [];

    for (var dimension in extremes)
    {
        ranges[dimension] = extremes[dimension].max - extremes[dimension].min;
    }

    return ranges;

}

function getDataExtremes(points) {
    
    var extremes = [];

    for (var i in data)
    {
        var point = data[i];

        for (var dimension in point)
        {
            if ( ! extremes[dimension] )
            {
                extremes[dimension] = {min: 1000, max: 0};
            }

            if (point[dimension] < extremes[dimension].min)
            {
                extremes[dimension].min = point[dimension];
            }

            if (point[dimension] > extremes[dimension].max)
            {
                extremes[dimension].max = point[dimension];
            }
        }
    }

    return extremes;
}
~~~~

The `getDataExtremes()` method loops through all the points and each
dimension in each point and finds the min and max values (note there’s a
hard-coded “1000″ in there, which you should change if you’re using
large numbers). The `getDataRanges()` function is just a helper that
takes that output and returns the range of each dimension (the maximum
value minus the minimum value).

Next up, we define a function that initializes *k* random cluster
centroids:

~~~~ {.code}
function initMeans(k) {

    if ( ! k )
    {
        k = 3;
    }

    while (k--)
    {
        var mean = [];

        for (var dimension in dataExtremes)
        {
            mean[dimension] = dataExtremes[dimension].min + ( Math.random() * dataRange[dimension] );
        }

        means.push(mean);
    }

    return means;

};
~~~~

We’re just creating new points with random coordinates within the range
and dimensions of our dataset.

Once we have our randomly seeded centroids, we need to enter our k-means
loop. As a reminder, the loop consists of first assigning all our data
points to the centroid closest to it, then moving the centroids to the
average position of all the data points assigned to it. We repeat that
until the centroids stop moving.

~~~~ {.code}
function makeAssignments() {

    for (var i in data)
    {
        var point = data[i];
        var distances = [];

        for (var j in means)
        {
            var mean = means[j];
            var sum = 0;

            for (var dimension in point)
            {
                var difference = point[dimension] - mean[dimension];
                difference *= difference;
                sum += difference;
            }

            distances[j] = Math.sqrt(sum);
        }

        assignments[i] = distances.indexOf( Math.min.apply(null, distances) );
    }

}
~~~~

The above function is called by our “loop” function and calculates the
[Euclidean distance](http://en.wikipedia.org/wiki/Euclidean_distance)
between each point and the cluster center.

Note that the above algorithm loops through each point and then loops
through each cluster centroid, making this an O(k\*n) algorithm. It’s
not terrible, but it might be computationally intensive if you have a
large number of data points or a large number of clusters or both. There
are ways you can optimize this, which we’ll perhaps discuss in a future
article. For one, we can try to eliminate the expensive `Math.sqrt()`
call; we could also try not to iterate through every point.

Once we have our list of assignments — in this case, just an associative
array of `point index => center index` — we can go ahead and update the
positions of the means (the cluster centers).

~~~~ {.code}
function moveMeans() {

    makeAssignments();

    var sums = Array( means.length );
    var counts = Array( means.length );
    var moved = false;

    for (var j in means)
    {
        counts[j] = 0;
        sums[j] = Array( means[j].length );
        for (var dimension in means[j])
        {
            sums[j][dimension] = 0;
        }
    }

    for (var point_index in assignments)
    {
        var mean_index = assignments[point_index];
        var point = data[point_index];
        var mean = means[mean_index];

        counts[mean_index]++;

        for (var dimension in mean)
        {
            sums[mean_index][dimension] += point[dimension];
        }
    }

    for (var mean_index in sums)
    {
        console.log(counts[mean_index]);
        if ( 0 === counts[mean_index] ) 
        {
            sums[mean_index] = means[mean_index];
            console.log("Mean with no points");
            console.log(sums[mean_index]);

            for (var dimension in dataExtremes)
            {
                sums[mean_index][dimension] = dataExtremes[dimension].min + ( Math.random() * dataRange[dimension] );
            }
            continue;
        }

        for (var dimension in sums[mean_index])
        {
            sums[mean_index][dimension] /= counts[mean_index];
        }
    }

    if (means.toString() !== sums.toString())
    {
        moved = true;
    }

    means = sums;

    return moved;

}
~~~~

The `moveMeans()` starts by calling the `makeAssignments()` function.
Once we have our assignments, we initialize two arrays: one called
“sums” and the other called “counts”. Since we’re calculating the
arithmetic mean (or average), we’ll need to know the sum of points’
dimensions as well as the number of points whose dimensions we’re
averaging.

We then hit three loops:

First we loop through our means and prepare our *sums* and *counts*
arrays. Our *sums* array will actually be multidimensional, because
we’re storing each dimension’s sum of each point of each mean in this
structure — so we have to zero-out the second-depth level of this 2
dimensional array.

Then we loop through our assignments and increment the *counts* counter
for each cluster center we have points assigned to, and additionally
loop through the point’s dimensions to fill in the *sums* array. At this
point we have all the data we need to calculate the new positions of the
cluster centers.

The final loop loops through our results, calculates the mean position
for each cluster center, and moves it. The final loop also checks to see
if a cluster center had *no* points assigned to it. If it didn’t have
any points assigned to it, we give it a new random position. This is
just us trying to kick that cluster center back into the solution.

Finally, we wrap up this function by checking to see if any one of our
cluster centers has moved — and we return either true or false.

To get this algorithm started, we run the following setup functions:

~~~~ {.code}
function setup() {

    canvas = document.getElementById('canvas');
    ctx = canvas.getContext('2d');

    dataExtremes = getDataExtremes(data);
    dataRange = getDataRanges(dataExtremes);
    means = initMeans(3);

    makeAssignments();
    draw();

    setTimeout(run, drawDelay);
}

function run() {

    var moved = moveMeans();
    draw();

    if (moved)
    {
        setTimeout(run, drawDelay);
    }

}
~~~~

`setup()` initializes everything we need, and then our `run()` function
checks to see if the algorithm has stopped, and loops based on a timer
so that we can watch the algorithm do its work in a reasonable
timeframe.

k-medians
---------

One major issue with the k-means algorithm isn’t a fault of the
algorithm’s, but rather of the concept of the arithmetic mean, or
average. The average is a pretty bad metric when you have outlying data.

If you work at a company where 5 people make $50,000 a year but one
person makes $1,000,000, the *median* salary is $50,000 (very
representative of salary at that company), but the *mean* salary is
$200,000 (not at all representative of salary at that company)!

This happens with all sorts of data, and can and will happen in the
k-means algorithm too. If you have a dataset prone to outliers, you’ll
find that k-means gets “stuck” on the outlier and ends up yielding poor
results. In that case, switch to k-medians! The algorithm is nearly the
same; instead of calculating the mean for your cluster centers, use the
median instead. I believe — but I’m not certain — that calculating the
median also has a performance advantage over the mean.

Results
-------

As you can see from the example below, k-means works very well for our
nice, neat data. Obviously it’ll have more difficulty with messy data,
like any other algorithm.

If you run the example below a number of times (click the play button on
the JSFiddle) you’ll eventually see it fall into a local optimum. This
should also demonstrate the usefulness of the “committee of machines”
solving method: while a bad solution does pop up from time to time, it
should be clear that a committee of machines will produce the correct
solution reliably.

Finally, if you like this series, please sign up for the mailing list
below and tell your friends! I also appreciate discussion, so feel free
to use the commenting tool below. And make sure to check out the other
[ML in JS
articles](http://burakkanber.com/blog/modeling-physics-in-javascript-introduction/ "Modeling Physics in Javascript: Introduction")!

Email me when new ML in JS articles are posted
----------------------------------------------

Email Address

**Survey: Would you buy an ML in JS e-book?**\
 I would pay $10 for a DRM-free e-book with tons of ML lessons and JS
examples.

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

[← Next post
|](http://burakkanber.com/blog/sitechat-a-postmortem-or-the-rise-and-fall-of-a-society/)
[Previous post
→](http://burakkanber.com/blog/physics-in-javascript-rigid-bodies-part-1-pendulum-clock/)

[RSS](http://burakkanber.com/blog/feed/ "Syndicate this site using RSS")
|
[Atom](http://burakkanber.com/blog/feed/atom/ "Syndicate this site using Atom")

![Clicky](//in.getclicky.com/100593568ns.gif)
