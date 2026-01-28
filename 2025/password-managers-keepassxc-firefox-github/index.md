---
title: "Password Managers | My Setup with KeePassXC, Firefox, and GitHub"
date: 2025-09-24T12:00:00+00:00
description: "Securely manage passwords on Linux with KeePassXC, Firefox autofill, and a private GitHub repo for syncing your encrypted database."
tags: ["linux", "password manager", "keepassxc", "firefox", "security", "github"]
categories: ["Linux", "Productivity"]
draft: false
comments: true
slug: "password-managers-keepassxc"
authors:
  - "abdo"
featuredImage: "cover.webp"
---

One of the most important things you can do to keep yourself safe online is to ensure that your passwords are unique in every website or service and also to be hard and difficult to guess or brute force. 

It’s almost impossible to remember every password. So, managing passwords securely is a non-negotiable part of modern digital life.
If you're on **Linux** or **Windows** or **Mac** and want a **secure, open-source, and flexible password manager**, [KeePassXC](https://keepassxc.org/) is one of the best tools available. 

In this post, I’ll walk you through **my personal setup**:  
- KeePassXC as the password manager, passwords database is stored locally 
- The official KeePassXC-Firefox extension for autofill  
- Syncing the encrypted database via a **private GitHub repository**  

This workflow gives me the balance of **security, privacy, and convenience** I need.

---

## Why Use a Password Manager?

A password manager solves three problems at once:  

1. **Strong passwords** — No need to remember them, so they can be long and random.  
2. **Convenience** — Autofill credentials in browsers like Firefox without copying/pasting.  
3. **Security** — Protects against phishing and reuse of weak passwords.  

Unlike proprietary cloud-based managers, **KeePassXC keeps you in control** of your data. No third-party servers, no subscription fees, and full offline functionality.

---

## Setting Up KeePassXC on Linux

On most distributions, KeePassXC is available via package managers:  

```bash
# Ubuntu / Debian
sudo apt install keepassxc

# Fedora
sudo dnf install keepassxc

# Arch Linux
sudo pacman -S keepassxc
```

Once installed:  

1. Create a new **database**.  
2. Choose a strong **master password** (this is the only one you’ll need to remember).  
3. Optionally, add a **key file** for extra protection.  

---

## KeePassXC + Firefox Integration

KeePassXC comes with a **browser integration feature**.  

Steps to enable it in Firefox:  

1. Install the [KeePassXC-Browser extension](https://addons.mozilla.org/firefox/addon/keepassxc-browser/).  
2. In KeePassXC, go to `Tools → Settings → Browser Integration` and enable **Firefox**.  
3. Pair Firefox with KeePassXC (you’ll be prompted with a key exchange).  

Now, whenever you visit a login page, KeePassXC can autofill your credentials. This keeps things both **secure and fast**.

---

## Syncing the Database via GitHub

I keep my password database in a **private GitHub repository**. Why?  
- Easy syncing across multiple devices.  
- Version control for peace of mind.  
- End-to-end security because the database is **encrypted locally**.
- There's 2-step authentication in my GitHub account, so it's another layer of protection.

### Setup:

```bash
# Clone your private repo
git clone git@github.com:your-username/passwords.git

# Move your KeePassXC database into it
mv ~/Documents/passwords.kdbx ~/passwords/

# Commit and push
cd ~/passwords
git add passwords.kdbx
git commit -m "Add KeePassXC database"
git push origin main
```

Now, on another machine, just clone or pull the repo and open the database with KeePassXC.

**Important**: The database is encrypted, but avoid syncing plaintext key files or backups in GitHub. Keep those strictly offline.

---

## Why This Setup Works for Me

- **Open-source tools only** (KeePassXC + GitHub + Firefox).  
- **No vendor lock-in** — I can migrate anytime.  
- **Offline first** — Works without internet.  
- **Secure syncing** with end-to-end encryption.  

It’s not a one-click solution like Bitwarden or 1Password, but if you’re already comfortable with Git and Linux, this setup is hard to beat.

---

## FAQ

### Is KeePassXC safe for Linux users?  
Yes. KeePassXC uses strong AES-256 encryption and is fully open source, meaning its code is audited and trusted by the Linux security community.  

### Why not just use a cloud password manager?  
Cloud services are convenient, but they’re also **centralized targets** for hackers. With KeePassXC, you **own your data** and can sync it however you like.  

### Can I use GitHub for password syncing?  
Yes, but always in a **private repository**. Since the database itself is encrypted, the contents remain secure as long as your master password is strong and also you can use a file (image) with it for more security.  

---

## Final Thoughts

For me, the combo of **KeePassXC + Firefox + private GitHub repo** has been rock-solid.  

If you’re already using Linux, Windows, Mac, give it a try — you might never look back at proprietary password managers again.  

---

**See also**:
- [Accessing Remote Git Repositories on Linux](/accessing-remote-git-repositories-on-linux) 
