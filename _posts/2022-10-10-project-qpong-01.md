---
title: "Let's make QPong more awesome!"
categories:
  - Project
tags:
  - Project
  - Qiskit
  - PICO-8
last_modified_at: 2022-10-10
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

I am participating in [QPong 2.0](https://github.com/qiskit-advocate/qamp-fall-22/issues/26) of the QAMP Fall 22.

It is in the sprint 3 now and the main goal of this sprint is to write a blog what I understood in the sprint 2.
There are three things to share:

1. How Python server code works: [dpallot/simple-websocket-server](https://github.com/dpallot/simple-websocket-server/)
2. What PICO-8 is and how to make a game with it: [qpong.p8](https://github.com/QPong/QPong-PICO-8/blob/master/qpong.p8)
3. How to create a multiplayer game with PICO-8: [picario-server](https://github.com/zacharypetersen1/picario-server)

If you are curious what is happened in the sprint 1, I highly recommend to check our presentation of checkpoint 1 through
[LinkedIn](https://www.linkedin.com/posts/huangjunye_we-had-the-checkpoint-1-presentations-for-activity-6984098523977797632-mKKi?utm_source=share&utm_medium=member_ios)
or [Twitter](https://twitter.com/HuangJunye/status/1578321075418501120?s=20&t=eUQp_l72nPxJFUdmz8c5dQ)!

## How Python server code works

I will explain how Python server code works based on this repository:
[dpallot/simple-websocket-server](https://github.com/dpallot/simple-websocket-server).

If you look into it, there are one folder and two files related to PyPl package:
`MANIFEST.in` and `setup.py`.
The PyPl package is a kind of a storage for using `pip install`.
When you type this command with a random package you would like to install,
it searches the package in [PyPl](https://pypi.org/) and installs it in the library folder, if it exists.

Therefore, it is good to upload your Python package in PyPl if you hope to distribute your project in an efficient way.
There is also a tutorial how to upload a package with your Python project:
[Packaging Python Projects](https://packaging.python.org/en/latest/tutorials/packaging-projects/).
I would like to create a Python package after our QPong server is ready.

Now let's check in the folder `SimpleWebSocketServer`.
There are 5 files and you can check what each file means shortly.

- `__init__.py`: it is for making the package.
- `SimpleExampleServer.py`: it is a main server file. Using the `--ssl` option, you can choose to turn on or off it.
  - `SimpleWebSocketServer.py`: all the functions are in here for sending and receiving messages.
- `SimpleHTTPSServer.py`: it is an example to make a HTTPS server with Python.
- `websocket.html`: after the server is on, you can use this file to test the server is working well or not.

If you give no options, the server is working as a simple echo server.

## What PICO-8 is and how to make QPong with it

PICO-8 is a paid tool to make a game easily.
(You can try PICO-8 through the education version: [www.pico-8-edu.com](https://www.pico-8-edu.com/).)
The code for it is written in the programming language, [Lua](https://www.lua.org/).

I don't know this language so I think it is great to take a look
[qpong.p8](https://github.com/QPong/QPong-PICO-8/blob/master/qpong.p8) code first.
In the file, a part of Qiskit code is included. At first I was surprised because Qiskit is orginally written in Python, not Lua.
After one of bi-weekly meetings, I knew that there is a repository called
[MicroQiskit](https://github.com/qiskit-community/MicroQiskit/tree/main/versions/Lua)
and it contains a lot of Qiskit codes using other languages.

## How to create a multiplayer game with PICO-8

It is important to figure out if the multiplayer mode is available in PICO-8
before determining the certain platform our team will focus on.
While exploring PICO-8 games, I found a multiplayer game that we can refer while converting the current QPong version:
[zacharypetersen1/picario-server](https://github.com/zacharypetersen1/picario-server).

To execute the above game, you should run `python BaseServer.py` first and open `index.html`.
After connecting to the server, it loads `picario.js` so `picario.p8` is not directly needed.
What's more, in `index.html`, there is a line `window.setInterval(update, 10);` and it means
the game is updated line by line every 10 milliseconds.
Then now you are wondering how to transmit the current position to the server.
To understand the network part, you may check the function `ReadPico8` in `index.html` and
the function `attempt_send` in `picario.p8`.

## References

- [Difference between 'python setup.py install' and 'pip install'](https://stackoverflow.com/questions/15724093/difference-between-python-setup-py-install-and-pip-install)
- [Game Design and Development: PICO-8](https://eugene.libguides.com/gamedesign/pico8)
- [PICO-8 Poke](https://pico-8.fandom.com/wiki/Poke)

---

💬 _Any comments and suggestions will be appreciated._
