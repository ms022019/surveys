﻿# **LEARNING ACTIVATION FUNCTIONS TO IMPROVE DEEP NEURAL NETWORKS**
<https://arxiv.org/abs/1412.6830>

著者：Forest Agostinelli, Matthew Hoffman, Peter Sadowski, Pierre Baldi（全員コーネル大学）

2014年12月
## **どんなもの？**
- 勾配効果法を用いて各ニューロンに対して独立に学習する活性化関数を設計した
- パラメータ数を大幅に増やすことなく深層学習の性能を大幅に向上させられることを実証
- さらに、ネットワークは多様な活性化関数を学習することから、標準的な1活性化関数fit-allアプローチは最適でない可能性が示唆された。
- Reluによる深層学習を改良し、CIFAR-10 (7.51%), CIFAR-100 (30.83%),  high-energy physics involving Higgs boson decay modes(?)で良い精度になった
## **先行研究と比較**
- 先行研究で学習パラメータを持つ手法（maxout, network-in-network）よりもパラメータ数が少なく実用的 
  - （数式付きで解説載っていたけど読めていない）
## **技術や手法のキモは？**
- 活性化関数のパラメータがネットワークの重みパラメータとともに勾配効果法によって学習される。
- パラメータ 
  - S：あらかじめ設定されたハイパーパラメータ
  - asi：学習時に勾配降下法で学習。線形部分の傾きを制御
  - bsi：学習時に勾配降下法で学習。ヒンジの位置を決定
- 学習時に、2×(隠れ層の数)×(ヒンジ数S)の追加パラメータが必要になるが、ネットワークの重み数に比べれば少ない。

![](./APL_img/img001.png)

↓S=1の場合のグラフ。実際はSが大きいのでヒンジの数が変わり、傾きは動的に変わる。

![](./APL_img/img002.png)
## **どうやって有効だと検証した？**
### **画像認識タスク**
- CIFAR-10 
  - 10クラスの32×32のカラー画像
  - 5万枚の学習画像と1万枚のテスト画像
- CIFAR-100 
  - 100クラスの32×32のカラー画像
  - 5万枚の学習画像と1万枚のテスト画像
- ネットワーク（Srivastava et al., 2014を参考にしている） 
  - 96, 128, 256のフィルタを持つ3つの畳込み層
  - それぞれ最大プーリング、平均プーリング、平均プーリングを持つ
  - カーネルサイズ3, stride2, 2048個のユニットを持つ2つの全結合層
  - ドロップアウトあり
  - 最終層はsoftmax
  - ベースラインはRelu
  - APLを用いる場合CIFAR-10ではS=5, CIFAR-100ではS=2
- 結果 
  - Augmentありとなし
  - 通常のCNNとnetwork in network(NIN)アーキテクチャで実験
  - 異なるランダム初期化を用いて5回学習された
  - APLを導入することでReluよりCIFAR-10で1％程度、100で3％程度改善

![](./APL_img/img003.png)

- CIFAR-10でSの値を変化させた場合の分類精度およびAPLパラメータの学習の有無 
  - S=5が一番精度高い
  - S=1の場合で見ると学習ありの方が精度高い

![](./APL_img/img004.png)
### **HIGGS BOSON DECAY**
- 概要（よく分かっていない） 
  - Higgs-to-τ+ τ− decayデータセット 
    - 8000万の衝突イベントが含まれ、衝突生成物の3次元モーメントとエネルギーを記述する25の実数値特徴量
    - 1000万例のテストデータ
  - 教師あり学習課題は、Higgsボゾンがτ+ τ−レプトンに崩壊する物理過程と、同様の測定分布を生成する背景過程の2種類を区別することである。
- モデル設定 
  - 8層のニューラルネットワークアーキテクチャ
  - ドロップアウトあり
  - APLユニット（提案手法）はS=2
- 結果 
  - Expected discovery significance 
    - 大きいほうが良い結果 
    - 実験物理学の用語らしい。新たな発見の確からしさを示す統計的な指標
    - 相対不確実性5%のシグナルイベント100個とバックグラウンドイベント5000個を用いたガウシアンσの単位でExpected discovery significanceの観点から測定
  - APLユニットを持つネットワークが一番良い性能。

![](./APL_img/img005.png)
### **学習前後の活性化関数**
初期値と違うパラメータになっている（だからどうとかは書いてなかった）

![](./APL_img/img006.png)

活性化関数の出力結果のばらつき

![](./APL_img/img007.png)
## **議論はある？**
- 特に書いていなかった
- Sはどうやって決めると良いのか気になった。表3（CIFAR-10の結果）だけを見た感じだとSをいくらにするかよりも学習パラメータがあるかどうかのほうが重要っぽいので雑に試す感じで良いかも
## **次に読むべき論文は？**
前回読んだ活性化関数の総説論文で学習型の活性化関数の話が気になったので、最初に紹介されていたこの論文を読んだ。

その他学習型の活性化関数の進化版も読むと良さそう。
