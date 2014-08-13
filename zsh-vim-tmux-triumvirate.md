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

The Text Triumvirate
--------------------

April 18, 2012\
 [zsh](/tag/zsh/) [vim](/tag/vim/) [unix](/tag/unix/)

### The Triumviri

In 62 BC, [Caesar](http://en.wikipedia.org/wiki/Julius_Caesar) united a
political alliance between himself, the statesman
[Crassus](http://en.wikipedia.org/wiki/Marcus_Licinius_Crassus), and the
military general [Pompey](http://en.wikipedia.org/wiki/Pompey).
Together, the three men formed a secret political faction called the
[Triumvirate](http://en.wikipedia.org/wiki/First_Triumvirate) that ruled
the [Roman Republic](http://en.wikipedia.org/wiki/Roman_Republic). The
Text Triumvirate is an alliance between the [zsh](http://www.zsh.org/),
[vim](http://www.vim.org/), and [tmux](http://tmux.sourceforge.net/).
Each of these venerable tools is extremely powerful in its own right;
however, together they are an unmatched productivity force that rules
all forms of text manipulation. This post aims to provide an overview of
how to create a highly functional and easy to configure Text Triumvirate
for those new to this tool chain. I try to focus on aspects of how to
integrate zsh, vim, and tmux with particular focus on my experiences
with two common problems—copy/paste functionality and color aesthetics.

[Opinions](#opinions)

[Aesthetics](#aesthetics)

[Setup](#setup)

[zsh](#zsh)

[vim](#vim)

[tmux](#tmux)

[Plug-ins](#plugins)

### Opinions

[Just like
Rands](http://www.randsinrepose.com/archives/2009/11/02/the_foamy_rules_for_rabid_tools.html),
I’m rabid foaming at the mouth crazy about my tools. I think the Text
Triumvirate is the most powerful tool chain available for text
manipulation. If you don’t use this tool chain, I would encourage you to
drink the Kool-Aid and try the Text Triumvirate. I think you owe it to
yourself if you spend hours each day wrangling text. At first it may be
awkward to switch tools, but your diligence will be rewarded. The
benefits to using zsh, vim, and tmux is that they are free, fast,
endlessly customizable, work on any operating system, function in remote
environments, are capable of remote [pair
programming](http://en.wikipedia.org/wiki/Pair_programming), and are
deeply integrated with one another and all of Unix. The net result is
greater efficiency and organization with all things text. The tool chain
can be managed entirely by [git](http://git-scm.com/) and cloned onto a
remote server or a new machine within a few seconds. Together, these
advantages have made me a faster and more productive writer and coder.

One of the great advantages to the Text Triumvirate is the ubiquitous
use of the split model for managing working environments. Split model
management allows tmux to act as glue to organize work flows. I find by
the end of a day I usually have several shell windows and a huge number
of scratch files, data files, source code files, documentation files,
and databases open. It’s a huge pain to close each window only to come
back the next day to open all the same files again. tmux and vim allows
dozens of panes and windows to be open around a particular project and
if you wish to switch to working on an entirely different project you
can detach from those windows, go to another project, and then reattach
to your first project in the same state you left it. I generally have
multiple projects for both work and personal that I’m working on at a
given time. The ability to attach and detach for working environments is
essential.^[1](#fn:1 "see footnote")^

Where we are going—zsh and vim wrapped in tmux [awesome
sauce](http://www.urbandictionary.com/define.php?term=awesome+sauce):

[![tmux\_screen](http://farm8.staticflickr.com/7096/7090273507_293daf55c0_b.jpg)](http://www.flickr.com/photos/74146045@N06/7090273507/ "tmux_screen by dr.bunsen, on Flickr")

The tmux session above has three windows named demo, docs, and scatch,
however only the top most demo window is visable in the screenshot. In
this window there are four splits. The top left split is a Z-shell
window, the bottom left window has an interactive python session, the
top right window is running vim with python code, and the bottom right
window contains markdown documentation.

### Aesthetics

I suggest setting up the Text Triumvirate with two color themes—one
theme for work projects and another theme for personal projects. I make
heavy use of [context-dependent
memory](http://en.wikipedia.org/wiki/Context-dependent_memory), so using
two themes helps me a great deal with cognition and demarcating work
projects from personal projects. Below is what my personal theme (left)
and work theme (right) look like. Both themes are versions of Ethan
Schoonover’s [solarized](http://ethanschoonover.com/solarized) project.
I use the dark theme for play because I usually work on my own projects
in the morning or evening when it’s dark outside. A dark theme is much
easier on my eyes at these times. For fonts, I used 14 point
[Inconsolata](http://levien.com/type/myfonts/inconsolata.html).^[2](#fn:2 "see footnote")^

[![zsh](http://farm8.staticflickr.com/7205/7080277925_db8735899d_b.jpg)](http://www.flickr.com/photos/74146045@N06/7080277925/ "zsh by dr.bunsen, on Flickr")

### Setup

The first thing to do is [remap the Capslock
key](http://www.drbunsen.org/remap-capslock.html) to the Control key.
The Capslock is a [vestige from the past](http://capsoff.org/history)
and it’s important to make better usage of the valuable keyboard real
estate. tmux in particular makes heavy usage of the Control key, so it’s
helpful to remap Control to a more ergonomic position.

To generate a respectable working environment for the Triumvirate,
download the [iTerm2](http://www.iterm2.com/) terminal emulator. iTerm2
has a number of performance enhancements, features, and customizations
over the stock Terminal.app. Once you start using iTerm2, go back and
read the [full docs](http://www.iterm2.com/#/section/documentation) to
see its capabilities. One useful feature is `Command-?`, which overlays
a nice visual for finding your current curson position quickly. Most of
iTerm2’s really cool features are outside the scope of this post. Make
sure you check out iTerm2’s instant replay, regex search, click to open
URLs, and jump to mark features.

Once iTerm2 is installed, add both light and dark solarized themes. The
solarized repo has iTerm2 color palettes and
[instructions](https://github.com/altercation/solarized/tree/master/iterm2-colors-solarized)
for configuring the themes in iTerm2, so it’s straight-forward to setup.
Another helpful iTerm2 configuration is to enable a system-wide
key-binding that bring iTerm2 forward to the front most window. I find
it faster to setup an explicit binding rather than using the Application
Switcher to toggle through applications. This shortcut is set in
`Preferences > Keys` and I use `Option-t` as my binding. The other
customization I would suggest is to uncheck the iTerm2 bell sound under
`Profiles > Terminal > Notifications`.

Since the Text Triumvirate is highly keyboard-centric, it is prudent to
map out a sane key-binding strategy for iTerm, zsh, vim, tmux, and any
other tools you use before configuring and committing your own bindings
to muscle memory. For window movements, I use the Option key. `Option-t`
brings iTerm2 to the front, `Option-i` brings Twitter to the front, etc.
I also use [Moom](http://manytricks.com/moom/) as my tiling manager on
OS X with all shortcuts configured with the Option key to send windows
to specific displays or destinations on the
screen.^[3](#fn:3 "see footnote")^

Next up, install [Homebrew](http://mxcl.github.com/homebrew/) and then
use Homebrew to install git, MacVim, tmux, and
reattach-to-user-namespace. The purpose of installing MacVim is twofold.
For one, the default vim that ships with OS X seems slow for a lot of
people. I’ve found using the vim distribution that ships with MacVim to
be much faster than the OS X version. The added advantage of installing
MacVim is you will get a newer version of vim on your system. Secondly,
copy/paste do not work optimally with the version of vim that ships with
OS X.

Once git is installed, initialize a new repo for storing the Text
Triumvirate config files. My repo is called *dotfiles* and contains all
my configurations for zsh, vim, and tmux. Read [Pro
Git](http://progit.org/book/) or [Git
Immersion](http://gitimmersion.com/) if you don’t know how to setup
version control for your files.

### zsh

Many great posts have been written about how to use zsh and why zsh is
superior to bash.^[4](#fn:4 "see footnote")^ zsh basically has all of
the functionality of bash as well as quite a few additional features. I
use zsh over bash because it has extended globbing, superior tab
completion, built-in spell correction, a better calculator (zcalc), and
a built-in batch file renaming tool (zmv). The other killer zsh feature
is [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)—a community
driven framework for zsh. oh-my-zsh comes prepackaged with very nice
themes, plugins, and customizations that make zsh extremely
powerful.^[5](#fn:5 "see footnote")^ If you take away nothing else from
this post install iTerm2 and use zsh as your default shell.

I store my .zshrc, .vimrc, and .tmux.conf configuration files within my
dotfiles directory and then symlink them to my home directory. This
approach allows zsh, vim, and tmux customizations to be maintained all
in one directory under version control. Since the Text Triumvirate uses
vim, it makes sense to setup zsh and tmux to also use vim and vim key
bindings and use vim as the default editor. Add these lines to the
.zshrc file for vim support in zsh:

    export EDITOR="vim"
    bindkey -v 

    # vi style incremental search
    bindkey '^R' history-incremental-search-backward
    bindkey '^S' history-incremental-search-forward
    bindkey '^P' history-search-backward
    bindkey '^N' history-search-forward  

While zsh supports most bash commands, it also supports a more
intelligent collection of commands. For example, if you want to move
inside a directory in bash you would type `cd foo`. In zsh you can just
type `foo` if you add this line to your .zshrc:

    setopt AUTO_CD

To setup a nice prompt, I used [Steve Losh’s excellent
prompt](http://stevelosh.com/blog/2010/02/my-extravagant-zsh-prompt/) as
a guide and then made a few minor modifications. Simply create a new
theme file within `oh-my-zsh/themes/` and add a line to your zshrc file
referencing the name of your theme (`ZSH_THEME=bunsen`). Here is my
version of Steve’s prompt:

    function virtualenv_info {
        [ $VIRTUAL_ENV ] && echo '('`basename $VIRTUAL_ENV`') '
    }

    function box_name {
        [ -f ~/.box-name ] && cat ~/.box-name || hostname -s
    }

    PROMPT='
    %{$fg[magenta]%}%n%{$reset_color%} at %{$fg[yellow]%}$(box_name)%{$reset_color%} in %{$
    fg_bold[green]%}${PWD/#$HOME/~}%{$reset_color%}$(git_prompt_info)
    $(virtualenv_info)%(?,,%{${fg_bold[blue]}%}[%?]%{$reset_color%} )$ '

    ZSH_THEME_GIT_PROMPT_PREFIX=" on %{$fg[magenta]%}"
    ZSH_THEME_GIT_PROMPT_SUFFIX="%{$reset_color%}"
    ZSH_THEME_GIT_PROMPT_DIRTY="%{$fg[green]%}!"
    ZSH_THEME_GIT_PROMPT_UNTRACKED="%{$fg[green]%}?"
    ZSH_THEME_GIT_PROMPT_CLEAN=""

    local return_status="%{$fg[red]%}%(?..⤬)%{$reset_color%}"
    RPROMPT='${return_status}%{$reset_color%}'

### vim

I’m going to focus specifically on aspects of integrating vim with the
Text Triumvirate rather than vim itself. To get solarized integration
with vim, install the [vim solarized
plugin](https://github.com/altercation/vim-colors-solarized) and then
append these lines to your vimrc:

    syntax enable
    let g:solarized_termtrans = 1
    colorscheme solarized
    togglebg#map("<F5>") 

Color management in the terminal can be tricky. On my system, I had to
explicitly add `let g:solarized_termtrans = 1` to get the proper color
rendering in terminal vim. Solarized provides a built in background
function to toggle light and dark themes using the `<F5>` function, so
if you want this functionality add the last line. Inside vim you can
also run `:set background=dark` or `:set background=light` to achieve
the same functionality.

vim handles copy/paste somewhat differently than GUI-based text editors.
Instead of a single copy/paste system, vim has numerous copy registers
and a couple paste modes. I’ve added the following lines to my vimrc to
make copy/paste with the system more intuitive.

    " Yank text to the OS X clipboard
    noremap <leader>y "*y
    noremap <leader>yy "*Y

    " Preserve indentation while pasting text from the OS X clipboard
    noremap <leader>p :set paste<CR>:put  *<CR>:set nopaste<CR>

The above mappings make using the OS X system clipboard much more
accessible. The first two commands yank selected text or a line into the
system clipboard, respectively. The last line maintains the formatting
of text pasted into vim. In practive I don’t find that I paste a great
deal of text in and out of vim. If I need to share code I usually use
the [vim gist plugin](https://github.com/mattn/gist-vim), which is
faster than copy/paste.

### tmux

tmux is the glue that holds the Text Triumvirate together. I’ve only
started using tmux within the last month, but I’m amazed at how
indespensible it now is to my workflow. Here is the wikipedia
description of tmux:

> tmux is a software application that can be used to multiplex several
> virtual consoles, allowing a user to access multiple separate terminal
> sessions inside a single terminal window or remote terminal session.
> It is useful for dealing with multiple programs from a command line
> interface, and for separating programs from the Unix shell that
> started the program.

Essentially, tmux allows you to create *sessions*, which you can attach
and detach from whenever you like. tmux is invaluable because you can
organize your work contextually.

Just like vim, the most difficult aspects of setting up and using tmux
is color management and copy/paste functionality with the system
clipboard. It’s straight forward to generate the proper solarized colors
by making sure tmux knows you are using 256 colors. Add this line to
your tmux.conf file:

    set -g default-terminal "screen-256color"

For copy/paste, tmux has a special copy mode. Copy mode tmux commands
start with a prefix key. By default the tmux prefix key is `Control-b`.
Most people, myself included, remap the prefix to `Control-a` because
it’s much easier to touch type and it is also the default binding of GNU
screen. When you see me refer to `prefix` below, it means `Control-a`.
So `<prefix> c` means: type `Control-a` and then `c`.

Copy/paste inside tmux is completely broken on OS X. Fortunately, Chris
Johnsen created a nice patch called
[reattach-to-user-namespace](https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard)
that is easy to install via Homebrew. The people at
[Thoughtbot](http://thoughtbot.com/) have a number of helpful blog posts
explaining how to use tmux and how to get copy/paste functionality
working (see
[here](http://robots.thoughtbot.com/post/2641409235/a-tmux-crash-course)
and
[here](http://robots.thoughtbot.com/post/2166174647/love-hate-tmux)).
Even with these instructions I didn’t initially grasp how to use tmux
with the OS X clipboard system, so here’s what you need add to your
tmux.conf file after reattach-to-user-namespace is installed:

    set -g default-command "reattach-to-user-namespace -l zsh"

    set -g mode-mouse on
    setw -g mouse-select-window on
    setw -g mouse-select-pane on

    # Copy mode
    setw -g mode-keys vi
    bind ` copy-mode
    unbind [
    unbind p
    bind p paste-buffer
    bind -t vi-copy v begin-selection
    bind -t vi-copy y copy-selection
    bind -t vi-copy Escape cancel
    bind y run "tmux save-buffer - | reattach-to-user-namespace pbcopy"

The first line configures tmux to use the wrapper program to start zsh
for each new tmux window that is opened. The next three lines are my
personal preferences for mouse handling inside tmux. You can keep or
discard these lines depending on your preferences. The real meat and
potatoes are the next ten lines that deal with copy mode.

tmux has it’s own copy/paste buffers in addition to the vim copy/paste
buffers, and OS X copy/paste. To work efficiently with tmux buffers,
enter copy mode with ``  ` ``. I've remapped the default copy bindings
to use the analgous vi bindings. To place text into a tmux copy/paste
buffer, enter copy mode and select the text to copy using `v` to make a
selection and then `y` to yank the selection. At this point, the text is
in a tmux copy/paste buffer. Running `<prefix> p` will paste the text.
However, if you want the text in the OS X copy/paste buffer, run
`<prefix> y`.

### Plug-ins

I would be remiss if I didn't mentions some of the incredible open
source projects that integrate especially well the Text Triumvirate.
Rather than explaining each tool in-depth, here are links to some of my
favorite projects with abridged descriptions:\

-   [Ack](http://betterthangrep.com/)—better than grep\
-   [Autojump](https://github.com/joelthelion/autojump/wiki/)—directory
    navigation
-   [Command-t](https://wincent.com/blog/tweaking-command-t-and-vim-for-use-in-the-terminal-and-tmux)—vim
    plugin for fuzzy search; (link to setting up with tmux)
-   [Formd](http://www.drbunsen.org/formd-a-markdown-formatting-tool.html)—my
    own markdown link formatting tool for prose
-   [Pandoc](http://johnmacfarlane.net/pandoc/)—markup format conversion
-   [Poweline-vim](https://github.com/Lokaltog/vim-powerline)—customized
    vim statusbar
-   [Pianobar](http://6xq.net/projects/pianobar/)—terminal Pandora music
    player
-   [pdfgrep](http://pdfgrep.sourceforge.net/)—grep for PDF files
-   [shelr](https://github.com/antono/shelr)—screen recording in the
    shell
-   [vimux](https://github.com/benmills/vimux)—interact with tmux from
    vim
-   [virtualenv](https://github.com/pypa/virtualenv)—Python virtual
    environment builder
-   [wemux](https://github.com/zolrath/wemux)—multi-user multiplexing
-   [yadr](http://skwp.github.com/dotfiles/)—an opinionated zsh, MacVim,
    and git configuration

### Update

Several people have contacted me asking how to setup tmux with the
attractive status bar I displayed in the above screenshot. I learned how
to do this from the [wemux project](https://github.com/zolrath/wemux).
Assuming you have installed
[vim-powerline](https://github.com/Lokaltog/vim-powerline) and are using
patched fonts, you can simply append the following lines your your
`.tmux.conf` file to get my status bar style. Thanks [Matt
Furden](https://github.com/zolrath)!

    set -g status-left-length 52
    set -g status-right-length 451
    set -g status-fg white
    set -g status-bg colour234
    set -g window-status-activity-attr bold
    set -g pane-border-fg colour245
    set -g pane-active-border-fg colour39
    set -g message-fg colour16
    set -g message-bg colour221
    set -g message-attr bold
    set -g status-left '#[fg=colour235,bg=colour252,bold] ❐ #S
    #[fg=colour252,bg=colour238,nobold]⮀#[fg=colour245,bg=colour238,bold] #(whoami)
    #[fg=colour238,bg=colour234,nobold]⮀'
    set -g window-status-format "#[fg=white,bg=colour234] #I #W "
    set -g window-status-current-format
    "#[fg=colour234,bg=colour39]⮀#[fg=colour25,bg=colour39,noreverse,bold] #I ⮁ #W
    #[fg=colour39,bg=colour234,nobold]⮀"

* * * * *

1.  The [Thoughtbot
    blog](http://robots.thoughtbot.com/post/2641409235/a-tmux-crash-course)
    has a nice overview of how windows and splits/panes work in
    tmux.[↩](#fnref:1 "return to article")

2.  Dan Benjamin has a [useful
    post](http://hivelogic.com/articles/top-10-programming-fonts/)
    highlighting a number of great monospace
    fonts.[↩](#fnref:2 "return to article")

3.  For tiling hotkeys I follow a [Cartesian coordinate
    system](http://en.wikipedia.org/wiki/Cartesian_coordinate_system)
    representing the four quadrants. Option-1—upper-right quadrant,
    Option-2—upper-left, etc. Moom also has excellent multi-display
    capabilities and supports sending windows to other
    displays.[↩](#fnref:3 "return to article")

4.  A few phenomonal Z-shell resouces include: [My Extravagant Zsh
    Prompt](http://stevelosh.com/blog/2010/02/my-extravagant-zsh-prompt/),
    [zsh-lovers](http://grml.org/zsh/zsh-lovers.html), [Bash in minds
    with
    zsh](http://intridea.com/blog/2011/5/18/its-not-enough-to-bash-in-heads-youve-got-to-bash-in-minds-with-zsh),
    and [Zzappers best of zsh
    tips.](http://www.zzapper.co.uk/zshtips.html)[↩](#fnref:4 "return to article")

5.  Some people find that Robby Russell's oh-my-zsh slows down zsh. I've
    never found it to be a problem, but if you do, consider using
    [Sorin's oh-my-zsh
    fork](https://github.com/sorin-ionescu/oh-my-zsh).[↩](#fnref:5 "return to article")

Powered by [Flask](http://flask.pocoo.org/). Hosted on
[S3](http://aws.amazon.com/s3/). Created by [Seth Brown](/about/).
Licensed under [CC-BY](http://creativecommons.org/licenses/by/3.0/).
