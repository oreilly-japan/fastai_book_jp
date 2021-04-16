---
layout: page
title: Colabの使い方
permalink: /start_colab/
---

### 無償利用できるGoogleのGPUノートブック

ここでは、本書のサンプルコードをGoogle Colabで実行する方法を簡単に説明する。
ColabはGPUが無償で使えるNotebookサービスだ。
Colabのノートブックは通常のJupyterをベースにしているが、若干異なる点もある。
Colabのドキュメントを読んで動作を理解しておこう。

注意: Colabは無償サービスだが、いつも使えるとは限らないし、実行結果を保存するにはひと手間かかる。
Colabのウェブサイトを呼んでこのシステムの制約をよく理解しておこう。
  
### 書籍の各章のオープン

下記のリンクから書籍の各章のノートブックをColabでオープンすることができる。


[1章 ディープラーニングへの旅路][chap01] |
[2章　モデルから実運用へ][chap02] |
[3章　データ倫理][chap03] |
[4章　舞台裏：数字のクラス分類器][chap04] |
[5章　画像クラス分類][chap05] |
[6章　他のコンピュータビジョン問題][chap06] |
[7章　SOTA モデルの訓練][chap07] |
[8章　協調フィルタリングの詳細][chap08] |
[9章　テーブル型データモデルの詳細][chap09] |
[10章　自然言語処理の詳細：RNN][chap10] |
[11章　fastai の中位API によるデータマングリング][chap11] |
[12章　言語モデルを1 から作る][chap12] |
[13章　畳み込みニューラルネットワーク][chap13] |
[14章　ResNet][chap14] |
[15章　アプリケーションアーキテクチャの詳細][chap15] |
[16章　訓練のプロセス][chap16] |
[17章　基礎からのニューラルネットワーク][chap17] |
[18章　CAM を用いたCNN の解釈][chap18] |
[19章　fastai Learner を1 から作る][chap19] |
[20章　おわりに][chap20] |

[app_jupyter]:https://colab.research.google.com/github/fastai/fastbook/blob/master/app_jupyter.ipynb
[chap01]:https://colab.research.google.com/github/fastai/fastbook/blob/master/01_intro.ipynb
[chap02]:https://colab.research.google.com/github/fastai/fastbook/blob/master/02_production.ipynb
[chap03]:https://colab.research.google.com/github/fastai/fastbook/blob/master/03_ethics.ipynb
[chap04]:https://colab.research.google.com/github/fastai/fastbook/blob/master/04_mnist_basics.ipynb
[chap05]:https://colab.research.google.com/github/fastai/fastbook/blob/master/05_pet_breeds.ipynb
[chap06]:https://colab.research.google.com/github/fastai/fastbook/blob/master/06_multicat.ipynb
[chap07]:https://colab.research.google.com/github/fastai/fastbook/blob/master/07_sizing_and_tta.ipynb
[chap08]:https://colab.research.google.com/github/fastai/fastbook/blob/master/08_collab.ipynb
[chap09]:https://colab.research.google.com/github/fastai/fastbook/blob/master/09_tabular.ipynb
[chap10]:https://colab.research.google.com/github/fastai/fastbook/blob/master/10_nlp.ipynb
[chap11]:https://colab.research.google.com/github/fastai/fastbook/blob/master/11_midlevel_data.ipynb
[chap12]:https://colab.research.google.com/github/fastai/fastbook/blob/master/12_nlp_dive.ipynb
[chap13]:https://colab.research.google.com/github/fastai/fastbook/blob/master/13_convolutions.ipynb
[chap14]:https://colab.research.google.com/github/fastai/fastbook/blob/master/14_resnet.ipynb
[chap15]:https://colab.research.google.com/github/fastai/fastbook/blob/master/15_arch_details.ipynb
[chap16]:https://colab.research.google.com/github/fastai/fastbook/blob/master/16_accel_sgd.ipynb
[chap17]:https://colab.research.google.com/github/fastai/fastbook/blob/master/17_foundations.ipynb
[chap18]:https://colab.research.google.com/github/fastai/fastbook/blob/master/18_CAM.ipynb
[chap19]:https://colab.research.google.com/github/fastai/fastbook/blob/master/19_learner.ipynb
[chap20]:https://colab.research.google.com/github/fastai/fastbook/blob/master/20_conclusion.ipynb


別の方法もある。ColabのWelcomeページに行き、'Github'をクリックする。
'Enter a GitHub URL or search by organization or user'と聞かれたら、
'fastai/fastbook'と入力する。すると、このコースのノートブックが列挙されるので、
使いたいものを選択すればいい。

注意: 2章の節の一つではVoilaを利用している。Colabでは残念ながらVoilaは動作しない。
この節に関しては、スキップするか、Gradientなど他のプラットフォームを使って欲しい
(Colab以外なら動作する)。


### GPUの使用

何よりも先に、Colabに対して、GPUを使いたいと伝える必要がある。これには「Runtime」タブをクリックして、
「Change runtime type」を選択する。
するとポップアップウィンドウが開く。その中に次のようなドロップダウンメニューがあるはずだ。

<img src="{{ site.baseurl }}/images/colab/02.png" width="80%">

このメニューからGPUを選択し、'Save'をクリックする。


<img src="{{ site.baseurl }}/images/colab/03.png" width="30%">

### ノートブックの設定
最初のセルには、fastaiやその他の必要なライブラリをセットアップするためのコードが書かれている。次のようなコードだ。

```python
!pip install -Uqq fastbook
import fastbook
fastbook.setup_book()
```

このセルの左側にある三角形の実行ボタンをクリックしてこのセルを実行しよう。`Ctrl-Enter`でも実行できる。

最初のセルを実行すると、次のように「Warning: This notebook was not authored by Google'」
という警告ウィンドウがポップアップする事がある。この場合は「'Run Anyway'」を選択すればよい。

<img src="{{ site.baseurl }}/images/colab/04.png" width="50%">


### Google Driveの利用

ColabはノートブックをGoogle Driveにセーブする。このため、セルを実行するとGoogle Driveにログインするように求めてくる。これにはGoogleアカウントが必要になる。

ログインをしたら、次のセルを実行する。このセルでは、次のようにしてインストールしたばかりの必要なライブラリをすべてインポートする。

```python
from fastbook import *
```

Google Driveへのパスは、`gdrive`という変数名で与えられる。この変数は`Path('/content/gdrive/My Drive')`を指している。
ファイルやモデルをセーブする際には、このパスの下のディレクトリを使う必要がある。

Githubからノートを開いた場合には、Google Driveにセーブする必要がある。
これには、`'File'`から`'Save'`すればよい。
次のようなポップアップが表示されるはずだ。

<img src="{{ site.baseurl }}/images/colab/09.png" width="100%">

ここで、`'SAVE A COPY IN DRIVE'`をクリックする。
こうすると別のタブでGoogle Driveに置かれたノートブックが開く。
セーブしたあとも作業を続けるなら新しいタブで開かれた方を使おう。
ノートブックはデフォルトでは、Google Driveの`Colab Notebooks`フォルダにセーブされる。

より高度で効率的なColabの使い方については、
この[ブログ記事](https://medium.com/@robertbracco1/configuring-google-colab-like-a-pro-d61c253f7573)を参照してほしい。