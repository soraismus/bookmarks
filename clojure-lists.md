Data Structures in Clojure: Singly-Linked Lists – Macromancy

[«](/index.html)

Data Structures in Clojure: Singly-Linked Lists
===============================================

[Max Countryman](https://twitter.com/MaxCountryman)
---------------------------------------------------

### January 16, 2014

About this Series
-----------------

This series is about the implementation of common data structures.
Throughout the series we will be implementing these data structures
ourselves, exploring how they work. Our implementations will be done in
Clojure. Consequently this tutorial is also about Lisp and Clojure. It
is important to note that, unlike the standard collection of data
structures found in Clojure, which are persistent, ours will be mutable.

To start with, we will explore a linked list implementation using
`deftype` (more on this later) to define a JVM class from Clojure-land.
This implementation will be expanded to include in-place reversal.
Finally we will utilize Clojure's built-in interfaces to give our linked
list access to some of the methods Clojure provides.

### Basic Requirements

While this is a tutorial, some working-knowledge of Clojure is assumed.
We will not cover setting up an environment for development or basic
introductory Clojure subjects. If you are looking for an introduction to
Clojure itself, consider [Clojure From the Ground
Up](http://aphyr.com/posts/301-clojure-from-the-ground-up-welcome "Clojure From the Ground Up").
However, no knowledge of Java or previous experience implementing these
data structures is otherwise required.

![Droste Effect](/images/droste.jpg)

Some background with
[recursion](https://encrypted.google.com/search?q=recursion "Did you mean: recursion")
will be beneficial. Most of the methods we implement will rely on
recursion because it is a concise and idiomatic choice for many of these
algorithms. That said most of the examples should be fairly easy to
follow even without a lot of experience with recursion.

Linked Lists
------------

Linked lists are a simple and common data structure. While being simple,
they can be surprisingly subtle. This makes them a good choice for an
introductory series on data structures and in fact our initial
implementation will be used again later when we explore hash tables (one
potential use for linked lists). They are also important with regard to
Lisp because S-expressions can be derived from linked lists. This is a
topic we will explore more closely in a future series.

### Basic Structure

[Singly-linked
lists](http://en.wikipedia.org/wiki/Linked_lists#Singly_linked_list "Linked Lists: Singly-Linked Lists")
are composed of links or nodes, where each node contains some data or
cargo and a next pointer. The next pointer points to the next node in
the list or some empty value, e.g. `nil`, indicating the end of the
list. A list of three nodes might look something like this:

![Singly-Linked List Structure](/images/linked-list.png)

In the above example the values "foo", "bar", and "baz" are all cargo.
While the arrows are pointers to the next node in the list. The last
node does not point to anything and instead holds `nil` as a terminating
value. This will be the basic structure of our singly-linked list
implementation.

### Performance Characteristics

Linked lists provide extremely efficient insertion and deletion at the
head of the list. These operations are
[O(1)](http://en.wikipedia.org/wiki/Time_complexity "Time Complexity").
Additionally, inserts and deletes into the middle of the list can be
performed in constant-time provided a reference is kept to the node
proceeding the node to be operated on, otherwise this is an O(n)
operation. Inserts and deletes at the end of the list can also be
performed in constant-time if the last element is known by some
reference.

Our implementation will not provide any but the first performance
guarantee, in the interest of simplicity. However, this implementation
could fairly trivially be extended to a doubly-linked list to provide
the aforementioned benefits.

### Applications

While linked list are an excellent choice for situations where an
algorithm interacts frequently with the head node or dynamic list size
is desired, arrays and dynamic arrays provide a different set of
advantages and ultimately a linked list is a [trade
off](http://en.wikipedia.org/wiki/Linked_lists#Tradeoffs "Linked Lists: Tradeoffs").

For instance, linked lists can be used to implement a Last-In First-Out
stack in a fairly simple manner. Given the fact that appends to the head
of the list are cheap, the `push` operation is inexpensive. Similarly,
the `pop` operation is cheap--both of these methods are constant time.

That said, where random access is needed, a linked list may not be the
best candidate. This is because we always have to traverse the list from
the head unlike an array, where knowing the index ahead of time gives us
efficient, near-constant lookups.

### Variations

Linked lists come in various flavors. Generally when we say linked list,
we mean a singly-linked list, which is what this article will focus on.
It is worth mentioning there exists [other
variations](http://en.wikipedia.org/wiki/Linked_lists#Basic_concepts_and_nomenclature "Linked List: Basic concepts and nomenclature")
which are beyond the scope of this tutorial.

Types in Clojure
----------------

### `defrecord` and `deftype`

Before we get started, we should briefly discuss methods for defining
custom types in Clojure, i.e. JVM classes. There are two:

    (defrecord Foo [])

and

    (deftype Foo [])

For this tutorial we will be using `deftype`. Records, via `defrecord`,
should be preferred for application-level data modeling. Records provide
map-like objects with faster access to their fields and which can
implement custom interfaces and protocols. Whereas `deftype` is a
superset of `defrecord` and does not provide accessors, equality, and
default interfaces. Because we are interested in defining our own data
structures, we will prefer `deftype` here.

Basic Implementation
--------------------

### Node Class

Now we will get started with our singly-linked list implementation.
Recall that our list will be made up of links or nodes. These nodes will
have two fields, a data field, which we will call `car` (you can think
of this as short for "cargo"), and a next pointer field, which we will
call `cdr`. (If you are curious about the nomenclature, [this bit of
Lisp history](https://en.wikipedia.org/wiki/CAR_and_CDR "CAR and CDR")
is relevant here.)

It seems like a class called `Node` might be a good way to encapsulate
this structure. Let us go ahead and outline a `Node` type with
`deftype`:

    (deftype Node [car cdr])

Notice that our binding vector contains two symbols, `car` and `cdr`.
Under the covers, these will become fields on the JVM class, of the same
name. If we were now to use our type from a REPL we would be able to see
how these fields work in practice:

    => (def node (Node. "foo" nil))
    => (.car node)
    "foo"
    => (.cdr node)
    nil

To construct a new `Node` instance, we use the the name of the class
suffixed with a period. This is a shortcut for telling Clojure to give
us a new `Node` object. We could also use `(new Node "foo" nil)`, to the
same effect.

With our newly constructed object, we can access its properties
directly. As we can see, `(.car node)` returns the expected value of
`"foo"`. Likewise `(.cdr node)` returns `nil`.

We now have a singly-linked list of one element in length. Our last
element terminates with a pointer to `nil`, indicating the end of the
list. This is looking good so far. But we probably will not be satisfied
with lists of only one node in practice, so we should think about how to
encapsulate lists of many nodes.

One way we can approach this, without writing any additional code, is to
simply nest invocations of the `Node` object. What if we do something
like this:

    => (def linked-list (Node. "foo" (Node. "bar" (Node. "baz" nil))))
    => (.car (.cdr linked-list))
    "bar"

Cool! That worked. By nesting constructions of the nodes, we built a
linked list with multiple elements. This is pretty neat, but there's
actually a problem with our implementation as it sits: how do we update
nodes?

When we use `deftype` to set up up our classes, they get compiled down
to JVM classes. During this process the fields are set up as
[`final`](http://bmanolov.free.fr/javamodifiers.php "Modifiers in Java").
This effectively makes them immutable. In Clojure-land, this is a
perfectly sane default. But not all data structures lend themselves to
these kinds of restrictions. Not only that, but one motivating factor
for defining custom types is performance. Sometimes, for performance
reasons, we want mutability. Some algorithms are just faster or simpler
when we can mutate data in place. Because of this, we have two options
for exposing our fields as mutable.

The [metadata](http://clojure.org/metadata "Clojure Metadata")
`^:volatile-mutable` and `^:unsynchronized-mutable`, will yield mutable
fields. While the particulars of these two options will not be covered
here entirely, essentially `^:volatile-mutable` will compile the field
with the `volatile` modifier. This means that set operations over these
fields are guaranteed to be visible to other threads immediately upon
completion. Whereas `^:unsynchronized-mutable` specifies a field which
does not make this guarantee.

We will use the volatile option for the sake of simplicity.

    (deftype Node [^:volatile-mutable car ^:volatile-mutable cdr])

Okay great! We can now modify the values of the car and cdr fields on
our objects. But there's actually another lurking problem here.

    => (def node (Node. "foo" nil))
    (.car node)
    IllegalArgumentException No matching field found: car for class Node  
    clojure.lang.Reflector.getInstanceField (Reflector.java:271)

Uh-oh. What happened? Well, when we made our field volatile, we also
made it private. It is no longer accessible from the outside world.
These fields will have to be accessible from outside the class if our
list is going to be useful. What we can do here is expose their values
via getter methods. At the same time we will also define methods for
setting these values from outside the class. Let us go ahead and write
those now.

    (definterface INode
      (getCar [])
      (getCdr [])
      (setCar [x])
      (setCdr [x]))

    (deftype Node [^:volatile-mutable car ^:volatile-mutable cdr]
      INode
      (getCar [this] car)
      (getCdr [this] cdr)
      (setCar [this x] (set! car x) this)
      (setCdr [this x] (set! cdr x) this))

Let us take a minute to talk about what we have done here. First we
added an interface called `INode`. This defines an interface for our
class. An interface is a collection of functions that can be applied to
a type which implements that interface. Again we won't cover this in
much depth here, however from Clojure `definterface` provides this
facility for us.

With our `INode` interface, we define our methods on the `Node` type.
Note that the `this` reference is implicit in the context of
`definterface`, so it is not a listed parameter of our interface
methods. But because the reference is needed from within our class, we
explicitly define it.

Finally we setup our getters and setters. These are pretty
straightforward. `set!` implicitly understand its context and therefore
only needs a field symbol and a value to bind it to. Here is what we
have now:

    => (def node (Node. "foo" nil))
    => (.getCar node)
    "foo"
    => (.getCdr (.setCdr node "bar"))
    "bar"

At this point our `Node` is starting to shape up. Not only can we define
a list of nodes but we can alter them later on and get meaningful data
back from them. This is really all we need to build useful linked lists.

Now let us extend our interface a bit. Say we want to reverse the
ordering of our linked list so that `(.getCar linked-list)` actually
returned "baz"?

Reversing a Singly-Linked List
------------------------------

It turns out there is an efficient, in time and space, way to do just
that. Returning to the original description of a linked list, we recall
that nodes have two values: a cargo and a next pointer. So to reverse
the list, perhaps all we need to do is start with the head node and swap
it with its neighbor? We could then continue on to the next node in the
list until we ran out of nodes.

If we consider a simple list of two elements:

![Simple Linked-List](/images/simple-linked-list.png)

We can say that we would like a's cdr value to hold a pointer to b and
for b's cdr value to be `nil`. In essence, we want to switch the two
nodes. This seems pretty straightforward. Here this will work. But what
if we have a slightly longer list? Will the same approach work for a
list of three elements?

This introduces a slight complication. If we were to have a list of
three elements in length, what happens after we switch the head node
with its neighbor? All would be fine until we went to select the next
node, which would actually be the first node. We need to ensure that we
do not step on ourselves as we traverse the list.

One approach here is to introduce an accumulator which ultimately will
become the new head of the list. With this accumulator we can cons the
current list's head on it. And then the cdr of the current head,
followed by the cdr of the cdr of the head, and so on, until we reach a
null cdr. At this point we would have a completely reversed set of node
elements collected in our accumulator. All we would then do is return
the accumulator.

Now we will implement this scheme in our reverse method:

    (definterface INode
      (getCar [])
      (setCar [x])
      (getCdr [])
      (setCdr [n])
      (reverse []))

    (deftype Node
      [^:volatile-mutable car ^:volatile-mutable ^INode cdr]
      INode
      (getCar [_] car)
      (setCar [_ x] (set! car x))
      (getCdr [_] cdr)
      (setCdr [_ n] (set! cdr n))
      (reverse [this]
        (loop [cur this new-head nil]
          (if-not cur
            (or new-head this)
            (recur (.getCdr cur) (Node. (.getCar cur) new-head))))))

Let us pick apart of the reverse method by stepping through it. Our main
goal here was to start with the head node and treat it as though it were
the tail, persisting whatever value its cdr held, assuming it was
non-nil, and then cons each successive node's car onto this new head. In
this way we reverse the list. To do this we use Clojure's loop construct
to recursively move through our nodes, passing over each one by one.

We start by defining `cur` and `new-head` in our loop. `new-head` is
what we will eventually return as the head node, it's our accumulator.
On the first iteration of the loop, it will point to `nil`. This is
because our linked list is null-terminated and this will serve as a
sentinel for the tail node. We then bind `cur` to the value of `head`.

![First Iteration](/images/reverse-first-iteration.png)

We now check for `cur` to be `nil`. This is our base case, the condition
under which the recursive loop will terminate. If we have any nodes this
check will fail and we will drop down into our second branch of the
conditional.

Here we do two important things. First we make good on our strategy of
consing the head node onto an accumulator, `new-head`. At this moment in
time, `new-head` points to `nil`. Take careful note here, this is
important because our list terminates with that value and therefore this
defines the final node in our soon-to-be-reversed list. `cur`'s cdr
value now points to `nil`, whereas before it pointed to the node that
held "a". The second thing we do is set new values for the second
iteration of the loop: `new-head` becomes the value of
`(Node. (.getCar cur) new-head))` (i.e. a new node containing "b"),
`cur` becomes the value of `(.getCdr cur)` (i.e. the node containing
"a"). We recur with these values.

![Second Iteration](/images/reverse-second-iteration.png)

So where does this leave us? We are entering the second iteration of our
loop. Our loop values have changed and we once again check if `cur` is
`nil`. In fact it is not (it is the node containing "a"). That puts us
in the second branch. Once again we set the cdr of `cur` to `new-head`.
Think about what `new-head` points to: the value of setting the cdr of
the the head node to the previous value of `new-head`, i.e. `nil`. In
other words, it points to a sublist of one element. That element was the
first node of our original list. We are going to set that value as the
cdr of the `cur` node, which is the node that holds "a". Again we recur.

![Third Iteration](/images/reverse-third-iteration.png)

This is the final iteration of our loop. This time `cur`'s value is in
fact `nil`. Our linked list is reversed. Time to give it a try:

    => (def linked-list (Node. "b" (Node. "a" nil)))
    => (.. linked-list getCar)
    "b"
    => (.. linked-list reverse getCar)
    "a"

Great. It works! There is one caveat to this approach: we originally
said that there was a solution that was efficient in both time and
space. While that is true, our implementation is not efficient in terms
of space. Because we copy the list instead of reassigning the node's
pointers, we end up consuming more memory than necessary. To remedy this
we could instead change the node's pointers in place.

    (deftype Node
      [^:volatile-mutable car ^:volatile-mutable ^INode cdr]
      INode
      (getCar [_] car)
      (setCar [_ x] (set! car x))
      (getCdr [_] cdr)
      (setCdr [this n] (set! cdr n) this)
      (reverse [this]
        (loop [cur this new-list nil]
          (if-not cur
            (or new-list this)
            (let [cur->cdr (.getCdr cur)]
              (recur cur->cdr (.setCdr cur new-list)))))))

However this is slightly dangerous if our linked list is bound to a var
such as `linked-list`. Since the original pointer bound to the var will
become the tail of the list and references to the rest of the list will
not be accessible. For simplicity we will not take that approach here.

Clojure Interfaces
------------------

Up until now we have used the methods we defined ourselves exclusively
to manipulate our custom type. While there is nothing inherently wrong
with this approach, it does mean we have been eschewing the use of
Clojure's API. One thing we might want to be able to do is call methods
like `first` and `next` over our linked list.

If we try this now we will get an error that informs us our object does
not participate in the [`ISeq`
interface](http://clojure.org/sequences "Sequences"). Indeed this is
true and it is also something we can fix. Let us implement a
generalization of the seq interface via `clojure.lang.Seqable`.

    (definterface INode
      (getCar [])
      (setCar [x])
      (getCdr [])
      (setCdr [n])
      (reverse []))

    (deftype Node
      [^:volatile-mutable car ^:volatile-mutable ^INode cdr]
      INode
      (getCar [_] car)
      (setCar [_ x] (set! car x))
      (getCdr [_] cdr)
      (setCdr [_ n] (set! cdr n))
      (reverse [this]
        (loop [cur this new-head nil]
          (if-not cur
            (or new-head this)
            (recur (.getCdr cur) (Node. (.getCar cur) new-head)))))

      clojure.lang.Seqable
      (seq [this]
        (loop [cur this acc ()]
          (if-not cur
            acc
            (recur (.getCdr cur) (conj acc (list (.getCar cur))))))))

Here we have implemented `clojure.lang.Seqable`. Our goal in defining
the `seq` method is to return a sequence that represents our object. So
in the case of our linked list that would be a sequence of all of our
nodes. Each node is referenced by the current node forming a list. We
use `concat`, in order to preserve the correct ordering, build up an
accumulator list. Ultimately we return this accumulator. This is what we
get when we call `seq` over a linked list instance now. Let us give it a
shot.

    => (def linked-list (Node. "foo" (Node. "bar" (Node. "baz" nil))))
    => (seq linked-list)
    ("foo" "bar" "baz")
    => (first linked-list)
    "foo"

By implementing the seqable interface we have access to some of
Clojure's built-in methods. Notably our object can now be cast to a
sequence which means `first`, `next`, and `rest` will work. While this
is certainly handy, there is a caveat.

Sometimes `clojure.lang.Seqable` will not be enough to satisfy the
`ISeq` interface. In this case, we will have to implement
`clojure.lang.ISeq` explicitly. Furthermore, Clojure objects implement
more than just the sequence interface. So you may find some built-in
methods will complain and throw errors.

### Completeness

In order for our object to be a "complete" Clojure datatype we would
have to implement a number of interfaces. This is largely outside the
scope of this tutorial and we will not attempt that here. But one
additional interface we might consider is
`clojure.lang.ITransientCollection` and its associated methods.

Also note that Clojure interfaces need not be implemented
comprehensively. That is to say that you can implement any number of
methods they provide and not necessarily all of them. That said,
generally it is a good idea to implement all of them or we may find we
end up with strange errors such as `AbstractMethodError`, indicating a
method that the interface provides was not implemented.

Conclusion
----------

This is the end of our exploration of a linked list implementation.
While our implementation is simple, it is correct and usable. By
utilizing Clojure's `deftype` we have created a proper JVM class. This
class could even be used from other JVM languages, such as Java.

Additionally we explored the implementation of a fundamental linked list
operation: reversal. Our solution is a recursive solution that builds up
a new list but successively taking nodes from the head of a given list
and appending those to the accumulator list.

Finally we took a look at how we can take advantage of some of Clojure's
built-in methods by implementing the `clojure.lang.Seqable` interface.
While this does not qualify as a comprehensive datatype from the
perspective of Clojure, it is nonetheless useful.

Our implementation could be used to implement a variety of more complex
data structures, some of which we will explore in the future. However
one simple exercise is to leverage what we have here to build a simple
stack. Try to implement the `push` and `pop` methods, where `push` takes
a value and stores it in a node appended to an underlying linked list
and `pop` takes the first node of that same list, returning its value.

    => (def s (stack))
    => (.push s "foo")
    => (.push s "bar")
    => (.pop s)
    "bar"
    => (.pop s)
    "foo"
    => (.pop s)
    nil

### Future Posts

Now that we have a linked list, we will use this linked list to build a
hash table. The next post will introduce hash tables and cover an
implementation that uses separate chaining and dynamic resizing. Stay
tuned!
