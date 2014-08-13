[![thoughtbot logo](/images/logo.svg "Giant Robots Smashing Into Other Giant Robots")](/)
-----------------------------------------------------------------------------------------

[GIANT ROBOTS SMASHING INTO OTHER GIANT ROBOTS](/)
--------------------------------------------------

### Written by [thoughtbot](http://thoughtbot.com)

[Josh Clayton](https://twitter.com/joshuaclayton)

[March 20, 2012](/how-to-copy-and-paste-with-tmux-on-mac-os-x)

-   [tmux](/tags/tmux)
-   [osx](/tags/osx)

**Update** (Jul1 19, 2013) - tmux 1.8 is out, and with it comes a
simplified integration with the OS X clipboard. Check out [tmux Copy and
Paste on OS X: A Better
Future](http://robots.thoughtbot.com/post/55885045171/tmux-copy-paste-on-os-x-a-better-future)
for or updated technique.

* * * * *

[tmux](http://tmux.sourceforge.net/) is [becoming pretty popular as of
late](http://www.google.com/trends/?q=tmux&geo=all&date=all), but as
with any new technology, there are skeptics. I'm here to quell some
rumors and outline how to start using tmux effectively.

Out of all the questions I get, the most common is, "Does copying and
pasting work in tmux? I heard it wasn't possible." Forget that noise;
copying and pasting works wonderfully with a couple of extra steps.

Step 1: Install reattach-to-user-namespace
------------------------------------------

If you're using Homebrew, run:

    brew install reattach-to-user-namespace

If you're not, [head to the
repository](https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard) and
follow the instructions to compile yourself. Be sure to check out Chris
Johnsen's [great
writeup](https://github.com/ChrisJohnsen/tmux-MacOSX-pasteboard/blob/master/README.md)
about what `reattach-to-user-namespace` is actually doing.

Step 2: Configure tmux
----------------------

Open up your tmux configuration (typically at \~/.tmux.conf) and throw
this line at the top of the file:

    set-option -g default-command "reattach-to-user-namespace -l zsh"

This assumes that you've installed reattach-to-user-namespace (and it's
in your `$PATH`) and that you're using zsh. Every time you open a new
window or pane, it'll run `reattach-to-user-namespace`, which digs into
some of Apple's inner-workings to enable pbcopy and pbpaste support.

Step 3: Configure Other Applications
------------------------------------

If you're using Vim in a terminal, set your clipboard to the unnamed
clipboard (make sure it's not wrapped in a conditional
`has("gui_running")`).

    set clipboard=unnamed

If you want copying and pasting to work from your tmux buffers, you may
want to run something similar to this:

    #!/bin/sh

    while true; do
      if test -n "`tmux showb 2> /dev/null`"; then
        tmux saveb -|pbcopy && tmux deleteb
      fi
      sleep 0.5
    done

That'll look to see if there are any buffers in tmux; if there are,
it'll copy it into the system clipboard and delete the buffer. This is
really handy if you're trying to grab a backtrace or the output from a
command.

Spend 12 weeks with us at our Boston office for the Metis [Rails
Bootcamp](http://www.thisismetis.com/ruby-on-rails-bootcamp?utm_source=thoughtbot&utm_medium=blog&utm_campaign=footer)
starting February 24.

-   [twitter](http://twitter.com/thoughtbot)
-   [newsletter](http://tinyletter.com/thoughtbot)

