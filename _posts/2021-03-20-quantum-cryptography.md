---
title: "What is quantum cryptography?"
categories:
  - Cryptography
tags:
  - Cryptography
  - Quantum Computing
  - RSA
  - Quantum Key Distribution
last_modified_at: 2021-03-20
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

Let's talk about quantum cryptography focused on quantum key distribution.<br/>

This page is based on a presentation, [Quantum Cryptography](https://www.slideshare.net/DayeongKang/quantum-cryptography-244773074),
and a book, [Quantum Computing Explained (translated in Korean)](http://acornpub.co.kr/book/quantum-explained).
Please check the presentation if you do not have enough time to read this whole page.
Then shall we start?

## 1. Key distribution

Key distribution is important in cryptography. The key is a main thing to encrypt and decrypt messages.<br/>

There is an easy example to understand how a key works: Caesar cipher.
This one has a same key to encrypt and decrypt.
If you want to encrypt a message, just plus the key value to each character.
Minus the key value to decrypt.
The picture below shows how this method works where the key value is 3.<br/>

![Caesar](https://user-images.githubusercontent.com/62553200/111858025-069e0700-8979-11eb-81dd-d2a0e539933e.PNG)

Alice send `EDG` to Bob. Bob will know the key is 3 and get the real message, `BAD`, right away.
At first, Eve cannot understand Alice's message.
However, she will know somehow what the key is while analysing messages and
it is true this encryption is really easy to break: just use brute forcing with random key values in 1 to 25. (Alphabet length is 26.)<br/>

Therefore, we should use more difficult encryption that is hardly breakable.

## 2. RSA: use mathematical algorithms

![rsa](https://user-images.githubusercontent.com/62553200/111858220-a9a35080-897a-11eb-9507-ea0e0a73a330.PNG)

RSA is a public-key cryptosystem, so it uses two keys. Encrypt the message with public key and decrypt it with private key.
I will skip the details because it is not the main part.
If you want to know how to generate these two keys,
please check my til: [RSA algorithm](https://github.com/tula3and/til/blob/master/Cryptography/RSA-algorithm.md#rsa-algorithm).

## 3. Quantum key distribution: use quantum mechanics

![qkd](https://user-images.githubusercontent.com/62553200/111858393-f3406b00-897b-11eb-88bc-d2320a0c88f8.PNG)

Quantum key distribution (QKD) uses not only quantum communication but also classical one.
The former is for QKD and the latter is for comparing base each other.
These base are different on each protocol.
There are three protocols: BB84, B92, and B91.
All the protocols are based on three principles: (1) no-cloning theorem, (2) wave function collapse,
and (3) irreversibility of measurement.

### (1) BB84

![슬라이드10](https://user-images.githubusercontent.com/62553200/111858635-d311ab80-897d-11eb-9832-cc390d7371d0.PNG)

Alice and Bob choose between two bases for each qubit and compare their base.
If their base are different, remove the result of that position.
The remaining result is a final key called shifted key.
After they make shifted keys, they should compare that two keys are same.
If error rate is too high, send a new quantum key again.
(High error means Eve transformed the quantum key with measurement while distributing.)

### (2) B92

![슬라이드11](https://user-images.githubusercontent.com/62553200/111858636-d442d880-897d-11eb-8465-773911af4ca7.PNG)

The number of base in B92 protocol is same as BB84, but the result states are divided simply 2, not 4.
It is much simple than previous one because Alice and Bob do not have to compare their base.
In this protocol, only Bob send positions of `|1>` and `|1'>` after his measurement.

### (3) B91

![슬라이드12](https://user-images.githubusercontent.com/62553200/111858637-d442d880-897d-11eb-8893-a7e5aec1aceb.PNG)

B91 use two bell states as bases. It is same as BB84 at comparing base and the number of result states is same as B92.
The results are exactly same each other after removing the positions which base are different. (So this one is the simplest!)

## References

- [David McMahon: Quantum Computing Explained (ISBN 9780470096994) (translated in Korean)](http://acornpub.co.kr/book/quantum-explained)
- [My til: RSA algorithm](https://github.com/tula3and/til/blob/master/Cryptography/RSA-algorithm.md#rsa-algorithm)
- [Wikipedia: Bell state](https://en.wikipedia.org/wiki/Bell_state)

---

💬 _Any comments and suggestions will be appreciated._
