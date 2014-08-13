Pushdown automaton
==================

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

In [computer science](/wiki/Computer_science "Computer science"), a
**pushdown automaton** (**PDA**) is a type of
[automaton](/wiki/Automata_theory "Automata theory") that employs a
[stack](/wiki/Stack_(data_structure) "Stack (data structure)").

The PDA is used in theories about what can be computed by machines. It
is more capable than a [finite-state
machine](/wiki/Finite-state_machine "Finite-state machine") but less
capable than a [Turing machine](/wiki/Turing_machine "Turing machine").
Because its input can be described with a [formal
grammar](/wiki/Formal_grammar "Formal grammar"), it can be used in
[parser](/wiki/Parser "Parser") design. The [deterministic pushdown
automaton](/wiki/Deterministic_pushdown_automaton "Deterministic pushdown automaton")
can handle all [deterministic context-free
languages](/wiki/Deterministic_context-free_language "Deterministic context-free language")
while the nondeterministic version can handle all [context-free
languages](/wiki/Context-free_language "Context-free language").

The term "pushdown" refers to the fact that the
[stack](/wiki/Stack_(abstract_data_type) "Stack (abstract data type)")
can be regarded as being "pushed down" like a tray dispenser at a
cafeteria, since the operations never work on elements other than the
top element. A **stack automaton**, by contrast, does allow access to
and operations on deeper elements. Stack automata can recognize a
strictly larger set of languages than deterministic pushdown
automata.^[*[citation\\ needed](/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed")*]^
A [nested stack
automaton](/wiki/Nested_stack_automaton "Nested stack automaton") allows
full access, and also allows stacked values to be entire sub-stacks
rather than just single finite symbols.

The remainder of this article describes the nondeterministic pushdown
automaton.

Contents
--------

-   [1 Operation](#Operation)
-   [2 Relation to backtracking](#Relation_to_backtracking)
-   [3 Formal Definition](#Formal_Definition)
-   [4 Example](#Example)
-   [5 Understanding the computation
    process](#Understanding_the_computation_process)
-   [6 PDA and Context-free Languages](#PDA_and_Context-free_Languages)
-   [7 Generalized Pushdown Automaton
    (GPDA)](#Generalized_Pushdown_Automaton_.28GPDA.29)
-   [8 See also](#See_also)
-   [9 References](#References)
-   [10 External links](#External_links)

Operation[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=1 "Edit section: Operation")]
--------------------------------------------------------------------------------------------------------

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/7/71/Pushdown-overview.svg/340px-Pushdown-overview.svg.png)](/wiki/File:Pushdown-overview.svg)

[![image](//bits.wikimedia.org/static-1.23wmf10/skins/common/images/magnify-clip.png)](/wiki/File:Pushdown-overview.svg "Enlarge")

a diagram of the pushdown automaton

Pushdown automata differ from [finite state
machines](/wiki/Finite_state_machine "Finite state machine") in two
ways:

1.  They can use the top of the stack to decide which transition to
    take.
2.  They can manipulate the stack as part of performing a transition.

Pushdown automata choose a transition by indexing a table by input
signal, current state, and the symbol at the top of the stack. This
means that those three parameters completely determine the transition
path that is chosen. Finite state machines just look at the input signal
and the current state: they have no stack to work with. Pushdown
automata add the stack as a parameter for choice.

Pushdown automata can also manipulate the stack, as part of performing a
transition. Finite state machines choose a new state, the result of
following the transition. The manipulation can be to push a particular
symbol to the top of the stack, or to pop off the top of the stack. The
automaton can alternatively ignore the stack, and leave it as it is. The
choice of manipulation (or no manipulation) is determined by the
transition table.

Put together: Given an input signal, current state, and stack symbol,
the automaton can follow a transition to another state, and optionally
manipulate (push or pop) the stack.

In general, pushdown automata may have several computations on a given
input string, some of which may be halting in accepting configurations.
If only one computation exists for all accepted strings, the result is a
[deterministic pushdown
automaton](/wiki/Deterministic_pushdown_automaton "Deterministic pushdown automaton")
(DPDA) and the language of these strings is a [deterministic
context-free
language](/wiki/Deterministic_context-free_language "Deterministic context-free language").
Not all context-free languages are deterministic. As a consequence of
the above the DPDA is a strictly weaker variant of the PDA and there
exists no algorithm for converting a PDA to an equivalent DPDA, if such
a DPDA exists.

If we allow a finite automaton access to two stacks instead of just one,
we obtain a more powerful device, equivalent in power to a [Turing
machine](/wiki/Turing_machine "Turing machine"). A [linear bounded
automaton](/wiki/Linear_bounded_automaton "Linear bounded automaton") is
a device which is more powerful than a pushdown automaton but less so
than a Turing machine.

Relation to backtracking[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=2 "Edit section: Relation to backtracking")]
--------------------------------------------------------------------------------------------------------------------------------------

Nondeterministic PDAs are able to handle situations where more than one
choices of action are available. In principle it is enough to create in
every such case new automaton instances that will handle the extra
choices. The problem with this approach is that in practice most of
these instances quickly fail. This can severely affect the automaton's
performance as the execution of multiple instances is a costly
operation. Situations such as these can be identified in the design
phase of the automaton by examining the grammar the automaton uses. This
makes possible the use of
[backtracking](/wiki/Backtracking "Backtracking") in every such case in
order to improve the performance of pushdown automaton.

Formal Definition[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=3 "Edit section: Formal Definition")]
------------------------------------------------------------------------------------------------------------------------

We use standard formal language notation:
![\\Gamma\^{\*}](//upload.wikimedia.org/math/2/2/9/22939e5e9b86cc61d9d40f1b196d49b9.png)
denotes the set of strings over alphabet
![\\Gamma](//upload.wikimedia.org/math/1/6/2/162d4c413f99ae2763b1ced17ed1a14b.png)
and
![\\varepsilon](//upload.wikimedia.org/math/c/6/9/c691dc52cc1ad756972d4629934d37fd.png)
denotes the [empty string](/wiki/Empty_string "Empty string").

A PDA is formally defined as a 7-tuple:

![M=(Q,\\ \\Sigma,\\ \\Gamma,\\ \\delta, \\ q\_{0},\\ Z, \\
F)](//upload.wikimedia.org/math/6/c/3/6c3686ad49dc10eaf44a82295aaacf82.png)
where

-   ![\\, Q
    ](//upload.wikimedia.org/math/0/0/4/004079a9e10ff7052646221da1745005.png)
    is a finite set of *states*
-   ![\\,\\Sigma](//upload.wikimedia.org/math/e/1/2/e127018dd67c00ad772f6ac07c10f1f9.png)
    is a finite set which is called the *input alphabet*
-   ![\\,\\Gamma](//upload.wikimedia.org/math/c/3/6/c36cf7a5ff5ae130f941d892275fe9ca.png)
    is a finite set which is called the *stack alphabet*
-   ![\\,\\delta](//upload.wikimedia.org/math/f/d/7/fd7b6480cb3e83c62e0e54aad9169d0f.png)
    is a finite subset of ![Q \\times (\\Sigma \\cup\\{\\varepsilon\\})
    \\times \\Gamma \\times Q \\times \\Gamma\^\*
    ](//upload.wikimedia.org/math/3/9/3/393c9d89e6c01ef551ab00957b295503.png),
    the *transition relation*.
-   ![\\,q\_{0}\\in\\, Q
    ](//upload.wikimedia.org/math/c/7/7/c77628ede9d0da756c109f93ef6397f0.png)
    is the *start state*
-   ![\\
    Z\\in\\,\\Gamma](//upload.wikimedia.org/math/3/2/d/32ded299692efd4418cbeee14c77bb7e.png)
    is the *initial stack symbol*
-   ![F\\subseteq
    Q](//upload.wikimedia.org/math/2/e/f/2ef3b9c9ae290bb1868e0a3e1ab6dfdd.png)
    is the set of *accepting states*

An element ![(p,a,A,q,\\alpha) \\in
\\delta](//upload.wikimedia.org/math/6/5/f/65f4a3a54f86203a2646b1ed5a853b2f.png)
is a transition of
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png).
It has the intended meaning that
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png),
in state ![p \\in
Q](//upload.wikimedia.org/math/3/a/e/3aef59523e2c3da700d5b2e356d9c568.png),
with ![a \\in \\Sigma
\\cup\\{\\varepsilon\\}](//upload.wikimedia.org/math/5/b/a/5baf028f50fc0b27f2331f1e3b2c2ed8.png)
on the input and with ![A \\in
\\Gamma](//upload.wikimedia.org/math/6/f/2/6f2921d7dad84cb19aafeaa7e6a76e51.png)
as topmost stack symbol, may read
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png),
change the state to
![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png),
pop
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png),
replacing it by pushing ![\\alpha \\in
\\Gamma\^\*](//upload.wikimedia.org/math/0/8/7/0876a02be1c8a28cd2ce8c7b3c313e40.png).
The ![(\\Sigma
\\cup\\{\\varepsilon\\})](//upload.wikimedia.org/math/9/3/8/9382531826bae0520045ceb70a83df60.png)
component of the transition relation is used to formalize that the PDA
can either read a letter from the input, or proceed leaving the input
untouched.

In many texts the transition relation is replaced by an (equivalent)
formalization, where

-   ![\\,\\delta](//upload.wikimedia.org/math/f/d/7/fd7b6480cb3e83c62e0e54aad9169d0f.png)
    is the *transition function*, mapping ![Q \\times (\\Sigma
    \\cup\\{\\varepsilon\\}) \\times
    \\Gamma](//upload.wikimedia.org/math/a/1/4/a143ad004a14e46525894de683a95bf7.png)
    into finite subsets of ![Q \\times
    \\Gamma\^\*](//upload.wikimedia.org/math/1/1/b/11b96c10327aa57987f11a51278ce6fb.png).

Here
![\\delta(p,a,A)](//upload.wikimedia.org/math/3/a/0/3a078b571b8cd2d9b058253c1a617497.png)
contains all possible actions in state
![p](//upload.wikimedia.org/math/8/3/8/83878c91171338902e0fe0fb97a8c47a.png)
with
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
on the stack, while reading
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)
on the input. One writes ![(q,\\alpha) \\in
\\delta(p,a,A)](//upload.wikimedia.org/math/f/c/7/fc75f79ee5c2b35d6d27d188cbbf1d1e.png)
for the function precisely when ![(p,a,A,q,\\alpha)
\\in\\delta](//upload.wikimedia.org/math/6/5/f/65f4a3a54f86203a2646b1ed5a853b2f.png)
for the relation. Note that *finite* in this definition is essential.

***Computations***

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/c/c0/Pushdown-step.svg/200px-Pushdown-step.svg.png)](/wiki/File:Pushdown-step.svg)

[![image](//bits.wikimedia.org/static-1.23wmf10/skins/common/images/magnify-clip.png)](/wiki/File:Pushdown-step.svg "Enlarge")

a step of the pushdown automaton

In order to formalize the semantics of the pushdown automaton a
description of the current situation is introduced. Any 3-tuple
![(p,w,\\beta) \\in Q \\times \\Sigma\^\* \\times
\\Gamma\^\*](//upload.wikimedia.org/math/2/f/b/2fbaa6fc74e329f87b3755812b097d5a.png)
is called an instantaneous description (ID) of
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png),
which includes the current state, the part of the input tape that has
not been read, and the contents of the stack (topmost symbol written
first). The transition relation
![\\delta](//upload.wikimedia.org/math/f/1/0/f10f03c9836c36537d2539196058bfa2.png)
defines the step-relation
![\\vdash\_{M}](//upload.wikimedia.org/math/9/0/1/9011868b6d80e23ede9c1b8a3f98e367.png)
of
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)
on instantaneous descriptions. For instruction ![(p,a,A,q,\\alpha) \\in
\\delta](//upload.wikimedia.org/math/6/5/f/65f4a3a54f86203a2646b1ed5a853b2f.png)
there exists a step ![(p,ax,A\\gamma) \\vdash\_{M}
(q,x,\\alpha\\gamma)](//upload.wikimedia.org/math/d/0/e/d0ef7ed11a349166a7a5085d08d1b64c.png),
for every
![x\\in\\Sigma\^\*](//upload.wikimedia.org/math/5/4/3/543d710c9ba5362457f1885b906072e2.png)
and every ![\\gamma\\in
\\Gamma\^\*](//upload.wikimedia.org/math/0/4/e/04e9a4766e3f5bcb2807ef0f01f0cde8.png).

In general pushdown automata are nondeterministic meaning that in a
given instantaneous description
![(p,w,\\beta)](//upload.wikimedia.org/math/e/b/5/eb5af1bf45e763ed61b3c59737d3ee28.png)
there may be several possible steps. Any of these steps can be chosen in
a computation. With the above definition in each step always a single
symbol (top of the stack) is popped, replacing it with as many symbols
as necessary. As a consequence no step is defined when the stack is
empty.

Computations of the pushdown automaton are sequences of steps. The
computation starts in the initial state
![q\_{0}](//upload.wikimedia.org/math/2/3/c/23c18abe2316bff7f7241577a432f0d5.png)
with the initial stack symbol
![Z](//upload.wikimedia.org/math/2/1/c/21c2e59531c8710156d34a3c30ac81d5.png)
on the stack, and a string
![w](//upload.wikimedia.org/math/f/1/2/f1290186a5d0b1ceab27f4e77c0c5d68.png)
on the input tape, thus with initial description
![(q\_{0},w,Z)](//upload.wikimedia.org/math/5/8/8/58897a380d436fe022eef55a1e4a1520.png).
There are two modes of accepting. The pushdown automaton either accepts
by final state, which means after reading its input the automaton
reaches an accepting state (in
![F](//upload.wikimedia.org/math/8/0/0/800618943025315f869e4e1f09471012.png)),
or it accepts by empty stack
(![\\varepsilon](//upload.wikimedia.org/math/c/6/9/c691dc52cc1ad756972d4629934d37fd.png)),
which means after reading its input the automaton empties its stack. The
first acceptance mode uses the internal memory (state), the second the
external memory (stack).

Formally one defines

1.  ![L(M) = \\{ w\\in\\Sigma\^\* | (q\_{0},w,Z) \\vdash\_M\^\*
    (f,\\varepsilon,\\gamma)](//upload.wikimedia.org/math/3/d/1/3d1705b8f312e7d03074f044c6e7679f.png)
    with ![f \\in
    F](//upload.wikimedia.org/math/6/f/8/6f8cae978742b850f4f902332ea9e86c.png)
    and ![\\gamma \\in \\Gamma\^\*
    \\}](//upload.wikimedia.org/math/4/c/a/4ca9d07904af7d3e0a9817503da36165.png)
    (final state)
2.  ![N(M) = \\{ w\\in\\Sigma\^\* | (q\_{0},w,Z) \\vdash\_M\^\*
    (q,\\varepsilon,\\varepsilon)](//upload.wikimedia.org/math/7/1/c/71c8c414ba18e5571e92530c139ab63d.png)
    with ![q \\in Q
    \\}](//upload.wikimedia.org/math/c/1/3/c131f16c18b5205524b2c93675a7193a.png)
    (empty stack)

Here
![\\vdash\_M\^\*](//upload.wikimedia.org/math/0/b/d/0bda31f2c2f65f45b1e827862ff618c8.png)
represents the reflexive and transitive closure of the step relation
![\\vdash\_M](//upload.wikimedia.org/math/d/7/f/d7f65e51b60b54a69a7b3b2c944881e6.png)
meaning any number of consecutive steps (zero, one or more).

For each single pushdown automaton these two languages need to have no
relation: they may be equal but usually this is not the case. A
specification of the automaton should also include the intended mode of
acceptance. Taken over all pushdown automata both acceptance conditions
define the same family of languages.

**Theorem.** For each pushdown automaton
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)
one may construct a pushdown automaton
![M'](//upload.wikimedia.org/math/c/0/c/c0c8156de7a5455113e67f33c15182fb.png)
such that
![L(M)=N(M')](//upload.wikimedia.org/math/e/6/d/e6d94242d969e403d4f0fe5290a0813f.png),
and vice versa, for each pushdown automaton
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)
one may construct a pushdown automaton
![M'](//upload.wikimedia.org/math/c/0/c/c0c8156de7a5455113e67f33c15182fb.png)
such that
![N(M)=L(M')](//upload.wikimedia.org/math/f/7/3/f73d943df27013bee1ca2d150e6add67.png)

Example[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=4 "Edit section: Example")]
----------------------------------------------------------------------------------------------------

The following is the formal description of the PDA which recognizes the
language ![\\{0\^n1\^n \\mid n \\ge 0
\\}](//upload.wikimedia.org/math/a/4/2/a426636faf664c992fa958eb2c3da39d.png)
by final state:

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/3/3c/Pda-example.svg/250px-Pda-example.svg.png)](/wiki/File:Pda-example.svg)

[![image](//bits.wikimedia.org/static-1.23wmf10/skins/common/images/magnify-clip.png)](/wiki/File:Pda-example.svg "Enlarge")

PDA for ![\\{0\^n1\^n \\mid n \\ge
0\\}](//upload.wikimedia.org/math/a/4/2/a426636faf664c992fa958eb2c3da39d.png)
(by final state)

![M=(Q,\\ \\Sigma,\\ \\Gamma,\\ \\delta, \\ p,\\ Z, \\
F)](//upload.wikimedia.org/math/a/6/1/a612743e9060c301bd690e52a4aa570d.png),
where

**states:** ![Q = \\{ p,q,r
\\}](//upload.wikimedia.org/math/6/4/5/6455f1f1698313b5a847b2bf08709f2a.png)

**input alphabet:** ![\\Sigma = \\{0,
1\\}](//upload.wikimedia.org/math/f/0/1/f0154bda85e12ce4f84d77d7dc5c170c.png)

**stack alphabet:** ![\\Gamma = \\{A,
Z\\}](//upload.wikimedia.org/math/a/6/a/a6a67a6d616dd261e8ac0d750f718c73.png)

**start state:** ![q\_{0} =
p](//upload.wikimedia.org/math/0/c/0/0c047d4915573a6f88bda29f7e33c8dc.png)

**start stack symbol:**
![Z](//upload.wikimedia.org/math/2/1/c/21c2e59531c8710156d34a3c30ac81d5.png)

**accepting states:** ![F =
\\{r\\}](//upload.wikimedia.org/math/c/7/7/c77d0f048331e0e4463c43ab5a3ea143.png)

The transition relation
![\\delta](//upload.wikimedia.org/math/f/1/0/f10f03c9836c36537d2539196058bfa2.png)
consists of the following six instructions:

![(p,0,Z,p,AZ)](//upload.wikimedia.org/math/2/f/f/2ff4f4279d56b3609cad380c90381950.png),
![(p,0,A,p,AA)](//upload.wikimedia.org/math/5/d/1/5d19bd8e1b2292cbae0907ffb5680a37.png),
![(p,\\epsilon,Z,q,Z)](//upload.wikimedia.org/math/7/b/0/7b06d96834a0806f0b4b40ddd652f408.png),
![(p,\\epsilon,A,q,A)](//upload.wikimedia.org/math/f/4/f/f4ff1ea23216fa7cb2540254caf1831b.png),
![(q,1,A,q,\\epsilon)](//upload.wikimedia.org/math/2/3/5/235db9492e7919394a9f0791bb028307.png),
and
![(q,\\epsilon,Z,r,Z)](//upload.wikimedia.org/math/a/5/2/a52d2406b66cb93cf7cc4a1800c96b67.png).

In words, the first two instructions say that in state
![p](//upload.wikimedia.org/math/8/3/8/83878c91171338902e0fe0fb97a8c47a.png)
any time the symbol
![0](//upload.wikimedia.org/math/c/f/c/cfcd208495d565ef66e7dff9f98764da.png)
is read, one
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
is pushed onto the stack. Pushing symbol
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
on top of another
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
is formalized as replacing top
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
by
![AA](//upload.wikimedia.org/math/3/b/9/3b98e2dffc6cb06a89dcb0d5c60a0206.png)
(and similarly for pushing symbol
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
on top of a
![Z](//upload.wikimedia.org/math/2/1/c/21c2e59531c8710156d34a3c30ac81d5.png)).

The third and fourth instructions say that, at any moment the automaton
may move from state
![p](//upload.wikimedia.org/math/8/3/8/83878c91171338902e0fe0fb97a8c47a.png)
to state
![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png).

The fifth instruction says that in state
![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png),
for each symbol
![1](//upload.wikimedia.org/math/c/4/c/c4ca4238a0b923820dcc509a6f75849b.png)
read, one
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
is popped. Finally, the sixth instruction says that the machine may move
from state
![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png)
to accepting state
![r](//upload.wikimedia.org/math/4/b/4/4b43b0aee35624cd95b910189b3dc231.png)
only when the stack consists of a single
![Z](//upload.wikimedia.org/math/2/1/c/21c2e59531c8710156d34a3c30ac81d5.png).

There seems to be no generally used representation for PDA. Here we have
depicted the instruction
![(p,a,A,q,\\alpha)](//upload.wikimedia.org/math/4/3/3/4337c0f6e72608611c9e5ba0c42e515e.png)
by an edge from state
![p](//upload.wikimedia.org/math/8/3/8/83878c91171338902e0fe0fb97a8c47a.png)
to state
![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png)
labelled by ![a;
A/\\alpha](//upload.wikimedia.org/math/9/9/7/99799a31e0eb90718249bf04c14f28e0.png)
(read
![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png);
replace
![A](//upload.wikimedia.org/math/7/f/c/7fc56270e7a70fa81a5935b72eacbe29.png)
by
![\\alpha](//upload.wikimedia.org/math/b/c/c/bccfc7022dfb945174d9bcebad2297bb.png)).

Understanding the computation process[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=5 "Edit section: Understanding the computation process")]
----------------------------------------------------------------------------------------------------------------------------------------------------------------

[![image](//upload.wikimedia.org/wikipedia/commons/thumb/2/2e/Pda-steps.svg/340px-Pda-steps.svg.png)](/wiki/File:Pda-steps.svg)

[![image](//bits.wikimedia.org/static-1.23wmf10/skins/common/images/magnify-clip.png)](/wiki/File:Pda-steps.svg "Enlarge")

accepting computation for
![0011](//upload.wikimedia.org/math/a/e/2/ae2bac2e4b4da805d01b2952d7e35ba4.png)

The following illustrates how the above PDA computes on different input
strings. The subscript
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)
from the step symbol
![\\vdash](//upload.wikimedia.org/math/b/f/7/bf73c9341a48c47c84a48dad635ff940.png)
is here omitted.

\(a) Input string = 0011. There are various computations, depending on
the moment the move from state
![p](//upload.wikimedia.org/math/8/3/8/83878c91171338902e0fe0fb97a8c47a.png)
to state
![q](//upload.wikimedia.org/math/7/6/9/7694f4a66316e53c8cdd9d9954bd611d.png)
is made. Only one of these is accepting.

\(i) ![(p,0011,Z) \\vdash (q,0011,Z) \\vdash
(r,0011,Z)](//upload.wikimedia.org/math/8/3/5/835d97c7f74ef9f5f380b79828340aeb.png).
The final state is accepting, but the input is not accepted this way as
it has not been read.

\(ii) ![(p,0011,Z) \\vdash (p,011,AZ) \\vdash
(q,011,AZ)](//upload.wikimedia.org/math/8/4/c/84ceac5528d267b74613328cf4bcff20.png).
No further steps possible.

\(iii) ![(p,0011,Z) \\vdash (p,011,AZ) \\vdash (p,11,AAZ) \\vdash
(q,11,AAZ)](//upload.wikimedia.org/math/9/8/c/98ce9a01acc6c9a89a3e58571151f5d6.png)
![ \\vdash (q,1,AZ) \\vdash
(q,\\epsilon,Z)](//upload.wikimedia.org/math/2/2/9/22903b1d01946255d943dcd664924c11.png)
![ \\vdash
(r,\\epsilon,Z)](//upload.wikimedia.org/math/d/2/d/d2d29ba01b23656d190c47cacd785fc1.png).
Accepting computation: ends in accepting state, while complete input has
been read.

\(b) Input string = 00111. Again there are various computations. None of
these is accepting.

\(i) ![(p,00111,Z) \\vdash (q,00111,Z) \\vdash
(r,00111,Z)](//upload.wikimedia.org/math/b/5/b/b5b6eb1d6f696a864385c01e48972c1f.png).
The final state is accepting, but the input is not accepted this way as
it has not been read.

\(ii) ![(p,00111,Z) \\vdash (p,0111,AZ) \\vdash
(q,0111,AZ)](//upload.wikimedia.org/math/0/7/8/0781ac738ad55836662ff4ddf6ec34d1.png).
No further steps possible.

\(iii) ![(p,00111,Z) \\vdash (p,0111,AZ) \\vdash (p,111,AAZ) \\vdash
(q,111,AAZ)](//upload.wikimedia.org/math/b/8/e/b8e49514c376df86ed5767821a62a609.png)
![ \\vdash (q,11,AZ) \\vdash
(q,1,Z)](//upload.wikimedia.org/math/0/e/4/0e4b488f0c80b622fe9a21f3b9a6dd09.png)
![ \\vdash
(r,1,Z)](//upload.wikimedia.org/math/d/1/d/d1d3af723e7fec02176601f11ab2d1ac.png).
The final state is accepting, but the input is not accepted this way as
it has not been (completely) read.

PDA and Context-free Languages[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=6 "Edit section: PDA and Context-free Languages")]
--------------------------------------------------------------------------------------------------------------------------------------------------

Every context-free grammar can be transformed into an equivalent
pushdown automaton. The derivation process of the grammar is simulated
in a leftmost way. Where the grammar rewrites a nonterminal, the PDA
takes the topmost nonterminal from its stack and replaces it by the
right-hand part of a grammatical rule (expand). Where the grammar
generates a terminal symbol, the PDA reads a symbol from input when it
is the topmost symbol on the stack (match). In a sense the stack of the
PDA contains the unprocessed data of the grammar, corresponding to a
pre-order traversal of a derivation tree.

Technically, given a context-free grammar, the PDA is constructed as
follows.

1.  ![(1,\\varepsilon,A,1,\\alpha)](//upload.wikimedia.org/math/b/6/4/b64c362f5580b22086cf98ac7fc3db39.png)
    for each rule
    ![A\\to\\alpha](//upload.wikimedia.org/math/8/7/9/879116be3dcfc0b4ee9ea767a1ca58ce.png)
    (*expand*)
2.  ![(1,a,a,1,\\varepsilon)](//upload.wikimedia.org/math/6/9/7/697e9b6d9ef147a11ede419296d6f6d2.png)
    for each terminal symbol
    ![a](//upload.wikimedia.org/math/0/c/c/0cc175b9c0f1b6a831c399e269772661.png)
    (*match*)

As a result we obtain a single state pushdown automata, the state here
is
![1](//upload.wikimedia.org/math/c/4/c/c4ca4238a0b923820dcc509a6f75849b.png),
accepting the context-free language by empty stack. Its initial stack
symbol equals the axiom of the context-free grammar.

The converse, finding a grammar for a given PDA, is not that easy. The
trick is to code two states of the PDA into the nonterminals of the
grammar.

**Theorem.** For each pushdown automaton
![M](//upload.wikimedia.org/math/6/9/6/69691c7bdcc3ce6d5d8a1361f22d04ac.png)
one may construct a context-free grammar
![G](//upload.wikimedia.org/math/d/f/c/dfcf28d0734569a6a693bc8194de62bf.png)
such that
![N(M)=L(G)](//upload.wikimedia.org/math/6/9/0/6902b83426ddf055df7b09288f295b08.png).

Generalized Pushdown Automaton (GPDA)[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=7 "Edit section: Generalized Pushdown Automaton (GPDA)")]
----------------------------------------------------------------------------------------------------------------------------------------------------------------

A GPDA is a PDA which writes an entire string of some known length to
the stack or removes an entire string from the stack in one step.

A GPDA is formally defined as a 6-tuple:

![M=(Q,\\ \\Sigma,\\ \\Gamma,\\ \\delta, \\ q\_{0}, \\
F)](//upload.wikimedia.org/math/6/1/e/61e403c4979bbe5604e64955fe6c53ea.png)

where Q,
![\\Sigma\\,](//upload.wikimedia.org/math/c/b/4/cb4efae84f23aaf41fa73a2bf19e9068.png),
![\\Gamma\\,](//upload.wikimedia.org/math/9/c/0/9c057f7e7e7e8a781beff7d4a3f30980.png),
q~0~ and F are defined the same way as a PDA.

![\\,\\delta](//upload.wikimedia.org/math/f/d/7/fd7b6480cb3e83c62e0e54aad9169d0f.png):
![Q \\times \\Sigma\_{\\epsilon} \\times \\Gamma\^{\*} \\longrightarrow
P( Q \\times \\Gamma\^{\*}
)](//upload.wikimedia.org/math/f/0/f/f0f6e5fe6f6726cfecfcf17b58c65aa6.png)

is the transition function.

Computation rules for a GPDA are the same as a PDA except that the
a~i+1~'s and b~i+1~'s are now strings instead of symbols.

GPDA's and PDA's are equivalent in that if a language is recognized by a
PDA, it is also recognized by a GPDA and vice versa.

One can formulate an analytic proof for the equivalence of GPDA's and
PDA's using the following simulation:

Let
![\\delta](//upload.wikimedia.org/math/f/1/0/f10f03c9836c36537d2539196058bfa2.png)(q~1~,
w, x~1~x~2~...x~m~)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(q~2~, y~1~y~2~...y~n~) be a transition of the GPDA

where ![q\_1, q\_2 \\in
Q](//upload.wikimedia.org/math/1/f/4/1f48b2b789858d83ef0e8d40f94146ce.png),
![w
\\in\\Sigma\_{\\epsilon}](//upload.wikimedia.org/math/1/b/2/1b2010c57614809de6a09f831de92f04.png),
![x\_1,
x\_2,\\ldots,x\_m\\in\\Gamma\^{\*}](//upload.wikimedia.org/math/e/6/8/e6841b6bf36bdeb554f913bd5e03d212.png),
![m\\geq0](//upload.wikimedia.org/math/f/5/a/f5ab63b78a8897cff30a3dda456224f6.png),
![y\_1,
y\_2,\\ldots,y\_n\\in\\Gamma\^{\*}](//upload.wikimedia.org/math/c/f/8/cf838b2a51811aaf3e4acf4cad4e5358.png),
![n\\geq
0](//upload.wikimedia.org/math/e/2/d/e2dbd4b26d758137070f5b0edfc107d6.png).

Construct the following transitions for the PDA:

![\\delta\^{'}](//upload.wikimedia.org/math/4/e/0/4e0408dd53b5f0d350de03455f4f8f64.png)(q~1~,
w, x~1~)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(p~1~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png))

![\\delta\^{'}](//upload.wikimedia.org/math/4/e/0/4e0408dd53b5f0d350de03455f4f8f64.png)(p~1~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png),
x~2~)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(p~2~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png))

![\\vdots](//upload.wikimedia.org/math/7/0/e/70e4c7b983e3bc5849c2cfd6af4a431e.png)

![\\delta\^{'}](//upload.wikimedia.org/math/4/e/0/4e0408dd53b5f0d350de03455f4f8f64.png)(p~m-1~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png),
x~m~)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(p~m~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png))

![\\delta\^{'}](//upload.wikimedia.org/math/4/e/0/4e0408dd53b5f0d350de03455f4f8f64.png)(p~m~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png),
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png)
)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(p~m+1~, y~n~)

![\\delta\^{'}](//upload.wikimedia.org/math/4/e/0/4e0408dd53b5f0d350de03455f4f8f64.png)(p~m+1~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png),
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png)
)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(p~m+2~, y~n-1~)

![\\vdots](//upload.wikimedia.org/math/7/0/e/70e4c7b983e3bc5849c2cfd6af4a431e.png)

![\\delta\^{'}](//upload.wikimedia.org/math/4/e/0/4e0408dd53b5f0d350de03455f4f8f64.png)(p~m+n-1~,
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png),
![\\epsilon](//upload.wikimedia.org/math/c/5/0/c50b9e82e318d4c163e4b1b060f7daf5.png)
)
![\\longrightarrow](//upload.wikimedia.org/math/b/7/8/b78286a473d99ba959caf406cea7e493.png)
(q~2~, y~1~)

See also[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=8 "Edit section: See also")]
------------------------------------------------------------------------------------------------------

-   [Stack machine](/wiki/Stack_machine "Stack machine")
-   [Context-free
    grammar](/wiki/Context-free_grammar "Context-free grammar")
-   [Finite automaton](/wiki/Finite_automaton "Finite automaton")

References[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=9 "Edit section: References")]
----------------------------------------------------------------------------------------------------------

-   [Michael Sipser](/wiki/Michael_Sipser "Michael Sipser") (1997).
    *Introduction to the Theory of Computation*. PWS Publishing.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [0-534-94728-X](/wiki/Special:BookSources/0-534-94728-X "Special:BookSources/0-534-94728-X").
    Section 2.2: Pushdown Automata, pp.101–114.
-   Jean-Michel Autebert, Jean Berstel, Luc Boasson, [Context-Free
    Languages and Push-Down
    Automata](http://www-igm.univ-mlv.fr/~berstel/Articles/1997CFLPDA.pdf),
    in: G. Rozenberg, A. Salomaa (eds.), Handbook of Formal Languages,
    Vol. 1, Springer-Verlag, 1997, 111-174.

External links[[edit](/w/index.php?title=Pushdown_automaton&action=edit&section=10 "Edit section: External links")]
-------------------------------------------------------------------------------------------------------------------

-   [JFLAP](http://www.jflap.org), simulator for several types of
    automata including nondeterministic pushdown automata

-   [v](/wiki/Template:Formal_languages_and_grammars "Template:Formal languages and grammars")
-   [t](/wiki/Template_talk:Formal_languages_and_grammars "Template talk:Formal languages and grammars")
-   [e](//en.wikipedia.org/w/index.php?title=Template:Formal_languages_and_grammars&action=edit)

[Automata theory](/wiki/Automata_theory "Automata theory"): [formal
languages](/wiki/Formal_language "Formal language") and [formal
grammars](/wiki/Formal_grammar "Formal grammar")

[Chomsky hierarchy](/wiki/Chomsky_hierarchy "Chomsky hierarchy")

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
-   **Nondeterministic pushdown**
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
"[http://en.wikipedia.org/w/index.php?title=Pushdown\_automaton&oldid=590385963](http://en.wikipedia.org/w/index.php?title=Pushdown_automaton&oldid=590385963)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Automata
    theory](/wiki/Category:Automata_theory "Category:Automata theory")
-   [Models of
    computation](/wiki/Category:Models_of_computation "Category:Models of computation")

Hidden categories:

-   [All articles with unsourced
    statements](/wiki/Category:All_articles_with_unsourced_statements "Category:All articles with unsourced statements")
-   [Articles with unsourced statements from September
    2012](/wiki/Category:Articles_with_unsourced_statements_from_September_2012 "Category:Articles with unsourced statements from September 2012")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Pushdown+automaton&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Pushdown+automaton "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Pushdown_automaton "View the content page [c]")
-   [Talk](/wiki/Talk:Pushdown_automaton "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Pushdown_automaton)
-   [Edit](/w/index.php?title=Pushdown_automaton&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Pushdown_automaton&action=history "Past versions of this page [h]")

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
    here](/wiki/Special:WhatLinksHere/Pushdown_automaton "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Pushdown_automaton "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Pushdown_automaton&oldid=590385963 "Permanent link to this revision of the page")
-   [Page
    information](/w/index.php?title=Pushdown_automaton&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q751443 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Pushdown_automaton&id=590385963 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Pushdown+automaton)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Pushdown+automaton&oldid=590385963&writer=rl)
-   [Printable
    version](/w/index.php?title=Pushdown_automaton&printable=yes "Printable version of this page [p]")

### Languages

-   [العربية](//ar.wikipedia.org/wiki/%D8%A7%D9%88%D8%AA%D9%88%D9%85%D8%A7%D8%AA_%D8%A7%D9%84%D8%AF%D9%81%D8%B9_%D8%A7%D9%84%D8%B3%D9%81%D9%84%D9%8A "اوتومات الدفع السفلي – Arabic")
-   [Bosanski](//bs.wikipedia.org/wiki/Potisni_automat "Potisni automat – Bosnian")
-   [Català](//ca.wikipedia.org/wiki/Aut%C3%B2mat_amb_pila "Autòmat amb pila – Catalan")
-   [Čeština](//cs.wikipedia.org/wiki/Z%C3%A1sobn%C3%ADkov%C3%BD_automat "Zásobníkový automat – Czech")
-   [Deutsch](//de.wikipedia.org/wiki/Kellerautomat "Kellerautomat – German")
-   [Español](//es.wikipedia.org/wiki/Aut%C3%B3mata_con_pila "Autómata con pila – Spanish")
-   [فارسی](//fa.wikipedia.org/wiki/%D8%A7%D8%AA%D9%88%D9%85%D8%A7%D8%AA%D9%88%D9%86_%D9%BE%D8%B4%D8%AA%D9%87%E2%80%8C%D8%A7%DB%8C "اتوماتون پشته‌ای – Persian")
-   [Français](//fr.wikipedia.org/wiki/Automate_%C3%A0_pile "Automate à pile – French")
-   [한국어](//ko.wikipedia.org/wiki/%ED%91%B8%EC%8B%9C%EB%8B%A4%EC%9A%B4_%EC%9E%90%EB%8F%99_%EA%B8%B0%EA%B3%84 "푸시다운 자동 기계 – Korean")
-   [Hrvatski](//hr.wikipedia.org/wiki/Potisni_automat "Potisni automat – Croatian")
-   [Italiano](//it.wikipedia.org/wiki/Automa_a_pila "Automa a pila – Italian")
-   [עברית](//he.wikipedia.org/wiki/%D7%90%D7%95%D7%98%D7%95%D7%9E%D7%98_%D7%9E%D7%97%D7%A1%D7%A0%D7%99%D7%AA "אוטומט מחסנית – Hebrew")
-   [Македонски](//mk.wikipedia.org/wiki/%D0%9F%D0%BE%D1%82%D0%B8%D1%81%D0%B5%D0%BD_%D0%B0%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82 "Потисен автомат – Macedonian")
-   [Nederlands](//nl.wikipedia.org/wiki/Stapelautomaat "Stapelautomaat – Dutch")
-   [日本語](//ja.wikipedia.org/wiki/%E3%83%97%E3%83%83%E3%82%B7%E3%83%A5%E3%83%80%E3%82%A6%E3%83%B3%E3%83%BB%E3%82%AA%E3%83%BC%E3%83%88%E3%83%9E%E3%83%88%E3%83%B3 "プッシュダウン・オートマトン – Japanese")
-   [Polski](//pl.wikipedia.org/wiki/Automat_ze_stosem "Automat ze stosem – Polish")
-   [Português](//pt.wikipedia.org/wiki/Aut%C3%B4mato_com_pilha "Autômato com pilha – Portuguese")
-   [Русский](//ru.wikipedia.org/wiki/%D0%90%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82_%D1%81_%D0%BC%D0%B0%D0%B3%D0%B0%D0%B7%D0%B8%D0%BD%D0%BD%D0%BE%D0%B9_%D0%BF%D0%B0%D0%BC%D1%8F%D1%82%D1%8C%D1%8E "Автомат с магазинной памятью – Russian")
-   [Slovenčina](//sk.wikipedia.org/wiki/Z%C3%A1sobn%C3%ADkov%C3%BD_automat "Zásobníkový automat – Slovak")
-   [Српски /
    srpski](//sr.wikipedia.org/wiki/%D0%9F%D0%BE%D1%82%D0%B8%D1%81%D0%BD%D0%B8_%D0%B0%D1%83%D1%82%D0%BE%D0%BC%D0%B0%D1%82 "Потисни аутомат – Serbian")
-   [Srpskohrvatski /
    српскохрватски](//sh.wikipedia.org/wiki/Potisni_automat "Potisni automat – Serbo-Croatian")
-   [Suomi](//fi.wikipedia.org/wiki/Pinoautomaatti "Pinoautomaatti – Finnish")
-   [Українська](//uk.wikipedia.org/wiki/%D0%90%D0%B2%D1%82%D0%BE%D0%BC%D0%B0%D1%82_%D0%B7_%D0%BC%D0%B0%D0%B3%D0%B0%D0%B7%D0%B8%D0%BD%D0%BD%D0%BE%D1%8E_%D0%BF%D0%B0%D0%BC%E2%80%99%D1%8F%D1%82%D1%82%D1%8E "Автомат з магазинною пам’яттю – Ukrainian")
-   [中文](//zh.wikipedia.org/wiki/%E4%B8%8B%E6%8E%A8%E8%87%AA%E5%8A%A8%E6%9C%BA "下推自动机 – Chinese")
-   [Edit
    links](//www.wikidata.org/wiki/Q751443#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 12 January 2014 at 17:44.\
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
-   [Mobile view](//en.m.wikipedia.org/wiki/Pushdown_automaton)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

