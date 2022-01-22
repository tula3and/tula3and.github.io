---
title: "Qiskit을 경험해보기 전에"
categories:
    - Qiskit
tags:
    - Qiskit
    - Quantum Computing
sidebar:
    - nav: Qiskit Tutorial
last_modified_at: 2020-10-24
author_profile: true
sitemap:
    changefreq: daily
    priority: 1.0
---

Qiskit을 경험해보기 전에, 양자 컴퓨팅에 대해서 살짝 알아봅시다.

## 1. Qubit

기존 컴퓨터의 기본 단위는 `비트(bit)`로 0 또는 1, 무조건 둘 중의 하나의 상태로만 존재합니다.<br/>
반면에 양자 컴퓨터의 세계에서는 `Quantum Bit`, 줄여서 `Qubit`이라 불리는 것을 기본 단위로 사용합니다.<br/>

같은 비트임에도 Qubit은 비트처럼 0 또는 1 중 하나만으로는 존재할 수는 없습니다.<br/>
이 이유는 바로 양자 세계의 `중첩(superposition)`이라는 특성 때문인데요.<br/>

> 중첩(superposition)은 0과 1의 두 가지 상태가 공존한다는 뜻입니다. 그래서 관측(measurement)하기 전까지는 어떤 상태인지 알 수 없습니다.
> 이런 요상한 상태를 설명하기 위해 슈뢰딩거의 고양이(Schrödingers Katze)가 주로 사용됩니다.

그럼 도대체 Qubit은 어떤 상태로 존재한다고 말해야할까요?

## 2. 확률 진폭(probability amplitude)

이러한 Qubit의 상태를 표현하기 위해 `확률`의 개념이 도입됩니다.<br/>
이때의 확률이란 건 0과 1에 부여된 각각의 `확률 진폭(probability amplitude)`을 이용해서 구할 수 있습니다.

확률 진폭 $r$은 하나의 복소수 값으로,
$r = a + bi$의 형태로 나타낼 수 있습니다.<br/>
확률 진폭의 켤레 복소수를 이용하여, 확률 $p$를 구합니다.<br/>

$$
p = || r ||^2 = (a+bi)(a-bi) = a^2+b^2
$$

위의 수식으로 확률 진폭의 값이 $a+bi$일 때,
확률의 값은 $a^2+b^2$인 것을 알 수 있습니다.<br/>

예를 들어 확률 진폭을 &alpha;, 그에 해당되는 상태가 0이라고 가정해봅시다.<br/>
이런 상태를 보통 [bra-ket notation](https://en.wikipedia.org/wiki/Bra%E2%80%93ket_notation)을 이용해
$\alpha|0>$으로 나타냅니다.<br/>

그래서 Qubit $\psi$에 대해 아래와 같이 두 가지 방법으로 나타낼 수 있습니다.<br/>

$$
|\psi> = \alpha|0> + \beta|1>
$$

$$
|\psi> = \binom{\alpha }{\beta }
$$

위와 같은 `ket` 형식이 아닌 `bra` 형식으로 나타낼 경우, 켤레 복소수로 취급됩니다.<br/>

$$
<\psi|=\binom{\alpha }{\beta }^\dagger = \begin{pmatrix}
 \alpha ^\dagger & \beta^\dagger
\end{pmatrix}
$$

따라서 `bra`와 `ket` 형식이 백터 곱으로 나타날 경우 다음과 같이 계산 됩니다.<br/>

$$
<\psi|\cdot |\psi> = <\psi|\psi> = \begin{pmatrix}
 \alpha ^\dagger & \beta^\dagger
\end{pmatrix}\cdot \binom{\alpha }{\beta } = \alpha^\dagger\alpha + \beta ^\dagger\beta
$$

$$
|\psi>\cdot<\psi| = |\psi><\psi| = \binom{\alpha }{\beta } \cdot \begin{pmatrix}
 \alpha ^\dagger & \beta^\dagger
\end{pmatrix} = \begin{pmatrix}
\alpha\alpha^\dagger & \alpha\beta^\dagger\\
\beta\alpha^\dagger & \beta\beta^\dagger
\end{pmatrix}
$$

## 3. 이제 Qiskit을 설치합시다!

> Qiskit 유튜브 채널의 [How to Install Qiskit](https://youtu.be/M4EkW4VwhcI)을 참고하면 쉽게 설치할 수 있습니다.<br/>

저는 Windows를 사용 중이라, [Anaconda](https://www.anaconda.com/products/individual)를 설치하였습니다.<br/>
설치가 다 되었다면 Anaconda prompt를 열어서 아래 명령을 입력해주세요.

```
pip install qiskit
```

> Anaconda의 가상 환경 관련 명령은 [이 문서](https://github.com/tula3and/til/blob/master/Qiskit/Anaconda.md#using-anaconda)를 참고해주세요.

Qiskit이 설치 완료되면 Jupyter Notebook을 열어서 설치가 잘 되었는지 확인해봅시다.

```
jupyter notebook
```

```
import qiskit
qiskit.__qiskit_version__
```

설치가 잘 되었다면 [Qiskit의 최신 버전](https://github.com/Qiskit/qiskit)이 출력될 것입니다.<br/>

---

💬 _Any comments and suggestions will be appreciated._
