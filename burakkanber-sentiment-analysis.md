[Burak Kanber, Engineer](/blog/)
================================

### The Blog

[Follow @bkanber](https://twitter.com/bkanber)

[Machine Learning: Sentiment Analysis](http://burakkanber.com/blog/machine-learning-sentiment-analysis/)
--------------------------------------------------------------------------------------------------------

On November 9, 2013

*This article demonstrates a simple but effective sentiment analysis
algorithm built on top of the [Naive Bayes classifier I
demonstrated](http://burakkanber.com/blog/machine-learning-naive-bayes-1/)
in the last ML in JS article. I’ll go over some basic sentiment analysis
concepts and then discuss how a Naive Bayes classifier can be modified
for sentiment analysis. If you’re just looking for the summary and code
demonstration, [jump down](#liveexample).*

Introduction
------------

Compared to the other major machine learning tasks, sentiment analysis
has surprisingly little written about it. This is in part because
sentiment analysis (and natural language in general) is difficult, but I
also suspect that many people opt to monetize their sentiment algorithms
rather than publishing them.

Sentiment analysis is highly applicable to enterprise business (“alert
me any time someone writes something negative about one of our products
anywhere on the internet!”), and sentiment analysis is fresh enough that
there’s a lot of “secret sauce” still out there. And now that the world
is run on social sites like Twitter, Facebook, Yelp, and Amazon
(Reviews), there are literally billions of data points that can be
analyzed every single day. So it’s hard to blame anyone for trying to
get a piece of that pie!

Today we’ll discuss an “easy” but effective sentiment analysis
algorithm. I put “easy” in quotes because our work today will build upon
the Naive Bayes classifer we talked about last time; but this will only
be “easy” if you’re familiar with those concepts already–you may want to
review that article before continuing.

There are better, more sophisticated algorithms than the one we’ll
develop today, but this is a great place to start. As always, I’ll
follow up with additional articles that cover more advanced topics in
sentiment analysis in the future.

I found [a nice, compact data
set](http://www.cs.cornell.edu/people/pabo/movie-review-data/) for this
experiment. It consists of 10,662 sentences from movie reviews, labeled
“positive” and “negative” (and split evenly). The primary goal today is
to figure out how to tweak the Naive Bayes classifier so that it works
well for sentiment analysis.

Modifying Naive Bayes
---------------------

“Naive Bayes Classification” is termed such because it makes the
assumption that each word is statistically independent from each other
word. In language, however, that assumption doesn’t hold true. “England”
follows the word “Queen [of]” much more commonly than most other words
(“Queen of Asparagus” just isn’t something you see every day), so it’s
clear that words are not independent of one another. Still, it’s been
shown that the “naive” assumption is pretty good for the purposes of
document classification.

And that’s good news, because sentiment analysis feels quite a bit like
classification: given a document, would you label it “positive” or
“negative”? The main difference between classification and sentiment
analysis is the idea that classification is objective but sentiment is
subjective. But let’s make another “naive” assumption and pretend that
we don’t care about that. Let’s try applying Bayes to sentiment and see
what happens.

For what it’s worth, lots of scientific discoveries are made by people
who are willing to “just try it and see what happens”. If it fails, so
what? Most experiments do. If it succceds though, that’d be great news!

Still, I don’t think we should completely ignore the fact that sentiment
is subjective and classification is objective. Subjective language is
trickier than objective language because it’s prone to idiom, turns of
phrase, sarcasm, negation, and other artifacts of emotive language.
Detecting what language a sentence is in, for instance, is
straightforward because it’s built on *facts*. Did you observe the word
“*allons-y*“? If so, probably French. But consider Borat-ifying a movie
review: “I loved this movie… not!” The naive approach may see the word
“loved” and label the review positive, but that “not” changes the entire
meaning of the sentence.

We should try to move just a little bit away from our assumption that
words are independent. One nice way of doing this — if you have lots of
training data — would be to use bigrams instead of unigrams. If you
recall, the training portion of a Bayes classifer involves looking
through your training set and counting the number of times a word
appears in a class of documents. (Eg: “Clinton” appeared 9% of the time
in “politics” articles, but only 0.1% of the time in “cooking”
articles.)

Our Bayes classifier can easily be extended to use bigrams just by
tokenizing our document two words at a time instead of one! Rather than
treating the words “This” “movie” “was” “not” “great” separately — and
risk getting confused by the negation — all we have to do is record them
as pairs: “This movie” “movie was” “was not” “not great”. Now our
classifer knows the difference between “was great” and “not great”!

Entropy
-------

Sadly, using bigrams poses a real problem for us: it increases our
entropy. Entropy is a concept used across lots of fields, but roughly
speaking, it’s a measure of the number of possible “states” a system can
be in. If we use 7,000 different words in conversational English and
build a unigram-based Bayes classifer for them, then we’ll need to store
data (word counts) for only 7,000 words. But if we start using bigrams,
we could *potentially* need to store data for up to 49,000,000 different
word *pairs* (that’s our maximum possible value though; in practice we’d
probably only encounter 30,000 unique word pairs or so).

But storage space and computational complexity is the least of our
concerns (30,000 records is nothing!). The real problem is the lack of
training data. Using bigrams and introducing more entropy into our
system means that each word pair is going to be relatively rare! A
unigram approach might encounter the word “great” 150 times in our
training data, but how many times will it see “great movie”? Or “not
great”? Or “was great”? Or “seriously great”? The bigram approach is
nice, but unless you have a huge training corpus it’s probably not the
way to go.

> Note that there are other ways to combat entropy, and that’s a good
> thing to do even when you’re using unigrams. I almost always *stem*
> words as part of the tokenization process. Stemming puts word
> variations like “great”, “greatly”, “greatest”, and “greater” all into
> one bucket, effectively decreasing your entropy and giving you more
> data around the concept of “great”.

Dealing with negation without using bigrams
-------------------------------------------

It turns out that dealing with negations (like “not great”) is a pretty
important step in sentiment analysis. A negation word can affect the
tone of all the words around it, and ignoring negations would be a
pretty big oversight. Bigrams, unfortunately, require too much training
data, so we’ve got to find a better way to consider negation terms.

Fortunately, the solution is “kick-yourself-for-not-thinking-of-it”
easy. Here’s how I do it: if I see a negation (like “not”, “never”,
“no”, etc), I just add an exclamation point to the beginning of every
word after it!

The sentence “This movie was not great” turns into “This movie was not
!great”, and the token “!great” gets stored in our Bayes classifer as
having appeared in a negative review. “Great” appears in positive
reviews, and its negation, “!great” appears in negative ones. It’s
simple, it’s clever, and it’s effective. Best of all: you get to use a
regular Naive Bayes classifer. The only modification you need to make is
in your tokenizer (the code that goes through the text, pre-processes
it, and splits it up in to words or “tokens”), but the Bayes
classification algorithm stays the same.

> I ran this experiment’s data through a variety of different
> tokenizers, and in this case I found it’s more effective to flag only
> the word immediately before and after the negation term, rather than
> all the words until the end of the sentence. My tokenizer takes the
> sentence “This movie was not good because of the plot” and turns it
> into “This movie !was not !good because of the plot”.

Being confident
---------------

Another effective trick you can use has nothing to do with the Bayes
algorithm itself but rather with how you handle its results. A nice
feature of a Bayes classifier is that it outright *tells* you its
“confidence” in its ruling (confidence is in quotes here because this
isn’t the *statistical* definition of confidence, I’m just
editorializing and calling probability “confidence”).

Recall that the output of a Bayes classifier is “the probability that
this document is ‘negative’”. If you have the luxury of not needing to
label every single document you encounter, it may be best to only label
documents that have over a certain percentage probability. This approach
should make intuitive sense: if you read something and you’re only 51%
sure it’s negative, maybe you shouldn’t be labeling that document at
all.

In this experiment, setting the “confidence threshold” to 75% or greater
increased the classifier’s accuracy from 78.5% to 85%, a 6.5
percentage-point difference!

You may not always have the luxury to abstain from guessing, but it
makes sense in most cases. For example, if you need to alert a client
about negative reviews, you could hit close to 90% accuracy if you only
sent alerts out when the probability of being negative was greater than
80% or so.

Implementation and cross-validation
-----------------------------------

I won’t do a code walkthrough like I usually do, since the core of this
project is just the Naive Bayes classifier we built last time. However,
the cross-validation and test procedure is worth talking about.

Cross-validation is how you should be testing your algorithms. In short,
here’s what you do:

-   Obtain your training data set.
-   Shuffle it up.
-   Set 20% (or so) of it aside. Don’t use that portion for training.
-   Train your algorithm on the other 80% of data.
-   Test your algorithm on the 20% you set aside. You test on this
    portion of the data because your algorithm has never seen it before
    (it wasn’t part of the training set), but it’s pre-labeled which
    means you can:
-   Record the algorithm’s accuracy (how many did it get right?)
-   Optionally, record the algorithm’s accuracy more granularly by
    recording true positives, true negatives, false positives, and false
    negatives. From this data you can discern not only the overall
    accuracy, but also the algorithm’s “precision” and “recall”, which
    we’ll talk about in-depth at a later date.
-   Repeat from step 2. I tend to shuffle, retrain, and retest about 30
    times in order to get a statistically significant sample. Average
    your individual run accuracies to get an overall algorithm accuracy.

Remember that your algorithm will likely be highly specific to the type
of training data you gave it. The program below gets 85% on movie
reviews, but if you use it to judge the sentiment of a married couple’s
conversation it’ll probably fall short.

Overview and results.
---------------------

This experiment’s goal was simple: use a Naive Bayes classifier for
sentiment analysis, and figure out what we can do to boost its accuracy.

I tried and tested 18 different variants of tokenizers and the Bayes
classifier, both with and without adjustments for rare tokens, resulting
in 36 total tests. Each test was run 30 times, for a total of 1,080
experiments. I found that the least effective tokenizer was the
“stemmed, stopword-removed bigram” tokenizer (61.4% accuracy and slow as
hell), and the most effective tokenizer had these properties:

-   Unigram tokenizer (look at one word at a time)
-   Tokens were stemmed (with the [Porter stemmer
    algorithm](https://github.com/kristopolous/Porter-Stemmer); this
    converts “greater”, “greatest”, and “greatly” all to “great”,
    thereby giving us more data points for the concept “great” and
    reducing our entropy)
-   Flag any word to the left and right of a negation term (ie, put an
    exclamation point in front of any word next to “not” or “never” or
    similar; this turns that word into a different, negatively-connated
    word as far as Bayes is concerned. “great” becomes distinct from
    “!great”, which implies “not great”.)
-   Deal with rare tokens by pulling them towards 50% “wordicity” (a
    term I use in the Bayes article) with a weight of 3 (ie, words we’ve
    seen fewer times are forced closer to 50%, because we don’t trust
    them as much).
-   Only label a document if we determine the probability to be 75% or
    greater.

The overall accuracy on that algorithm was 85.0%, which was pleasantly
surprising to me. Most machine learning algorithms max out near 90%, so
this turned out to be very successful for our “introduction to sentiment
analysis”!

Live example
------------

Wait for the system to train, and then write a movie-review-like
sentence and see if it gets your sentiment right.

I’ll be writing at least two more articles about sentiment analysis, so
please be sure to ask any questions or make any suggestions you have in
the comments below.

If you like this article, or thought it was helpful, please consider:

-   Sharing it with friends on Facebook and Twitter
-   Discussing it on Hacker News and Reddit
-   Discussing it in the comments section below
-   [Following me on Twitter @bkanber](//twitter.com/bkanber)
-   [Reading my other ML in JS
    articles](http://burakkanber.com/blog/machine-learning-in-other-languages-introduction/)
-   Signing up for the ML in JS mailing list below — I only send emails
    about new articles, never spam, won’t ever sell your info

Email me when new ML in JS articles are posted
----------------------------------------------

Email Address

**Survey: Would you buy an ML in JS e-book?**\
 I would pay $10 for a DRM-free e-book with tons of ML lessons and JS
examples.

Please enable JavaScript to view the [comments powered by
Disqus.](http://disqus.com/?ref_noscript)

[Previous post
→](http://burakkanber.com/blog/why-im-learning-morse-code/)

[RSS](http://burakkanber.com/blog/feed/ "Syndicate this site using RSS")
|
[Atom](http://burakkanber.com/blog/feed/atom/ "Syndicate this site using Atom")

![Clicky](//in.getclicky.com/100593568ns.gif)
