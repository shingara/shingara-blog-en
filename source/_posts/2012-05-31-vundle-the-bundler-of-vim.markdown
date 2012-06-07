---
layout: post
title: "vundle the bundler of vim"
date: 2012-05-31 09:48
comments: true
categories:
  - vim
  - editor
---

I use vim like text editor since my school. I allways appreciate vim and
I really don't want change to another one.

# How I manage my plugin around the time

Vim is really powerfull with this plugins. You need really quick use it.
It's really important to manage it. All around the time, I change how I
manage it with new technique inside vim or develop by other developper.

## Handmade management

In start of my vim usage, I manage my plugin by copying file in
different directory. I put in versionning my `.vimrc` and `.vim`. But
this technique can be hard to maintain in time. The installation is not
really simple and know if you need update or not is hard too. So the
evolution of plugins become rare.

## Vimball management

In vim evolution, the vimball management system was implemented. With
this vimball, you can more easily install plugin and know which version
you use. So you can improve your plugins management. But to know if a
new version is available, you need go to vimscript.org and check each of
your plugins. The update become rare too.

## git-submodule management

With github, all plugins become a git repository. We can check all new
commit on our favorite plugin. I start to link all plugin in a new
submodule and link file in good directory. The update was really simple
and my plugins can be up to date. But the installation become really
hard.

## Janus management

Yehuda Katz and Carl Lerche start a new project call [janus](https://github.com/carlhuda/janus).
This project's goal is an easy plugin management by rake task. In this
Rakefile, you can see the list of your plugin and with some rake task
you can install it and update it. This project was really great. It
start with a bunch of plugins pre-configure and good if you start using
vim and you are not aware with what plugin can be good to you. But in
time, the management can be a little tricky with version of janus. So
it's a really good project to start vim.

## Pathogen management, the first vim plugin to manage you plugins

To closed of janus's released, [Tim Pope](https://github.com/tpope) ( a
very important vim plugin developper ) release [Pathogen](https://github.com/tpope/vim-pathogen).
This plugin help to manage your vim plugin directly in your `vimrc` file.
I don't really use it. But a lot of people say it's a really good
plugin. There are a lot of fork of janus trying to use pathogen
directly.

## Vundle management

Now, someone told me about a very good plugin to manage his vim's
plugin, [Vundle](https://github.com/gmarik/vundle). As Pathogen, the
plugin management is do directly in your `vimrc`. His usage is really
simple and well designed. If you are a ruby guy and use already [Bundler](http://gembundler.org),
you can easily understand how works Vundle and how use it. Vundle is
directly inspired of this project.

# Using Vundle

## Install Vundle

To install vundle, you just need clone the project's repository in your
`.vim` directory and add 2 lignes in your `vimrc` file

```
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
```

Add in your `vimrc`

```vim
set rtp+=~/.vim/bundle/vundle/
call vundle#rc()
```

## Configure your plugins

To use vundle after his installation, it's really simple. You just need
add in your `vimrc` a `Bundle` command with path to your plugin. By
example, if you want install the `vim-rails` plugin, you juste need add
this line in your `vimrc` :

```vim
Bundle 'tpope/vim-rails.git'
```

## Install your plugins

Now, when your vim is open, you can install or update your plugins by
two vim's command.

 * `BundleInstall` install your plugins
 * `BundleInstall!` update your plugins

Now, you can try this wonderfull plugin and install all plugins you want
try.

# My vimrc

If you want see my own `vimrc` your can see it on one of my [github
repository](https://github.com/shingara/vim-conf). With the Vundle help,
it's really simple to understand what plugin I really use. All is in
only one file.

[Traduction Française](http://blog.shingara.fr/vundle-ou-le-bundler-de-vim.html)
