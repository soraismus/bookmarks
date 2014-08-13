[skip to main](#main) | [skip to sidebar](#sidebar)

Monday, August 22, 2011
-----------------------

### Regular expressions in lexing and parsing

Comments extracted from a code review. I've been asked to disseminate
them more widely.

\

I should say something about regular expressions in lexing and \
parsing. Regular expressions are hard to write, hard to write well, \
and can be expensive relative to other technologies. (Even when they \
are implemented correctly in N\*M time, they have significant \
overheads, especially if they must capture the output.) \
 \
Lexers, on the other hand, are fairly easy to write correctly (if not \
as compactly), and very easy to test. Consider finding alphanumeric \
identifiers. It's not too hard to write the regexp (something like \
"[a-ZA-Z\_][a-ZA-Z\_0-9]\*"), but really not much harder to write as a \
simple loop. The performance of the loop, though, will be much higher \
and will involve much less code under the covers. A regular expression \
library is a big thing. Using one to parse identifiers is like using a \
Ferrari to go to the store for milk. \
 \
And when we want to adjust our lexer to admit other character types, \
such as Unicode identifiers, and handle normalization, and so on, the \
hand-written loop can cope easily but the regexp approach will break \
down. \
 \
A similar argument applies to parsing. Using regular expressions to \
explore the parse state to find the way forward is expensive, \
overkill, and error-prone. Standard lexing and parsing techniques are \
so easy to write, so general, and so adaptable there's no reason to \
use regular expressions. They also result in much faster, safer, and \
compact implementations.

\

Another way to look at it is that lexers and parsing are matching \
statically-defined patterns, but regular expressions' strength is that \
they provide a way to express patterns dynamically. They're great in \
text editors and search tools, but when you know at compile time what \
all the things are you're looking for, regular expressions bring far \
more generality and flexibility than you need. \
 \
Finally, on the point about writing well. Regular expressions are, in \
my experience, widely misunderstood and abused. When I do code reviews \
involving regular expressions, I fix up a far higher fraction of the \
regular expressions in the code than I do regular statements. This is \
a sign of misuse: most programmers (no finger pointing here, just \
observing a generality) simply don't know what they are or how to use \
them correctly. \
 \
Encouraging regular expressions as a panacea for all text processing \
problems is not only lazy and poor engineering, it also reinforces \
their use by people who shouldn't be using them at all. \
 \
So don't write lexers and parsers with regular expressions as the \
starting point. Your code will be faster, cleaner, and much easier to \
understand and to maintain.

Posted by
[rob](http://www.blogger.com/profile/18259238879445421354 "author profile")
at [7:36
PM](http://commandcenter.blogspot.com/2011/08/regular-expressions-in-lexing-and.html "permanent link")

[Newer
Post](http://commandcenter.blogspot.com/2011/09/we-open-in-well-lit-corporate.html "Newer Post")
[Older
Post](http://commandcenter.blogspot.com/2010/08/know-your-science.html "Older Post")
[Home](http://commandcenter.blogspot.com/)

About Me
--------

[![My
Photo](http://www.herpolhode.com/rob/doodles/june062002-1thumb.jpg)](http://www.blogger.com/profile/18259238879445421354)

 [rob](http://www.blogger.com/profile/18259238879445421354)
  ~ Usual noise at bottom of page

[View my complete
profile](http://www.blogger.com/profile/18259238879445421354)

[![image](http://img1.blogblog.com/img/icon18_wrench_allbkg.png)](//www.blogger.com/rearrange?blogID=6983287&widgetType=Profile&widgetId=Profile1&action=editWidget&sectionId=sidebar "Edit")

Links
-----

-   [Home page at Google](http://research.google.com/people/r/)
-   [Renee French](http://reneefrench.blogspot.com/)
-   [Rob Pike](http://robpike.blogspot.com/)

[![image](http://img1.blogblog.com/img/icon18_wrench_allbkg.png)](//www.blogger.com/rearrange?blogID=6983287&widgetType=LinkList&widgetId=LinkList1&action=editWidget&sectionId=sidebar "Edit")

Blog Archive
------------

-   [►](javascript:void(0))
    [2014](http://commandcenter.blogspot.com/search?updated-min=2014-01-01T00:00:00-08:00&updated-max=2015-01-01T00:00:00-08:00&max-results=1)
    (1)
    -   [►](javascript:void(0))
        [January](http://commandcenter.blogspot.com/2014_01_01_archive.html)
        (1)

-   [►](javascript:void(0))
    [2013](http://commandcenter.blogspot.com/search?updated-min=2013-01-01T00:00:00-08:00&updated-max=2014-01-01T00:00:00-08:00&max-results=1)
    (1)
    -   [►](javascript:void(0))
        [May](http://commandcenter.blogspot.com/2013_05_01_archive.html)
        (1)

-   [►](javascript:void(0))
    [2012](http://commandcenter.blogspot.com/search?updated-min=2012-01-01T00:00:00-08:00&updated-max=2013-01-01T00:00:00-08:00&max-results=3)
    (3)
    -   [►](javascript:void(0))
        [September](http://commandcenter.blogspot.com/2012_09_01_archive.html)
        (1)

    -   [►](javascript:void(0))
        [June](http://commandcenter.blogspot.com/2012_06_01_archive.html)
        (1)

    -   [►](javascript:void(0))
        [April](http://commandcenter.blogspot.com/2012_04_01_archive.html)
        (1)

-   [▼](javascript:void(0))
    [2011](http://commandcenter.blogspot.com/search?updated-min=2011-01-01T00:00:00-08:00&updated-max=2012-01-01T00:00:00-08:00&max-results=3)
    (3)
    -   [►](javascript:void(0))
        [December](http://commandcenter.blogspot.com/2011_12_01_archive.html)
        (1)

    -   [►](javascript:void(0))
        [September](http://commandcenter.blogspot.com/2011_09_01_archive.html)
        (1)

    -   [▼](javascript:void(0))
        [August](http://commandcenter.blogspot.com/2011_08_01_archive.html)
        (1)
        -   [Regular expressions in lexing and
            parsing](http://commandcenter.blogspot.com/2011/08/regular-expressions-in-lexing-and.html)

-   [►](javascript:void(0))
    [2010](http://commandcenter.blogspot.com/search?updated-min=2010-01-01T00:00:00-08:00&updated-max=2011-01-01T00:00:00-08:00&max-results=1)
    (1)
    -   [►](javascript:void(0))
        [August](http://commandcenter.blogspot.com/2010_08_01_archive.html)
        (1)

-   [►](javascript:void(0))
    [2008](http://commandcenter.blogspot.com/search?updated-min=2008-01-01T00:00:00-08:00&updated-max=2009-01-01T00:00:00-08:00&max-results=2)
    (2)
    -   [►](javascript:void(0))
        [April](http://commandcenter.blogspot.com/2008_04_01_archive.html)
        (2)

-   [►](javascript:void(0))
    [2006](http://commandcenter.blogspot.com/search?updated-min=2006-01-01T00:00:00-08:00&updated-max=2007-01-01T00:00:00-08:00&max-results=2)
    (2)
    -   [►](javascript:void(0))
        [June](http://commandcenter.blogspot.com/2006_06_01_archive.html)
        (1)

    -   [►](javascript:void(0))
        [April](http://commandcenter.blogspot.com/2006_04_01_archive.html)
        (1)

-   [►](javascript:void(0))
    [2004](http://commandcenter.blogspot.com/search?updated-min=2004-01-01T00:00:00-08:00&updated-max=2005-01-01T00:00:00-08:00&max-results=2)
    (2)
    -   [►](javascript:void(0))
        [June](http://commandcenter.blogspot.com/2004_06_01_archive.html)
        (1)

    -   [►](javascript:void(0))
        [May](http://commandcenter.blogspot.com/2004_05_01_archive.html)
        (1)

[![image](http://img1.blogblog.com/img/icon18_wrench_allbkg.png)](//www.blogger.com/rearrange?blogID=6983287&widgetType=BlogArchive&widgetId=BlogArchive1&action=editWidget&sectionId=sidebar "Edit")

Followers
---------

[![image](http://img1.blogblog.com/img/icon18_wrench_allbkg.png)](//www.blogger.com/rearrange?blogID=6983287&widgetType=Followers&widgetId=Followers1&action=editWidget&sectionId=sidebar "Edit")
