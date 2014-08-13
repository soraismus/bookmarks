Chomsky hierarchy
=================

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

Within the field of [computer
science](/wiki/Computer_science "Computer science"), specifically in the
area of [formal languages](/wiki/Formal_language "Formal language"), the
**Chomsky hierarchy** (occasionally referred to as
**Chomsky-Schützenberger hierarchy**) is a [containment
hierarchy](/wiki/Containment_hierarchy "Containment hierarchy") of
classes of [formal grammars](/wiki/Formal_grammar "Formal grammar").
This hierarchy of grammars was described by [Noam
Chomsky](/wiki/Noam_Chomsky "Noam Chomsky") in
1956.^[[1]](#cite_note-1)^ It is also named after [Marcel-Paul
Schützenberger](/wiki/Marcel-Paul_Sch%C3%BCtzenberger "Marcel-Paul Schützenberger"),
who played a crucial role in the development of the theory of [formal
languages](/wiki/Formal_language "Formal language"). The Chomsky
Hierarchy, in essence, allows the possibility for the understanding and
use of a computer science model which enables a programmer to accomplish
meaningful linguistic goals systematically.

Contents
--------

-   [1 Formal grammars](#Formal_grammars)
-   [2 The hierarchy](#The_hierarchy)
-   [3 References](#References)
-   [4 External links](#External_links)

Formal grammars[[edit](/w/index.php?title=Chomsky_hierarchy&action=edit&section=1 "Edit section: Formal grammars")]
-------------------------------------------------------------------------------------------------------------------

Main article: [Formal grammar](/wiki/Formal_grammar "Formal grammar")

A formal grammar of this type consists of:

-   a finite set of *[production
    rules](/wiki/Formal_grammar#Introductory_example "Formal grammar")*
    (*left-hand side* ![\\rightarrow
    \\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
    *right-hand side*) where each side consists of a sequence of the
    following symbols:
-   a finite set of *[nonterminal
    symbols](/wiki/Nonterminal_symbol "Nonterminal symbol")* (indicating
    that some production rule can yet be applied)
-   a finite set of *[terminal
    symbols](/wiki/Terminal_symbol "Terminal symbol")* (indicating that
    no production rule can be applied)
-   a *start symbol* (a distinguished nonterminal symbol)

A formal grammar defines (or *generates*) a *formal language*, which is
a (usually infinite) set of finite-length sequences of symbols (i.e.
[strings](/wiki/String_(computer_science) "String (computer science)"))
that may be constructed by applying production rules to another sequence
of symbols which initially contains just the start symbol. A rule may be
applied to a sequence of symbols by replacing an occurrence of the
symbols on the left-hand side of the rule with those that appear on the
right-hand side. A sequence of rule applications is called a
*derivation*. Such a grammar defines the formal language: all words
consisting solely of terminal symbols which can be reached by a
derivation from the start symbol.

Nonterminals are often represented by uppercase letters, terminals by
lowercase letters, and the start symbol by
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png).
For example, the grammar with terminals ![\\{a,
b\\}](//upload.wikimedia.org/math/b/2/7/b27d8e76c40e677920e4de6fd4e26022.png),
nonterminals ![\\{S, A,
B\\}](//upload.wikimedia.org/math/2/2/0/220f0945b4963cf92a8f5ff8e9a8268b.png),
production rules

![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![ABS](//upload.wikimedia.org/math/7/d/8/7d8a220d2262f9d6c658d549ee12cf2c.png)

![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
ε (where ε is the empty string)

![BA](//upload.wikimedia.org/math/5/f/c/5fc810cf62601df84b7923b9964c53e6.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![AB](//upload.wikimedia.org/math/b/8/6/b86fc6b051f63d73de262d4c34e3a0a9.png)

![BS](//upload.wikimedia.org/math/9/a/2/9a231c14a3416b1055b8ffb960151aee.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![b](//upload.wikimedia.org/math/9/2/e/92eb5ffee6ae2fec3ad71c777531578f.png)

![Bb](//upload.wikimedia.org/math/d/5/2/d526f1c8ef6c1e4e980e2b8471352d23.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![bb](//upload.wikimedia.org/math/2/1/a/21ad0bd836b90d08f4cf640b4c298e7c.png)

![Ab](//upload.wikimedia.org/math/0/e/4/0e4c46df226b9c0cb391311c54f28efe.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![ab](//upload.wikimedia.org/math/1/8/7/187ef4436122d1cc2f40dc2b92f0eba0.png)

![Aa](//upload.wikimedia.org/math/9/8/5/98568d540134639be4655198a36614a4.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![aa](//upload.wikimedia.org/math/4/1/2/4124bc0a9335c27f086f24ba207a4912.png)

and start symbol
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png),
defines the language of all words of the form ![ a\^n b\^n
](//upload.wikimedia.org/math/2/e/a/2eac9225dd9c5e425b9a2d556d1bf70e.png)
(i.e.
![n](//upload.wikimedia.org/math/7/b/8/7b8b965ad4bca0e41ab51de7b31363a1.png)
copies of
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)
followed by
![n](//upload.wikimedia.org/math/7/b/8/7b8b965ad4bca0e41ab51de7b31363a1.png)
copies of
![b](//upload.wikimedia.org/math/9/2/e/92eb5ffee6ae2fec3ad71c777531578f.png)).
The following is a simpler grammar that defines the same language:
Terminals ![\\{a,
b\\}](//upload.wikimedia.org/math/b/2/7/b27d8e76c40e677920e4de6fd4e26022.png),
Nonterminals
![\\{S\\}](//upload.wikimedia.org/math/d/b/0/db0296f8a7636fc8420d23e321dadcfe.png),
Start symbol
![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png),
Production rules

![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![aSb](//upload.wikimedia.org/math/8/6/5/865024c112864081b09b36da8e6dcdf0.png)

![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
ε

As another example, a grammar for a toy subset of English language is
given by terminals ![\\{ generate, hate, great, green, ideas, linguists
\\}](//upload.wikimedia.org/math/3/9/1/391341ce5a9eca8f528827b5992a7ece.png),
nonterminals ![\\{\\textit{SENTENCE}, \\textit{NOUNPHRASE},
\\textit{VERBPHRASE}, \\textit{NOUN}, \\textit{VERB}, \\textit{ADJ}
\\}](//upload.wikimedia.org/math/e/1/2/e1287299094be64137f4cfd21be2cdfd.png),
production rules

![\\textit{SENTENCE}](//upload.wikimedia.org/math/8/3/c/83c0a24a395ea45b146ab9d22297eb39.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{NOUNPHRASE} \\;
\\textit{VERBPHRASE}](//upload.wikimedia.org/math/3/f/5/3f574798e1b689dd2ca8712f63dd1ca8.png)

![\\textit{NOUNPHRASE}](//upload.wikimedia.org/math/9/7/c/97c2dd38b63fb8d8e98bb789e6490ef5.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{ADJ} \\;
\\textit{NOUNPHRASE}](//upload.wikimedia.org/math/c/a/0/ca0634738bd09d5791d76eb740bc2b74.png)

![\\textit{NOUNPHRASE}](//upload.wikimedia.org/math/9/7/c/97c2dd38b63fb8d8e98bb789e6490ef5.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{NOUN}](//upload.wikimedia.org/math/a/8/2/a82f104d6911a763ad99a31072277de9.png)

![\\textit{VERBPHRASE}](//upload.wikimedia.org/math/a/2/9/a29371d9b37d6b38476011aa1ae2303d.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{VERB} \\;
\\textit{NOUNPHRASE}](//upload.wikimedia.org/math/5/7/e/57ebf049b20243e9820e01259d11c626.png)

![\\textit{VERBPHRASE}](//upload.wikimedia.org/math/a/2/9/a29371d9b37d6b38476011aa1ae2303d.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{VERB}](//upload.wikimedia.org/math/f/7/0/f701ef9d7ffda49ad7c634a245c56b61.png)

![\\textit{NOUN}](//upload.wikimedia.org/math/a/8/2/a82f104d6911a763ad99a31072277de9.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{ideas}](//upload.wikimedia.org/math/f/8/7/f8709bd792e207f4b837172506e86498.png)

![\\textit{NOUN}](//upload.wikimedia.org/math/a/8/2/a82f104d6911a763ad99a31072277de9.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{linguists}](//upload.wikimedia.org/math/a/e/9/ae9886df352d29a4b12af986a6f9a244.png)

![\\textit{VERB}](//upload.wikimedia.org/math/f/7/0/f701ef9d7ffda49ad7c634a245c56b61.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{generate}](//upload.wikimedia.org/math/c/c/1/cc11961c2e1dde31522b41910abaf4bb.png)

![\\textit{VERB}](//upload.wikimedia.org/math/f/7/0/f701ef9d7ffda49ad7c634a245c56b61.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{hate}](//upload.wikimedia.org/math/0/6/9/06946e225965973d3735bb125d0ec098.png)

![\\textit{ADJ}](//upload.wikimedia.org/math/4/1/5/4155f44d6ba1e253c9981136ee68a5a9.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{great}](//upload.wikimedia.org/math/8/6/4/864434aff9ded5f952bc800482a2aff9.png)

![\\textit{ADJ}](//upload.wikimedia.org/math/4/1/5/4155f44d6ba1e253c9981136ee68a5a9.png)
![\\rightarrow
\\,](//upload.wikimedia.org/math/8/5/5/8558a668c3dd3fe346c448d980f33d6c.png)
![\\textit{green}](//upload.wikimedia.org/math/f/1/6/f16c51c28d3d91aee17d7ab7764492fd.png)

and start symbol
![\\textit{SENTENCE}](//upload.wikimedia.org/math/8/3/c/83c0a24a395ea45b146ab9d22297eb39.png).
An example derivation is

*SENTENCE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*NOUNPHRASE VERBPHRASE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*ADJ NOUNPHRASE VERBPHRASE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*ADJ NOUN VERBPHRASE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*ADJ NOUN VERB NOUNPHRASE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*ADJ NOUN VERB ADJ NOUNPHRASE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*ADJ NOUN VERB ADJ ADJ NOUNPHRASE*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*ADJ NOUN VERB ADJ ADJ NOUN*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*great NOUN VERB ADJ ADJ NOUN*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*great linguists VERB ADJ ADJ NOUN*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*great linguists generate ADJ ADJ NOUN*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*great linguists generate great ADJ NOUN*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*great linguists generate great green NOUN*
![\\rightarrow](//upload.wikimedia.org/math/8/3/e/83e37b7246fdfcb99b2754210ebeae27.png)
*great linguists generate great green ideas*.

Other sequences that can be derived from this grammar are "*ideas hate
great linguists*", and "*ideas generate*". While these sentences are
nonsensical, they are syntactically correct. A syntactically incorrect
sentence like e.g. "*ideas ideas great hate*" cannot be derived from
this grammar. See "[Colorless green ideas sleep
furiously](/wiki/Colorless_green_ideas_sleep_furiously "Colorless green ideas sleep furiously")"
for a similar example given by Chomsky in 1957; see [Phrase structure
grammar](/wiki/Phrase_structure_grammar "Phrase structure grammar") and
[Phrase structure
rules](/wiki/Phrase_structure_rules "Phrase structure rules") for more
natural-language examples and the problems of formal grammars in that
area.

The hierarchy[[edit](/w/index.php?title=Chomsky_hierarchy&action=edit&section=2 "Edit section: The hierarchy")]
---------------------------------------------------------------------------------------------------------------

[![The Chomsky
hierarchy](//upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Chomsky-hierarchy.svg/200px-Chomsky-hierarchy.svg.png)](/wiki/File:Chomsky-hierarchy.svg)

[![image](//bits.wikimedia.org/static-1.23wmf10/skins/common/images/magnify-clip.png)](/wiki/File:Chomsky-hierarchy.svg "Enlarge")

Set inclusions described by the Chomsky hierarchy

The Chomsky hierarchy consists of the following levels:

-   Type-0 grammars ([unrestricted
    grammars](/wiki/Unrestricted_grammar "Unrestricted grammar"))
    include all formal grammars. They generate exactly all languages
    that can be recognized by a [Turing
    machine](/wiki/Turing_machine "Turing machine"). These languages are
    also known as the [recursively enumerable
    languages](/wiki/Recursively_enumerable_language "Recursively enumerable language").
    Note that this is different from the [recursive
    languages](/wiki/Recursive_language "Recursive language") which can
    be *decided* by an [always-halting Turing
    machine](/wiki/Machine_that_always_halts "Machine that always halts").
-   Type-1 grammars ([context-sensitive
    grammars](/wiki/Context-sensitive_grammar "Context-sensitive grammar"))
    generate the [context-sensitive
    languages](/wiki/Context-sensitive_language "Context-sensitive language").
    These grammars have rules of the form ![\\alpha A\\beta \\rightarrow
    \\alpha\\gamma\\beta](//upload.wikimedia.org/math/e/5/b/e5b4080885f3db9df1732daa2dbb828a.png)
    with
    ![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
    a nonterminal and
    ![\\alpha](//upload.wikimedia.org/math/b/c/c/bccfc7022dfb945174d9bcebad2297bb.png),
    ![\\beta](//upload.wikimedia.org/math/0/7/1/071997f13634882f823041b057f90923.png)
    and
    ![\\gamma](//upload.wikimedia.org/math/3/3/4/334de1ea38b615839e4ee6b65ee1b103.png)
    strings of terminals and nonterminals. The strings
    ![\\alpha](//upload.wikimedia.org/math/b/c/c/bccfc7022dfb945174d9bcebad2297bb.png)
    and
    ![\\beta](//upload.wikimedia.org/math/0/7/1/071997f13634882f823041b057f90923.png)
    may be empty, but
    ![\\gamma](//upload.wikimedia.org/math/3/3/4/334de1ea38b615839e4ee6b65ee1b103.png)
    must be nonempty. The rule ![S \\rightarrow
    \\epsilon](//upload.wikimedia.org/math/0/4/8/04812fdcd5b6ac1b57afc7351a501f03.png)
    is allowed if
    ![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
    does not appear on the right side of any rule. The languages
    described by these grammars are exactly all languages that can be
    recognized by a [linear bounded
    automaton](/wiki/Linear_bounded_automaton "Linear bounded automaton")
    (a nondeterministic Turing machine whose tape is bounded by a
    constant times the length of the input.)
-   Type-2 grammars ([context-free
    grammars](/wiki/Context-free_grammar "Context-free grammar"))
    generate the [context-free
    languages](/wiki/Context-free_language "Context-free language").
    These are defined by rules of the form ![A \\rightarrow
    \\gamma](//upload.wikimedia.org/math/5/c/6/5c67b8ceeb27f488be5376974f5b033a.png)
    with
    ![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
    a nonterminal and
    ![\\gamma](//upload.wikimedia.org/math/3/3/4/334de1ea38b615839e4ee6b65ee1b103.png)
    a string of terminals and nonterminals. These languages are exactly
    all languages that can be recognized by a non-deterministic
    [pushdown automaton](/wiki/Pushdown_automaton "Pushdown automaton").
    Context-free languages – or rather the subset of [deterministic
    context-free
    language](/wiki/Deterministic_context-free_language "Deterministic context-free language")
    – are the theoretical basis for the phrase structure of most
    [programming
    languages](/wiki/Programming_language "Programming language"),
    though their syntax also includes context-sensitive name resolution
    due to declarations and
    [scope](/wiki/Scope_(computer_science) "Scope (computer science)").
    Often a subset of grammars are used to make parsing easier, such as
    by an [LL parser](/wiki/LL_parser "LL parser").
-   Type-3 grammars ([regular
    grammars](/wiki/Regular_grammar "Regular grammar")) generate the
    [regular languages](/wiki/Regular_language "Regular language"). Such
    a grammar restricts its rules to a single nonterminal on the
    left-hand side and a right-hand side consisting of a single
    terminal, possibly followed by a single nonterminal (right regular).
    Alternatively, the right-hand side of the grammar can consist of a
    single terminal, possibly preceded by a single nonterminal (left
    regular); these generate the same languages – however, if
    left-regular rules and right-regular rules are combined, the
    language need no longer be regular. The rule ![S \\rightarrow
    \\epsilon](//upload.wikimedia.org/math/0/4/8/04812fdcd5b6ac1b57afc7351a501f03.png)
    is also allowed here if
    ![S](//upload.wikimedia.org/math/5/d/b/5dbc98dcc983a70728bd082d1a47546e.png)
    does not appear on the right side of any rule. These languages are
    exactly all languages that can be decided by a [finite state
    automaton](/wiki/Finite_state_automaton "Finite state automaton").
    Additionally, this family of formal languages can be obtained by
    [regular
    expressions](/wiki/Regular_expression "Regular expression"). Regular
    languages are commonly used to define search patterns and the
    lexical structure of programming languages.

Note that the set of grammars corresponding to [recursive
languages](/wiki/Recursive_language "Recursive language") is not a
member of this hierarchy; these would be properly between Type-0 and
Type-1.

Every regular language is context-free, every context-free language, not
containing the empty string, is context-sensitive and every
context-sensitive language is recursive and every recursive language is
recursively enumerable. These are all proper inclusions, meaning that
there exist recursively enumerable languages which are not
context-sensitive, context-sensitive languages which are not
context-free and context-free languages which are not regular.

The following table summarizes each of Chomsky's four types of grammars,
the class of language it generates, the type of automaton that
recognizes it, and the form its rules must have.

  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  Grammar   Languages                                                                                           Automaton                                                                                                      Production rules (constraints)
  --------- --------------------------------------------------------------------------------------------------- -------------------------------------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------
  Type-0    [Recursively enumerable](/wiki/Recursively_enumerable_language "Recursively enumerable language")   [Turing machine](/wiki/Turing_machine "Turing machine")                                                        ![\\alpha \\rightarrow \\beta](//upload.wikimedia.org/math/6/d/6/6d60466dde1f5f57fd2e5f15d6d10171.png) (no restrictions)

  Type-1    [Context-sensitive](/wiki/Context-sensitive_grammar "Context-sensitive grammar")                    [Linear-bounded non-deterministic Turing machine](/wiki/Linear_bounded_automaton "Linear bounded automaton")   ![\\alpha A \\beta \\rightarrow \\alpha \\gamma \\beta](//upload.wikimedia.org/math/e/5/b/e5b4080885f3db9df1732daa2dbb828a.png)

  Type-2    [Context-free](/wiki/Context-free_grammar "Context-free grammar")                                   Non-deterministic [pushdown automaton](/wiki/Pushdown_automaton "Pushdown automaton")                          ![A \\rightarrow \\gamma](//upload.wikimedia.org/math/5/c/6/5c67b8ceeb27f488be5376974f5b033a.png)

  Type-3    [Regular](/wiki/Regular_grammar "Regular grammar")                                                  [Finite state automaton](/wiki/Finite_state_automaton "Finite state automaton")                                ![A \\rightarrow a](//upload.wikimedia.org/math/3/c/3/3c3575164d217096a12d41b39a76934c.png)\
                                                                                                                                                                                                                                and\
                                                                                                                                                                                                                                ![A \\rightarrow aB](//upload.wikimedia.org/math/3/a/3/3a399ac1d57d31ac7eed4efc05b71d03.png)
  --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

However, there are further categories of formal languages, some of which
are given in the expandable navigation box at the bottom of this page.

References[[edit](/w/index.php?title=Chomsky_hierarchy&action=edit&section=3 "Edit section: References")]
---------------------------------------------------------------------------------------------------------

1.  **[\^](#cite_ref-1)** Chomsky, Noam (1956). ["Three models for the
    description of
    language"](http://www.chomsky.info/articles/195609--.pdf). *IRE
    Transactions on Information Theory* (2): 113–124.
    [doi](/wiki/Digital_object_identifier "Digital object identifier"):[10.1109/TIT.1956.1056813](http://dx.doi.org/10.1109%2FTIT.1956.1056813).

-   Chomsky, Noam (1959). ["On certain formal properties of
    grammars"](http://www.diku.dk/hjemmesider/ansatte/henglein/papers/chomsky1959.pdf).
    *Information and Control* **2** (2): 137–167.
    [doi](/wiki/Digital_object_identifier "Digital object identifier"):[10.1016/S0019-9958(59)90362-6](http://dx.doi.org/10.1016%2FS0019-9958%2859%2990362-6).
-   Chomsky, Noam; Schützenberger, Marcel P. (1963). "The algebraic
    theory of context free languages". In Braffort, P.; Hirschberg, D.
    *Computer Programming and Formal Languages*. Amsterdam: North
    Holland. pp. 118–161. Cite uses deprecated parameters
    ([help](/wiki/Help:CS1_errors#deprecated_params "Help:CS1 errors"))
-   Davis, Martin E.; Sigal, Ron; Weyuker, Elaine J. (1994).
    *Computability, complexity, and languages: Fundamentals of
    theoretical computer science*. Boston: Academic Press, Harcourt,
    Brace. p. 327.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [0-12-206382-1](/wiki/Special:BookSources/0-12-206382-1 "Special:BookSources/0-12-206382-1").

External links[[edit](/w/index.php?title=Chomsky_hierarchy&action=edit&section=4 "Edit section: External links")]
-----------------------------------------------------------------------------------------------------------------

-   [http://www.staff.ncl.ac.uk/hermann.moisl/ell236/lecture5.htm](http://www.staff.ncl.ac.uk/hermann.moisl/ell236/lecture5.htm)

-   [v](/wiki/Template:Formal_languages_and_grammars "Template:Formal languages and grammars")
-   [t](/wiki/Template_talk:Formal_languages_and_grammars "Template talk:Formal languages and grammars")
-   [e](//en.wikipedia.org/w/index.php?title=Template:Formal_languages_and_grammars&action=edit)

[Automata theory](/wiki/Automata_theory "Automata theory"): [formal
languages](/wiki/Formal_language "Formal language") and [formal
grammars](/wiki/Formal_grammar "Formal grammar")

**Chomsky hierarchy**

[Grammars](/wiki/Formal_grammar "Formal grammar")

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

-   [v](/wiki/Template:Noam_Chomsky "Template:Noam Chomsky")
-   [t](/wiki/Template_talk:Noam_Chomsky "Template talk:Noam Chomsky")
-   [e](//en.wikipedia.org/w/index.php?title=Template:Noam_Chomsky&action=edit)

[Noam Chomsky](/wiki/Noam_Chomsky "Noam Chomsky")

-   [Political
    views](/wiki/Noam_Chomsky%27s_political_views "Noam Chomsky's political views")
-   [Bibliography](/wiki/Noam_Chomsky_bibliography "Noam Chomsky bibliography")
-   **Chomsky hierarchy**
-   "[Colorless green ideas sleep
    furiously](/wiki/Colorless_green_ideas_sleep_furiously "Colorless green ideas sleep furiously")"

Select\
 bibliography

[Linguistics](/wiki/Linguistics "Linguistics")

-   *[Syntactic
    Structures](/wiki/Syntactic_Structures "Syntactic Structures")*
    (1957)
-   *[Current issues in linguistic
    theory](/w/index.php?title=Current_issues_in_linguistic_theory&action=edit&redlink=1 "Current issues in linguistic theory (page does not exist)")*
    (1964)
-   *[Aspects of the Theory of
    Syntax](/wiki/Aspects_of_the_Theory_of_Syntax "Aspects of the Theory of Syntax")*
    (1965)
-   *[Cartesian Linguistics: A Chapter in the History of Rationalist
    Thought](/wiki/Cartesian_linguistics "Cartesian linguistics")*
    (1966)
-   *[The Sound Pattern of
    English](/wiki/The_Sound_Pattern_of_English "The Sound Pattern of English")*
    (1968)
-   *[Conditions on
    Transformations](/wiki/Conditions_on_Transformations "Conditions on Transformations")*
    (1973)
-   *[The Logical Structure of Linguistic
    Theory](/w/index.php?title=The_Logical_Structure_of_Linguistic_Theory&action=edit&redlink=1 "The Logical Structure of Linguistic Theory (page does not exist)")*
    (1975)
-   *[Lectures on Government and Binding: The Pisa
    Lectures](/wiki/Lectures_on_Government_and_Binding:_The_Pisa_Lectures "Lectures on Government and Binding: The Pisa Lectures")*
    (1981)
-   *[Knowledge of language: its nature, origin, and
    use](/wiki/Knowledge_of_language:_its_nature,_origin,_and_use "Knowledge of language: its nature, origin, and use")*
    (1986)
-   *[The Minimalist
    Program](/wiki/Minimalist_program "Minimalist program")* (1995)
-   *[New Horizons in the Study of Language and
    Mind](/w/index.php?title=New_Horizons_in_the_Study_of_Language_and_Mind&action=edit&redlink=1 "New Horizons in the Study of Language and Mind (page does not exist)")*
    (2000)

[Politics](/wiki/Politics "Politics")

-   *[The Responsibility of
    Intellectuals](/wiki/The_Responsibility_of_Intellectuals "The Responsibility of Intellectuals")*
    (1967)
-   *[American Power and the New
    Mandarins](/wiki/American_Power_and_the_New_Mandarins "American Power and the New Mandarins")*
    (1969)
-   *[The Fateful
    Triangle](/wiki/The_Fateful_Triangle "The Fateful Triangle")* (1983)
-   *[The Soviet Union Versus
    Socialism](/w/index.php?title=The_Soviet_Union_Versus_Socialism&action=edit&redlink=1 "The Soviet Union Versus Socialism (page does not exist)")*
    (1986)
-   *[Manufacturing Consent: The Political Economy of the Mass
    Media](/wiki/Manufacturing_Consent:_The_Political_Economy_of_the_Mass_Media "Manufacturing Consent: The Political Economy of the Mass Media")*^[[a]](#cite_note-note_Herman-2)^
    (1988)
-   *[Necessary
    Illusions](/wiki/Necessary_Illusions "Necessary Illusions")* (1989)
-   *[Deterring
    Democracy](/wiki/Deterring_Democracy "Deterring Democracy")* (1991)
-   *[Objectivity and Liberal
    Scholarship](/wiki/Objectivity_and_Liberal_Scholarship "Objectivity and Liberal Scholarship")*
    (1997)
-   *[The Culture of
    Terrorism](/w/index.php?title=The_Culture_of_Terrorism&action=edit&redlink=1 "The Culture of Terrorism (page does not exist)")*
    (1999)
-   *[Hegemony or Survival: America's Quest for Global
    Dominance](/wiki/Hegemony_or_Survival "Hegemony or Survival")*
    (2003)
-   *[Failed States: The Abuse of Power and the Assault on
    Democracy](/wiki/Failed_States:_The_Abuse_of_Power_and_the_Assault_on_Democracy "Failed States: The Abuse of Power and the Assault on Democracy")*
    (2006)

Collections\
 (articles and interviews)

-   *[Class Warfare](/wiki/Class_Warfare "Class Warfare")* (1996)
-   *[Middle East
    Illusions](/wiki/Middle_East_Illusions:_Including_Peace_in_the_Middle_East%3F_Reflections_on_Justice_and_Nationhood "Middle East Illusions: Including Peace in the Middle East? Reflections on Justice and Nationhood")*
    (2003)
-   *[Imperial
    Ambitions](/wiki/Imperial_Ambitions "Imperial Ambitions")* (2005)
-   *[Interventions](/wiki/Interventions "Interventions")* (2007)
-   *[Gaza in Crisis](/wiki/Gaza_in_Crisis "Gaza in Crisis")* (2010)
-   *[Making the Future](/wiki/Making_the_Future "Making the Future")*
    (2012)
-   *[Occupy](/wiki/Occupy_(Chomsky_book) "Occupy (Chomsky book)")*
    (2012)

Filmography

-   *[Manufacturing Consent: Noam Chomsky and the
    Media](/wiki/Manufacturing_Consent:_Noam_Chomsky_and_the_Media "Manufacturing Consent: Noam Chomsky and the Media")*
    (1992)
-   *[Last Party
    2000](/w/index.php?title=Last_Party_2000&action=edit&redlink=1 "Last Party 2000 (page does not exist)")*
    (2001)
-   *[Power and Terror: Noam Chomsky in Our
    Times](/w/index.php?title=Power_and_Terror:_Noam_Chomsky_in_Our_Times&action=edit&redlink=1 "Power and Terror: Noam Chomsky in Our Times (page does not exist)")*
    (2002)
-   *[Distorted Morality – America's War On
    Terror?](/w/index.php?title=Distorted_Morality_%E2%80%93_America%27s_War_On_Terror%3F&action=edit&redlink=1 "Distorted Morality – America's War On Terror? (page does not exist)")*
    (2003)
-   *[Noam Chomsky: Rebel Without a
    Pause](/w/index.php?title=Noam_Chomsky:_Rebel_Without_a_Pause&action=edit&redlink=1 "Noam Chomsky: Rebel Without a Pause (page does not exist)")*
    (2003) (TV)
-   *[Peace, Propaganda & the Promised
    Land](/wiki/Peace,_Propaganda_%26_the_Promised_Land "Peace, Propaganda & the Promised Land")*
    (2004)
-   *[Is the Man Who Is Tall
    Happy?](/wiki/Is_the_Man_Who_Is_Tall_Happy%3F "Is the Man Who Is Tall Happy?")*
    (2013)

Family

-   [William Chomsky](/wiki/William_Chomsky "William Chomsky")
-   [Carol Chomsky](/wiki/Carol_Chomsky "Carol Chomsky")
-   [Aviva Chomsky](/wiki/Aviva_Chomsky "Aviva Chomsky")

1.  **[\^](#cite_ref-note_Herman_2-0)** with [Edward S.
    Herman](/wiki/Edward_S._Herman "Edward S. Herman")

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=Chomsky\_hierarchy&oldid=584414955](http://en.wikipedia.org/w/index.php?title=Chomsky_hierarchy&oldid=584414955)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [1956 in computer
    science](/wiki/Category:1956_in_computer_science "Category:1956 in computer science")
-   [Formal
    languages](/wiki/Category:Formal_languages "Category:Formal languages")
-   [Generative
    linguistics](/wiki/Category:Generative_linguistics "Category:Generative linguistics")
-   [Noam Chomsky](/wiki/Category:Noam_Chomsky "Category:Noam Chomsky")

Hidden categories:

-   [Pages containing cite templates with deprecated
    parameters](/wiki/Category:Pages_containing_cite_templates_with_deprecated_parameters "Category:Pages containing cite templates with deprecated parameters")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Chomsky+hierarchy&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Chomsky+hierarchy "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Chomsky_hierarchy "View the content page [c]")
-   [Talk](/wiki/Talk:Chomsky_hierarchy "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Chomsky_hierarchy)
-   [Edit](/w/index.php?title=Chomsky_hierarchy&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Chomsky_hierarchy&action=history "Past versions of this page [h]")

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
    here](/wiki/Special:WhatLinksHere/Chomsky_hierarchy "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Chomsky_hierarchy "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Chomsky_hierarchy&oldid=584414955 "Permanent link to this revision of the page")
-   [Page information](/w/index.php?title=Chomsky_hierarchy&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q190913 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Chomsky_hierarchy&id=584414955 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Chomsky+hierarchy)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Chomsky+hierarchy&oldid=584414955&writer=rl)
-   [Printable
    version](/w/index.php?title=Chomsky_hierarchy&printable=yes "Printable version of this page [p]")

### Languages

-   [Afrikaans](//af.wikipedia.org/wiki/Chomsky-hi%C3%ABrargie "Chomsky-hiërargie – Afrikaans")
-   [বাংলা](//bn.wikipedia.org/wiki/%E0%A6%9A%E0%A6%AE%E0%A7%8D%E2%80%8C%E0%A6%B8%E0%A7%8D%E0%A6%95%E0%A6%BF_%E0%A6%B8%E0%A7%8D%E0%A6%A4%E0%A6%B0%E0%A6%95%E0%A7%8D%E0%A6%B0%E0%A6%AE "চম্‌স্কি স্তরক্রম – Bengali")
-   [Български](//bg.wikipedia.org/wiki/%D0%99%D0%B5%D1%80%D0%B0%D1%80%D1%85%D0%B8%D1%8F_%D0%BD%D0%B0_%D0%A7%D0%BE%D0%BC%D1%81%D0%BA%D0%B8 "Йерархия на Чомски – Bulgarian")
-   [Bosanski](//bs.wikipedia.org/wiki/Chomskyjeva_hijerarhija "Chomskyjeva hijerarhija – Bosnian")
-   [Català](//ca.wikipedia.org/wiki/Jerarquia_de_Chomsky "Jerarquia de Chomsky – Catalan")
-   [Čeština](//cs.wikipedia.org/wiki/Chomsk%C3%A9ho_hierarchie "Chomského hierarchie – Czech")
-   [Deutsch](//de.wikipedia.org/wiki/Chomsky-Hierarchie "Chomsky-Hierarchie – German")
-   [Ελληνικά](//el.wikipedia.org/wiki/%CE%99%CE%B5%CF%81%CE%B1%CF%81%CF%87%CE%AF%CE%B1_%CE%A4%CF%83%CF%8C%CE%BC%CF%83%CE%BA%CE%B9 "Ιεραρχία Τσόμσκι – Greek")
-   [Español](//es.wikipedia.org/wiki/Jerarqu%C3%ADa_de_Chomsky "Jerarquía de Chomsky – Spanish")
-   [فارسی](//fa.wikipedia.org/wiki/%D9%88%D8%B1%D8%A7%D8%AB%D8%AA_%DA%86%D8%A7%D9%85%D8%B3%DA%A9%DB%8C "وراثت چامسکی – Persian")
-   [Français](//fr.wikipedia.org/wiki/Hi%C3%A9rarchie_de_Chomsky "Hiérarchie de Chomsky – French")
-   [한국어](//ko.wikipedia.org/wiki/%EC%B4%98%EC%8A%A4%ED%82%A4_%EC%9C%84%EA%B3%84 "촘스키 위계 – Korean")
-   [Hrvatski](//hr.wikipedia.org/wiki/Chomskyjeva_hijerarhija "Chomskyjeva hijerarhija – Croatian")
-   [Italiano](//it.wikipedia.org/wiki/Gerarchia_di_Chomsky "Gerarchia di Chomsky – Italian")
-   [עברית](//he.wikipedia.org/wiki/%D7%94%D7%94%D7%99%D7%A8%D7%A8%D7%9B%D7%99%D7%94_%D7%A9%D7%9C_%D7%97%D7%95%D7%9E%D7%A1%D7%A7%D7%99 "ההיררכיה של חומסקי – Hebrew")
-   [ქართული](//ka.wikipedia.org/wiki/%E1%83%A9%E1%83%9D%E1%83%9B%E1%83%A1%E1%83%99%E1%83%98%E1%83%A1_%E1%83%98%E1%83%94%E1%83%A0%E1%83%90%E1%83%A0%E1%83%A5%E1%83%98%E1%83%90 "ჩომსკის იერარქია – Georgian")
-   [Қазақша](//kk.wikipedia.org/wiki/%D0%A5%D0%BE%D0%BC%D1%81%D0%BA%D0%B8%D0%B9_%D0%B8%D0%B5%D1%80%D0%B0%D1%80%D1%85%D0%B8%D1%8F%D1%81%D1%8B "Хомский иерархиясы – Kazakh")
-   [Latina](//la.wikipedia.org/wiki/Hierarchia_Chomskiana "Hierarchia Chomskiana – Latin")
-   [Македонски](//mk.wikipedia.org/wiki/%D0%A5%D0%B8%D0%B5%D1%80%D0%B0%D1%80%D1%85%D0%B8%D1%98%D0%B0_%D0%BD%D0%B0_%D0%A7%D0%BE%D0%BC%D1%81%D0%BA%D0%B8 "Хиерархија на Чомски – Macedonian")
-   [Nederlands](//nl.wikipedia.org/wiki/Chomskyhi%C3%ABrarchie "Chomskyhiërarchie – Dutch")
-   [日本語](//ja.wikipedia.org/wiki/%E3%83%81%E3%83%A7%E3%83%A0%E3%82%B9%E3%82%AD%E3%83%BC%E9%9A%8E%E5%B1%A4 "チョムスキー階層 – Japanese")
-   [Norsk
    bokmål](//no.wikipedia.org/wiki/Chomskyhierarkiet "Chomskyhierarkiet – Norwegian (bokmål)")
-   [Norsk
    nynorsk](//nn.wikipedia.org/wiki/Chomskyhierarkiet "Chomskyhierarkiet – Norwegian Nynorsk")
-   [Polski](//pl.wikipedia.org/wiki/Hierarchia_Chomsky%27ego "Hierarchia Chomsky'ego – Polish")
-   [Português](//pt.wikipedia.org/wiki/Hierarquia_de_Chomsky "Hierarquia de Chomsky – Portuguese")
-   [Română](//ro.wikipedia.org/wiki/Ierarhia_Chomsky "Ierarhia Chomsky – Romanian")
-   [Русский](//ru.wikipedia.org/wiki/%D0%98%D0%B5%D1%80%D0%B0%D1%80%D1%85%D0%B8%D1%8F_%D0%A5%D0%BE%D0%BC%D1%81%D0%BA%D0%BE%D0%B3%D0%BE "Иерархия Хомского – Russian")
-   [Slovenčina](//sk.wikipedia.org/wiki/Chomsk%C3%A9ho_hierarchia "Chomského hierarchia – Slovak")
-   [Српски /
    srpski](//sr.wikipedia.org/wiki/Hijerarhija_%C4%8Comskog "Hijerarhija Čomskog – Serbian")
-   [Srpskohrvatski /
    српскохрватски](//sh.wikipedia.org/wiki/Chomskyjeva_hijerarhija "Chomskyjeva hijerarhija – Serbo-Croatian")
-   [Suomi](//fi.wikipedia.org/wiki/Chomskyn_hierarkia "Chomskyn hierarkia – Finnish")
-   [Українська](//uk.wikipedia.org/wiki/%D0%86%D1%94%D1%80%D0%B0%D1%80%D1%85%D1%96%D1%8F_%D0%A7%D0%BE%D0%BC%D1%81%D0%BA%D1%96 "Ієрархія Чомскі – Ukrainian")
-   [中文](//zh.wikipedia.org/wiki/%E4%B9%94%E5%A7%86%E6%96%AF%E5%9F%BA%E8%B0%B1%E7%B3%BB "乔姆斯基谱系 – Chinese")
-   [Edit
    links](//www.wikidata.org/wiki/Q190913#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 3 December 2013 at 20:05.\
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
-   [Mobile view](//en.m.wikipedia.org/wiki/Chomsky_hierarchy)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

