---
title: "First contribution to Qiskit Metal"
categories:
  - Project
tags:
  - Project
  - Qiskit
  - Metal
  - Cloud
  - WSL
last_modified_at: 2021-10-04
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

I sent a PR as a "draft" to Qiskit Metal for the first time:
[Execute import qiskit_metal without PySide2 #713](https://github.com/Qiskit/qiskit-metal/pull/713).

## The reason why I contributed

I joined in this project, [cloud-ready](https://github.com/qiskit-advocate/qamp-fall-21/issues/16), related to my first advocate activity, QAMP.
The mentor, Marco, specified several things of the project and I had chosen an import issue as a top priority among them.
It is an import error and this occurs when executing `import qiskit_metal` in the cloud environment.
Our main project is providing Qiskit Metal as a service, similar to Composer or Lab in IBM Quantum: https://quantum-computing.ibm.com/.
Therefore, it is quite critical error which must be solved.

## How I figured out the issue

First of all, the import error is from PySide2 and I found it occurs on linux environment:
[pyside2 installation problem on ubuntu18.04](https://stackoverflow.com/questions/63186438/pyside2-installation-problem-on-ubuntu18-04-python-3-8-3-on-anaconda).
The goal is to execute `import qiskit_metal` without an error so I found all `PySide` in the files of Qiskit Metal.
*I have used mainly WSL (Windows Subsystem for Linux) on ubuntu 18.04.

```
grep -r 'PySide' .
```

![grep](https://user-images.githubusercontent.com/62553200/135808167-83631dcc-c9e7-47fe-94f8-fd174a58ec4c.png)

I simply made all the lines to captions in `__init__.py`, linked to the files above.
However, this method broke the whole program. (Captioned all in the end...)
I need to find other ways.

Secondly, I deleted the folder `_gui`
and removed lines which have the word, `_gui` with the command below.

```
sed -i '/_gui/d' <file_name>
```

This command remove lines which have the word `_gui` only and overwrite the file.
I also used this with the word, PySide.
*If you want to remove files including a specific word, execute the command below.

```
rm -f grep -r "<the specific word>" * -l`
```

Doing by trial and error, I found that I have to modify just four files to execute without PySide2.
(Details are on my PR.)
Lastly, I ran `import qiskit_metal` without PySide2.
I used Anaconda so I do not have any error with PySide2 in my local.
Hence, I removed it and then checked other programs work well.
And... `import qiskit_metal` executed fine. No errors!

*This PR must be fixed to merge, but I just want to leave a log for the FIRST offical PR.

---

💬 _Any comments and suggestions will be appreciated._
