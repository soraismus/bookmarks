[![thoughtbot logo](/images/logo.svg "Giant Robots Smashing Into Other Giant Robots")](/)
-----------------------------------------------------------------------------------------

[GIANT ROBOTS SMASHING INTO OTHER GIANT ROBOTS](/)
--------------------------------------------------

### Written by [thoughtbot](http://thoughtbot.com)

[Joël Quenneville](https://twitter.com/joelquen)

[August 08, 2013](/running-specs-from-vim-sent-to-tmux-via-tslime)

-   [tmux](/tags/tmux)
-   [vim](/tags/vim)
-   [ruby](/tags/ruby)
-   [testing](/tags/testing)

When I first started working at thoughtbot, I was impressed to see some
of my colleagues sending the current spec in vim to a tmux pane with the
press of a key. This makes for an very fast testing/feedback loop and
streamlines the TDD process.

Determining which spec(s) to run
--------------------------------

We don't always want to run the full test suite. Often, we just want to
run a single spec or even a single example. We recently released
[vim-rspec](http://robots.thoughtbot.com/running-specs-from-vim), a vim
plugin that allows us to run the desired set of specs with the press of
a key.

Sending the spec command
------------------------

`vim-rspec` only does half the job however. We need a second component
to send the commands from vim to the desired tmux pane.
[tslime.vim](https://github.com/jgdavey/tslime.vim) is a plugin that
does exactly that by providing a `Send_to_Tmux()` function.

Tying it all together
---------------------

First install `vim-rspec` and `tslime.vim` using
[Vundle](https://github.com/gmarik/vundle):

    Bundle 'thoughtbot/vim-rspec'
    Bundle 'jgdavey/tslime.vim'

Add the following to your `.vimrc`:

    let g:rspec_command = 'call Send_to_Tmux("rspec {spec}\n")'

This tells `vim-rspec` to use `Send_to_Tmux` to run the selected specs.

Finally, all this wouldn't be of much use if we couldn't run it at the
press of a key. Add these keybindings to your `.vimrc` (this step is
unnecessary if you're using
[thoughtbot/dotfiles](https://github.com/thoughtbot/dotfiles)):

    " vim-rspec mappings
    map <Leader>t :call RunCurrentSpecFile()<CR>
    map <Leader>s :call RunNearestSpec()<CR>
    map <Leader>l :call RunLastSpec()<CR>
    map <Leader>a :call RunAllSpecs()<CR>

Thats all you have to do! The first time you run a spec this way, you
will be asked to input the names of your tmux session, window and pane
(protip, you can tab-complete these). Every subsequent time, your tests
will just magically run in the desired pane without you ever needing to
leave vim.

Bonus
-----

You can completely customize the command that gets sent to your tmux
pane. For example, to run the specs using
[Spring](https://github.com/jonleighton/spring) you would use

    let g:rspec_command = 'call Send_to_Tmux("spring rspec {spec}\n")'

Spend 12 weeks with us at our Boston office for the Metis [Rails
Bootcamp](http://www.thisismetis.com/ruby-on-rails-bootcamp?utm_source=thoughtbot&utm_medium=blog&utm_campaign=footer)
starting February 24.

-   [twitter](http://twitter.com/thoughtbot)
-   [newsletter](http://tinyletter.com/thoughtbot)

