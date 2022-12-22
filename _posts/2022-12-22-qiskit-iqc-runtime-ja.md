---
title: "Qiskit RuntimeでPrimitiveを使ってみましょう！"
categories:
    - Qiskit
tags:
    - Qiskit
    - Quantum Computing
    - Open Source
last_modified_at: 2022-12-22
author_profile: true
sitemap:
    changefreq: daily
    priority: 1.0
---

IBM Quantum Challenge Fall 2022のラボ１を簡単に振り返りながら、皆さんと一緒にQiskit RuntimeのPrimitiveを理解してみたいと思います。

2022年、ついに先月の11月に、[IBM Quantum Challenge Fall 2022](https://research.ibm.com/blog/quantum-challenge-fall-2022)が終わりました！チャレンジのメイントピックはQiskit RuntimeのPrimitives関連で、このPrimitivesを使用して様々なアプリケーションを学ぶことができ、色々な意味で実に面白いチャレンジだったと思います。

いつもチャレンジは面白いですが、今回はさらに興味深いものでした。その理由は８月からIBM Quantumでインターンとして働いたので、私も今回のチャレンジの準備に参加したからです。チャレンジのノートブック全体の４つのうち３つに貢献しましたが、そのうちの最初のノートブック、ラボ１をメインに担当しました。最初のノートブックはQiskit RuntimeとPrimitiveについて説明したもので、その中にはQiskit RuntimeのPrimitiveにあるエラー緩和技術の紹介もあります。このラボ１のコンテンツを今回の記事で簡単に共有したいと思います。

## Primitive入門

Qiskit RuntimeにはPrimitiveと呼べるプログラムの基本要素のようなものがあります。現在はSamplerとEstimatorという２種類のPrimitiveがあります。どのPrimitiveを使うかは、目的によって決定されます。

Samplerは確率分布のサンプリングをするためのものです。Samplerの結果は量子回路を実行した後に得られる結果ととても似ていると思うかもしれません。実は違いがあり、それはSamplerが「準確率」と呼ばれる分布結果を返すことです。準確率と呼ばれる理由は確率が負の値をとることができるためで、この性質によりQiskit Runtimeではエラー緩和技術を適用することができます。ラボ２では量子機械学習についてSamplerの使用例を確認することもできます。さらに、[チュートリアル](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials.html#sampler)にもSamplerの使用例がいくつかあります。

Estimatorは量子演算子の期待値を計算するためのものです。Estimatorは、Samplerと比べて、観測量(observable)というもう１つ必要なものがあります。観測量は[「SparsePauliOp」](https://qiskit.org/documentation/stubs/qiskit.quantum_info.SparsePauliOp.html)を使って設定できます。期待値についてもう少し説明すると、Qiskitではz軸の測定がデフォルトであるため、演算子XおよびYを使用した測定用にいくつかのゲートが追加されます。

$$
\langle \psi | Z | \psi \rangle
$$

$$
\langle \psi | X | \psi \rangle = (H | \psi \rangle)^\dagger Z (H | \psi \rangle) = \langle \psi | HZH | \psi \rangle
$$

$$
\langle \psi | Y | \psi \rangle = (HS^\dagger | \psi \rangle)^\dagger Z (HS^\dagger | \psi \rangle) = \langle \psi | SHZHS^\dagger | \psi \rangle
$$

Estimatorを使ってジョブを送ると、観測量に基づいていくつかの演算子が追加されます。ラボ３と４または[チュートリアル](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials.html#estimator)ではEstimatorの使用例を確認することもできます。

## Qiskit RuntimeでのPrimitiveの使い方

現在、Qiskit RuntimeではSessionを使うことが推奨されています。Sessionの実装は、以下の手順に従います。

1. ルーチンを実行するための適切なQiskit Runtimeのサービスとバックエンドを設定します。
2. サービスとバックエンドでSessionを作成します。
3. Session内のSamplerまたはEstimatorのPrimitiveでインスタンスを作ります。

この3つの手順があれば、好きなように量子回路を実行できます。以下に、各Primitiveのコード例を示しました。

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

IBM Quantum Challenge Fall 2022の[メインリポジトリ](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22)（[ラボ 1 ノートブック](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22/blob/main/content/lab-1/lab1-ja.ipynb)）でさらに多くの例を確認できます。

## エラー緩和技術の適応が容易

Qiskit Runtimeでユニークな機能の1つは、エラー緩和技術を追加するのが簡単なことです。この技術がどのように機能するかを見せるために、 チャレンジではノイズありシミュレーターが紹介されました。このノイズありシミュレーターは実際のバックエンドを模倣しますので、理想的なシミュレーターにノイズを適用して長い待ち時間なしで実行できます。

このノイズありシミュレーターを使うには、Primitiveを初期化するときにOptionsを追加することが必要です。Optionsの中にresilience_levelとoptimization_levelの値を設定できます。どのエラー軽減技術を使うのかはOptionsの設定によって異なります。

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

今、Qiskit Runtimeで利用可能なエラー緩和技術には、 5 つの技術（DD、M3、T-REx、ZNE、PEC）があります。この技術に関して、詳しく読みたい方は公式サイトの[チュートリアル](https://qiskit.org/documentation/partners/qiskit_ibm_runtime/tutorials/Error-Suppression-and-Error-Mitigation.html)をチェックしてください！

## まとめ

今回のチャレンジのトピックの設定から最後の問題確定まで参加しながら、私自信も学んだことがたくさんあります。特にラボ１を担当したので、Qiskit RuntimeのPrimitiveに関しては完全に理解することができました！この記事はラボ１の内容を簡単に要約したものなので、さらに興味がある場合はぜひチャレンジの[メインリポジトリ](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22)の[ラボ 1 ノートブック](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22/blob/main/content/lab-1/lab1-ja.ipynb)を確認してください！

ちなみに、今月がインターンシップの最後の月ですが、働いた5ヶ月間、量子コンピューティング分野の探求ができたチャレンジ以外にも、英語と日本語のコミュニケーションの向上やネットワーキングで、将来の進路設定に関し、多くの助けを受けることができました。この意味のある経験を可能にしてくれた小林有里さんと沼田祈史さんに感謝します！

## References

- [IQC Fall 2022 - Lab 1 solution](https://github.com/qiskit-community/ibm-quantum-challenge-fall-22/blob/main/solutions-by-authors/lab-1/lab1.ipynb)
- [Mesurement error mitigation using M3](https://quantum-enablement.org/how-to/mitigation/M3/m3_mitigation.html)
- [TULA Log: Getting close to Qiskit Runtime](https://tula3and.github.io/qiskit/qiskit-iqc-runtime/)

---

💬 _Any comments and suggestions will be appreciated._
