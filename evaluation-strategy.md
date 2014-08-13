Evaluation strategy
===================

From Wikipedia, the free encyclopedia

Jump to: [navigation](#mw-navigation), [search](#p-search)

**Evaluation strategies**

-   [Strict evaluation](/wiki/Strict_evaluation "Strict evaluation")
    -   [Applicative order](/wiki/Applicative_order "Applicative order")
    -   [Call by value](/wiki/Call_by_value "Call by value")
    -   [Call by reference](/wiki/Call_by_reference "Call by reference")
    -   [Call by sharing](/wiki/Call_by_sharing "Call by sharing")
    -   [Call by
        copy-restore](/w/index.php?title=Call_by_copy-restore&action=edit&redlink=1 "Call by copy-restore (page does not exist)")

-   [Non-strict
    evaluation](/wiki/Non-strict_evaluation "Non-strict evaluation")
    -   [Normal
        order](/wiki/Evaluation_strategy#Normal_order "Evaluation strategy")
    -   [Call by name](/wiki/Call_by_name "Call by name")
    -   Call by need, [Lazy
        evaluation](/wiki/Lazy_evaluation "Lazy evaluation")
    -   [Call by macro
        expansion](/w/index.php?title=Call_by_macro_expansion&action=edit&redlink=1 "Call by macro expansion (page does not exist)")

-   [Nondeterministic
    strategies](/w/index.php?title=Nondeterministic_strategies&action=edit&redlink=1 "Nondeterministic strategies (page does not exist)")
    -   [Full-reduction](/w/index.php?title=Full-reduction&action=edit&redlink=1 "Full-reduction (page does not exist)")
    -   [Call by future](/wiki/Call_by_future "Call by future")
    -   [Optimistic
        evaluation](/w/index.php?title=Optimistic_evaluation&action=edit&redlink=1 "Optimistic evaluation (page does not exist)")

-   Other
    -   [Partial
        evaluation](/wiki/Partial_evaluation "Partial evaluation")
    -   [Remote evaluation](/wiki/Remote_evaluation "Remote evaluation")
    -   [Short-circuit
        evaluation](/wiki/Short-circuit_evaluation "Short-circuit evaluation")

-   [v](/wiki/Template:Evaluation_strategy "Template:Evaluation strategy")
-   [t](/w/index.php?title=Template_talk:Evaluation_strategy&action=edit&redlink=1 "Template talk:Evaluation strategy (page does not exist)")
-   [e](//en.wikipedia.org/w/index.php?title=Template:Evaluation_strategy&action=edit)

A [programming
language](/wiki/Programming_language "Programming language") uses an
evaluation strategy to determine **when** to evaluate the argument(s) of
a function call (for function, also read: operation, method, or
relation) and **what** kind of value to pass to the function. For
example, call-by-worth/pass-by-reference specifies that a function
application evaluates the argument before it proceeds to the evaluation
of the function's body and that it passes two capabilities to the
function, namely, the ability to look up the current value of the
argument and to modify it via an assignment
statement.^[[1]](#cite_note-1)^ The notion of [reduction
strategy](/wiki/Reduction_strategy_(lambda_calculus) "Reduction strategy (lambda calculus)")
in [lambda calculus](/wiki/Lambda_calculus "Lambda calculus") is similar
but distinct.

In practical terms, many modern programming languages have converged on
a call-by-worth, pass-by-reference strategy for function calls (C\#,
Java). Some older languages, especially unsafe languages such as C++,
combine several notions of parameter passing. Historically,
call-by-value and call-by-name date back to Algol 60, a language
designed in the late 1950s. Only purely functional languages such as
Clean and Haskell use by-need.

Contents
--------

-   [1 Strict evaluation](#Strict_evaluation)
    -   [1.1 Applicative order](#Applicative_order)
    -   [1.2 Call by value](#Call_by_value)
        -   [1.2.1 Implicit limitations](#Implicit_limitations)

    -   [1.3 Call by reference](#Call_by_reference)
    -   [1.4 Call by sharing](#Call_by_sharing)
    -   [1.5 Call by copy-restore](#Call_by_copy-restore)
    -   [1.6 Partial evaluation](#Partial_evaluation)

-   [2 Non-strict evaluation](#Non-strict_evaluation)
    -   [2.1 Normal order](#Normal_order)
    -   [2.2 Call by name](#Call_by_name)
    -   [2.3 Call by need](#Call_by_need)
    -   [2.4 Call by macro expansion](#Call_by_macro_expansion)

-   [3 Nondeterministic strategies](#Nondeterministic_strategies)
    -   [3.1 Full β-reduction](#Full_.CE.B2-reduction)
    -   [3.2 Call by future](#Call_by_future)
    -   [3.3 Optimistic evaluation](#Optimistic_evaluation)

-   [4 See also](#See_also)
-   [5 Notes](#Notes)
-   [6 References](#References)

Strict evaluation[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=1 "Edit section: Strict evaluation")]
-------------------------------------------------------------------------------------------------------------------------

Main article: [Strict
evaluation](/wiki/Strict_evaluation "Strict evaluation")

In *strict evaluation,* the arguments to a
[function](/wiki/Subroutine "Subroutine") are always evaluated
completely before the function is applied.

Under [Church encoding](/wiki/Church_encoding "Church encoding"), [eager
evaluation](/wiki/Eager_evaluation "Eager evaluation") of
[operators](/wiki/Operator_(programming) "Operator (programming)") maps
to strict evaluation of functions; for this reason, strict evaluation is
sometimes called "eager". Most existing programming languages use strict
evaluation for functions.

### Applicative order[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=2 "Edit section: Applicative order")]

*Applicative order* (or *leftmost
innermost*^[[2]](#cite_note-2)^^[[3]](#cite_note-3)^) evaluation refers
to an evaluation strategy in which the arguments of a function are
evaluated from left to right in a [post-order
traversal](/wiki/Post-order_traversal "Post-order traversal") of
reducible expressions
([redexes](//en.wiktionary.org/wiki/redex "wiktionary:redex")). Unlike
call-by-value, applicative order evaluation reduces terms within a
function body as much as possible before the function is applied.

### Call by value [[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=3 "Edit section: Call by value")]

*Call-by-value* evaluation is the most common evaluation strategy, used
in languages as different as
[C](/wiki/C_(programming_language) "C (programming language)") and
[Scheme](/wiki/Scheme_(programming_language) "Scheme (programming language)").
In call-by-value, the argument expression is evaluated, and the
resulting value is bound to the corresponding variable in the function
(frequently by copying the value into a new memory region). If the
function or procedure is able to assign values to its parameters, only
its local copy is assigned — that is, anything passed into a function
call is unchanged in the caller's
[scope](/wiki/Scope_(programming) "Scope (programming)") when the
function returns.

Call-by-value is not a single evaluation strategy, but rather the family
of evaluation strategies in which a function's argument is evaluated
before being passed to the function. While many programming languages
(such as Common Lisp, Eiffel and Java) that use call-by-value evaluate
function arguments left-to-right, some evaluate functions and their
arguments right-to-left, and others (such as Scheme, OCaml and C) leave
the order unspecified.

#### Implicit limitations[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=4 "Edit section: Implicit limitations")]

In some cases, the term "call-by-value" is problematic, as the value
which is passed is not the value of the variable as understood by the
ordinary meaning of value, but an implementation-specific
[reference](/wiki/Reference_(computer_science) "Reference (computer science)")
to the value. The effect is that what syntactically looks like
call-by-value may end up rather behaving like call-by-reference or
[call-by-sharing](#Call_by_sharing), often depending on very subtle
aspects of the language semantics.

The reason for passing a reference is often that the language
technically does not provide a value representation of complicated data,
but instead represents them as a data structure while preserving some
semblance of value appearance in the source code. Exactly where the
boundary is drawn between proper values and data structures masquerading
as such is often hard to predict. In
[C](/wiki/C_(programming_language) "C (programming language)"), a vector
(of which strings are special cases) is a data structure and thus
treated as a reference to a memory area, but a
[struct](/wiki/Struct_(C_programming_language) "Struct (C programming language)")
is a value even if it has fields that are vectors. In
[Maple](/wiki/Maple_(software) "Maple (software)"), a vector is a
special case of a table and therefore a data structure, but a list
(which gets rendered and can be indexed in exactly the same way) is a
value. In [Tcl](/wiki/Tcl "Tcl"), values are "dual-ported" such that the
value representation is used at the script level, and the language
itself manages the corresponding data structure, if one is required.
Modifications made via the data structure are reflected back to the
value representation, and vice-versa.

The description "call-by-value where the value is a reference" is common
(but should not be understood as being call-by-reference); another term
is [call-by-sharing](#Call_by_sharing). Thus the behaviour of
call-by-value Java or Visual Basic and call-by-value C or Pascal are
significantly different: in C or Pascal, calling a function with a large
structure as an argument will cause the entire structure to be copied
(except if it's actually a reference to a structure), *potentially*
causing serious performance degradation, and mutations to the structure
are invisible to the caller. However, in Java or Visual Basic only the
reference to the structure is copied, which is fast, and mutations to
the structure are visible to the caller.

### Call by reference [[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=5 "Edit section: Call by reference")]

In *call-by-reference* evaluation (also referred to as
*pass-by-reference*), a function receives an implicit
[reference](/wiki/Reference_(computer_science) "Reference (computer science)")
to a variable used as argument, rather than a copy of its value. This
typically means that the function can modify (i.e. [assign
to](/wiki/Assignment_(computer_science) "Assignment (computer science)"))
the variable used as argument—something that will be seen by its caller.
Call-by-reference can therefore be used to provide an additional channel
of communication between the called function and the calling function.
The same effect can be emulated in languages like C by passing a pointer
(not to be confused with call-by-reference), or in languages like Java
by passing a holding object, that can be set by the caller. A
call-by-reference language makes it more difficult for a programmer to
track the effects of a function call, and may introduce subtle bugs.

Many languages support call-by-reference in some form or another, but
comparatively few use it as a default, e.g. [Perl](/wiki/Perl "Perl"). A
few languages, such as [C++](/wiki/C%2B%2B "C++"),
[PHP](/wiki/PHP "PHP"), [Visual Basic
.NET](/wiki/Visual_Basic_.NET "Visual Basic .NET"),
[C\#](/wiki/C_Sharp_(programming_language) "C Sharp (programming language)")
and [REALbasic](/wiki/REALbasic "REALbasic"), default to call-by-value,
but offer special syntax for call-by-reference parameters. C++
additionally offers
call-by-reference-to-[const](/wiki/Const-correctness "Const-correctness").
In [purely functional](/wiki/Purely_functional "Purely functional")
languages there is typically no semantic difference between the two
strategies (since their data structures are immutable, so there is no
possibility for a function to modify any of its arguments), so they are
typically described as call-by-value even though implementations
frequently use call-by-reference internally for the efficiency benefits.

Even among languages that don't exactly support call-by-reference, many,
including [C](/wiki/C_(programming_language) "C (programming language)")
and [ML](/wiki/ML_(programming_language) "ML (programming language)"),
support explicit
[references](/wiki/Reference_(computer_science) "Reference (computer science)")
(objects that refer to other objects), such as
[pointers](/wiki/Pointer_(computer_programming) "Pointer (computer programming)")
(objects representing the memory addresses of other objects), and these
can be used to effect or simulate call-by-reference (but with the
complication that a function's caller must explicitly generate the
reference to supply as an argument).

Example that demonstrates call-by-reference in
[E](/wiki/E_(programming_language) "E (programming language)"):

~~~~ {.de1}
 def modify(var p, &q) {
     p := 27 # passed by value - only the local parameter is modified
     q := 27 # passed by reference - variable used in call is modified
 }
 
 ? var a := 1
 # value: 1
 ? var b := 2
 # value: 2
 ? modify(a,&b)
 ? a
 # value: 1
 ? b
 # value: 27
~~~~

Example that simulates call-by-reference in
[C](/wiki/C_(programming_language) "C (programming language)"):

~~~~ {.de1}
void Modify(int p, int * q, int * o)
{
    p = 27; // passed by value - only the local parameter is modified
    *q = 27; // passed by value or reference, check call site to determine which
    *o = 27; // passed by value or reference, check call site to determine which
}
int main()
{
    int a = 1;
    int b = 1;
    int x = 1;
    int * c = &x;
    Modify(a, &b, c);   // a is passed by value, b is passed by reference by creating a pointer,
                        // c is a pointer passed by value
    // b and x are changed
    return(0);
}
~~~~

### Call by sharing[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=6 "Edit section: Call by sharing")]

Also known as "call by object" or "call by object-sharing" is an
evaluation strategy first named by [Barbara
Liskov](/wiki/Barbara_Liskov "Barbara Liskov") et al. for the language
[CLU](/wiki/CLU_programming_language "CLU programming language") in
1974.^[[4]](#cite_note-CLU_Reference_Manual-4)^ It is used by languages
such as
[Python](/wiki/Python_(programming_language) "Python (programming language)"),^[[5]](#cite_note-Lundh_Call_By_Object-5)^
[Iota](/wiki/Iota_and_Jot "Iota and Jot"),
[Java](/wiki/Java_(programming_language) "Java (programming language)")
(for object references),^[[6]](#cite_note-6)^ Ruby, Scheme, OCaml,
[AppleScript](/wiki/AppleScript "AppleScript"), and many other
languages. However, the term "call by sharing" is not in common use; the
terminology is inconsistent across different sources. For example, in
the Java community, they say that Java is pass-by-value, whereas in the
Ruby community, they say that Ruby is
pass-by-reference^[*[citation\\ needed](/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed")*]^,
even though the two languages exhibit the same semantics. Call by
sharing implies that values in the language are based on objects rather
than [primitive types](/wiki/Primitive_types "Primitive types").

The semantics of call by sharing differ from call by reference in that
assignments to function arguments within the function aren't visible to
the caller (unlike by reference
semantics)^[*[citation\\ needed](/wiki/Wikipedia:Citation_needed "Wikipedia:Citation needed")*]^,
so e.g. if a variable was passed, it is not possible to simulate an
assignment on that variable in the caller's scope. However since the
function has access to the same object as the caller (no copy is made),
mutations to those objects, if the objects are
[mutable](/wiki/Mutable_object "Mutable object"), within the function
are visible to the caller, which may appear to differ from call by value
semantics. For [immutable
objects](/wiki/Immutable_object "Immutable object"), there is no real
difference between call by sharing and call by value, except for the
object identity. The use of call by sharing with mutable objects is an
alternative to [input/output
parameters](/wiki/Output_parameter "Output parameter"):^[[7]](#cite_note-CA1021-7)^
the parameter is not assigned to (the argument is not overwritten and
object identity is not changed), but the object (argument) is mutated.

For example in Python, lists are mutable, so:

~~~~ {.de1}
def f(l):
    l.append(1)
m = []
f(m)
print m
~~~~

...yields [1] because the argument `l` is mutated.

An important subtlety is the distinction between mutation and
assignment. In Python the code:

~~~~ {.de1}
def f(l):
    l += [1]
m = []
f(m)
print m
~~~~

...yields [1] because the statement `l += [1]` acts as `l.extend([1])`,
but the similar code:

~~~~ {.de1}
def f(l):
    l = l + [1]
m = []
f(m)
print m
~~~~

...yields [] because the statement `l = l + [1]` creates a new local
variable, rather than mutating the argument.^[[a]](#cite_note-8)^

Although this term has widespread usage in the Python community,
identical semantics in other languages such as Java and Visual Basic are
often described as call by value, where the value is implied to be a
reference to the object.

### Call by copy-restore[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=7 "Edit section: Call by copy-restore")]

*Call-by-copy-restore*, *copy-in copy-out*, *call-by-value-result* or
*call-by-value-return* (as termed in the
[Fortran](/wiki/Fortran "Fortran") community) is a special case of
call-by-reference where the provided reference is unique to the caller.
This variant has gained attention in multiprocessing contexts and
[Remote procedure
call](/wiki/Remote_procedure_call "Remote procedure call"): if a
parameter to a function call is a reference that might be accessible by
another thread of execution, its contents may be copied to a new
reference that is not; when the function call returns, the updated
contents of this new reference are copied back to the original reference
("restored").

The semantics of call-by-copy-restore also differ from those of
call-by-reference where two or more function arguments
[alias](/wiki/Aliasing_(computing) "Aliasing (computing)") one another;
that is, point to the same variable in the caller's environment. Under
call-by-reference, writing to one will affect the other;
call-by-copy-restore avoids this by giving the function distinct copies,
but leaves the result in the caller's environment
[undefined](/wiki/Undefined_behaviour "Undefined behaviour") depending
on which of the aliased arguments is copied back first - will the copies
be made in left-to-right order both on entry and on return?

When the reference is passed to the callee uninitialized, this
evaluation strategy may be called *call-by-result*.

### Partial evaluation[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=8 "Edit section: Partial evaluation")]

Main article: [Partial
evaluation](/wiki/Partial_evaluation "Partial evaluation")

In *partial evaluation*, evaluation may continue into the body of a
function that has not been applied. Any sub-expressions that do not
contain unbound variables are evaluated, and function applications whose
argument values are known may be reduced. In the presence of
side-effects, complete partial evaluation may produce unintended
results; for this reason, systems that support partial evaluation tend
to do so only for "pure" expressions (expressions without side-effects)
within functions.

Non-strict evaluation[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=9 "Edit section: Non-strict evaluation")]
---------------------------------------------------------------------------------------------------------------------------------

[![Question
book-new.svg](//upload.wikimedia.org/wikipedia/en/thumb/9/99/Question_book-new.svg/50px-Question_book-new.svg.png)](/wiki/File:Question_book-new.svg)

This section **does not
[cite](/wiki/Wikipedia:Citing_sources "Wikipedia:Citing sources") any
[references or
sources](/wiki/Wikipedia:Verifiability "Wikipedia:Verifiability")**.
Please help improve this section by [adding citations to reliable
sources](/wiki/Help:Introduction_to_referencing/1 "Help:Introduction to referencing/1").
Unsourced material may be challenged and
[removed](/wiki/Wikipedia:Verifiability#Burden_of_evidence "Wikipedia:Verifiability").
*(June 2013)*

In *non-strict evaluation,* arguments to a function are not evaluated
unless they are actually used in the evaluation of the function body.

Under Church encoding, [lazy
evaluation](/wiki/Lazy_evaluation "Lazy evaluation") of operators maps
to non-strict evaluation of functions; for this reason, non-strict
evaluation is often referred to as "lazy". Boolean expressions in many
languages use a form of non-strict evaluation called [short-circuit
evaluation](/wiki/Short-circuit_evaluation "Short-circuit evaluation"),
where evaluation returns as soon as it can be determined that an
unambiguous Boolean will result — for example, in a disjunctive
expression where *true* is encountered, or in a conjunctive expression
where *false* is encountered, and so forth. Conditional expressions also
usually use lazy evaluation, where evaluation returns as soon as an
unambiguous branch will result.

### Normal order[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=10 "Edit section: Normal order")]

*Normal-order* (or *leftmost outermost*) evaluation is the evaluation
strategy where the outermost redex is always reduced, applying functions
before evaluating function arguments.

In contrast, a call-by-name strategy does not evaluate inside the body
of an unapplied function.

### Call by name[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=11 "Edit section: Call by name")]

In *call-by-name* evaluation, the arguments to a function are not
evaluated before the function is called — rather, they are substituted
directly into the function body (using [capture-avoiding
substitution](/wiki/Capture-avoiding_substitution "Capture-avoiding substitution"))
and then left to be evaluated whenever they appear in the function. If
an argument is not used in the function body, the argument is never
evaluated; if it is used several times, it is re-evaluated each time it
appears. (See [Jensen's
Device](/wiki/Jensen%27s_Device "Jensen's Device").)

Call-by-name evaluation is occasionally preferable to call-by-value
evaluation. If a function's argument is not used in the function,
call-by-name will save time by not evaluating the argument, whereas
call-by-value will evaluate it regardless. If the argument is a
non-terminating computation, the advantage is enormous. However, when
the function argument is used, call-by-name is often slower, requiring a
mechanism such as a
[thunk](/wiki/Thunk_(functional_programming)#Call_by_name "Thunk (functional programming)").

An early use was [ALGOL 60](/wiki/ALGOL_60 "ALGOL 60"). .NET languages
can simulate call-by-name using delegates or Expression<T\> parameters.
The latter results in an abstract syntax tree being given to the
function.
[Eiffel](/wiki/Eiffel_(programming_language) "Eiffel (programming language)")
provides agents, which represents an operation to be evaluated when
needed. [Seed7](/wiki/Seed7 "Seed7") provides call-by-name with function
parameters.

### Call by need[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=12 "Edit section: Call by need")]

Main article: [Lazy evaluation](/wiki/Lazy_evaluation "Lazy evaluation")

*Call-by-need* is a [memoized](/wiki/Memoization "Memoization") version
of call-by-name where, *if the function argument is evaluated,* that
value is stored for subsequent uses. In a "pure" (effect-free) setting,
this produces the same results as call-by-name; when the function
argument is used two or more times, call-by-need is almost always
faster.

Because evaluation of expressions may happen arbitrarily far into a
computation, languages using call-by-need generally do not support
computational effects (such as
[mutation](/wiki/Mutable_object "Mutable object")) except through the
use of
[monads](/wiki/Monads_in_functional_programming "Monads in functional programming")
and [uniqueness types](/wiki/Uniqueness_type "Uniqueness type"). This
eliminates any unexpected behavior from variables whose values change
prior to their delayed evaluation.

[Lazy evaluation](/wiki/Lazy_evaluation "Lazy evaluation") is the most
commonly used implementation strategy for call-by-need semantics, but
variations exist — for instance [optimistic
evaluation](#Optimistic_evaluation).

[Haskell](/wiki/Haskell_(programming_language) "Haskell (programming language)")
is the best-known language that uses call-by-need evaluation.
[R](/wiki/R_(programming_language) "R (programming language)") also uses
a form of call-by-need. .NET languages can simulate call-by-need using
the type `Lazy<T>`.

### Call by macro expansion[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=13 "Edit section: Call by macro expansion")]

*Call-by-macro-expansion* is similar to call-by-name, but uses textual
substitution rather than capture-avoiding substitution. With uncautious
use, macro substitution may result in [variable
capture](/w/index.php?title=Variable_capture&action=edit&redlink=1 "Variable capture (page does not exist)")
and lead to undesired behavior. [Hygienic
macros](/wiki/Hygienic_macros "Hygienic macros") avoid this problem by
checking for and replacing shadowed variables that are not parameters.

Nondeterministic strategies[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=14 "Edit section: Nondeterministic strategies")]
----------------------------------------------------------------------------------------------------------------------------------------------

### Full β-reduction[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=15 "Edit section: Full β-reduction")]

Under *full β-reduction,* any function application may be reduced
(substituting the function's argument into the function using
capture-avoiding substitution) at any time. This may be done even within
the body of an unapplied function.

### Call by future[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=16 "Edit section: Call by future")]

See also: [Futures and
promises](/wiki/Futures_and_promises "Futures and promises")

*Call-by-future* (or *parallel call-by-name*) is a
[concurrent](/wiki/Concurrent_programming "Concurrent programming")
evaluation strategy: the value of a
[future](/wiki/Futures_and_promises "Futures and promises") expression
is computed concurrently with the flow of the rest of the program. When
the value of the future is needed, the main program blocks until the
future finishes computing, if it has not already completed by then.

This strategy is non-deterministic, as the evaluation can occur at any
time between when the future is created (when the expression is given)
and when the value of the future is used. It is similar to call-by-need
in that the value is only computed once, and computation may be deferred
until the value is needed, but it may be started before. Further, if the
value of a future is not needed, such as if it is a local variable in a
function that returns, the computation may be terminated part-way
through.

If implemented with processes or threads, creating a future will spawn a
new process or thread, accessing the value will synchronize this with
the main thread, and terminating the computation of the future
corresponds to killing the thread computing its value.

### Optimistic evaluation[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=17 "Edit section: Optimistic evaluation")]

*Optimistic evaluation* is another variant of call-by-need in which the
function's argument is partially evaluated for some amount of time
(which may be adjusted at runtime), after which evaluation is aborted
and the function is applied using call-by-need. This approach avoids
some of the runtime expense of call-by-need, while still retaining the
desired termination characteristics.

See also[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=18 "Edit section: See also")]
--------------------------------------------------------------------------------------------------------

-   [Beta normal form](/wiki/Beta_normal_form "Beta normal form")
-   [Comparison of programming
    languages](/wiki/Comparison_of_programming_languages "Comparison of programming languages")
-   [Lambda calculus](/wiki/Lambda_calculus "Lambda calculus")
-   [Parameter (computer
    science)](/wiki/Parameter_(computer_science) "Parameter (computer science)")
-   [Eval()](/wiki/Eval() "Eval()")

Notes[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=19 "Edit section: Notes")]
--------------------------------------------------------------------------------------------------

1.  **[\^](#cite_ref-8)** As a matter of Python syntax, note that
    `l += x` is not equivalent to `l = l + x` – semantically it is a
    mutator, not an assignment. Further, `l += x` is not literally
    equivalent to `l.extend(x)` either, due to scoping issues: `l += x`
    requires that `l` be in the local scope, while `l.extend(x)` looks
    to enclosing scopes too.

References[[edit](/w/index.php?title=Evaluation_strategy&action=edit&section=20 "Edit section: References")]
------------------------------------------------------------------------------------------------------------

![image](//upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Text_document_with_red_question_mark.svg/40px-Text_document_with_red_question_mark.svg.png)

This article includes a [list of
references](/wiki/Wikipedia:Citing_sources "Wikipedia:Citing sources"),
but **its sources remain unclear because it has insufficient [inline
citations](/wiki/Wikipedia:Citing_sources#Inline_citations "Wikipedia:Citing sources")**.
Please help to
[improve](/wiki/Wikipedia:WikiProject_Fact_and_Reference_Check "Wikipedia:WikiProject Fact and Reference Check")
this article by
[introducing](/wiki/Wikipedia:When_to_cite "Wikipedia:When to cite")
more precise citations. *(April 2012)*

1.  **[\^](#cite_ref-1)** Essentials of Programming Languages by Daniel
    P. Friedman and Mitchell Wand, MIT Press 1989--2006
2.  **[\^](#cite_ref-2)** ["Lambda
    Calculus"](https://www.cs.uiowa.edu/~hzhang/c123/Lecture5.pdf).
    Cs.uiowa.edu. Retrieved 2013-08-18.
3.  **[\^](#cite_ref-3)** ["applicative order reduction definition of
    applicative order reduction in the Free Online
    Encyclopedia"](http://encyclopedia2.thefreedictionary.com/applicative+order+reduction).
    Encyclopedia2.thefreedictionary.com. Retrieved 2013-08-18.
4.  **[\^](#cite_ref-CLU_Reference_Manual_4-0)** Liskov, Barbara;
    Atkinson, Russ; Bloom, Toby; Moss, Eliot; Schaffert, Craig;
    Scheifler, Craig; Snyder, Alan (October 1979). ["CLU Reference
    Manual"](http://www.lcs.mit.edu/publications/pubs/pdf/MIT-LCS-TR-225.pdf)
    (PDF). *Laboratory for Computer Science*. Massachusetts Institute of
    Technology. Retrieved 2011-05-19.
5.  **[\^](#cite_ref-Lundh_Call_By_Object_5-0)** Lundh, Fredrik. ["Call
    By Object"](http://effbot.org/zone/call-by-object.htm).
    *effbot.org*. Retrieved 2011-05-19.
6.  **[\^](#cite_ref-6)** ["Iota Language
    Definition"](http://www.cs.cornell.edu/courses/cs412/2001sp/iota/iota.html).
    *CS 412/413 Introduction to Compilers*. Cornell University. 2001.
    Retrieved 2011-05-19.
7.  **[\^](#cite_ref-CA1021_7-0)** [CA1021: Avoid out
    parameters](http://msdn.microsoft.com/en-us/library/ms182131.aspx)

-   [Abelson, Harold](/wiki/Hal_Abelson "Hal Abelson"); [Sussman, Gerald
    Jay](/wiki/Gerald_Jay_Sussman "Gerald Jay Sussman") (1996).
    [*Structure and Interpretation of Computer
    Programs*](http://mitpress.mit.edu/sicp/full-text/book/book.html)
    (Second ed.). Cambridge, Massachusetts: The MIT Press.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [978-0-262-01153-2](/wiki/Special:BookSources/978-0-262-01153-2 "Special:BookSources/978-0-262-01153-2").
-   Baker-Finch, Clem; King, David; Hall, Jon; Trinder, Phil
    (1999-03-10). ["An Operational Semantics for Parallel
    Call-by-Need"](http://cs.anu.edu.au/people/Clem.Baker-Finch/Research/par-cbn-tr/)
    (ps). *Research report* (Faculty of Mathematics & Computing, The
    Open University) **99** (1).
-   Ennals, Robert; Peyton Jones, Simon (2003). ["Optimistic Evaluation:
    a fast evaluation strategy for non-strict
    programs"](http://research.microsoft.com/en-us/um/people/simonpj/Papers/optimistic/icfp2003.pdf)
    (PDF). International Conference on Functional Programming. ACM
    Press.
-   Ludäscher, Bertram (2001-01-24). ["CSE 130 lecture
    notes"](http://users.sdsc.edu/~ludaesch/CSE130/ln5.html). *CSE 130:
    Programming Languages: Principles & Paradigms*.
-   [Pierce, Benjamin C.](/wiki/Benjamin_C._Pierce "Benjamin C. Pierce")
    (2002). *[Types and Programming
    Languages](/wiki/Types_and_Programming_Languages "Types and Programming Languages")*.
    [MIT Press](/wiki/MIT_Press "MIT Press").
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [0-262-16209-1](/wiki/Special:BookSources/0-262-16209-1 "Special:BookSources/0-262-16209-1").
-   Sestoft, Peter (2002). [*Demonstrating Lambda Calculus
    Reduction*](http://www.dina.kvl.dk/~sestoft/papers/sestoft-lamreduce.pdf)
    (PDF). In Mogensen, T; Schmidt, D; Sudborough, I. H. "The essence of
    computation : complexity, analysis, trnasformation : essays
    dedicated to Neil D. Jones". *The Essence of Computation:
    Complexity, Analysis, Transformation. Essays Dedicated to Neil D.
    Jones*. Lecture Notes in Computer Science **2566**
    (Springer-Verlag). pp. 420–435.
    [ISBN](/wiki/International_Standard_Book_Number "International Standard Book Number")
    [3-540-00326-6](/wiki/Special:BookSources/3-540-00326-6 "Special:BookSources/3-540-00326-6").
-   ["Call by Value and Call by Reference in C
    Programming"](http://digg.com/newsbar/topnews/c_programming_lesson_call_by_value_and_call_by_reference).
    *Call by Value and Call by Reference in C Programming explained*.

![image](//en.wikipedia.org/wiki/Special:CentralAutoLogin/start?type=1x1)

Retrieved from
"[http://en.wikipedia.org/w/index.php?title=Evaluation\_strategy&oldid=583605541](http://en.wikipedia.org/w/index.php?title=Evaluation_strategy&oldid=583605541)"

[Categories](/wiki/Help:Category "Help:Category"):

-   [Evaluation
    strategy](/wiki/Category:Evaluation_strategy "Category:Evaluation strategy")
-   [Programming language
    topics](/wiki/Category:Programming_language_topics "Category:Programming language topics")

Hidden categories:

-   [All articles with unsourced
    statements](/wiki/Category:All_articles_with_unsourced_statements "Category:All articles with unsourced statements")
-   [Articles with unsourced statements from January
    2011](/wiki/Category:Articles_with_unsourced_statements_from_January_2011 "Category:Articles with unsourced statements from January 2011")
-   [Articles with unsourced statements from October
    2009](/wiki/Category:Articles_with_unsourced_statements_from_October_2009 "Category:Articles with unsourced statements from October 2009")
-   [Articles needing additional references from June
    2013](/wiki/Category:Articles_needing_additional_references_from_June_2013 "Category:Articles needing additional references from June 2013")
-   [All articles needing additional
    references](/wiki/Category:All_articles_needing_additional_references "Category:All articles needing additional references")
-   [Articles lacking in-text citations from April
    2012](/wiki/Category:Articles_lacking_in-text_citations_from_April_2012 "Category:Articles lacking in-text citations from April 2012")
-   [All articles lacking in-text
    citations](/wiki/Category:All_articles_lacking_in-text_citations "Category:All articles lacking in-text citations")

Navigation menu
---------------

### Personal tools

-   [Create
    account](/w/index.php?title=Special:UserLogin&returnto=Evaluation+strategy&type=signup)
-   [Log
    in](/w/index.php?title=Special:UserLogin&returnto=Evaluation+strategy "You're encouraged to log in; however, it's not mandatory. [o]")

### Namespaces

-   [Article](/wiki/Evaluation_strategy "View the content page [c]")
-   [Talk](/wiki/Talk:Evaluation_strategy "Discussion about the content page [t]")

### 

### Variants[](#)

### Views

-   [Read](/wiki/Evaluation_strategy)
-   [Edit](/w/index.php?title=Evaluation_strategy&action=edit "You can edit this page. 
    Please review your changes before saving. [e]")
-   [View
    history](/w/index.php?title=Evaluation_strategy&action=history "Past versions of this page [h]")

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
    here](/wiki/Special:WhatLinksHere/Evaluation_strategy "List of all English Wikipedia pages containing links to this page [j]")
-   [Related
    changes](/wiki/Special:RecentChangesLinked/Evaluation_strategy "Recent changes in pages linked from this page [k]")
-   [Upload file](/wiki/Wikipedia:File_Upload_Wizard "Upload files [u]")
-   [Special
    pages](/wiki/Special:SpecialPages "A list of all special pages [q]")
-   [Permanent
    link](/w/index.php?title=Evaluation_strategy&oldid=583605541 "Permanent link to this revision of the page")
-   [Page
    information](/w/index.php?title=Evaluation_strategy&action=info)
-   [Data
    item](//www.wikidata.org/wiki/Q2881121 "Link to connected data repository item [g]")
-   [Cite this
    page](/w/index.php?title=Special:Cite&page=Evaluation_strategy&id=583605541 "Information on how to cite this page")

### Print/export

-   [Create a
    book](/w/index.php?title=Special:Book&bookcmd=book_creator&referer=Evaluation+strategy)
-   [Download as
    PDF](/w/index.php?title=Special:Book&bookcmd=render_article&arttitle=Evaluation+strategy&oldid=583605541&writer=rl)
-   [Printable
    version](/w/index.php?title=Evaluation_strategy&printable=yes "Printable version of this page [p]")

### Languages

-   [Deutsch](//de.wikipedia.org/wiki/Auswertung_(Informatik) "Auswertung (Informatik) – German")
-   [Nederlands](//nl.wikipedia.org/wiki/Call-by-name "Call-by-name – Dutch")
-   [日本語](//ja.wikipedia.org/wiki/%E8%A9%95%E4%BE%A1%E6%88%A6%E7%95%A5 "評価戦略 – Japanese")
-   [Português](//pt.wikipedia.org/wiki/Estrat%C3%A9gia_de_avalia%C3%A7%C3%A3o "Estratégia de avaliação – Portuguese")
-   [Русский](//ru.wikipedia.org/wiki/%D0%9F%D0%B5%D1%80%D0%B5%D0%B4%D0%B0%D1%87%D0%B0_%D0%BF%D0%B0%D1%80%D0%B0%D0%BC%D0%B5%D1%82%D1%80%D0%B0_(%D0%BF%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5) "Передача параметра (программирование) – Russian")
-   [Slovenčina](//sk.wikipedia.org/wiki/Vyhodnocovacia_strat%C3%A9gia "Vyhodnocovacia stratégia – Slovak")
-   [中文](//zh.wikipedia.org/wiki/%E6%B1%82%E5%80%BC%E7%AD%96%E7%95%A5 "求值策略 – Chinese")
-   [Edit
    links](//www.wikidata.org/wiki/Q2881121#sitelinks-wikipedia "Edit interlanguage links")

-   This page was last modified on 28 November 2013 at 01:20.\
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
-   [Mobile view](//en.m.wikipedia.org/wiki/Evaluation_strategy)

-   [![Wikimedia
    Foundation](//bits.wikimedia.org/images/wikimedia-button.png)](//wikimediafoundation.org/)
-   [![Powered by
    MediaWiki](//bits.wikimedia.org/static-1.23wmf11/skins/common/images/poweredby_mediawiki_88x31.png)](//www.mediawiki.org/)

