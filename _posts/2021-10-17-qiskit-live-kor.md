---
title: "Qiskit 최적화와 머신 러닝 맛보기"
categories:
  - Qiskit
tags:
  - Qiskit
  - Quantum Challenge
  - Quantum Computing
  - Optimization
  - Machine Learning
last_modified_at: 2021-10-17
author_profile: true
sitemap:
  changefreq: daily
  priority: 1.0
---

본 포스팅은 IBM Quantum Challenge Fall 2021을 위해 준비된 Qiskit 유튜브 라이브 세션 Part 1:
[Qiskit Optimization & Machine Learning Demo Session with Atsushi Matsuo & Anton Dekusar](https://youtu.be/claoY57eVIc)을 보고 작성된 내용입니다.

## IBM Quantum Challenge Fall 2021

IBM Quantum Challenge Fall 2021은 10월 27일부터 11월 6일까지 진행될 예정입니다.
참가 신청은 이 링크에서 가능합니다: [https://challenges.quantum-computing.ibm.com/fall-2021](https://challenges.quantum-computing.ibm.com/fall-2021).
이번 Quantum Challenge 다루게 될 주제는 Qiskit 금융과 네이처, 머신 러닝, 최적화로 총 4개입니다.
아래는 각 주제의 튜토리얼 링크를 첨부해두었습니다.

- Qiskit 금융: [https://qiskit.org/documentation/tutorials/finance/index.html](https://qiskit.org/documentation/tutorials/finance/index.html)
- Qiskit 네이처 : [https://qiskit.org/documentation/nature/tutorials/index.html](https://qiskit.org/documentation/nature/tutorials/index.html)
- Qiskit 머신 러닝: [https://qiskit.org/documentation/machine-learning/tutorials/index.html](https://qiskit.org/documentation/machine-learning/tutorials/index.html)
- Qiskit 최적화: [https://quantum-computing.ibm.com/lab/docs/iql/optimization](https://quantum-computing.ibm.com/lab/docs/iql/optimization)

Quantum Challenge 참가자들을 위해 Qiskit 유튜브 채널을 통해 두 번의 라이브 세션이 진행되었습니다.

- Part 1: [Qiskit Optimization & Machine Learning Demo Session with Atsushi Matsuo & Anton Dekusar](https://youtu.be/claoY57eVIc)
- Part 2: [Qiskit Nature & Finance Demo Session with Max Rossmannek & Julien Gacon](https://youtu.be/UtMVoGXlz04)

이번 포스팅에서는 첫 번째 세션을 보고 정리한 내용을 공유해보고자 합니다.

## Qiskit 최적화

최적화는 가능한 해답 중에서 가장 최적의 해답을 찾는 것을 의미합니다.
최적화의 대상이 되는 함수를 목적함수(objective function)라 하고,
각 문제에 따라 결과 값을 최소화 또는 최대화를 해야 합니다.
간단하게 생각하자면, 최적화 문제들은 여행 계획을 세우는 것과 비슷합니다.
여행지에서 가능한 선택지 모두를 분석해 세부 사항을 나열하고,
"최소화"된 비용으로 "최대화"된 이익을 즐길 수 있도록 고민합니다.

문제가 간단할 때는 이런 고민을 단순 비교로도 해결할 수 있지만, 복잡해질수록 어떤 게 최적인지 판단하는 것이 점점 더 어려워질 것입니다.
쉽게 판단이 어려운 여러 문제들을 해결하기 위해 최적화가 등장한 것입니다.
이제 Qiskit 최적화를 이용해 대표적인 최적화 문제를 풀어봅시다.

### Quadratic programs

Quadratic program은 목적함수가 이차식인 최적화 문제입니다.
아래 코드에서는 `2x + y + z`의 최댓값을 찾는 목적함수를 정의하고 있습니다.

```python
from qiskit.optimization import QuadraticProgram
# Define QuadraticProgram
qp = QuadraticProgram()
# Add variables
qp.binary_var('x')
qp.binary_var('y')
qp.integer_var(lowerbound=0, upperbound=7, name='z')
# Add an objective function 
qp.maximize(linear={'x': 2, 'y': 1, 'z': 1})
# Add a constraint
# xyz_leq: x + y + z <= 2
qp.linear_constraint(linear={'x': 1, 'y': 1, 'z': 1}, sense='LE', rhs=2, name='xyz_leq')
print(qp.export_as_lp_string())
```

그러나 이렇게 `QuadraticProgram`으로 정의된 문제를 양자 알고리즘으로 바로 다룰 수는 없습니다.
양자 알고리즘으로 풀기 위해서는 먼저 이 문제를 `QUBO`로 변환해야 합니다.
QUBO는 Quadratic Unconstrained Binary Optimization의 줄임말로,
Quadratic program의 제약조건을 모두 없애고 목적함수의 모든 변수는 이진수로 대체한 형태입니다.
변환 과정은 [이차계획법 문제를 위한 변환기](https://qiskit.org/documentation/stable/0.26/locale/ko_KR/tutorials/optimization/2_converters_for_quadratic_programs.html)에
잘 설명되어 있으니 차례대로 따라가면서 실행하면 되겠습니다.

위의 링크에 나와있는 일련의 과정을 거친 후 QUBO로 변환이 완료되었다면, `Ising Hamiltonian`으로의 최종 변환만 거치면 끝입니다.

```python
# Convert to Ising Hamiltonian
op, offset = qubo.to_ising()
# Convert to QuadraticProgram again
# qp = QuadraticProgram()
# qp.from_ising(op, offset, linear=True)
```

이제 `MinimumEgenOptimizer`를 통해 `qubo`의 최적해를 구할 수 있습니다.
여러 QAOA, VQE, CplexSolver (classical exact method) 등 여러 방법이 있지만, 이번에는 QAOA를 사용해서 해를 구해보겠습니다.

```python
from qiskit import BasicAer
from qiskit.optimization import QuadraticProgram
from qiskit.optimization.algorithms import MinimumEigenOptimizer

# `AttributeError: 'EvolvedOp' object has no attribute 'broadcast_arguments'` occured in Colab
# from qiskit.utils import algorithm_globals, QuantumInstance
# from qiskit.algorithms import QAOA

# Thanks to https://blog.xa0.de/post/Solving-QUBOs-with-qiskit-QAOA-example/
from qiskit.aqua import aqua_globals, QuantumInstance
from qiskit.aqua.algorithms import QAOA

algorithm_globals.random_seed = 10598
quantum_instance = QuantumInstance(BasicAer.get_backend('qasm_simulator'))
qaoa_mes = QAOA(quantum_instance=quantum_instance)
qaoa = MinimumEigenOptimizer(qaoa_mes)
qaoa_result = qaoa.solve(qubo)
print(qaoa_result)
```

위의 코드를 실행하고 나면 Quadratic program의 해를 도출할 수 있습니다.

### Max-cut problems

Max-cut problem은 그래프의 정점을 두 개의 부분집합으로 나눠, 그 두 집합 사이에
지나는 (간선의 수 + 각 간선의 가중치 합)이 최대가 되도록 하는 해를 찾는 최적화 문제입니다.
제약조건이 없기 때문에 바로 QAOA를 통해 해를 구할 수 있습니다.

```python
import networkx as nx
# Make a graph with degree=2 and node=5
graph = nx.random_regular_graph(d=2, n=5, seed=111)
pos = nx.spring_layout(graph, seed=111)

# Application class for a Max-cut problem
# !pip install qiskit_optimization
# Make a Max-cut problem from the graph
from qiskit_optimization.applications import Maxcut
maxcut = Maxcut(graph)
maxcut.draw(pos=pos)
```

위의 그래프를 QuadraticProgram으로 변환하고 QAOA를 통해 해를 구하면 끝입니다.

```python
# Make a QuadraticProgram by calling to_quadratic_program()
qp = maxcut.to_quadratic_program()
print(qp)

from qiskit import Aer
from qiskit.utils import QuantumInstance
from qiskit_optimization.algorithms import MinimumEigenOptimizer
from qiskit.algorithms import QAOA, NumPyMinimumEigensolver

qins = QuantumInstance(backend=Aer.get_backend('qasm_simulator'), shots=1000, seed_simulator=123)
# Define QAOA solver
meo = MinimumEigenOptimizer(min_eigen_solver=QAOA(reps=1, quantum_instance=qins))
result = meo.solve(qp)
print('result:\n', result)
print('\nsolution:\n', maxcut.interpret(result))
print('\ntime:', result.min_eigen_solver_result.optimizer_time)
maxcut.draw(result, pos=pos)
```

`qiskit_optimization.applications`에서 Maxcut을 바로 지원하고 있어서 해를 구하기가 매우 쉬워졌습니다.

*Maxcut의 메소드로는 총 4개를 지원하고 있습니다:
[Maxcut](https://qiskit.org/documentation/optimization/stubs/qiskit_optimization.applications.Maxcut.html#qiskit_optimization.applications.Maxcut).

## Qiskit 머신 러닝

분류는 지도 학습의 일종으로, 주어진 데이터를 통해 카테고리 별 학습 후 새로운 데이터가 들어왔을 때
카테고리를 기계 스스로 판단하도록 합니다. 그 중 이진 분류는 카테고리가 딱 두 개만 있는 것입니다.

이진 분류는 경계선을 통해 구역을 나눌 수 있는데, 그 경계선은 하나로 정해지지 않을 가능성이 큽니다.
그래서 Support Vector Machine, 이하 SVM을 통해 경계선 중 가장 마진(margin)이 큰 경우를 찾습니다.
그런데 SVM을 통해서도 마땅한 경계선을 찾을 수 없는 경우가 생길 때는 Kernelized SVM을 사용합니다.
이를 사용해 2D였던 공간을 고차원 공간으로 변환해, 다시 그 경계를 찾습니다.

### 양자 커널

어떤 feature map을 선택하느냐에 따라 얼마나 좋은 분류 결과를 얻을 수 있는지가 결정됩니다.
이번 세션에서는 Z와 ZZ feature map을 비교하였습니다.
데이터 셋은
[데모 세션 발표 자료](https://github.com/qiskit-community/qiskit-application-modules-demo-sessions/blob/main/qiskit-machine-learning/qiskit-machine-learning-demo.ipynb)에서
확인 가능합니다.

```python
# !pip install qiskit_machine_learning
from qiskit_machine_learning.kernels import QuantumKernel
```

- Z feature map 기반 커널
  ```python
  from qiskit.circuit.library import ZFeatureMap

  z_feature_map = ZFeatureMap(feature_dimension=adhoc_dimension, reps=2)
  z_kernel = QuantumKernel(feature_map=z_feature_map, quantum_instance=quantum_instance)
  ```

- ZZ feature map 기반 커널
  ```python
  from qiskit.circuit.library import ZZFeatureMap

  zz_feature_map = ZZFeatureMap(feature_dimension=adhoc_dimension, reps=2, entanglement='linear')
  zz_kernel = QuantumKernel(feature_map=zz_feature_map, quantum_instance=quantum_instance)
  ```

위에서 만든 두 커널을 `scikit-learn`의 SVC를 통해 점수를 구해서 비교했습니다.

```python
from sklearn.svm import SVC

svc_z = SVC(kernel=z_kernel.evaluate)
svc_z.fit(train_features, train_labels)
score_z = svc_z.score(test_features, test_labels)

svc_zz = SVC(kernel=zz_kernel.evaluate)
svc_zz.fit(train_features, train_labels)
score_zz = svc_zz.score(test_features, test_labels)

print(f'ZFeatureMap: {score_z}, ZZFeatureMap: {score_zz}') # ZFeatureMap: 0.9, ZZFeatureMap: 1.0
```

여기서 점수는 평균 정확도를 나타내는 값으로 이 데이터에서는 ZZ feature map을 선택하는 것이 더 적합하다는 것을 알 수 있습니다.

## References

- [qiskit-community/qiskit-application-modules-demo-sessions](https://github.com/qiskit-community/qiskit-application-modules-demo-sessions)
- [모두를 위한 컨벡스 최적화: 05-02 Quadratic Programming (QP)](https://wikidocs.net/17852)
- [Qiskit: 양자 근사 최적화 알고리즘 (Quantum Approximate Optimization Algorithm)](https://qiskit.org/documentation/stable/0.24/locale/ko_KR/tutorials/algorithms/06_qaoa.html)
- [Kernel-SVM](https://ratsgo.github.io/machine%20learning/2017/05/30/SVM3/)
- [sklearn.svm.SVC](https://scikit-learn.org/stable/modules/generated/sklearn.svm.SVC.html)

---

💬 _Any comments and suggestions will be appreciated._
