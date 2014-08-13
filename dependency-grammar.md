Dependency grammar
==================

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

**Dependency grammar** (**DG**) is a class of modern
[syntactic](/wiki/Syntax "Syntax") theories that are all based on the
dependency relation and that can be traced back primarily to the work of
[Lucien Tesnière](/wiki/Lucien_Tesni%C3%A8re "Lucien Tesnière"). The
dependency relation views the (finite) verb as the structural center of
all clause structure. All other syntactic units (e.g. words) are either
directly or indirectly dependent on the verb. DGs are distinct from
[phrase structure
grammars](/wiki/Phrase_structure_grammar "Phrase structure grammar")
(constituency grammars), since DGs lack phrasal nodes - although they
acknowledge phrases. Structure is determined by the relation between a
word (a head) and its dependents. Dependency structures are flatter than
constituency structures in part because they lack a
[finite](/wiki/Finite_verb "Finite verb") [verb
phrase](/wiki/Verb_phrase "Verb phrase")
[constituent](/wiki/Constituent_(linguistics) "Constituent (linguistics)"),
and they are thus well suited for the analysis of languages with free
word order, such as [Czech](/wiki/Czech_language "Czech language") and
[Turkish](/wiki/Turkish_language "Turkish language").

Contents
--------

-   [1 History](#History)
-   [2 Dependency vs. constituency](#Dependency_vs._constituency)
-   [3 Dependency grammars](#Dependency_grammars)
-   [4 Representing dependencies](#Representing_dependencies)
-   [5 Types of dependencies](#Types_of_dependencies)
    -   [5.1 Semantic dependencies](#Semantic_dependencies)
    -   [5.2 Morphological dependencies](#Morphological_dependencies)
    -   [5.3 Prosodic dependencies](#Prosodic_dependencies)
    -   [5.4 Syntactic dependencies](#Syntactic_dependencies)

-   [6 Linear order and
    discontinuities](#Linear_order_and_discontinuities)
-   [7 Syntactic functions](#Syntactic_functions)
-   [8 See also](#See_also)
-   [9 Notes](#Notes)
-   [10 References](#References)
-   [11 Implementations](#Implementations)
-   [12 External links](#External_links)

History[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=1 "Edit section: History")]
----------------------------------------------------------------------------------------------------

The notion of dependencies between grammatical units has existed since
the earliest recorded grammars, e.g.
[Pāṇini](/wiki/P%C4%81%E1%B9%87ini "Pāṇini"), and the dependency concept
therefore arguably predates the constituency notion by many
centuries.^[[1]](#cite_note-1)^ [Ibn
Maḍāʾ](/wiki/Ibn_Ma%E1%B8%8D%C4%81%CA%BE "Ibn Maḍāʾ"), a 12th-century
[linguist](/wiki/Linguist "Linguist") from [Córdoba,
Andalusia](/wiki/C%C3%B3rdoba,_Andalusia "Córdoba, Andalusia"), may have
been the first grammarian to use the term *dependency* in the
grammatical sense that we use today. In early modern times, the
dependency concept seems to have coexisted side by side with the
constituency concept, the latter having entered Latin, French, English
and other grammars from the widespread study of [term
logic](/wiki/Term_logic "Term logic") of antiquity.^[[2]](#cite_note-2)^

Modern dependency grammars, however, begin primarily with the work of
[Lucien Tesnière](/wiki/Lucien_Tesni%C3%A8re "Lucien Tesnière").
Tesnière was a Frenchman, a [polyglot](/wiki/Polyglot "Polyglot"), and a
professor of linguistics at the universities in Strasbourg and
Montpellier. His major work *Éléments de syntaxe structurale* was
published posthumously in 1959 – he died in 1954. The basic approach to
syntax he developed seems to have been seized upon independently by
others in the 1960s^[[3]](#cite_note-3)^ and a number of other
dependency-based grammars have gained prominence since those early
works.^[[4]](#cite_note-4)^ DG has generated a lot of interest in
Germany^[[5]](#cite_note-5)^ in both theoretical syntax and language
pedagogy. In recent years, the great development surrounding
dependency-based theories has come from [computational
linguistics](/wiki/Computational_linguistics "Computational linguistics").
Dependency-based systems are increasingly being used to parse natural
language and generate tree banks. Interest in dependency grammar is
growing at present, international conferences on dependency linguistics
being a relatively recent development ([Depling
2011](http://depling.org), [Depling
2013](http://ufal.mff.cuni.cz/project/depling13/)).

Dependency vs. constituency[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=2 "Edit section: Dependency vs. constituency")]
--------------------------------------------------------------------------------------------------------------------------------------------

Dependency is a one-to-one correspondence: for every element (e.g. word
or morph) in the sentence, there is exactly one node in the structure of
that sentence that corresponds to that element. The result of this
one-to-one correspondence is that dependency grammars are word (or
morph) grammars. All that exist are the elements and the dependencies
that connect the elements into a structure. This situation should be
compared with the constituency relation of [phrase structure
grammars](/wiki/Phrase_structure_grammar "Phrase structure grammar").
Constituency is a one-to-one-or-more correspondence, which means that,
for every element in a sentence, there are one or more nodes in the
structure that correspond to that element. The result of this difference
is that dependency structures are minimal^[[6]](#cite_note-6)^ compared
to their constituency structure counterparts, since they tend to contain
many fewer nodes.

[![Dependency vs.
constituency](//upload.wikimedia.org/wikipedia/commons/0/0d/Wearetryingtounderstandthedifference_%282%29.jpg)](/wiki/File:Wearetryingtounderstandthedifference_(2).jpg "Dependency vs. constituency")

These two trees illustrate just two possible ways to render the
dependency and constituency relations (see below). The dependency tree
is an "ordered" tree, i.e. it reflects actual word order. Many
dependency trees abstract away from linear order and focus just on
hierarchical order, which means they do not show actual word order. The
constituency tree follows the conventions of [bare phrase
structure](/wiki/Minimalist_program "Minimalist program") (BPS), whereby
the words themselves are employed as the node labels.

The distinction between dependency- and constituency-based grammars
derives in a large part from the initial division of the clause. The
constituency relation derives from an initial binary division, whereby
the clause is split into a subject [noun
phrase](/wiki/Noun_phrase "Noun phrase") (NP) and a
[predicate](/wiki/Predicate_(grammar) "Predicate (grammar)") [verb
phrase](/wiki/Verb_phrase "Verb phrase") (VP). This division is
certainly present in the basic analysis of the clause that we find in
the works of, for instance, [Leonard
Bloomfield](/wiki/Leonard_Bloomfield "Leonard Bloomfield") and [Noam
Chomsky](/wiki/Noam_Chomsky "Noam Chomsky"). Tesnière, however, argued
vehemently against this binary division, preferring instead to position
the verb as the root of all clause structure. Tesnière's stance was that
the subject-predicate division stems from [term
logic](/wiki/Term_logic "Term logic") and has no place in
linguistics.^[[7]](#cite_note-7)^ The importance of this distinction is
that if one acknowledges the initial subject-predicate division in
syntax as something real, then one is likely to go down the path of
constituency grammar, whereas if one rejects this division, then the
only alternative is to position the verb as the root of all structure,
which means one has chosen the path of dependency grammar.

Dependency grammars[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=3 "Edit section: Dependency grammars")]
----------------------------------------------------------------------------------------------------------------------------

The following frameworks are dependency-based:

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Quranic-arabic-corpus.png/250px-Quranic-arabic-corpus.png)](/wiki/File:Quranic-arabic-corpus.png)

[![image](//bits.wikimedia.org/static-1.23wmf10/skins/common/images/magnify-clip.png)](/wiki/File:Quranic-arabic-corpus.png "Enlarge")

Hybrid constituency/dependency tree from the [Quranic Arabic
Corpus](/wiki/Quranic_Arabic_Corpus "Quranic Arabic Corpus")

-   [Algebraic syntax](/wiki/Algebraic_syntax "Algebraic syntax")
-   [Operator grammar](/wiki/Operator_grammar "Operator grammar")
-   [Functional generative
    description](/wiki/Functional_generative_description "Functional generative description")
-   Lexicase grammar
-   [Meaning–text
    theory](/wiki/Meaning%E2%80%93text_theory "Meaning–text theory")
-   [Word grammar](/wiki/Word_grammar "Word grammar")
-   [Extensible dependency
    grammar](http://www.ps.uni-saarland.de/~rade/xdg.html)

[Link grammar](/wiki/Link_grammar "Link grammar") is also based on the
dependency relation, but link grammar does not include directionality in
the dependencies between words, and thus does not describe
head-dependent relationships. Hybrid dependency/constituency grammar
uses dependencies between words, but also includes dependencies between
phrasal nodes – see for example the [Quranic Arabic Dependency
Treebank](/wiki/Quranic_Arabic_Corpus "Quranic Arabic Corpus"). The
derivation trees of [Tree-adjoining
grammar](/wiki/Tree-adjoining_grammar "Tree-adjoining grammar") are
dependency-based, although the full trees of TAG are constituency-based,
so in this regard, it is not clear whether TAG should be viewed more as
a dependency or constituency grammar.

There are major differences between the grammars just listed. In this
regard, the dependency relation is compatible with other major tenets of
theories of grammar. Thus like constituency grammars, dependency
grammars can be mono- or multistratal, representational or derivational,
construction- or rule-based.

Representing dependencies[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=4 "Edit section: Representing dependencies")]
----------------------------------------------------------------------------------------------------------------------------------------

There are various conventions that DGs employ to represent dependencies.
The following schemata (in addition to the tree above and the trees
further below) illustrate some of these conventions:

[![Conventions for illustrating
dependencies](//upload.wikimedia.org/wikipedia/commons/9/9e/Conventions.jpg)](/wiki/File:Conventions.jpg "Conventions for illustrating dependencies")

The representations in (a-d) are trees, whereby the specific conventions
employed in each tree vary. Solid lines are *dependency edges* and
lightly dotted lines are *projection lines*. The only difference between
tree (a) and tree (b) is that tree (a) employs the category class to
label the nodes whereas tree (b) employs the words themselves as the
node labels.^[[8]](#cite_note-8)^ Tree (c) is a reduced tree insofar as
the string of words below and projection lines are deemed unnecessary
and are hence omitted. Tree (d) abstracts away from linear order and
reflects just hierarchical order.^[[9]](#cite_note-9)^ The arrow arcs in
(e) are an alternative convention used to show dependencies and are
favored by [Word
Grammar](/wiki/Word_grammar "Word grammar")^[[10]](#cite_note-10)^ The
brackets in (f) are seldom used, but are nevertheless quite capable of
reflecting the dependency hierarchy; dependents appear enclosed in more
brackets than their heads. And finally, the indentations like those in
(g) are another convention that is sometimes employed to indicate the
hierarchy of words.^[[11]](#cite_note-11)^ Dependents are placed
underneath their heads and indented. Like tree (d), the indentations in
(g) abstract away from linear order.

The point to these conventions is that they are just that, namely
conventions. They do not influence the basic commitment to dependency as
the relation that is grouping syntactic units.

Types of dependencies[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=5 "Edit section: Types of dependencies")]
--------------------------------------------------------------------------------------------------------------------------------

The dependency representations above (and further below) show syntactic
dependencies. Indeed, most work in dependency grammar focuses on
syntactic dependencies. Syntactic dependencies are, however, just one of
three or four types of dependencies. [Meaning-Text
Theory](/wiki/Meaning-Text_Theory "Meaning-Text Theory"), for instance,
emphasizes the role of semantic and morphological dependencies in
addition to syntactic dependencies.^[[12]](#cite_note-12)^ A fourth
type, prosodic dependencies, can also be acknowledged. Distinguishing
between these types of dependencies can be important, in part because if
one fails to do so, the likelihood that semantic, morphological, and/or
prosodic dependencies will be mistaken for syntactic dependencies is
great. The following four subsections briefly sketch each of these
dependency types. During the discussion, the existence of syntactic
dependencies is taken for granted and used as an orientation point for
establishing the nature of the other three dependency types.

### Semantic dependencies[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=6 "Edit section: Semantic dependencies")]

Semantic dependencies are understood in terms of
[predicates](/wiki/Predicate_(grammar) "Predicate (grammar)") and their
[arguments](/wiki/Argument_(linguistics) "Argument (linguistics)").^[[13]](#cite_note-13)^
The arguments of a predicate are semantically dependent on that
predicate. Often, semantic dependencies overlap with and point in the
same direction as syntactic dependencies. At times, however, semantic
dependencies can point in the opposite direction of syntactic
dependencies, or they can be entirely independent of syntactic
dependencies. The hierarchy of words in the following examples show
standard syntactic dependencies, whereas the arrows indicate semantic
dependencies:

[![Semantic
dependencies](//upload.wikimedia.org/wikipedia/commons/a/ac/Semantic_dependencies.png)](/wiki/File:Semantic_dependencies.png "Semantic dependencies")

The two arguments *Sam* and *Sally* in tree (a) are dependent on the
predicate *likes*, whereby these arguments are also syntactically
dependent on *likes*. What this means is that the semantic and syntactic
dependencies overlap and point in the same direction (down the tree).
Attributive adjectives, however, are predicates that take their head
noun as their argument, hence *big* is a predicate in tree (b) that
takes *bones* as its one argument; the semantic dependency points up the
tree and therefore runs counter to the syntactic dependency. A similar
situation obtains in (c), where the preposition predicate *on* takes the
two arguments *the picture* and *the wall*; one of these semantic
dependencies points up the syntactic hierarchy, whereas the other points
down it. Finally, the predicate *to help* in (d) takes the one argument
*Jim* but is not directly connected to *Jim* in the syntactic hierarchy,
which means that that semantic dependency is entirely independent of the
syntactic dependencies.

### Morphological dependencies[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=7 "Edit section: Morphological dependencies")]

Morphological dependencies obtain between words or parts of
words.^[[14]](#cite_note-14)^ When a given word or part of a word
influences the form of another word, then the latter is morphologically
dependent on the former. Agreement and concord are therefore
manifestations of morphological dependencies. Like semantic
dependencies, morphological dependencies can overlap with and point in
the same direction as syntactic dependencies, overlap with and point in
the opposite direction of syntactic dependencies, or be entirely
independent of syntactic dependencies. The arrows are now used to
indicate morphological dependencies.

[![Morphological dependencies
1](//upload.wikimedia.org/wikipedia/commons/6/6b/Mophological_dependencies_1.png)](/wiki/File:Mophological_dependencies_1.png "Morphological dependencies 1")

The plural *houses* in (a) demands the plural of the demonstrative
determiner, hence *these* appears, not *this*, which means there is a
morphological dependency that points down the hierarchy from *houses* to
*these*. The situation is reversed in (b), where the singular subject
*Sam* demands the appearance of the agreement suffix *-s* on the finite
verb *works*, which means there is a morphological dependency pointing
up the hierarchy from *Sam* to *works*. The type of determiner in the
German examples (c) and (d) influences the inflectional suffix that
appears on the adjective *alt*. When the indefinite article *ein*
appears, it lacks gender, so the strong masculine ending *-er* appears
on the adjective. When the definite article *der* appears, in contrast,
it shows masculine gender, which means the weak ending *-e* appears on
the adjective. Thus since the choice of determiner impacts the
morphological form of the adjective, there is a morphological dependency
pointing from the determiner to the adjective, whereby this
morphological dependency is entirely independent of the syntactic
dependencies. Consider further the following French sentences:

[![Morphological dependencies
2'](//upload.wikimedia.org/wikipedia/commons/e/e5/Morphological_dependencies_2%27.png)](/wiki/File:Morphological_dependencies_2%27.png "Morphological dependencies 2'")

The masculine subject *le chien* in (a) demands the masculine form of
the predicative adjective *blanc*, whereas the feminine subject *la
maison* demands the feminine form of this adjective. A morphological
dependency that is entirely independent of the syntactic dependencies
therefore points again across the syntactic hierarchy.

Morphological dependencies play an important role in typological
studies. Languages are classified as mostly
[head-marking](/wiki/Head-marking_language "Head-marking language")
(*Sam work-s*) or mostly
[dependent-marking](/wiki/Dependent-marking_language "Dependent-marking language")
(*these houses*), whereby most if not all languages contain at least
some minor measure of both head and dependent
marking.^[[15]](#cite_note-15)^

### Prosodic dependencies[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=8 "Edit section: Prosodic dependencies")]

Prosodic dependencies are acknowledged in order to accommodate the
behavior of [clitics](/wiki/Clitic "Clitic").^[[16]](#cite_note-16)^ A
clitic is a syntactically autonomous element that is prosodically
dependent on a host. A clitic is therefore integrated into the prosody
of its host, meaning that it forms a single word with its host. Prosodic
dependencies exist entirely in the linear dimension (horizontal
dimension), whereas standard syntactic dependencies exist in the
hierarchical dimension (vertical dimension). Classic examples of clitics
in English are reduced auxiliaries (e.g. *-ll*, *-s*, *-ve*) and the
possessive marker *-s*. The prosodic dependencies in the following
examples are indicated with the hyphen and the lack of a vertical
projection line:

[![Prosodic
dependencies'](//upload.wikimedia.org/wikipedia/commons/5/55/Prosodic_dependencies%27.png)](/wiki/File:Prosodic_dependencies%27.png "Prosodic dependencies'")

The hyphens and lack of projection lines indicate prosodic dependencies.
A hyphen that appears on the left of the clitic indicates that the
clitic is prosodically dependent on the word immediately to its left
(*He'll*, *There's*), whereas a hyphen that appears on the right side of
the clitic (not shown here) indicates that the clitic is prosodically
dependent on the word that appears immediately to its right. A given
clitic is often prosodically dependent on its syntactic dependent
(*He'll*, *There's*) or on its head (*would've*). At other times, it can
depend prosodically on a word that is neither its head nor its immediate
dependent (*Florida's*).

### Syntactic dependencies[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=9 "Edit section: Syntactic dependencies")]

Syntactic dependencies are the focus of most work in dependency grammar,
as stated above. How the presence and the direction of syntactic
dependencies are determined is of course often open to debate. In this
regard, it must be acknowledged that the validity of syntactic
dependencies in the trees throughout this article is being taken for
granted. However, these hierarchies are such that many dependency
grammars can largely support them, although there will certainly be
points of disagreement. The basic question about how syntactic
dependencies are discerned has proven difficult to answer definitively.
One should acknowledge in this area, however, that the basic task of
identifying and discerning the presence and direction of the syntactic
dependencies of dependency grammars is no easier or harder than
determining the constituent groupings of constituency grammars. A
variety of heuristics are employed to this end, basic [constituency
tests](/wiki/Constituent_(linguistics) "Constituent (linguistics)")
being useful tools; the syntactic dependencies assumed in the trees in
this article are grouping words together in a manner that most closely
matches the results of standard permutation, substitution, and ellipsis
constituency tests. Etymological considerations also provide helpful
clues about the direction of dependencies. A promising principle upon
which to base the existence of syntactic dependencies is
distribution.^[[17]](#cite_note-17)^ When one is striving to identify
the root of a given phrase, the word that is most responsible for
determining the distribution of that phrase as a whole is the root of
that phrase.

Linear order and discontinuities[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=10 "Edit section: Linear order and discontinuities")]
-------------------------------------------------------------------------------------------------------------------------------------------------------

Traditionally, DGs have had a different approach to linear order (word
order) than constituency grammars. Dependency-based structures are
minimal compared to their constituency-based counterparts, and these
minimal structures allow one to focus intently on the two ordering
dimensions.^[[18]](#cite_note-18)^ Separating the vertical dimension
(hierarchical order) from the horizontal dimension (linear order) is
easily accomplished. This aspect of dependency-based structures has
allowed DGs, starting with Tesnière (1959), to focus on hierarchical
order in a manner that is hardly possible for constituency grammars. For
Tesnière, linear order was secondary to hierarchical order insofar as
hierarchical order preceded linear order in the mind of a speaker. The
stemmas (trees) that Tesnière produced reflected this view; they
abstracted away from linear order to focus almost entirely on
hierarchical order. Many DGs that followed Tesnière adopted this
practice, that is, they produced tree structures that reflect
hierarchical order alone, e.g.

[![Two unordered
trees](//upload.wikimedia.org/wikipedia/commons/3/39/Dg-new-1.jpg)](/wiki/File:Dg-new-1.jpg "Two unordered trees")

The traditional focus on hierarchical order generated the impression
that DGs have little to say about linear order, and it has contributed
to the view that DGs are particularly well-suited to examine languages
with free word order. A negative result of this focus on hierarchical
order, however, is that there is a dearth of dependency-based
explorations of particular word order phenomena, such as of standard
[discontinuities](/wiki/Discontinuity_(linguistics) "Discontinuity (linguistics)").
Comprehensive dependency grammar accounts of
[topicalization](/wiki/Topicalization "Topicalization"),
[*wh*-fronting](/wiki/Wh-movement "Wh-movement"),
[scrambling](/wiki/Scrambling_(linguistics) "Scrambling (linguistics)"),
and [extraposition](/wiki/Extraposition "Extraposition") are mostly
absent from many established dependency-based frameworks. This situation
can be contrasted with constituency grammars, which have devoted
tremendous effort to exploring these phenomena.

The nature of the dependency relation does not, however, prevent one
from focusing on linear order. Dependency-based structures are as
capable of exploring word order phenomena as constituency-based
structures. The following trees illustrate this point; they represent
one way of exploring discontinuities using dependency-based structures.
The trees suggest the manner in which common discontinuities can be
addressed. An example from German is used to illustrate a scrambling
discontinuity:

[![8 ordered
trees](//upload.wikimedia.org/wikipedia/commons/6/6d/Dg-new-2.jpg)](/wiki/File:Dg-new-2.jpg "8 ordered trees")

The a-trees on the left show
[projectivity](/wiki/Discontinuity_(linguistics) "Discontinuity (linguistics)")
violations (= crossing lines), and the b-trees on the right demonstrate
one means of addressing these violations. The displaced constituent
takes on a word as its
[head](/wiki/Head_(linguistics) "Head (linguistics)") that is not its
[governor](/wiki/Government_(linguistics) "Government (linguistics)").
The words in red mark the
[catena](/wiki/Catena_(linguistics) "Catena (linguistics)") (=chain) of
words that extends from the root of the displaced constituent to the
[governor](/wiki/Government_(linguistics) "Government (linguistics)") of
that constituent.^[[19]](#cite_note-19)^ Discontinuities are then
explored in terms of these catenae. The limitations on topicalization,
*wh*-fronting, scrambling, and extraposition can be explored and
identified by examining the nature of the catenae involved.

Syntactic functions[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=11 "Edit section: Syntactic functions")]
-----------------------------------------------------------------------------------------------------------------------------

Traditionally, DGs have treated the syntactic functions (= grammatical
functions, [grammatical
relations](/wiki/Grammatical_relation "Grammatical relation")) as
primitive. They posit an inventory of functions (e.g. subject, object,
oblique, determiner, attribute, predicative, etc.). These functions can
appear as labels on the dependencies in the tree structures,
e.g.^[[20]](#cite_note-20)^

[![Syntactic functions
1](//upload.wikimedia.org/wikipedia/commons/c/c3/Syntactic_functions_1.png)](/wiki/File:Syntactic_functions_1.png "Syntactic functions 1")

The syntactic functions in this tree are shown in green: ATTR
(attribute), COMP-P (complement of preposition), COMP-TO (complement of
to), DET (determiner), P-ATTR (prepositional attribute), PRED
(predicative), SUBJ (subject), TO-COMP (to complement). The functions
chosen and abbreviations used in the tree here are merely representative
of the general stance of DGs toward the syntactic functions. The actual
inventory of functions and designations employed vary from DG to DG.

As a primitive of the theory, the status of these functions is much
different than in some constituency grammars. Traditionally,
constituency grammars derive the syntactic functions from the
constellation. For instance, the object is identified as the NP
appearing inside finite VP, and the subject as the NP appearing outside
of finite VP. Since DGs reject the existence of a finite VP constituent,
they were never presented with the option to view the syntactic
functions in this manner. The issue is a question of what comes first:
traditionally, DGs take the syntactic functions to be primitive and they
then derive the constellation from these functions, whereas constituency
grammars traditionally take the constellation to be primitive and they
then derive the syntactic functions from the constellation.

This question about what comes first (the functions or the
constellation) is not an inflexible matter. The stances of both grammar
types (dependency and constituency grammars) is not narrowly limited to
the traditional views. Dependency and constituency are both fully
compatible with both approaches to the syntactic functions. Indeed,
monostratal systems, be they dependency- or constituency-based, will
likely reject the notion that the functions are derived from the
constellation or that the constellation is derived from the functions.
They will take both to be primitive, which means neither can be derived
from the other.

See also[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=12 "Edit section: See also")]
-------------------------------------------------------------------------------------------------------

-   [Catena](/wiki/Catena_(linguistics) "Catena (linguistics)")
-   [Constituent](/wiki/Constituent_(linguistics) "Constituent (linguistics)")
-   [Discontinuity](/wiki/Discontinuity_(linguistics) "Discontinuity (linguistics)")
-   [Finite verb](/wiki/Finite_verb "Finite verb")
-   [Lucien Tesnière](/wiki/Lucien_Tesni%C3%A8re "Lucien Tesnière")
-   [Phrase structure
    grammar](/wiki/Phrase_structure_grammar "Phrase structure grammar")
-   [Predicate](/wiki/Predicate_(grammar) "Predicate (grammar)")
-   [Verb phrase](/wiki/Verb_phrase "Verb phrase")

Notes[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=13 "Edit section: Notes")]
-------------------------------------------------------------------------------------------------

1.  **[\^](#cite_ref-1)** Concerning the history of the dependency
    concept, see Percival (1990).
2.  **[\^](#cite_ref-2)** Concerning the influence of term logic on the
    theory of grammar, see Percival (1976).
3.  **[\^](#cite_ref-3)** Concerning early dependency grammars that may
    have developed independently of Tesnière's work, see for instance
    Hays (1960), Gaifman (1965), and Robinson (1970).
4.  **[\^](#cite_ref-4)** Some prominent dependency grammars that were
    well established by the 1980s are from Hudson (1984), Sgall,
    Hajičová et Panevova (1986), Mel’čuk (1988), and Starosta (1988).
5.  **[\^](#cite_ref-5)** Some prominent dependency grammars from the
    German schools are from Heringer (1996), Engel (1994), Eroms (2000),
    and Ágel et al. (2003/6) is a massive two volume collection of
    essays on dependency and valence grammars from more than 100
    authors.
6.  **[\^](#cite_ref-6)** The minimality of dependency structures is
    emphasized, for instance, by Ninio (2006) and by Osborne et al.
    (2011).
7.  **[\^](#cite_ref-7)** Concerning Tesnière's rejection of the
    subject-predicate division of the clause, see Tesnière
    (1959:103–105), and for discussion of empirical considerations that
    support Tesnière's point, see Matthews (2007:17ff.), Miller
    (2011:54ff.), and Osborne et al. (2011:323f.).
8.  **[\^](#cite_ref-8)** The conventions illustrated with trees (a) and
    (b) are preferred by Osborne et al. (2011, 2013).
9.  **[\^](#cite_ref-9)** Unordered trees like (d) are associated above
    all with Tesnière's stemmas and with the syntactic strata of
    Mel’čuk's Meaning-Text Theory.
10. **[\^](#cite_ref-10)** Three major works on Word Grammar are Hudson
    (1984, 1990, 2007).
11. **[\^](#cite_ref-11)** Lobin (2003) makes heavy use of he
    indentations.
12. **[\^](#cite_ref-12)** For a discussion of semantic, morphological,
    and syntactic dependencies in Meaning-Text Theary, see Melʹc̆uk
    (2003:191ff.).
13. **[\^](#cite_ref-13)** Concerning semantic dependencies, see Melʹc̆uk
    (2003:192f.).
14. **[\^](#cite_ref-14)** Concerning morphological dependencies, see
    Melʹc̆uk (2003:193ff.).
15. **[\^](#cite_ref-15)** The distinction between head- and
    dependent-marking was established by Nichols (1986). Nichols was
    using a dependency-based understanding of these distinctions.
16. **[\^](#cite_ref-16)** Concerning prosodic dependencies and the
    analysis of clitics, see Groß (2011).
17. **[\^](#cite_ref-17)** Distribution is primary principle used by
    Owens (1984:36), Schubert (1988:40), and Melʹc̆uk (2003:200) for
    discerning syntactic dependencies.
18. **[\^](#cite_ref-18)** Concerning the importance of the two ordering
    dimensions, see Tesnière (1959:16ff).
19. **[\^](#cite_ref-19)** See Osborne et al. (2012) concerning catenae.
20. **[\^](#cite_ref-20)** For discussion and examples of the labels for
    syntactic functions that are attached to dependency edges and arcs,
    see for instance Mel'cuk (1988:22, 69) and van Valin (2001:102ff.).

References[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=14 "Edit section: References")]
-----------------------------------------------------------------------------------------------------------

-   Ágel, Vilmos; Eichinger, Ludwig M.; Eroms, Hans Werner; Hellwig,
    Peter; Heringer, Hans Jürgen; Lobin, Henning, eds. (2003).
    [*Dependenz und Valenz:Ein internationales Handbuch der
    zeitgenössischen Forschung* [*Dependency and Valency:An
    International Handbook of Contemporary
    Research*]](http://www.degruyter.com/view/serial/18595) (in German).
    Berlin: de Gruyter.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [978-3110141900](/wiki/Special:BookSources/978-3110141900 "Special:BookSources/978-3110141900").
    Retrieved 24 August 2012.
-   Engel, U. 1994. Syntax der deutschen Sprache, 3rd edition. Berlin:
    Erich Schmidt Verlag.
-   Eroms, Hans-Werner (2000). [*Syntax der deutschen
    Sprache*](http://www.degruyter.com/view/product/5087?format=G).
    Berlin [u.a.]: de Gruyter.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [978-3110156669](/wiki/Special:BookSources/978-3110156669 "Special:BookSources/978-3110156669").
    Retrieved 24 August 2012.
-   Groß, T. 2011. Clitics in dependency morphology. Depling 2011
    Proceedings, 58-68.
-   Helbig, Gerhard; Buscha, Joachim (2007). [*Deutsche Grammatik: ein
    Handbuch für den Ausländerunterricht* [*German Grammar: A Guide for
    Foreigners
    Teaching*]](http://www.langenscheidt.de/produkt/2881_8731/Deutsche_Grammatik-Buch/978-3-468-49493-2)
    (6. [Dr.]. ed.). Berlin: Langenscheidt.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [978-3-468-49493-2](/wiki/Special:BookSources/978-3-468-49493-2 "Special:BookSources/978-3-468-49493-2").
    Retrieved 24 August 2012.
-   Heringer, H. 1996. Deutsche Syntax dependentiell. Tübingen:
    Stauffenburg.
-   Hays, D. 1960. Grouping and dependency theories. P-1910, RAND
    Corporation.
-   Hudson, Richard (1984). *Word grammar* (1. publ. ed.). Oxford, OX,
    England: B. Blackwell.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [978-0631131861](/wiki/Special:BookSources/978-0631131861 "Special:BookSources/978-0631131861").
-   Hudson, R. 1990. An English Word Grammar. Oxford: Basil Blackwell.
-   Hudson, R. 2007. Language Networks: The New Word Grammar. Oxford
    University Press.
-   Liu, H. 2009. Dependency Grammar: from Theory to Practice. Beijing:
    Science Press.
-   Lobin, H. 2003. Koordinationssyntax als prozedurales Phänomen.
    Tübingen: Gunter Narr-Verlag.
-   Matthews, P. H. (2007). [*Syntactic Relations: a critical
    survey*](http://www.cambridge.org/us/knowledge/isbn/item1157046) (1.
    publ. ed.). Cambridge: Cambridge University Press.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [9780521608299](/wiki/Special:BookSources/9780521608299 "Special:BookSources/9780521608299").
    Retrieved 24 August 2012.
-   Melʹc̆uk, Igor A. (1987). [*Dependency syntax : theory and
    practice*](http://www.sunypress.edu/p-164-dependency-syntax.aspx).
    Albany: State University Press of New York.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [978-0-88706-450-0](/wiki/Special:BookSources/978-0-88706-450-0 "Special:BookSources/978-0-88706-450-0").
    Retrieved 24 August 2012.
-   Melʹc̆uk, I. 2003. Levels of dependency in linguistic description:
    Concepts and problems. In Ágel et al., 170-187.
-   Miller, J. 2011. A critical introduction to syntax. London:
    continuum.
-   Nichols, J. 1986. Head-marking and dependent-marking languages.
    Language 62, 56-119.
-   Ninio, A. 2006. Language and the learning curve: A new theory of
    syntactic development. Oxford: Oxford University Press.
-   Osborne, T., M. Putnam, and T. Groß 2011. Bare phrase structure,
    label-less trees, and specifier-less syntax: Is Minimalism becoming
    a dependency grammar? The Linguistic Review 28, 315–364.
-   Osborne, T., M. Putnam, and T. Groß 2012. Catenae: Introducing a
    novel unit of syntactic analysis. Syntax 15, 4, 354-396.
-   Owens, J. 1984. On getting a head: A problem in dependency grammar.
    Lingua 66, 25-42.
-   Percival, K. 1976. On the historical source of immediate-constituent
    analysis. In: Notes from the linguistic underground, James McCawley
    (ed.), Syntax and Semantics 7, 229–242. New York: Academic Press.
-   Percival, K. 1990. Reflections on the history of dependency notions
    in linguistics. Historiographia Linguistica, 17, 29–47.
-   Robinson, J. 1970. Dependency structures and transformational rules.
    Language 46, 259–285.
-   Schubert, K. 1988. Metataxis: Contrastive dependency syntax for
    machine translation. Dordrecht: Foris.
-   Sgall, P., E. Hajičová, and J. Panevová 1986. The meaning of the
    sentence in its semantic and pragmatic aspects. Dordrecht: D. Reidel
    Publishing Company.
-   Starosta, S. 1988. The case for lexicase. London: Pinter Publishers.
-   Tesnière, L. 1959. Éléments de syntaxe structurale. Paris:
    Klincksieck.
-   Tesnière, L. 1969. Éléments de syntaxe structurale, 2nd edition.
    Paris: Klincksieck.
-   van Valin, R. 2001. An introduction to syntax. Cambridge, UK:
    Cambridge University Press.

Implementations[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=15 "Edit section: Implementations")]
---------------------------------------------------------------------------------------------------------------------

-   [DeSR](http://sites.google.com/site/desrparser/) A statistical
    shift/reduce dependency parser
-   [MaltParser](http://maltparser.org/) A system for data-driven
    dependency parsing
-   [MST Parser](http://sourceforge.net/projects/mstparser/) A
    non-projective dependency parser that searches for maximum spanning
    trees over directed graphs
-   [Mate Parser](http://code.google.com/p/mate-tools/) Joint
    non-projective labeled dependency parser and part-of-speech tagger
-   [MST Parser (C\#)](https://github.com/rasoolims/MSTParserCSharp/) A
    non-projective dependency parser that searches for maximum spanning
    trees over directed graphs (C\# conversion of the Java code)
-   [RelEx](http://wiki.opencog.org/w/RelEx) A parser that generates a
    dependency parse for the English language, by applying [graph
    rewriting](/wiki/Graph_rewriting "Graph rewriting") to the output of
    the [link grammar](/wiki/Link_grammar "Link grammar") parser. [Open
    source](/wiki/Open_source "Open source") license
-   [ClearParser](http://code.google.com/p/clearparser/) A statistical,
    transition-based dependency parser.
-   [Stanford parser](http://nlp.stanford.edu/software/lex-parser.shtml)
    A statistical phrase-structure parser which provides a tool to
    convert the output into a form of dependency graph called "Stanford
    Dependencies"
-   [TULE](http://www.parsit.it/tule.php) A linguistic framework that
    takes a natural language sentence in input (Italian) and returns a
    full dependency tree describing its syntactic structure
-   [XDG Development
    Kit](http://www.ps.uni-saarland.de/~rade/mogul/publish/doc/debusmann-xdk/)
    An integrated development environment for Extensible Dependency
    Grammar (XDG)

External links[[edit](/w/index.php?title=Dependency_grammar&action=edit&section=16 "Edit section: External links")]
-------------------------------------------------------------------------------------------------------------------

-   [Link Grammar online
    demonstration](http://www.link.cs.cmu.edu/link/submit-sentence-4.html)
-   [Extensible Dependency Grammar articles and grammar development
    kit](http://www.ps.uni-sb.de/~rade/xdg.html)
-   [Prague Dependency Treebank](http://ufal.mff.cuni.cz/pdt2.0/)
-   [Persian Dependency Treebank](http://dadegan.ir/en)
-   [Quranic Arabic Dependency Treebank](http://corpus.quran.com)
-   [Word Grammar](http://www.phon.ucl.ac.uk/home/dick/wg.htm)
-   [Depling 2011: The first International Conference on Dependency
    Linguistics](http://depling.org)

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=Dependency\_grammar&oldid=593068691](http://en.wikipedia.org/w/index.php?title=Dependency_grammar&oldid=593068691)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Grammar
    frameworks](/wiki/Category:Grammar_frameworks "Category:Grammar frameworks")
-   [Natural language
    parsing](/wiki/Category:Natural_language_parsing "Category:Natural language parsing")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Dependency+grammar&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Dependency+grammar "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Dependency_grammar "View the content page [c]")
-   [Talk](/wiki/Talk:Dependency_grammar "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Dependency_grammar)
-   [Edit](/w/index.php?title=Dependency_grammar&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Dependency_grammar&action=history "Past versions of this page [h]")

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
    here](/wiki/Special:WhatLinksHere/Dependency_grammar "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Dependency_grammar "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Dependency_grammar&oldid=593068691 "Permanent link to this revision of the page")
-   [Page
    information](/w/index.php?title=Dependency_grammar&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q674834 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Dependency_grammar&id=593068691 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Dependency+grammar)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Dependency+grammar&oldid=593068691&writer=rl)
-   [Printable
    version](/w/index.php?title=Dependency_grammar&printable=yes "Printable version of this page [p]")

### Languages

-   [Čeština](//cs.wikipedia.org/wiki/Z%C3%A1vislostn%C3%AD_syntax "Závislostní syntax – Czech")
-   [Deutsch](//de.wikipedia.org/wiki/Dependenzgrammatik "Dependenzgrammatik – German")
-   [Esperanto](//eo.wikipedia.org/wiki/Dependogramatikoj "Dependogramatikoj – Esperanto")
-   [فارسی](//fa.wikipedia.org/wiki/%D8%AF%D8%B3%D8%AA%D9%88%D8%B1_%D9%88%D8%A7%D8%A8%D8%B3%D8%AA%DA%AF%DB%8C "دستور وابستگی – Persian")
-   [Français](//fr.wikipedia.org/wiki/Grammaire_de_d%C3%A9pendance "Grammaire de dépendance – French")
-   [한국어](//ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4_%EB%AC%B8%EB%B2%95 "의존 문법 – Korean")
-   [Nederlands](//nl.wikipedia.org/wiki/Dependentiegrammatica "Dependentiegrammatica – Dutch")
-   [日本語](//ja.wikipedia.org/wiki/%E4%BE%9D%E5%AD%98%E6%96%87%E6%B3%95 "依存文法 – Japanese")
-   [Русский](//ru.wikipedia.org/wiki/%D0%93%D1%80%D0%B0%D0%BC%D0%BC%D0%B0%D1%82%D0%B8%D0%BA%D0%B0_%D0%B7%D0%B0%D0%B2%D0%B8%D1%81%D0%B8%D0%BC%D0%BE%D1%81%D1%82%D0%B5%D0%B9 "Грамматика зависимостей – Russian")
-   [Edit
    links](//www.wikidata.org/wiki/Q674834#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 30 January 2014 at 04:56.\
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
-   [Mobile view](//en.m.wikipedia.org/wiki/Dependency_grammar)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

