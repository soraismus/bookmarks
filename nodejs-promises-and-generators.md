http://strongloop.com/strongblog/node-js-callback-hell-promises-generators/

-   [](https://www.facebook.com/strongloop)
-   [](https://twitter.com/StrongLoop)
-   [](https://plus.google.com/114210079151116472527)
-   [](https://github.com/strongloop)
-   [✉](http://www.cloudflare.com/email-protection#6102000d0d0300020a211215130e0f060d0e0e114f020e0c)

-   [Login](/login "Login")
    -   [Register](https://strongloop.com/register/ "Register")

-   [Blog](http://strongloop.com/strongblog/)

**[![image](http://strongloop.com/wp-content/uploads/2013/04/logo-strongloop.png)](http://strongloop.com/)**

-   [Products](http://strongloop.com/strongloop-suite/)
    -   [Installation Instructions](http://strongloop.com/get-started)
    -   [LoopBack](http://strongloop.com/mobile-application-development/loopback/)
    -   [StrongOps](http://strongloop.com/node-js-performance/strongops/)
    -   [StrongNode](http://strongloop.com/node-js-support/strongnode/)
    -   [Consulting](http://strongloop.com/node-js-consulting/strongexpert/)
    -   [Training](http://strongloop.com/node-js-consulting/strongexpert/)
    -   [Certification](http://strongloop.com/node-js-consulting/strongexpert/)
    -   [Pricing](http://strongloop.com/strongloop-suite/subscription-plans/)
    -   [FAQ](http://strongloop.com/strongloop-suite/faq/)

-   [Developers](http://strongloop.com/developers/)
    -   [Installation Instructions](http://strongloop.com/get-started/)
    -   [iOS SDK](http://strongloop.com/mobile/ios/)
    -   [Android SDK](http://strongloop.com/mobile/android/)
    -   [Forums](http://strongloop.com/developers/forums/)
    -   [Videos](http://strongloop.com/developers/videos/)
    -   [Events](http://strongloop.com/developers/events/)
    -   [Node.js
        Infographic](http://strongloop.com/developers/node-js-infographic/)
    -   [Newsletter Archives](http://strongloop.com/newsletter/)

-   [Documentation](http://docs.strongloop.com/display/DOC/Getting+started#install-tools)
-   [🔍](?s=)

[StrongBlog](http://strongloop.com/strongblog/ "Permanent Link: StrongBlog")
============================================================================

You are here: [Home](http://strongloop.com "StrongLoop") /
[StrongBlog](http://strongloop.com/strongblog/ "StrongBlog") /
[Community](http://strongloop.com/strongblog/category/community/ "View all posts in Community")
/ Managing Node.js Callback Hell with Promises, Generators and Other
App...

In the Loop
===========

[✎]()

[Managing Node.js Callback Hell with Promises, Generators and Other Approaches](http://strongloop.com/strongblog/node-js-callback-hell-promises-generators/ "Permanent Link: Managing Node.js Callback Hell with Promises, Generators and Other Approaches")
============================================================================================================================================================================================================================================================

03 Feb 2014/[0
Comments](http://strongloop.com/strongblog/node-js-callback-hell-promises-generators/#respond "Comment on Managing Node.js Callback Hell with Promises, Generators and Other Approaches")/in
[Community](http://strongloop.com/strongblog/category/community/),
[How-To](http://strongloop.com/strongblog/category/howto/),
[StrongOps](http://strongloop.com/strongblog/category/strongops/) /by
[Marc
Harter](http://strongloop.com/strongblog/author/marc/ "Posts by Marc Harter")

[](http://www.addtoany.com/share_save)

We know it most endearingly as “callback hell” or the “pyramid of doom”:

1

2

3

4

5

6

7

doAsync1(function () {

doAsync2(function () {

doAsync3(function () {

doAsync4(function () {

})

})

})

Callback hell is subjective, as heavily nested code can be perfectly
fine sometimes. Asynchronous code is *hellish* when it becomes overly
complex to manage the flow. A good question to see how much “hell” you
are in is: *how much refactoring pain would I endure
if****doAsync2****happened before****doAsync1****?*The goal isn’t about
removing levels of indentation but rather writing modular (and
testable!) code that is easy to reason about and resilient.

In this article, we will write a module using a number of tools and
libraries to show how control flow can work. We’ll even look at an up
and coming solution made possible by the *next* version of Node.\

**The problem**
---------------

Let’s say we want to write a module that finds the largest file within a
directory.

1

2

3

4

5

var findLargest = require('./findLargest')

findLargest('./path/to/dir', function (er, filename) {

if (er) return console.error(er)

console.log('largest file was:', filename)

})

Let’s break down the steps to accomplish this:

-   Read the files in the provided directory
-   Get the [stats](http://nodejs.org/api/fs.html#fs_class_fs_stats) on
    each file in the directory
-   Determine which is largest (pick one if multiple have the same size)
-   Callback with the name of the largest file

If an error occurs at any point, callback with that error instead. We
also should never call the callback more than once.

**A nested approach**
---------------------

The first approach is nested, not horribly, but the logic reads inward.

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

var fs = require('fs')

var path = require('path')

module.exports = function (dir, cb) {

fs.readdir(dir, function (er, files) { // [1]

if (er) return cb(er)

var counter = files.length

var errored = false

var stats = []

files.forEach(function (file, index) {

fs.stat(path.join(dir,file), function (er, stat) { // [2]

if (errored) return

if (er) {

errored = true

return cb(er)

}

stats[index] = stat // [3]

if (--counter == 0) { // [4]

var largest = stats

.filter(function (stat) { return stat.isFile() }) // [5]

.reduce(function (prev, next) { // [6]

if (prev.size \> next.size) return prev

return next

})

cb(null, files[stats.indexOf(largest)]) // [7]

}

})

})

})

}

-   Read all the files inside the directory
-   Gets the stats on each file. This is done in parallel so we are
    using a **counter** to track when all the I/O has finished. We are
    also using a **errored** boolean to prevent the provided callback
    (**cb)** from being called more than once if an error occurs.
-   Collect the stats for each file. Notice we are setting up a parallel
    array here (files to stats).
-   Check to see if all parallel operations have completed
-   Only grab regular files (not links or directories, etc)
-   Reduce the list to the largest file
-   Pull the filename associated with the stat and callback

This may be a perfectly fine approach to solving this problem. However,
its tricky to manage the parallel operation and ensure we only callback
once. We’ll look at managing that a little later, but lets first look at
breaking this into smaller modular chunks first.

**A modular approach**
----------------------

Our nested approach can be broken out into three modular units:

-   Grabbing the files from a directory
-   Grabbing the stats for those files
-   Processing the stats and files to determine the largest

Since the first task is essentially just **fs.readdir()**, we won’t
write a function for that. However, let’s write a function that, given a
set of paths, will return all the stats for those paths while
maintaining the ordering:

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

function getStats (paths, cb) {

var counter = paths.length

var errored = false

var stats = []

paths.forEach(function (path, index) {

fs.stat(path, function (er, stat) {

if (errored) return

if (er) {

errored = true

return cb(er)

}

stats[index] = stat

if (--counter == 0) cb(null, stats)

})

})

}

Now, we need a processing function that compares the stats and files and
returns the largest filename:

1

2

3

4

5

6

7

8

9

function getLargestFile (files, stats) {

var largest = stats

.filter(function (stat) { return stat.isFile() })

.reduce(function (prev, next) {

if (prev.size \> next.size) return prev

return next

})

return files[stats.indexOf(largest)]

}

Let’s tie the whole thing together:

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

var fs = require('fs')

var path = require('path')

module.exports = function (dir, cb) {

fs.readdir(dir, function (er, files) {

if (er) return cb(er)

var paths = files.map(function (file) { // [1]

return path.join(dir,file)

})

getStats(paths, function (er, stats) {

if (er) return cb(er)

var largestFile = getLargestFile(files, stats)

cb(null, largestFile)

})

})

}

-   Generate a list of paths from the files and directory

A modular approach makes reusing and testing methods easier. The main
export is easier to reason about as well. However, we are still manually
managing the parallel stat task. Let’s switch over to some control flow
modules and see what we can do.

**An async approach**
---------------------

The [async](https://github.com/caolan/async) module is widely popular
and stays close to the Node core way of doing things. Let’s take a look
at how we could write this using async:

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

var fs = require('fs')

var async = require('async')

var path = require('path')

module.exports = function (dir, cb) {

async.waterfall([ // [1]

function (next) {

fs.readdir(dir, next)

},

function (files, next) {

var paths =

files.map(function (file) { return path.join(dir,file) })

async.map(paths, fs.stat, function (er, stats) { // [2]

next(er, files, stats)

})

},

function (files, stats, next) {

var largest = stats

.filter(function (stat) { return stat.isFile() })

.reduce(function (prev, next) {

if (prev.size \> next.size) return prev

return next

})

next(null, files[stats.indexOf(largest)])

}

], cb) // [3]

}

-   [async.waterfall](https://github.com/caolan/async#waterfalltasks-callback)
    provides a series flow of execution where data from one operation
    can be passed to the next function in the series using the **next**
    callback.
-   [async.map](https://github.com/caolan/async#maparr-iterator-callback)
    lets us run fs.stat over a set of paths in parallel and calls back
    with an array (with order maintained) of the results.
-   The **cb**function will be called either after the last step has
    completed or if any error has occurred along the way. It will only
    be called once.

The async module guarantees only one callback will be fired. It also
propagates errors and manages parallelism for us.

**A promises approach**
-----------------------

[Promises](http://www.html5rocks.com/en/tutorials/es6/promises/) provide
error handling and [functional programming
perks](http://strongloop.com/strongblog/how-to-compose-node-js-promises-with-q/).
How would we approach this problem using promises? For that, let’s
utilize the [Q](https://github.com/kriskowal/q) module (although other
promise libraries could be employed):

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

var fs = require('fs')

var path = require('path')

var Q = require('q')

var fs\_readdir = Q.denodeify(fs.readdir) // [1]

var fs\_stat = Q.denodeify(fs.stat)

module.exports = function (dir) {

return fs\_readdir(dir)

.then(function (files) {

var promises = files.map(function (file) {

return fs\_stat(path.join(dir,file))

})

return Q.all(promises).then(function (stats) { // [2]

return [files, stats] // [3]

})

})

.then(function (data) { // [4]

var files = data[0]

var stats = data[1]

var largest = stats

.filter(function (stat) { return stat.isFile() })

.reduce(function (prev, next) {

if (prev.size \> next.size) return prev

return next

})

return files[stats.indexOf(largest)]

})

}

-   Since Node core functionality isn’t promise-aware, we make it so.
-   [Q.all](https://github.com/kriskowal/q/wiki/API-Reference#promiseall)
    will run all the stat calls in parallel and the result array order
    is maintained.
-   Since we want to pass files and stats to the next **then** function,
    it’s the last thing returned.

Unlike the previous examples, any *exceptions* thrown inside the promise
chain (i.e. **then**) are caught and handled. The client API changes as
well to be promise centric:

1

2

3

4

5

6

var findLargest = require('./findLargest')

findLargest('./path/to/dir')

.then(function (er, filename) {

console.log('largest file was:', filename)

})

.catch(console.error)

> Although designed this way above, you don’t have to expose a promise
> interface. Many promise libraries have a way to expose a nodeback
> style as well. With Q, we could do this using the
> [**nodeify**](https://github.com/kriskowal/q/wiki/API-Reference#wiki-promisenodeifycallback)
> function.

The scope of promises is not developed here. I would recommend reading
more about them
[here](http://strongloop.com/strongblog/promises-in-node-js-with-q-an-alternative-to-callbacks/).

**A generators approach**
-------------------------

As promised in at the beginning of the article, there is a new kid on
the block that is available to play with in Node \>=0.11.2:
*generators!*

Generators are lightweight co-routines for JavaScript. They allow a
function to be suspended and resumed via the **yield** keyword.
Generator functions have a special syntax: **function\* ()**. With this
superpower, we can also suspend and resume *asynchronous operations*
using constructs such as promises or “thunks” leading to
“synchronous-looking” asynchronous code.

> A “thunk” is a function that *returns a callback* as opposed to
> calling it*.* The callback has the same signature as your typical
> nodeback function (i.e. error is the first argument). Read more
> [here](https://github.com/visionmedia/co#thunks-vs-promises).

Let’s look at one example that enables generators for asynchronous
control flow: the [co](https://github.com/visionmedia/co) module from TJ
Holowaychuk. Here’s how to write our largest file program:

1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

var co = require('co')

var thunkify = require('thunkify')

var fs = require('fs')

var path = require('path')

var readdir = thunkify(fs.readdir) <strong\>[1]</strong\>

var stat = thunkify(fs.stat)

module.exports = co(function\* (dir) { // [2]

var files = yield readdir(dir) // [3]

var stats = yield files.map(function (file) { // [4]

return stat(path.join(dir,file))

})

var largest = stats

.filter(function (stat) { return stat.isFile() })

.reduce(function (prev, next) {

if (prev.size \> next.size) return prev

return next

})

return files[stats.indexOf(largest)] // [5]

})

-   Since Node core functionality isn’t “thunk”-aware, we make it so.
-   co takes a generator function which can be suspended at anytime
    using the **yield**keyword
-   The generator function will suspend until **readdir** returns. The
    resulting value is assigned to the **files** variable.
-   co can also handle arrays a set of parallel operations to perform. A
    result array with order maintained is assigned to **stats**.
-   The final result is returned.

We can consume this generator function with the same callback API we
specified at the beginning of this article. Co has some nice error
handling as any errors (including exceptions raised) will be passed to
the callback function. Generators also enable the use of try/catch
blocks around yield statements which co takes advantage of:

1

2

3

4

5

try {

var files = yield readdir(dir)

} catch (er) {

console.error('something happened whilst reading the directory')

}

Co has a lot of neat support for arrays, objects, nested generators,
promises and more.

> There are other generator modules rising up as well. The Q module has
> a neat
> [Q.async](https://github.com/kriskowal/q/wiki/API-Reference#qasyncgeneratorfunction)
> method that behaves similarly to co using generators.

**Wrapping up**
---------------

In this article, we investigated a variety of different approaches to
mitigating “callback hell”, that is, getting control over the flow of
your application. I am personally most intrigued by the generator idea.
I am curious how that will play out with new frameworks like
[koa](https://github.com/koajs/koa).

Although we didn’t employ it while looking at the 3rd party modules, a
modular approach can be applied to any flow libraries (async, promises,
generators). Can you think of ways to make the examples more modular?
Have a library or technique that has worked well for you? Share it in
the comments!

> Want to check out and play with all the code samples used in this
> article as well as another generator example? There is a [GitHub
> repo](https://github.com/strongloop-community/handling-callback-hell)
> set up for that!

**Use StrongOps to Monitor Node Apps**
--------------------------------------

Ready to start monitoring event loops, manage Node clusters and chase
down memory leaks? We’ve made it easy to get started with
[StrongOps](http://strongloop.com/node-js-performance/strongops/) either
locally or on your favorite cloud, with a [simple npm
install](http://strongloop.com/get-started/).

[![Screen Shot 2014-02-03 at 3.25.40
AM](http://strongloop.com/wp-content/uploads/2014/02/Screen-Shot-2014-02-03-at-3.25.40-AM.png)](http://strongloop.com/wp-content/uploads/2014/02/Screen-Shot-2014-02-03-at-3.25.40-AM.png)

**What’s next?**
----------------

-   What’s in the upcoming Node v0.12 release? [Big performance
    optimizations](http://strongloop.com/strongblog/performance-node-js-v-0-12-whats-new/),
    read [Ben Noordhuis’](https://github.com/bnoordhuis) blog to learn
    more.
-   Watch [Bert Belder’s](https://github.com/piscisaureus) comprehensive
    [video](http://strongloop.com/developers/videos/#whats-new-in-nodejs-v012)presentation
    on all the new upcoming features in v0.12
-   Ready to develop APIs in Node.js and get them connected to your
    data? We’ve made it easy to get started either locally or on your
    favorite cloud, with a [simple npm
    install](http://strongloop.com/get-started/).[\
    ](http://strongloop.com/get-started/)

[](http://www.addtoany.com/share_save)

[](http://strongloop.com/strongblog/node-js-is-faster-than-java/)

### < Previous Post

0 replies

### Leave a Reply

Want to join the discussion? \
Feel free to contribute!

### Leave a Reply [Cancel reply](/strongblog/node-js-callback-hell-promises-generators/#respond)

You must be [logged in](/login) to post a comment.

[![image](http://strongloop.com/wp-content/uploads/2013/04/btn-newsletter-signup.png)](http://strongloop.com/newsletter-registration/)

[![image](http://strongloop.com/wp-content/uploads/2013/11/sl-newsletter-archive.png)](http://strongloop.com/newsletter/)

### Most Viewed

-   [What Makes Node.js
    Faster…](http://strongloop.com/strongblog/node-js-is-faster-than-java/ "What Makes Node.js Faster Than Java?")
    posted on 01/30/2014
-   [What's New in Node.js
    v0….](http://strongloop.com/strongblog/performance-node-js-v-0-12-whats-new/ "What's New in Node.js v0.12 – Performance Optimizations")
    posted on 01/21/2014
-   [Creating Desktop
    Applicat…](http://strongloop.com/strongblog/creating-desktop-applications-with-node-webkit/ "Creating Desktop Applications With node-webkit")
    posted on 11/26/2013

### Recent Posts

-   [**Managing Node.js Callback Hell with Promises, Generators and
    Other ApproachesFebruary 3, 2014 - 7:39
    pm**](http://strongloop.com/strongblog/node-js-callback-hell-promises-generators/ "Managing Node.js Callback Hell with Promises, Generators and Other Approaches")
-   [**What Makes Node.js Faster Than Java?January 30, 2014 - 5:24
    pm**](http://strongloop.com/strongblog/node-js-is-faster-than-java/ "What Makes Node.js Faster Than Java?")
-   [**Why the Enterprise needs an API Tier built in Node.jsJanuary 29,
    2014 - 4:48
    pm**](http://strongloop.com/strongblog/node-js-api-tier-enterprise/ "Why the Enterprise needs an API Tier built in Node.js")

### Categories

-   [BACN](http://strongloop.com/strongblog/category/bacn/ "View all posts filed under BACN")
    (2)
-   [Case
    Studies](http://strongloop.com/strongblog/category/case-studies/ "View all posts filed under Case Studies")
    (3)
-   [Cloud](http://strongloop.com/strongblog/category/cloud/ "View all posts filed under Cloud")
    (8)
-   [Community](http://strongloop.com/strongblog/category/community/ "View all posts filed under Community")
    (59)
-   [Events](http://strongloop.com/strongblog/category/events/ "View all posts filed under Events")
    (7)
-   [How-To](http://strongloop.com/strongblog/category/howto/ "View all posts filed under How-To")
    (40)
-   [LoopBack](http://strongloop.com/strongblog/category/loopback-2/ "View all posts filed under LoopBack")
    (17)
-   [Mobile](http://strongloop.com/strongblog/category/mobile/ "View all posts filed under Mobile")
    (16)
-   [News](http://strongloop.com/strongblog/category/news/ "View all posts filed under News")
    (59)
-   [Product](http://strongloop.com/strongblog/category/product/ "View all posts filed under Product")
    (20)
-   [StrongNode](http://strongloop.com/strongblog/category/strongnode/ "View all posts filed under StrongNode")
    (2)
-   [StrongOps](http://strongloop.com/strongblog/category/strongops/ "View all posts filed under StrongOps")
    (4)

### Archives

-   [February
    2014](http://strongloop.com/strongblog/2014/02/ "February 2014")
-   [January
    2014](http://strongloop.com/strongblog/2014/01/ "January 2014")
-   [December
    2013](http://strongloop.com/strongblog/2013/12/ "December 2013")
-   [November
    2013](http://strongloop.com/strongblog/2013/11/ "November 2013")
-   [October
    2013](http://strongloop.com/strongblog/2013/10/ "October 2013")
-   [September
    2013](http://strongloop.com/strongblog/2013/09/ "September 2013")
-   [August
    2013](http://strongloop.com/strongblog/2013/08/ "August 2013")
-   [July 2013](http://strongloop.com/strongblog/2013/07/ "July 2013")
-   [June 2013](http://strongloop.com/strongblog/2013/06/ "June 2013")
-   [May 2013](http://strongloop.com/strongblog/2013/05/ "May 2013")
-   [April 2013](http://strongloop.com/strongblog/2013/04/ "April 2013")
-   [March 2013](http://strongloop.com/strongblog/2013/03/ "March 2013")
-   [February
    2013](http://strongloop.com/strongblog/2013/02/ "February 2013")

### Developers

-   [Forum](http://strongloop.com/developers/forums/)
-   [Training](http://strongloop.com/strongloop-suite/strongexpert/)
-   [Events](http://strongloop.com/developers/events/)
-   [Node.js
    Infographic](http://strongloop.com/developers/node-js-infographic/)

### StrongLoop Suite

-   [Why StrongLoop Suite?](http://strongloop.com/strongloop-suite/)
-   [Get Started](http://strongloop.com/strongloop-suite/get-started/)
-   [Subscription
    Plans](http://strongloop.com/strongloop-suite/subscription-plans/)
-   [Downloads](http://strongloop.com/strongloop-suite/downloads/)
-   [Support](http://strongloop.com/strongloop-suite/strongsupport/)
-   [Services](http://strongloop.com/strongloop-suite/strongexpert/)
-   [Documentation](http://docs.strongloop.com/)
-   [FAQ](http://strongloop.com/strongloop-suite/faq/)
-   [Partners](http://strongloop.com/partners/)

### About Us

-   [About StrongLoop](http://strongloop.com/about/)
-   [Management Team](http://strongloop.com/about/team/)
-   [Investors and
    Advisors](http://strongloop.com/about/investors-advisors/)
-   [Careers](http://strongloop.com/careers/)
-   [Blog](http://strongloop.com/strongblog/)
-   [Newsletter](http://strongloop.com/newsletter/)

### Contact Us

-   ![image](http://strongloop.com/wp-content/uploads/2013/08/footer-icon-email.png)[Email](http://www.cloudflare.com/email-protection#d5b6b4b9b9b7b4b6be95a6a1a7babbb2b9babaa5fbb6bab8)
-   -   ![image](http://strongloop.com/wp-content/uploads/2013/08/footer-icon-twitter.png)[Twitter](https://twitter.com/StrongLoop)
-   ![image](http://strongloop.com/wp-content/uploads/2013/08/footer-icon-github.png)[GitHub](https://github.com/strongloop)
-   ![image](http://strongloop.com/wp-content/uploads/2013/08/footer-icon-facebook.png)[Facebook](https://www.facebook.com/strongloop)
-   ![image](http://strongloop.com/wp-content/uploads/2013/04/footer-icon-googleplus.png)[Google+](https://plus.google.com/114210079151116472527)

### Legal

-   [Privacy Policy](http://strongloop.com/privacy-policy/)
-   [Terms of Use](http://strongloop.com/terms-of-use/)
-   [License](http://strongloop.com/license/)

© 2014 - StrongLoop - [Enfold Theme by Kriesi](http://www.kriesi.at)

[ StrongLoop Weekly Review – Feb 3,
2014](http://strongloop.com/strongblog/strongloop-weekly-review-feb-3-2014/)

![image](//googleads.g.doubleclick.net/pagead/viewthroughconversion/982419083/?value=0&guid=ON&script=0)

[](#top)
