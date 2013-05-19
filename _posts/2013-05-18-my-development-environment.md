---
layout: post
published: false
title: "My development environment"
---

I work as a Ruby developer at [Mynewsdesk][mynewsdesk] where we
building a large scale Ruby on Rails application equivalent to PRWeb,
or PR Newswire in some ways, but better!

I spend my days inside [`tmux`][tmux] and [`vim`][vim],
I love not using my mouse as little as possible, even though it's
centimeters away from my fingertips on my gorgeous 13" Macbook Air.

I like configuring my environment quite extensively but still keep the
my tools not to far from their intended behaviours.

# The one true editor
I'm probably going to eat up my hat if I change my editor at some
point, but for the time being I'm absolutely in love with `vim`
and plan to stick with it a while longer.

I've got a few plugins that I rely heavily on, my favorites are:

* [ctrlp](http://github.com/kien/ctrlp.vim)
* [nerdtree](http://github.com/scrooloose/nerdtree)
* [unimpaired.vim](http://github.com/tpope/vim-unimpaired)
* [commentary.vim](http://github.com/tpope/vim-commentary)
* [dispatch.vim](http://github.com/tpope/vim-dispatch)
* [endwise.vim](http://github.com/tpope/vim-endwise)
* [fugitive.vim](http://github.com/tpope/vim-fugitive)
* [rails.vim](http://github.com/tpope/vim-rails)
* [rake.vim](http://github.com/tpope/vim-rake)
* [repeat.vim](http://github.com/tpope/vim-repeat)
* [surround.vim](http://github.com/tpope/vim-surround)

Most of them, besides ctrlp and nerdtree, are made by the fantastic
[Tim Pope][tpope], *almost everyone of his plugins will be worth your
while*

## A few mods to CtrlP
As programmers we spend a large part of our days jumping between files,
and the easiest way to get around is with a fuzzy finder.

One little annoyance I had with Ctrlp was that it took some time for it
to index all of the files, this was only noticeable when in our huge
Rails app at Mynewsdesk, which has 8000-something files
in it!!

So I found this little configuration somewhere on the web which takes
advantage of how fast `git` is:

* If you are in a `git` repository, it will use `git ls-files` instead of
the slower `find`.
* If you are in a normal directory, *it will only index the first 10000
files instead of everything*

Here is the snippet:

{% highlight vim %}
let g:ctrlp_max_files = 10000
let g:ctrlp_user_command = {
                  \ 'types': { 1: ['.git/', 'cd %s && git ls-files'] },
                  \ 'fallback': 'find %s -type f | head -' . g:ctrlp_max_files
                  \ }
{% endhighlight %}

More cool stuff can be found in my open sourced [dotfiles][my_dotfiles]
repository on Github

# Tmux
Tmux lets you switch easily between several programs in one terminal,
split the windows, detach them (they keep running in the background)
and reattach them to a different terminal

The major reason why I run terminal `vim` is due to `tmux` and the flexibility
the combo gives me.

Usually I have a few splits open inside `tmux`, one for running `vim` and
a few more for random terminal usage. The combination makes it is extremely easy
to open up a new split to do something else and then continue along your train
of thought of whatever you were coding on before getting distracted.

I used to have different `tmux` sessions depending on what I was doing, I had
one for work and one for play. I persisted my work session to disk so I could
reattach to it even though I rebooted my laptop and all of my names of
all of my different project windows would still be intact. But these saved
session would get slower after a few days of hacking which bugged me a bit

Then I found this neat function that automatically renames your `tmux` windows
depending on the current directory. Now I have one `tmux` that I attach to both
on and off work, which I can shutdown if I so please.

Here is the snippet:

{% highlight bash %}
rename_tmux_window_to_current_dir() {
  if [ "$TERM" = "screen-256color" ]; then
    if [ "$PWD" != "$LPWD" ]; then
      LPWD="$PWD"
      tmux rename-window ${PWD//*\//}
    fi
  fi
}
{% endhighlight %}

Just add `rename_tmux_window_to_current_dir` to your `precmd` function in `zsh`,
in `bash` just add the code below to your config and you are all set.

{% highlight bash %}
export PROMPT_COMMAND=rename_tmux_window_to_current_dir
{% endhighlight %}

# Conclusion
If you are more interested of my environment check out my
[dotfiles][my_dotfiles] for more configurations or ask me anything on
[Twitter]({{ site.author.twitter }})

[mynewsdesk]:http://mynewsdesk.com
[tmux]:http://tmux.sourceforge.net/
[vim]:http://vim.org
[tpope]:http://github.com/tpope
[my_dotfiles]:https://github.com/metamorfos/dotfiles
