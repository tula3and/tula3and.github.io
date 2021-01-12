---
title: "Connect Unity projects with Github"
categories: 
  - Github
tags:
  - Github
  - Unity
  - LFS
  - Sourcetree
last_modified_at: 2020-09-14
author_profile: true
sitemap :
  changefreq : daily
  priority : 1.0
---
While I uploaded Unity project files, I faced a error.<br/>
"`this is larger than GitHub's recommended maximum file size of 50.00 MB`"<br/>

The solution for this error is using [Git Large File Storage](https://git-lfs.github.com/), in short, Git LFS.<br/>
In this post, I will share two ways to connect Unity projects with Github; Git and Sourcetree.<br/>

Before starting, (1) download [Git LFS](https://git-lfs.github.com/) and (2) create a new repository with `Unity .gitignore`.

![git_ignore.png](https://user-images.githubusercontent.com/62553200/93023833-c017ca80-f62c-11ea-9073-2dc791c8e503.png)

## 1. Git

(For Mac, Linux) Open terminal and move to your project folder.<br/>
(For Windows) Open Git Bash at your project folder.<br/>

Then, use the following commands on your environment.<br/>
Replace `<Github URL>`, `<file type>`, and `<folder name>` with your own options.

```
$ git init
$ git remote add origin <Github URL>
$ git pull origin master
```

```
$ git lfs install

$ git lfs track "*.<file type>"
$ git lfs track "<folder name>/**"

$ git add .gitattributes

$ git commit -m "Add gitattributes"
$ git push -u origin master
```

Using `git lfs track`, choose large files or the whole folder.<br/>
I use only `git lfs track "Assets/**"` for [my project](https://github.com/tula3and/pie_planet).

```
$ git add .
$ git commit -m "Add all the files"
$ git push
```

Then you can see files on Github.

## 2. Sourcetree

Connect a empty local repository with your Github repository.<br/>
Go to top bar; Repository → Git LFS → Initialise Repository.<br/>
Add using `file type` or `folder name/**`.<br/>

Then, you just commit the changes and check files on Github.<br/>

---

💬 *Any comments and suggestions will be appreciated.*
