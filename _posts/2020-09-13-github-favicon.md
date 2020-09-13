---
title: "Let's add a favicon to GitHub pages"
categories: 
  - Github
tags:
  - Github
  - Favicon
  - HTML
last_modified_at: 2020-09-13
author_profile: true
---

## 1. What is a favicon?

When you open a website, you can see a icon in tabs. It is a favicon. I bring a screenshot of [Google main page](https://www.google.co.kr/).

![google_favicon.png](https://user-images.githubusercontent.com/62553200/93010963-f02f8100-f5cc-11ea-8015-7f0ed74ee7c9.png)

## 2. How to make a favicon?

There are lots of sites for generating favicon.ico. I use [this site](https://www.favicon-generator.org/) to make my favicon.

![generator.png](https://user-images.githubusercontent.com/62553200/93010971-1523f400-f5cd-11ea-80f4-c5184d3d496e.png)

(1) In the first block, click it and unzip the file. I put images to `assets / logo.ico`.

(2) In the second block, copy all the lines and paste to `_includes / head.html`. Then, you should add `{{site.baseurl}}/assets/logo.ico` after the codes, `href=`.

![tula_favicon.png](https://user-images.githubusercontent.com/62553200/93010976-1a813e80-f5cd-11ea-84bd-ea87f1440abb.png)

After commit all these things, you can check your beautiful favicon!
