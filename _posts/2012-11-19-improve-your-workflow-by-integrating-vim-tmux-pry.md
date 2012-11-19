---
layout: post
title: "Improve Your Workflow by integrating vim/tmux/pry"
description: ""
category: 
tags: []
---
{% include JB/setup %}

Motivation
----------

When I joined [Crowdtap](http://corp.crowdtap.com/jobs/) last June,
I needed to massively refactor one of our applications, and I needed
to seriously speed up my workflow.

I wanted things to be _instant_ and _interactive_. I needed to feel closer
to the code.

I tried [Spork](https://github.com/sporkrb/spork), but found it limited
in what it could do: switching from rspec/cucumber is hard, reloading is
problematic. [Zeus](https://github.com/burke/zeus) lifts these limitations,
but introduces new ones. Regardless, These solutions solve the _instant_
problem to some extent, but do not address the _interactive_ one.

irb-config
----------

I am approaching the problem differently. Instead of having to use a
preloader, I found that staying in the ruby interactive shell turns out to be
very effective. To support my workflow, I made [irb-config](https://github.com/nviennot/irb-config).
It allows running [RSpec](https://github.com/rspec/rspec)/[Cucumber](https://github.com/cucumber/cucumber)
tests directly in the rails console.

[irb-config](https://github.com/nviennot/irb-config) sits on top of [RVM](https://rvm.io/),
bypasses [Bundler](http://gembundler.com/)
to load [Pry](https://github.com/pry/pry),
[Pry Doc](https://github.com/pry/pry-doc),
[Pry Rails](https://github.com/rweng/pry-rails),
[Pry Debugger](https://github.com/nixme/pry-debugger),
[Pry Stack Explorer](https://github.com/pry/pry-stack_explorer),
[Awesome Print](https://github.com/michaeldv/awesome_print),
a [Mongoid](https://github.com/mongoid/mongoid) pretty printer with [Coderay](https://github.com/rubychan/coderay),
and a [Gnuplot](https://github.com/rdp/ruby_gnuplot) integration.

Workflow
---------

I am a heavy user of [Vim](http://www.vim.org/), but it doesn't have a shell like
[Emacs](http://www.gnu.org/software/emacs/). Fortunately, we have [tmux](http://tmux.sourceforge.net/)
to the rescue. By using the [vim screen](https://github.com/ervandew/screen) plugin,
we can integrate vim and irb-config, which is more powerful than using
something like [guard-rspec](https://github.com/guard/guard-rspec) in my opinion.

Let me share my workflow with you.

<div class="screencast" markdown="1">
<video class="video-js vjs-default-skin" controls="controls" poster="/assets/themes/the-minimum/img/screencast_poster.jpg"
    width="690" height="388" preload="true" data-setup="{}">
  <source type="video/mp4" src="http://velvetpulse.s3.amazonaws.com/screencasts/irb-config.mp4" />
</video>
[download mp4](http://velvetpulse.s3.amazonaws.com/screencasts/irb-config.mp4) (09:13 / 720p / 26Mb)
</div>

Notes
-----

### Starting in the dev environment

When doing testing in the console with irb-config, always run `rails c`
and let irb-config switch between your development environment and test
environment. This will allow painless reloading.

If you application has some gems that do not like to have their environment
switched, start the console with `rails c test`. But be careful, you will
have to use `:W` to reload the files you are changing.

### Vim bindings

You can use my [vim-config](https://github.com/nviennot/vim-config), or
install the [screen](https://github.com/ervandew/screen) plugin and these bindings:


<div class="small">
{% highlight vim %}
map <F5> :ScreenShellVertical<CR>
command -nargs=? -complete=shellcmd W  :w | :call ScreenShellSend("load '".@%."';")
map <Leader>r :w<CR> :call ScreenShellSend("rspec ".@% . ':' . line('.'))<CR>
map <Leader>e :w<CR> :call ScreenShellSend("cucumber --format=pretty ".@% . ':' . line('.'))<CR>
map <Leader>b :w<CR> :call ScreenShellSend("break ".@% . ':' . line('.'))<CR>
{% endhighlight %}
</div>

Want it?
--------

    git clone git://github.com/nviennot/irb-config.git ~/.irb
    cd ~/.irb
    make install

You don't need to change any of your projects. irb-config is pervasive.
When the `.irbrc` file is loaded, irb-config adds the global gemset to Bundler,
and loads pry.

Config files used
-----------------

* [https://github.com/nviennot/irb-config](https://github.com/nviennot/irb-config)
* [https://github.com/nviennot/tmux-config](https://github.com/nviennot/tmux-config)
* [https://github.com/nviennot/vim-config](https://github.com/nviennot/vim-config)
* [https://github.com/nviennot/zsh-config](https://github.com/nviennot/zsh-config)
