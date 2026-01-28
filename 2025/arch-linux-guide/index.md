---
date: 2025-06-24
draft: false
title: Arch Linux with Hyprland | Comprehensive guide
description: Arch Linux is one of the most famous Linux distributions. It is very popular for superusers. Hyprland is also a very good and amazing window manager for Wayland. This is a complete guide for using Arch Linux with Hyprland for daily basis with Windows as dual boot. Starting from installation until all programs I use and my workflow
categories: 
    - Linux
tags: 
    - Linux
    - Hyprland
slug: "arch-linux-guide"
authors:
  - "abdo"
featuredImage: "cover.webp"
---
## Start with the programs
You should begin with the programs you're willing to use, then choose the suitable hardware with your targeted OS in mind. **Examples**:
- The free version of **Davinci Resolve** on **Linux** doesn't support **H264** codec unlike **Windows**, but supports **AV1**, so you need a graphic card with **AV1** encoder or to buy the paid version of **Davinci Resolve** or to dual boot **Windows**.
- If you'll use **Blender** then, you should buy some **Nvidia** GPU as **CUDA** is very supported on **Blender**. **Blender** and **CUDA** works very well on both **Linux** and **Windows**.
- Running **LLMs** locally, VRAM  will be very important, **Apple**'s unified memory may be very useful.
- **Adobe programs**, **Microsoft Office**, **Valorant**, ... Not working on **Linux** but very supported on **Windows**.

Have a program or two to run on Windows? Very simple, [dual-boot](https://sudostart.com/#dual-boot).

## Choosing a Linux distribution
In my opinion easing the use of Linux makes it harder. Any problem may faces you, you're on your own to fix it. So, at least the ability to read and write English well is a must to get started e.g. to:
- Search online or ask AI tools like **ChatGPT**
- Read a wiki or documentation
- Read forums and **Reddit** posts
- Read a comprehensive guide like this

The fear and difficulty of using a terminal is what makes you a beginner, and "Easy distros" is very bad in making you in no need to open a one.

Searching, basic commands, basic vim usage, basic knowledge about Linux filesystem and the most important is... getting started.

### Why Arch Linux
I've had a good experience with **Arch Linux** + **Hyprland**. What I mean by good experience is whether the system fits my needs or not.
- **Arch Wiki** is one of the most comprehensive and valuable resources for Linux users not just Arch.
- **Rolling release** Arch Linux follows a **rolling release** model, meaning it receives continuous updates instead of periodic major releases
- **Bleeding-edge** packages and drivers
You get access to the latest versions of software, libraries, and drivers as soon as they’re released.
- **Endless Software Availability** The **AUR** is a community-driven repository that offers packages not included in the official Arch Linux repositories.
- **Community** You'll find so many solved problems you may face in Arch Linux, also there's a **newbie corner** that almost will cover all beginner's questions.
#### My reasons to use **Arch Linux**
- Building the latest versions of open-source projects like Hyprland, Waybar and Quran-Companion to modify and tinker with the code.
- New packages and drivers, specially **Nvidia** GPU drivers. A newer GPU driver may affect the performance to make **Arch Linux** better than gaming distros like **Nobara Linux**
- AUR make it very easy to install latest versions of libraries, IDEs, programs. e.g. clipse-bin, google-chrome, clion, intellij, android-studio, ... instead of building from source or installing appimage from a website. AUR helpers like **paru** makes it more easier to manage and update these packages.

This is a walkthrough video for this tutorial:

{{< youtubeLite id="VF3dec6r0zs" label="Blowfish-tools demo" >}}

## Installing Arch Linux + Hyprland
### Connect to internet (WIFI)
```shell
rfkill unblock wifi

iwctl
device list
# replace wlan0 with your device
station wlan0 scan
station wlan0 connect SSID
```
### archinstall
proceed with minimal profile with NetworkManager, pipewire and grub selected.
### Post fresh minimal installation
#### sudo without password
To run all commands without password (you still need sudo for root permission)
```shell
# add this at the end of /etc/sudoers
%wheel ALL=(ALL:ALL) NOPASSWD: ALL
```
#### connect to internet:
```shell
nmtui # Activate connection
```
#### Install paru (AUR helper):
```shell
mkdir .src
cd .src
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

#### Configure pacman:
In `/etc/pacman.conf`:
##### Enable multilib
Uncomment these 2 lines:
```shell
[multilib]
Include = /etc/pacman.d/mirrorlist
```
##### Enable colors and change progress bar look
Under `[options]`, add or uncomment `Color` and `ILoveCandy`

#### Hyprland installation
##### Install using the package manager
It's the recommended way for normal users
```shell
sudo pacman -S hyprland
```
##### Manual installation
#### Install Hyprland dependencies:
```shell
git clone https://github.com/hyprwm/hyprland-wiki
```
open using vim pages/Getting Started/Installation
copy the dependencies of Arch Linux manual installation (y in vim)
``:term`` in vim and paste it and enjoy you don't have to write them one by one :'D
#### Build and install Hyprland
```shell
git clone --recursive https://github.com/hyprwm/hyprland
cd Hyprland
make all && sudo make install
```
Install **kitty**, the default terminal for **Hyprland** and give a look to the default keybindings, just the necessary ones until you apply your configuration.
```shell
sudo pacman -S kitty
```
#### NVIDIA GPUs
Very important references:
- [Nvidia GPU - Arch Wiki](https://wiki.archlinux.org/title/NVIDIA)
- [Nvidia GPU - Hyprland Wiki](https://wiki.hyprland.org/Nvidia/)
- [Multi-GPU - Hyprland Wiki](https://wiki.hyprland.org/Configuring/Multi-GPU/)
```shell
sudo pacman -S nvidia-open-dkms nvidia-utils lib32-nvidia-utils libva-nvidia-driver
```
Create this file `/etc/modprobe.d/nvidia.conf`:
```
options nvidia_drm modeset=1
```
Edit this file `/etc/mkinitcpio.conf`:
```
MODULES=(nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

Add this environment variables to hyprland config:
```
env = LIBVA_DRIVER_NAME,nvidia
env = __GLX_VENDOR_LIBIRARY_NAME,nvidia
env = NVD_BACKEND,direct
env = ELECTRON_OZONE_PLATFORM_HINT,auto
```

#### Display manager and Hyprland session
The recommended way to start Hyprland is using uwsm in systemd distros (or just write Hyprland after login in tty). Also you can use SDDM with some fancy theme.
##### Using uwsm (Universal Wayland Session Manager)
[uwsm - Arch Wiki](https://wiki.archlinux.org/title/Universal_Wayland_Session_Manager)
```shell
sudo pacman -S uwsm vim
```
To launch Hyprland directly after tty login :
paste this in your shell profile (mostly ~/.bashrc)
```shell
if uwsm check may-start; then
    exec uwsm start hyprland.desktop
fi
```
##### Using SDDM
[SDDM - Arch Wiki](https://wiki.archlinux.org/title/SDDM)
```shell
sudo pacman -S sddm
sudo systemctl enable sddm
```
#### Install or make your dots
I already have configs for my workflow on Hyprland on a GitHub repo. You can also clone them and edit to fit yours.
https://github.com/abdalrahmanshaban0/Arch-Dots
#### Install all your favorite programs from a script
You may install  some programs or dependencies on demand or don't remember to write then in a single script for installation, a tip for that is to open ~/.bash_history and search for every -S and collect these packages in a script. But first, you must know and choose what these programs are.
### Programs
#### Fonts
```shell
sudo pacman -S noto-fonts noto-fonts-emoji ttf-liberation ttf-jetbrains-mono ttf-hack-nerd ttf-dejavu noto-fonts-cjk
```
#### Text editor
```shell
paru -S neovim gvim
```
#### Bash prompt
```shell
pacman -S starship
# add this to the end of ~/.bashrc
eval "$(starship init bash)"
```
#### Application launcher
```shell
sudo pacman -S wofi
# open it using:
pidof wofi || wofi --show drun
```
#### Wallpapers
```shell
paru -S swww waypaper
```
#### Network manager and Bluetooth 
```shell
paru -S network-manager-applet bluez bluez-utils bluez-obex blueman
sudo systemctl enable bluetooth.service
```
#### Firewall
```shell
sudo pacman -S ufw
sudo ufw enable
sudo systemctl enable ufw.service
```
#### File manager and archiving
```shell
sudo pacman -S thunar gvfs gvfs-mtp ntfs-3g thunar-volman ffmpegthumbnailer tumbler man-db lsd bzip2 gzip p7zip unrar zip unzip 
```
#### Terminal
```shell
sudo pacman -S alacritty
```
#### Clipboard
```shell
paru -S wl-clipboard wl-clip-persist clipse-bin
```
In ~/.conf/hypr/hyprland.conf
```
exec-once = clipse -listen

windowrulev2 = float, class:(clipse)
windowrulev2 = size 622 652, class:(clipse)
windowrulev2 = stayfocused, class:(clipse)

bind = $mainMod SHIFT, V, exec, alacritty --class clipse -e clipse
```
#### Audio
```shell
paru -S pipewire pipewire-pulse pipewire-alsa wireplumber pavucontrol
```
#### Authentication for GUI apps
[Hyprland wiki](https://wiki.hypr.land/Hypr-Ecosystem/hyprpolkitagent/)
```shell
sudo pacman -S hyprpolkitagent
# exec-once = systemctl --user start hyprpolkitagent
```
#### Optional (just to be cool)
```shell
paru -S cava fastfetch cmatrix
# A dock if you want
paru -S nwg-dock-hyprland
```
#### Notification manager
```shell
sudo pacman -S swaync
```
#### Screenshots
```shell
paru -S grimblast
# example
grimblast copysave area
```
#### Web browser
```shell
paru -S brave-bin google-chrome firefox
# for English spell checking support & Text-to-Speech
sudo pacman -S hunspell-en_US speech-dispatcher 
```
uBlock Origin filter list to hide YouTube Shorts for firefox [here](https://github.com/gijsdev/ublock-hide-yt-shorts)

Chromium-based browsers flags for Wayland (google-chrome, brave, ...):
![Chromium-based browsers flags for Wayland](chromium-based-browsers-flags-for-wayland.webp#center)
####  PDF viewer
```shell
paru -S evince xournalpp
```
#### GTK settings (themes)
```shell
paru -S nwg-look papirus-icon-theme
# I use Catppuccin Mocha GTK Theme
# Download it from https://www.gnome-look.org/p/1996672
# unzip it and copy it to /usr/share/themes
```
#### media player
```shell
paru -S mpv playerctl
```
#### poweroff, reboot, logout, lock
```shell
paru -S wlogout hyprlock
```
#### Note taking
```shell
paru -S obsidian
```
#### Brightness control
```shell
paru -S brightnessctl
```
#### git commands UI
```shell
paru -S lazygit
```
#### System monitoring
```shell
paru -S btop
```
#### Image viewer and editor tool
```shell
paru -S swappy
```
#### Printing
```shell
paru -S cups cups-pdf ghostscript gsfonts gutenprint system-config-printer
sudo systemctl enable cups
```
#### Status bar (Waybar)
```shell
sudo pacman -S waybar
```
My [Islamic prayer timings](https://sudostart.com/islamic-prayer-timings-for-linux/) daemon and custom-module for **Waybar**.

Nice icons:
```shell
paru -S ttf-font-awesome ttf-bootstrap-icons ttf-nerd-fonts-symbols
```
Fid your icons:
- [Bootstrap](https://icons.getbootstrap.com/)
- [Nerd Fonts](https://www.nerdfonts.com/cheat-sheet)
- [Awesome Fonts](https://fontawesome.com/icons)


#### Photo editing
```shell
paru -S gimp
# mostly I use online AI tools
```
#### Wine (Run windows programs)
enable multilib in /etc/pacman.conf
```shell
sudo pacman -Syyu wine wine-mono wine-gecko winetricks
```
#### Quran
```shell
paru -S quran-companion
```
#### Screen sharing
```shell
paru -S obs-studio xdg-desktop-portal-hyprland
```
#### Video editing
**Kdenlive** sucks and **Davinci Resolve** on Linux doesn't support **H.264** codec (the paid version does, also you can transcode all of your project videos for some hours and torture your hardware). The best solution for me is using a GPU that supports **AV1** video encoder. For audio, the ACC is not supported, so I use PCM 24-bit, For further information about [Supported Condecs](https://documents.blackmagicdesign.com/SupportNotes/DaVinci_Resolve_18_Supported_Codec_List.pdf?_v=1705996810000)

I'm currently using an AMD CPU that contains an integrated graphics that has AV1 encoder and Nvidia RTX4060 that also has AV1 encoder (I use the 4060 of course). This is my OBS studio settings:
![obs studio video output settings](obs_video_output_settings.webp#center)
##### Installing [Davinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve)
After installing the program from the official website:
```shell
# necessary packages
sudo pacman -S libxcrypt libxcrypt-compat pango
# run the application with this command:
LD_PRELOAD="/usr/lib/libgio-2.0.so /usr/lib/libgmodule-2.0.so /usr/lib/libglib-2.0.so" /opt/resolve/bin/resolve
# Note: you can edit the exec in the application launcher of davinci resolve at /usr/share/applications/com.blackmagicdesign.resolve.desktop
Exec=env LD_PRELOAD="/usr/lib/libgio-2.0.so /usr/lib/libgmodule-2.0.so /usr/lib/libglib-2.0.so" /opt/resolve/bin/resolve %u
```
###### AMD GPU
You need to download packages to support OpenCl to make Davinci Resolve work:
```sh
sudo pacman -S rocm-opencl-runtime opencl-rusticl-mesa
```
Note: Maybe you'll need to download packgaes for **ROCm** of AMD if **Davinci Resolve** supports it.
###### Nvidia GPU
```shell
# If needed only (No GPU problem)
sudo pacman -S cuda
```
#### Digital Camera as webcam
First make sure that your cam in [Supported  Cameras](http://www.gphoto.org/proj/libgphoto2/support.php). Also you need to see the [ArchWiki](https://wiki.archlinux.org/title/Webcam_setup) for more details.

```shell
sudo pacman -S v4l2loopback-dkms v4l2loopback-utils v4l-utils gphoto2 gvfs-gphoto2
```

First, connect your camera by usb to your computer. Runs this command and make sure you see your camera name `gphoto2 --auto-detect`. Show connected devices: `v4l2-ctl --list-devices`. You may find `/dev/video0` , in a laptop you may find 2 devices.

Loading `v4l2loopback` kernel module:
```shell
# Edit /etc/mkinitcpio.conf
MODULES=(... v4l2loopback ...)
# create /etc/modprobe.d/webcam.conf
options v4l2loopback card_label=Video-Loopback exclusive_caps=1
```

This is my aliases for some controls of my DSLR (Canon 2000d).
```shell
alias dslrcam="gphoto2 --stdout --capture-movie | ffmpeg -i - -vcodec rawvideo -pix_fmt yuv420p -threads 0 -f v4l2 /dev/video0"

alias dslrphoto="gphoto2 --capture-image-and-download"
```
In OBS studio, add video capture device, this is my settings: 
![OBS webcam settings](obs_settings_for_webcam.webp#center)
In my case, I don't have a capture card, I connect the camera using usb, so the resolution I have is `1024x576` so I add a filer for upscaling to `1920x1080`:
![obs scaling filter](obs_scaling_filter.webp#center)
I recommend using capture card if your camera has `clean HDMI output`, if not you may try to crop the portion of the video the focus square appears in.

#### Gaming
```shell
sudo pacman --needed -S lutris mangohud gamemode libxnvctrl goverlay vulkan-tools vulkan-icd-loader lib32-vulkan-icd-loader freetype2 lib32-freetype2
```
#### Torrent client
```shell
sudo pacman -S qbittorrent
```
#### Password manager
```shell
sudo pacman -S keepassxc
```
#### Dual Boot
I'm dual booting on 2 different drives and choose which one to boot in using Grub. This is the easier way, the order of installing linux or windows doesn't matter. 

If you want to dual boot on the same drive, Install Linux first with manual partitioning and let a free (unallocated) space at the end for **Windows**. like this:
![dual boot partitioning](dual-boot-patitioning.webp#center)

To make Grub supports that :
```shell
paru -S os-prober
```
Edit /etc/default/grub to uncomment the line (maybe last line) :
```
GRUB_DISABLE_OS_PROBER=false
```
You can add a Grub theme (install one from gnome-look). The theme dir usually contains `install.sh` that you can run as root to install the theme.

For manual way, copy the theme dir (that contains theme.txt) to either `/usr/share/grub/themes` or `/boot/grub/themes`. Then edit this line in `/etc/default/grub` with the theme.txt path like this:


```
GRUB_THEME="/boot/grub/themes/Xenlism-Arch/theme.txt"
```
update grub (generating grub.cfg)
```
sudo grub-mkconfig -o /boot/grub/grub.cfg
```

**See also**:
- [Make your Linux terminal look cool](/make-your-linux-terminal-look-cool)
