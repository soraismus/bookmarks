Latent Dirichlet allocation
===========================

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

Not to be confused with [linear discriminant
analysis](/wiki/Linear_discriminant_analysis "Linear discriminant analysis").

In [natural language
processing](/wiki/Natural_language_processing "Natural language processing"),
**latent Dirichlet allocation** (**LDA**) is a [generative
model](/wiki/Generative_model "Generative model") that allows sets of
observations to be explained by
[unobserved](/wiki/Latent_variable "Latent variable") groups that
explain why some parts of the data are similar. For example, if
observations are words collected into documents, it posits that each
document is a mixture of a small number of topics and that each word's
creation is attributable to one of the document's topics. LDA is an
example of a [topic model](/wiki/Topic_model "Topic model") and was
first presented as a [graphical
model](/wiki/Graphical_models "Graphical models") for topic discovery by
[David Blei](/wiki/David_Blei "David Blei"), [Andrew
Ng](/wiki/Andrew_Ng "Andrew Ng"), and [Michael
Jordan](/wiki/Michael_I._Jordan "Michael I. Jordan") in
2003.^[[1]](#cite_note-blei2003-1)^

Contents
--------

-   [1 Topics in LDA](#Topics_in_LDA)
-   [2 Model](#Model)
    -   [2.1 Mathematical definition](#Mathematical_definition)

-   [3 Inference](#Inference)
-   [4 Applications, extensions and similar
    techniques](#Applications.2C_extensions_and_similar_techniques)
-   [5 See also](#See_also)
-   [6 Notes](#Notes)
-   [7 External links](#External_links)

Topics in LDA[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=1 "Edit section: Topics in LDA")]
-------------------------------------------------------------------------------------------------------------------------

In LDA, each document may be viewed as a
[mixture](/wiki/Mixture_model "Mixture model") of various topics. This
is similar to [probabilistic latent semantic
analysis](/wiki/PLSA "PLSA") (pLSA), except that in LDA the topic
distribution is assumed to have a
[Dirichlet](/wiki/Dirichlet_distribution "Dirichlet distribution")
[prior](/wiki/Prior_probability "Prior probability"). In practice, this
results in more reasonable mixtures of topics in a document. It has been
noted, however, that the [pLSA](/wiki/PLSA "PLSA") model is equivalent
to the LDA model under a uniform Dirichlet prior
distribution.^[[2]](#cite_note-2)^

For example, an LDA model might have topics that can be classified as
**CAT\_related** and **DOG\_related**. A topic has probabilities of
generating various words, such as *milk*, *meow*, and *kitten*, which
can be classified and interpreted by the viewer as "CAT\_related".
Naturally, the word *cat* itself will have high probability given this
topic. The **DOG\_related** topic likewise has probabilities of
generating each word: *puppy*, *bark*, and *bone* might have high
probability. Words without special relevance, such as *the* (see
[function word](/wiki/Function_word "Function word")), will have roughly
even probability between classes (or can be placed into a separate
category). A topic is not strongly defined, neither
[semantically](/wiki/Semantics "Semantics") nor
[epistemologically](/wiki/Epistemology "Epistemology"). It is identified
on the basis of
[supervised](/wiki/Supervised_learning "Supervised learning") labeling
and (manual) pruning on the basis of their likelihood of co-occurrence.
A lexical word may occur in several topics with a different probability,
however, with a different typical set of neighboring words in each
topic.

Each document is assumed to be characterized by a particular set of
topics. This is akin to the standard [bag of words
model](/wiki/Bag_of_words_model "Bag of words model") assumption, and
makes the individual words
[exchangeable](/wiki/De_Finetti%27s_theorem "De Finetti's theorem").

Model[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=2 "Edit section: Model")]
---------------------------------------------------------------------------------------------------------

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Latent_Dirichlet_allocation.svg/250px-Latent_Dirichlet_allocation.svg.png)](/wiki/File:Latent_Dirichlet_allocation.svg)

[![image](//bits.wikimedia.org/static-1.23wmf12/skins/common/images/magnify-clip.png)](/wiki/File:Latent_Dirichlet_allocation.svg "Enlarge")

[Plate notation](/wiki/Plate_notation "Plate notation") representing the
LDA model.

With [plate notation](/wiki/Plate_notation "Plate notation"), the
dependencies among the many variables can be captured concisely. The
boxes are “plates” representing replicates. The outer plate represents
documents, while the inner plate represents the repeated choice of
topics and words within a document. M denotes the number of documents, N
the number of words in a document. Thus:

*α* is the parameter of the Dirichlet prior on the per-document topic
distributions,

*β* is the parameter of the Dirichlet prior on the per-topic word
distribution,

![\\theta
\_{i}](//upload.wikimedia.org/math/0/f/9/0f998b6f411439b867c3ecce1259d5a2.png)
is the topic distribution for document *i*,

![\\phi
\_{k}](//upload.wikimedia.org/math/6/d/2/6d2e87758c566dbc1968a86049e1bf7d.png)
is the word distribution for topic *k*,

![z\_{{ij}}](//upload.wikimedia.org/math/5/2/b/52ba40236eda37f4665d21bd1f02a272.png)
is the topic for the *j*th word in document *i*, and

![w\_{{ij}}](//upload.wikimedia.org/math/8/a/c/8ac26dc240f3c9aff7384479dbfe0675.png)
is the specific word.

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/4/4d/Smoothed_LDA.png/251px-Smoothed_LDA.png)](/wiki/File:Smoothed_LDA.png)

[![image](//bits.wikimedia.org/static-1.23wmf12/skins/common/images/magnify-clip.png)](/wiki/File:Smoothed_LDA.png "Enlarge")

Plate notation for smoothed LDA

The
![w\_{{ij}}](//upload.wikimedia.org/math/8/a/c/8ac26dc240f3c9aff7384479dbfe0675.png)
are the only [observable
variables](/wiki/Observable_variable "Observable variable"), and the
other variables are [latent
variables](/wiki/Latent_variable "Latent variable"). Mostly, the basic
LDA model will be extended to a smoothed version to gain better results
.^[*[citation\\ needed](/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed")*]^
The plate notation is shown on the right, where *K* denotes the number
of topics considered in the model and:

![\\phi
](//upload.wikimedia.org/math/7/f/2/7f20aa0b3691b496aec21cf356f63e04.png)
is a *K\*V* (*V* is the dimension of the vocabulary) [Markov
matrix](/wiki/Markov_matrix "Markov matrix") each row of which denotes
the word distribution of a topic.

The generative process behind is that documents are represented as
random mixtures over latent topics, where each topic is characterized by
a distribution over words. LDA assumes the following generative process
for a corpus
![D](//upload.wikimedia.org/math/f/6/2/f623e75af30e62bbd73d6df5b50bb7b5.png)
consisting of
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)
documents each of length
![N\_{i}](//upload.wikimedia.org/math/f/d/d/fdddfb449333f2f69953de63c0075048.png):

\1. Choose ![\\theta \_{i}\\,\\sim \\,{\\mathrm {Dir}}(\\alpha
)](//upload.wikimedia.org/math/6/f/a/6faf6a9336e6913a28d8cf63d5d1def6.png),
where ![i\\in \\{1,\\dots
,M\\}](//upload.wikimedia.org/math/a/c/f/acfa7d3fa8b5dede007f1d23ff9ac34d.png)
and ![{\\mathrm {Dir}}(\\alpha
)](//upload.wikimedia.org/math/f/c/6/fc64b7c2a4a8f0f99e372f8637c049a4.png)
is the [Dirichlet
distribution](/wiki/Dirichlet_distribution "Dirichlet distribution") for
parameter ![\\alpha
](//upload.wikimedia.org/math/b/c/c/bccfc7022dfb945174d9bcebad2297bb.png)

\2. Choose ![\\phi \_{k}\\,\\sim \\,{\\mathrm {Dir}}(\\beta
)](//upload.wikimedia.org/math/5/5/b/55b1b9749eb302cba8aef2aab9010bc7.png),
where ![k\\in \\{1,\\dots
,K\\}](//upload.wikimedia.org/math/7/2/6/72642dd5cf05d3557823f432314382fe.png)

\3. For each of the word positions
![i,j](//upload.wikimedia.org/math/e/e/8/ee813f0ede8664a8049b1b6720f03b60.png),
where ![j\\in \\{1,\\dots
,N\_{i}\\}](//upload.wikimedia.org/math/8/0/a/80a2e5934a2633762e879836ede2bb05.png),
and ![i\\in \\{1,\\dots
,M\\}](//upload.wikimedia.org/math/a/c/f/acfa7d3fa8b5dede007f1d23ff9ac34d.png)

\(a) Choose a topic ![z\_{{i,j}}\\,\\sim \\,{\\mathrm
{Multinomial}}(\\theta
\_{i}).](//upload.wikimedia.org/math/3/9/4/394cf7f80243afcc5ecc6aa6ead8aabd.png)

\(b) Choose a word ![w\_{{i,j}}\\,\\sim \\,{\\mathrm
{Multinomial}}(\\phi
\_{{z\_{{i,j}}}})](//upload.wikimedia.org/math/3/b/e/3be9c1435a78875bf6f3efed40227464.png).

(Note that the [Multinomial
distribution](/wiki/Multinomial_distribution "Multinomial distribution")
here refers to the Multinomial with only one trial. It is formally
equivalent to the [categorical
distribution](/wiki/Categorical_distribution "Categorical distribution").)

The lengths
![N\_{i}](//upload.wikimedia.org/math/f/d/d/fdddfb449333f2f69953de63c0075048.png)
are treated as independent of all the other data generating variables
(![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png)
and
![z](//upload.wikimedia.org/math/f/b/a/fbade9e36a3f36d3d676c1b808451dd7.png)).
The subscript is often dropped, as in the plate diagrams shown here.

### Mathematical definition[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=3 "Edit section: Mathematical definition")]

A formal description of smoothed LDA is as follows:

Definition of variables in the model

Variable

Type

Meaning

![K](//upload.wikimedia.org/math/a/5/f/a5f3c6a11b03839d46af9fb43c97c188.png)

integer

number of topics (e.g. 50)

![V](//upload.wikimedia.org/math/5/2/0/5206560a306a2e085a437fd258eb57ce.png)

integer

number of words in the vocabulary (e.g. 50,000 or 1,000,000)

![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)

integer

number of documents

![N\_{{d=1\\dots
M}}](//upload.wikimedia.org/math/a/d/9/ad9b823748490cd0ee51ad6fd9383c99.png)

integer

number of words in document *d*

![N](//upload.wikimedia.org/math/8/d/9/8d9c307cb7f3c4a32822a51922d1ceaa.png)

integer

total number of words in all documents; sum of all
![N\_{d}](//upload.wikimedia.org/math/f/e/8/fe85e818d5015e7588479fdac71eb13e.png)
values, i.e. ![N=\\sum
\_{{d=1}}\^{{M}}N\_{d}](//upload.wikimedia.org/math/e/4/2/e427876d2830d1e0ecc16b697ce2cba7.png)

![\\alpha \_{{k=1\\dots
K}}](//upload.wikimedia.org/math/7/4/a/74a001677ca87c724ba6f8b2e959c5a8.png)

positive real

prior weight of topic *k* in a document; usually the same for all
topics; normally a number less than 1, e.g. 0.1, to prefer sparse topic
distributions, i.e. few topics per document

![{\\boldsymbol \\alpha
}](//upload.wikimedia.org/math/d/f/a/dfa582582277f03e01623324e7c7f2c2.png)

*K*-dimension vector of positive reals

collection of all ![\\alpha
\_{k}](//upload.wikimedia.org/math/5/b/4/5b40922222fead3802cf8f924d49bf21.png)
values, viewed as a single vector

![\\beta \_{{w=1\\dots
V}}](//upload.wikimedia.org/math/3/2/d/32d9797682e405469764186c14b32ccd.png)

positive real

prior weight of word *w* in a topic; usually the same for all words;
normally a number much less than 1, e.g. 0.001, to strongly prefer
sparse word distributions, i.e. few words per topic

![{\\boldsymbol \\beta
}](//upload.wikimedia.org/math/9/9/3/9935bd88ab5407d4e230ad1a02530f34.png)

*V*-dimension vector of positive reals

collection of all ![\\beta
\_{w}](//upload.wikimedia.org/math/9/e/b/9eb8bdc758d364851e74f2e1416791ac.png)
values, viewed as a single vector

![\\phi \_{{k=1\\dots K,w=1\\dots
V}}](//upload.wikimedia.org/math/6/f/e/6fec06698237aea6496f8b8e09e28c31.png)

probability (real number between 0 and 1)

probability of word *w* occurring in topic *k*

![{\\boldsymbol \\phi }\_{{k=1\\dots
K}}](//upload.wikimedia.org/math/6/9/8/698a911443287fea1e2ac4b91f671bb4.png)

*V*-dimension vector of probabilities, which must sum to 1

distribution of words in topic *k*

![\\theta \_{{d=1\\dots M,k=1\\dots
K}}](//upload.wikimedia.org/math/e/e/e/eeed5bc83fa361aa83c48057a9602459.png)

probability (real number between 0 and 1)

probability of topic *k* occurring in document *d* for a given word

![{\\boldsymbol \\theta }\_{{d=1\\dots
M}}](//upload.wikimedia.org/math/f/3/2/f323c7241e040ffd2761220fae7e5ce4.png)

*K*-dimension vector of probabilities, which must sum to 1

distribution of topics in document *d*

![z\_{{d=1\\dots M,w=1\\dots
N\_{d}}}](//upload.wikimedia.org/math/e/7/6/e7613f388218c3b921fda781409a7c27.png)

integer between 1 and *K*

identity of topic of word *w* in document *d*

![{\\mathbf
{Z}}](//upload.wikimedia.org/math/2/2/a/22a17bcbf054634f78c66a01ee83b6eb.png)

*N*-dimension vector of integers between 1 and *K*

identity of topic of all words in all documents

![w\_{{d=1\\dots M,w=1\\dots
N\_{d}}}](//upload.wikimedia.org/math/9/5/6/95683cf752f3cabef2060d1f9947bbc9.png)

integer between 1 and *V*

identity of word *w* in document *d*

![{\\mathbf
{W}}](//upload.wikimedia.org/math/b/7/6/b76e57cce219c9d21b9de2d59f035bdf.png)

*N*-dimension vector of integers between 1 and *V*

identity of all words in all documents

We can then mathematically describe the random variables as follows:

![{\\begin{array}{lcl}{\\boldsymbol \\phi }\_{{k=1\\dots K}}&\\sim
&\\operatorname {Dirichlet}\_{V}({\\boldsymbol \\beta
})\\\\{\\boldsymbol \\theta }\_{{d=1\\dots M}}&\\sim &\\operatorname
{Dirichlet}\_{K}({\\boldsymbol \\alpha })\\\\z\_{{d=1\\dots M,w=1\\dots
N\_{d}}}&\\sim &\\operatorname {Categorical}\_{K}({\\boldsymbol \\theta
}\_{d})\\\\w\_{{d=1\\dots M,w=1\\dots N\_{d}}}&\\sim &\\operatorname
{Categorical}\_{V}({\\boldsymbol \\phi
}\_{{z\_{{dw}}}})\\\\\\end{array}}](//upload.wikimedia.org/math/9/d/2/9d282094c6bea69b861ea247429b2eb9.png)

Inference[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=4 "Edit section: Inference")]
-----------------------------------------------------------------------------------------------------------------

See also: [Dirichlet-multinomial
distribution](/wiki/Dirichlet-multinomial_distribution "Dirichlet-multinomial distribution")

Learning the various distributions (the set of topics, their associated
word probabilities, the topic of each word, and the particular topic
mixture of each document) is a problem of [Bayesian
inference](/wiki/Bayesian_inference "Bayesian inference"). The original
paper used a [variational
Bayes](/wiki/Variational_Bayes "Variational Bayes") approximation of the
[posterior
distribution](/wiki/Posterior_distribution "Posterior distribution");^[[1]](#cite_note-blei2003-1)^
alternative inference techniques use [Gibbs
sampling](/wiki/Gibbs_sampling "Gibbs sampling")^[[3]](#cite_note-3)^
and [expectation
propagation](/wiki/Expectation_propagation "Expectation propagation").^[[4]](#cite_note-4)^

Following is the derivation of the equations for [collapsed Gibbs
sampling](/wiki/Collapsed_Gibbs_sampling "Collapsed Gibbs sampling"),
which means ![\\varphi
](//upload.wikimedia.org/math/3/5/3/3538eb9c84efdcbd130c4c953781cfdb.png)s
and ![\\theta
](//upload.wikimedia.org/math/5/0/d/50d91f80cbb8feda1d10e167107ad1ff.png)s
will be integrated out. For simplicity, in this derivation the documents
are all assumed to have the same length
![N\_{{}}](//upload.wikimedia.org/math/6/5/f/65fb61cc0eab8f326777d6e01739e90f.png).
The derivation is equally valid if the document lengths vary.

According to the model, the total probability of the model is:

![P({\\boldsymbol {W}},{\\boldsymbol {Z}},{\\boldsymbol {\\theta
}},{\\boldsymbol {\\varphi }};\\alpha ,\\beta )=\\prod
\_{{i=1}}\^{K}P(\\varphi \_{i};\\beta )\\prod \_{{j=1}}\^{M}P(\\theta
\_{j};\\alpha )\\prod \_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta
\_{j})P(W\_{{j,t}}|\\varphi
\_{{Z\_{{j,t}}}}),](//upload.wikimedia.org/math/f/e/1/fe1114adeb8d77b29f4c57967eb0baf9.png)

where the bold-font variables denote the vector version of the
variables. First of all, ![{\\boldsymbol {\\varphi
}}](//upload.wikimedia.org/math/5/c/5/5c526c139c01420b2bece529c57c8f92.png)
and ![{\\boldsymbol {\\theta
}}](//upload.wikimedia.org/math/7/9/e/79e960f60e6ba59a14e1ebc17b90ba9a.png)
need to be integrated out.

**Failed to parse(unknown function '\\begin'):
{\\begin{aligned}&P({\\boldsymbol {Z}},{\\boldsymbol {W}};\\alpha
,\\beta )=\\int \_{{{\\boldsymbol {\\theta }}}}\\int \_{{{\\boldsymbol
{\\varphi }}}}P({\\boldsymbol {W}},{\\boldsymbol {Z}},{\\boldsymbol
{\\theta }},{\\boldsymbol {\\varphi }};\\alpha ,\\beta
)\\,d{\\boldsymbol {\\varphi }}\\,d{\\boldsymbol {\\theta }}\\\\=&\\int
\_{{{\\boldsymbol {\\varphi }}}}\\prod \_{{i=1}}\^{K}P(\\varphi
\_{i};\\beta )\\prod \_{{j=1}}\^{M}\\prod
\_{{t=1}}\^{N}P(W\_{{j,t}}|\\varphi \_{{Z\_{{j,t}}}})\\,d{\\boldsymbol
{\\varphi }}\\int \_{{{\\boldsymbol {\\theta }}}}\\prod
\_{{j=1}}\^{M}P(\\theta \_{j};\\alpha )\\prod
\_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})\\,d{\\boldsymbol {\\theta
}}.\\end{aligned}}**

\
 All the ![\\theta
](//upload.wikimedia.org/math/5/0/d/50d91f80cbb8feda1d10e167107ad1ff.png)s
are independent to each other and the same to all the ![\\varphi
](//upload.wikimedia.org/math/3/5/3/3538eb9c84efdcbd130c4c953781cfdb.png)s.
So we can treat each ![\\theta
](//upload.wikimedia.org/math/5/0/d/50d91f80cbb8feda1d10e167107ad1ff.png)
and each ![\\varphi
](//upload.wikimedia.org/math/3/5/3/3538eb9c84efdcbd130c4c953781cfdb.png)
separately. We now focus only on the ![\\theta
](//upload.wikimedia.org/math/5/0/d/50d91f80cbb8feda1d10e167107ad1ff.png)
part.

![\\int \_{{{\\boldsymbol {\\theta }}}}\\prod \_{{j=1}}\^{M}P(\\theta
\_{j};\\alpha )\\prod \_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta
\_{j})d{\\boldsymbol {\\theta }}=\\prod \_{{j=1}}\^{M}\\int \_{{\\theta
\_{j}}}P(\\theta \_{j};\\alpha )\\prod
\_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})\\,d\\theta
\_{j}.](//upload.wikimedia.org/math/a/6/3/a63a222fa481fda480d5d72fca49eebd.png)

We can further focus on only one ![\\theta
](//upload.wikimedia.org/math/5/0/d/50d91f80cbb8feda1d10e167107ad1ff.png)
as the following:

![\\int \_{{\\theta \_{j}}}P(\\theta \_{j};\\alpha )\\prod
\_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})\\,d\\theta
\_{j}.](//upload.wikimedia.org/math/3/8/2/382b80c24ed2da30311c5ab21906a63d.png)

Actually, it is the hidden part of the model for the
![j\^{{th}}](//upload.wikimedia.org/math/1/b/5/1b5da877f0af7a43c8ca18315bd4fff3.png)
document. Now we replace the probabilities in the above equation by the
true distribution expression to write out the explicit equation.

**Failed to parse(unknown function '\\begin'): {\\begin{aligned}&\\int
\_{{\\theta \_{j}}}P(\\theta \_{j};\\alpha )\\prod
\_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})\\,d\\theta \_{j}=&\\int
\_{{\\theta \_{j}}}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}\\alpha \_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma
(\\alpha \_{i})}}\\prod \_{{i=1}}\^{K}\\theta \_{{j,i}}\^{{\\alpha
\_{i}-1}}\\prod \_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})\\,d\\theta
\_{j}.\\end{aligned}}**

\
 Let
![n\_{{j,r}}\^{i}](//upload.wikimedia.org/math/7/9/4/794d209d81019f0a798a6fbd015e2ac4.png)
be the number of word tokens in the
![j\^{{th}}](//upload.wikimedia.org/math/1/b/5/1b5da877f0af7a43c8ca18315bd4fff3.png)
document with the same word symbol (the
![r\^{{th}}](//upload.wikimedia.org/math/a/1/b/a1b7a52a84155477b21497ee4b36a972.png)
word in the vocabulary) assigned to the
![i\^{{th}}](//upload.wikimedia.org/math/f/a/f/faf3f005accabd7bf56d1732bfea75f8.png)
topic. So,
![n\_{{j,r}}\^{i}](//upload.wikimedia.org/math/7/9/4/794d209d81019f0a798a6fbd015e2ac4.png)
is three dimensional. If any of the three dimensions is not limited to a
specific value, we use a parenthesized point ![(\\cdot
)](//upload.wikimedia.org/math/f/3/4/f34dd6e523b75fa8408b81c9a95e84fb.png)
to denote. For example, ![n\_{{j,(\\cdot
)}}\^{i}](//upload.wikimedia.org/math/f/0/4/f04d948d0d2c1f1578f4800e7a9e53ce.png)
denotes the number of word tokens in the
![j\^{{th}}](//upload.wikimedia.org/math/1/b/5/1b5da877f0af7a43c8ca18315bd4fff3.png)
document assigned to the
![i\^{{th}}](//upload.wikimedia.org/math/f/a/f/faf3f005accabd7bf56d1732bfea75f8.png)
topic. Thus, the right most part of the above equation can be rewritten
as:

![\\prod \_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})=\\prod
\_{{i=1}}\^{K}\\theta \_{{j,i}}\^{{n\_{{j,(\\cdot
)}}\^{i}}}.](//upload.wikimedia.org/math/d/d/a/dda08e4b84c5ccf9ad3b5a49767835bb.png)

So the ![\\theta
\_{j}](//upload.wikimedia.org/math/2/5/b/25bf949511b43457cb41efb083c8dd38.png)
integration formula can be changed to:

**Failed to parse(unknown function '\\begin'): {\\begin{aligned}&\\int
\_{{\\theta \_{j}}}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}\\alpha \_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma
(\\alpha \_{i})}}\\prod \_{{i=1}}\^{K}\\theta \_{{j,i}}\^{{\\alpha
\_{i}-1}}\\prod \_{{i=1}}\^{K}\\theta \_{{j,i}}\^{{n\_{{j,(\\cdot
)}}\^{i}}}\\,d\\theta \_{j}\\\\=&\\int \_{{\\theta \_{j}}}{\\frac
{\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}\\alpha \_{i}{\\bigr )}}{\\prod
\_{{i=1}}\^{K}\\Gamma (\\alpha \_{i})}}\\prod \_{{i=1}}\^{K}\\theta
\_{{j,i}}\^{{n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i}-1}}\\,d\\theta
\_{j}.\\end{aligned}}**

\
 Clearly, the equation inside the integration has the same form as the
[Dirichlet
distribution](/wiki/Dirichlet_distribution "Dirichlet distribution").
According to the [Dirichlet
distribution](/wiki/Dirichlet_distribution "Dirichlet distribution"),

![\\int \_{{\\theta \_{j}}}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i}{\\bigr )}}{\\prod
\_{{i=1}}\^{K}\\Gamma (n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i})}}\\prod
\_{{i=1}}\^{K}\\theta \_{{j,i}}\^{{n\_{{j,(\\cdot )}}\^{i}+\\alpha
\_{i}-1}}\\,d\\theta
\_{j}=1.](//upload.wikimedia.org/math/1/7/c/17c731ee6f05fad4278e6ad6104dee32.png)

Thus,

**Failed to parse(unknown function '\\begin'): {\\begin{aligned}&\\int
\_{{\\theta \_{j}}}P(\\theta \_{j};\\alpha )\\prod
\_{{t=1}}\^{N}P(Z\_{{j,t}}|\\theta \_{j})\\,d\\theta \_{j}=\\int
\_{{\\theta \_{j}}}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}\\alpha \_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma
(\\alpha \_{i})}}\\prod \_{{i=1}}\^{K}\\theta
\_{{j,i}}\^{{n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i}-1}}\\,d\\theta
\_{j}\\\\=&{\\frac {\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}\\alpha
\_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma (\\alpha \_{i})}}{\\frac
{\\prod \_{{i=1}}\^{K}\\Gamma (n\_{{j,(\\cdot )}}\^{i}+\\alpha
\_{i})}{\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}n\_{{j,(\\cdot
)}}\^{i}+\\alpha \_{i}{\\bigr )}}}\\int \_{{\\theta \_{j}}}{\\frac
{\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}n\_{{j,(\\cdot )}}\^{i}+\\alpha
\_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma (n\_{{j,(\\cdot
)}}\^{i}+\\alpha \_{i})}}\\prod \_{{i=1}}\^{K}\\theta
\_{{j,i}}\^{{n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i}-1}}\\,d\\theta
\_{j}\\\\=&{\\frac {\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}\\alpha
\_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma (\\alpha \_{i})}}{\\frac
{\\prod \_{{i=1}}\^{K}\\Gamma (n\_{{j,(\\cdot )}}\^{i}+\\alpha
\_{i})}{\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}n\_{{j,(\\cdot
)}}\^{i}+\\alpha \_{i}{\\bigr )}}}.\\end{aligned}}**

\
 Now we turn our attentions to the ![{\\boldsymbol {\\varphi
}}](//upload.wikimedia.org/math/5/c/5/5c526c139c01420b2bece529c57c8f92.png)
part. Actually, the derivation of the ![{\\boldsymbol {\\varphi
}}](//upload.wikimedia.org/math/5/c/5/5c526c139c01420b2bece529c57c8f92.png)
part is very similar to the ![{\\boldsymbol {\\theta
}}](//upload.wikimedia.org/math/7/9/e/79e960f60e6ba59a14e1ebc17b90ba9a.png)
part. Here we only list the steps of the derivation:

**Failed to parse(unknown function '\\begin'): {\\begin{aligned}&\\int
\_{{{\\boldsymbol {\\varphi }}}}\\prod \_{{i=1}}\^{K}P(\\varphi
\_{i};\\beta )\\prod \_{{j=1}}\^{M}\\prod
\_{{t=1}}\^{N}P(W\_{{j,t}}|\\varphi \_{{Z\_{{j,t}}}})\\,d{\\boldsymbol
{\\varphi }}\\\\=&\\prod \_{{i=1}}\^{K}\\int \_{{\\varphi
\_{i}}}P(\\varphi \_{i};\\beta )\\prod \_{{j=1}}\^{M}\\prod
\_{{t=1}}\^{N}P(W\_{{j,t}}|\\varphi \_{{Z\_{{j,t}}}})\\,d\\varphi
\_{i}\\\\=&\\prod \_{{i=1}}\^{K}\\int \_{{\\varphi \_{i}}}{\\frac
{\\Gamma {\\bigl (}\\sum \_{{r=1}}\^{V}\\beta \_{r}{\\bigr )}}{\\prod
\_{{r=1}}\^{V}\\Gamma (\\beta \_{r})}}\\prod \_{{r=1}}\^{V}\\varphi
\_{{i,r}}\^{{\\beta \_{r}-1}}\\prod \_{{r=1}}\^{V}\\varphi
\_{{i,r}}\^{{n\_{{(\\cdot ),r}}\^{i}}}\\,d\\varphi \_{i}\\\\=&\\prod
\_{{i=1}}\^{K}\\int \_{{\\varphi \_{i}}}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{r=1}}\^{V}\\beta \_{r}{\\bigr )}}{\\prod \_{{r=1}}\^{V}\\Gamma
(\\beta \_{r})}}\\prod \_{{r=1}}\^{V}\\varphi \_{{i,r}}\^{{n\_{{(\\cdot
),r}}\^{i}+\\beta \_{r}-1}}\\,d\\varphi \_{i}\\\\=&\\prod
\_{{i=1}}\^{K}{\\frac {\\Gamma {\\bigl (}\\sum \_{{r=1}}\^{V}\\beta
\_{r}{\\bigr )}}{\\prod \_{{r=1}}\^{V}\\Gamma (\\beta \_{r})}}{\\frac
{\\prod \_{{r=1}}\^{V}\\Gamma (n\_{{(\\cdot ),r}}\^{i}+\\beta
\_{r})}{\\Gamma {\\bigl (}\\sum \_{{r=1}}\^{V}n\_{{(\\cdot
),r}}\^{i}+\\beta \_{r}{\\bigr )}}}.\\end{aligned}}**

\
 For clarity, here we write down the final equation with both
![{\\boldsymbol {\\phi
}}](//upload.wikimedia.org/math/4/1/2/41241deb334ac2314e64e4d79651ba74.png)
and ![{\\boldsymbol {\\theta
}}](//upload.wikimedia.org/math/7/9/e/79e960f60e6ba59a14e1ebc17b90ba9a.png)
integrated out:

**Failed to parse(unknown function '\\begin'):
{\\begin{aligned}&P({\\boldsymbol {Z}},{\\boldsymbol {W}};\\alpha
,\\beta )\\\\=&\\prod \_{{j=1}}\^{M}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}\\alpha \_{i}{\\bigr )}}{\\prod \_{{i=1}}\^{K}\\Gamma
(\\alpha \_{i})}}{\\frac {\\prod \_{{i=1}}\^{K}\\Gamma (n\_{{j,(\\cdot
)}}\^{i}+\\alpha \_{i})}{\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i}{\\bigr )}}}\\times
\\prod \_{{i=1}}\^{K}{\\frac {\\Gamma {\\bigl (}\\sum
\_{{r=1}}\^{V}\\beta \_{r}{\\bigr )}}{\\prod \_{{r=1}}\^{V}\\Gamma
(\\beta \_{r})}}{\\frac {\\prod \_{{r=1}}\^{V}\\Gamma (n\_{{(\\cdot
),r}}\^{i}+\\beta \_{r})}{\\Gamma {\\bigl (}\\sum
\_{{r=1}}\^{V}n\_{{(\\cdot ),r}}\^{i}+\\beta \_{r}{\\bigr
)}}}.\\end{aligned}}**

\
 The goal of Gibbs Sampling here is to approximate the distribution of
![P({\\boldsymbol {Z}}|{\\boldsymbol {W}};\\alpha ,\\beta
)](//upload.wikimedia.org/math/1/d/a/1da3b4c220c648c4605e5aaf0ada3ab0.png).
Since ![P({\\boldsymbol {W}};\\alpha ,\\beta
)](//upload.wikimedia.org/math/b/a/3/ba3b911aa0d8af53deb14484105f74f5.png)
is invariable for any of Z, Gibbs Sampling equations can be derived from
![P({\\boldsymbol {Z}},{\\boldsymbol {W}};\\alpha ,\\beta
)](//upload.wikimedia.org/math/2/f/7/2f7982b78430a22b1617df3a7faa54ad.png)
directly. The key point is to derive the following conditional
probability:

![P(Z\_{{(m,n)}}|{\\boldsymbol {Z\_{{-(m,n)}}}},{\\boldsymbol
{W}};\\alpha ,\\beta )={\\frac {P(Z\_{{(m,n)}},{\\boldsymbol
{Z\_{{-(m,n)}}}},{\\boldsymbol {W}};\\alpha ,\\beta )}{P({\\boldsymbol
{Z\_{{-(m,n)}}}},{\\boldsymbol {W}};\\alpha ,\\beta
)}},](//upload.wikimedia.org/math/8/3/f/83f0ae5fe0e20806929e6b7fac65c852.png)

where
![Z\_{{(m,n)}}](//upload.wikimedia.org/math/4/8/3/483f9db5b6b49854099ede463b893165.png)
denotes the
![Z](//upload.wikimedia.org/math/2/1/c/21c2e59531c8710156d34a3c30ac81d5.png)
hidden variable of the
![n\^{{th}}](//upload.wikimedia.org/math/a/0/e/a0e136f5efca1d4258a9e43add95712f.png)
word token in the
![m\^{{th}}](//upload.wikimedia.org/math/b/2/7/b2716e0672a0f3f07599fcdf54abb375.png)
document. And further we assume that the word symbol of it is the
![v\^{{th}}](//upload.wikimedia.org/math/b/2/b/b2bbcf005705d94772ba0ff20f4b0482.png)
word in the vocabulary. ![{\\boldsymbol
{Z\_{{-(m,n)}}}}](//upload.wikimedia.org/math/c/9/1/c916670f6000f9104b9563b29ed6ecb8.png)
denotes all the
![Z](//upload.wikimedia.org/math/2/1/c/21c2e59531c8710156d34a3c30ac81d5.png)s
but
![Z\_{{(m,n)}}](//upload.wikimedia.org/math/4/8/3/483f9db5b6b49854099ede463b893165.png).
Note that Gibbs Sampling needs only to sample a value for
![Z\_{{(m,n)}}](//upload.wikimedia.org/math/4/8/3/483f9db5b6b49854099ede463b893165.png),
according to the above probability, we do not need the exact value of
![P(Z\_{{m,n}}|{\\boldsymbol {Z\_{{-(m,n)}}}},{\\boldsymbol {W}};\\alpha
,\\beta
)](//upload.wikimedia.org/math/1/2/6/126e2114e7979de80d516934c90c80c9.png)
but the ratios among the probabilities that
![Z\_{{(m,n)}}](//upload.wikimedia.org/math/4/8/3/483f9db5b6b49854099ede463b893165.png)
can take value. So, the above equation can be simplified as:

**Failed to parse(unknown function '\\begin'):
{\\begin{aligned}&P(Z\_{{(m,n)}}=k|{\\boldsymbol
{Z\_{{-(m,n)}}}},{\\boldsymbol {W}};\\alpha ,\\beta )\\\\\\propto
&P(Z\_{{(m,n)}}=k,{\\boldsymbol {Z\_{{-(m,n)}}}},{\\boldsymbol
{W}};\\alpha ,\\beta )\\\\=&\\left({\\frac {\\Gamma \\left(\\sum
\_{{i=1}}\^{K}\\alpha \_{i}\\right)}{\\prod \_{{i=1}}\^{K}\\Gamma
(\\alpha \_{i})}}\\right)\^{M}\\prod \_{{j\\neq m}}{\\frac {\\prod
\_{{i=1}}\^{K}\\Gamma (n\_{{j,(\\cdot )}}\^{i}+\\alpha \_{i})}{\\Gamma
{\\bigl (}\\sum \_{{i=1}}\^{K}n\_{{j,(\\cdot )}}\^{i}+\\alpha
\_{i}{\\bigr )}}}\\\\&\\times \\left({\\frac {\\Gamma {\\bigl (}\\sum
\_{{r=1}}\^{V}\\beta \_{r}{\\bigr )}}{\\prod \_{{r=1}}\^{V}\\Gamma
(\\beta \_{r})}}\\right)\^{K}\\prod \_{{i=1}}\^{K}\\prod \_{{r\\neq
v}}\\Gamma (n\_{{(\\cdot ),r}}\^{i}+\\beta \_{r})\\\\&\\times {\\frac
{\\prod \_{{i=1}}\^{K}\\Gamma (n\_{{m,(\\cdot )}}\^{i}+\\alpha
\_{i})}{\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}n\_{{m,(\\cdot
)}}\^{i}+\\alpha \_{i}{\\bigr )}}}\\prod \_{{i=1}}\^{K}{\\frac {\\Gamma
(n\_{{(\\cdot ),v}}\^{i}+\\beta \_{v})}{\\Gamma {\\bigl (}\\sum
\_{{r=1}}\^{V}n\_{{(\\cdot ),r}}\^{i}+\\beta \_{r}{\\bigr
)}}}\\\\\\propto &{\\frac {\\prod \_{{i=1}}\^{K}\\Gamma (n\_{{m,(\\cdot
)}}\^{i}+\\alpha \_{i})}{\\Gamma {\\bigl (}\\sum
\_{{i=1}}\^{K}n\_{{m,(\\cdot )}}\^{i}+\\alpha \_{i}{\\bigr )}}}\\prod
\_{{i=1}}\^{K}{\\frac {\\Gamma (n\_{{(\\cdot ),v}}\^{i}+\\beta
\_{v})}{\\Gamma {\\bigl (}\\sum \_{{r=1}}\^{V}n\_{{(\\cdot
),r}}\^{i}+\\beta \_{r}{\\bigr )}}}.\\end{aligned}}**

\
 Finally, let
![n\_{{j,r}}\^{{i,-(m,n)}}](//upload.wikimedia.org/math/6/b/0/6b084e60f16a337c39f7ab3690355729.png)
be the same meaning as
![n\_{{j,r}}\^{i}](//upload.wikimedia.org/math/7/9/4/794d209d81019f0a798a6fbd015e2ac4.png)
but with the
![Z\_{{(m,n)}}](//upload.wikimedia.org/math/4/8/3/483f9db5b6b49854099ede463b893165.png)
excluded. The above equation can be further simplified by treating terms
not dependent on
![k](//upload.wikimedia.org/math/8/c/e/8ce4b16b22b58894aa86c421e8759df3.png)
as constants:

**Failed to parse(unknown function '\\begin'): {\\begin{aligned}\\propto
&{\\frac {\\prod \_{{i\\neq k}}\\Gamma (n\_{{m,(\\cdot
)}}\^{{i,-(m,n)}}+\\alpha \_{i})}{\\Gamma {\\bigl (}(\\sum
\_{{i=1}}\^{K}n\_{{m,(\\cdot )}}\^{{i,-(m,n)}}+\\alpha \_{i})+1{\\bigr
)}}}\\prod \_{{i\\neq k}}{\\frac {\\Gamma (n\_{{(\\cdot
),v}}\^{{i,-(m,n)}}+\\beta \_{v})}{\\Gamma {\\bigl (}\\sum
\_{{r=1}}\^{V}n\_{{(\\cdot ),r}}\^{{i,-(m,n)}}+\\beta \_{r}{\\bigr
)}}}\\\\\\times &\\Gamma (n\_{{m,(\\cdot )}}\^{{k,-(m,n)}}+\\alpha
\_{k}+1){\\frac {\\Gamma (n\_{{(\\cdot ),v}}\^{{k,-(m,n)}}+\\beta
\_{v}+1)}{\\Gamma {\\bigl (}(\\sum \_{{r=1}}\^{V}n\_{{(\\cdot
),r}}\^{{k,-(m,n)}}+\\beta \_{r})+1{\\bigr )}}}\\\\\\propto &{\\frac
{\\Gamma (n\_{{m,(\\cdot )}}\^{{k,-(m,n)}}+\\alpha \_{k}+1)}{\\Gamma
{\\bigl (}(\\sum \_{{i=1}}\^{K}n\_{{m,(\\cdot )}}\^{{i,-(m,n)}}+\\alpha
\_{i})+1{\\bigr )}}}{\\frac {\\Gamma (n\_{{(\\cdot
),v}}\^{{k,-(m,n)}}+\\beta \_{v}+1)}{\\Gamma {\\bigl (}(\\sum
\_{{r=1}}\^{V}n\_{{(\\cdot ),r}}\^{{k,-(m,n)}}+\\beta \_{r})+1{\\bigr
)}}}\\\\=&{\\frac {\\Gamma (n\_{{m,(\\cdot )}}\^{{k,-(m,n)}}+\\alpha
\_{k}){\\bigl (}n\_{{m,(\\cdot )}}\^{{k,-(m,n)}}+\\alpha \_{k}{\\bigr
)}}{\\Gamma {\\bigl (}\\sum \_{{i=1}}\^{K}n\_{{m,(\\cdot
)}}\^{{i,-(m,n)}}+\\alpha \_{i}{\\bigr )}{\\bigl (}\\sum
\_{{i=1}}\^{K}n\_{{m,(\\cdot )}}\^{{i,-(m,n)}}+\\alpha \_{i}{\\bigr
)}}}{\\frac {\\Gamma {\\bigl (}n\_{{(\\cdot ),v}}\^{{k,-(m,n)}}+\\beta
\_{v}{\\bigr )}{\\bigl (}n\_{{(\\cdot ),v}}\^{{k,-(m,n)}}+\\beta
\_{v}{\\bigr )}}{\\Gamma {\\bigl (}\\sum \_{{r=1}}\^{V}n\_{{(\\cdot
),r}}\^{{k,-(m,n)}}+\\beta \_{r}{\\bigr )}{\\bigl (}\\sum
\_{{r=1}}\^{V}n\_{{(\\cdot ),r}}\^{{k,-(m,n)}}+\\beta
\_{r})}}\\\\\\propto &{\\frac {{\\bigl (}n\_{{m,(\\cdot
)}}\^{{k,-(m,n)}}+\\alpha \_{k}{\\bigr )}}{{\\bigl (}\\sum
\_{{i=1}}\^{K}n\_{{m,(\\cdot )}}\^{{i,-(m,n)}}+\\alpha \_{i}{\\bigr
)}}}{\\frac {{\\bigl (}n\_{{(\\cdot ),v}}\^{{k,-(m,n)}}+\\beta
\_{v}{\\bigr )}}{{\\bigl (}\\sum \_{{r=1}}\^{V}n\_{{(\\cdot
),r}}\^{{k,-(m,n)}}+\\beta \_{r})}}\\\\\\propto &{\\bigl
(}n\_{{m,(\\cdot )}}\^{{k,-(m,n)}}+\\alpha \_{k}{\\bigr )}{\\frac
{{\\bigl (}n\_{{(\\cdot ),v}}\^{{k,-(m,n)}}+\\beta \_{v}{\\bigr
)}}{{\\bigl (}\\sum \_{{r=1}}\^{V}n\_{{(\\cdot
),r}}\^{{k,-(m,n)}}+\\beta \_{r})}}.\\end{aligned}}**

\
 Note that the same formula is derived in the article on the
[Dirichlet-multinomial
distribution](/wiki/Dirichlet-multinomial_distribution#A_combined_example:_LDA_topic_models "Dirichlet-multinomial distribution"),
as part of a more general discussion of integrating [Dirichlet
distribution](/wiki/Dirichlet_distribution "Dirichlet distribution")
priors out of a [Bayesian
network](/wiki/Bayesian_network "Bayesian network").

Applications, extensions and similar techniques[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=5 "Edit section: Applications, extensions and similar techniques")]
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Topic modeling is a classic problem in [information
retrieval](/wiki/Information_retrieval "Information retrieval"). Related
models and techniques are, among others, [latent semantic
indexing](/wiki/Latent_semantic_indexing "Latent semantic indexing"),
[independent component
analysis](/wiki/Independent_component_analysis "Independent component analysis"),
[probabilistic latent semantic indexing](/wiki/PLSI "PLSI"),
[non-negative matrix
factorization](/wiki/Non-negative_matrix_factorization "Non-negative matrix factorization"),
and [Gamma-Poisson
distribution](/wiki/Gamma-Poisson_distribution "Gamma-Poisson distribution").

The LDA model is highly modular and can therefore be easily extended.
The main field of interest is modeling relations between topics. This is
achieved by using another distribution on the simplex instead of the
Dirichlet. The Correlated Topic Model^[[5]](#cite_note-5)^ follows this
approach, inducing a correlation structure between topics by using the
[logistic normal
distribution](/wiki/Logistic_normal_distribution "Logistic normal distribution")
instead of the Dirichlet. Another extension is the hierarchical LDA
(hLDA),^[[6]](#cite_note-6)^ where topics are joined together in a
hierarchy by using the nested [Chinese restaurant
process](/wiki/Chinese_restaurant_process "Chinese restaurant process").
LDA can also be extended to a corpus in which a document includes two
types of information (e.g., words and names), as in the [LDA-dual
model](/w/index.php?title=LDA-dual_model&action=edit&redlink=1 "LDA-dual model (page does not exist)").^[[7]](#cite_note-7)^
Supervised versions of LDA include L-LDA, which has been found to
perform better than SVM under certain conditions.^[[8]](#cite_note-8)^
Nonparametric extensions of LDA include the [Hierarchical Dirichlet
process](/wiki/Hierarchical_Dirichlet_process "Hierarchical Dirichlet process")
mixture model, which allows the number of topics to be unbounded and
learnt from data and the [Nested Chinese Restaurant
Process](/w/index.php?title=Nested_Chinese_Restaurant_Process&action=edit&redlink=1 "Nested Chinese Restaurant Process (page does not exist)")
which allows topics to be arranged in a hierarchy whose structure is
learnt from data.

As noted earlier, PLSA is similar to LDA. The LDA model is essentially
the Bayesian version of PLSA model. The Bayesian formulation tends to
perform better on small datasets because Bayesian methods can avoid
overfitting the data. In a very large dataset, the results are probably
the same. One difference is that PLSA uses a variable
![d](//upload.wikimedia.org/math/8/2/7/8277e0910d750195b448797616e091ad.png)
to represent a document in the training set. So in PLSA, when presented
with a document the model hasn't seen before, we fix ![\\Pr(w\\mid
z)](//upload.wikimedia.org/math/3/7/8/378ec7adbc08b8d529f92de4154d4a19.png)—the
probability of words under topics—to be that learned from the training
set and use the same EM algorithm to infer ![\\Pr(z\\mid
d)](//upload.wikimedia.org/math/d/4/1/d418ac8dd2c8289994647268c30f7a5f.png)—the
topic distribution under
![d](//upload.wikimedia.org/math/8/2/7/8277e0910d750195b448797616e091ad.png).
Blei argues that this step is cheating because you are essentially
refitting the model to the new data.

Variations on LDA have been used to automatically put natural images
into categories, such as "bedroom" or "forest", by treating an image as
a document, and small patches of the image as
words;^[[9]](#cite_note-9)^ one of the variations is called [Spatial
Latent Dirichlet
Allocation](/w/index.php?title=Spatial_Latent_Dirichlet_Allocation&action=edit&redlink=1 "Spatial Latent Dirichlet Allocation (page does not exist)").^[[10]](#cite_note-10)^

See also[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=6 "Edit section: See also")]
---------------------------------------------------------------------------------------------------------------

-   [Pachinko
    allocation](/wiki/Pachinko_allocation "Pachinko allocation")
-   [tf-idf](/wiki/Tf-idf "Tf-idf")

Notes[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=7 "Edit section: Notes")]
---------------------------------------------------------------------------------------------------------

1.  \^ [^***a***^](#cite_ref-blei2003_1-0)
    [^***b***^](#cite_ref-blei2003_1-1) Blei, David M.; Ng, Andrew Y.;
    [Jordan, Michael I](/wiki/Michael_I._Jordan "Michael I. Jordan")
    (January 2003). ["Latent Dirichlet
    allocation"](http://jmlr.csail.mit.edu/papers/v3/blei03a.html). In
    Lafferty, John. *[Journal of Machine Learning
    Research](/wiki/Journal_of_Machine_Learning_Research "Journal of Machine Learning Research")*
    **3** (4–5): *pp.* 993–1022.
    [doi](/wiki/Digital_object_identifier "Digital object identifier"):[10.1162/jmlr.2003.3.4-5.993](http://dx.doi.org/10.1162%2Fjmlr.2003.3.4-5.993).
2.  **[\^](#cite_ref-2)** Girolami, Mark; Kaban, A. (2003). ["On an
    Equivalence between PLSI and
    LDA"](http://www.cs.bham.ac.uk/~axk/sigir2003_mgak.pdf). Proceedings
    of SIGIR 2003. New York: Association for Computing Machinery.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [1-58113-646-3](/wiki/Special:BookSources/1-58113-646-3 "Special:BookSources/1-58113-646-3").
    Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
3.  **[\^](#cite_ref-3)** Griffiths, Thomas L.; Steyvers, Mark (April 6,
    2004). ["Finding scientific
    topics"](//www.ncbi.nlm.nih.gov/pmc/articles/PMC387300).
    *Proceedings of the National Academy of Sciences* **101** (Suppl.
    1): 5228–5235.
    [doi](/wiki/Digital_object_identifier "Digital object identifier"):[10.1073/pnas.0307752101](http://dx.doi.org/10.1073%2Fpnas.0307752101).
    [PMC](/wiki/PubMed_Central "PubMed Central")
    [387300](//www.ncbi.nlm.nih.gov/pmc/articles/PMC387300).
    [PMID](/wiki/PubMed_Identifier "PubMed Identifier")
    [14872004](//www.ncbi.nlm.nih.gov/pubmed/14872004).
4.  **[\^](#cite_ref-4)** Minka, Thomas; Lafferty, John (2002).
    ["Expectation-propagation for the generative aspect
    model"](https://research.microsoft.com/~minka/papers/aspect/minka-aspect.pdf).
    Proceedings of the 18th Conference on Uncertainty in Artificial
    Intelligence. San Francisco, CA: Morgan Kaufmann.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [1-55860-897-4](/wiki/Special:BookSources/1-55860-897-4 "Special:BookSources/1-55860-897-4").
    Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
5.  **[\^](#cite_ref-5)** Blei, David M.; Lafferty, John D. (2006).
    ["Correlated topic
    models"](http://www.cs.cmu.edu/~lafferty/pub/ctm.pdf). *Advances in
    Neural Information Processing Systems* **18**. Cite uses deprecated
    parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
6.  **[\^](#cite_ref-6)** Blei, David M.; [Jordan, Michael
    I.](/wiki/Michael_I._Jordan "Michael I. Jordan"); Griffiths, Thomas
    L.; Tenenbaum, Joshua B (2004). ["Hierarchical Topic Models and the
    Nested Chinese Restaurant
    Process"](http://cocosci.berkeley.edu/tom/papers/ncrp.pdf). Advances
    in Neural Information Processing Systems 16: Proceedings of the 2003
    Conference. MIT Press.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [0-262-20152-6](/wiki/Special:BookSources/0-262-20152-6 "Special:BookSources/0-262-20152-6").
    Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
7.  **[\^](#cite_ref-7)** Shu, Liangcai; Long, Bo; Meng, Weiyi (2009).
    ["A Latent Topic Model for Complete Entity
    Resolution"](http://www.cs.binghamton.edu/~meng/pub.d/icde09-LatentTopic.pdf).
    25th IEEE International Conference on Data Engineering (ICDE 2009).
    Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
8.  **[\^](#cite_ref-8)** Quercia, Daniele; Harry Askham, Jon Crowcroft
    (2012). ["TweetLDA: Supervised Topic Classification and Link
    Prediction in
    Twitter"](http://www.cl.cam.ac.uk/~dq209/publications/websci_tweetlda.pdf).
    *ACM WebSci*. Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
9.  **[\^](#cite_ref-9)** Li, Fei-Fei; Perona, Pietro. "A Bayesian
    Hierarchical Model for Learning Natural Scene Categories".
    *Proceedings of the 2005 IEEE Computer Society Conference on
    Computer Vision and Pattern Recognition (CVPR'05)* **2**: 524–531.
    Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
10. **[\^](#cite_ref-10)** Wang, Xiaogang; Grimson, Eric (2007).
    ["Spatial Latent Dirichlet
    Allocation"](http://machinelearning.wustl.edu/mlpapers/paper_files/NIPS2007_102.pdf).
    *Proceedings of Neural Information Processing Systems Conference
    (NIPS)*. Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))

External links[[edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit&section=8 "Edit section: External links")]
---------------------------------------------------------------------------------------------------------------------------

-   Extremely useful lecture for understanding LDA [LDA and Topic
    Modelling Video Lecture by David
    Blei](http://videolectures.net/mlss09uk_blei_tm/)
-   [D. Mimno's LDA
    Bibliography](http://mimno.infosci.cornell.edu/topics.html) An
    exhaustive list of LDA-related resources (incl. papers and some
    implementations)
-   [Gensim](/wiki/Gensim "Gensim") Python+[NumPy](/wiki/NumPy "NumPy")
    implementation of LDA for input larger than the available RAM.
-   [topicmodels](http://cran.r-project.org/web/packages/topicmodels/index.html)
    and [lda](http://cran.r-project.org/web/packages/lda/index.html) are
    two [R](/wiki/R_(programming_language) "R (programming language)")
    packages for LDA analysis.
-   ["Text Mining with R" including LDA
    methods](http://www.r-bloggers.com/RUG/2010/10/285/), video
    presentation to the October 2011 meeting of the Los Angeles R users
    group
-   [MALLET](http://mallet.cs.umass.edu/index.php) Open source
    Java-based package from the University of Massachusetts-Amherst for
    topic modeling with LDA, also has an independently developed GUI,
    the [Topic Modeling
    Tool](http://code.google.com/p/topic-modeling-tool/)
-   [LDA in
    Mahout](https://cwiki.apache.org/confluence/display/MAHOUT/Latent+Dirichlet+Allocation)
    implementation of LDA using [MapReduce](/wiki/MapReduce "MapReduce")
    on the [Hadoop](/wiki/Hadoop "Hadoop") platform
-   [Perl implementation of
    LDA](http://www.people.fas.harvard.edu/~ptoulis/code/LDA_Perl.zip) A
    non-optimized implementation of the LDA model in
    [Perl](/wiki/Perl "Perl"), with documentation
-   [Latent Dirichlet Allocation (LDA) Tutorial for the Infer.NET
    Machine Computing
    Framework](http://research.microsoft.com/en-us/um/cambridge/projects/infernet/docs/Latent%20Dirichlet%20Allocation.aspx)
    Microsoft Research C\# Machine Learning Framework

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=Latent\_Dirichlet\_allocation&oldid=594344098](http://en.wikipedia.org/w/index.php?title=Latent_Dirichlet_allocation&oldid=594344098)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Statistical natural language
    processing](/wiki/Category:Statistical_natural_language_processing "Category:Statistical natural language processing")
-   [Latent variable
    models](/wiki/Category:Latent_variable_models "Category:Latent variable models")
-   [Probabilistic
    models](/wiki/Category:Probabilistic_models "Category:Probabilistic models")

Hidden categories:

-   [Pages containing cite templates with deprecated
    parameters](/wiki/Category:Pages_containing_cite_templates_with_deprecated_parameters "Category:Pages containing cite templates with deprecated parameters")
-   [All articles with unsourced
    statements](/wiki/Category:All_articles_with_unsourced_statements "Category:All articles with unsourced statements")
-   [Articles with unsourced statements from September
    2013](/wiki/Category:Articles_with_unsourced_statements_from_September_2013 "Category:Articles with unsourced statements from September 2013")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Latent+Dirichlet+allocation&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Latent+Dirichlet+allocation "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Latent_Dirichlet_allocation "View the content page [c]")
-   [Talk](/wiki/Talk:Latent_Dirichlet_allocation "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Latent_Dirichlet_allocation)
-   [Edit](/w/index.php?title=Latent_Dirichlet_allocation&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Latent_Dirichlet_allocation&action=history "Past versions of this page [h]")

### Actions[](#)

### Search

![Search](//bits.wikimedia.org/static-1.23wmf12/skins/vector/images/search-ltr.png?303-4)

[](/wiki/Main_Page "Visit the main page")

### Navigation

-   [Main page](/wiki/Main_Page "Visit the main page [z]")
-   [Contents](/wiki/Portal:Contents "Guides to browsing Wikipedia")
-   [Featured
    content](/wiki/Portal:Featured_content "Featured content – the best of Wikipedia")
-   [Current
    events](/wiki/Portal:Current_events "Find background information on current events")
-   [Random article](/wiki/Special:Random "Load a random article [x]")
-   [Donate to
    Wikipedia](https://donate.wikimedia.org/wiki/Special:FundraiserRedirector?utm_source=donate&utm_medium=sidebar&utm_campaign=C13_en.wikipedia.org&uselang=en "Support us")
-   [Wikimedia Shop](//shop.wikimedia.org "Visit the Wikimedia Shop")

### Interaction

-   [Help](/wiki/Help:Contents "Guidance on how to use and edit Wikipedia")
-   [About Wikipedia](/wiki/Wikipedia:About "Find out about Wikipedia")
-   [Community
    portal](/wiki/Wikipedia:Community_portal "About the project, what you can do, where to find things")
-   [Recent
    changes](/wiki/Special:RecentChanges "A list of recent changes in the wiki [r]")
-   [Contact page](//en.wikipedia.org/wiki/Wikipedia:Contact_us)

### Tools

-   [What links
    here](/wiki/Special:WhatLinksHere/Latent_Dirichlet_allocation "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Latent_Dirichlet_allocation "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Latent_Dirichlet_allocation&oldid=594344098 "Permanent link to this revision of the page")
-   [Page
    information](/w/index.php?title=Latent_Dirichlet_allocation&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q269236 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Latent_Dirichlet_allocation&id=594344098 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Latent+Dirichlet+allocation)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Latent+Dirichlet+allocation&oldid=594344098&writer=rl)
-   [Printable
    version](/w/index.php?title=Latent_Dirichlet_allocation&printable=yes "Printable version of this page [p]")

### Languages

-   [Deutsch](//de.wikipedia.org/wiki/Latent_Dirichlet_Allocation "Latent Dirichlet Allocation – German")
-   [Español](//es.wikipedia.org/wiki/Latent_Dirichlet_Allocation "Latent Dirichlet Allocation – Spanish")
-   [فارسی](//fa.wikipedia.org/wiki/%D8%AA%D8%AE%D8%B5%DB%8C%D8%B5_%D9%BE%D9%86%D9%87%D8%A7%D9%86_%D8%AF%DB%8C%D8%B1%DB%8C%DA%A9%D9%84%D9%87 "تخصیص پنهان دیریکله – Persian")
-   [Français](//fr.wikipedia.org/wiki/Allocation_de_Dirichlet_latente "Allocation de Dirichlet latente – French")
-   [한국어](//ko.wikipedia.org/wiki/%EC%9E%A0%EC%9E%AC_%EB%94%94%EB%A6%AC%ED%81%B4%EB%A0%88_%ED%95%A0%EB%8B%B9 "잠재 디리클레 할당 – Korean")
-   [Русский](//ru.wikipedia.org/wiki/%D0%9B%D0%B0%D1%82%D0%B5%D0%BD%D1%82%D0%BD%D0%BE%D0%B5_%D1%80%D0%B0%D0%B7%D0%BC%D0%B5%D1%89%D0%B5%D0%BD%D0%B8%D0%B5_%D0%94%D0%B8%D1%80%D0%B8%D1%85%D0%BB%D0%B5 "Латентное размещение Дирихле – Russian")
-   [中文](//zh.wikipedia.org/wiki/%E9%9A%90%E5%90%AB%E7%8B%84%E5%88%A9%E5%85%8B%E9%9B%B7%E5%88%86%E5%B8%83 "隐含狄利克雷分布 – Chinese")
-   [Edit
    links](//www.wikidata.org/wiki/Q269236#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 7 February 2014 at 09:12.\
-   Text is available under the [Creative Commons Attribution-ShareAlike
    License](//en.wikipedia.org/wiki/Wikipedia:Text_of_Creative_Commons_Attribution-ShareAlike_3.0_Unported_License)[](//creativecommons.org/licenses/by-sa/3.0/);
    additional terms may apply. By using this site, you agree to the
    [Terms of Use](//wikimediafoundation.org/wiki/Terms_of_Use) and
    [Privacy Policy.](//wikimediafoundation.org/wiki/Privacy_policy) \
     Wikipedia® is a registered trademark of the [Wikimedia Foundation,
    Inc.](//www.wikimediafoundation.org/), a non-profit organization.

-   [Privacy
    policy](//wikimediafoundation.org/wiki/Privacy_policy "wikimedia:Privacy policy")
-   [About Wikipedia](/wiki/Wikipedia:About "Wikipedia:About")
-   [Disclaimers](/wiki/Wikipedia:General_disclaimer "Wikipedia:General disclaimer")
-   [Contact Wikipedia](//en.wikipedia.org/wiki/Wikipedia:Contact_us)
-   [Developers](https://www.mediawiki.org/wiki/Special:MyLanguage/How_to_contribute)
-   [Mobile view](//en.m.wikipedia.org/wiki/Latent_Dirichlet_allocation)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf12/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

https://en.wikipedia.org/wiki/Latent_Dirichlet_allocation
