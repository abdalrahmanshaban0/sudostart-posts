---
date: '2025-09-03'
draft: false
title: 'Accessing Remote Git Repositories on Linux'
description: 'Learn how to access and manage remote Git repositories on Linux using HTTPS or SSH. Step-by-step guide with commands'
categories: ["Linux"]
tags: ["Linux", "Git"]
slug: "Accessing-Remote-Git-Repositories-on-Linux"
authors:
  - abdo
comments: true
featuredImage: cover.webp
---
Version control is the backbone of modern software development, and Git is its most widely used tool. Whether you’re working solo or collaborating on open-source projects, you need to know how to connect your local Linux machine to remote repositories.  

In this guide, you’ll learn:  
- How to authenticate via HTTPS and SSH  
- How to clone, pull, and push to remotes  

---

## What You Need Before Starting
Make sure you have:  

- **Git installed:**  
  ```bash
  git --version
  ```  
  If missing:  
  ```bash
  sudo apt install git    # Debian/Ubuntu  
  sudo dnf install git    # Fedora  
  sudo pacman -S git      # Arch  
  ```

- A GitHub/GitLab account (or another Git hosting service).  
- Basic knowledge of Linux terminal commands.  

---

## Setting Your Git Identity
Configure your username and email once for all:  
```bash
git config --global user.name "Jane Doe"
git config --global user.email "jane_doe@example.com"
```
Verify settings:  
```bash
git config --list
```

---

## Authentication Methods: HTTPS vs SSH
### SSH (Recommended)
- **URL format:**  
  `git@github.com:user/repo.git`  
- **Pros:** No password prompts after setup.  
- **Steps:**  
  1. Generate a key:  
     ```bash
     ssh-keygen -t ed25519 -C "jane_doe@example.com"
     ```  
  2. Copy your key to clipboard:  
     ```bash
     cat ~/.ssh/id_ed25519.pub
     ```  
  3. Add it to GitHub/GitLab → **Settings → [SSH Keys](https://github.com/settings/keys)**.
  4. Test connection:  
     ```bash
     ssh -T git@github.com
     ```

### HTTPS
- **URL format:**  
  `https://github.com/user/repo.git`  
- **Pros:** Simple to set up.  
- **Steps:**
    1. got to Github **Settings → Developer Settings → Tokens**
    2. Generate a new token and copy it
  ```bash
  git clone https://github.com/user/repo.git

  Username: your-github-username
  Password: <paste your personal access token>
  ```
- **Cons:** May prompt for credentials unless you enable a credential helper:
  ```bash
  git config --global credential.helper store
  ```
---

## Cloning a Remote Repository
To download a repository:  
```bash
git clone https://github.com/user/repo.git
# or using SSH:
git clone git@github.com:user/repo.git
```
This creates a project directory containing all files and commit history. You can add ``--recursive`` after ``clone`` if the repository you are cloning contains submodules.

---

## Linking a Local Project to a Remote
If you already have a project and want to push it online:  
```bash
git init
git remote add origin https://github.com/user/repo.git
git add .
git commit -m "Initial commit"
git push -u origin main
```

---

## Fetching, Pulling, and Pushing

- **Fetch:** Download remote changes without merging.  
  ```bash
  git fetch origin
  ```

- **Pull:** Download and merge changes into your branch.  
  ```bash
  git pull origin main
  ```

- **Push:** Upload your local changes.  
  ```bash
  git push origin main
  ```

