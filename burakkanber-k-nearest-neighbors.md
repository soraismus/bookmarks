[Burak Kanber, Engineer](/blog/)
================================

### The Blog

[Follow @bkanber](https://twitter.com/bkanber)

[Machine Learning in JS: k-nearest-neighbor Introduction](http://burakkanber.com/blog/machine-learning-in-js-k-nearest-neighbor-part-1/)
----------------------------------------------------------------------------------------------------------------------------------------

On September 7, 2012

*This article is part of the [Machine Learning in
Javascript](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/ "Machine Learning in Javascript: Introduction")
series. My goal is to teach ML from fundamental to advanced topics using
a common language. Javascript is an excellent choice because it requires
no special environment to run, and its lack of ML libraries forces us to
learn and write the code from scratch.*

Just looking for the example code? The [JSFiddle is at the bottom of the
page](#fiddle).

Today we’re going to look at the k-nearest-neighbor algorithm (we’ll
abbreviate it kNN throughout this article). I love this algorithm
because it’s dead simple, but can still solve some exciting problems.
Its strength doesn’t lie in mathematical or theoretical sophistication,
but rather in the fact that it can elegantly handle lots of different
input parameters (“dimensions”).

k-nearest-neighbor also serves an ulterior motive of mine: it’s a great
way to introduce the concept of “supervised learning”. So let’s go ahead
and build the k-nearest-neighbor algorithm. And we’ll graph our results,
because I love visuals!

Supervised Learning
-------------------

There are two giant umbrella categories within Machine Learning:
supervised and unsupervised learning. Briefly, *un*supervised learning
is like data mining — look through some data and see what you can pull
out of it. Typically, you don’t have much additional information before
you start the process. We’ll be looking at an unsupervised learning
algorithm called k-means next week, so we can discuss that more in the
next article.

Supervised learning, on the other hand, starts with “training data”.
Supervised learning is what we do as children as we learn about the
world. We’re in the kitchen with mom. Mom shows us an apple and says the
word “apple”. You see an object and your mother has labeled it for you.

The next day, she shows you a different apple. It’s smaller, it’s less
red, and it has a slightly different shape. But your mother shows it to
you and says the word “apple”. This process repeats for a couple of
weeks. Every day your mother shows you a slightly different apple, and
tells you they’re apples. Through this process you come to understand
what an apple is.

Not every apple is exactly the same. Had you only ever seen one apple in
your life, you might assume that every apple is *identical*. But
instead, your mother has trained you to recognize the overall features
of an apple. You’re now able to create this category, or label, of
objects in your mind. When you see an apple in the future you can
recognize it as an apple because you’ve come to understand that while
all apples share some features, they don’t have to be identical to still
be apples.

This is called “generalization” and is a very important concept in
supervised learning algorithms. We would be useless if we couldn’t
recognize that an iPhone was an iPhone because it had a different case,
or because it had a scratch across the screen.

When we build certain types of ML algorithms we therefore need to be
aware of this idea of generalization. Our algorithms should be able to
generalize but not *over*-generalize (easier said than done!). We want
“this is red and kind of round and waxy, it must be an apple” and
*not*“this is red and round, it must be a ball; this other thing is
orange and round, it must be a ball.” That’s overgeneralization and can
be a problem. Of course, under-generalization is a problem too. This is
one of the main difficulties with ML: being able to find the
generalization sweet spot. There are some tests you can run as you’re
training an algorithm to help you find the sweet spot, but we’ll talk
about those in the future when we get to more advanced algorithms.

Many supervised learning problems are “classification” problems. The
classification problem goes like this: there’s a bucket of apples,
oranges, and pears. Each piece of fruit has a sticker that tells you
which fruit it is — except one! Figure out which fruit the mystery fruit
is by learning from the other fruits you’re given.

The classification problem is usually very easy for humans, but tough
for computers. kNN is one type of many different classification
algorithms.

Features
--------

This is a great time to introduce another important aspect of ML:
features. Features are what you break an object down into as you’re
processing it for ML. If you’re looking to determine whether an object
is an apple or an orange, for instance, you may want to look at the
following features: shape, size, color, waxiness, surface texture, etc.
It also turns out that sometimes an individual feature ends up not being
helpful. Apples and oranges are roughly the same size, so that feature
can probably be ignored and save you some computational overhead. In
this case, size doesn’t really add new information to the mix.

Knowing what features to look for is an important skill when designing
for ML algorithms. Sometimes you can get by on intuition, but most of
the time you’ll want to use a separate algorithm to determine which are
the most important features of a data set (we’ll talk about that in a
much more advanced article).

As you can imagine, features aren’t always as straightforward as “color,
size, shape”. Processing documents is a tricky example. In some
scenarios, each *word* in a document is an individual feature. Or maybe
each pair of consecutive words (“bigrams”) is a feature. We’ll talk
about document classification in a future article as well.

The Problem
-----------

Given the number of rooms and area (in square feet) of a type of
dwelling, figure out if it’s an apartment, house, or flat.

As always, we’re starting with the most contrived possible problem in
order to learn the basics. The description of this problem has *given*us
the features we need to look at: number of rooms, and square feet. We
can also assume that, since this is a supervised learning problem, we’ll
be given a handful of example apartments, houses, and flats.

What “k-nearest-neighbor” Means
-------------------------------

I think the best way to teach the kNN algorithm is to simply define what
the phrase “k-nearest-neighbor” actually means.

Here’s a table of the example data we’re given for this problem:

Rooms

Area

Type

1

350

apartment

2

300

apartment

3

300

apartment

4

250

apartment

4

500

apartment

4

400

apartment

5

450

apartment

7

850

house

7

900

house

7

1200

house

8

1500

house

9

1300

house

8

1240

house

10

1700

house

9

1000

house

1

800

flat

3

900

flat

2

700

flat

1

900

flat

2

1150

flat

1

1000

flat

2

1200

flat

1

1300

flat

We’re going to plot the above as points on a graph in two dimensions,
using number of rooms as the x-axis and the area as the y-axis.

When we inevitably run into a new, unlabeled data point (“mystery
point”), we’ll put that on the graph too. Then we’ll pick a number
(called “k”) and just find the “k” closest points on the graph to our
mystery point. If the majority of the points close to the new point are
“flats”, then we’ll guess that our mystery point is a flat.

That’s what k-nearest-neighbor means. “If the 3 (or 5 or 10, or ‘k’)
nearest neighbors to the mystery point are two apartments and one house,
then the mystery point is an apartment.”

Here’s the (simplified) procedure:

-   Put all the data you have (including the mystery point) on a graph.
-   Measure the distances between the mystery point and every other
    point.
-   Pick a number. Three is usually good for small data sets.
-   Figure out what the three closest points to the mystery point are.
-   The majority of the three closest points is the answer.

If you’re having trouble visualizing this, please take a quick break to
scroll down to the bottom of the page and run the JS fiddle. That should
illustrate the concept. Then come back up here and continue reading!

The Code
--------

Let’s start building this thing. There are a few fine points that will
come out as we’re implementing the algorithm, so please read the
following carefully. If you skim, you’ll miss out on another important
concept!

I’ll build objects of two different classes for this algorithm: the
“Node”, and a “NodeList”. A Node represents a single data point from the
set, whether it’s a pre-labeled (training) point, or an unknown point.
The NodeList manages all the nodes and also does some canvas drawing to
graph them all.

The Node constructor has nothing to it. It just expects an object with
the properties “type”, “area”, and “rooms”:

~~~~ {.code}
var Node = function(object) {
    for (var key in object)
    {
        this[key] = object[key];
    }
};
~~~~

When I build these algorithms “for realsies” I usually abstract the
features a little bit more. This example requires “area” and “rooms” to
be hard-coded in certain places, but I usually build a generalized kNN
algorithm that can work with arbitrary features rather than our
pre-defined ones here. I’ll leave that as an exercise for you!

Similarly, the NodeList constructor is simple:

~~~~ {.code}
var NodeList = function(k) {
    this.nodes = [];
    this.k = k;
};
~~~~

The NodeList constructor takes the “k” from k-nearest-neighbor as its
sole argument. Please fork the JSFiddle code and experiment with
different values of k. Don’t forget to try the number 1 as well!

Not shown is a simple NodeList.prototype.add(node) function — that just
takes a node and pushes it onto the this.nodes array.

At this point, we could dive right in to calculating distances, but I’d
like to take a quick diversion.

Normalizing Features
--------------------

Look at the data in the table above. The number of rooms varies from 1
to 10, and the area ranges from 250 to 1700. What would happen if we
tried to graph this data onto a chart (without scaling anything)? For
the most part, the data points would be lined up in a vertical column.
That’ll look pretty ugly, and hard to read.

Unfortunately, that’s not just an aesthetic problem. This is an issue of
a large discrepancy of scale of our data features. The difference
between 1 room and 10 rooms is *huge* when you consider what it means
for classifying a “flat” vs a “house”! But the same difference of 9,
when you’re talking about square feet, is *nothing*. If you were to
measure distances between Nodes right now without adjusting for that
discrepancy, you’d find that the number of rooms would have almost no
effect on the results because those points are so close together on the
x-axis (the “rooms” axis).

Consider the difference between a dwelling with 1 room and 700 square
feet and 5 rooms and 700 square feet. Looking at the data table by eye,
you’d recognize the first to be a flat and the second to be an
apartment. But if you were to graph these and run kNN, it would consider
them both to be flats.

So instead of looking at absolute values of number of rooms and area, we
should normalize these values to be between 0 and 1. After
normalization, the lowest number of rooms (1) becomes 0, and the largest
number of rooms (10) becomes 1. Similarly, the smallest area (250)
becomes 0 and the largest area (1700) becomes 1. That puts everything on
the same playing field and will adjust for discrepancies of scale. It’s
a simple thing to do that makes all the difference in the world.

> Pro-tip: you don’t need to scale things evenly (into a square) like I
> described above. If area is more important to the problem than the
> number of rooms, you can scale those two features differently — this
> is called “weighting”, and gives more importance to one feature or
> another. There are also algorithms that will determine the ideal
> feature weights for you. All in due time…

To start normalizing our data we should give NodeList a way of finding
the minimum and maximum values of each feature:

~~~~ {.code}
NodeList.prototype.calculateRanges = function() {
    this.areas = {min: 1000000, max: 0};
    this.rooms = {min: 1000000, max: 0};
    for (var i in this.nodes)
    {
        if (this.nodes[i].rooms < this.rooms.min)
        {
            this.rooms.min = this.nodes[i].rooms;
        }

        if (this.nodes[i].rooms > this.rooms.max)
        {
            this.rooms.max = this.nodes[i].rooms;
        }

        if (this.nodes[i].area < this.areas.min)
        {
            this.areas.min = this.nodes[i].area;
        }

        if (this.nodes[i].area > this.areas.max)
        {
            this.areas.max = this.nodes[i].area;
        }
    }

};
~~~~

As I mentioned earlier, the best approach would be to abstract the
features and not have areas or rooms hard-coded. But doing it this way
reads a little more clearly to me.

Now that we have our minimum and maximum values, we can move along to
the meat and berries of the algorithm. After we’ve added all our Nodes
to the NodeList:

~~~~ {.code}
NodeList.prototype.determineUnknown = function() {

    this.calculateRanges();

    /*
     * Loop through our nodes and look for unknown types.
     */
    for (var i in this.nodes)
    {

        if ( ! this.nodes[i].type)
        {
            /*
             * If the node is an unknown type, clone the nodes list and then measure distances.
             */
            
            /* Clone nodes */
            this.nodes[i].neighbors = [];
            for (var j in this.nodes)
            {
                if ( ! this.nodes[j].type)
                    continue;
                this.nodes[i].neighbors.push( new Node(this.nodes[j]) );
            }

            /* Measure distances */
            this.nodes[i].measureDistances(this.areas, this.rooms);

            /* Sort by distance */
            this.nodes[i].sortByDistance();

            /* Guess type */
            console.log(this.nodes[i].guessType(this.k));

        }
    }
};
~~~~

That’s a mouthful. First off, we calculate the min and max ranges so
that the NodeList is aware of it.

We then loop through the Nodes and look for any unknown nodes (yes, this
can do more than one mystery node at a time).

When we find an unknown Node, we clone the Nodes in the NodeList and
make the Node aware of them. The reason we do this is because each
unknown Node will have to calculate its own distance to every other Node
— so we can’t use global state here.

Finally, we call three Node methods in succession on the unknown Node:
measureDistances, sortByDistance, and guessType.

~~~~ {.code}
Node.prototype.measureDistances = function(area_range_obj, rooms_range_obj) {
    var rooms_range = rooms_range_obj.max - rooms_range_obj.min;
    var area_range  = area_range_obj.max  - area_range_obj.min;

    for (var i in this.neighbors)
    {
        /* Just shortcut syntax */
        var neighbor = this.neighbors[i];

        var delta_rooms = neighbor.rooms - this.rooms;
        delta_rooms = (delta_rooms ) / rooms_range;

        var delta_area  = neighbor.area  - this.area;
        delta_area = (delta_area ) / area_range;

        neighbor.distance = Math.sqrt( delta_rooms*delta_rooms + delta_area*delta_area );
    }
};
~~~~

The measureDistances function takes the NodeList’s set of ranges for min
and max rooms and areas. If you’ve abstracted away from hard-coding
features, the argument to this method would just be an array of ranges
for features, but here we’ve hardcoded it.

We quickly calculate the rooms\_range (here, it’ll be 9) and area\_range
(here, it’ll be 1450).

Then we loop through our Node’s neighbors (we gave this Node the
neighbors property above when we cloned the Nodes from NodeList). For
each one of the neighbors we calculate the difference in number of rooms
and area. After each of those calculations, we normalize by dividing
each feature by its range.

If the difference between the number of rooms turns out to be 3 and the
total range is 9 we end up for 0.333 for the value of that delta. This
number should always be between -1 and +1, since we’ve normalized to a
square for this problem.

Finally, we calculate the distance using the Pythagorean theorem. Note
that if you have more than 2 features (dimensions), you still keep the
Math.sqrt — you just add all the squared features like so:

~~~~ {.code}
Math.sqrt( a*a + b*b + c*c + d*d + ... + z*z );
~~~~

It should be pretty clear now what the true strength of this algorithm
is. While our feeble minds may only be able to figure out correlations
involving 5 or 10 different features, this algorithm can do that for
hundreds or thousands of dimensions.

~~~~ {.code}
Node.prototype.sortByDistance = function() {
    this.neighbors.sort(function (a, b) {
        return a.distance - b.distance;
    });
};
~~~~

This above sortByDistance is just a helper function to sort the Node’s
neighbors by distance.

~~~~ {.code}
Node.prototype.guessType = function(k) {
    var types = {};

    for (var i in this.neighbors.slice(0, k))
    {
        var neighbor = this.neighbors[i];

        if ( ! types[neighbor.type] )
        {
            types[neighbor.type] = 0;
        }

        types[neighbor.type] += 1;
    }

    var guess = {type: false, count: 0};
    for (var type in types)
    {
        if (types[type] > guess.count)
        {
            guess.type = type;
            guess.count = types[type];
        }
    }

    this.guess = guess;

    return types;
};
~~~~

The final piece of the algorithm is the guessType method. This method
accepts the value of “k” from NodeList and slices out the k closest
neighbors. It then tallies up the types of the neighbors, picks the most
common one, and returns the result.

Algorithm finished. Congratulations! Now let’s figure out how to graph
this thing.

Graphing it With Canvas
-----------------------

Graphing our results is pretty straightforward, with a few exceptions:

-   We’ll color code apartments = red, houses = green, flats = blue.
-   We have to scale the graph into a square (for the same reasons we
    had to normalize the features).
-   We’ll also add a little bit of padding to the edges of the graph.
-   We’ll display the result of the kNN guess by drawing the radius that
    encompasses the “k” nearest neighbors, and coloring that radius with
    the result’s color.

~~~~ {.code}
NodeList.prototype.draw = function(canvas_id) {
    var rooms_range = this.rooms.max - this.rooms.min;
    var areas_range = this.areas.max - this.areas.min;

    var canvas = document.getElementById(canvas_id);
    var ctx = canvas.getContext("2d");
    var width = 400;
    var height = 400;
    ctx.clearRect(0,0,width, height);

    for (var i in this.nodes)
    {
        ctx.save();

        switch (this.nodes[i].type)
        {
            case 'apartment':
                ctx.fillStyle = 'red';
                break;
            case 'house':
                ctx.fillStyle = 'green';
                break;
            case 'flat':
                ctx.fillStyle = 'blue';
                break;
            default:
                ctx.fillStyle = '#666666';
        }

        var padding = 40;
        var x_shift_pct = (width  - padding) / width;
        var y_shift_pct = (height - padding) / height;

        var x = (this.nodes[i].rooms - this.rooms.min) * (width  / rooms_range) * x_shift_pct + (padding / 2);
        var y = (this.nodes[i].area  - this.areas.min) * (height / areas_range) * y_shift_pct + (padding / 2);
        y = Math.abs(y - height);


        ctx.translate(x, y);
        ctx.beginPath();
        ctx.arc(0, 0, 5, 0, Math.PI*2, true);
        ctx.fill();
        ctx.closePath();
        

        /* 
         * Is this an unknown node? If so, draw the radius of influence
         */

        if ( ! this.nodes[i].type )
        {
            switch (this.nodes[i].guess.type)
            {
                case 'apartment':
                    ctx.strokeStyle = 'red';
                    break;
                case 'house':
                    ctx.strokeStyle = 'green';
                    break;
                case 'flat':
                    ctx.strokeStyle = 'blue';
                    break;
                default:
                    ctx.strokeStyle = '#666666';
            }

            var radius = this.nodes[i].neighbors[this.k - 1].distance * width;
            radius *= x_shift_pct;
            ctx.beginPath();
            ctx.arc(0, 0, radius, 0, Math.PI*2, true);
            ctx.stroke();
            ctx.closePath();

        }

        ctx.restore();

    }

};
~~~~

I won’t describe the above in detail, but rather point out two lines
that I want you to look at and figure out on your own:

-   The line starting “var x = ”
-   The line starting “var radius = ”

It’s a good but potentially frustrating exercise to try and figure out
what’s going on in those two lines, but please don’t give up until you
understand them!

Finally, we’ll throw some code together to create random mystery points
and do that every 5 seconds:

~~~~ {.code}
var run = function() {
    nodes = new NodeList(3);
    for (var i in data)
    {
        nodes.add( new Node(data[i]) );
    }
    var random_rooms = Math.round( Math.random() * 10 );
    var random_area = Math.round( Math.random() * 2000 );
    nodes.add( new Node({rooms: random_rooms, area: random_area, type: false}) );

    nodes.determineUnknown();
    nodes.draw("canvas");
};

window.onload = function() {
    setInterval(run, 5000);
    run();
};
~~~~

If you want to play with the value of “k”, do it in the run() function
above.

The kNN algorithm isn’t the most sophisticated classifier there is, but
it has some excellent uses. What’s even more exciting is that you don’t
*have* to use kNN as a classifier; the concepts behind kNN are very
flexible and can be used for non-classification problems. Consider the
following problems that kNN might be a good candidate for (list includes
both classification problems and otherwise):

-   What named color (blue, red, green, yellow, grey, purple, etc) is a
    given RGB value closest to? This is useful if you have an image
    search application and want to “search for purple images” for
    instance.
-   You’re building a dating site and want to rank matches for a given
    profile. The features could be location, age, height, and weight,
    for instance. Pick the 20 closest neighbors and rank them by kNN
    distance.
-   Quickly find 5 documents similar to a given document. Features are
    words in this case. Not the most sophisticated algorithm to solve
    this problem, but it works in a pinch.
-   Given data for previous e-shoppers, figure out if the person
    currently browsing your site is likely to make a purchase. Features
    could be time of day, number of pages browsed, location, referral
    source, etc.

Drawbacks, caveats
------------------

There are two issues with kNN I’d like to briefly point out. First of
all, it should be pretty clear that if your training data is all over
the place, this algorithm won’t work well. The data needs to be
“separable”, or clustered somehow. Random speckles on the graph is no
help, and very few ML algorithms can discern patterns from nearly-random
data.

Secondly, you’ll run into performance problems if you have thousands and
thousands of Nodes. Calculating all those distances adds up! One way
around this is to pre-filter out Nodes outside of a certain feature’s
range. For example, if our mystery point has \# of rooms = 3, we might
not even calculate distances for points with \# of rooms \> 6 at all.

Here’s the JSFiddle. The known points are colored, the mystery point is
grey, and the result of the kNN guess is signified by the colored radius
around the mystery point. The radius encapsulates the 3 nearest
neighbors. A new random mystery point is solved for every 5 seconds:

Email me when new ML in JS articles are posted
----------------------------------------------

Email Address

**Survey: Would you buy an ML in JS e-book?**\
 I would pay $10 for a DRM-free e-book with tons of ML lessons and JS
examples.

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

[← Next post
|](http://burakkanber.com/blog/effective-teaching-is-a-long-con/)
[Previous post
→](http://burakkanber.com/blog/modeling-physics-javascript-gravity-and-drag/)

[RSS](http://burakkanber.com/blog/feed/ "Syndicate this site using RSS")
|
[Atom](http://burakkanber.com/blog/feed/atom/ "Syndicate this site using Atom")

![Clicky](//in.getclicky.com/100593568ns.gif)
