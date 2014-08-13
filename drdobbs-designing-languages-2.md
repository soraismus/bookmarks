-   [Subscribe](/subscribe/)
-   [Newsletters](/newsletters/)
-   [Digital
    Library](http://www.informationweek.com/whitepaper?itc=ddj-header-twdl)
-   [RSS](/rss/)

-   -   

Search: Site Source Code

-   [Home](/)
-   [Articles](/articles)
-   [News](/news)
-   [Blogs](/blogs)
-   [Source Code](/sourcecode)
-   [Dobb's on DVD](https://store.drdobbs.com/)
-   [Dobb's TV](/tv)
-   [Webinars & Events](/webinars)

-   [![Facebook](http://twimgs.com/ddj/v2/images/h-facebook_sm.png)](http://www.facebook.com/pages/Dr-Dobbs/17631669579)
-   [![Twitter](http://twimgs.com/ddj/v2/images/h-twitter_sm.png)](http://twitter.com/dr_dobbs)

Sections*▼*

-   [Home](/)
-   [Articles](/articles)
-   [News](/news)
-   [Blogs](/blogs)
-   [Source Code](/sourcecode)
-   [Dobb's on DVD](https://store.drdobbs.com/)
-   [Dobb's TV](/tv)
-   [Webinars & Events](/webinars)

\

-   [Cloud](/cloud)
-   [Mobile](/mobile)
-   [Parallel](/parallel)
-   [.NET](/windows)
-   [JVM Languages](/jvm)
-   [C/C++](/cpp)
-   [Tools](/tools)
-   [Design](/architecture-and-design)
-   [Testing](/testing)
-   [Web Dev](/web-development)
-   [Jolt Awards](/joltawards)

Channels*▼*

-   [Cloud](/cloud)
-   [Mobile](/mobile)
-   [Parallel](/parallel)
-   [.NET](/windows)
-   [JVM Languages](/jvm)
-   [C/C++](/cpp)
-   [Tools](/tools)
-   [Design](/architecture-and-design)
-   [Testing](/testing)
-   [Web Dev](/web-development)
-   [Jolt Awards](/joltawards)

[![RSS](http://i.cmpnet.com/ddj/v2/images/rss.gif)](/rss/architecture-and-design)

Design
------

[Tweet](https://twitter.com/share)

[](http://www.addthis.com/bookmark.php?v=250&winname=addthis&pub=Dr.Dobbs&source=tbx-250&lng=en-US&s=stumbleupon&url=http://www.drdobbs.com/architecture-and-design/so-you-want-to-write-your-own-language/240165488&title=So%20You%20Want%20To%20Write%20Your%20Own%20Language?&ate=AT-Dr.Dobbs/-/fs-0/4d4fc3809f4d8545/1&sms_ss=1&at_xt=1&CXNID=2000001.5215456080540439074NXC&pre=http%3A%2F%2Fwww.drdobbs.com%2F&tt=0 "Send to StumbleUpon")
[![image](http://i.cmpnet.com/ddj/v2/images/share_email_icon.gif)](javascript:emailLauncher('/email?articleUrl=','/architecture-and-design/so-you-want-to-write-your-own-language/240165488') "Send As Email")
[![image](http://i.cmpnet.com/ddj/v2/images/share_print_icon.gif)](/article/print?articleId=240165488&siteSectionName=architecture-and-design)
[Permalink](http://www.drdobbs.com/architecture-and-design/so-you-want-to-write-your-own-language/240165488#)

So You Want To Write Your Own Language?
=======================================

By [**Walter Bright**](/authors/Walter-Bright), January 21, 2014

[](http://www.drdobbs.com/architecture-and-design/so-you-want-to-write-your-own-language/240165488#disqus_thread)

The naked truth about the joys, frustrations, and hard work of writing
your own programming language\

Now that I mention it, error messages are a big factor in the quality of
implementation of the language. It's what the user sees, after all. If
you're tempted to put out error messages like "bad syntax," perhaps you
should consider taking up a career as a chartered accountant instead of
writing a language. Good error messages are surprisingly hard to write,
and often, you won't discover how bad the error messages are until you
work the tech support emails.

The philosophies of error message handling are:

1.  Print the first message and quit. This is, of course, the simplest
    approach, and it works surprisingly well. Most compilers' follow-on
    messages are so bad that the practical programmer ignores all but
    the first one anyway. The holy grail is to find all the actual
    errors in one compile pass, leading to:
2.  Guess what the programmer intended, repair the syntax trees, and
    continue. This is an ever-popular approach. I've tried it
    indefatigably for decades, and it's just been a miserable failure.
    The compiler seems to always guess wrong, and subsequent messages
    with the "fixed" syntax trees are just ludicrously wrong.
3.  The poisoning approach. This is much like how floating-point NaNs
    are handled. Any operation with a NaN operand silently results in a
    NaN. Applying this to error recovery, and any constructs that have a
    leaf for which an error occurred, is itself considered erroneous
    (but no additional error messages are emitted for it). Hence, the
    compiler is able to detect multiple errors as long as the errors are
    in sections of code with no dependency between them. This is the
    approach we've been using in the D compiler, and are very pleased
    with the results.

What else does the user care about in the hidden part of the compiler?
Speed. I hear it over and over — compiler speed matters a lot. In fact,
compile speed is often the first thing I hear when I ask a company what
tipped the balance for choosing D. The reality is, most compilers are
pigs. To blow people away with your language, show them that it compiles
as fast as hitting the return key on the compile command.

Wanna know the secret of making your compiler fast? *Use a profiler.*

Sounds too easy, right? Trite, even. But raise your hands if you
routinely use a profiler. Be honest, everyone says they do, but that
profiler manual remains in its pristine shrink wrap. I'm just astonished
at the number of programmers who never use profilers. But it's great for
me as a competitive advantage that never ceases to pay dividends.

Some other tools you simply must be using:

-   **valgrind.** I suspect valgrind has almost single-handedly saved C
    and C++ from oblivion. I can't heap enough praise on this tool. It
    has saved my error-prone sorry self untold numbers of frustrating
    hours.
-   **git and github.** Not many tools are transformative, but these
    are. Not only do they provide an automated backup, but they enable
    collaborative work on the project by people all over the world. They
    also provide a complete history of where and from whom every line of
    code came from, in case there's a legal issue.
-   **Automated testing framework.** Compilers are enormously
    complicated beasts. Without constant testing of revisions, the
    project will reach a point where it cannot advance, as more bugs
    than improvements will be added. Add to this a coverage analyzer,
    which will show if the test suite is exercising all the code or not.
-   **Automated documentation generator.** The D project participants,
    of course, built our own (Ddoc), and it, too, was transformative.
    Before Ddoc, the documentation had only a random correlation with
    the code, and too often, they had nothing to do with each other.
    After Ddoc, the two were brought in sync.
-   **Bugzilla.** This is an automated bug tracking tool. Bugzilla
    represented a great leap forward from my pathetic older scheme of
    emails and folders, a system that simply cannot scale. Programmers
    are far less tolerant of buggy compilers than they used to be; this
    has to be addressed aggressively head on.

### Lowering

One semantic technique that is obvious in hindsight (but took Andrei
Alexandrescu to point out to me) is called "lowering." It consists of,
internally, rewriting more complex semantic constructs in terms of
simpler ones. For example, `while` loops and `foreach` loops can be
rewritten in terms of `for` loops. Then, the rest of the code only has
to deal with `for` loops. This turned out to uncover a couple of latent
bugs in how `while` loops were implemented in D, and so was a nice win.
It's also used to rewrite `scope guard` statements in terms of
`try-finally` statements, etc. Every case where this can be found in the
semantic processing will be win for the implementation.

If it turns out that there are some special-case rules in the language
that prevent this "lowering" rewriting, it might be a good idea to go
back and revisit the language design.

Any time you can find commonality in the handling of semantic
constructs, it's an opportunity to reduce implementation effort and
bugs.

### Runtime Library

Rarely mentioned, but critical, is the need to write a runtime library.
This is a major project. It will serve as a demonstration of how the
language features work, so it had better be good. Some critical things
to get right include:

1.  I/O performance. Most programs spend a lot of time in I/O. Slow I/O
    will make the whole language look bad. The benchmark is C stdio. If
    the language has elegant, lovely I/O APIs, but runs at only half the
    speed of C I/O, then it just isn't going to be attractive.
2.  Memory allocation. A high percentage of time in most programs is
    spent doing mundane memory allocation. Get this wrong at your peril.
3.  Transcendental functions. OK, I lied. Nobody cares about the
    accuracy of transcendental functions, they only care about their
    speed. My proof comes from trying to port the D runtime library to
    different platforms, and discovering that the underlying C
    transcendental functions often fail the accuracy tests in the D
    library test suite. C library functions also often do a poor job
    handling the arcana of the IEEE floating-point bestiary — NaNs,
    infinities, subnormals, negative 0, etc. In D, we compensated by
    implementing the transcendental functions ourselves. Transcendental
    floating-point code is pretty tricky and arcane to write, so I'd
    recommend finding an existing library you can license and adapting
    that. \
    A common trap people fall into with standard libraries is filling
    them up with trivia. Trivia is sand clogging the gears and just dead
    weight that has to be carried around forever. My general rule is if
    the explanation for what the function does is more lines than the
    implementation code, then the function is likely trivia and should
    be booted out.

### After The Prototype

You've done it, you've got a great prototype of a new language. Now
what? Next comes the hardest part. This is where most new languages
fail. You'll be doing what every nascent rock band does — play shopping
malls, high school dances, dive bars, and so on, slowly building up an
audience. For languages, this means preparing presentations, articles,
tutorials, and books on the language. Then, going to programmer
meetings, conferences, companies, anywhere they'll have you, and showing
it off. You'll get used to public speaking, and even find you enjoy it.
(I enjoy it a lot.)

There's one huge thing working in your favor: With the global reach of
the Internet, there's an instantly reachable global audience. Another
favorable fact is that programmer meetings, conferences, etc., all are
looking for great content. They love talks about new languages and new
programming ideas. My experience with such audiences is that they are
friendly and will give you lots of constructive feedback.

Of course, then you'll almost certainly be forced to reevaluate some
cherished features of the language and reengineer them.

But hey, you went into this with your eyes open!

### Acknowledgment

Thanks to Andrei Alexandrescu for his advice on this article.

* * * * *

*Walter Bright is the designer of the D language. He regularly blogs
for* [Dr. Dobb's](http://www.drdobbs.com/author/Walter-Bright).

\

\
[Previous](/architecture-and-design/so-you-want-to-write-your-own-language/240165488?pgno=1)
[1](/architecture-and-design/so-you-want-to-write-your-own-language/240165488?pgno=1)
**2**

### Related Reading

-   [News](#toc-news "News")
-   [Commentary](#toc-commentary "Commentary")

### News

-   [A Node-Locked Software
    Painkiller](/cloud/a-node-locked-software-painkiller/240165756?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "A Node-Locked Software Painkiller")
-   [Temboo's Sketch Builder Arduino
    Yún](/tools/temboos-sketch-builder-arduino-yn/240165696?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Temboo's Sketch Builder Arduino Yún")
-   [Transforming .NET To Fully Instrumented
    SaaS](/cloud/transforming-net-to-fully-instrumented-s/240165483?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Transforming .NET To Fully Instrumented SaaS")
-   [Plugging API Power Into Big Data Predictive
    Analytics](/tools/plugging-api-power-into-big-data-predict/240165440?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Plugging API Power Into Big Data Predictive Analytics")

### Commentary

-   [A PaaS With "Deep Support" For .NET and
    Java](/windows/a-paas-with-deep-support-for-net-and-jav/240165757?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "A PaaS With ")
-   [Developer Internet of Things App Starter
    Kit](/web-development/developer-internet-of-things-app-starter/240165717?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "Developer Internet of Things App Starter Kit")
-   [Galileo: The Slowest Fast Computer
    Around?](/embedded-systems/galileo-the-slowest-fast-computer-around/240165716?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "Galileo: The Slowest Fast Computer Around?")
-   [Worksoft Integrates with
    JIRA](/cloud/worksoft-integrates-with-jira/240165674?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "Worksoft Integrates with JIRA")

-   [Slideshow](#toc-slideshow "Slideshow")
-   [Video](#toc-video "Video")

### Slideshow

-   [Jolt Awards: The Best
    Books](/joltawards/jolt-awards-the-best-books/240162065?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Jolt Awards: The Best Books")
-   [Jolt Awards: The Best Testing
    Tools](/joltawards/jolt-awards-the-best-testing-tools/240155296?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Jolt Awards: The Best Testing Tools")
-   [Jolt Awards 2013: The Best Programmer
    Libraries](/joltawards/jolt-awards-2013-the-best-programmer-lib/240151743?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Jolt Awards 2013: The Best Programmer Libraries")
-   [Jolt Awards: The Best
    Books](/joltawards/jolt-awards-the-best-books/240007480?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Jolt Awards: The Best Books")

### Video

-   [Stephen Wolfram
    Interview](/embedded-systems/stephen-wolfram-interview/240164413?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Stephen Wolfram Interview")
-   [PTECH: Educating for
    Innovation](/architecture-and-design/ptech-educating-for-innovation/240163352?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "PTECH: Educating for Innovation")
-   [Perceptual
    Computing](/tools/perceptual-computing/240152397?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Perceptual Computing")
-   [Latest Ericsson Mobility
    Report](/mobile/latest-ericsson-mobility-report/240142893?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Latest Ericsson Mobility Report")

-   [Most Popular](#toc-most-popular "Most Popular")

### Most Popular

-   [Engineering Managers Should Code 30% of Their
    Time](/architecture-and-design/engineering-managers-should-code-30-of-t/240165174?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "Engineering Managers Should Code 30% of Their Time")
-   [A Simple and Efficient FFT Implementation in C++:\
     Part
    I](/cpp/a-simple-and-efficient-fft-implementatio/199500857?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "A Simple and Efficient FFT Implementation in C++:<br> Part I")
-   [Logging In
    C++](/cpp/logging-in-c/201804215?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "Logging In C++")
-   [Writing Lock-Free Code: A Corrected
    Queue](/parallel/writing-lock-free-code-a-corrected-queue/210604448?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "Writing Lock-Free Code: A Corrected Queue")

* * * * *

### More Insights

### White Papers

-   [Optimizing data storage in federal government data
    centers](http://www.informationweek.com/whitepaper/government/cloud-saas/optimizing-data-storage-in-federal-government-data-wp1344373902?articleID=191705448&cid=SBX_ddj_well_wp_default_architecture_and_design&itc=SBX_ddj_well_wp_default_architecture_and_design)
-   [Reducing Cost & Complexity: Pay as you grow data
    protection](http://www.informationweek.com/whitepaper/Storage/Storage-Systems/reducing-cost-complexity-pay-as-you-grow-data-p-wp1381011785?articleID=191739900&cid=SBX_ddj_well_wp_default_architecture_and_design&itc=SBX_ddj_well_wp_default_architecture_and_design)

[More
\>\>](/whitepaper/architecture-and-design/more.html?cid=SBX_ddj_well_wp_default_architecture_and_design&itc=SBX_ddj_well_wp_default_architecture_and_design "More White Papers")

### Reports

-   [State of Cloud 2011: Time for Process
    Maturation](http://analytics.informationweek.com/abstract/5/5116/Cloud-Computing/research-2011-state-of-cloud.html?cid=SBX_ddj_well_Analytics_default_architecture_and_design&itc=SBX_ddj_well_Analytics_default_architecture_and_design)
-   [SaaS and E-Discovery: Navigating Complex
    Waters](http://analytics.informationweek.com/abstract/5/5638/Cloud-Computing/strategy-saas-and-e-discovery.html?cid=SBX_ddj_well_Analytics_default_architecture_and_design&itc=SBX_ddj_well_Analytics_default_architecture_and_design)

[More
\>\>](/analytics/architecture-and-design/more.html?cid=SBX_ddj_well_Analytics_default_architecture_and_design&itc=SBX_ddj_well_Analytics_default_architecture_and_design "More Reports")

### Webcasts

-   [Client Windows Migration: Expert Tips for Application
    Readiness](http://www.enterpriseefficiency.com/webinar.asp?webinar_id=30046&webinar_promo=30457&cid=SBX_ddj_well_Webcast_default_architecture_and_design&itc=SBX_ddj_well_Webcast_default_architecture_and_design&K=SBX_DDJ_WL)
-   [Agile Service Desk: Keeping Pace or Getting out Paced by New
    Technology?](https://webinar.informationweek.com/16480?keycode=SBX&cid=SBX_ddj_well_Webcast_default_architecture_and_design&itc=SBX_ddj_well_Webcast_default_architecture_and_design&K=SBX_DDJ_WL)

[More
\>\>](/webcast/architecture-and-design/more.html?cid=SBX_ddj_well_Webcast_default_architecture_and_design&itc=SBX_ddj_well_Webcast_default_architecture_and_design "More Webcast")

* * * * *

INFO-LINK

-   -   -   -   

\
 \

![image](http://twimgs.com/ddj/images/dobbs_disqus_logo.gif)

\
\

### Currently we allow the following HTML tags in comments:

### Single tags

*These tags can be used alone and don't need an ending tag.*

`<br>` Defines a single line break \
\
 `<hr>` Defines a horizontal line

### Matching tags

*These require an ending tag - e.g. `<i>italic text</i>`*

`<a>` Defines an anchor\
\
 `<b>` Defines bold text\
\
 `<big>` Defines big text\
\
 `<blockquote>` Defines a long quotation\
\
 `<caption>` Defines a table caption\
\
 `<cite>` Defines a citation\
\
 `<code>` Defines computer code text\
\
 `<em>` Defines emphasized text\
\
 `<fieldset>` Defines a border around elements in a form\
\
 `<h1>` This is heading 1\
\
 `<h2>` This is heading 2\
\
 `<h3>` This is heading 3\
\
 `<h4>` This is heading 4\
\
 `<h5>` This is heading 5\
\
 `<h6>` This is heading 6\
\
 `<i>` Defines italic text\
\
 `<p>` Defines a paragraph\
\
 `<pre>` Defines preformatted text\
\
 `<q>` Defines a short quotation\
\
 `<samp>` Defines sample computer code text\
\
 `<small>` Defines small text\
\
 `<span>` Defines a section in a document\
\
 `<s>` Defines strikethrough text\
\
 `<strike>` Defines strikethrough text \
\
 `<strong>` Defines strong text \
\
 `<sub>` Defines subscripted text \
\
 `<sup>` Defines superscripted text \
\
 `<u>` Defines underlined text\
\

*Dr. Dobb's* encourages readers to engage in spirited, healthy debate,
including taking us to task. However, *Dr. Dobb's* moderates all
comments posted to our site, and reserves the right to modify or remove
any content that it determines to be derogatory, offensive,
inflammatory, vulgar, irrelevant/off-topic, racist or obvious marketing
or spam. *Dr. Dobb's* further reserves the right to disable the profile
of any commenter participating in said activities.

  ------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  ![Disqus Tips](http://twimgs.com/informationweek/Disqus_art/hintbox_info.jpg)   **To upload an avatar photo,** first [complete your Disqus profile.](http://disqus.com/dashboard/) | [View the list](#) of **supported HTML tags** you can use to style comments. | Please read our [**commenting policy.**](#)
  ------------------------------------------------------------------------------- ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Design Recent Articles
----------------------

-   [Jolt Awards: Coding
    Tools](/architecture-and-design/jolt-awards-coding-tools/240165725)
-   [Interoperating Between C++ and
    Objective-C](/architecture-and-design/interoperating-between-c-and-objective-c/240165502)
-   [A Quarter Century of
    Tcl](/architecture-and-design/a-quarter-century-of-tcl/240165482)
-   [The Dangerous Art of Predicting Tech
    Directions](/architecture-and-design/the-dangerous-art-of-predicting-tech-dir/240165376)
-   [Engineering Managers Should Code 30% of Their
    Time](/architecture-and-design/engineering-managers-should-code-30-of-t/240165174)
-   [](/architecture-and-design/)
-   [](/architecture-and-design/)

Most Popular
------------

[Stories](#) [Blogs](#)

-   [Jolt Awards: Coding
    Tools](/architecture-and-design/jolt-awards-coding-tools/240165725)
-   [So You Want To Write Your Own
    Language?](/architecture-and-design/so-you-want-to-write-your-own-language/240165488)
-   [2013 Developer Salary
    Survey](/architecture-and-design/2013-developer-salary-survey/240163580)
-   [Jolt Awards: The Best
    Books](/architecture-and-design/jolt-awards-the-best-books/240162065)
-   [Engineering Managers Should Code 30% of Their
    Time](/architecture-and-design/engineering-managers-should-code-30-of-t/240165174)
-   [](/architecture-and-design/)
-   [](/architecture-and-design/)

-   [C++11:
    unique\_ptr](/architecture-and-design/c11-uniqueptr/240002708)
-   [Scrum's Flawed Notion of Product
    Owner](/architecture-and-design/scrums-flawed-notion-of-product-owner/240165148)
-   [Scrum != Agile](/architecture-and-design/scrum-agile/240164386)
-   [SDLC: SDLC models Advantages &
    disadvantages](/architecture-and-design/sdlc-sdlc-models-advantages-disadvantag/228701074)
-   [OO as a Prerequisite to
    Agile](/architecture-and-design/oo-as-a-prerequisite-to-agile/240164883)
-   [](/architecture-and-design/)
-   [](/architecture-and-design/)

### Video

[**View All Videos**](/tv)

This month's Dr. Dobb's Journal
-------------------------------

[![The Dr. Dobb's Digital Digest cover - February
Issue](http://twimgs.com/ddj/digital/covers/ddj201401-314.jpg)](/digital/012214?k=ddjtm&cid=onedit_ds_ddjtm)

[**This month**](/digital/012214?k=ddjtm&cid=onedit_ds_ddjtm), we focus
in on languages. The creator of Nimrod gives you a tour of a language
that compiles to C, C++, Objective-C, and JavaScript. Walter Bright
fills you in on how he came to write D. The dark details of unit testing
in Python are revealed, **[and much
more!](/digital/012214?k=ddjtm&cid=onedit_ds_ddjtm)**\
\
 [Download the latest issue today.
\>\>](/digital/012214?k=ddjtm&cid=onedit_ds_ddjtm)

Upcoming Events
---------------

[Live Events](#tab_live-events "Live Events")
[WebCasts](#tab_webcasts "WebCasts")

-   [Learn to Leverage Video for your Business @ Enterprise Connect
    3/17-3/20 - Enterprise
    Connect](http://www.enterpriseconnect.com/orlando/conference/video.php/?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Acknowledge the Inevitable: How to Prepare For, Respond to, and
    Recover from a Security Incident - Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/15?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Building the Physical Network for the Software-Defined Data Center
    - Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/6?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Navigating the Big Spectrum of Big Data's Solutions Workshop |
    March 31 at Interop - Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/21?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Application Analysis Day Workshop | March 31 at Interop Las Vegas -
    Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/12?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)

[Deeper Network Security: Protection Tips
Revealed](https://webinar.informationweek.com/16445?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[Innovations in Integration: Achieving Holistic Rapid Detection and
Response](https://webinar.informationweek.com/15579?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[How to Improve Customer Analytics: Best
Practices](https://webinar.informationweek.com/16450?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[Are You Really Ready for a Software
Audit?](http://webinar.informationweek.com/16502?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[Want Information Fast or Want it Right? Learn How to Have
Both](http://webinar.informationweek.com/15832?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

\
 \
 \

[More
Webcasts\>\>](/webcast/architecture_and_design/more.html?cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design)

Featured Reports
----------------

[What's
this?](# "UBM Smart boxes auto deliver premium content that is contextually relevant to the article or site section where it is located")

-   [Return of the
    Silos](http://analytics.informationweek.com/abstract/1/7955/Application-Performance-Optimization/return-of-the-silos.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [State of Cloud 2011: Time for Process
    Maturation](http://analytics.informationweek.com/abstract/5/5116/Cloud-Computing/research-2011-state-of-cloud.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [SaaS 2011: Adoption Soars, Yet Deployment Concerns
    Linger](http://analytics.informationweek.com/abstract/5/5674/Cloud-Computing/research-saas-2011.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [Research: State of the IT Service
    Desk](http://analytics.informationweek.com/abstract/21/7594/Security/research-state-of-the-it-service-desk.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [Will IPv6 Make Us
    Unsafe?](http://analytics.informationweek.com/abstract/21/7375/Security/will-ipv6-make-us-unsafe*.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)

[More
\>\>](/analytics/architecture_and_design/more.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)

![image](http://twimgs.com/informationweek/smartbox/images/smartbox.PNG)

\

Featured Whitepapers
--------------------

[What's
this?](# "UBM Smart boxes auto deliver premium content that is contextually relevant to the article or site section where it is located")

-   [Simplify Cloud Infrastructure with IBM PureFlex and IBM Flex
    System](http://www.informationweek.com/whitepaper/Storage/Storage-Systems/simplify-cloud-infrastructure-with-ibm-pureflex-an-wp1382071216?articleID=191739957&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [IBM System Storage Interactive Product Guide: Intelligent,
    efficient and automated storage for your IT
    infrastructure](http://www.informationweek.com/whitepaper/Storage/Storage-Systems/ibm-system-storage-interactive-product-guide-inte-wp1390835549?articleID=191740771&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [Enterprise Integration Patterns Flash
    Cards](http://www.informationweek.com/whitepaper/Business-Intelligence/Business-Process-Management/enterprise-integration-patterns-flash-cards-wp1391012138?articleID=191740783&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [How Insurers Can Effectively Meet the Challenges of the
    ACA](http://www.insurancetech.com/whitepaper/Regulation-Financial-Systems/Compliance/how-insurers-can-effectively-meet-the-challenges-o-wp1390839773?articleID=191740763&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [The Role of Integrated Network Services in Next-Generation
    Application
    Delivery](http://www.informationweek.com/whitepaper/Infrastructure/Network-Systems-Management/the-role-of-integrated-network-services-in-next-ge-wp1390513664?articleID=191740755&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)

[More
\>\>](/whitepaper/architecture_and_design/more.html?cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)

![image](http://twimgs.com/informationweek/smartbox/images/smartbox.PNG)

\

Most Recent Premium Content
---------------------------

[Digital Issues](/digitaledition/)

-   [February - **Languages**](http://dc.ubm-us.com/i/245989)
-   [The Design of Messaging Middleware and 10 Tips from Tech
    Writers](http://www.drdobbs.com/digital/012914/)
-   [January - **Mobile
    Development**](http://www.drdobbs.com/digital/121712/?cid=ddj_premium_January2013)
-   [February - **Parallel
    Programming**](http://www.drdobbs.com/digital/012213/?cid=ddj_premium_February2013)
-   [March - **Windows
    Programming**](http://www.drdobbs.com/digital/022513/?cid=ddj_premium_March2013)
-   [April - **Programming
    Languages**](http://www.drdobbs.com/digital/032513/?cid=ddj_premium_April2013)
-   [May - **Web
    Development**](http://www.drdobbs.com/digital/042213/?cid=ddj_premium_May2013)
-   [June - **Database
    Development**](http://www.drdobbs.com/digital/052013/?cid=ddj_premium_June2013)
-   [July -
    **Testing**](http://www.drdobbs.com/digital/062413/?cid=ddj_premium_July2013)
-   [August - **Debugging and Defect
    Management**](http://www.drdobbs.com/digital/072213/?cid=ddj_premium_August2013)
-   [September - **Version
    Control**](http://www.drdobbs.com/digital/082613)
-   [October - **DevOps**](http://www.drdobbs.com/digital/092313/)
-   [November- **Really Big
    Data**](http://www.drdobbs.com/digital/102113)
-   [December -
    **Design**](http://www.drdobbs.com/digital/111113?k=ddjtm&cid=onedit_ds_ddjtm)
-   [January - **C &
    C++**](http://www.drdobbs.com/digital/121911/?cid=ddj_premium_January2012)
-   [February - **Parallel
    Programming**](http://www.drdobbs.com/digital/011912/?cid=ddj_premium_February2012)
-   [March - **Microsoft
    Technologies**](http://www.drdobbs.com/digital/021912/?cid=ddj_premium_March2012)
-   [April - **Mobile
    Development**](http://www.drdobbs.com/digital/031912/?cid=ddj_premium_April2012)
-   [May - **Database
    Programming**](http://www.drdobbs.com/digital/042312/?cid=ddj_premium_May2012)
-   [June - **Web
    Development**](http://www.drdobbs.com/digital/052112/?cid=ddj_premium_June2012)
-   [July -
    **Security**](http://www.drdobbs.com/digital/061812/?cid=ddj_premium_July2012)
-   [August - **ALM & Development
    Tools**](http://www.drdobbs.com/digital/072312/?cid=ddj_premium_August2012)
-   [September - **Cloud & Web
    Development**](http://www.drdobbs.com/digital/082012/?cid=ddj_premium_September2012)
-   [October - **JVM
    Languages**](http://www.drdobbs.com/digital/092412/?cid=ddj_premium_October2012)
-   [November -
    **Testing**](http://www.drdobbs.com/digital/102212/?cid=ddj_premium_November2012)
-   [December -
    **DevOps**](http://www.drdobbs.com/digital/111912/?cid=ddj_premium_December2012)

\
\

[![UBM
Tech](http://twimgs.com/ddj/v2/images/ubm_tech_logo_footer_grey88x111.jpg)](http://tech.ubm.com/)

-   FEATURED UBM TECH SITES:
-   [InformationWeek](http://www.informationweek.com/)
-   [Network Computing](http://www.networkcomputing.com/)
-   [Dr. Dobb's](http://www.drdobbs.com/)
-   [Dark Reading](http://www.darkreading.com/)

-   OUR MARKETS:
-   [Business
    Technology](http://tech.ubm.com/businesses/business-technology/)
-   [Electronics](http://tech.ubm.com/businesses/electronics/)
-   [Game & App
    Development](http://tech.ubm.com/businesses/game-app-development/)

-   **Working With Us:**
-   [Advertising
    Contacts](http://createyournextcustomer.techweb.com/contact-us/)
-   [Event Calendar](http://events.ubm.com/?company=10)
-   [Tech Marketing
    Solutions](http://createyournextcustomer.techweb.com)
-   [Corporate Site](http://tech.ubm.com/)
-   [Contact Us / Feedback](mailto:feedback@techweb.com)

-   [Terms of Service](http://legal.us.ubm.com/terms-of-service/)
-   [Privacy Statement](http://legal.us.ubm.com/privacy-policy/)
-   [Copyright © 2014 UBM Tech, All rights
    reserved](http://legal.us.ubm.com/copyright-notice/)
-   [Powered by Zend/PHP](http://www.zend.com)

-   [Dr. Dobb's Home](/)
-   [Articles](/articles)
-   [News](/news)
-   [Blogs](/blogs)
-   [Source Code](/sourcecode)
-   [Dobb's on DVD](https://store.drdobbs.com/)
-   [Dobb's TV](/tv)
-   [Webinars & Events](/webinars)

-   [About Us](/aboutus)
-   [Contact Us](/contactus)
-   [Site Map](/sitemap)
-   [Editorial Calendar](/edcal)

\

[![image](http://cmpglobalvista.112.2O7.net/b/ss/cmpglobalvista/1/H.16--NS/0)](http://www.omniture.com "Web Analytics")
