  -------------------------------- --------------------------------------------------------------------------------------
  [![[\*]](/dodeca3_100.gif)](/)   [Ned Batchelder](/) : [Blog](/blog) | [Code](/code) | [Text](/text) | [Site](/site)\
                                   Deleting code\
                                    » [Home](/) : [Text](/text)
  -------------------------------- --------------------------------------------------------------------------------------

Created 22 December 2002, last updated 6 November 2012

This document is also available in
[Russian](http://nedbatchelder.com/text/deleting-code_ru.html).

There's plenty of information out there about how to write code. Here's
some advice on how to delete code.

The best way to delete code
===========================

This may seem obvious, but I guess it isn't, because of the variety of
other ways developers have of deleting code. Here's how to delete code:

> Select a section of code in your editor, hit the backspace key, and be
> done with it.

Most developers don't like getting rid of stuff. They want to keep
chunks of code around in case they need them again. They worked hard to
write that chunk of code. They debugged it, it works. They don't want to
just throw it away.

These developers want to keep their old code around, and they do it by
disabling it in some way: commenting it out, conditionalizing it, or
just not calling it anymore.

To those developers, I say, "Use the source (control), Luke". A source
code control system (like [Git](http://git-scm.com/),
[Mercurial](http://mercurial.selenic.com/), or
[Subversion](http://subversion.apache.org/)), means you never have to
worry that something is gone forever. Your repository will have the old
code if you need it again.

If you don't have a source control system (!?!?!) or just don't want to
be bothered digging back through the history, then copy the chunk of
code to a separate file some place, and save it away. But don't leave it
where it doesn't belong: in your source code.

What's the big deal?
====================

If you have a chunk of code you don't need any more, there's one big
reason to delete it for real rather than leaving it in a disabled state:
to reduce noise and uncertainty. Some of the worst enemies a developer
has are noise or uncertainty in his code, because they prevent him from
working with it effectively in the future.

A chunk of code in a disabled state just causes uncertainty. It puts
questions in other developers' minds:

-   Why did the code used to be this way?
-   Why is this new way better?
-   Are we going to switch back to the old way?
-   How will we decide?

If the answer to one of these questions is important for people to know,
then write a comment spelling it out. Don't leave your co-workers
guessing.

Commenting out code
===================

It's very easy to comment out a line or two (or twenty!) lines of code:

> `//  OldWayStepOne(fooey);//  OldWayStepTwo(gooey);    NewWay(fooey, gooey);`

This is bad. Comments should be used to provide people with information
they need when reading or writing the code. They should be used to help
the future developers who will be working with the code. These comments
don't do that. In fact, they do just the opposite. In addition to
removing the old code from being compiled, these comments add confusion,
uncertainty, and doubt into the code.

Future developers looking at this code know that it used to work the
OldWay, and they know that now it works the NewWay, but they don't know
why the OldWay has been kept around:

-   Maybe NewWay is just an experiment? If so, what's better about it?
    How and when will the final decision be made to keep it?
-   Maybe OldWay is better, but there was something wrong with it? If
    so, what was wrong with it? Was it something wrong with the OldWay
    code, or they way we're calling it? When will it be fixed?
-   Maybe the design has changed, and OldWay is doing unnecessary work?

Any commented-out code is an implicit question: Why is this still here?
There are reasons to keep a piece of commented-out code. Changes get
made that you know will be reversed soon. Changes get made that the
developer is uncertain of. It's OK to keep the code, but say why you're
keeping it. Comments are for people, and a line of code in a comment
doesn't tell anyone anything.

> Don't comment out a piece of code without saying why (in the comment).

Isn't this better?:

> `//  OldWay did a better job, but is too inefficient until MumbleFrabbitz//  is overhauled, so we'll use NewWay until the M4 milestone.//    OldWayStepOne(fooey);//    OldWayStepTwo(gooey);    NewWay(fooey, gooey);`

Now, who knows if MumbleFrabbitz will really be overhauled for the M4
milestone? Maybe it won't be. That's OK; who knows what the future will
bring? But at least this way the developers will know why the code is
being kept around. With the information about why the change was made,
and why the old code is still there, the developers will know when they
can finally fully commit to the NewWay, or when they can switch back to
the better solution.

Conditional compilation
=======================

Developers who want to comment out large chunks will use conditional
compilation instead (if the language supports it). In C++:

> `#if 0    OldWayStepOne(fooey);    ...    OldWayStepTwenty(hooey);#endif`

In Python:

> `if 0:    OldWayStepOne(fooey)    ...    OldWayStepTwenty(hooey)`

This is no better than commenting out the code: it's just more
convenient for the guy doing the removing. In fact, in some ways it is
worse than commenting out the code. Some IDEs don't syntax-color this
code as a comment, so it's easy for other developers to read this code
and not realize it has been disabled.

The same rule applies as for commenting out code:

> Don't conditionalize away code without explaining why.

If you must use the C preprocessor to remove code, "\#if 0" is really
the best way to do it, since it is at least clear that the code should
never be compiled.

At Lotus, the source code for Notes include many lines of code removed
with "\#ifdef LATER", under the (correct) assumption that there was no
preprocessor symbol called "LATER". This is a very weak form of
documentation; it indicates that the code isn't ready to be compiled
yet, but that it will be later. But when? A running joke among the
developers was that we should define "LATER" and see what happened!

By using never-defined symbols to remove code, you leave doubt in
developers minds as to what the symbols mean. Maybe there's a
configuration of the code called "LATER" that has to be taken into
account.

Uncalled code
=============

Let's say you have a great class, and it has many methods. One day you
discover that you no longer are calling a particular method. Do you
leave it in or take it out?

There's no single answer to the question, because it depends on the
class and the method. The answer depends on whether you think the method
might be called again in the future. A coarse answer could be: if the
class is part of the framework, then leave it, if it is part of the
application, then remove it. (I'll have to write another piece about
framework vs. application).

Leaving pointers
================

One compromise that you might consider is to remove a large chunk of
unused code, but leave behind a pointer to where it could be found if it
were needed. I've used comments like this before:

> `//  (There used to be another algorithm here that used hashing, that//  was faster, but had race conditions.  If you want it, it's in//  commit 771de15b or earlier of ThingMap.java in the repo.)`

It's small, it's unobtrusive, but it gives a little history, and a place
to go looking for more information.

Accidental droppings
====================

Sometimes, while writing code, you really are unsure about whether to
keep or delete a line of code, and you want to try compiling or running
the code before you decide what to do. You comment out the line. A
number of files get changed, and by the time you are ready to check in
the code, you've forgotten where all those temporary removals are. You
check in the code, and you've left accidental droppings all over the
place.

> Always use a distinctive marker in your commented-out lines of code,
> so you can quickly find them all when it's time to clean up and check
> in.

A simple convention like this:

> `//- OldWayImUnsureOf(zooey);`

makes all the difference. By using "//-" to comment out the line, you've
left a marker that you can easily find when you are getting ready to
check in your code.

You can use it for larger chunks as well:

> `#if 0 //- I don't think I need this with the new FooBar    OldWayStepOne(fooey);    ...    OldWayStepTwenty(hooey);#endif`

Keep things tidy
================

While deleting code, it is all too easy to leave phantom stubs behind.
Try hard to trim these properly. For example, when getting rid of OldWay
here:

> `if (bDoThing) {    OldWay();}`

Don't just take out the line calling OldWay. Get rid of the empty if as
well. Then if bDoThing was only tested here, also get rid of it. Examine
the code that set bDoThing. Is it now obsolete? Get rid of it. Be
merciless. Keep the code tidy. Make sure it makes sense with no dead-end
off ramps that can only be understood by knowing what used to be there.

It is tempting to leave this code in, because it will be difficult to
understand whether it is all still needed or not. But if you leave the
empty if clause, some other developer will come along later, and see it,
realize it can't be right, and have to investigate. It will take them
longer to understand the empty if than it would take you to remove it.

Don't worry, be happy
=====================

I know it seems drastic to just chop out code that you sweated over.
Don't worry: it will be OK. There's a reason you wanted to disable it or
whatever. The source control system will still have a copy if you need
to go back to it. Look at it this way: what are the chances you need to
go back and get it, compared to the certainty that you'll have to be
looking at those stupid commented-out lines for the rest of the
project's life?

Go ahead, delete that old code. You won't miss it.

See also
========

-   [Erroneously empty code
    paths](http://nedbatchelder.com/text/empty-code-paths.html), about
    mistakes in defensive coding.
-   [Fix error handling
    first](http://nedbatchelder.com/text/fix-err-hand.html), about
    ensuring that your error handling code is always running its best.
-   [My blog](http://nedbatchelder.com/blog), where other similar topics
    are discussed.

Comments
========

![[gravatar]](http://www.gravatar.com/avatar/cf0dee795d5dc163eb9eb33b687dd104.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa184.jpg&size=40)

**[Paul A. Francis](wso.williams.edu)** 5:16 PM on 26 Jan 2004

I am happy in the best way forever. \
Be happy in the best way forever.

![[gravatar]](http://www.gravatar.com/avatar/d41d8cd98f00b204e9800998ecf8427e.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa51.jpg&size=40)

**[Doug](http://www.dsauder.com/weblog/)** 1:33 PM on 21 Aug 2004

You should mention that old code often becomes out-of-date, and
therefore even more worthless. A great example of this is a main method
in Java that a developer adds to a class with test code for the class.
The main method method does not get executed because it is not the
application entry point. Developers want to leave the main method in the
class so that they may test the class in the future. However, as the
class is modified, the test code in main rarely gets modified. A class
that is a couple years old may have a main method with test code that is
completely useless.

![[gravatar]](http://www.gravatar.com/avatar/fff5fc93512414dd788098f65e31e418.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa186.jpg&size=40)

**[craig](http://infinityzero.com)** 12:36 PM on 26 Aug 2004

Amen, brother! \
 \
You tell them.

![[gravatar]](http://www.gravatar.com/avatar/84172872e4f61a5db11bcd610eca5c08.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa183.jpg&size=40)

**[Max Ischenko](http://ischenko.blogspot.com)** 5:05 AM on 23 Dec 2004

You wrote: "Always use a distinctive marker in your commented-out lines
of code, so you can quickly find them all when it's time to clean up and
check in." \
 \
IMO, using diff command before check-in is a better approach. It'd
detect not only commented out lines, but accidentally deleted ones as
well.

![[gravatar]](http://www.gravatar.com/avatar/154333574f5f12462078dc43dff1e4cb.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa205.jpg&size=40)

**Allie** 1:56 PM on 8 Feb 2005

Hmmmm. I need some help cleaning out my closets....

![[gravatar]](http://www.gravatar.com/avatar/c344f627c1071e7ad580fa715410941c.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa151.jpg&size=40)

**fel** 4:26 PM on 28 Mar 2005

i agree with the author, when the code \
is commented out, and then the real code changes, the old code is not
only obsolete, but it may cause hard -to-detect errors, \
 \
as for the special marks : \
i use /// in javascript or \#\# in tcl \
 \
and of, course, vim: /\\/\\/\\/.\*/

![[gravatar]](http://www.gravatar.com/avatar/13c69ceaef99bd296bcdeae695da27a6.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa279.jpg&size=40)

**steve hartley** 5:30 AM on 4 Nov 2005

I agree with your comments on commented code - ie get rid of it! \
 \
Can anyone recommend a tool which will detect commented Java code? I
have a large code base that would be too big to check by eye. \
 \
I've been experimenting with PMD
([http://pmd.sourceforge.net/](http://pmd.sourceforge.net/)) but there
doesn't seem to be a rule for commented code. I'm considering writing
one, but before I do, just checking no-one has done this already...

![[gravatar]](http://www.gravatar.com/avatar/d0c1fa6cac0115d5d817101ac0f75f7f.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa200.jpg&size=40)

**Abu** 8:33 PM on 10 Nov 2005

I think these notes are very useful for the fresh and experienced
progrmmers too.. \
This will help for fast coding..

![[gravatar]](http://www.gravatar.com/avatar/d493d68d589c92ba2c383be57c762ff7.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa80.jpg&size=40)

**Hafez the Enforcer** 5:00 PM on 26 Jan 2006

When I come across some code that I know has been if 0'd or commented
out for many moons, and I am \*totally\* comfortable with it, almost to
the point of being ridiculous, then and only then do I delete it. \
 \
Relying on version control doesn't make much sense to me--I've seen too
many VC systems screw up, leaving people high-n-dry. Redundancy,
redundancy, and more redundancy. If the swath of code is really large, I
move it to a folder called dino, short for dinosaur. \
 \
Never rely on VC or nightly tape backups; they are just conveniences.
tar or zip and a CD burner are your friend. So is RAID. Of course you
don't have to burn a CD every night, but what if the tapes turn out to
be duds? Then what are you gonna do? Did I mention redundancy. OK, this
strays off the topic a bit; the real point is that history matters. \
 \
Sometimes, knowing what you threw away is important. When it's time for
maintenance programmers to come in and do things, then it's probably
appropriate to "clean house", but otherwise I'll continue to do what
I've always been doing. It seems to be working quite well, and if it
ain't broke I'm not going to fix it. Did I mention redundancy?

![[gravatar]](http://www.gravatar.com/avatar/c510febb9bed68b5cc4a09f076701e0f.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa34.jpg&size=40)

**Shaun** 8:02 PM on 26 Jan 2006

Commenting code out within a visual studio.net environment is perfectly
fine. You simply wrap the code comment with a region tag and vs hides
the comment... \
label the tag with some detail and voila. \
 \
You \*could\* argue that this might not work with other environments
however in my experience most people standardise on vs.net in the .net
dev world...

![[gravatar]](http://www.gravatar.com/avatar/86790ddd55c56e34926c5ff1d9ce39d3.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa86.jpg&size=40)

**Russell Mull** 2:57 AM on 27 Jan 2006

While selecting a block of code and hitting backspace is a passable
solution, in my experience it's much more satisfying to do it line by
line, by holding the appropriate shortcut. (ctrl-L, ctrl-K, or just d,
depending on your editorial persuasion) I had the pleasure today of
ctrl-L-ing about 150 lines of broken, unnecessary code. Watching it
dissapear, one line at a time... there's nothing quite like it.

![[gravatar]](http://www.gravatar.com/avatar/1a77a584a4eba3f6519d91030b8fcd20.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa280.jpg&size=40)

**[Getahun Dana](http://How%20to%20delete)** 3:49 AM on 29 Jan 2006

I have no code.But I have many things in my computer recorded which I
donot want need any more.For example,if I write about "states", this
will be recorded.So,at other times the moment I put the word s,"states"
will written.So now I want to delete many things recorded in such a way
which I do not want to see any more.I am looking forward to seeing your
help

![[gravatar]](http://www.gravatar.com/avatar/130cb83b50e2246a40cd4db93921e5ae.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa277.jpg&size=40)

**Jason** 6:46 PM on 1 May 2006

Like developing it, deleting code is an iterative process. \
 \
First you do \
 \
// \#define USE\_OLD\_WAY 1 \
 \
\#if USE\_OLD\_WAY \
 // BUGBUG - remove this after testing \
 // old code \
\#else \
 // new code \
\#endif \
 \
and then after it's made it through a few test passes, it's safe to
yank.

![[gravatar]](http://www.gravatar.com/avatar/526188d18df02a6ad7c28ca29c79438f.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa228.jpg&size=40)

**Ricky Darnell** 8:50 PM on 31 Dec 2006

How do I get rid of this code: Code \
0x80240016.

![[gravatar]](http://www.gravatar.com/avatar/81737f6f0710b4cdd03004239cb98fa9.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa14.jpg&size=40)

**[RT Cunningham](http://untwistedvortex.com)** 6:40 AM on 4 Feb 2007

I always develop with two sources, one commented and one not commented.
I usually have them both open in the editor at the same time. Easier
than using CVS for me.

![[gravatar]](http://www.gravatar.com/avatar/c72d40a0909547f2a646bc263a42c910.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa259.jpg&size=40)

**[Me too](http://www.lasvillas.eu)** 7:48 AM on 16 Feb 2007

I used to do the old way: \
in some .h file: \
 \
\#define ENABLE\_STUFF \
 \
in a .c/.cpp file \
. \
. \
\#ifdef ENABLE\_STUFF \
//use stuff \
\#endif \
 \
But just before a checkin, or shortly after, I delete the unreachable
code. \
Source Safe keeps always a copy that I could return to.

![[gravatar]](http://www.gravatar.com/avatar/c72d40a0909547f2a646bc263a42c910.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa259.jpg&size=40)

**[But, be careful](http://www.lasvillas.eu)** 7:54 AM on 16 Feb 2007

Important to not be confused, in the case of a debug/release build, it
is useful to use conditional compiling. \
 \
The following example is NOT a way to disable code, rather a way to get
some output when debugging an application: \
 \
\#if(DEBUG) \
MakeSomeDebugPrint(); \
\#endif

![[gravatar]](http://www.gravatar.com/avatar/49bbd201a15eba32c5894caafd894942.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa67.jpg&size=40)

**Matt** 10:36 PM on 28 Oct 2008

@steve hartley: \
 \
grep -F '//' \*.cpp

![[gravatar]](http://www.gravatar.com/avatar/aebc2d07a50011e2a30d38435fde564a.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa14.jpg&size=40)

**[JROM](http://jrom.net)** 4:33 AM on 10 Nov 2008

@Matt, i don't think it's going to print the comments from some **java**
source... And you still need the /\* other comment syntax \*/

![[gravatar]](http://www.gravatar.com/avatar/c61e474085276079fee33935f0043ccf.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa130.jpg&size=40)

**Jass** 8:20 AM on 10 Nov 2008

//Any commented-out code is an implicit question: Why is this still
here? \
 \
Erm... Sentimental value is not a good enough reason? \*rolling eyes\* \
 \
LOL! Nice write up! :)

![[gravatar]](http://www.gravatar.com/avatar/7947cc07f5267f582221cdf93f3c93f9.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa202.jpg&size=40)

**Robert** 10:40 AM on 10 Nov 2008

I'm with Max Ischenko on this one. At my company we have a code review
policy, so I use a diff tool after checkin but before submitting
something for review. In effect, I do a code review of my own code.

![[gravatar]](http://www.gravatar.com/avatar/4bb59d212ca7657d85304e5ed41b8c09.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa162.jpg&size=40)

**[Paul A Houle](http://gen5.info/q/)** 1:55 PM on 11 Nov 2008

Hell yeah! \
 \
I was picking apart a complex C\# class today and noticed a partial
class that never got used. Right to the bit bucket... \
 \
I'm not sure if you mention it, but using version control is good for
getting the psychological freedom to delete code. It always helps to be
able to tell your co-workers (or yourself) that you can get it back in a
few minutes when you need it.

![[gravatar]](http://www.gravatar.com/avatar/26375f7a3094df9e3f435b639fe2bce3.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa93.jpg&size=40)

**[David Hofmann](http://hofmanndavid.blogspot.com)** 8:00 PM on 11 Nov
2008

I do agree 100 %

![[gravatar]](http://www.gravatar.com/avatar/64029c12d260bd5e3cb2504f4155998f.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa233.jpg&size=40)

**bob** 4:26 PM on 12 Nov 2008

Good stuff man, I need to move more of my stuff to SVN, I'm still
learning it, but it makes sense to delete it

![[gravatar]](http://www.gravatar.com/avatar/484bacaf770b16ef8a06123e4c137e5d.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa26.jpg&size=40)

**pongi** 7:19 AM on 13 Nov 2008

The nicest thing about comments is that when you read again your
previously (sometimes long before) written comments you feel like you're
speaking to the yourself of the past, sort of another you. And the best
part is trying to please the yourself of the future when you write
comments, so to laugh when the future one read it. Sounds crazy, but I
swear it's truly funny.

![[gravatar]](http://www.gravatar.com/avatar/7ff4ae8730917924998c2660c9ed6f5d.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa141.jpg&size=40)

**Robin** 7:52 AM on 13 Nov 2008

Sometimes I found commenting code is a better solution. In case the
project was fall into the hand of a new developer, and he does not know
there is such piece of previously deleted code in CVS, he properly wont'
look it up in CVS history. It can happen in a poorly organized team,
with zero documentation and none test case.

![[gravatar]](http://www.gravatar.com/avatar/ad62b67a4386fa0ad88faf3bc6364c32.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa124.jpg&size=40)

**[Tom Ryder](http://.sanctum.geek.nz)** 9:09 PM on 5 Jan 2009

Thank heaven someone agrees with me on this. I've thought that it was
obvious having a version control system obviates the need for commenting
out blocks of code for years. Use the comments for just that -- end this
painful business of disabling code and cluttering up my beautifully
crafted classes!

![[gravatar]](http://www.gravatar.com/avatar/5d10e2a11f1ac819462aab138cfa9936.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa192.jpg&size=40)

**Dorothy** 8:27 PM on 12 Mar 2009

I havecome up w/a code & don't knowhow I did or howto get rid of it. \
Even here what I am writing is in code but gets received in
English.Anytime I fill in a comment space or message on a card it goes
in code. How do I get ridof it?

![[gravatar]](http://www.gravatar.com/avatar/7d9bc9cc63d3a44875f4866865ee6e03.jpg?default=http%3A%2F%2Fnedbatchelder.com%2Fpix%2Favatar%2Fa7.jpg&size=40)

**Chris2048** 9:30 AM on 12 Jul 2013

The "It's still in revision!" Argument has two flaws: \
 \
Discoverability: \
How do I or anyone else know the code is there? Do I have to look at
every revision of every class I touch? Surely, the fact that a
particular revision may be of use to later development is something that
should be explicitly communicated? \
 \
No-Commit: \
What If I write some code, that may be useful, but isn't needed not
(like a WIP, or another way to do something). The effort in creating
that code may make it worthwhile to keep around, but I haven't actually
committed it - In this case I might have to create \*at least\* two
commits - one to put the code in revision, and a second to remove it...

Add a comment:
--------------

name

email

Ignore this:

not displayed and no spam.

Leave this empty:

www

not searched.

Name and either email or www are required.

Don't put anything here:

Leave this empty:

URLs auto-link and some tags are allowed: <a\><b\><i\><p\><br\><pre\>.

Email me future comments

-   Search this site:
      -- --
         
      -- --

-   [About me](/site/aboutned.html)
-   Also me:
    -   [twitter](http://twitter.com/nedbat) ·
    -   [email](mailto:ned@nedbatchelder.com)

-   Tip me:
    -   [gittip](http://gittip.com/nedbat) ·
    -   [bitcoin](https://coinbase.com/checkouts/c2a7f9652ecc0f6bf9c240c05109305f)

-   You might like:
    -   » [My blog](/blog)
    -   » [My wife's books](http://susansenator.com/makingpeace.html)\
        [![Making Peace With
        Autism](/pix/makingpeacetiny.png)](http://susansenator.com/makingpeace.html)
        [![Autism Mom's Survival
        Guide](/pix/survivalguidetiny.png)](http://susansenator.com/survivalguide.html)

[© Copyright 2002–2012, Ned Batchelder](/site/legal.html)
