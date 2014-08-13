[Burak Kanber, Engineer](/blog/)
================================

### The Blog

[Follow @bkanber](https://twitter.com/bkanber)

[Machine Learning: Naive Bayes Document Classification Algorithm in Javascript](http://burakkanber.com/blog/machine-learning-naive-bayes-1/)
--------------------------------------------------------------------------------------------------------------------------------------------

On March 20, 2013

This article is part of the [Machine Learning in
Javascript](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/ "Machine Learning in Javascript: Introduction")
series which teaches the essential machine learning algorithms using
Javascript for examples. I use Javascript because it’s well-known and
universally supported, making it an excellent language to use for
teaching. There’s a mailing list at the bottom of the page if you want
to know about new articles; you can also [follow me on twitter:
@bkanber](https://twitter.com/bkanber).

Are you just looking for the code example? [Scroll
down!](http://burakkanber.com/blog/machine-learning-k-means-clustering-in-javascript-part-1/#fiddle)

Introduction and Motivation
---------------------------

Document classification is one of my favorite tasks. “Document
classification” is exactly what you think it is: given a document and a
set of labels, apply the most appropriate label to that document. Labels
(or “classes” or “categories”) can be things like:

-   “spam” or “not spam” (most mail clients use some form of Bayesian
    spam detection)
-   “written by a male” or “written by a female” (yes, there are clues
    that can hint to the gender of the author)
-   “technology”, “politics”, “finance”, “sports” (if you need to
    automatically categorize old articles from a newspaper)

Today we’re going to solve a simple problem: **language detection.** Put
another way: “given a piece of text, determine if it’s in Spanish,
English, or French”.

The Very Basics of Natural Language Processing
----------------------------------------------

First, we’ll have to change the way we think about “documents”. At their
most basic level, documents are just collections of arranged characters.
Some characters may be letters, others are not. Letters make words, and
words have meaning. Sometimes punctuation alters the meaning of words,
other times it doesn’t matter too much! (See what I did there?) We
typically study documents by studying the individual words in them.
Sometimes, if the situation calls for it, we’ll look at more than one
word at a time (word pairs are called “bigrams”), but since this is only
“part 1″ of this Bayes series we’ll only consider unigrams for now.

Because natural language itself is so quirky and complicated, we
generally try to simplify things; we try to reduce the *entropy* of the
system. If you consider both uppercase and lowercase letters, you can
create 7.3 million different four-letter words. If you limit yourself to
only lowercase letters, that figure drops down to only 450,000. Why is
this important? You want your system to use as much data as it can, and
treating “YOU’RE” and “you’re” and “You’re” separately only serves to
keep what you learn about each in separate buckets. Converting all of
those to a simple “you’re” is best, because it’ll allow you to study the
*concept* of the word “you’re” rather than the various syntaxes of the
word.

Of course, more advanced data scientists will recognize that sometimes
you *do*want these quirks in your system. When detecting spam emails,
for instance, it turns out that “offer” and “OFFER” are two very
different things. Often, spam detection algorithms will *not*normalize
based on case because of this. Similarly, “money back” and “money
back!!!!” have two very different meanings, so spam detection algorithms
will generally also leave the punctuation intact.

You may also decide that you also want the meanings of the words
“imagine”, “imagination”, and “imaginary” to be lumped together. To do
so, you might chop off the ending of each, converting them to “imagin” —
it’s neither verb nor noun, neither singular nor plural, it’s simply a
concept. This is called “stemming”, and it serves to treat different
words with the same meaning as the same entity. These entropy-reduction
techniques are very important in machine learning as a whole, since
training sets for learning algorithms are usually limited.

The process of splitting a document up into discrete chunks that you can
study is called “tokenization”. For now, we’ll keep our tokenization
simple: we’ll remove any punctuation, make everything lowercase, and
split the document up by spaces to get our tokens.

Bayes’ Theorem
--------------

I’m not going to talk about the math around [Bayes’
theorem](http://en.wikipedia.org/wiki/Bayes'_theorem), because
interested readers can easily learn the basics of probability and Bayes’
theorem on their own. Instead, we’ll aim to develop an intuition about
this important tool in probability.

We’re trying to figure out which language a previously unseen document
is in. We have a stack of pre-labeled documents in English, French, and
Spanish. Since we decided above that we can learn about a document by
inspecting the individual words in that document, let’s start there.

If you look at a single word in a document, you can easily figure out
how many times it appeared in your training data. Using that
information, you can determine the probability that a certain language
will use a given word. For example: “vous” is the first word in your
document of unknown origin. “Vous” may show up in all of your French
documents (100%), no Spanish documents, and a small number of English
documents (5%). That’s a great hint! The document is French!

But no, we can’t stop there. Just because “vous” is clearly a French
word doesn’t mean the document itself is French. It may be an English
novel quoting a French character. So we can’t just simply look at the
“probability that ‘vous’ is French”, which is what we just tried.
Instead, we need to determine the “probability that this document is
French given that the word ‘vous’ is in it”. Fortunately, Bayes’ theorem
does exactly that. If we apply Bayes’ theorem to the fake numbers I gave
above, we find that there’s a 98% chance that a document is French if
“vous” appears in it. The formula to calculate that is quite simple; see
[Bayes’ theorem](http://en.wikipedia.org/wiki/Bayes'_theorem).

Of course, if the next phrase in the document is “said Jean Pierre, the
French museum curator”, we know there’s a much smaller chance of the
document being French. So we look at each word in turn and calculate the
“probability that the document is (French|English|Spanish) given that
this word is in it”. We combine those individual probabilities and end
up with an overall “probability that this document is French [given that
all these words are in it]“. If that probability is high enough, you can
act upon it.

Why it Works
------------

The Naive Bayes Classifier is so named because it assumes that each word
in the document has nothing to do with the next word. That’s a naive
assumption. But it turns out that, while naive, it’s actually a great
simplifying assumption; studying words separately like this actually
yields very good results. You could also study bigrams or trigrams (sets
of two or three words at a time), at which point the classifier is no
longer “naive” but it’ll require a much larger amount of training data
and storage space.

The reason the NB classifier works well for document classification is
that it de-correlates the number of times a word is seen in a given
language from its statistical importance. The word “a” is found in many
languages. Perhaps it even appears in 100% of your English training set.
But that doesn’t mean that documents that have it are English. We use
Bayes to convert the “probability that ‘a’ appears in an English
document” (which is 100%) to the “probability that this document is
English because it has ‘a’ in it” (maybe 50%).

Therefore, the common stuff that’s found everywhere is given a very weak
significance and the stuff that’s found more uniquely across a category
is given a much stronger weight. The end result is a very smart, simple
algorithm that has low error rates (“low” being a relative term). It’s
not magic, there’s no neural network, there’s no “intelligence”, it’s
just math and probability.

The Code: Training
------------------

As usual, let’s just dive in. The first thing our document classifier
needs to be able to do is train itself given a piece of text and a label
for that text. Since I decided to build this classifier as generalized
as possible, we won’t hard code the labels or even put a cap on the
number of labels allowed. (Though naive Bayes classifiers with only two
possible labels, like spam/ham, *are* a little bit easier to build.)

~~~~ {.code}
Bayes.train = function (text, label) {
    registerLabel(label);
    var words = tokenize(text);
    var length = words.length;
    for (var i = 0; i < length; i++)
        incrementStem(words[i], label);
    incrementDocCount(label);
};
~~~~

The `registerLabel` function simply adds the label to the "database" (in
this case, localStorage) so that we can retrieve a list of labels later.

The `tokenize` function in this case is very simple. We'll look at more
interesting tokenization techniques in another article (tokenization can
be an important part of these classifiers), but this one is
straight-forward:

~~~~ {.code}
var tokenize = function (text) {
    text = text.toLowerCase().replace(/\W/g, ' ').replace(/\s+/g, ' ').trim().split(' ').unique();
    return text;
};
~~~~

In this case, we use `unique()` because we're only interested in
*whether* a word shows up in a document, and not the number of times it
shows up. In certain situations, you may get better results by
considering the number of times a word appears in a document.

We then loop through each word (or "token"), and call `incrementStem` on
it. This function is also very simple: it just records the number of
times a word was seen for a given label.

Finally, we call `incrementDocCount`, which records how many documents
we've seen for a given label.

The end result of training is that we have a database that stores each
label we've ever seen (in our example, it'll hold "english", "spanish",
and "french"), stores the number of times a word has been seen for a
label (eg, "le" was seen in French documents 30 times), and stores the
total number of documents for each label (eg, we saw 40 French
documents).

The Code: Guessing
------------------

Training a naive Bayes classifier is dead simple and really fast, as
demonstrated above. Guessing a label given a document is a little
tougher, but writing the algorithm is easy to those who understand
probability. If you don't understand probability, that's ok; you can
spend some time reading up on naive Bayes classifiers and you'll always
have this example to come back to and study.

The first thing we'll do in our guessing function (other than
initializing variables; see the JSFiddle for the minutiae) is a little
bit of bookkeeping:

~~~~ {.code}
for (var j = 0; j < labels.length; j++) {
    var label = labels[j];
    docCounts[label] = docCount(label);
    docInverseCounts[label] = docInverseCount(label);
    totalDocCount += parseInt(docCounts[label]);
}
~~~~

Our goal here is to set ourselves up for calculating certain
probabilities later. To do this, we need to know the number of documents
we've seen for a given label (docCounts), but we also need to know the
number of documents *not* in that label (docInverseCounts). Finally, we
need to know the total number of documents we've ever seen.

You could flip this function upside down and get docInverseCount simply
by subtracting a label's docCount from the totalDocCount -- in fact,
that approach is better and faster, but I did it with an explicit
docInverseCount function because it reads a little easier.

Given the above information we can determine, for example, the
probability that any arbitrary document would be French. We also know
the probability that any document is NOT French.

~~~~ {.code}
for (var j = 0; j < labels.length; j++) {
    var label = labels[j];
    var logSum = 0;
    ...
~~~~

Next, we look at each label. We set up a `logSum` variable, which will
store the probability that the document is in this label's category.

~~~~ {.code}
for (var i = 0; i < length; i++) {
    var word = words[i];
    var _stemTotalCount = stemTotalCount(word);
    if (_stemTotalCount === 0) {
        continue;
    } else {
        var wordProbability = stemLabelCount(word, label) / docCounts[label];
        var wordInverseProbability = stemInverseLabelCount(word, label) / docInverseCounts[label];
        var wordicity = wordProbability / (wordProbability + wordInverseProbability);

        wordicity = ( (1 * 0.5) + (_stemTotalCount * wordicity) ) / ( 1 + _stemTotalCount );
        if (wordicity === 0)
            wordicity = 0.01;
        else if (wordicity === 1)
            wordicity = 0.99;
   }

    logSum += (Math.log(1 - wordicity) - Math.log(wordicity));
}
scores[label] = 1 / ( 1 + Math.exp(logSum) );
~~~~

The above is the meat of the algorithm. For each label we're
considering, we look at each word in the document. The \_stemTotalCount
variable holds the number of times we've seen that word in *any*
document during training. If we've never seen this word before, just
skip it! We don't have any information on it, so why use it?

`wordProbability` represents the "probability that this word shows up in
a [French|English|Spanish] document". If you've seen 40 French
documents, and 30 of them have the word "le" in them, this value is
0.75. `wordInverseProbability` is the probability that the word shows up
in any other category than the one we're considering.

The funny `wordicity` variable is what happens when you apply Bayes'
theorem to the two probabilities above. While the wordProbability
variable represents "the probability that [le] shows up in a [French]
document", the wordicity variable represents "the probability that this
document is [French] given that [le] is in it". The distinction is
subtle but very important. If you're having trouble understanding the
distinction at this point, I strongly recommend saying those two phrases
out loud and making sure you understand the difference before moving on.

The wordicity line above also makes the assumption that English, French,
and Spanish documents are all equally common and starting off on the
same footing. This assumption makes the calculation a little simpler,
but you can consider the *a priori* probabilities of each language if
you'd like. A note on that later.

The line below the wordicity definition is an optional *adjustment* for
words that we've only seen in training a few times. If you've only seen
a word once, for instance, you don't really have enough information
about that word to determine if it's really French or Spanish. So we
make a weighted adjustment: we bring the wordicity closer to 50% if we
haven't seen it too many times. The "0.5" in that equation is the value
we should try to adjust towards, and the "1"s in the equation are the
weight -- if you increase this value, the wordicity will remain close to
0.5 longer. If you have a large training set, you can make the weight 5
or 10 or 20 or 50 (depending on how big your training set is). Since we
have a very small training set, I made this value 1 but realistically I
should have just omitted the line completely (my training set is only 15
paragraphs). I just wanted to show you that adjusting for rare words is
something that you can do.

Below that, we avoid letting wordicity be either 0 or 1 since we're
about to use a log function on our data, and either of those values
would kind of mess up the results.

The logSum line isn't really a part of the mathematical equations, but
is rather a practical consideration. After calculating the wordicity for
each word, we need to combine those probabilities somehow. The normal
mathematical way to do that would be to multiply each probability
together and divide by the multiplication of all the inverses.
Unfortunately, floating point math isn't perfect and you can run into
"floating point underflow", where the number gets too small for floating
point math to deal with. So instead, we take a log of the numerator and
denominator (combined probabilities and their inverses), and add up the
logs.

Finally, after we've combined all the individual word probabilities with
the logSum line, we undo the log function we just used to get the
probability back in the 0 to 1 range. Note that this happens outside of
the "look at each word" loop but still inside the "look at each label"
loop.

Important Assumptions and Caveats
---------------------------------

One thing I'd like to point out is that the above makes a big
simplifying assumption: we've assumed that English, French, and Spanish
documents are all *equally likely* to appear. In our example, this is a
good assumption since you guys are probably going to test one of each
later, but in the real world this isn't necessarily true.

The Bayes classification algorithm does actually let you consider the *a
priori* probability of a document's language (meaning, the probability
that a document is English just based on the number of English documents
out there, before considering the actual contents of the document), but
I've simply left this out. It's not too hard to put in; adding just a
few more terms to the wordicity calculation can do this. The next Bayes
article I write will use the full form of Bayes theorem.

Finally, please note that I was *really lazy* while training this
algorithm. You can see from the JSFiddle that I've only used 5
paragraphs from each language to train the thing on. That's not nearly
enough to go by, as the words seen during training are the only words it
knows. I've found that this example *does* work well if you type in
sentences or paragraphs (try just copy/pasting stuff from news sites),
but simple nouns and phrases probably won't work. For example, you'll
get the wrong result for "la tortuga" ("the turtle" in Spanish) simply
because we never showed it the word "tortuga" before. The algorithm will
guess French in this case because it's seen slightly more "la"s in
French than it's seen in Spanish. A larger training set would fix this
issue.

Results
-------

We're basically done. All we have to do now is either report all the
labels' probabilities or just pluck out the highest one. Try pasting
some English, French, or Spanish news text in the JSFiddle below -- you
should see the guessed language and the probability that led us to guess
that language. This example works better with sentences or paragraphs;
the more words you give it to guess by, the better the chance that it
has seen one of those words in its limited training set.

So far, I've had 100% accuracy when copying and pasting sentences from
news sites. Try it below and see for yourself!

If you like this article, or thought it was helpful, please consider:

-   Sharing it with friends on Facebook and Twitter
-   Discussing it on Hacker News and Reddit
-   Discussing it in the comments section below
-   [Following me on Twitter @bkanber](//twitter.com/bkanber)
-   [Reading my other ML in JS
    articles](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/)
-   Signing up for the ML in JS mailing list below — I only send emails
    about new articles, never spam, won't ever sell your info

Email me when new ML in JS articles are posted
----------------------------------------------

Email Address

**Survey: Would you buy an ML in JS e-book?**\
 I would pay $10 for a DRM-free e-book with tons of ML lessons and JS
examples.

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

[← Next post
|](http://burakkanber.com/blog/bash-scripting-why-didnt-i-start-this-earlier/)
[Previous post
→](http://burakkanber.com/blog/i-was-just-tricked-into-learning-another-language/)

[RSS](http://burakkanber.com/blog/feed/ "Syndicate this site using RSS")
|
[Atom](http://burakkanber.com/blog/feed/atom/ "Syndicate this site using Atom")

![Clicky](//in.getclicky.com/100593568ns.gif)
