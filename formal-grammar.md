Formal grammar
==============

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

In [formal language
theory](/wiki/Formal_language_theory "Formal language theory"), a
**grammar** (when the context is not given, often called a **formal
grammar** for clarity) is a set of [production
rules](/wiki/Production_(computer_science) "Production (computer science)")
for
[strings](/wiki/String_(computer_science) "String (computer science)")
in a [formal language](/wiki/Formal_language "Formal language"). The
rules describe how to form strings from the language's
[alphabet](/wiki/Alphabet_(computer_science) "Alphabet (computer science)")
that are valid according to the language's
[syntax](/wiki/Syntax_(programming_languages) "Syntax (programming languages)").
A grammar does not describe the [meaning of the
strings](/wiki/Semantics "Semantics") or what can be done with them in
whatever context—only their form.

[Formal language
theory](/wiki/Formal_language_theory "Formal language theory"), the
discipline which studies formal grammars and languages, is a branch of
applied [mathematics](/wiki/Mathematics "Mathematics"). Its applications
are found in [theoretical computer
science](/wiki/Theoretical_computer_science "Theoretical computer science"),
[theoretical
linguistics](/wiki/Theoretical_linguistics "Theoretical linguistics"),
[formal
semantics](/wiki/Formal_semantics_(logic) "Formal semantics (logic)"),
[mathematical logic](/wiki/Mathematical_logic "Mathematical logic"), and
other areas.

A formal grammar is a set of rules for rewriting strings, along with a
"start symbol" from which rewriting starts. Therefore, a grammar is
usually thought of as a language generator. However, it can also
sometimes be used as the basis for a
"[recognizer](/wiki/Recognizer "Recognizer")"—a function in computing
that determines whether a given string belongs to the language or is
grammatically incorrect. To describe such recognizers, formal language
theory uses separate formalisms, known as [automata
theory](/wiki/Automata_theory "Automata theory"). One of the interesting
results of automata theory is that it is not possible to design a
recognizer for certain formal languages.

[Parsing](/wiki/Parsing "Parsing") is the process of recognizing an
utterance (a string in natural languages) by breaking it down to a set
of symbols and analyzing each one against the grammar of the language.
Most languages have the meanings of their utterances structured
according to their syntax—a practice known as [compositional
semantics](/wiki/Compositional_semantics "Compositional semantics"). As
a result, the first step to describing the meaning of an utterance in
language is to break it down part by part and look at its analyzed form
(known as its [parse tree](/wiki/Parse_tree "Parse tree") in computer
science, and as its [deep
structure](/wiki/Deep_structure "Deep structure") in [generative
grammar](/wiki/Generative_grammar "Generative grammar")).

Contents
--------

-   [1 Introductory example](#Introductory_example)
-   [2 Formal definition](#Formal_definition)
    -   [2.1 The syntax of grammars](#The_syntax_of_grammars)
    -   [2.2 The semantics of grammars](#The_semantics_of_grammars)
    -   [2.3 Example](#Example)

-   [3 The Chomsky hierarchy](#The_Chomsky_hierarchy)
    -   [3.1 Context-free grammars](#Context-free_grammars)
    -   [3.2 Regular grammars](#Regular_grammars)
    -   [3.3 Other forms of generative
        grammars](#Other_forms_of_generative_grammars)
    -   [3.4 Recursive grammars](#Recursive_grammars)

-   [4 Analytic grammars](#Analytic_grammars)
-   [5 See also](#See_also)
-   [6 References](#References)
-   [7 External links](#External_links)

Introductory example[[edit](/w/index.php?title=Formal_grammar&action=edit&section=1 "Edit section: Introductory example")]
--------------------------------------------------------------------------------------------------------------------------

A grammar mainly consists of a set of rules for transforming strings.
(If it *only* consisted of these rules, it would be a [semi-Thue
system](/wiki/Semi-Thue_system "Semi-Thue system").) To generate a
string in the language, one begins with a string consisting of only a
single *start symbol*. The *[production
rules](/wiki/Production_(computer_science) "Production (computer science)")*
are then applied in any order, until a string that contains neither the
start symbol nor designated *nonterminal symbols* is produced. A
production rule is applied to a string by replacing one occurrence of
its left-hand side in the string by its right-hand side (*cf.* the
operation of the theoretical [Turing
machine](/wiki/Turing_machine "Turing machine")). The language formed by
the grammar consists of all distinct strings that can be generated in
this manner. Any particular sequence of production rules on the start
symbol yields a distinct string in the language. If there are multiple
ways of generating the same single string, the grammar is said to be
[ambiguous](/wiki/Ambiguous_grammar "Ambiguous grammar").

For example, assume the alphabet consists of *a* and *b*, the start
symbol is *S*, and we have the following production rules:

\1. ![S \\rightarrow
aSb](//upload.wikimedia.org/math/c/5/f/c5fd10fab66e7fe4c235d5436f9b81e1.png)

\2. ![S \\rightarrow
ba](//upload.wikimedia.org/math/5/3/1/53143c5e5792f140ab16895f63baa16a.png)

then we start with *S*, and can choose a rule to apply to it. If we
choose rule 1, we obtain the string *aSb*. If we then choose rule 1
again, we replace *S* with *aSb* and obtain the string *aaSbb*. If we
now choose rule 2, we replace *S* with *ba* and obtain the string
*aababb*, and are done. We can write this series of choices more
briefly, using symbols: ![S \\Rightarrow aSb \\Rightarrow aaSbb
\\Rightarrow
aababb](//upload.wikimedia.org/math/a/b/6/ab6ac8d7fdaae1ea12f3a7797d38f4c5.png).
The language of the grammar is then the infinite set ![\\{a\^nbab\^n | n
\\ge 0 \\} = \\{ba, abab, aababb, aaababbb, \\dotsc
\\}](//upload.wikimedia.org/math/8/2/4/824e99884963b182f3f51644082e728c.png),
where
![a\^k](//upload.wikimedia.org/math/7/7/2/772d08a1575d3a088fffbdcb24fed41c.png)
is
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)
repeated
![k](//upload.wikimedia.org/math/8/c/e/8ce4b16b22b58894aa86c421e8759df3.png)
times (and
![n](//upload.wikimedia.org/math/7/b/8/7b8b965ad4bca0e41ab51de7b31363a1.png)
in particular represents the number of times production rule 1 has been
applied).

Formal definition[[edit](/w/index.php?title=Formal_grammar&action=edit&section=2 "Edit section: Formal definition")]
--------------------------------------------------------------------------------------------------------------------

### The syntax of grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=3 "Edit section: The syntax of grammars")]

In the classic formalization of generative grammars first proposed by
[Noam Chomsky](/wiki/Noam_Chomsky "Noam Chomsky") in the
1950s,^[[1]](#cite_note-Chomsky1956-1)^^[[2]](#cite_note-Chomsky1957-2)^
a grammar *G* consists of the following components:

-   A finite set *N* of *[nonterminal
    symbols](/wiki/Nonterminal_symbol "Nonterminal symbol")*, that is
    [disjoint](/wiki/Disjoint_sets "Disjoint sets") with the strings
    formed from *G*.
-   A finite set
    ![\\Sigma](//upload.wikimedia.org/math/a/6/4/a643a0ef5974b64678111d03125054fc.png)
    of *[terminal symbols](/wiki/Terminal_symbol "Terminal symbol")*
    that is [disjoint](/wiki/Disjoint_sets "Disjoint sets") from *N*.
-   A finite set *P* of *production rules*, each rule of the form

![(\\Sigma \\cup N)\^{\*} N (\\Sigma \\cup N)\^{\*} \\rightarrow
(\\Sigma \\cup N)\^{\*}
](//upload.wikimedia.org/math/f/6/f/f6fa6c714598b85b3ac17450b9f1ac7f.png)

where
![{\*}](//upload.wikimedia.org/math/0/6/a/06ab38ad98e39bcacc7a8082887e64c3.png)
is the [Kleene star](/wiki/Kleene_star "Kleene star") operator and
![\\cup](//upload.wikimedia.org/math/4/3/2/432c1df69e11aba7c5c5070e7578609f.png)
denotes [set union](/wiki/Union_(set_theory) "Union (set theory)"). That
is, each production rule maps from one string of symbols to another,
where the first string (the "head") contains an arbitrary number of
symbols provided at least one of them is a nonterminal. In the case that
the second string (the "body") consists solely of the [empty
string](/wiki/Empty_string "Empty string") – i.e., that it contains no
symbols at all – it may be denoted with a special notation (often
![\\Lambda](//upload.wikimedia.org/math/b/4/1/b41b9dd23fcbbf9d222a2b66fd285d85.png),
*e* or
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png))
in order to avoid confusion.

-   A distinguished symbol ![S \\in
    N](//upload.wikimedia.org/math/7/5/b/75be1f8744d91d32e1e8f88a45fb6e08.png)
    that is the *start symbol*.

A grammar is formally defined as the [tuple](/wiki/Tuple "Tuple") ![(N,
\\Sigma, P,
S)](//upload.wikimedia.org/math/8/0/8/80894a9352944767c0e593f9f868ed17.png).
Such a formal grammar is often called a [rewriting
system](/wiki/Rewriting_system "Rewriting system") or a [phrase
structure
grammar](/wiki/Phrase_structure_grammar "Phrase structure grammar") in
the literature.^[[3]](#cite_note-3)^^[[4]](#cite_note-4)^

### The semantics of grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=4 "Edit section: The semantics of grammars")]

The operation of a grammar can be defined in terms of relations on
strings:

-   Given a grammar ![G = (N, \\Sigma, P,
    S)](//upload.wikimedia.org/math/b/a/a/baa5a15814c30a750ae903e13fd031c5.png),
    the binary relation
    ![\\Rightarrow\_G](//upload.wikimedia.org/math/3/8/6/386a966d03f0fd8ba97a4d449d3105f7.png)
    (pronounced as "G derives in one step") on strings in ![(\\Sigma
    \\cup
    N)\^{\*}](//upload.wikimedia.org/math/4/f/1/4f13bf02c7fdc255616f45da4d75f452.png)
    is defined by:

![x \\Rightarrow\_G y \\mbox{ iff } \\exists u, v, p, q \\in (\\Sigma
\\cup N)\^\*: (x = upv) \\wedge (p \\rightarrow q \\in P) \\wedge (y =
uqv)](//upload.wikimedia.org/math/7/b/8/7b8e5df15022d1dd684ec9518cb4b91b.png)

-   the relation
    ![{\\Rightarrow\_G}\^\*](//upload.wikimedia.org/math/3/d/7/3d798534ed22caff3094cf127c91948a.png)
    (pronounced as *G derives in zero or more steps*) is defined as the
    [reflexive transitive
    closure](/wiki/Reflexive_transitive_closure "Reflexive transitive closure")
    of
    ![\\Rightarrow\_G](//upload.wikimedia.org/math/3/8/6/386a966d03f0fd8ba97a4d449d3105f7.png)
-   a *sentential form* is a member of ![(\\Sigma \\cup
    N)\^\*](//upload.wikimedia.org/math/a/8/8/a88b85a21028be170594ce46cef02dcd.png)
    that can be derived in a finite number of steps from the start
    symbol
    ![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png);
    that is, a sentential form is a member of ![\\{ w \\in (\\Sigma
    \\cup N)\^\* \\mid S {\\Rightarrow\_G}\^\* w
    \\}](//upload.wikimedia.org/math/6/9/0/690f0a2be97a1bfd44831e875a6254eb.png).
    A sentential form that contains no nonterminal symbols (i.e. is a
    member of
    ![\\Sigma\^\*](//upload.wikimedia.org/math/0/b/c/0bc769430aba62db086ee21b2e1d8c12.png))
    is called a *sentence*.^[[5]](#cite_note-5)^
-   the *language* of
    ![G](//upload.wikimedia.org/math/d/f/c/dfcf28d0734569a6a693bc8194de62bf.png),
    denoted as
    ![\\boldsymbol{L}(G)](//upload.wikimedia.org/math/1/7/4/1747c9b4a6a267e82c2cbc19001043f2.png),
    is defined as all those sentences that can be derived in a finite
    number of steps from the start symbol
    ![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png);
    that is, the set ![\\{ w \\in \\Sigma\^\* \\mid S
    {\\Rightarrow\_G}\^\* w
    \\}](//upload.wikimedia.org/math/6/3/d/63d71b0d9a9b4cc57eaeaad6f7157970.png).

Note that the grammar ![G = (N, \\Sigma, P,
S)](//upload.wikimedia.org/math/b/a/a/baa5a15814c30a750ae903e13fd031c5.png)
is effectively the [semi-Thue
system](/wiki/Semi-Thue_system "Semi-Thue system") ![(N \\cup \\Sigma,
P)](//upload.wikimedia.org/math/3/7/9/379a7bc894fe2affd3ef6278bf72f524.png),
rewriting strings in exactly the same way; the only difference is in
that we distinguish specific *nonterminal* symbols which must be
rewritten in rewrite rules, and are only interested in rewritings from
the designated start symbol
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
to strings without nonterminal symbols.

### Example[[edit](/w/index.php?title=Formal_grammar&action=edit&section=5 "Edit section: Example")]

*For these examples, formal languages are specified using [set-builder
notation](/wiki/Set-builder_notation "Set-builder notation").*

Consider the grammar
![G](//upload.wikimedia.org/math/d/f/c/dfcf28d0734569a6a693bc8194de62bf.png)
where ![N = \\left \\{S, B\\right
\\}](//upload.wikimedia.org/math/2/b/6/2b631b62f7e6c73e2da99a9dfcad1c5d.png),
![\\Sigma = \\left \\{a, b, c\\right
\\}](//upload.wikimedia.org/math/3/2/0/32063895b55e6c8bc795d092ff9bd434.png),
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
is the start symbol, and
![P](//upload.wikimedia.org/math/4/4/c/44c29edb103a2872f519ad0c9a0fdaaa.png)
consists of the following production rules:

\1. ![S \\rightarrow
aBSc](//upload.wikimedia.org/math/4/7/9/479977fa1099d74247f85f1b51ff421a.png)

\2. ![S \\rightarrow
abc](//upload.wikimedia.org/math/f/1/3/f132a03f66fcbf46fd2301e95df494d1.png)

\3. ![Ba \\rightarrow
aB](//upload.wikimedia.org/math/1/7/0/170a10c424505886ba0fede9f675fc11.png)

\4. ![Bb \\rightarrow bb
](//upload.wikimedia.org/math/6/8/1/6812fb9ceb83a1201fa03af71a59ba0e.png)

This grammar defines the language ![L(G) = \\left \\{ a\^{n}b\^{n}c\^{n}
| n \\ge 1 \\right
\\}](//upload.wikimedia.org/math/1/1/6/116c21806289959f49253c0031c9fa48.png)
where
![a\^{n}](//upload.wikimedia.org/math/9/0/d/90d876f07570915b02327435ebca63ad.png)
denotes a string of *n* consecutive
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)'s.
Thus, the language is the set of strings that consist of 1 or more
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)'s,
followed by the same number of
![b](//upload.wikimedia.org/math/9/2/e/92eb5ffee6ae2fec3ad71c777531578f.png)'s,
followed by the same number of
![c](//upload.wikimedia.org/math/4/a/8/4a8a08f09d37b73795649038408b5f33.png)'s.

Some examples of the derivation of strings in
![L(G)](//upload.wikimedia.org/math/a/4/d/a4d5bad8db632c708fe9b47dcaa35eaf.png)
are:

-   ![\\boldsymbol{S} \\Rightarrow\_2
    \\boldsymbol{abc}](//upload.wikimedia.org/math/a/1/9/a1918a6df3063f522ff691448b92bc1d.png)
-   ![\\boldsymbol{S} \\Rightarrow\_1 \\boldsymbol{aBSc} \\Rightarrow\_2
    aB\\boldsymbol{abc}c \\Rightarrow\_3 a\\boldsymbol{aB}bcc
    \\Rightarrow\_4
    aa\\boldsymbol{bb}cc](//upload.wikimedia.org/math/b/c/2/bc2373150fa99e5f1b029817910982cd.png)
-   ![\\boldsymbol{S} \\Rightarrow\_1 \\boldsymbol{aBSc} \\Rightarrow\_1
    aB\\boldsymbol{aBSc}c \\Rightarrow\_2 aBaB\\boldsymbol{abc}cc
    \\Rightarrow\_3 a\\boldsymbol{aB}Babccc \\Rightarrow\_3
    aaB\\boldsymbol{aB}bccc
    ](//upload.wikimedia.org/math/2/3/5/23576cffdb7682a8e3b7742b6069b7ed.png)![
    \\Rightarrow\_3 aa\\boldsymbol{aB}Bbccc \\Rightarrow\_4
    aaaB\\boldsymbol{bb}ccc \\Rightarrow\_4
    aaa\\boldsymbol{bb}bccc](//upload.wikimedia.org/math/f/a/4/fa43a1ffb1ea7ce16dd9774c5a9588e6.png)

(Note on notation: ![P \\Rightarrow\_i
Q](//upload.wikimedia.org/math/9/0/1/90123caa54fb6e0c21948f677517b49e.png)
reads "String *P* generates string *Q* by means of production *i*", and
the generated part is each time indicated in bold type.)

The Chomsky hierarchy[[edit](/w/index.php?title=Formal_grammar&action=edit&section=6 "Edit section: The Chomsky hierarchy")]
----------------------------------------------------------------------------------------------------------------------------

Main article: [Chomsky
hierarchy](/wiki/Chomsky_hierarchy "Chomsky hierarchy")

When [Noam Chomsky](/wiki/Noam_Chomsky "Noam Chomsky") first formalized
generative grammars in 1956,^[[1]](#cite_note-Chomsky1956-1)^ he
classified them into types now known as the [Chomsky
hierarchy](/wiki/Chomsky_hierarchy "Chomsky hierarchy"). The difference
between these types is that they have increasingly strict production
rules and can express fewer formal languages. Two important types are
*[context-free
grammars](/wiki/Context-free_grammar "Context-free grammar")* (Type 2)
and *[regular grammars](/wiki/Regular_grammar "Regular grammar")* (Type
3). The languages that can be described with such a grammar are called
*[context-free
languages](/wiki/Context-free_language "Context-free language")* and
*[regular languages](/wiki/Regular_language "Regular language")*,
respectively. Although much less powerful than [unrestricted
grammars](/wiki/Unrestricted_grammar "Unrestricted grammar") (Type 0),
which can in fact express any language that can be accepted by a [Turing
machine](/wiki/Turing_machine "Turing machine"), these two restricted
types of grammars are most often used because
[parsers](/wiki/Parsing "Parsing") for them can be efficiently
implemented.^[[6]](#cite_note-Grune.26Jacobs1990-6)^ For example, all
regular languages can be recognized by a [finite state
machine](/wiki/Finite_state_machine "Finite state machine"), and for
useful subsets of context-free grammars there are well-known algorithms
to generate efficient [LL parsers](/wiki/LL_parser "LL parser") and [LR
parsers](/wiki/LR_parser "LR parser") to recognize the corresponding
languages those grammars generate.

### Context-free grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=7 "Edit section: Context-free grammars")]

A *[context-free
grammar](/wiki/Context-free_grammar "Context-free grammar")* is a
grammar in which the left-hand side of each production rule consists of
only a single nonterminal symbol. This restriction is non-trivial; not
all languages can be generated by context-free grammars. Those that can
are called *context-free languages*.

The language ![L(G) = \\left \\{ a\^{n}b\^{n}c\^{n} | n \\ge 1 \\right
\\}](//upload.wikimedia.org/math/1/1/6/116c21806289959f49253c0031c9fa48.png)
defined above is not a context-free language, and this can be strictly
proven using the [pumping lemma for context-free
languages](/wiki/Pumping_lemma_for_context-free_languages "Pumping lemma for context-free languages"),
but for example the language ![\\left \\{ a\^{n}b\^{n} | n \\ge 1
\\right
\\}](//upload.wikimedia.org/math/1/7/3/173cc15b4b438cdba9fa589427dab81e.png)
(at least 1
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)
followed by the same number of
![b](//upload.wikimedia.org/math/9/2/e/92eb5ffee6ae2fec3ad71c777531578f.png)'s)
is context-free, as it can be defined by the grammar
![G\_2](//upload.wikimedia.org/math/2/1/d/21deb5c93cef58f00f9ba61cc69e997f.png)
with ![N=\\left \\{S\\right
\\}](//upload.wikimedia.org/math/f/5/f/f5f7d755a902e5c60a27eac02c91ae86.png),
![\\Sigma=\\left \\{a,b\\right
\\}](//upload.wikimedia.org/math/f/5/4/f542641f010013ad0f863c8dabbba46e.png),
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
the start symbol, and the following production rules:

\1. ![S \\rightarrow
aSb](//upload.wikimedia.org/math/c/5/f/c5fd10fab66e7fe4c235d5436f9b81e1.png)

\2. ![S \\rightarrow
ab](//upload.wikimedia.org/math/c/8/2/c823900ed00fa3c4d9a49442b54baea6.png)

A context-free language can be recognized in
![O(n\^3)](//upload.wikimedia.org/math/6/8/0/6809c59370e21b3e6e8fd117442fd377.png)
time (*see* [Big O notation](/wiki/Big_O_notation "Big O notation")) by
an algorithm such as [Earley's
algorithm](/wiki/Earley%27s_algorithm "Earley's algorithm"). That is,
for every context-free language, a machine can be built that takes a
string as input and determines in
![O(n\^3)](//upload.wikimedia.org/math/6/8/0/6809c59370e21b3e6e8fd117442fd377.png)
time whether the string is a member of the language, where
![n](//upload.wikimedia.org/math/7/b/8/7b8b965ad4bca0e41ab51de7b31363a1.png)
is the length of the string.^[[7]](#cite_note-Earley1970-7)^
[Deterministic context-free
languages](/wiki/Deterministic_context-free_language "Deterministic context-free language")
is a subset of context-free languages that can be recognized in linear
time.^[[8]](#cite_note-8)^ There exist various algorithms that target
either this set of languages or some subset of it.

### Regular grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=8 "Edit section: Regular grammars")]

In [regular grammars](/wiki/Regular_grammar "Regular grammar"), the left
hand side is again only a single nonterminal symbol, but now the
right-hand side is also restricted. The right side may be the empty
string, or a single terminal symbol, or a single terminal symbol
followed by a nonterminal symbol, but nothing else. (Sometimes a broader
definition is used: one can allow longer strings of terminals or single
nonterminals without anything else, making languages [easier to
denote](/wiki/Syntactic_sugar "Syntactic sugar") while still defining
the same class of languages.)

The language ![\\left \\{ a\^{n}b\^{n} | n \\ge 1 \\right
\\}](//upload.wikimedia.org/math/1/7/3/173cc15b4b438cdba9fa589427dab81e.png)
defined above is not regular, but the language ![\\left \\{ a\^{n}b\^{m}
\\,| \\, m,n \\ge 1 \\right
\\}](//upload.wikimedia.org/math/b/4/c/b4ca2868f6b858f7f28394678fcf5ec2.png)
(at least 1
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)
followed by at least 1
![b](//upload.wikimedia.org/math/9/2/e/92eb5ffee6ae2fec3ad71c777531578f.png),
where the numbers may be different) is, as it can be defined by the
grammar
![G\_3](//upload.wikimedia.org/math/9/2/3/923f9cefae019b8c7c74d882ea7d99f9.png)
with ![N=\\left \\{S, A,B\\right
\\}](//upload.wikimedia.org/math/6/2/e/62ef91f92db3b07605683a8a7018c103.png),
![\\Sigma=\\left \\{a,b\\right
\\}](//upload.wikimedia.org/math/f/5/4/f542641f010013ad0f863c8dabbba46e.png),
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
the start symbol, and the following production rules:

1.  ![S \\rightarrow
    aA](//upload.wikimedia.org/math/2/6/7/267e3e10bc9e1f3dc19fd2a8e502fdf5.png)
2.  ![A \\rightarrow
    aA](//upload.wikimedia.org/math/5/6/e/56e5e8888abe83d0f4f731bde2611d72.png)
3.  ![A \\rightarrow
    bB](//upload.wikimedia.org/math/0/a/d/0ad7d9e719089fa0b1ae8e629f670d4b.png)
4.  ![B \\rightarrow
    bB](//upload.wikimedia.org/math/6/9/3/693698756012b482bbc850c9045486b0.png)
5.  ![B \\rightarrow
    \\epsilon](//upload.wikimedia.org/math/6/f/1/6f144830d809c7a805d97b8a30874192.png)

All languages generated by a regular grammar can be recognized in linear
time by a [finite state
machine](/wiki/Finite_state_machine "Finite state machine"). Although,
in practice, regular grammars are commonly expressed using [regular
expressions](/wiki/Regular_expression "Regular expression"), some forms
of regular expression used in practice do not strictly generate the
regular languages and do not show linear recognitional performance due
to those deviations.

### Other forms of generative grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=9 "Edit section: Other forms of generative grammars")]

Many extensions and variations on Chomsky's original hierarchy of formal
grammars have been developed, both by linguists and by computer
scientists, usually either in order to increase their expressive power
or in order to make them easier to analyze or
[parse](/wiki/Parsing "Parsing"). Some forms of grammars developed
include:

-   [Tree-adjoining
    grammars](/wiki/Tree-adjoining_grammar "Tree-adjoining grammar")
    increase the expressiveness of conventional generative grammars by
    allowing rewrite rules to operate on [parse
    trees](/wiki/Parse_tree "Parse tree") instead of just
    strings.^[[9]](#cite_note-JoshiEtAl1975-9)^
-   [Affix
    grammars](/wiki/Affix_grammar "Affix grammar")^[[10]](#cite_note-Koster1971-10)^
    and [attribute
    grammars](/wiki/Attribute_grammar "Attribute grammar")^[[11]](#cite_note-Knuth1968-11)^^[[12]](#cite_note-Knuth1971-12)^
    allow rewrite rules to be augmented with semantic attributes and
    operations, useful both for increasing grammar expressiveness and
    for constructing practical language translation tools.

### Recursive grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=10 "Edit section: Recursive grammars")]

Not to be confused with [Recursive
language](/wiki/Recursive_language "Recursive language").

A recursive grammar is a grammar which contains production rules that
are
[recursive](/wiki/Recursion_(computer_science) "Recursion (computer science)").
For example, a grammar for a [context-free
language](/wiki/Context-free_language "Context-free language") is
[left-recursive](/wiki/Left_recursion "Left recursion") if there exists
a non-terminal symbol *A* that can be put through the production rules
to produce a string with *A* as the leftmost
symbol.^[[13]](#cite_note-13)^ All types of grammars in the [Chomsky
hierarchy](/wiki/Chomsky_hierarchy "Chomsky hierarchy") can be
recursive.

Analytic grammars[[edit](/w/index.php?title=Formal_grammar&action=edit&section=11 "Edit section: Analytic grammars")]
---------------------------------------------------------------------------------------------------------------------

Though there is a tremendous body of literature on
[parsing](/wiki/Parsing "Parsing")
[algorithms](/wiki/Algorithms "Algorithms"), most of these algorithms
assume that the language to be parsed is initially *described* by means
of a *generative* formal grammar, and that the goal is to transform this
generative grammar into a working parser. Strictly speaking, a
generative grammar does not in any way correspond to the algorithm used
to parse a language, and various algorithms have different restrictions
on the form of production rules that are considered well-formed.

An alternative approach is to formalize the language in terms of an
analytic grammar in the first place, which more directly corresponds to
the structure and semantics of a parser for the language. Examples of
analytic grammar formalisms include the following:

-   [The Language Machine](http://languagemachine.sourceforge.net/)
    directly implements unrestricted analytic grammars. Substitution
    rules are used to transform an input to produce outputs and
    behaviour. The system can also produce [the
    lm-diagram](http://languagemachine.sourceforge.net/picturebook.html)
    which shows what happens when the rules of an unrestricted analytic
    grammar are being applied.
-   [Top-down parsing
    language](/wiki/Top-down_parsing_language "Top-down parsing language")
    (TDPL): a highly minimalist analytic grammar formalism developed in
    the early 1970s to study the behavior of [top-down
    parsers](/wiki/Top-down_parsing "Top-down parsing").^[[14]](#cite_note-Birman1970-14)^
-   [Link grammars](/wiki/Link_grammar "Link grammar"): a form of
    analytic grammar designed for
    [linguistics](/wiki/Linguistics "Linguistics"), which derives
    syntactic structure by examining the positional relationships
    between pairs of
    words.^[[15]](#cite_note-Sleater.26Temperly1991-15)^^[[16]](#cite_note-Sleater.26Temperly1993-16)^
-   [Parsing expression
    grammars](/wiki/Parsing_expression_grammar "Parsing expression grammar")
    (PEGs): a more recent generalization of TDPL designed around the
    practical
    [expressiveness](/wiki/Expressivity_(computer_science) "Expressivity (computer science)")
    needs of [programming
    language](/wiki/Programming_language "Programming language") and
    [compiler](/wiki/Compiler "Compiler")
    writers.^[[17]](#cite_note-17)^

See also[[edit](/w/index.php?title=Formal_grammar&action=edit&section=12 "Edit section: See also")]
---------------------------------------------------------------------------------------------------

-   [Abstract syntax
    tree](/wiki/Abstract_syntax_tree "Abstract syntax tree")
-   [Adaptive grammar](/wiki/Adaptive_grammar "Adaptive grammar")
-   [Ambiguous grammar](/wiki/Ambiguous_grammar "Ambiguous grammar")
-   [Backus–Naur form
    (BNF)](/wiki/Backus%E2%80%93Naur_form "Backus–Naur form")
-   [Categorial grammar](/wiki/Categorial_grammar "Categorial grammar")
-   [Concrete syntax
    tree](/wiki/Concrete_syntax_tree "Concrete syntax tree")
-   [Extended Backus–Naur form
    (EBNF)](/wiki/Extended_Backus%E2%80%93Naur_form "Extended Backus–Naur form")
-   [Grammar framework](/wiki/Grammar_framework "Grammar framework")
-   [L-system](/wiki/L-system "L-system")
-   [Lojban](/wiki/Lojban "Lojban")
-   [Post canonical
    system](/wiki/Post_canonical_system "Post canonical system")
-   [Shape grammar](/wiki/Shape_grammar "Shape grammar")
-   [Well-formed
    formula](/wiki/Well-formed_formula "Well-formed formula")

References[[edit](/w/index.php?title=Formal_grammar&action=edit&section=13 "Edit section: References")]
-------------------------------------------------------------------------------------------------------

1.  \^ [^***a***^](#cite_ref-Chomsky1956_1-0)
    [^***b***^](#cite_ref-Chomsky1956_1-1) Chomsky, Noam (1956). "Three
    Models for the Description of Language". *[IRE Transactions on
    Information
    Theory](/wiki/IRE_Transactions_on_Information_Theory "IRE Transactions on Information Theory")*
    **2** (2): 113–123.
    [doi](/wiki/Digital_object_identifier "Digital object identifier"):[10.1109/TIT.1956.1056813](http://dx.doi.org/10.1109%2FTIT.1956.1056813).
2.  **[\^](#cite_ref-Chomsky1957_2-0)** Chomsky, Noam (1957). *Syntactic
    Structures*. The Hague: [Mouton](/wiki/Mouton "Mouton").
3.  **[\^](#cite_ref-3)** [Ginsburg,
    Seymour](/wiki/Seymour_Ginsburg "Seymour Ginsburg") (1975).
    *Algebraic and automata theoretic properties of formal languages*.
    North-Holland. pp. 8–9.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [0-7204-2506-9](/wiki/Special:BookSources/0-7204-2506-9 "Special:BookSources/0-7204-2506-9").
4.  **[\^](#cite_ref-4)** [Harrison, Michael
    A.](/w/index.php?title=Michael_A._Harrison&action=edit&redlink=1 "Michael A. Harrison (page does not exist)")
    (1978). *Introduction to Formal Language Theory*. Reading, Mass.:
    Addison-Wesley Publishing Company. p. 13.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [0-201-02955-3](/wiki/Special:BookSources/0-201-02955-3 "Special:BookSources/0-201-02955-3").
5.  **[\^](#cite_ref-5)** [Sentential
    Forms](http://www.seas.upenn.edu/~cit596/notes/dave/cfg7.html),
    Context-Free Grammars, David Matuszek
6.  **[\^](#cite_ref-Grune.26Jacobs1990_6-0)** Grune, Dick & Jacobs,
    Ceriel H., *Parsing Techniques – A Practical Guide*, Ellis Horwood,
    England, 1990.
7.  **[\^](#cite_ref-Earley1970_7-0)** Earley, Jay, "An Efficient
    Context-Free Parsing Algorithm," *Communications of the ACM*, Vol.
    13 No. 2, pp. 94-102, February 1970.
8.  **[\^](#cite_ref-8)** [Knuth, D.
    E.](/wiki/Donald_Knuth "Donald Knuth") (July 1965). ["On the
    translation of languages from left to
    right"](http://www.cs.dartmouth.edu/~mckeeman/cs48/mxcom/doc/knuth65.pdf).
    *Information and Control* **8** (6): 607–639.
    [doi](/wiki/Digital_object_identifier "Digital object identifier"):[10.1016/S0019-9958(65)90426-2](http://dx.doi.org/10.1016%2FS0019-9958%2865%2990426-2).
    Retrieved 29 May 2011.
    [edit](//en.wikipedia.org/w/index.php?title=Template:Cite_doi/10.1016.2FS0019-9958.2865.2990426-2&action=edit&editintro=Template:Cite_doi/editintro2)
9.  **[\^](#cite_ref-JoshiEtAl1975_9-0)** Joshi, Aravind K., *et al.*,
    "Tree Adjunct Grammars," *Journal of Computer Systems Science*, Vol.
    10 No. 1, pp. 136-163, 1975.
10. **[\^](#cite_ref-Koster1971_10-0)** Koster , Cornelis H. A., "Affix
    Grammars," in *ALGOL 68 Implementation*, North Holland Publishing
    Company, Amsterdam, p. 95-109, 1971.
11. **[\^](#cite_ref-Knuth1968_11-0)** Knuth, Donald E., "Semantics of
    Context-Free Languages," *Mathematical Systems Theory*, Vol. 2 No.
    2, pp. 127-145, 1968.
12. **[\^](#cite_ref-Knuth1971_12-0)** Knuth, Donald E., "Semantics of
    Context-Free Languages (correction)," *Mathematical Systems Theory*,
    Vol. 5 No. 1, pp 95-96, 1971.
13. **[\^](#cite_ref-13)** [Notes on Formal Language Theory and
    Parsing](http://www.cs.may.ie/~jpower/Courses/parsing/parsing.pdf#search='indirect%20left%20recursion'),
    James Power, Department of Computer Science National University of
    Ireland, Maynooth Maynooth, Co. Kildare,
    Ireland.[JPR02](/w/index.php?title=JPR02&action=edit&redlink=1 "JPR02 (page does not exist)")
14. **[\^](#cite_ref-Birman1970_14-0)** Birman, Alexander, *The TMG
    Recognition Schema*, Doctoral thesis, Princeton University, Dept. of
    Electrical Engineering, February 1970.
15. **[\^](#cite_ref-Sleater.26Temperly1991_15-0)** Sleator, Daniel D. &
    Temperly, Davy, "Parsing English with a Link Grammar," Technical
    Report CMU-CS-91-196, Carnegie Mellon University Computer Science,
    1991.
16. **[\^](#cite_ref-Sleater.26Temperly1993_16-0)** Sleator, Daniel D. &
    Temperly, Davy, "Parsing English with a Link Grammar," *Third
    International Workshop on Parsing Technologies*, 1993. (Revised
    version of above report.)
17. **[\^](#cite_ref-17)** Ford, Bryan, *Packrat Parsing: a Practical
    Linear-Time Algorithm with Backtracking*, Master’s thesis,
    Massachusetts Institute of Technology, Sept. 2002.

External links[[edit](/w/index.php?title=Formal_grammar&action=edit&section=14 "Edit section: External links")]
---------------------------------------------------------------------------------------------------------------

-   [Yearly Formal Grammar conference](http://cs.haifa.ac.il/~shuly/fg/)

-   [v](/wiki/Template:Formal_languages_and_grammars "Template:Formal languages and grammars")
-   [t](/wiki/Template_talk:Formal_languages_and_grammars "Template talk:Formal languages and grammars")
-   [e](//en.wikipedia.org/w/index.php?title=Template:Formal_languages_and_grammars&action=edit)

[Automata theory](/wiki/Automata_theory "Automata theory"): [formal
languages](/wiki/Formal_language "Formal language") and **formal
grammars**

[Chomsky hierarchy](/wiki/Chomsky_hierarchy "Chomsky hierarchy")

**Grammars**

[Languages](/wiki/Formal_language "Formal language")

Minimal [automaton](/wiki/Finite-state_machine "Finite-state machine")

-   Type-0
-   —
-   Type-1
-   —
-   —
-   —
-   Type-2
-   —
-   —
-   Type-3
-   —

-   [Unrestricted](/wiki/Unrestricted_grammar "Unrestricted grammar")
-   (no common name)
-   [Context-sensitive](/wiki/Context-sensitive_grammar "Context-sensitive grammar")
-   [Indexed](/wiki/Indexed_grammar "Indexed grammar")
-   [Linear context-free rewriting
    systems](/wiki/Generalized_context-free_grammar#Linear_Context_free_Rewriting_Systems_.28LCFRSs.29 "Generalized context-free grammar")
    etc.
-   [Tree-adjoining](/wiki/Tree-adjoining_grammar "Tree-adjoining grammar")
    etc.
-   [Context-free](/wiki/Context-free_grammar "Context-free grammar")
-   [Deterministic
    context-free](/wiki/Deterministic_context-free_grammar "Deterministic context-free grammar")
-   [Visibly pushdown](/wiki/Nested_word "Nested word")
-   [Regular](/wiki/Regular_grammar "Regular grammar")
-   —

-   [Recursively
    enumerable](/wiki/Recursively_enumerable_language "Recursively enumerable language")
-   [Decidable](/wiki/Recursive_language "Recursive language")
-   [Context-sensitive](/wiki/Context-sensitive_language "Context-sensitive language")
-   [Indexed](/wiki/Indexed_language "Indexed language")
-   [Mildly
    context-sensitive](/wiki/Mildly_context-sensitive_language "Mildly context-sensitive language")
-   [Tree-adjoining](/wiki/Tree-adjoining_grammar "Tree-adjoining grammar")
-   [Context-free](/wiki/Context-free_language "Context-free language")
-   [Deterministic
    context-free](/wiki/Deterministic_context-free_language "Deterministic context-free language")
-   [Visibly pushdown](/wiki/Nested_word "Nested word")
-   [Regular](/wiki/Regular_language "Regular language")
-   [Star-free](/wiki/Star-free_language "Star-free language")

-   [Turing machine](/wiki/Turing_machine "Turing machine")
-   [Decider](/wiki/Machine_that_always_halts "Machine that always halts")
-   [Linear-bounded](/wiki/Linear_bounded_automaton "Linear bounded automaton")
-   [Nested
    stack](/wiki/Nested_stack_automaton "Nested stack automaton")
-   [Thread automaton](/wiki/Thread_automaton "Thread automaton")
-   [Embedded
    pushdown](/wiki/Embedded_pushdown_automaton "Embedded pushdown automaton")
-   [Nondeterministic
    pushdown](/wiki/Pushdown_automaton "Pushdown automaton")
-   [Deterministic
    pushdown](/wiki/Deterministic_pushdown_automaton "Deterministic pushdown automaton")
-   [Visibly pushdown](/wiki/Nested_word "Nested word")
-   [Finite](/wiki/Finite-state_machine "Finite-state machine")
-   [Counter-free (with aperiodic finite
    monoid)](/wiki/Aperiodic_finite_state_automaton "Aperiodic finite state automaton")

Each category of languages is a [proper subset](/wiki/Subset "Subset")
of the category directly above it. Any automaton and any grammar in each
category has an equivalent automaton or grammar in the category directly
above it.

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=Formal\_grammar&oldid=581347229](http://en.wikipedia.org/w/index.php?title=Formal_grammar&oldid=581347229)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Formal
    languages](/wiki/Category:Formal_languages "Category:Formal languages")
-   [Grammar](/wiki/Category:Grammar "Category:Grammar")
-   [Linguistics](/wiki/Category:Linguistics "Category:Linguistics")
-   [Mathematical
    logic](/wiki/Category:Mathematical_logic "Category:Mathematical logic")
-   [Syntax](/wiki/Category:Syntax "Category:Syntax")
-   [Automata
    theory](/wiki/Category:Automata_theory "Category:Automata theory")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Formal+grammar&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Formal+grammar "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Formal_grammar "View the content page [c]")
-   [Talk](/wiki/Talk:Formal_grammar "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Formal_grammar)
-   [Edit](/w/index.php?title=Formal_grammar&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Formal_grammar&action=history "Past versions of this page [h]")

### Actions[](#)

### Search

![Search](//bits.wikimedia.org/static-1.23wmf11/skins/vector/images/search-ltr.png?303-4)

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
    here](/wiki/Special:WhatLinksHere/Formal_grammar "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Formal_grammar "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Formal_grammar&oldid=581347229 "Permanent link to this revision of the page")
-   [Page information](/w/index.php?title=Formal_grammar&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q373045 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Formal_grammar&id=581347229 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Formal+grammar)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Formal+grammar&oldid=581347229&writer=rl)
-   [Printable
    version](/w/index.php?title=Formal_grammar&printable=yes "Printable version of this page [p]")

### Languages

-   [Bosanski](//bs.wikipedia.org/wiki/Formalna_gramatika "Formalna gramatika – Bosnian")
-   [Català](//ca.wikipedia.org/wiki/Gram%C3%A0tica_formal "Gramàtica formal – Catalan")
-   [Čeština](//cs.wikipedia.org/wiki/Form%C3%A1ln%C3%AD_gramatika "Formální gramatika – Czech")
-   [Deutsch](//de.wikipedia.org/wiki/Formale_Grammatik "Formale Grammatik – German")
-   [Eesti](//et.wikipedia.org/wiki/Formaalne_grammatika "Formaalne grammatika – Estonian")
-   [Ελληνικά](//el.wikipedia.org/wiki/%CE%A4%CF%85%CF%80%CE%B9%CE%BA%CE%AE_%CE%B3%CF%81%CE%B1%CE%BC%CE%BC%CE%B1%CF%84%CE%B9%CE%BA%CE%AE "Τυπική γραμματική – Greek")
-   [Español](//es.wikipedia.org/wiki/Gram%C3%A1tica_formal "Gramática formal – Spanish")
-   [Esperanto](//eo.wikipedia.org/wiki/Formala_gramatiko "Formala gramatiko – Esperanto")
-   [Français](//fr.wikipedia.org/wiki/Grammaire_formelle "Grammaire formelle – French")
-   [Galego](//gl.wikipedia.org/wiki/Gram%C3%A1tica_formal "Gramática formal – Galician")
-   [한국어](//ko.wikipedia.org/wiki/%ED%98%95%EC%8B%9D_%EB%AC%B8%EB%B2%95 "형식 문법 – Korean")
-   [Hrvatski](//hr.wikipedia.org/wiki/Formalna_gramatika "Formalna gramatika – Croatian")
-   [Italiano](//it.wikipedia.org/wiki/Grammatica_formale "Grammatica formale – Italian")
-   [Magyar](//hu.wikipedia.org/wiki/Form%C3%A1lis_nyelvtan "Formális nyelvtan – Hungarian")
-   [Nederlands](//nl.wikipedia.org/wiki/Formele_grammatica "Formele grammatica – Dutch")
-   [日本語](//ja.wikipedia.org/wiki/%E5%BD%A2%E5%BC%8F%E6%96%87%E6%B3%95 "形式文法 – Japanese")
-   [Norsk
    bokmål](//no.wikipedia.org/wiki/Formell_grammatikk "Formell grammatikk – Norwegian (bokmål)")
-   [Polski](//pl.wikipedia.org/wiki/Gramatyka_formalna "Gramatyka formalna – Polish")
-   [Português](//pt.wikipedia.org/wiki/Gram%C3%A1tica_formal "Gramática formal – Portuguese")
-   [Русский](//ru.wikipedia.org/wiki/%D0%A4%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D0%B0%D1%8F_%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B0 "Формальная грамматика – Russian")
-   [Slovenčina](//sk.wikipedia.org/wiki/Gramatika_(informatika) "Gramatika (informatika) – Slovak")
-   [Српски /
    srpski](//sr.wikipedia.org/wiki/%D0%A4%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D0%BD%D0%B0_%D0%B3%D1%80%D0%B0%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B0 "Формална граматика – Serbian")
-   [Srpskohrvatski /
    српскохрватски](//sh.wikipedia.org/wiki/Formalna_gramatika "Formalna gramatika – Serbo-Croatian")
-   [Suomi](//fi.wikipedia.org/wiki/Formaali_kielioppi "Formaali kielioppi – Finnish")
-   [Svenska](//sv.wikipedia.org/wiki/Formell_grammatik "Formell grammatik – Swedish")
-   [Українська](//uk.wikipedia.org/wiki/%D0%A4%D0%BE%D1%80%D0%BC%D0%B0%D0%BB%D1%8C%D0%BD%D1%96_%D0%B3%D1%80%D0%B0%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B8 "Формальні граматики – Ukrainian")
-   [中文](//zh.wikipedia.org/wiki/%E5%BD%A2%E5%BC%8F%E6%96%87%E6%B3%95 "形式文法 – Chinese")
-   [Edit
    links](//www.wikidata.org/wiki/Q373045#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 12 November 2013 at 16:05.\
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
-   [Mobile view](//en.m.wikipedia.org/wiki/Formal_grammar)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

