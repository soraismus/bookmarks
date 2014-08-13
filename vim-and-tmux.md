[![thoughtbot logo](/images/logo.svg "Giant Robots Smashing Into Other Giant Robots")](/)
-----------------------------------------------------------------------------------------

[GIANT ROBOTS SMASHING INTO OTHER GIANT ROBOTS](/)
--------------------------------------------------

### Written by [thoughtbot](http://thoughtbot.com)

[Derek Prior](https://twitter.com/derekprior)

[June 15, 2013](/seamlessly-navigate-vim-and-tmux-splits)

-   [vim](/tags/vim)
-   [tmux](/tags/tmux)

My tmux session for a given project typically has two horizontal tmux
panes with Vim occupying the top 80% of the screen and a shell running
below that occupying the remainder. For the most part I stay in Vim,
splitting often, and [moving amongst those splits with
ease](http://robots.thoughtbot.com/vim-splits-move-faster-and-more-naturally).

![tmux and Vim
splits](http://media.tumblr.com/1c2e99dfd815537bdbbbe933eca51f99/tumblr_inline_moe8bluR2C1qz4rgp.jpg)

Then I'll decide to pop down to the shell split for a moment and
confidently hit `<C-j>`… nothing. Hmph. Let's try again. `<C-j> `…
nothing. Oh, right, that's not a Vim split, it's a tmux split. `<C-a>j`.

I'm constantly stumbling over the disconnect between Vim and tmux
splits. There's a little mental hurdle I have to clear each time and I
fail at that more often than I'd care to admit. It's 2013; Shouldn't the
computer be able to figure out what I *meant* and just do that? As it
turns out, yes.

Vim and tmux, Together in Harmony
---------------------------------

[Mislav Marohnić](http://mislav.uniqpath.com/) devised a
[solution](https://gist.github.com/mislav/5189704) that allows for using
the same key strokes to switch between Vim and tmux splits. With this in
place, I can use `<C-h/j/k/l>` to seamlessly navigate all my splits, be
they from tmux or Vim.

Mislav's post has an installation script, but a few of us at thoughtbot
had trouble getting those exact scripts running successfully. We've been
able to simplify and generalize the solution some.

Installation
------------

To get the whole thing running, follow these two steps:

1.  Install the
    [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
    Vim plugin from [Chris Toomey](http://ctoomey.com). With
    [vundle](https://github.com/gmarik/vundle), that’s as simple as
    adding the following to your \~/.vimrc file:

        Bundle 'christoomey/vim-tmux-navigator'

2.  Add the following to your \`\~/.tmux.conf\` configuration file to
    conditionally bind the movement keys depending on the active
    command:

        # smart pane switching with awareness of vim splits
        bind -n C-h run "(tmux display-message -p '#{pane_current_command}' | grep -iq vim && tmux send-keys C-h) || tmux select-pane -L"
        bind -n C-j run "(tmux display-message -p '#{pane_current_command}' | grep -iq vim && tmux send-keys C-j) || tmux select-pane -D"
        bind -n C-k run "(tmux display-message -p '#{pane_current_command}' | grep -iq vim && tmux send-keys C-k) || tmux select-pane -U"
        bind -n C-l run "(tmux display-message -p '#{pane_current_command}' | grep -iq vim && tmux send-keys C-l) || tmux select-pane -R"
        bind -n C-\ run "(tmux display-message -p '#{pane_current_command}' | grep -iq vim && tmux send-keys 'C-\\') || tmux select-pane -l"

**Note:** An earlier version of this post included more steps and
dependencies. Thanks to readers [Chis Eskow](http://chriseskow.com/) and
[Christopher
Sexton](http://www.codeography.com/2013/06/19/navigating-vim-and-tmux-splits)
for helping simplify the setup.

What's next?
------------

If you found this useful, you might also enjoy:

-   [Navigating Ruby Files in
    Vim](https://learn.thoughtbot.com/products/21-navigating-ruby-files-with-vim)
-   [Running Specs from Vim Sent to tmux via
    Tslime](http://robots.thoughtbot.com/running-specs-from-vim-sent-to-tmux-via-tslime)
-   [Use RSpec.vim with tmux and
    Dispatch](http://robots.thoughtbot.com/use-rspec-vim-with-tmux-and-dispatch)

Spend 12 weeks with us at our Boston office for the Metis [Rails
Bootcamp](http://www.thisismetis.com/ruby-on-rails-bootcamp?utm_source=thoughtbot&utm_medium=blog&utm_campaign=footer)
starting February 24.

-   [twitter](http://twitter.com/thoughtbot)
-   [newsletter](http://tinyletter.com/thoughtbot)

