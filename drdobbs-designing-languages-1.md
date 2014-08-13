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

My career has been all about designing programming languages and writing
compilers for them. This has been a great joy and source of satisfaction
to me, and perhaps I can offer some observations about what you're in
for if you decide to design and implement a professional programming
language. This is actually a book-length topic, so I'll just hit on a
few highlights here and avoid topics well covered elsewhere.

### Work

First off, you're in for a lot of work…years of work…most of which will
be wandering in the desert. The odds of success are heavily stacked
against you. If you are not strongly self-motivated to do this, it isn't
going to happen. If you need validation and encouragement from others,
it isn't going to happen.

Fortunately, embarking on such a project is not major dollar investment;
it won't break you if you fail. Even if you do fail, depending on how
far the project got, it can look pretty good on your résumé and be good
for your career.

### Design

One thing abundantly clear is that syntax matters. It matters an awful
lot. It's like the styling on a car — if the styling is not appealing,
it simply doesn't matter how hot the performance is. The syntax needs to
be something your target audience will like.

Trying to go with something they've not seen before will make language
adoption a much tougher sell.

I like to go with a mix of familiar syntax and aesthetic beauty. It's
got to look good on the screen. After all, you're going to spend plenty
of time looking at it. If it looks awkward, clumsy, or ugly, it will
taint the language.

There are a few things I (perhaps surprisingly) suggest should not be
considerations. These are false gods:

1.  Minimizing keystrokes. Maybe this mattered when programmers used
    paper tape, and it matters for small languages like bash or awk. For
    larger applications, much more programming time is spent reading
    than writing, so reducing keystrokes shouldn't be a goal in itself.
    Of course, I'm not suggesting that large amounts of boilerplate is a
    good idea.
2.  Easy parsing. It isn't hard to write parsers with arbitrary
    lookahead. The looks of the language shouldn't be compromised to
    save a few lines of code in the parser. Remember, you'll spend a lot
    of time staring at the code. That comes first. As mentioned below,
    it still should be a context-free grammar.
3.  Minimizing the number of keywords. This metric is just silly, but I
    see it cropping up repeatedly. There are a million words in the
    English language, I don't think there is any looming shortage. Just
    use your good judgment.

Things that are true gods:

1.  Context-free grammars. What this really means is the code should be
    parsable without having to look things up in a symbol table. C++ is
    famously not a context-free grammar. A context-free grammar, besides
    making things a lot simpler, means that IDEs can do syntax
    highlighting without integrating most of a compiler front end. As a
    result, third-party tools become much more likely to exist.
2.  Redundancy. Yes, the grammar should be redundant. You've all heard
    people say that statement terminating `;` are not necessary because
    the compiler can figure it out. That's true — but such
    non-redundancy makes for incomprehensible error messages. Consider a
    syntax with no redundancy: Any random sequence of characters would
    then be a valid program. No error messages are even possible. A good
    syntax needs redundancy in order to diagnose errors and give good
    error messages.
3.  Tried and true. Absent a very strong reason, it's best to stick with
    tried and true grammatical forms for familiar constructs. It really
    cuts the learning curve for the language and will increase adoption
    rates. Think of how people will hate the language if it swaps the
    operator precedence of `+` and `*`. Save the divergence for features
    not generally seen before, which also signals the user that this is
    new.

As always, these principles should not be taken as dicta. Use good
judgment. Any language design principle blindly followed leads to
disaster. The principles are rarely orthogonal and frequently conflict.
It's a lot like designing a house — making the master closet bigger
means the master bedroom gets smaller. It's all about finding the right
balance.

Getting past the syntax, the meat of the language will be the semantic
processing, which is where meaning is assigned to the syntactical
constructs. This is where you'll be spending the vast bulk of design and
implementation. It's much like the organs in your body — they are unseen
and we don't think about them unless they are going wrong. There won't
be a lot of glory in the semantic work, but in it will be the whole
point of the language.

Once through the semantic phase, the compiler does optimizations and
then code generation — collectively called the "back end." These two
passes are very challenging and complicated. Personally, I love working
with this stuff, and grumble that I've got to spend time on other
issues. But unless you really like it, and it takes a fairly unhinged
programmer to delight in the arcana of such things, I recommend taking
the common sense approach and using an existing back end, such as the
JVM, CLR, gcc, or LLVM. (Of course, I can always set you up with the
glorious Digital Mars back end!)

### Implementation

How best to implement it? I hope I can at least set you off in the right
direction. The first tool that beginning compiler writers often reach
for is regex. Regex is just the wrong tool for lexing and parsing. Rob
Pike [explains
why](http://commandcenter.blogspot.com/2011/08/regular-expressions-in-lexing-and.html)
reasonably well. I'll close that with the famous quote from Jamie
Zawinski:

*"Some people, when confronted with a problem, think 'I know, I'll use
regular expressions.' Now they have two problems."*

Somewhat more controversial, I wouldn't bother wasting time with lexer
or parser generators and other so-called "compiler compilers." They're a
waste of time. Writing a lexer and parser is a tiny percentage of the
job of writing a compiler. Using a generator will take up about as much
time as writing one by hand, and it will marry you to the generator
(which matters when porting the compiler to a new platform). And
generators also have the unfortunate reputation of emitting lousy error
messages.

\

\
**1**
[2](/architecture-and-design/so-you-want-to-write-your-own-language/240165488?pgno=2)
[Next](/architecture-and-design/so-you-want-to-write-your-own-language/240165488?pgno=2)

### Related Reading

-   [News](#toc-news "News")
-   [Commentary](#toc-commentary "Commentary")

### News

-   [Telerik Shoots Down Silver Bullet
    Frameworks](/mobile/telerik-shoots-down-silver-bullet-framew/240165853?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Telerik Shoots Down Silver Bullet Frameworks")
-   [Open Source Spika For Mobile Comms
    Apps](/mobile/open-source-spika-for-mobile-comms-apps/240165793?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Open Source Spika For Mobile Comms Apps")
-   [Temboo's Sketch Builder Arduino
    Yún](/tools/temboos-sketch-builder-arduino-yn/240165696?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Temboo's Sketch Builder Arduino Yún")
-   [Fully Supported OpenJDK From
    Azul](/jvm/fully-supported-openjdk-from-azul/240165650?cid=SBX_ddj_related_news_default_architecture_and_design&itc=SBX_ddj_related_news_default_architecture_and_design "Fully Supported OpenJDK From Azul")

### Commentary

-   [Ansible Shines With Watches, Stars, and
    Forks](/tools/ansible-shines-with-watches-stars-and-fo/240165874?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "Ansible Shines With Watches, Stars, and Forks")
-   [Open Source Spika For Mobile Comms
    Apps](/mobile/open-source-spika-for-mobile-comms-apps/240165793?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "Open Source Spika For Mobile Comms Apps")
-   [A Node-Locked Software
    Painkiller](/cloud/a-node-locked-software-painkiller/240165756?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "A Node-Locked Software Painkiller")
-   [Windows 9 "Threshold" To Preview At Build
    2014](/windows/windows-9-threshold-to-preview-at-build/240165586?cid=SBX_ddj_related_commentary_default_architecture_and_design&itc=SBX_ddj_related_commentary_default_architecture_and_design "Windows 9 ")

-   [Slideshow](#toc-slideshow "Slideshow")
-   [Video](#toc-video "Video")

### Slideshow

-   [Jolt Awards: Coding
    Tools](/joltawards/jolt-awards-coding-tools/240165725?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Jolt Awards: Coding Tools")
-   [2013 Developer Salary
    Survey](/architecture-and-design/2013-developer-salary-survey/240163580?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "2013 Developer Salary Survey")
-   [Developer Reading
    List](/architecture-and-design/developer-reading-list/240145159?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Developer Reading List")
-   [Developer's Reading
    List](/tools/developers-reading-list/240003875?cid=SBX_ddj_related_slideshow_default_architecture_and_design&itc=SBX_ddj_related_slideshow_default_architecture_and_design "Developer's Reading List")

### Video

-   [Teen Computer Scientist Wins Big at
    ISEF](/architecture-and-design/teen-computer-scientist-wins-big-at-isef/240155247?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Teen Computer Scientist Wins Big at ISEF")
-   [Intel's Silvermont
    Microarchitecture](/architecture-and-design/intels-silvermont-microarchitecture/240154584?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Intel's Silvermont Microarchitecture")
-   [Heterogeneous
    Programming](/parallel/heterogeneous-programming/240144126?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Heterogeneous Programming")
-   [Latest Ericsson Mobility
    Report](/mobile/latest-ericsson-mobility-report/240142893?cid=SBX_ddj_related_video_default_architecture_and_design&itc=SBX_ddj_related_video_default_architecture_and_design "Latest Ericsson Mobility Report")

-   [Most Popular](#toc-most-popular "Most Popular")

### Most Popular

-   [Jolt Awards: Coding
    Tools](/joltawards/jolt-awards-coding-tools/240165725?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "Jolt Awards: Coding Tools")
-   [Developer Reading List: The Must-Have Books for
    JavaScript](/tools/developer-reading-list-the-must-have-boo/240148421?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "Developer Reading List: The Must-Have Books for JavaScript")
-   [The Rise And Fall of Languages in
    2013](/jvm/the-rise-and-fall-of-languages-in-2013/240165192?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "The Rise And Fall of Languages in 2013")
-   [IBM Without Its Business
    Machines](/cloud/ibm-without-its-business-machines/240165748?cid=SBX_ddj_related_mostpopular_default_architecture_and_design&itc=SBX_ddj_related_mostpopular_default_architecture_and_design "IBM Without Its Business Machines")

* * * * *

### More Insights

### White Papers

-   [VMware Solution Brief for
    K-12](http://www.informationweek.com/whitepaper/Business-Intelligence/Information-Management/vmware-solution-brief-for-k-12-wp1387828187?articleID=191740533&cid=SBX_ddj_well_wp_default_architecture_and_design&itc=SBX_ddj_well_wp_default_architecture_and_design)
-   [How Insurers Can Effectively Meet the Challenges of the
    ACA](http://www.insurancetech.com/whitepaper/Regulation-Financial-Systems/Compliance/how-insurers-can-effectively-meet-the-challenges-o-wp1390839773?articleID=191740763&cid=SBX_ddj_well_wp_default_architecture_and_design&itc=SBX_ddj_well_wp_default_architecture_and_design)

[More
\>\>](/whitepaper/architecture-and-design/more.html?cid=SBX_ddj_well_wp_default_architecture_and_design&itc=SBX_ddj_well_wp_default_architecture_and_design "More White Papers")

### Reports

-   [Research: State of the IT Service
    Desk](http://analytics.informationweek.com/abstract/21/7594/Security/research-state-of-the-it-service-desk.html?cid=SBX_ddj_well_Analytics_default_architecture_and_design&itc=SBX_ddj_well_Analytics_default_architecture_and_design)
-   [Database
    Defenses](http://analytics.informationweek.com/abstract/21/7616/Security/database-defenses.html?cid=SBX_ddj_well_Analytics_default_architecture_and_design&itc=SBX_ddj_well_Analytics_default_architecture_and_design)

[More
\>\>](/analytics/architecture-and-design/more.html?cid=SBX_ddj_well_Analytics_default_architecture_and_design&itc=SBX_ddj_well_Analytics_default_architecture_and_design "More Reports")

### Webcasts

-   [Thwart off Application-Based Security Exploits: Protect Against
    Zero-Day Attacks, Malware, Advanced Persistent
    Threats](https://webinar.informationweek.com/16544?keycode=SBX&cid=SBX_ddj_well_Webcast_default_architecture_and_design&itc=SBX_ddj_well_Webcast_default_architecture_and_design&K=SBX_DDJ_WL)
-   [Datacenter Modernization: How Customers are Standardizing in
    Preparation for the
    Future](https://webinar.informationweek.com/16302?keycode=SBX&cid=SBX_ddj_well_Webcast_default_architecture_and_design&itc=SBX_ddj_well_Webcast_default_architecture_and_design&K=SBX_DDJ_WL)

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

-   [Explore the Latest in Unified Comm at Enterprise Connect March
    17-20 - Enterprise
    Connect](http://www.enterpriseconnect.com/orlando/conference/unified-communications.php/?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Explore Cloud Comm. @ Enterprise Connect | $200 Off with Code MPIWK
    - Enterprise
    Connect](You'll%20come%20away%20with%20input%20that%20helps%20you%20decide%20how%20to%20use%20the%20Cloud%20for%20maximum%20benefit%20in%20your%20Communications%20&%20Collaboration%20future.&cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Humans Aren’t Computers: Effective Management Strategies for IT
    Leaders - Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/24?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Acknowledge the Inevitable: How to Prepare For, Respond to, and
    Recover from a Security Incident - Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/15?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)
-   [Developing and Deploying Cross-Platform Mobile Applications -
    Interop Las
    Vegas](http://www.interop.com/lasvegas/schedule-builder/session-id/16?cid=SBX_ddj_fture_LiveEvent_default_architecture_and_design&itc=SBX_ddj_fture_LiveEvent_default_architecture_and_design&_mc=MP_BTMSBXDDJFT)

[Client Windows Migration: Expert Tips for Application
Readiness](http://www.enterpriseefficiency.com/webinar.asp?webinar_id=30046&webinar_promo=30457&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[WinXP at the 11th
Hour](https://webinar.informationweek.com/14525?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[Innovations in Integration: Achieving Holistic Rapid Detection and
Response](https://webinar.informationweek.com/15579?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[Balancing BYOD with Security and
Manageability](https://webinar.informationweek.com/16349?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

[How to Really Put Big Data to
Work](https://webinar.informationweek.com/16385?keycode=SBX&cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design&K=SBX_DDJ_FT)

\
 \
 \

[More
Webcasts\>\>](/webcast/architecture_and_design/more.html?cid=SBX_ddj_fture_Webcast_default_architecture_and_design&itc=SBX_ddj_fture_Webcast_default_architecture_and_design)

Featured Reports
----------------

[What's
this?](# "UBM Smart boxes auto deliver premium content that is contextually relevant to the article or site section where it is located")

-   [State of Cloud 2011: Time for Process
    Maturation](http://analytics.informationweek.com/abstract/5/5116/Cloud-Computing/research-2011-state-of-cloud.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [Research: Federal Government Cloud Computing
    Survey](http://analytics.informationweek.com/abstract/13/6134/Outsourcing-Services/research-federal-government-cloud-computing-survey.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [SaaS 2011: Adoption Soars, Yet Deployment Concerns
    Linger](http://analytics.informationweek.com/abstract/5/5674/Cloud-Computing/research-saas-2011.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [Research: State of the IT Service
    Desk](http://analytics.informationweek.com/abstract/21/7594/Security/research-state-of-the-it-service-desk.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)
-   [Database
    Defenses](http://analytics.informationweek.com/abstract/21/7616/Security/database-defenses.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)

[More
\>\>](/analytics/architecture_and_design/more.html?cid=SBX_ddj_fture_Analytics_default_architecture_and_design&itc=SBX_ddj_fture_Analytics_default_architecture_and_design)

![image](http://twimgs.com/informationweek/smartbox/images/smartbox.PNG)

\

Featured Whitepapers
--------------------

[What's
this?](# "UBM Smart boxes auto deliver premium content that is contextually relevant to the article or site section where it is located")

-   [IBM System Storage Interactive Product Guide: Intelligent,
    efficient and automated storage for your IT
    infrastructure](http://www.informationweek.com/whitepaper/Storage/Storage-Systems/ibm-system-storage-interactive-product-guide-inte-wp1390835549?articleID=191740771&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [Crossing the Sustainability Chasm: Strategies and Tactics to
    Achieve Sustainability
    Goals](http://www.informationweek.com/whitepaper/Storage/Virtualization/crossing-the-sustainability-chasm-strategies-and-wp1338649925?articleID=191704919&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [How Insurers Can Effectively Meet the Challenges of the
    ACA](http://www.insurancetech.com/whitepaper/Regulation-Financial-Systems/Compliance/how-insurers-can-effectively-meet-the-challenges-o-wp1390839773?articleID=191740763&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
-   [InfoSphere BigInsights for Hadoop Quick Start
    Edition](http://www.informationweek.com/whitepaper/Software/Databases/infosphere-biginsights-for-hadoop-quick-start-edit-wp1390546011?articleID=191740757&cid=SBX_ddj_fture_wp_default_architecture_and_design&itc=SBX_ddj_fture_wp_default_architecture_and_design)
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
