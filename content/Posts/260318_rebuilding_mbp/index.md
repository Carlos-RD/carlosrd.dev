+++
date = '2026-03-18T21:08:13-04:00'
draft = false
title = 'Rebuilding my MacBook'
description = 'Rebuilding my MacBook' 
categories = ['technology']
tags = ['MacOS']
+++

<div style="text-align: justify">
At the beginning of the year I had to set up my MacBook. This post is a quick guide as a reference
in case I need to configure another Mac from scratch. Hopefully it helps you as well.

## Configure MacOS Preferences

I start by configuring system preferences while updates are being installed in the background.
  
  1. Desktop & Dock > Disable "Click wallpaper to reveal desktop" and "Show widgets"
  2. Enable: "Automatically hide and show the dock". Disable: "Animate opening applications" and "Show suggested and recent apps in Dock"
  2. Map Caps Lock to ESC: System Settings > Keyboard > Modifier Keys 

Configure Finder settings: 

  1. General: New finder window set to home folder. Show as list. 
  2. Tags: Remove tags 
  3. Sidebar: Disable recents. Rearrange icon order
  4. Advanced: Set When performing search to "current folder".
  5. Finder > View > show pathbar and status bar. 

## Install Applications

### Homebrew

[Homebrew](https://brew.sh/) is the cornerstone of how I install and keep apps and packages up to date.

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Raycast

```shell
brew install --cask raycast
```

[Raycast](https://www.raycast.com/) makes my life so much easier. I use it to launch apps,
manage [Homebrew](https://brew.sh/) packages, and as my window manager.

![Raycast](raycast.jpeg#center "Quick snapshot of my app and window management shortcuts")

### iTerm2 + Oh My Zsh + Powerlevel10k

[iTerm2](https://iterm2.com/) is my terminal of choice. This time I went with [Anonymous Pro](https://www.marksimonson.com/fonts/view/anonymous/) as my font.

I pair it with [Oh My Zsh](https://ohmyz.sh/) and [Powerlevel10k](https://github.com/romkatv/powerlevel10k). Although its development stopped in 2024 and many people are migrating to Starship which I've already been using on Linux.

### Tmux

[Tmux](https://github.com/tmux/tmux/wiki) to force me to use the CLI as my main development workspace.

### Git and Lazygit

[Git](https://git-scm.com/) and [Lazygit](https://github.com/jesseduffield/lazygit) for version control.

### LazyVim

[LazyVim](https://www.lazyvim.org/) as my main text editor. I'm very far from using it to its full
potential, but I'll get there if I stick with it!

### Karabiner-Elements

I use [Karabiner-Elements](https://karabiner-elements.pqrs.org/) to manage my key mappings for external keyboards, mapping Caps Lock to Escape so one does not injure his left hand, and configure [home row mods](https://precondition.github.io/home-row-mods).

### Obsidian

[Obsidian](https://obsidian.md/) as my second brain and knowledge management system.

### GNU Stow

[GNU Stow](https://www.gnu.org/software/stow/) so I don't go crazy managing my [dotfiles](https://github.com/Carlos-RD/dotfiles).

### Hidden Bar

To keep the menu bar nice and clean.

### Go + Hugo + Blowfish

Install [Go](https://go.dev/doc/install), [Hugo](https://gohugo.io/installation/macos/) and [Blowfish](https://blowfish.page/docs/installation/) so I can start blogging again.

### Shottr

[Shottr](https://shottr.cc/) is my go to for taking and annotating screenshots.

## Wallpaper

Perhaps the most crucial step of the whole process is setting your favorite wallpaper and installing your music player platform. Et voilà! We are off to the races!

</div>

