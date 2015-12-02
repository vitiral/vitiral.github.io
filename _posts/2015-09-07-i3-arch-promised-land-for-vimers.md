---
layout: post
title:  "i3 Arch: the promised land for vimmers"
date:   2015-09-07
categories: linux
---

About a year ago I picked up vim and have never looked back. Vim made text editing *fun*, and made programming in python something that I could look forward to as a job. Ever since learning vim, I have wanted to change everything about my computer to use vimcommands, but have never really succeeded until now.

Around the same time I learned vim, I tried out a linux distribution called [arch linux](archlinux.org). Arch bills itself as a light and simple linux distribution that tries to keep it simple. It is a rolling distribution that expects it's users to understand how to use linux and the tools they have installed. In return, it gives it's users the best wiki of any linux distribution, a fast binary package manger and a stable rolling release that stays at the cutting edge at all times.

When you install arch, you start out at a shell with an internet connection and that is pretty much it. You are expected to create your partitions with `parted`, mount a drive and do a base install, do a kernel swap and get everything running. Fortunately, the documentation is pristine, the [Arch Begginner's Guide](https://wiki.archlinux.org/index.php/Beginners'_guide) being one of the best introductions to linux that I know of.

When I first tried out Arch linux a year ago, I got through the begginer's guide and was left with a base install and unsure where I should go. I didn't want to use Unity, so what should I use? Somehow I ended up with the Plasma desktop -- which was a complete disaster. Arch doesn't come with any of the bells and whistles by default, so I found myself with a desktop with almost nothing installed that I needed - it had no network widgets, no program search feature -- nothing. So I quit, thinking I wasn't yet ready for Arch.

A few weeks ago I discovered the perfect window manager for Arch: the [i3 window manager](https://i3wm.org/). It perfectly follows the Arch Way, being simple (about 10,000 lines of C code), configured via text and having excellent documentation. It is also the perfect desktop for those of us who want to only use the keyboard at all times and don't even want everything to look flashy -- the default status bar is just a line of text that looks like I am in a terminal.

So that is what these first several blogs will be about: the perfect desktop environment for vimmers. The main things I want to focus on are the following:
- vim-like keyboard commands for everything (if reasonable/possible)
- fast and lightweight
- full featured -- no terminal webbrowsers here :)

To overview, the software that I have selected that most meets these requirements are as follows:
- [Arch Linux](archlinux.org). The base install is minimal and allows us to configure everything without the OS getting in the way
- [i3 window manager](https://i3wm.org/) is our window manager (i.e. desktop). We will also use i3lock for our screen lock.
- [urxvt](https://wiki.archlinux.org/index.php/Rxvt-unicode) as a terminal emulator. It is lightweight and very configurable via text, allowing us to do things like map `Alt+c` to copy and `Alt+v` to paste.
- [vim](https://wiki.archlinux.org/index.php/Vim) with a highly customized `.vimrc` and several packages to make it a full featured IDE (code completion/lookup, refactoring support, etc)
- [tmux](https://wiki.archlinux.org/index.php/Tmux) with highly customized `.tmux.conf` file to make it vim mode for additional terminal emulation. This is particularly useful if you ssh into your work machine, and may not be necessary for you (as i3 is often better IMO)
- [firefox](https://www.mozilla.org/en-US/firefox/desktop/) with the [vimperator](https://addons.mozilla.org/en-us/firefox/addon/vimperator/) addon. This allows us to use vim commands to browse the internet. I hardly ever use the mouse anymore.

**Some more minor software**
- [zsh](https://wiki.archlinux.org/index.php/Zsh) as our command line. Zsh is highly configurable and is generally considered more user friendly than bash. It has useful features like history searches based on the substring you have and vim-mode (although vim-mode requires some configurating with substring search)
- [apvlv](http://naihe2010.github.io/apvlv/) as a vim-like pdf viewer.
- [WeeChat](https://weechat.org/) with bitlbee as a chat client. I haven't actually gotten this working yet, but I hear it's good.
- [blog software](http://jekyllrb.com/) -- the software that supports this blog. All blog posts are just written with vim in github markdown and pushed to github.
- [litevault](https://github.com/vitiral/litevault) -- command line password manager written in a single python file (written by me)

> Before I go too far, you can view pretty much everything I am going to say from my ever evolving [dotfiles](https://github.com/vitiral/dotfiles).

In this series, we will learn how to configure all of these for their "vim mode", giving us a uniform interface to work with our programs and having to learn only a few tried and true key commands.
