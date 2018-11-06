---
layout: post
title:  "Sharpening My Tools"
date:   2018-11-06
categories: update
---

As most Vim users know, Vim is a highly customizable text editor.  Sometimes I put off spending time on my Vim setup in favor of getting more critical (in my mind) things done.  However, when I do get around to investing the time to improve my workflows, I always wish I had done it sooner as I find it makes my life easier and my coding so much more efficient.  Here’s some recent additions I have made to my Vim workflow that have really made a difference.  Also, I would like to publically renew my commitment to constant workflow improvement :)

*Ctrl-F (and other commands, see :h cmdline-window) opens a command line window* - I stumbled across this and can’t believe I didn’t know about it sooner. Accesses Vim command line history and makes moving around while typing commands so much easier.

_Running Specs asynchronously with tpope/vim-dispatch plugin_ - another one to add to the “why didn’t I use this sooner” list.  I was previously using the thoughtbot/vim-rspec mappings but I didn’t like that the specs opened a different buffer over top of the spec file.  Now I can see what spec is failing and work in the spec file at the same time.  It’s awesome! After installing the vim-dispatch plug-in here’s what I added to my vimrc.

``` map <leader>a :Dispatch rspec %<CR>
 map <leader>s :execute "Dispatch rspec %:" . line(".")<CR>```

*Automatically opening an rspec file while looking at a source file* - Ugh, it pains me to think of the time I’ve lost on this one :)

```nnoremap <leader> sp :sp join(["spec"] + split(expand(
"%:r"),"/")[1:-1],"/")."_spec.".expand("%:e")<cr>```

