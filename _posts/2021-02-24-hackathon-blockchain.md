---
title: "Blockchain with Qiskit: Qoupang"
categories:
  - Hackathon
tags:
  - Hackathon
  - Project
  - Qiskit
  - Blockchain
  - Presentation
last_modified_at: 2021-03-06
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

I participated in Qiskit Hackathon Korea, from February 16 to February 19.<br/>
Thankfully, our project, Qoupang, got the Community Choice Award!🎉<br/>
The entire code can be found in the github repository shared: [qoupang](https://github.com/tula3and/qoupang).<br/>

In this project, I worked as a project manager.<br/>
Therefore, I delivered [the presentation](https://www.slideshare.net/DayeongKang/qoupang) of the project and made a QRNG also!<br/>

## Motivation of this project

1. Security issues in logistic
2. The noise problem in NISQ era: noise is good to generate random numbers.
3. Real blockchain use cases with quantum computing
4. Contribution to the Qiskit community: for SWAG delivery

## Our work

We used [Goorm IDE](https://ide.goorm.io/) to share codes.<br/>

First, build a webpage to get an email. This page includes two pop-up windows:
to display the loading screen while building a block and the success screen.
The TxHash value shows in the second one.
This value is really important because you can see the real transaction in Klaytnscope using the value.<br/>

Second, build a QRNG to make hash values.
I worked on this part. The main circuit is based on this paper:
[Quantum random number generators with entanglement for public randomness testing](https://www.nature.com/articles/s41598-019-56706-2).
I usually do not use real backends in Qiskit because of long job queues.
However, I could use them in this Hackathon.
One of our team's mentors has the internal source of IBMQ, so I can use it during the hackathon.
(48 qubits were amazing.)<br/>

Finally, connect frontend to backend.
This step is divided into four small parts:
(1) get an email from frontend,
(2) connect hash values and the email for key-value,
(3) send all the information to write a transaction in Klaytn,
(4) Update the status of each data.
As you can see the name, Klaytn, in (3), we used [Klaytn API Service](https://www.klaytnapi.com/ko/landing/main)
in this project.
(These codes (related blockchain) were written in node.js.)

---

If you want to see how it works, check this video on Youtube: [Qoupang](https://youtu.be/nM2pvhyPBj4).<br/>

Check my previous Qiskit Hackathon project also: [quantum-ugly-duckling](https://tula3and.github.io/hackathon/hackathon-discord/).<br/>
A cute ugly duck became a delivery duck for Qoupang😆

---

💬 _Any comments and suggestions will be appreciated._
