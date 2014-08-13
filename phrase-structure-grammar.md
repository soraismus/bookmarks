Phrase structure grammar
========================

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

The term **phrase structure grammar** was originally introduced by [Noam
Chomsky](/wiki/Noam_Chomsky "Noam Chomsky") as the term for
[grammars](/wiki/Formal_grammar "Formal grammar") as defined by [phrase
structure
rules](/wiki/Phrase_structure_rules "Phrase structure rules"),^[[1]](#cite_note-1)^
i.e. [rewrite rules](/wiki/Rewrite_rule "Rewrite rule") of the type
studied previously by [Emil Post](/wiki/Emil_Post "Emil Post") and [Axel
Thue](/wiki/Axel_Thue "Axel Thue") (see [Post canonical
systems](/wiki/Post_canonical_system "Post canonical system")). Some
authors, however, reserve the term for more restricted grammars in the
[Chomsky hierarchy](/wiki/Chomsky_hierarchy "Chomsky hierarchy"):
[context-sensitive
grammars](/wiki/Context-sensitive_grammar "Context-sensitive grammar"),
or [context-free
grammars](/wiki/Context-free_grammar "Context-free grammar"). In a
broader sense, phrase structure grammars are also known as *constituency
grammars*. The defining trait of phrase structure grammars is thus their
adherence to the constituency relation, as opposed to the dependency
relation of [dependency
grammars](/wiki/Dependency_grammar "Dependency grammar").

Contents
--------

-   [1 Constituency relation](#Constituency_relation)
-   [2 Dependency relation](#Dependency_relation)
-   [3 Non-descript grammars](#Non-descript_grammars)
-   [4 See also](#See_also)
-   [5 Notes](#Notes)
-   [6 References](#References)

Constituency relation[[edit](/w/index.php?title=Phrase_structure_grammar&action=edit&section=1 "Edit section: Constituency relation")]
--------------------------------------------------------------------------------------------------------------------------------------

In [linguistics](/wiki/Linguistics "Linguistics"), phrase structure
grammars are all those grammars that are based on the constituency
relation, as opposed to the dependency relation associated with
dependency grammars; hence phrase structure grammars are also known as
constituency grammars.^[[2]](#cite_note-2)^ Any of several related
theories for the parsing of [natural
language](/wiki/Natural_language "Natural language") qualify as
constituency grammars, and most of them have been developed from
Chomsky's work, including

-   [Government and Binding
    Theory](/wiki/Government_and_Binding_Theory "Government and Binding Theory"),
-   [Generalized Phrase Structure
    Grammar](/wiki/Generalized_Phrase_Structure_Grammar "Generalized Phrase Structure Grammar"),
-   [Head-Driven Phrase Structure
    Grammar](/wiki/Head-Driven_Phrase_Structure_Grammar "Head-Driven Phrase Structure Grammar"),
-   [Lexical Functional
    Grammar](/wiki/Lexical_Functional_Grammar "Lexical Functional Grammar"),
-   [The Minimalist
    Program](/wiki/The_Minimalist_Program "The Minimalist Program"), and
-   [Nanosyntax](/wiki/Nanosyntax "Nanosyntax").

Further grammar frameworks and formalisms also qualify as
constituency-based, although they may not think of the themselves as
having spawned from Chomsky's work, e.g.

-   [Arc Pair Grammar](/wiki/Arc_Pair_Grammar "Arc Pair Grammar") and
-   [Categorial Grammar](/wiki/Categorial_grammar "Categorial grammar").

The fundamental trait that these frameworks all share is that they view
sentence structure in terms of the constituency relation. The
constituency relation derives from the
[subject](/wiki/Subject_(grammar) "Subject (grammar)")-[predicate](/wiki/Predicate_(grammar) "Predicate (grammar)")
division of Latin and Greek grammars that is based on [term
logic](/wiki/Term_logic "Term logic") and reaches back to
[Aristotle](/wiki/Aristotle "Aristotle") in antiquity. Basic
[clause](/wiki/Clause "Clause") structure is understood in terms of a
binary division of the clause into
[subject](/wiki/Subject_(grammar) "Subject (grammar)") ([noun
phrase](/wiki/Noun_phrase "Noun phrase") NP) and
[predicate](/wiki/Predicate_(grammar) "Predicate (grammar)") ([verb
phrase](/wiki/Verb_phrase "Verb phrase") VP).

The binary division of the clause results in a one-to-one-or-more
correspondence. For each element in a sentence, there are one or more
nodes in the tree structure that one assumes for that sentence. A two
word sentence such as *Luke laughed* necessarily implies three (or more)
nodes in the syntactic structure: one for the noun *Luke* (subject NP),
one for the verb *laughed* (predicate VP), and one for the entirety
*Luke laughed* (sentence S). The constituency grammars listed above all
view sentence structure in terms of this one-to-one-or-more
correspondence.

[![Constituency and dependency
relations](//upload.wikimedia.org/wikipedia/commons/8/8e/Thistreeisillustratingtherelation%28PSG%29.png)](/wiki/File:Thistreeisillustratingtherelation(PSG).png "Constituency and dependency relations")

Dependency relation[[edit](/w/index.php?title=Phrase_structure_grammar&action=edit&section=2 "Edit section: Dependency relation")]
----------------------------------------------------------------------------------------------------------------------------------

By the time of [Gottlob Frege](/wiki/Gottlob_Frege "Gottlob Frege"), a
competing understanding of the logic of sentences had arisen. Frege
rejected the binary division of the sentence and replaced it with an
understanding of sentence logic in terms of predicates and their
arguments. On this alternative conception of sentence logic, the binary
division of the clause into subject and predicate was not possible. It
therefore opened the door to the dependency relation (although the
dependency relation had also existed in a less obvious form in
traditional grammars long before Frege). The dependency relation was
first acknowledged concretely and developed as the basis for a
comprehensive theory of syntax and grammar by [Lucien
Tesnière](/wiki/Lucien_Tesni%C3%A8re "Lucien Tesnière") in his
posthumously published work *Éléments de syntaxe structurale* (Elements
of Structural Syntax).^[[3]](#cite_note-3)^

The dependency relation is a one-to-one correspondence: for every
element (word or morph) in a sentence, there is just one node in the
syntactic structure. The distinction is thus a
[graph-theoretical](/wiki/Graph_theory "Graph theory") distinction. The
dependency relation restricts the number of nodes in the syntactic
structure of a sentence to the exact number of syntactic units (usually
words) that that sentence contains. Thus the two word sentence *Luke
laughed* implies just two syntactic nodes, one for *Luke* and one for
*laughed*. Some prominent dependency grammars are listed here:

-   [Algebraic Syntax](/wiki/Algebraic_syntax "Algebraic syntax")
-   [Functional Generative
    Description](/wiki/Functional_Generative_Description "Functional Generative Description")
-   Lexicase
-   [Meaning-Text
    Theory](/wiki/Meaning-Text_Theory "Meaning-Text Theory")
-   [Operator Grammar](/wiki/Operator_Grammar "Operator Grammar")
-   [Word Grammar](/wiki/Word_grammar "Word grammar")

Since these grammars are all based on the dependency relation, they are
by definition NOT phrase structure grammars.

Non-descript grammars[[edit](/w/index.php?title=Phrase_structure_grammar&action=edit&section=3 "Edit section: Non-descript grammars")]
--------------------------------------------------------------------------------------------------------------------------------------

Other grammars generally avoid attempts to group syntactic units into
clusters in a manner that would allow classification in terms of the
constituency vs. dependency distinction. In this respect, the following
grammar frameworks do not come down solidly on either side of the
dividing line:

-   [Construction
    grammar](/wiki/Construction_grammar "Construction grammar")
-   [Cognitive grammar](/wiki/Cognitive_grammar "Cognitive grammar")

See also[[edit](/w/index.php?title=Phrase_structure_grammar&action=edit&section=4 "Edit section: See also")]
------------------------------------------------------------------------------------------------------------

-   [Dependency grammar](/wiki/Dependency_grammar "Dependency grammar")
-   [Gottlob Frege](/wiki/Gottlob_Frege "Gottlob Frege")
-   [Lucien Tesnière](/wiki/Lucien_Tesni%C3%A8re "Lucien Tesnière")
-   [Predicate](/wiki/Predicate_(grammar) "Predicate (grammar)")
-   [Subject](/wiki/Subject_(grammar) "Subject (grammar)")
-   [Verb phrase](/wiki/Verb_phrase "Verb phrase")

Notes[[edit](/w/index.php?title=Phrase_structure_grammar&action=edit&section=5 "Edit section: Notes")]
------------------------------------------------------------------------------------------------------

1.  **[\^](#cite_ref-1)** See Chomsky (1957).
2.  **[\^](#cite_ref-2)** Matthews (1981:71ff.) provides an insightful
    discussion of the distinction between constituency- and
    dependency-based grammars. See also Allerton (1979:238f.), McCawley
    (1988:13), Mel'cuk (1988:12-14), Borsley (1991:30f.), Sag and Wasow
    (1999:421f.), van Valin (2001:86ff.).
3.  **[\^](#cite_ref-3)** See Tesnière (1959).

References[[edit](/w/index.php?title=Phrase_structure_grammar&action=edit&section=6 "Edit section: References")]
----------------------------------------------------------------------------------------------------------------

-   Allerton, D. 1979. Essentials of grammatical theory. London:
    Routledge & Kegan Paul.
-   Borsley, R. 1991. Syntactic theory: A unified approach. London:
    Edward Arnold.
-   Chomsky, Noam 1957. Syntactic structures. The Hague/Paris: Mouton.
-   Matthews, P. Syntax. 1981. Cambridge, UK: Cambridge University
    Press, [ISBN
    978-0521297097](/wiki/Special:BookSources/9780521297097).
-   McCawley, T. 1988. The syntactic phenomena of English, Vol. 1.
    Chicago: The University of Chicago Press.
-   Mel'cuk, I. 1988. Dependency syntax: Theory and practice. Albany:
    SUNY Press.
-   Sag, I. and T. Wasow. 1999. Syntactic theory: A formal introduction.
    Stanford, CA: CSLI Publications.
-   Tesnière, Lucien 1959. Éleménts de syntaxe structurale. Paris:
    Klincksieck.
-   van Valin, R. 2001. An introduction to syntax. Cambridge, UK:
    Cambridge University Press.

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=Phrase\_structure\_grammar&oldid=590964444](http://en.wikipedia.org/w/index.php?title=Phrase_structure_grammar&oldid=590964444)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Noam Chomsky](/wiki/Category:Noam_Chomsky "Category:Noam Chomsky")
-   [Natural language
    processing](/wiki/Category:Natural_language_processing "Category:Natural language processing")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Phrase+structure+grammar&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Phrase+structure+grammar "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Phrase_structure_grammar "View the content page [c]")
-   [Talk](/wiki/Talk:Phrase_structure_grammar "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Phrase_structure_grammar)
-   [Edit](/w/index.php?title=Phrase_structure_grammar&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Phrase_structure_grammar&action=history "Past versions of this page [h]")

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
    here](/wiki/Special:WhatLinksHere/Phrase_structure_grammar "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Phrase_structure_grammar "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Phrase_structure_grammar&oldid=590964444 "Permanent link to this revision of the page")
-   [Page
    information](/w/index.php?title=Phrase_structure_grammar&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q1134367 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Phrase_structure_grammar&id=590964444 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Phrase+structure+grammar)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Phrase+structure+grammar&oldid=590964444&writer=rl)
-   [Printable
    version](/w/index.php?title=Phrase_structure_grammar&printable=yes "Printable version of this page [p]")

### Languages

-   [Deutsch](//de.wikipedia.org/wiki/Phrasenstrukturgrammatik "Phrasenstrukturgrammatik – German")
-   [日本語](//ja.wikipedia.org/wiki/%E5%8F%A5%E6%A7%8B%E9%80%A0%E6%96%87%E6%B3%95 "句構造文法 – Japanese")
-   [Русский](//ru.wikipedia.org/wiki/%D0%93%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B0_%D1%81_%D1%84%D1%80%D0%B0%D0%B7%D0%BE%D0%B2%D0%BE%D0%B9_%D1%81%D1%82%D1%80%D1%83%D0%BA%D1%82%D1%83%D1%80%D0%BE%D0%B9 "Грамматика с фразовой структурой – Russian")
-   [Svenska](//sv.wikipedia.org/wiki/Frasstrukturgrammatik "Frasstrukturgrammatik – Swedish")
-   [Edit
    links](//www.wikidata.org/wiki/Q1134367#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 16 January 2014 at 13:32.\
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
-   [Mobile view](//en.m.wikipedia.org/wiki/Phrase_structure_grammar)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

