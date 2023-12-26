---
title: "Neovim First Time Setup Experience"
date: 2023-12-25T16:36:36+05:30
draft: false
tags:
- productivity
- developer setup
- neovim
categories:
- productivity
- developer setup
- neovim
---

After years of dancing around the idea, I have decided to switch to NeoVim over the next week. This post will document my journey
with it over the week of 25<sup>th</sup> of December 2023 to the 1<sup>st</sup> of January 2024.

I will continue using vim-mode in VSCode over the next week and starting using NeoVim for personal projects (now that I have graduated,
I am actually doing personal projects on the weekends).

### Why

VSCode is increasing becoming a "cloud" based editor and moving off the local system. While I think "you will own nothing" is eventual,
I still like things that work without connecting to the internet.

Also, VSCode even without all the plugins I've loaded into it just doesn't feel as snappy as Vim or Neovim. That's important to me.


Finally, this begs the question why NeoVim over Vim? Because LUA is far less painful to learn than VimL and might someday
actually be useful for something else. There's also a vibrant community around Neovim on Reddit and very good tutorials online.

### Basic Setup


We will use Lazyvim's starter template [located here](https://www.lazyvim.org/installation) as a starting point to move from OOTB Neovim to something more usable. An advantage of this plugin manager is lazy loading and since perf is what we're after this looks like a good option.

#### 25 <sup>th</sup> December 2023

* Added starter template - because starter.
* Mapped leader to `,` because that's what I'm used to.
* Ran into [this bug](https://github.com/neovim/neovim/issues/25295) and remembered why I moved off this kinda tech in the first place. 
* Fixed the issue by updating nvim, since the devs were kind enough to backport the fix!
* Removed a bunch of plugins that come in the lazyvim example which I'm like 99% sure I won't be using.
* Installed vim-sneak because I'm used to easymotion, however per the internet that messes up the LSP because it modifies the file buffers.

Over the week I will continue to understand and remove bloat from this config to personalise it. But I like this backwards approach
over starting from scratch and not having a usable setup.
