---
date: 2025-09-22T17:16:18+03:00
draft: false
title: Make your Linux terminal look cool
description: Learn how to make your Linux terminal look amazing with Fish shell, Kitty terminal, Fastfetch, Btop, and customizations like cursor animations, colorful output, and improved commands display.
categories:
  - Linux
tags:
comments: true
featuredImage: cover.webp
authors:
  - abdo
---
## Fish shell
Why use Fish? Because it’s user-friendly, comes with auto suggestions, syntax highlighting, and is much more modern compared to Bash or zsh.  


```shell
sudo pacman -S fish
```

### Making fish the default shell:
```shell
chsh -s /usr/bin/fish
```

### Edit greeting message
```shell
set -U fish_greeting "Hello there"
```
or you can disable it
```shell
set -U fish_greeting
```

### customize fish prompt
The reference is [Tide](https://github.com/IlanCosman/tide) fish prompt.

```shell
# install fisher (to easily install fish plugins)
sudo pacman -S fisher
# install tide prompt
fisher install IlanCosman/tide@v6
# to open the configuration wizard
tide configure
```

## Terminal (with cursor animations)

<video width="100%" controls>
  <source src="alacritty-cursor-animation.mp4" type="video/mp4">
  Alacritty terminal with cursor animations
</video>

I've tried 2 terminals that enables cursor trail or animations, **Alacritty** and **Kitty**. It's by default available in **kitty** but in **alacritty** you must build it from source with some shaders. Here are the 2 options, my default terminal now is **Kitty** as also it supports images like in fastfetch shows below and other features.
### Alacritty
There's an [Alacritty fork](https://github.com/GregTheMadMonk/alacritty-smooth-cursor) that supports cursor animations which looks stunning.

You can build it from source or install it from the **AUR** if you're using Arch Linux:
```shell
paru -S alacritty-smooth-cursor-git
```
You can find my alacirtty config in my repo for [Arch-Dots](https://github.com/abdalrahmanshaban0/Arch-Dots).

### Kitty
Just by adding this in `~/.config/kitty/kitty.conf` cursor trail will work
```
cursor_trail 10
cursor_trail_decay 0.1 0.3
cursor_trail_start_threshold 0
```

## Making commands output look better 
### ls alternative (eza)
```shell
sudo pacman -S eza
```
You can these aliases (put them in `~/.config/fish/config.fish`):
```shell
alias ls="eza -l --color --icons --git --group-directories-first"
alias la="eza -la --color --icons --git --group-directories-first"
```

### Pacman
To enable colors and change progress bar look, edit `/etc/pacman.conf`,
under `[options]`, add or uncomment `Color` and `ILoveCandy`.

<video width="100%" controls>
  <source src="pacman-colors-ilovecandy.mp4" type="video/mp4">
  Pacman with colors and ILoveCandy progress bar
</video>

### Fastfetch
[Fastfetch](https://github.com/fastfetch-cli/fastfetch) is a CLI tool for fetching system information and displaying them in a pretty way. It's better than neofetch which is deprecated now.

First you should specify what info to be displayed and the ascii figure that will appear.

You can run `fastfetch --gen-config-full` to generate a full config file with all optional options then you can comment what you don't want to be displayed.

There will be default default ascii logo for your distro you can search on the web for any thing else. And feel free to see my [dotfiles](https://github.com/abdalrahmanshaban0/Arch-Dots)

![Fastfetch](fastfetch.webp#center)

You must install `imagemagick` to use png logos in fastfetch if your terminal supports that (kitty does).

```shell
sudo pacman -S imagemagick
```

### Btop
You can edit just 2 lines to make it looks like this:
![Btop config](btop.webp#center)
```
color_theme = "tokyo-night"
theme_background = False
```
Also you can select what blocks to show by default, I added gpu0
```
shown_boxes = "mem net proc gpu0 cpu"
```

👉 You're welcome to visit my [Arch-Dots](https://github.com/abdalrahmanshaban0/Arch-Dots) repo for all my dotfiles.

**See also**:
- [Arch Linux with Hyprland | Comprehensive guide](/arch-linux-guide)
