---
layout: page
title: Amazon SageMaker の使い方
permalink: /start_sagemaker/
---
# Amazon SageMaker

このページは、サンプルコードをAmazon SageMakerで実行するための簡単なガイドを提供する。ここでは、読者がすでにAWSのアカウントを持っていることを前提とする。もしまだ持っていないなら[このインストラクション](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)に従ってAWSのアカウントを作ってアクティベイトしよう。

ここでは、ノートブックインスタンス、ノートブックのライフサイクル設定、IAMロールなどのSageMaker資源のプロビジョンすべてに[AWS CloudFormation](https://aws.amazon.com/cloudformation/) を用いる。デフォルトでは、*ml.p2.xlarge*のSageMakerノートブックインスタンスを起動する。このインスタンスはNvidia K80 GPU と50 GBのEBSディスクを持つ。


## 価格

デフォルトインスタンスタイプである`ml.p2.xlarge`は1時間あたり$1.26だ。
時間あたりの単価はインスタンスタイプによって異なる。
[利用可能なタイプのリスト](https://aws.amazon.com/sagemaker/pricing/)を確認しよう。
`ml.p2.xlarge`や`ml.p3.2xlarge`インスタンスを利用するには、明示的にリミット値の増加を
[このページ](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase)から
リクエストする必要がある。
SageMakerタイプのリミット値を選択し、利用したいリージョンを選ぶ。
新しいリミット値として1を選び、記述を追加して右下のボタンからサブミットする。
インスタンスを停止しないといつまでも課金されることになる。

 <img alt="limitincrease" src="{{ site.baseurl }}/images/aws/increase_limit_sagemaker.png" class="screenshot">

## 設定

### SageMakerノートブックインスタンスの作成

1. ここでは[AWS CloudFormation](https://aws.amazon.com/cloudformation/)を用いて[SageMaker Notebook Instance](https://docs.aws.amazon.com/sagemaker/latest/dg/nbi.html)を作成し、コースのエクササイズを実行するためのJupyter環境を構築する。CloudFormationスタックを起動するには、下のテーブルから最寄りのリージョンを選び「Launch Stack」リンクをクリックすれば良い。

    リージョン | 名前 | 起動リンク
    --- | --- | ---
    US West (Oregon) Region | us-west-2 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://us-west-2.console.aws.amazon.com/cloudformation/home?region=us-west-2#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    US East (N. Virginia) Region | us-east-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    US East (Ohio) Region | us-east-2 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://us-east-2.console.aws.amazon.com/cloudformation/home?region=us-east-2#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    US West (N. California) Region | us-west-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://us-west-1.console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)    
    Asia Pacific (Tokyo) Region | ap-northeast-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://ap-northeast-1.console.aws.amazon.com/cloudformation/home?region=ap-northeast-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    Asia Pacific (Seoul) Region | ap-northeast-2 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://ap-northeast-2.console.aws.amazon.com/cloudformation/home?region=ap-northeast-2#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    Asia Pacific (Sydney) Region | ap-southeast-2 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://ap-southeast-2.console.aws.amazon.com/cloudformation/home?region=ap-southeast-2#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    Asia Pacific (Mumbai) Region | ap-south-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://ap-south-1.console.aws.amazon.com/cloudformation/home?region=ap-south-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack) 
    Asia Pacific (Singapore) Region | ap-southeast-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://ap-southeast-1.console.aws.amazon.com/cloudformation/home?region=ap-southeast-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)           
    Canada (central) Region | ca-central-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://ca-central-1.console.aws.amazon.com/cloudformation/home?region=ca-central-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)       
    EU (Ireland) Region | eu-west-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://eu-west-1.console.aws.amazon.com/cloudformation/home?region=eu-west-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    EU (Frankfurt) Region | eu-central-1 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://eu-central-1.console.aws.amazon.com/cloudformation/home?region=eu-central-1#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)
    EU (London) Region | eu-west-2 | [![CloudFormation]({{ site.baseurl }}/images/aws/cfn-launch-stack.png)](https://eu-west-2.console.aws.amazon.com/cloudformation/home?region=eu-west-2#/stacks/create/review?filter=active&templateURL=https://fastai-cfn.s3.amazonaws.com/sagemaker-cfn-course-v4.yml&stackName=FastaiSageMakerStack)    

1. すると下図のようにAWS CloudFormationのWebコンソールがオープンし、AWS資源を構築するためのテンプレートが表示される。入力パラメータをチェックし、必要に応じて変更する。**I acknowledge that AWS CloudFormation might create IAM resources.** 
と書かれたチェックボックスをチェックし、**Create** ボタンをクリックしてスタックを作成する。

    <img alt="create stack" src="{{ site.baseurl }}/images/sagemaker/create_stack.png" class="screenshot">

1. CloudFormationのページが開く。スタックのステイタスは`CREATE_IN_PROGRESS`となっているはずだ。ステイタスが**CREATE_COMPLETE**になるまで待つ。AWSのWebコンソールの左上隅にあるServicesメニューから"sage"と入力すると、下図のように`Amazon SageMaker`のリンクが出てくる。これをクリックしてSageMakerのWebコンソールを開く。


   <img alt="sage" src="{{ site.baseurl }}/images/sagemaker/01.png" class="screenshot">

1. 左側のナビゲーションバーから、「Notebook instances」を選択する。このページから、ノートブックのインスタンスを作成し管理しアクセスする。下図を見ると、**fastai-v4** という名前のノートブックインスタンスのステイタスが、`InService`となっていることがわかる。

   <img alt="openjupyter" src="{{ site.baseurl }}/images/sagemaker/open_juypter.png" class="screenshot">
   
   最初にノートブックインスタンスを作成する際には、fastaiライブラリと依存するライブラリがインストールされるので、10分程度かかる。
      
### fastaiコース教材の利用

`Open Jupyter`リンクをクリックすると、fastaiコースのノートブックがすでにインストールされたJupyterノートブックのWebインターフェイスへとリダイレクトされる。

<img alt="coursenotebooks" src="{{ site.baseurl }}/images/sagemaker/course_notebooks.png" class="screenshot">

ノートブックを最初にオープンする際には、Jupyterに用いるカーネルを選択するように求められる。下図に示すように`fastai`という名前のカーネルがあるので、ドロップダウンメニューからそれを選んで、`Set Kernel`のボタンをクリックする。

<img alt="selectfastaikernel" src="{{ site.baseurl }}/images/sagemaker/selectkernel.png" class="screenshot">

`fastai`というカーネルが現れない場合には、依存ライブラリやfastaiライブラリのインストールがまだ終わっていない。完了までには10分かかるので、それを待ってからページをリフレッシュして、`fastai`カーネルを選択しよう。

### インスタンスの停止

- 利用が終わったらノートブックタブをクローズし、**stopをクリックすることを忘れないように！** *stop*ボタンがクリックされるまで、課金が発生し続ける。

    <img alt="stop" src="images/sagemaker/stop_instance.png" class="screenshot">


### 作業の再開

ノートブックを使った演習に戻りたければ、ノートブックインスタンスを選択しStartを選択すればいい。数分まってから前回やったところまで戻れば良い。fastaiライブラリはすでにインストールされているし、ノートブックもセーブされているはずなので、初めて実行するときよりも早いはずだ。

## 質問があったら

コースの内容に質問や疑問があれば、[fast.ai forum](http://forums.fast.ai/)にポストしてほしい。
