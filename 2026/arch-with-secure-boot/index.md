---
date: 2026-01-28
draft: false
title: How to Use Arch Linux with Secure Boot Using rEFInd (Dual-Boot Guide)
description: A complete Arch Linux Secure Boot guide using rEFInd. For dual boot with Windows and gaming with Secure Boot support. Covers MOK enrollment and kernel signing
categories:
  - Linux
tags:
  - Linux
  - Securboot
  - Arch
  - rEFInd
  - dual_boot
  - secure_boot
slug: arch-with-secure-boot
comments: true
authors:
  - aref
featuredImage: cover.webp
---

Arch Linux and Secure Boot don’t always get along out of the box. While Arch is famous for flexibility and control, Secure Boot is strict, opinionated, and unforgiving when things aren’t signed correctly.

In this guide, you’ll learn **how to run Arch Linux with Secure Boot enabled using the rEFInd boot manager**, with a focus on **dual booting alongside Windows**. This setup is especially useful if you:

- Want a clean and modern UEFI boot experience
- Dual boot Arch Linux and Windows on the same system
- Play games that **require Secure Boot**, such as **VALORANT**
- Prefer managing your own Machine Owner Keys (MOK)

By the end of this guide, you’ll have a Secure Boot–enabled Arch Linux system that boots reliably using **rEFInd**, without disabling firmware security features.

---

## What You’ll Need 
- `refind` – Boot manager
- `shim-signed` – Secure Boot–compatible-preloader (from the AUR)
- `sbsigntool` – EFI binary signing
- `mokutil` – MOK key management
- `openssl` – Key generation
  
```shell
yay -S refind sbsigntool shim-signed mokutil openssl
```


## Generate Secure Boot Keys
```shell
mkdir -p ~/secureboot && cd ~/secureboot
sudo openssl req -newkey rsa:2048 -nodes -keyout MOK.key -new -x509 -sha256 -days 3650 -subj "/CN=Arch Secure Boot/" -out MOK.crt
sudo openssl x509 -outform DER -in MOK.crt -out MOK.cer
```
#### Move the generated key in rEFInd directory
```shell
sudo cp ~/secureboot/MOK.key /etc/refind.d/refind_local.key
sudo cp ~/secureboot/MOK.cer /etc/refind.d/refind_local.cer
sudo cp ~/secureboot/MOK.crt /etc/refind.d/refind_local.crt
sudo cp ~/secureboot/* /root/
```


## Installing rEFInd to the system
```shell
sudo refind-install --shim /usr/share/shim-signed/shimx64.efi --localkeys --yes
```
#### Regenerating the kernel image
```shell
sudo mkinitcpio -P
```


## Signing the kernel
```shell
sudo sbsign --key ~/secureboot/MOK.key --cert /secureboot/MOK.crt --output /boot/vmlinuz-linux /boot/vmlinuz-linux
```
### Automating the kernel signing 
Create this file `/etc/initcpio/post/kernel-sbsign`:
```
#!/usr/bin/env bash

kernel="$1"
[[ -n "$kernel" ]] || exit 0

# use already installed kernel if it exists
[[ ! -f "$KERNELDESTINATION" ]] || kernel="$KERNELDESTINATION"

keypairs=(/root/MOK.key /root/MOK.crt)

for (( i=0; i<${#keypairs[@]}; i+=2 )); do
    key="${keypairs[$i]}" cert="${keypairs[(( i + 1 ))]}"
    if ! sbverify --cert "$cert" "$kernel" &>/dev/null; then
        sbsign --key "$key" --cert "$cert" --output "$kernel" "$kernel"
    fi
done
```
##### Make it executable
```shell
sudo chmod +x /etc/initcpio/post/kernel-sbsign
```


## Pacman hook for rEFInd
Create this file `/etc/pacman.d/hooks/refind.hook`:
```
[Trigger]
Operation=Upgrade
Type=Package
Target=refind

[Action]
Description = Updating rEFInd on ESP
When=PostTransaction
Exec=/usr/bin/refind-install --shim /usr/share/shim-signed/shimx64.efi --localkeys --yes
```


## Enroll the MOK key to the system's UEFI
```shell
sudo mokutil --import ~/secureboot/MOK.cer
```
you will be prompted to enter a password 
choose a simple one then reboot into the bios 
- Enable secure boot
- make rEFInd Boot Manager the default or move it to the top
- Save and exit
![Enroll-MOK](MOK.webp#center)
Now MOK manager will ask if you want to proceed with booting or enroll the key. Choose "Enroll MOK" -> "Continue" and enter the ==password created when enrolling==

> If you don't see **Enroll MOK** option you can use Enroll key from disk and then navigate to your /boot/EFI/refind/keys/refind_local.cer and hit enter -> continue ->Yes

---