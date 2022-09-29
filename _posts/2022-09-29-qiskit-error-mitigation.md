---
title: "What is quantum error mitigation?"
categories:
    - Qiskit
tags:
    - Qiskit
    - Quantum Computing
last_modified_at: 2022-09-29
author_profile: true
sitemap:
    changefreq: daily
    priority: 1.0
---

Let’s get to know what quantum error and quantum error mitigation are, with a small cartoon!

## Have you heard of quantum error?

You know the word, noise. It is something like unpleasant or that causes disturbance.
In the quantum computer, the concept of the noise is same as you imagine. Something is not that good.
Let's think about the "normal" computer.
You request a calculation to the computer and surely, you wouldn't doubt its output.
However, if you are not 100% sure that the result is correct, can you say the calculation process is meaningful?

The computer is powerful because we can believe its output!
However, quantum computers are still not safe for this kind of belief
because we are living in the noisy intermediate-scale quantum (NISQ) era.
Any result from real quantum computer (or hardware) has a noise so the result is different from what you expected.

It is completely fine that you already know the right result but mostly,
we use computers to solve problems that we do not know the correct answer.
Thankfully, there is a way to decrease the noise.
That's what error mitigation is for!
We can reduce the noise with error mitigation and there are various techniques which are still under research.

## Difference between error mitigation and error correction

You also may be heard the word, error correction, instead of error mitigation.
They look similar but definitely distinct. I prepared images to understand their difference all at once.

<img src="https://user-images.githubusercontent.com/62553200/192946078-abc71897-c7f0-4405-affe-6e627eb90324.png" width="500">

There is one pitiful person under the rain.
Suppose that all the raindrops are the quantum noise.
Then you can grasp the meaning of error mitigation and error correction with the images below.

![two_situations](https://user-images.githubusercontent.com/62553200/192946073-627daed3-9a1d-4795-9f3b-050fbf004880.png)

Raindrops are blocked by the umbrella, but some of them still matter.
On the other hand, all of them are perfectly blocked by the purple car.
It is simply explained what error mitigation and error correction are.

Error mitigation is for suppressing errors and error correction is for correcting errors,
including suppressing them. Then you may curious why we use error mitigation, instead of error correction.
Ideally, error correction is awesome but it requires too many qubits to make it happen.
Therefore, error mitigation is preferred because it is ealistically possible method for now.

## Techniques for error mitigation

There are a lot of error mitigation techniques.
In this post, I will introduce two of them; M3 and DD.

M3 is matrix-free measurement error mitigation.
When you execute a quantum circuit in a certain quantum hardware,
you can get a matrix $\vec{P}$ as a result. If you know the ideal result $\vec{Q}$, you can calculate a matrix $\vec{A}$ defined in the below.

$$
\vec{P} = \vec{A} \cdot \vec{Q}
$$

From $\vec{A}$, you can compute $A^{-1}$ and it means you can find an ideal result of any result from this quantum hardware.
However, the bigger the matrix size, the more difficult it is to find $A^{-1}$.
It's a good time to apply M3 to lower the matrix size for easy calculation.

DD is dynamical decoupling.
This method helps to maintain states of idle qubits.
If a qubit is idle, its information is lost by interactions with the surroundings.
In this situation, you may think about applying DD; it puts several quantum gates to idle qubits.
Note that any series of newly added gates should not change the original intended outcome.

For detailed explanation, you may check this two videos from Nick Knows of Qiskit Youtube.

- [Nick Knows - Measurement Errors (M3)](https://youtu.be/9ZSBkH-2zjs)
- [Nick Knows - Dynamical Decoupling](https://youtu.be/67jRWQuW3Fk)

## References

- [QGSS 2021 - Noise in Quantum Computers (part 1)](https://learn.qiskit.org/summer-school/2021/lec3-1-noise-quantum-computers-1)
- [Quantum Error Correction - Todd A. Brun](https://arxiv.org/pdf/1910.03672.pdf)
- [Qiskit documentation - M3](https://qiskit.org/documentation/partners/mthree/)
- [Qiskit documentation - DD](https://qiskit.org/documentation/stubs/qiskit.transpiler.passes.DynamicalDecoupling.html#dynamicaldecoupling)

---

💬 _Any comments and suggestions will be appreciated._
