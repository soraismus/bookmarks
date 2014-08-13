[Dr. Bunsen](/ "Dr. Bunsen.")
=============================

![image](/static/bunsen-thumb.svg)

created by [Seth Brown](http://www.drbunsen.org/about/)

-   [Archives](/archives/)
-   [Projects](https://github.com/drbunsen)
-   [About](/about/)
-   [Contact](mailto:sethbrown@drbunsen.org)
-   [Twitter](https://twitter.com/intent/user?screen_name=DrBunsen)
-   [Feed](/feed.xml "Atom feed")
-   [Search](http://nerdquery.com/?catid=19&category=18&setcss1=)

Vim Croquet
-----------

January 27, 2014\
 [vim](/tag/vim/) [visualization](/tag/visualization/)
[haskell](/tag/haskell/) [python](/tag/python/)
[javascript](/tag/javascript/) [unix](/tag/unix/) [data](/tag/data/)
[workflow](/tag/workflow/)

### Introduction

I recently discovered an interesting game called
[VimGolf](http://vimgolf.com). The objective of the game is to transform
a snippet of text from one form to another in as few keystrokes as
possible. As I was playing around with different puzzles on the site, I
started to get curious about my text editing habits. I wanted to better
understand how I manipulated text with vim and to see if I could
identify any inefficiencies in my workflow. I spend a huge amount of
time inside my text editor, so correcting even slight areas of friction
can result in worthwhile productivity gains. This post explains my
analysis and how I reduced the number of keystrokes I use in vim. I call
this game Vim Croquet.

-   [Data Acquisition](#data_acquisition)
-   [Key Heat Map](#key_heat_map)
-   [Command Frequency](#command_frequency)
-   [Command Complexity](#command_complexity)
-   [Conclusions](#conclusions)

### Data Acquisition

I started my analysis by collecting data. All my text editing on a
computer is done with vim, so for 45 days I logged every keystroke I
used in vim with the `scriptout` flag. For convenience, I aliased vim in
my shell to record all my keystrokes into a log file:

    alias vim='vim -w ~/.vimlog "$@"'

Next, I needed to parse the resulting data. Parsing vim is complicated.
vim is a modal editor where a single command can have different meanings
in different modes. Commands can also have contextual effects where the
behavior of certain actions can be different depending on where they are
executed within a buffer. For example, typing `cib` in normal mode moves
the user into insert mode if the command is executed between
parentheses, but leaves the user in normal mode if executed outside of
parentheses. If `cib` is executed in insert mode it has an altogether
different behavior; it writes the characters `cib` into the current
buffer.

I looked at several candidate tools for parsing vim commands including
industrial parser libraries like [antler](http://www.antlr.org) and
[parsec](http://legacy.cs.uu.nl/daan/parsec.html) as well as a
vim-specific project called
[vimprint](https://github.com/nelstrom/vimprint). After some
deliberation, I decided to write my own tool. I don’t do a lot of
language processing, so investing the time to learn a sophisticated
parser seemed unwarranted.

I wrote a crude lexer in Haskell to
[tokenize](http://en.wikipedia.org/wiki/Tokenization) the keystrokes I
collected into individual vim commands. My lexer uses
[monoids](http://en.wikipedia.org/wiki/Monoid) to extract normal mode
commands from my log for further analysis. Here’s the source code for
the lexer:

    import qualified Data.ByteString.Lazy.Char8 as LC
    import qualified Data.List as DL
    import qualified Data.List.Split as LS
    import Data.Monoid
    import System.IO

    main = hSetEncoding stdout utf8 >> 
           LC.getContents >>= mapM_ putStrLn . process

    process =   affixStrip 
              . startsWith 
              . splitOnMode
              . modeSub
              . capStrings 
              . split mark 
              . preprocess

    subs = appEndo . mconcat . map (Endo . sub)

    sub (s,r) lst@(x:xs)
        | s `DL.isPrefixOf` lst = sub'
        | otherwise = x:sub (s,r) xs
        where
            sub' = r ++ sub (s,r) (drop (length s) lst)
    sub (_,_) [] = []

    preprocess =   subs meta 
                 . DL.intercalate " "
                 . DL.words
                 . DL.unwords
                 . DL.lines 
                 . LC.unpack

    splitOnMode = DL.concat $ map (\el -> split mode el)

    startsWith = filter (\el -> mark `DL.isPrefixOf` el && el /= mark)

    modeSub = map (subs mtsl)

    split s r = filter (/= "") $ s `LS.splitOn` r

    affixStrip =   clean 
                 . concat 
                 . map (\el -> split mark el)

    capStrings = map (\el -> mark ++ el ++ mark)

    clean = filter (not . DL.isInfixOf "[M")

    (mark, mode, n) = ("-(*)-","-(!)-", "")
    meta = [("\"",n),("\\",n),("\195\130\194\128\195\131\194\189`",n),
            ("\194\128\195\189`",n),("\194\128kb\ESC",n), 
            ("\194\128kb",n),("[>0;95;c",n), ("[>0;95;0c",n),
            ("\ESC",mark),("\ETX",mark),("\r",mark)]
    mtsl = [(":",mode),("A",mode), ("a",mode), ("I",mode), ("i",mode),
            ("O",mode),("o",mode),("v", mode),("/",mode),("\ENQ","⌃e"),
            ("\DLE","⌃p"),("\NAK","⌃u"),("\EOT","⌃d"),("\ACK","⌃f"),
            ("\STX","⌃f"),("\EM","⌃y"),("\SI","⌃o"),("\SYN","⌃v"),
            ("\DC2","⌃r")]

Here’s a sample of the data in its unprocessed form and its structure
after lexing:

    cut -c 1-42 ~/.vimlog | tee >(cat -v;echo) | ./lexer
    `Mihere's some text^Cyyp$bimore ^C0~A.^C:w^M:q

    `M
    yyp$b
    0~

My lexer reads from stdin and sends processed normal mode commands to
stdout. In the above example pipe, I use a process substitution to print
a representation of the unprocessed data on the second line and the
resulting output of the lexer on subsequent lines. Each line in the
output of the lexer represents a grouping of normal mode commands
executed in sequence. The lexer correctly determined that I started in
normal mode by navigating to a specific buffer using the `` `M `` mark,
then typed `here's some text` in insert mode, then copy and pasted the
line and moved to the start of the last word on the line using `yyp$b`,
then entered additional text, and finally navigating to the start of the
line and capitalizing the first character using `0~`.

### Key Heat Map

After lexing my log data, I forked [Patrick
Wied’s](http://www.patrick-wied.at/) awesome
[heatmap-keyboard](http://www.patrick-wied.at/projects/heatmap-keyboard/)
project and added my own custom layout to read the output of my lexer.
Patrick’s project does not detect most meta-characters like escape,
control, and command, so it was necessary for me to write a data loader
in JavaScript and make some other modifications so the heatmap would
accurately depict key usage in vim. I translated
[metacharacters](http://en.wikipedia.org/wiki/Metacharacter) used in vim
to unicode representations and mapped these onto the keyboard. Here’s
what my key usage looked like based on $\\approx 500,000$ normal mode
keystrokes processed by my lexer. Increasing wavelengths denotes more
prevalent key usage:

![image](/static/images/posts/vim-keyboard.jpg)

A prominent feature of the heatmap is the prevalent usage of the control
key. I use control for numerous movement commands in vim. For example, I
use `⌃p` for [Control P](http://kien.github.io/ctrlp.vim/) and I cycle
forward and backward through open buffers with `⌃j` and `⌃k`,
respectively. Control is an efficient movement on [my Kinesis
Advantage](/an-affray-with-rsi/) because I remap it to left thumb
delete.

Another pattern in the heatmap that jumped out at me was my heavy use of
`⌃E` and `⌃Y`. I routinely use these commands to navigate up and down
through source code, but moving vertically with these commands is
inefficient. Each time one of these commands is executed, the cursor
only moves a few lines at a time. A more efficient pattern would be to
use larger vertical movements with `⌃U` and `⌃D`. These commands move
the cursor up or down a half screen at a time, respectively.

### Command Frequency

The heatmap gives a good overview of how I use individual keys, but I
also wanted to learn more about how I used different key sequences. I
sorted the lines in the output of my lexer by frequency to uncover my
most used normal commands using a simple one-liner:

    $ sort normal_cmds.txt | uniq -c | sort -nr | head -10 | \
        awk '{print NR,$0}' | column -t

    1   2542    j
    2   2188    k
    3   1927    jj
    4   1610    p
    5   1602    ⌃j
    6   1118    Y
    7   987     ⌃e
    8   977     zR
    9   812     P
    10  799     ⌃y

Seeing `zR` rank as my 8th most used sequence was unexpected. After
pondering this, I realized a huge inefficiency in my text editing. My
`.vimrc` is setup to automatically [fold
text](http://vimcasts.org/episodes/how-to-fold/). The problem with this
configuration is that I almost immediately unfold all folded text, so it
makes no sense for my vim configuration to use automatically fold text
by default. Therefore, I removed this setting so that I would no longer
need to repeatedly use the `zR` command.

### Command Complexity

Another optimization I wanted to looked at was normal mode command
complexity. I was curious to see if I could find any commands that I
routinely used which also required an excessive number of keystrokes to
execute. I wanted to find these commands so that I could create
shortcuts to speed up their excution. I used
[entropy](http://en.wikipedia.org/wiki/Information_theory#Entropy) as a
proxy to measure command complexity using a short script in Python:

    #!/usr/bin/env python
    import sys
    from codecs import getreader, getwriter
    from collections import Counter
    from operator import itemgetter
    from math import log, log1p

    sys.stdin = getreader('utf-8')(sys.stdin)
    sys.stdout = getwriter('utf-8')(sys.stdout)

    def H(vec, correct=True):
        """Calculate the Shannon Entropy of a vector
        """
        n = float(len(vec))
        c = Counter(vec)
        h = sum(((-freq / n) * log(freq / n, 2)) for freq in c.values())

        # impose a penality to correct for size
        if all([correct is True, n > 0]):
            h = h / log1p(n)

        return h

    def main():
        k = 1
        lines = (_.strip() for _ in sys.stdin)
        hs = ((st, H(list(st))) for st in lines)
        srt_hs = sorted(hs, key=itemgetter(1), reverse=True)
        for n, i in enumerate(srt_hs[:k], 1):
            fmt_st = u'{r}\t{s}\t{h:.4f}'.format(r=n, s=i[0], h=i[1])
            print fmt_st

    if __name__ == '__main__':
        main()

The entropy script reads from stdin and finds the normal mode command
with the highest entropy. I used the output of my lexer as input for my
entropy calculation:

    $ sort normal_cmds.txt | uniq -c | sort -nr | sed "s/^[ \t]*//" | \
        awk 'BEGIN{OFS="\t";}{if ($1>100) print $1,$2}' | \
        cut -f2 | ./entropy.py

    1 ggvG＄"zy 1.2516

In the command above, I first
[filtered](http://en.wikipedia.org/wiki/Filter_(higher-order_function))
all the normal mode commands that I executed more than 100 times. Then,
among this subset, I found the command with the highest entropy. This
analysis precipitated the command `ggvG$"zy`, which I executed 246 times
in 45 days. The command takes an unwieldy 11 keystrokes and yanks the
entire current buffer into the z register. I typically use this command
to move the contents of one buffer into another buffer. Since I use this
sequence so frequently, I added a short cut to my `.vimrc` to reduce the
number of keystrokes I need to execute:

    nnoremap <leader>ya ggvG$"zy

### Conclusions

My Vim Croquet match revealed three optimizations to decrease the number
of keystrokes I use in vim:

-   Use coarser navigation commands like `^U` and `^D` instead of `^E`
    and `^Y`
-   Prevent buffers from automatically folding text to obviate using
    `zR`
-   Create shortcuts for verbose commands that are frequently used like
    `ggvG$"zy`

These 3 simple changes have saved me thousands of superfluous keystrokes
each month.

The code snippets above are presented in isolation and may be difficult
to follow. To help clarify the steps in my analysis, here’s my Makefile,
which shows how the code presented in this post fits together:

    SHELL           := /bin/bash
    LOG             := ~/.vimlog
    CMDS            := normal_cmds.txt
    FRQS            := frequencies.txt
    ENTS            := entropy.txt
    LEXER_SRC       := lexer.hs
    LEXER_OBJS      := lexer.{o,hi}
    LEXER_BIN       := lexer
    H               := entropy.py
    UTF             := iconv -f iso-8859-1 -t utf-8

    .PRECIOUS: $(LOG)
    .PHONY: all entropy clean distclean

    all: $(LEXER_BIN) $(CMDS) $(FRQS) entropy

    $(LEXER_BIN): $(LEXER_SRC)
        ghc --make $^

    $(CMDS): $(LEXER_BIN)
        cat $(LOG) | $(UTF) | ./$^ > $@

    $(FRQS): $(H) $(LOG) $(CMDS)
        sort $(CMDS) | uniq -c | sort -nr | sed "s/^[ \t]*//" | \
          awk 'BEGIN{OFS="\t";}{if ($$1>100) print NR,$$1,$$2}' > $@

    entropy: $(H) $(FRQS)
        cut -f3 $(FRQS) | ./$(H)

    clean:
        @- $(RM) $(LEXER_OBJS) $(LEXER_BIN) $(CMDS) $(FRQS) $(ENTS)

    distclean: clean

Powered by [Flask](http://flask.pocoo.org/). Hosted on
[S3](http://aws.amazon.com/s3/). Created by [Seth Brown](/about/).
Licensed under [CC-BY](http://creativecommons.org/licenses/by/3.0/).
