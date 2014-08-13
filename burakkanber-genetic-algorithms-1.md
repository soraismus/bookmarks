[Burak Kanber, Engineer](/blog/)
================================

### The Blog

[Follow @bkanber](https://twitter.com/bkanber)

[Machine Learning: Introduction to Genetic Algorithms](http://burakkanber.com/blog/machine-learning-genetic-algorithms-part-1-javascript/)
------------------------------------------------------------------------------------------------------------------------------------------

On September 4, 2012

*The [Introduction to “Machine Learning in
Javascript”](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/)
post provides a nice introduction and context for this post and the rest
of the series.*

I like starting my machine learning classes with genetic algorithms
(which we’ll abbreviate “GA” sometimes). Genetic algorithms are probably
the least practical of the ML algorithms I cover, but I love starting
with them because they’re fascinating and they do a good job of
introducing the “cost function” or “error function”, and the idea of
local and global optima — concepts both important and common to most
other ML algorithms.

Genetic algorithms are inspired by nature and evolution, which is
seriously cool to me. It’s no surprise, either, that artificial neural
networks (“NN”) are also modeled from biology: evolution is the best
general-purpose learning algorithm we’ve experienced, and the brain is
the best general-purpose problem solver we know. These are two very
important pieces of our biological existence, and also two rapidly
growing fields of artificial intelligence and machine learning study.
While I’m tempted to talk more about the distinction I make between the
GA’s “learning algorithm” and the NN’s “problem solver” terminology,
we’ll drop the topic of NNs altogether and concentrate on GAs… for now.

One phrase I used above is profoundly important: “general-purpose”. For
almost any specific computational problem, you can probably find an
algorithm that solves it more efficiently than a GA. But that’s not the
point of this exercise, and it’s also not the point of GAs. You use the
GA not when you have a complex problem, but when you have a complex
problem of problems. Or you may use it when you have a complicated set
of disparate parameters.

One application that comes to mind is bipedal robot walking. It’s damn
hard to make robots walk on two legs. Hand-coding a walking routine will
almost certainly fail. Even if you succeed in making a robot walk, the
next robot that comes off the line might have a slightly different
center of balance, and that algorithm you slaved over no longer works.
Instead of enduring the inevitable heartbreak, you might use a GA to
“teach the robot to learn to walk” rather than simply “teaching the
robot to walk”.

Let’s build a GA in Javascript.

### The Problem

Build a genetic algorithm in Javascript that reproduces the text “Hello,
World!”.

Naturally, everything starts with “Hello, World!” and so building a GA
to reproduce that phrase is apropos. Note that this problem is highly
contrived. At one point we’re even going to type the phrase “Hello,
World!” into the source code! Now that seems silly — if you know the
desired result, why program the algorithm in the first place? The answer
is simple: this is a learning exercise. The next GA exercise (which will
be in PHP) will be a little less contrived, but we need to start
somewhere.

### Genetic Algorithm Basics

The basic approach to GAs is to generate a bunch of “answer candidates”
and use some sort of feedback to figure out how close the candidate is
to optimal. Far-from-optimal candidates literally die and are never seen
again. Close-to-optimal candidates combine with each other and maybe
mutate slightly; this is an attempt to modify the candidates from time
to time and see if they get closer to optimal or farther from optimal.

These “answer candidates” are called **~~genes~~ chromosomes**. (*Note:
my terminology here was incorrect. Genes are technically the individual
characters of the solution candidate, while the whole thing is called a
chromosome. Semantics are important!*)

Chromosomes mate, produce offspring, and mutate. They either die due to
survival of the fittest, or are allowed to produce offspring who may
have more desirable traits and adhere to natural selection.

This may be a strange way to think about solving for “Hello, World!” but
stick with it. This example isn’t the only problem that can be solved
with GAs!

### The Chromosome

The chromosome is a representation of a solution candidate. In our case,
the chromosome itself is a string. Let’s assume that all chromosomes
have a length of 13 characters (the same as “Hello, World!”). Here are
some possible chromosomes that could be solution candidates for our
problem:

-   Gekmo+ xosmd!
-   Gekln, worle”
-   Fello, wosld!
-   Gello, wprld!
-   Hello, world!

Obviously that last one is the “correct” (or globally-optimum)
chromosome. But how do we measure the optimality of a chromosome?

#### Cost Function

The cost function (or error function, or fitness function as the
inverse) is some sort of measure of the optimality of a chromosome. If
we’re calling it “fitness function” then we’re shooting for higher
scores, and if we’re using “cost function” then we’re looking for low
scores.

In this case, we might define a cost function to be something like the
following:

> For each character in the string, figure out the difference in ASCII
> representation between the candidate character and the target
> character, and then square it so that the “cost” is always positive.

For example, if we have a capital “A” (ASCII 65) but it’s supposed to be
a capital “C” (ASCII 67), then our cost for that character is 4 (67 – 65
= 2, and 2\^2 = 4).

Again, the reason we’re using the square of the difference is so that we
never end up with a negative cost. You could just use absolute value if
you want, too. Please experiment with different approaches — that’s how
you learn!

Using that rule as a cost function, we can calculate the costs of the
above 5 example chromosomes (in parentheses):

-   Gekmo+ xosmd! **(7)**
-   Gekln, worle” **(5)**
-   Fello, wosld! **(5)**
-   Gello, wprld! **(2)**
-   Hello, world! **(0)**

In this case, since this problem is easy and contrived, we know that
we’re shooting for a cost of 0 and that we can stop there. Sometimes
that’s not the case. Sometimes you’re just looking for the lowest cost
you can find, and need to figure out different ways to end the
calculation. Other times you’re looking for the *highest* “fitness
score” you can find, and similarly need to figure out some other
criteria to use to stop the calculation.

The cost function is a very important aspect of GAs, because if you’re
clever enough, you can use it to reconcile *completely disparate
parameters.* In our case, we’re just looking at letters. But what if
you’re building a driving directions app and need to weigh tolls vs
distance vs speed vs traffic lights vs bad neighborhoods vs bridges?
Those are completely disparate parameters that you can reduce into one,
neat, tidy cost function for a route by applying different weights to
each parameter.

#### Mating and Death

Mating is a fact of life, and we use it tons in GAs. Mating is a magical
moment that two chromosomes in love with each other share. The technical
term for mating is “crossover”, but I’ll continue calling it “mating”
here, because that paints a more intuitive picture.

We haven’t talked about “populations” in GAs yet (we’ll get to that a
bit later) but for now I’ll just say that when you run a GA, you don’t
just look at one chromosome at a time. You might have a population of 20
or 100 or 5,000 going all at once. Just like in evolution, you might be
inclined to have the best and strongest chromosomes of the population
mate with each other, with the hope that their offspring will be even
healthier than either parent.

Mating strings, like in our “Hello, World!” example is pretty easy. You
can pick two candidates (two strings; two chromosome) and pick a point
in the middle of the string. This point can be dead-center if you want,
or randomized if you prefer. Experiment with it! Take that middle point
(called a “pivot” point), and make two new chromosomes by combining the
first half of one with the second half of the other and vice versa.

Take these two strings for example:

-   Hello, wprld! **(1)**
-   Iello, world! **(1)**

Cutting them in half and making two new strings from the alternating
halves gives us these two new “children”:

-   Iello, wprld! **(2)**
-   Hello, world! **(0)**

As you can see, the offspring includes one bastard child that has the
worst traits of both parents, but also an angel child that has the best
traits of both.

Mating is how you get from one generation of genes to the next.

#### Mutation

Mating alone has a problem: in-breeding. If all you do is mate your
candidates to go from generation to generation, you’ll get stuck near a
“local optimum”: an answer that’s *pretty good* but not necessarily the
“global optimum” (the best you can hope for).

(*I was recently informed that the above paragraph is misleading. Some
readers got the impression that the most important aspect of chromosome
evolution is the mating, when in actuality a GA would achieve very
little if not for the combined effects of both mating and mutation. The
mating helps discover more optimal solutions from already-good solutions
[many problems' solutions, like our Hello World problem, can be divided
into optimal sub-solutions, like "Hello" and "world" separately], but
it’s the mutation that pushes the search for solutions in new
directions.*)

Think of the world that these genes are living in as a physical setting.
It’s really hilly with all sorts of weird peaks and valleys. There is
one valley that’s the lowest of all, but there are also tons of other
little valleys — while these other valleys are lower than the land
directly around them, they’re still above sea-level overall. Searching
for a solution is like starting a bunch of balls on the hills in *random
places*. You let the balls go and they roll downhill. Eventually, the
balls will get stuck in the valleys — but many of them will get stuck in
the random mini-valleys that are stilly pretty high up the hill (the
local optima). It’s your job to make sure at least one of the balls ends
up in the lowest point on the whole map: the global optimum. Since the
balls all start in random places, it’s hard to do this from the outset,
and it’s impossible to predict which ball will get stuck where. But what
you *can*do is visit a bunch of the balls *at random* and give them a
kick. Maybe the kick will help, maybe it’ll hurt — but the idea here is
to shake up the system a little bit to make sure things aren’t getting
stuck in local optima for too long.

This is called **mutation**. It’s a completely random process by which
you target an unsuspecting chromosome and blast it with just enough
radiation to make one of its letters randomly change.

Here’s an example to illustrate. Let’s say you end up with these two
chromosomes:

-   Hfllp, worlb!
-   Hfllp, worlb!

Again, a contrived example, but it happens. Your two chromosomes are
exactly the same. That means that their children will be exactly the
same as the parents, and no progress will ever be made. But if 1 in 100
chromosomes has one letter randomly mutated, it’s only a matter of time
before chromosome \#2 above turns into “Ifllp, worlb!” — and then the
evolution continues because the children will finally be different from
the parents once again. Mutation pushes the evolution forward.

How and when you mutate is up to you. Again, experiment. The code I’ll
give you later has a very high mutation rate (50%), but that’s really
just for demonstration. You might make it low, like 1%. My code makes
only *one letter* move by *one ASCII code* but you can have yours be
more radical. Experiment, test, and learn. It’s the only way.

#### Chromosomes: Summary

Chromosomes are representations of candidate solutions to your problem.
They consist of the representation itself (in our case, a 13-character
string), a cost or fitness score and function, the ability to mate
(“crossover”), and the ability to mutate.

I like thinking about these things in OOP terms. The “Chromosome” class
therefore has the following properties:

**Properties:**

-   Genetic code
-   Cost/fitness score

**Methods:**

-   Mate
-   Mutate
-   Calculate Fitness Score

We’ll now look at how to have genes interact with each other in the
final piece of the GA puzzle: the “Population”.

### The Population

The population is a group of chromosomes. The population generally
remains the same size but will typically evolve to better average cost
scores over time.

You get to choose your population size. I picked 20 for mine below, but
you could choose 10 or 100 or 10,000 if you want. There are advantages
and disadvantages, but as I’ve said a few times by now: experiment and
learn for yourself!

The population experiences “generations”. A typical generation may
consist of:

-   Calculating the cost/fitness score for each chromosome
-   Sorting the chromosome by cost/fitness score
-   Killing a certain number of the weakest members — you pick the
    number of chromosome that will die
-   Mating a certain number of the strongest members — again, you pick
    how you do this
-   Mutating members at random
-   Some kind of completeness test — ie, how do you determine when to
    consider the problem “solved”?

#### Starting and Finishing

Starting a population is easy. Just fill it with completely random
chromosomes. In our case, the cost scores for completely random strings
will be *horrendous.* My code starts at an average cost score of 30,000.
But that’s ok — that’s what evolution is for. This is why we’re here.

Knowing when to stop the population is a little trickier. Today’s
example is pretty simple: stop when you get a cost of 0. But this isn’t
always the case. Sometimes you don’t know the minimum achievable cost.
Or, if you’re using fitness instead of cost, you may not know the
maximum possible fitness.

In those cases you should specify a completeness criteria. This can be
anything you want, but here’s a starting suggestion to jump off from:

> Stop the algorithm if the best score hasn’t changed in 1,000
> generations, and use that as your answer.

Criteria like that may mean that you never achieve the global optimum,
but in many cases you don’t *need* to achieve the global optimum.
Sometimes “close enough” really is good enough.

I’ll soon be writing another article on GAs (for PHP this time) with a
slightly different problem, and that one will have a completeness rule
similar to the above. It might be hard to swallow that “close enough is
good enough” right now, but once you see the example in action hopefully
you’ll believe me.

### The Code

Finally! I like OOP methods, but I also like rough and simple code, so
I’ll do this as straight-forwardly as possible though it may be a little
rough around the edges.

(*Note that while I changed the occurrences of the term “gene” to
“chromosome” in the text above, the code below still uses the incorrect
“gene” terminology. It’s a semantic and pedantic difference, but where
would we be without semantic pedantics?*)

~~~~ {.code}
var Gene = function(code) {
        if (code)
                this.code = code;
        this.cost = 9999;
};
Gene.prototype.code = '';
Gene.prototype.random = function(length) {
        while (length--) {
                this.code += String.fromCharCode(Math.floor(Math.random()*255));
        }
};
~~~~

Simple. It’s just a class that takes a string as a constructor, sets a
cost, and has a helper function to create a new, random chromosome.

~~~~ {.code}
Gene.prototype.calcCost = function(compareTo) {
        var total = 0;
        for(i = 0; i < this.code.length; i++) {
                total += (this.code.charCodeAt(i) - compareTo.charCodeAt(i)) * (this.code.charCodeAt(i) - compareTo.charCodeAt(i));
        }
        this.cost = total;
};
~~~~

The cost function takes the “model” string as an argument, finds the
differences between ASCII codes, and squares them.

~~~~ {.code}
Gene.prototype.mate = function(gene) {
        var pivot = Math.round(this.code.length / 2) - 1;

        var child1 = this.code.substr(0, pivot) + gene.code.substr(pivot);
        var child2 = gene.code.substr(0, pivot) + this.code.substr(pivot);

        return [new Gene(child1), new Gene(child2)];
};
~~~~

The mating function takes another chromosome as an argument, finds the
center point, and returns an array of two new children.

~~~~ {.code}
Gene.prototype.mutate = function(chance) {
        if (Math.random() > chance)
                return;

        var index = Math.floor(Math.random()*this.code.length);
        var upOrDown = Math.random()
~~~~

The mutate method takes a float as an argument — the percent chance that
the chromosome will mutate. If the chromosome is to mutate we randomly
decide if we’re going to add or subtract one from the randomly-selected
character code. I was going too fast to write a proper
String.prototype.replaceAt method, so I just took an easy shortcut
there.

~~~~ {.code}
var Population = function(goal, size) {
        this.members = [];
        this.goal = goal;
        this.generationNumber = 0;
        while (size--) {
                var gene = new Gene();
                gene.random(this.goal.length);
                this.members.push(gene);
        }
};
~~~~

The Population class constructor takes the target string and population
size as arguments, then fills the population with random chromosomes.

~~~~ {.code}
Population.prototype.sort = function() {
        this.members.sort(function(a, b) {
                return a.cost - b.cost;
        });
}
~~~~

I define a Population.prototype.sort method as a helper function to sort
the population by their cost score.

~~~~ {.code}
Population.prototype.generation = function() {
        for (var i = 0; i < this.members.length; i++) {
                this.members[i].calcCost(this.goal);    
        }

        this.sort();
        this.display();
        var children = this.members[0].mate(this.members[1]);
        this.members.splice(this.members.length - 2, 2, children[0], children[1]);

        for (var i = 0; i < this.members.length; i++) {
                this.members[i].mutate(0.5);
                this.members[i].calcCost(this.goal);
                if (this.members[i].code == this.goal) { 
                        this.sort();
                        this.display();
                        return true;
                }
        }
        this.generationNumber++;
        var scope = this;
        setTimeout(function() { scope.generation(); } , 20);
};
~~~~

The meatiest population method is the generation method. There’s no real
magic here. The display() method (not shown on this page) just renders
output to the page, and I set a timeout between generations so that
things don’t explode.

Note that in this example I’m only mating the top two chromosomes. This
doesn’t have to be your approach.

~~~~ {.code}
window.onload = function() {
        var population  = new Population("Hello, world!", 20);
        population.generation();
};
~~~~

That gets the ball rolling. See it in action:

It’s slow — but it’s not too difficult to figure out where the
inefficiencies are. If you’re clever enough you can certainly make it
lightning fast. As always, I encourage you to fork and experiment and
learn on your own.

Notice that there’s nothing in the above that can’t be done in *any*
programming language. But you probably expected that, since this is
“Machine Learning in All Languages”, after all.

Happy learning! The next post in this series is [ML in JS:
k-nearest-neighbor](http://burakkanber.com/blog/machine-learning-in-js-k-nearest-neighbor-part-1/ "Machine Learning in JS: k-nearest-neighbor Introduction").

There’s also a [Genetic Algorithms Part
2](http://burakkanber.com/blog/machine-learning-genetic-algorithms-in-javascript-part-2/ "Machine Learning: Genetic Algorithms in Javascript Part 2"),
which you should read if you want to get a little more advanced.

Email me when new ML in JS articles are posted
----------------------------------------------

Email Address

**Survey: Would you buy an ML in JS e-book?**\
 I would pay $10 for a DRM-free e-book with tons of ML lessons and JS
examples.

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

[← Next post |](http://burakkanber.com/blog/using-gmail-and-imap/)
[Previous post
→](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/)

[RSS](http://burakkanber.com/blog/feed/ "Syndicate this site using RSS")
|
[Atom](http://burakkanber.com/blog/feed/atom/ "Syndicate this site using Atom")

![Clicky](//in.getclicky.com/100593568ns.gif)
