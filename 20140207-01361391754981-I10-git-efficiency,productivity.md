[Mixu's tech blog](http://blog.mixu.net/)
=========================================

When I read what I write I learn what I think

-   [Node book](http://book.mixu.net/)
-   [nwm](http://nwm.mixu.net/)
-   [Github](https://github.com/mixu/)
-   [![image](http://blog.mixu.net/wp-content/themes/cleanr/images/rss_feed.png)](http://blog.mixu.net/feed/)

* * * * *

Git tips and tricks
-------------------

This post is basically a collection of things I think I should remember
about git: setup, commands, multiple repos, more obscure options etc.

**Config**

Two things: enable colors, and make `git push` push the current branch
to a branch of the same name (and not all the branches, which is the
default). This means when you do git push, it pushes your working branch
but not other branches.

    git config --global color.ui true
    git config --global push.default current

**Zsh/bash aliases**

These are small things, but they make things so easy. I instinctively
type gs after changing directory nowadays.

    alias ga='git add'
    alias gp='git push'
    alias gl='git log'
    alias gs='git status'
    alias gd='git diff'
    alias gdc='git diff --cached'
    alias g='git'

    git config --global alias.lg "log --graph 
    --pretty=format:'%Cred%h%Creset
    -%C(yellow)%d%Creset %s %Cgreen(%cr)
    %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative"

Apparently, the “git lg” above can be replicated fairly well with:

    git log --decorate --graph --oneline

For more git log related discussion, see [this HN
thread](http://news.ycombinator.com/item?id=4130494).

**Managing multiple git repositories**

A fairly simple way of doing this is to add a function that you can use
to run the same command in multiple directories. This one is for zsh:

    gr() {
      local cmd="$*"
      for dir in /home/m/repos/{one,two,three}; do
       (
        print "nin $dir"
        cd $dir
        eval "$cmd"
       )
       done
    }

    alias grs="gr git --no-pager status -sb"
    alias grl="gr git --no-pager log --decorate --graph --oneline -n 3"
    alias grd="gr git diff"
    alias grdc="gr git diff --cached"
    alias grn="gr npm ls"

In the example below, I 1) switch all working repos to master, 2) fetch
the recent remote changes, 3) check the status of every repository, and
4) after going into those couple of dirs and stashing stuff (not shown
in detail), pull all repos to update them

    gr git checkout master
    gr git fetch
    grs
    in /home/m/repos/one
    ## master
    in /home/m/repos/two
    ## master...origin/master [behind 8]
    in /home/m/repos/three
    ## master...origin/master [behind 4]
     M Makefile
    gr git pull

**Add all changes or all changes to existing files**

    $ git add -A

Adds all files and file deletes, so you don’t need to enter them
manually.

    $ git add -u

Same, but doesn’t add files that didn’t exist before.

**Log-fu**

    $ git log -p

.. to show the patches alongside the log

    $ git fetch && \
    git log --author="`git config --get user.name`"\
    --pretty=format:'%h %an %Cred%ar %Cgreen%s' refactor..master

Shows the commits that you’ve made between “refactor” and “master”, e.g.
for emailing a changelog with your commits:

    6591466 Mikito Takada 7 months ago Port over more code
    1f464fb Mikito Takada 7 months ago Refactor window
    41a3ec9 Mikito Takada 9 months ago Handle switching from window to root window

    $ git log staging -1 -p -- my/subdir

Show the latest commit that affects the my/subdir path on the branch
“staging” with the patch.

    $ git log --stat

Show the changed files only.

    $ git log --author=foo

Only commits by author foo.

    $ git log --stat -- subdirectory

Show the commits affecting only “./subdirectory”, with a summary of the
files (–stat).

For example:

    $ git log --stat -- .

Shows the summary of commits affecting the current directory.

    $ git log --grep='.*foo.*'

Grep for a specific log message.

    $ git log --since="1 week ago" --until="2 weeks ago"

Filter commits by date.

    $ git shortlog -sn

Shows a “top contributors list” by commits.

**Diff-fu**

Differences between the staged files and the last commit:

    $ git diff --cached

I use that a lot just before committing the changes to check what my
commit changed.

So git diff is for the files that are unstaged, while git diff –cached
is for staged files.

What’s different between my branch and some other branch, e.g. master?

    $ git diff my_branch..master

What changed in the last three commits?

    $ git diff HEAD^3

Ignore whitespace?

    $ git diff HEAD^ --word-diff

What’s different between my branch and some other branch in subdirectory
(or file)?

    $ git diff my_branch..master subdirectory

Remember, git diff outputs an applicable patch:

    $ git diff > my.patch

    $ git apply < my.patch

**Grep-fu**

    $ git grep -e foo --or -e bar --not -e baz

Grep for foo or bar, but exclude the expression baz.

    $ git grep --untracked foo

Include untracked files in grep.

    $ git grep --cached foo

Include cached (staged) files in grep.

    $ git grep --no-index foo /var/bar

Use “git grep” instead of regular grep, grep for foo in /var/bar.
Compared to regular grep, git grep is nicer due to the colors and paging
defaults.

**Creating a feature branch and merging it back**

Checkout the branch you want to base your work on:

    $ git checkout master

Then create a branch and check it out:

    $ git checkout -b local_name

Push the branch to the remote

    $ git push -u origin local_name

Later on, merge it:

    $ git checkout master

    $ git merge local_name

Look at the unmerged branches:

    $ git branch --no-merged

Look at branches that contain a particular commit (e.g. when
cherrypicking):

    $ git branch --contains 1234abcd

**Stashing changes**

    $ git stash

    $ git stash list

    $ git stash apply

**Blaming**

    $ git blame path/to/file

See who changed what.

    $ git blame abce1234^ path/to/file

See the blame, starting one commit before abce1234. E.g. when you need
to trace back a particular line of code in history.

**Working with remotes**

Changing from Github to Bitbucket

    $ git remote -v # look at remotes

    $ git remove rm origin # delete

    $ git remote add origin git://addr/to/repo # add new remote

    $ git push -u origin master # push to new remote

Adding a upstream repo to pull in new changes from a forked project

    $ git remote add upstream git://addr/to/repo

    $ git checkout master && git pull upstream master

**Keeping the number of merges lower on small commits**

For small commits made on top of a frequently changing branch like
master, you might want to rebase your local changes on top of the
current remote before you merge a feature branch ([more in
depth](http://gitready.com/advanced/2009/02/11/pull-with-rebase.html)):

    $ git pull --rebase

    $ git push

**Resetting the current branch**

When you want to reset to the current HEAD.

    $ git reset --hard HEAD

When a branch tracking a remote has become outdated (e.g. you are on
staging now but your commits have diverged):

    $ git reset --hard origin/staging

**Cherry-picking a commit**

    $ git cherry-pick sha1_of_commit

Reverting (unapplying) a bad commit:

    $ git revert sha1_of_commit

**Total number of remote branches**

First, remove local branches that do not exist on origin (–dry-run if
you want to preview):

    $ git remote prune origin

    $ git branch -r | wc -l
     123

**Get the latest commit of all remote branches**

    #/bin/bash
    git branch -r | while read line ;
    do
      echo 
      echo "************" ${line#origin/}
      echo
      git log -1 $line
    done

**Get the latest commit of all remote branches, and summary, ordered by
age**

Use this Node script:

[https://gist.github.com/91ba46dee32b221b3a84](https://gist.github.com/91ba46dee32b221b3a84)

**Delete a local or remote branch**

Delete a local branch:

    git branch -d some_branch_name

Really delete a local branch, even if git complains about it in the
previous command:

    git branch -D some_branch_name

Delete a remote branch, e.g. in Github:

     git push origin :some_branch_name

The reason for that syntax is that you can do \`git push origin
local\_branch\_name:remote\_branch\_name\` so what that line above is
doing is essentially pushing NULL to the remote branch, thereby killing
it off.

This entry was posted on Friday, April 6th, 2012 at 2:35 am and is filed
under
[Technology](http://blog.mixu.net/category/tech/ "View all posts in Technology").
You can follow any comments to this entry through the [RSS
2.0](http://blog.mixu.net/2012/04/06/git-tips-and-tricks/feed/) feed.
You can [leave a comment](#respond), or
[trackback](http://blog.mixu.net/2012/04/06/git-tips-and-tricks/trackback/)
from your own site.

\

### One comment

\

### Leave a comment

[Click here to cancel reply.](/2012/04/06/git-tips-and-tricks/#respond)

You must be [logged
in](http://blog.mixu.net/wp-login.php?redirect_to=http%3A%2F%2Fblog.mixu.net%2F2012%2F04%2F06%2Fgit-tips-and-tricks%2F)
to post a comment.

-   -   Single page apps in depth
    -------------------------

    [New! Free ebook on single page applications at
    singlepageappbook.com](http://singlepageappbook.com/)

-   Mixu’s Node book
    ----------------

    [Read my free ebook on Node.js at
    book.mixu.net](http://book.mixu.net/)

-   Archives
    --------

    -   [Full archives (all posts on one
        page)](http://blog.mixu.net/archives/)
    -   [About](http://mixu.net/)

-   Popular Posts
    -------------

    Sorry. No data so far.

-   Categories
    ----------

    -   [Business](http://blog.mixu.net/category/business/ "View all posts filed under Business")
    -   [How-to](http://blog.mixu.net/category/how-to/ "View all posts filed under How-to")
    -   [Kohana](http://blog.mixu.net/category/kohana/ "View all posts filed under Kohana")
    -   [Meta](http://blog.mixu.net/category/meta/ "View all posts filed under Meta")
    -   [Node.js](http://blog.mixu.net/category/node-js/ "View all posts filed under Node.js")
    -   [Opinion](http://blog.mixu.net/category/opinion/ "View all posts filed under Opinion")
    -   [Technology](http://blog.mixu.net/category/tech/ "View all posts filed under Technology")
    -   [Uncategorized](http://blog.mixu.net/category/uncategorized/ "View all posts filed under Uncategorized")

-   Blogroll
    --------

    -   [Admin](http://blog.mixu.net/wp-admin/)

* * * * *

© 2014. Mixu's tech blog. Powered by [WordPress](http://wordpress.org/).
Theme design by [WPShoppe](http://wpshoppe.com/).
http://blog.mixu.net/2012/04/06/git-tips-and-tricks/
