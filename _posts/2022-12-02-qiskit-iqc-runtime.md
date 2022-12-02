---
title: "Getting close to Qiskit Runtime"
categories:
    - Qiskit
tags:
    - Qiskit
    - Quantum Computing
    - Open Source
last_modified_at: 2022-12-02
author_profile: true
sitemap:
    changefreq: daily
    priority: 1.0
---

Let's understand Primitives in Qiskit Runtime with a summary of Lab 1 in the IBM Quantum Challenge Fall 2022.

Finally [the IBM Quantum Challenge Fall 2022](https://tula3and.github.io/experience/ibm-quantum-challenge-fall/#) is finished! 🥳
Every challenge is always interesting, but this time is even more than the usual.
The reason why is that I joined as one of team members preparing this event.
I contributed 3 labs among 4 labs in total.
Especially, I mainly focused on the first lab so here I would like to share the contents of lab 1:
Introduction to Primitives on Qiskit Runtime.

## Introduction to Primitives

In Qiskit Runtime,
Primitives are a kind of basement of the program.
There are two Primitives for now: the Sampler and the Estimator.
Which primitive to use totally depends on your purpose.

The Sampler is for sampling probability distributions so the result might look very similar
to the one when you receive after executing quantum circuits.
However, one difference is that the Sampler result is called quasi-probability
because it is possible to apply error mitigation techniques in Qiskit Runtime.
In lab 2, you can check examples for the Sampler in regard to quantum machine learning.
There are also a few use cases of the Sampler in [Tutorials](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials.html#sampler).

The Estimator is for calculating expectation values of quantum operators,
so it takes one more thing comparing to the Sampler: observables.
You can set these observables using [SparsePauliOp](https://qiskit.org/documentation/stubs/qiskit.quantum_info.SparsePauliOp.html).
For more details on expectation values, in Qiskit,
z-axis measurement is the default so some gates are added for measurement with operators `X` and `Y`.

$$
\langle \psi | Z | \psi \rangle
$$

$$
\langle \psi | X | \psi \rangle = (H | \psi \rangle)^\dagger Z (H | \psi \rangle) = \langle \psi | HZH | \psi \rangle
$$

$$
\langle \psi | Y | \psi \rangle = (HS^\dagger | \psi \rangle)^\dagger Z (HS^\dagger | \psi \rangle) = \langle \psi | SHZHS^\dagger | \psi \rangle
$$

If you send a job with the Estimator,
it will append additional operators based on the observables.
In lab 3 and 4, you can check examples for the Estimator and also in
[Tutorials](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials.html#estimator).

## How to use Primitives in Qiskit Runtime

Currently, the Session style is recommended in Qiskit Runtime.
For this, you can follow the steps below:

1. Set a proper Qiskit Runtime service and a backend for executing your routine.
2. Create a `Session` with the service and the backend.
3. Make instances with Primitives inside the Session; the `Sampler` and the `Estimator`.

With these three things, you can execute any quantum circuit whatever you want. I left example codes for each Primitive in the followings.

```python
with Session(service=service, backend=backend):
    sampler = Sampler(options=options)
    job = sampler.run(circuits=[qc] * 50)
```

```python
with Session(service=service, backend=backend):
    estimator = Estimator(options=options)
    job = estimator.run(
      circuits=[qc]*len(individual_phases),
      parameter_values=individual_phases,
      observables=[op]*len(individual_phases)
    )
```

You can check more examples in the main repository of the IBM Quantum Challenge Fall 2022:
[Lab 1 notebook](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22/blob/main/content/lab-1/lab1.ipynb).

## Easy to adapt error mitigation techniques

In Qiskit Runtime,
one of the unique functionalities is that you can add error mitigation techniques so easily.
To show how these techniques work, a noise model is introduced in the challenge.
It mimics the real backend so it enables to apply the noise to the ideal simulator and execute quickly without a long queue time.

You just need to add a `Options` while initializing Primitives.
In the `Options`, you can set values for `resilience_level` and `optimization_level`.
Which error mitigation technique to apply depends on your `Options` setup.

```python
options = Options()
options.resilience_level = 0 # No error mitigation
#options.resilience_level = 1 # M3 in the Sampler
#options.resilience_level = 1 # T-REx in the Estimator
#options.resilience_level = 2 # ZNE in the Estimator
#options.resilience_level = 3 # PEC in the Estimator
options.optimization_level = 0 # No optimization
#options.optimization_level = 3 # Dynamical decoupling
```

In total, there are five available techiniques: DD, M3, T-REx, ZNE, and PEC. If you want to read more details, you can check the official
[Tutorials](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/Error-Suppression-and-Error-Mitigation.html).

## References

- [IQC Fall 2022 - Lab 1 solution](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22/blob/main/solutions-by-authors/lab-1/lab1.ipynb)
- [Mesurement error mitigation using M3](https://quantum-enablement.org/how-to/mitigation/M3/m3_mitigation.html)

---

💬 _Any comments and suggestions will be appreciated._
