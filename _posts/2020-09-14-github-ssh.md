---
title: "Generate a new SSH key and add it to Github"
categories: 
  - Github
tags:
  - Github
  - Security
  - SSH
  - Sourcetree
last_modified_at: 2020-09-14
author_profile: true
---
Let's generate a SSH key for Github.<br/>
Then add it to Github and connect it with Sourcetree.<br/>

## 1. Check if a SSH key exists

(For Mac, Linux) Open terminal.<br/>
(For Windows) Open Git Bash.<br/>

Then, type `ls -al ~/.ssh` on your environment.<br/>
If `id_rsa` and `id_rsa.pub` exist, you can pass step 2.

## 2. Generate a new SSH key

```
$ ssh-keygen -t rsa -b 4096 -C "<your email>"
```
You can see `Generating public/private rsa key pair.` and finish making your SSH key.

## 3. Copy the public key

(For Mac) `pbcopy < ~/.ssh/id_rsa.pub`<br/>
(For Linux) (1) `xclip -sel clip < ~/.ssh/id_rsa.pub` or (2) `cat ~/.ssh/id_rsa.pub` and copy the output.<br/>
(For Windows) `cat ~/.ssh/id_rsa.pub | clip`

## 4. Add to Github

Log in Github → Settings → SSH and GPG keys → New SSH key<br/>
Then, paste your public key to the box.<br/>

If you don't use Sourcetree, you have finished it!

## 5. Connect it with Sourcetree

Tools → Options<br/>
Below SSH Client Configuration, change SSH Client to `OpenSSH`.

---

💬 *Any comments and suggestions will be appreciated.*
