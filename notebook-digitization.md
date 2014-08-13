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

Custom Notebooks
----------------

February 11, 2013\
 [workflow](/tag/workflow/) [markdown](/tag/markdown/)
[python](/tag/python/) [tex](/tag/tex/) [office](/tag/office/)
[writing](/tag/writing/)

### Introduction

Notebooks are an indispensable tool for all facets of my work. I draw,
diagram, and write to generate ideas, work through problems, and to
learn things. Keeping this analog information organized in notebooks
provides a log of how my projects develop and it allows me to revisit
nascent ideas at a later date. This post is about how I use notebooks.

-   [Specifications](#specifications)
-   [Customization](#customization)
-   [Digitization](#digitization)
-   [Printing](#printing)
-   [Code](#code)

### Specifications

I’ve been on a quest to find the perfect notebook for a long
time.^[1](#fn:1 "see footnote")^ Each notebook design I’ve tried has
fallen short in some capacity. My ideal notebook borrows the [accordion
binding](http://ep.yimg.com/ca/I/moleskine_2249_144721761) of the
[Moleskine Japenese
notebook](http://www.amazon.com/gp/product/8862933096/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=8862933096&linkCode=as2&tag=drbunsenblog-20"),
filled with [90g Clairefontaine
paper](http://www.exaclair.com/brands_clairefontaine.shtml), with
equivalent dimensions to the iPad, bound with the spine positioned on
the narrow edge at the top of the notebook, and printed with a subtle
isometric dot grid similar to some [Rhodia
notebooks](http://www.amazon.com/gp/product/B003UCL77U/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B003UCL77U&linkCode=as2&tag=drbunsenblog-20).
The dots needs to be subtle as to not interfere with free form drawing,
but still discernible for synthesizing [graph
drawings](http://en.wikipedia.org/wiki/Graph_drawing) or aligning words
and math.

The accordion style allows the pages to be easily scanned without
dismantling the notebook binding to lay each page flush on a scanner.
Since the notebook is essentially one giant piece of folded paper, an
idea that takes root on one page can flow seamlessly to the next page
without the unnecessary end of page interruption. My choice of notebook
paper is based on the instruments I use. I need paper that can handle a
variety of media like charcoal, pens, markers, and pencils.
Clairefontaine 90g is the best paper I’ve found—it’s smooth, but with
enough tooth to handle soft leads.^[2](#fn:2 "see footnote")^

Most of the tools I use for work are firmly seated in the digital world.
A physical notebook, in contrast, feels like an anachronism. A challenge
to [effective information processing](/information-processing/) is
merging disparate forms of content into a unified format. Morticing
these two worlds together is not trivial. Digitizing a notebook into a
format that allows drawing, math, and handwriting to be searched and
edited is difficult. I’ve tried digital notebook software and always
found it nowhere near as effective as analog notebooks. The two main
problems are input and output. Digital styluses have no where near the
precision, breath of capabilities, or speed of physical writing tools.
On the other side, digital software rarely allows exporting data in an
open standard format or in the same format as the import data. Another
problem I have is with the way I use notebooks. When writing on a page,
I like to use my left hand to mark other pages that contain content I am
reading or referencing. Flipping back and forth between reference pages
and the page I’m working on is something that is awkward to replicate
with digital notebooks.

### Customization

The best solution I’ve found for notebooks is to roll my own. [Here’s a
small example PDF](http://dl.dropbox.com/u/50123/drbunsen-notebook.pdf)
of my custom notebook. For the basic pages, I use a modified [Cornell
style](http://en.wikipedia.org/wiki/Cornell_Notes) format. I like to use
various notations in the left gutter, which I can reference in the
summary section at the bottom of the page, on other pages, or in the
table of contents. I write short summary information at the bottom of
the page along with some special symbol notation to the right of a QR
code. These symbols are intended to be used by
[Aperture’s](https://itunes.apple.com/us/app/aperture/id408981426?mt=12&partnerId=30&siteID=85FUliSlDIA)
automatic face recognition to auto-tag notebook pages when they are
scanned.^[3](#fn:3 "see footnote")^ At the bottom of the page, I also
place a Volume-Page index. The volume number is a reference to the
notebook and the page number is the page number of that volume. For
example, 12.74 is the 74th page of notebook \#12. This index notation
generates a unique reference for every page in my notebook collection.
The bottom left corner of each page looks like this:

![image](/static/images/posts/example_screenshot.png)

### Digitization

The contents of each notebook page are inextricably linked to digital
resources. For example, I often need to reference companion digital
media files, images, or web pages. The QR code located in the bottom
left corner of each page is my tether to the digital world. Each
notebook page is associated with a unique markdown file. The link to the
file is embedded in the QR code. If I’m writing on a notebook page, I
can use an iOS app like
[QRReader](https://itunes.apple.com/us/app/qr-reader-for-iphone/id368494609?mt=8)
to automatically opens the corresponding markdown file in [Nebulous
Notes](https://itunes.apple.com/app/nebulous-notes-for-dropbox/id375006422?ign-mpt=uo%3D5&partnerId=30&siteID=85FUliSlDIA).
Within this markdown file I store all the relevant digital data that
accompanies this notebook page.

When I print a notebook, I also prepopulate each markdown file with YAML
front matter and an image/link placeholder for the corresponding
notebook page. When I scan the notebook at a latter date, the
placeholder image/link automatically points to the scanned notebook
page. This method allows the markdown file to be the permanent record
for both the digital and analog information as it contains all the
supporting digital information along with the scanned notebook image.

I previously used [Gollum](https://github.com/github/gollum) to generate
a wiki from all my markdown notebook files, but I’ve since switched to
using [Jekyll](https://github.com/mojombo/jekyll). Since I use Jekyll to
generate this website, it’s a better abstraction to use Jekyll for my
notebook as well. Both my website and notes share the same CSS, mathjax,
and layout format. Using Jekyll allows me to link notes together, create
tags for each notebook page, keep everything version controlled with
git, share content on the web, and [use Vim to
write](http://www.drbunsen.org/writing-in-vim.html). Any stylistic
changes made to my site is automatically reflected in my notes as well.
A basic notebook page looks like this:

[![image](/static/images/posts/notebook_screenshot.png)](/static/images/posts/notebook_screenshot.png)

-   Note that the math is rendered with MathJax/JavaScript, so it
    doesn’t show up in the screen shot I took with
    [Paparazzi!](http://derailer.org/paparazzi/).

### Printing

Printing is possibly the hardest step of this entire process. It’s not
easy to find a company that can print custom notebooks. Most companies
are severely restricted in the dimensions and type of paper they use for
printing. The best solution I’ve found for creating notebooks is to
order my own paper and do the printing myself. I then go to a local
printing store and get them to cut and bind the notebook. I’ve yet to
find a printing company capable of generating a notebook with the
accordion binding, so I’m forced to use a conventional notebooks layout.
I use a spiral binding with black vinyl front and back covers.

### Code

For [most print
projects](http://www.drbunsen.org/creating-letters-with-tex-and-jinja2.html),
I use XeTeX with Python and [Jinja](http://jinja.pocoo.org/docs/), but I
elected to just use pure Python for this project. The output of this
code creates a TeX file which can then be compiled with XeTeX to
generate notebook PDFs for printing. Below I’ve provided a Makefile and
the main code for this project. See [my GitHub
account](https://github.com/drbunsen) for the full code used in this
project. Here’s what the Makefile and main code look like. For the QR
codes, I use
[python-qrcode](https://github.com/lincolnloop/python-qrcode), my
typesetting engine is [XeTex](http://en.wikipedia.org/wiki/XeTeX), and
my font is [Garamond](http://garamond.org/):

    SHELL      := /bin/bash

    NAME        := notebook
    MD_DIR      := md_dir
    IMG_DIR     := ../img_dir
    VOL         := 1
    PAGES       := 10
    SCRIPT      := $(NAME).py
    BACKGROUND  := $(NAME).eps
    NOTEBOOK    := $(NAME).pdf
    TEX         := $(NAME).tex
    PY_CMD      := python $(SCRIPT) $(MD_DIR) $(IMG_DIR) $(VOL) $(PAGES)
    TEX_CMD     := for i in {1..2}; do xelatex $(TEX); done
    CMD         := $(PY_CMD) && $(TEX_CMD)
    ACC         := $(NAME).log $(NAME).aux $(NAME).tex *.png

    .PHONY: all clean distclean

    all: $(NOTEBOOK) clean

    $(NOTEBOOK): $(SCRIPT)
        @echo Building notebook...
        mkdir -p $(MD_DIR) $(IMG_DIR)
        $(CMD)

    clean:
        @- $(RM) $(ACC)

    distclean: clean

    #!/usr/bin/env python
    # -*- encoding: utf-8 -*-
    """
    Author: Seth Brown
    Description: Notebook generation code
    """

    import os
    import sys
    import qrcode
    import datetime

    def ref_file(page_stamp, img_dir):
        """ Generate associated markdown files for the notebook.
        """
        date = datetime.datetime.today().strftime('%Y-%m-%d-').rstrip('-')
        img = ''.join((page_stamp, '.png'))
        img_fn = os.path.join(img_dir, img)
        fill = ('---', 'title: "{0}"'.format(page_stamp),
                'published: {0}'.format(date),
                'updated: {0}'.format(date),
                'tags: []',
                '---', '\n\n',
        '<a href="{0}"><img class=centered src="{0}" width="600" /></a>'.\
            format(img_fn))

        fill = '\n'.join(line for line in fill)

        return fill

    def support_files(date, page_stamp, md_dif, img_dir):
        fn = date + page_stamp + '.md'
        mkd_file = os.path.join(md_dir, fn)
        if not os.path.isfile(mkd_file):
            with open(mkd_file, mode='w') as outfile:
                fill = ref_file(page_stamp, img_dir)
                outfile.write(fill)

    def preface(vol_no):
        # the date the notebook was created
        est_date = datetime.date.today().strftime('%B %Y')
        preamble = (r'\documentclass{article}',
                    r'\usepackage[paperheight=9.5in,paperwidth=7.31in]{geometry}',
                    r'\pagestyle{empty}',
                    r'\usepackage{wallpaper}',
                    r'\usepackage{fontspec}',
                    r'\usepackage{background}',
                    r'\setromanfont{GaramondNo8}',
                    r'\SetBgScale{1}',
                    r'\SetBgAngle{0}',
                    r'\SetBgColor{black}',
                    r'\SetBgOpacity{1}',
                    r'\SetBgPosition{current page.south}',
                    r'\begin{document}',
                    r'\vspace*{3cm}',
                    r'\NoBgThispage',
                    r'\centering',
                    r'\begin{Huge}',
                    r'\textbf{\emph{Vol. ' + str(vol_no) + r'}}',
                    r'\end{Huge}\\',
                    r'\vspace{0.1cm}',
                    r'\textbf{\emph{' + est_date + r'}}\\',
                    r'\vspace{1cm}',
                    r'\begin{huge}',
                    r'Seth Brown',
                    r'\end{huge}\\',
                    r'\vspace{0.1cm}',
                    r'sethbrown@drbunsen.org \\',
                    r'\vspace{10cm}',
                    r'\newpage',
                    r'\section*{Table of Contents}',
                    r'\vspace{3cm}',
                    r'\centering',
                    '\n')

        return '\n'.join(i for i in preamble)

    def page_stamps(bookno, page_end=500):
        pages = xrange(0, page_end)
        stamps = (''.join((str(bookno), '-', str(page))) for page in pages)

        return [stamp for stamp in stamps]

    def toc_lines(stamps):
        dots = '.' * 100
        eol = r'\\'

        return ''.join('{0:<10}{1}{2}\n'.format(stamp, dots, eol)
                for stamp in stamps)

    def qr(date, page_stamp, md_dir):
        qr_handle = ''.join(('qr_', page_stamp, '.png'))
        fn = date + page_stamp + '.md'
        qr_path = os.path.join(md_dir, fn)
        img_link = ''.join(('nebulous://notebook/', qr_path))
        img = qrcode.make(img_link)
        img.save(qr_handle)

        return qr_handle

    def notepage(page_stamp, qr_handle):
        page = (r'\newpage',
                r'\mbox{}',
                r'\CenterWallPaper{1.02}{notebook.eps}',
                r'\SetBgVshift{0.5cm}',
                r'\SetBgContents{' + page_stamp + r'}',
                r'\LLCornerWallPaper{0.09}{' + qr_handle + '}')

        return '\n'.join(i for i in page)

    def end():
        return  '\n'.join((r'\newpage', '\n', r'\end{document}'))

    def main(md_dir, img_dir, vol, pages):
        vol = int(vol)
        pages = int(pages)
        date = datetime.datetime.today().strftime('%Y-%m-%d-')

        stamps = page_stamps(vol, pages)
        [support_files(date, stamp, md_dir, img_dir) for stamp in stamps]

        with open('notebook.tex', mode='w') as outfile:
            p = preface(vol)
            t = toc_lines(stamps)
            a = p + t
            outfile.write(a)
            for stamp in stamps:
                q = qr(date, stamp, md_dir)
                n = notepage(stamp, q)
                outfile.write(n)
            e = end()
            outfile.write(e)

    if __name__ == '__main__':
        (md_dir, img_dir, vol, pages) = sys.argv[1:5]
        main(md_dir, img_dir, vol, pages)

* * * * *

1.  No topic is safe from the ineffable Dr. Drang. See his thoughts on
    custom notebooks
    [here](http://www.leancrew.com/all-this/2008/09/my-circa-steno-notebook/%20)
    and
    [here.](http://www.leancrew.com/all-this/2011/05/new-circarolla-steno-notebook/)
    Ben Deaton has some [great
    ideas](http://jbdeaton.com/2012/circa-trumps-moleskine/) about
    notebook as well.[↩](#fnref:1 "return to article")

2.  Clairefontaine’s [Triomphe
    paper](http://www.amazon.com/gp/product/B008BIXRPI/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B008BIXRPI&linkCode=as2&tag=drbunsenblog-20)
    is also exceptional for use with pens. The paper is exceptionally
    smooth.[↩](#fnref:2 "return to article")

3.  This is a hack. Aperture works fairly well with faces, but not
    symbols. It can recognize symbols, but it needs a lot of training. I
    plan to use this feature more in the future when image recognition
    matures.[↩](#fnref:3 "return to article")

Powered by [Flask](http://flask.pocoo.org/). Hosted on
[S3](http://aws.amazon.com/s3/). Created by [Seth Brown](/about/).
Licensed under [CC-BY](http://creativecommons.org/licenses/by/3.0/).
