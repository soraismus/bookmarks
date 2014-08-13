[Burak Kanber, Engineer](/blog/)
================================

### The Blog

[Follow @bkanber](https://twitter.com/bkanber)

[Machine Learning: Genetic Algorithms in Javascript Part 2](http://burakkanber.com/blog/machine-learning-genetic-algorithms-in-javascript-part-2/)
--------------------------------------------------------------------------------------------------------------------------------------------------

On September 15, 2012

Today we’re going to revisit the genetic algorithm. If you haven’t read
[Genetic Algorithms Part
1](http://burakkanber.com/blog/machine-learning-genetic-algorithms-part-1-javascript/ "Machine Learning: Genetic Algorithms Part 1 (Javascript)")
yet, I strongly recommend reading that now. This article will skip over
the fundamental concepts covered in part 1 — so if you’re new to genetic
algorithms you’ll definitely want to start there.

Just looking for [the example?](#fiddle)

The Problem
-----------

You’re a scientist that has recently been framed for murder by an evil
company. Before you flee the lab you have an opportunity to steal 1,000
pounds (or kilograms!) of pure elements from the chemical warehouse;
your plan is to later sell them and survive off of the earnings.

> Given the weight and value of each element, which combination should
> you take to maximize the total value without exceeding the weight
> limit?

This is called [the knapsack
problem](http://en.wikipedia.org/wiki/Knapsack_problem). The one above
is a one-dimensional problem, meaning the only constraint is weight. We
could complicate matters by also considering volume, but we need to
start somewhere. Note that in our version of the problem only one piece
of each element is available, and each piece has a fixed weight. There
are some knapsack problems where you can take unlimited platinum or up
to 3 pieces of gold or something like that, but here we only have one of
each available to us.

Why is this problem tough to solve? We’ll be using 118 elements. The
brute-force approach would require that we test 2^118^ or 3.3 \* 10^35^
different combinations of elements.

Greedy Algorithm
----------------

A quick benchmark we’ll use for our solution is called the “greedy”
solution. The greedy algorithm grabs the most valued items and puts them
into the knapsack until it can’t fit any more.

Sometimes this works great. Sometimes it doesn’t. Imagine that there’s a
piece of gold in the warehouse that’s valued at $1,000 but weighs 600
pounds. And there’s also a piece of cadmium that has a value of $950 but
only weighs 300 pounds, and there are a bunch of other elements that
have a pretty high value but reasonably light weights. The greedy
algorithm still tosses gold in there, and all that precious available
weight is taken up by something that’s not really worth it.

The “naive” greedy algorithm for our dataset will give us a total value
of $3649 with a total weight of 998 pounds.

You may at this point be thinking “why don’t we just figure out value
*per pound* for each element and use that?” Sure, that works too! It
will, in fact, work way better than the above.

Using that approach, the “weighted” greedy algorithm gives us a total
value of $4901 and a total weight of 969.

So those are our numbers to beat: we should expect to handily beat
$3,649, and we’ll be happy if we also beat $4,901.

Why does the greedy algorithm work well for this type of problem?
Because the greedy algorithm is solving for the “highest value per unit
weight” and that’s very close to what we want to fill our knapsack with.
However, the greedy algorithm will not perform well in instances where
there’s a large range of weights and values. That is, the greedy
algorithm will perform better if the range of values (and/or weights) of
the elements is between $1 – $100, but will perform worse if the range
is $1 – $500.

> Respond in the comments: why would the greedy algorithm perform worse
> with a larger range of weights and values?

Our GA might lose to the greedy algorithm from time to time, but that’s
ok. The GA will continue to perform well as complexity increases, but
the greedy algorithm will not.

So we’re still tackling a pretty simplified and contrived problem here,
but it’s certainly more complex and useful than our “Hello, World!” from
the last article. Let’s get started.

Key Differences From “Hello World”
----------------------------------

There are some major differences between our problem and the previous
“Hello, World!”:

-   We can’t use elements (genes) more than once, while “Hello, World!”
    can (must!) use letters more than once.
-   In the “Hello, World!” example, we knew that the string needed to be
    13 characters long. Here we don’t know how many elements to take.
-   We don’t know the highest possible value (“fitness” or “score”) for
    this problem. It could be $4,901 like the greedy algorithm guessed,
    or it could be $10,000, or $23,304

Representing the Chromosome
---------------------------

The “Hello, World!” algorithm represented the chromosome as a string.
Mutating involved randomly changing a letter. Mating (or crossover, as
it’s really called) involved splicing two strings together at the
halfway point. We need to do things a little differently here.

It turns out that representing our solution requires a little more
finesse than “Hello, World!”. Since we don’t know how many elements to
choose, we can’t use a fixed length string. (Just the first of our
problems!)

Instead, I propose we use a “bitmask” of sorts. We don’t have to use an
actual bitmask, but my proposal is to use a representation of *all* of
the available elements, and set each “present” or not.

Our chromosome could look like this:

~~~~ {.code}
Helium: present
Hydrogen: not present
Lithium: not present
~~~~

And et cetera for all 118 elements. Or if you want to go the bitmask
route:

~~~~ {.code}
10000011000001000100000010000010010010000
~~~~

Where each bit represents a single element and the value of the bit
indicates whether that element is in the knapsack or not.

Additionally, if we were to allow more than one of each element, the
representation could look like so:

~~~~ {.code}
Helium: 0
Hydrogen: 4
Lithium: 2
~~~~

What would *not* work well is the following:

~~~~ {.code}
In Knapsack: Helium, Lithium, Lead, Tin
~~~~

The above makes mating more difficult. You can make it work, but it
feels like you’d be jumping through hoops to pull it off. Structure
feels better for this problem.

We have a specific difficulty with mating: we need to make sure that
even after mating and mutation we still only have at most one of each
element in the list. Using the bitmask approach will help us to that
end, but that’s a common pitfall when trying the list approach above.

> The difficulty of making sure something happens only once in a
> chromosome is a common one. If you’re familiar with the traveling
> salesman problem, it’s easy to imagine a scenario where you mate two
> solutions and end up visiting the same city twice. That city appeared
> in the first half of the first parent and the second half of the
> second parent — therefore appearing nowhere in one child but twice in
> the other child.

Overweight Populations
----------------------

For this problem we’re going to keep track of three properties of the
population: weight, value, and *score*.

Score is the same thing as value, with one difference: score accounts
for the population being overweight.

You may be tempted to throw out overweight populations completely. It’s
a natural instinct because overweight solutions are not acceptable
solutions! But there’s a good, practical reason we don’t want to throw
out overweight chromosomes: there will be some slightly overweight
(1,001 pounds) chromosomes that have very high values and just need to
be “tweaked” a little to bring them within the weight range.

There could be a lot of potential in some of the overweight chromosomes.
Rather than killing them, we’ll penalize them just enough that they
still get to reproduce, but are unlikely to be the \#1 pick. That’s what
we’ll use “score” for. If you’re underweight, then your score is just
your total value. If you’re overweight, however, we’ll penalize you 50
points for every pound over weight. Feel free to play with this number.

Evolutionarily, this “encourages” promising chromosomes to drop some
weight. All they need is a little tweaking. There’s no use in throwing
out a potentially strong candidate!

We’ll cover the specifics of mating and mutation and the important of
death (really called “elitism”) as we’re looking at the code.

The Code
--------

First, let’s take a look at the data set. I wrote a simple PHP script
that generates random weights and values (ranged 1 – 500) for each
element and outputs the set as JSON. It looks something like this:

~~~~ {.code}
"Hydrogen":{
    "weight":389,
    "value":400},
"Helium":{
    "weight":309,
    "value":380},
"Lithium":{
    "weight":339,
    "value":424},
"Beryllium":{
    "weight":405,
    "value":387},
"Boron":{
    "weight":12,
    "value":174},
~~~~

And so on.

We then define three quick and easy helper functions:

~~~~ {.code}
function length(obj) {
    var length = 0;
    for (var i in obj)
        length++;
    return length;
}

function clone(obj) {
    obj = JSON.parse(JSON.stringify(obj));
    return obj;
}

function pickRandomProperty(obj) {
    var result;
    var count = 0;
    for (var prop in obj)
        if (Math.random() < 1 / ++count)
           result = prop;
    return result;
}
~~~~

The ‘length’ property only exists for javascript arrays, so we create a
length() function that works for objects.

We create a clone function that ensures our element objects aren’t
passed by reference.

Finally, we create a function that picks a random property of an object.
This is an analog to PHP’s ‘array\_rand’ function, which returns a
random array key.

Chromosome Functions
--------------------

~~~~ {.code}
var Chromosome = function(members) {
    this.members = members;
    for (var element in this.members)
    {
        if (typeof this.members[element]['active'] == 'undefined')
        {
            this.members[element]['active'] = Math.round( Math.random() );
        }
    }
    this.mutate();
    this.calcScore();
};

Chromosome.prototype.weight = 0;
Chromosome.prototype.value = 0;
Chromosome.prototype.members = [];
Chromosome.prototype.maxWeight = 1000;
Chromosome.prototype.mutationRate = 0.7;
Chromosome.prototype.score = 0;
~~~~

The chromosome constructor takes an object of ‘members’. In this case,
we’ll either be passing our original list of elements data when we’re
creating a brand new chromosome, *or* we’ll be passing in the results of
a mating operation.

The constructor randomly activates elements if the ‘active’ property
isn’t yet defined. The end result is that this will create a random
chromosome if we’re creating one from scratch, and it’ll leave a
pre-configured chromosome alone.

The prototype also specifies some defaults. The mutationRate property is
the chance that a chromosome will mutate.

~~~~ {.code}
Chromosome.prototype.mutate = function() {
    if (Math.random() > this.mutationRate)
        return false;
    var element = pickRandomProperty(this.members);
    this.members[element]['active'] = Number(! this.members[element]['active']);
};
~~~~

The mutate method is most similar to the “Hello, World!” example. If the
chromosome is to mutate then we simply pick an element at random and
toggle its ‘active’ property. I cast to Number here. It would have been
more semantic to cast the Math.random() in the constructor to Boolean.
I’ll ignore this, as I’ve already pasted all the code into this post.

~~~~ {.code}
Chromosome.prototype.calcScore = function() {
    if (this.score)
        return this.score;

    this.value = 0;
    this.weight = 0;
    this.score = 0;

    for (var element in this.members)
    {
        if (this.members[element]['active'])
        {
            this.value += this.members[element]['value'];
            this.weight += this.members[element]['weight'];
        }
    }

    this.score = this.value;

    if (this.weight > this.maxWeight)
    {
        this.score -= (this.weight - this.maxWeight) * 50;
    }

    return this.score;
};
~~~~

The calcScore method starts with a tiny performance optimization: if
we’ve calculated the score already, just serve the cached score — it’s
just a nice way to not have to worry about at which point in the
chromosome life cycle to calculate the score.

We then look through the elements and add up the value and weights for
the active ones. We then apply a penalty of 50 points per overweight
pound.

~~~~ {.code}
Chromosome.prototype.mateWith = function(other) {
    var child1 = {};
    var child2 = {};
    var pivot = Math.round( Math.random() * (length(this.members) - 1) );
    var i = 0;
    for (var element in elements)
    {
        if (i < pivot)
        {
            child1[element] = clone(this.members[element]);
            child2[element] = clone(other.members[element]);
        }
        else
        {
            child2[element] = clone(this.members[element]);
            child1[element] = clone(other.members[element]);
        }
        i++;
    }

    child1 = new Chromosome(child1);
    child2 = new Chromosome(child2);

    return [child1, child2];
};
~~~~

In the “Hello, World!” example we picked the center point as the pivot
point when mating two chromosomes; in this example we pick a random
point instead.\
 This adds a little more randomness to the system and can help avoid
local optima.

Once we’ve picked our pivot point we create two children by splicing the
parents at the pivot and combining. We then use our chromosome
constructor to generate chromosome objects and return them.

The Population
--------------

~~~~ {.code}
var Population = function(elements, size)
{
    if ( ! size )
        size = 20;
    this.elements = elements;
    this.size = size;
    this.fill();
};

Population.prototype.elitism = 0.2;
Population.prototype.chromosomes = [];
Population.prototype.size = 100;
Population.prototype.elements = false;
~~~~

The population constructor is straightforward: we give it the master
list of elements and the desired population size. We also define the
‘elitism’ parameter; this is the percentage of chromosomes that will
survive from one generation to the next.

~~~~ {.code}
Population.prototype.fill = function() {
    while (this.chromosomes.length < this.size)
    {
        if (this.chromosomes.length < this.size / 3)
        {
            this.chromosomes.push( new Chromosome( clone(this.elements) ) );
        }
        else
        {
            this.mate();
        }
    }
};
~~~~

We use the fill method to initialize the population; we’ll also use it
to fill the population after killing the weakest chromosomes. A little
bit of logic determines whether we should create random chromosomes or
fill the population through mating instead. If our population size is
20, the first 6 chromosomes will be random and the remaining will be
generated by mating. If the population size ever dips below 30% (perhaps
due to death/elitism), new random chromosomes will be created until the
population is diverse enough to create babies through mating.

Yes, ‘this.chromosomes.length’ in a while loop is bad form. If you
expect to use a large population size — or want this to be highly
optimized — do this the right way and cache the length.

~~~~ {.code}
Population.prototype.sort = function() {
    this.chromosomes.sort(function(a, b) { return b.calcScore() - a.calcScore(); });
};

Population.prototype.kill = function() {
    var target = Math.floor( this.elitism * this.chromosomes.length );
    while (this.chromosomes.length > target)
    {
        this.chromosomes.pop();
    }
};
~~~~

The sort function above is just a helper; note that we use the calcScore
method instead of accessing the ‘score’ property directly. If the score
hasn’t been calculated by this point, it will be now; if the score was
already calculated we just use calcScore as an accessor.

After sorting, the kill method removes the weakest chromosomes from the
bottom of the list by popping them until we reach our elitism value.

~~~~ {.code}
Population.prototype.mate = function() {
    var key1 = pickRandomProperty(this.chromosomes);
    var key2 = key1;

    while (key2 == key1)
    {
        key2 = pickRandomProperty(this.chromosomes);
    }

    var children = this.chromosomes[key1].mateWith(this.chromosomes[key2]);
    this.chromosomes = this.chromosomes.concat(children);
};
~~~~

The mate method is always called after the kill method, so the only
chromosomes allowed to reproduce are the elite ones in the population
(in our example, the best 20%). Rather than mating only the best two
chromosomes (like we did in “Hello, World!”), we pick any two random
chromosomes to mate — with the exception that we won’t mate a chromosome
with itself.

Again, this serves to add a little more randomness to the system, and
will avoid stasis if the top two chromosomes remain the same for many
generations — we see that happen sometimes in the “Hello, World!”
example and fix it here.

~~~~ {.code}
Population.prototype.generation = function(log) {
    this.sort();
    this.kill();
    this.mate();
    this.fill();
    this.sort();
};
~~~~

Then we define a “generation”. The generation starts by sorting the
chromosomes in terms of score. We then kill the weakest members.

Then we do something a little intriguing: we call mate() once and then
call fill(), which we know will also call mate(). The reason we call
mate() explicitly is as a bit of insurance: if the elitism parameter is
less than 0.3 we want to mate at least once before potentially
“polluting” the population with random members. This really depends on
the elitism value; if you keep it over 0.3 you don’t have to call mate()
explicitly, because fill() will do that for you. But if you have elitism
= 0.2 like we do, then we want to run at least one mating routine that
involves only the elite, and not the new random chromosomes we introduce
with fill().

Finally, we sort once more at the end of the generation. We could just
as easily leave this part out, but it’s nice to see the chromosomes in
order after every generation if you’re debugging.

Running and Stopping
--------------------

I briefly introduced this idea in the “Hello, World!” example: we don’t
know the best possible score in this problem, therefore we won’t know
when to stop.

The technique (one of many) that we’ll use is to stop when you’ve had
100 (or 1,000 or 1,000,000) generations with no improvement. We’ll just
call this the “threshold” or the “stop threshold”.

~~~~ {.code}
Population.prototype.run = function(threshold, noImprovement, lastScore, i) {
    if ( ! threshold )
        threshold = 1000;
    if ( ! noImprovement )
        noImprovement = 0;
    if ( ! lastScore )
        lastScore = false;
    if ( ! i )
        i = 0;

    if (noImprovement < threshold)
    {
        lastScore = this.chromosomes[0].calcScore();
        this.generation();

        if (lastScore >= this.chromosomes[0].calcScore())
        {
            noImprovement++;
        }
        else
        {
            noImprovement = 0;
        }

        i++;

        if (i % 10 == 0)
            this.display(i, noImprovement);
        var scope = this;
        setTimeout(function() { scope.run(threshold, noImprovement, lastScore, i) }, 1);

        return false;

    }
    this.display(i, noImprovement);
};
~~~~

The run method is an iterative function. It doesn’t need to be. The only
reason it is in this example is because writing to the DOM in the middle
of a fast-moving loop doesn’t work — the DOM doesn’t update until
execution is done. [It’s just a javascript
thing.](http://stackoverflow.com/questions/8110905/javascript-a-loop-with-innerhtml-is-not-updating-during-loop-execution)

To get around the DOM limitation, we use a short setTimeout and have the
run method call itself iteratively until we’re done. In general,
however, this function could just use a while loop instead of calling
itself — but in that case you’d either need to use console.log or just
wait until the loop is done to watch the results.

Other than that DOM messiness, the run method is straightforward. We
compare last generation’s best score to this generation’s best. If there
was improvement, then we reset the ‘noImprovement’ counter. If we have
noImprovement equal to our stop threshold, we stop.

Not shown is a simple display method used to print the results to a
table on the page. We only call it every 10 generations, and then once
again when we’re done.

In Action
---------

Our JSFiddle is below. You’re given a slider that lets you set the stop
tolerance and a big “Go” button. You then get to watch the population’s
evolution.

Even with a 10-generation stop threshold, the GA consistently beats the
naive greedy algorithm, though this is to be expected.

At a 50-generation stop threshold, the GA also consistently beats the
weighted greedy algorithm. This is a happy result. There are some
datasets where the GA will never beat the greedy algorithm, and other
datasets where the greedy algorithm doesn’t perform well at all. This
seems like an in-between. We don’t *crush* the greedy algorithm, but we
do outperform it by a significant margin.

The best score I’ve observed so far is 5944, with a weight of 992.
Please let me know in the comments below if you find a better score, and
I’ll post the updates here.

*Update:*Sylvain Zimmer below found a solution with a score of 5968 and
weight 998.

As always, feel free to fork and play with this example. I would
encourage you to experiment with different values of population size,
elitism, and mutation rate and observe the results.

Email me when new ML in JS articles are posted
----------------------------------------------

Email Address

**Survey: Would you buy an ML in JS e-book?**\
 I would pay $10 for a DRM-free e-book with tons of ML lessons and JS
examples.

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

[← Next post
|](http://burakkanber.com/blog/physics-in-javascript-rigid-bodies-part-1-pendulum-clock/)
[Previous post
→](http://burakkanber.com/blog/preparing-for-the-future-of-technology/)

[RSS](http://burakkanber.com/blog/feed/ "Syndicate this site using RSS")
|
[Atom](http://burakkanber.com/blog/feed/atom/ "Syndicate this site using Atom")

![Clicky](//in.getclicky.com/100593568ns.gif)
