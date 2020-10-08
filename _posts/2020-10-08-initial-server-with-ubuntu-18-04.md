---
title: "Initial Server with Ubuntu 18.04"
categories:
  - notes
tags:
  - server
  - ubuntu
---
## 1. Logging in as Root
```bash
ssh root@server_ip
```

## 2. Creating a New User
```bash
adduser luat
```
luat is my username (you need to hange to your)
You need to type password and some information.

## 3. Granting Administrative Privileges
To allow my user to run commands with administrative privileges, I add the new user to the sudo group:
```bash
usermod -aG sudo luat
```

## 4. Setting Up a Basic Firewall
See list UFW profile:
```bash
ufw app list
```
Allow SSH connection after enable ufw firewall:
```bash
ufw allow OpenSSH
```
Enable firewall:
```bash
ufw enable
```
You can see OpenSSH has been allowed by ufw:
```bash
ufw status
```

## 5. Sync ssh public key from root account
If when create VPS you choose to use SSH key authentication, now you can use rsync to copy .ssh folder to current user:
```bash
rsync --archive --chown=luat:luat ~/.ssh /home/luat
```

Now you can ssh to your server by new user:
```bash
ssh luat@server_ip
```
