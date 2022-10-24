# Developer associate

[試験ガイド](https://d1.awsstatic.com/ja_JP/training-and-certification/docs-dev-associate/AWS-Certified-Developer-Associate_Exam-Guide.pdf)
## 推奨される全般的な IT の知識 
- 1 つ以上の高水準プログラミング言語についての詳細な知識
- アプリケーションライフサイクル管理に関する理解
- サーバーレスアプリケーション用のコードを記述する能力
- 開発プロセスでのコンテナの使用に関する理解

## アナリティクス
### Amazon Elasticsearch Service (Amazon ES)
### Amazon Kinesis

## アプリケーション統合

### サービスの分類

- ストリーミングデータ
    - Amazon Kinesis - テキストから動画までをサポートする
    - Amazon Managed Stream Kafka - Kafkaをマネージドサービスで提供する
    - Amazon API Gateway - WebSocketをサポートする
- メッセ―ジデータ
    - Push方式
        - Amazon SNS
    - Pull方式
        - Point-to-point形式
            - Publish Subscribe方式
                - Amazon SQS
        - Point-to-point形式及びPublish Subscribe方式
            - Amazon MQ

### Amazon EventBridge (Amazon CloudWatch Events)
[Amazon EventBridge](https://www.youtube.com/watch?v=H7641kZMghg&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)



### Amazon Simple Notification Service (Amazon SNS)
[Amazon Simple Notification Service (SNS)](https://www.youtube.com/watch?v=bPCjOG_jQlc&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- Pub Sub型のメッセージ配信サービス
- Topic - Topicは名前しか持たない
- Subscribeでは、行動するTopicとProtocolを指定する。
- メッセージにはメッセージ属性を追加することができる。
- メッセージ属性はJSON形式で定義される。
- メッセージ送信ができない場合はRetryされる。


### Amazon Simple Queue Service (Amazon SQS)
[AWS Black Belt Online Seminar Amazon Simple Queue Service](https://www.youtube.com/watch?v=avfc0gQ7X0A&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- セキュリティ - 利用するユーザのアクセス制御、データの暗号化
- 耐久性 - 複数のサーバにデータを分散して保存している。
- スケーラビリティ - ほぼ無制限のTPS
- フルマネージド型
- メッセージの最大サイズ256KB
- 最大14日間キューにデータを保存する
- スタンダードキュー
    - ほぼ無制限のスループット
    - 少なくとも1回の配信（2回以上配信されることもある）
    - 配信順序はベストエフォート
    - 100万件をこえた場合、100万件ごとに0.4USD
- FIFOキュー
    - 1秒間あたり最大300件のメッセージ
    - 1回のみ配信
    - 配信順序は保たれる
    - 100万件をこえた場合、100万件ごとに0.5USD
- ショートポーリング
    - メッセージが無い場合は「空」を応答
    - 分散されたサーバ中からサンプリングされたサーバのメッセージを応答する
    - API呼び出し数が多くなり、料金が高くなる可能性がある
- ロングポーリング
    - メッセージが無い場合はタイムアウト
    - 全てのサーバをクエリしてメッセージを応答する
    - API呼び出し数が少なくなり、料金が安くなる可能性がある
- ポーリングでは、特定のIDや条件でメッセージを取得することはできない
- 取得したメッセージは削除する必要がある
- 可視性タイムアウト
    - あるコンシューマがメッセージが取得してから、他のコンシューマが同じメッセージを取得することをできなくする時間
    - デフォルトで30秒
    - スタンダードキューの場合は、可視性タイムアウトではメッセージを二回受信しない保証にならない。
- 遅延キューとメッセージタイマー
    - プロデューサが送信したメッセージをキューから取り出すことをできなくするタイマー
    - 遅延キューはキュー全体に、メッセージタイマーはメッセージに対して遅延時間を設定する
    - 両方指定された場合はメッセージタイマーが優先される
    - メッセージタイマーはFIFOは未サポート
- Dead Letter Queue
    - 正しく処理できないメッセージがキュー内に滞留する事を防ぐ
    - キューに最大受信数を指定すると、メッセージの取得がその数に達するとDead Letter Queueに入れられる。
    - 同一リージョン、同一アカウント、同一タイプで作成する必要がある。
    - データの保持期間は元のキューに追加された際のタイムスタンプに基づく。
- メッセージの暗号化
    - KMSを利用する
    - KMSを利用するために追加の料金がかかる。
    - メッセージ本体のみを暗号化する
    - メッセージ属性は暗号化対象ではない
    - プロデューサもコンシューマもKMSへのアクセス権が必要になる
- キューのアクセス制御
    - IAMポリシーかSQSポリシー、またはその両方で制御する
- メッセージ属性
    - メッセージ本体とは別に最大10個のメタ情報を持たせることができる
    - メッセージ属性のサイズも256KBの容量制限に含まれる。

### Amazon Pinpoint

### AWS Step Functions
[AWS Step Functions](https://www.youtube.com/watch?v=PGyasNJ1QTQ&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- オーケストレーションを行うためのサービス
- ステートマシンとはワークフローにことである
- ステートマシンをASL(Amazon State Language)と呼ばれるJSONで記述する
- ASLはStateLint(Ruby)で文法チェックする事ができる
- ステートマシンの実行方法
    - Amazon Cloud Watch Events: S3へのファイル保存をトリガーにする等
    - Amazon API Gateway
    - マネージメントコンソール
    - AWS CLI
    - SDK
- ステートマシンから呼び出し可能なAWSのサービス
    - Lambda
    - DynamoDB : 既存アイテムの取得、新規アイテムの登録
    - AWS Batch
    - ECSタスクの実行
    - SNSへのメッセ―ジ送信
    - SQSへのメッセージ送信
    - AWS Glueジョブの実行
    - AWS SageMakerのジョブ起動
- ステートマシンからActivityを実行可能
- ActivityとはECSやコンテナ上のアプリからポーリングする事で独自の処理を事項する仕組み
- ステート実行への引数はInputPathとParametersという属性名で指定する。
- ステート実行の結果はResultPathという属性名で指定する。
- ステート実行の最終的に結果はOutputPath属性の指定されるフィルタを使ってフィルタされる。
- ローカル環境でStepFunctionsを開発するためには、StepFunctionsLocalを使うことができる。

### Amazon Simple Workflow Service(SWF)

- StepFunctionsとよく似たサービスである。
- プログラミングベースでワークフローを制御する。
- 現在は、StepFunctionsを使うことが推奨されている。


## コンピューティング
### Amazon EC2

### AWS Elastic Beanstalk
[https://www.youtube.com/watch?v=LhmFZryVLiI&t=12s&ab_channel=AmazonWebServices](https://www.youtube.com/watch?v=LhmFZryVLiI&t=12s&ab_channel=AmazonWebServices)
数クリックでWebサーバ環境又はワーカー環境を作成することができる。

- Webサーバ環境の場合
    - EC2 インスタンス
    - インスタンスセキュリティグループ
    - Amazon S3 バケット
    - Amazon CloudWatch アラーム
    - AWS CloudFormation スタック 
    - ドメイン名
    - Auto Scaling
    - ELB

- ワーカー環境の場合

### AWS Lambda
[AWS Lambda Part1](https://www.youtube.com/watch?v=QvPgjEwgiew&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[AWS Lambda Part2](https://www.youtube.com/watch?v=96ku2x1NCaE&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[AWS Lambda Part3](https://www.youtube.com/watch?v=rMG18Fr896U&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[AWS Lambda Part4](https://www.youtube.com/watch?v=AOx5iNmxOC8&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- 隔離されたコンテナで実行される
- １つのコンテナでは同時に１つのイベントだけを処理する
- 実行されることは、ビルドしてパッケージングしたうえでZIP形式アップロードする必要がある
- アップロードしたものはS3に保存されて、実行時以外は暗号化される
- メモリ
    - 64MBごとに128MBから3008MBの間で設定可能
    - メモリ容量に応じてCPU能力が比例する
    - メモリ容量がある程度増えるとマルチコアのCPUになる
- タイムアウト
    - 最大15分
- 実行ロール
    - 必要なAWSリソースへのアクセスを許可するロール
- Lambdaが動作するOSのAMIも公開されている
- イベントソース
    - ポーリングベース
        - ストリームベース Kinesis
        - 非ストリームベース SQS
    - それ以外  API Gateway等
- 非同期呼出と同期呼出がある。
- 非同期呼出は、呼び出しの成否のみが返却される。失敗した場合は、自動的に2回までリトライされる
- ポーリングベースでストリームベースの場合は、データの有効期限が切れるまでリトライが行われる
- デフォルトの同時実行数はアカウント単位で1000となっている
    - 1000の同時実行数を、関数単位で任意に割り当てることが可能
- コールドスタート - コンテナの作成、デプロイパッケージのロードと展開、ランタイムの初期化、関数の実行が行われる。
- ウォームスタート - コンテナの作成、デプロイパッケージのロードと展開、ランタイムの初期化までを再利用（コンテナの再利用）する。
- コールドスタートは、１つもコンテナが無い、利用可能な数以上のリクエストが来た場合、コードを変更した場合に発生する
- 環境変数を使うことができる。
    - 環境変数はデフォルトでLambda用のKMSサービスキーを使って暗号化される
    - 関数が呼び出されると自動的に復号される
    - 暗号化の実施はデプロイプロセス後に実施される
    - キーとバリューの合計サイズは4KB以下の制限がある
- Lambdaはバージョニングする事ができる
- バージョンにはエイリアスを設定することができる
- エイリアスを利用したカナリアデプロイのような運用も可能
- 関数の共通部分は、Lambda Layersとして共通化する事ができる。
- Lambda Layersは最大で5個まで利用する事ができる

## コンテナ:
[CON142 Docker入門](https://www.youtube.com/watch?v=CGfRsyQW1rE&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=30&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

### コンテナセキュリティ
[CON231 コンテナセキュリティ入門 Part.1](https://www.youtube.com/watch?v=I1o01lkQNHY&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=24&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F9)
[CON231 コンテナセキュリティ入門 Part.2](https://www.youtube.com/watch?v=OTwC6zpgZjc&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=23&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[CON231 コンテナセキュリティ入門 Part.3](https://www.youtube.com/watch?v=drWE7enGFvo&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=22&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

### ECSとFarget
[CON303 Amazon Elastic Container Service − EC２ / Fargate Spot ことはじめ](https://www.youtube.com/watch?v=fvzbLMrteZg&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=36&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

### Amazon Elastic Container Registry (Amazon ECR)
[CON241 Elastic Container Registry](https://www.youtube.com/watch?v=wNWzdoeHhUE&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- コンテナイメージはS3に保存される。
- リポジトリにはリポジトリポリシーを設定できる。
- privateとpublicのレジストリを作成できる。
- privateとpublicで機能が異なる。
- privateの場合は、イメージの脆弱性スキャンが可能
- basic scanningは無料で、Enhanced scanningは有料

### Amazon Elastic Container Service (Amazon ECS)
[ECS入門](https://www.youtube.com/watch?v=XAyrpXj4TVA&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=42&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[ECS](https://www.youtube.com/watch?v=tmMLLjQrrRA&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[ECS Deep Dive](https://www.youtube.com/watch?v=FeRkcA33-d0&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=35&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- 起動タイプは、EC2かFargateのいずれかを選択する。
- EC2の場合は、Amazon ECS-Optimized AMIから作成する。
  以下が事前にインストールされている。
    - Dockerデーモン
    - ECSコンテナエージェント
- ECSコンテナエージェントはECSコントロールプレーンと通信して、コンテナインスタンスの管理やタスクの実行・停止を行う。
- ECSコンテナエージェント自体もコンテナで実行される。
- EC2タイプの場合は、OSやエージェントのパッチ当てや更新を自分で行う必要がある。
- EC2インスタンス数のスケーリングを自身で行う必要がある。
- Fargateでは、インスタンスを意識する必要がない。
- タスク定義は、アプリケーションを構成する１つ以上のコンテナの情報を定義するjson形式のテキストファイル。
  使用するコンテナ、起動タイプ、コンテナの実行方法などを指定する。
  アプリケーションを実行すると、タスク定義をインスタンス化したタスクを実行することになる。
- タスク定義は変更する事ができず、バージョンを更新する事で更新していく。
- タスク定義内のコンテナ定義では、環境変数とSecretというAWS Secret ManagerやAWS Systems Managerで管理するセキュアな情報を使用することができる。


### Amazon Elastic Kubernetes Service (Amazon EKS)
[CON222 Amazon Elastic Kubernetes Service (Amazon EKS) でのData Plane管理](https://www.youtube.com/watch?v=WzCxHW0wNBo&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- セルフマネージド型のデータプレーン
- マネージド型のデータプレーン
    - AutoScalingグループを管理して、自動でノードの起動終了を任せることができる。
- Fargateのデータプレーン


### Amazon Fargate
[CON202 ECS Fargate入門](https://www.youtube.com/watch?v=5fXwkTgWrjw&list=PLzWGOASvSx6FIwIC2X1nObr1KcMCBBlqY&index=43&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
[AWS Fargate](https://www.youtube.com/watch?v=rwwOoFBq2AU&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- 完全マネージド型のコンテナサービス
- ECSから利用する事ができる
- 価格はEC2タイプより少し高い
- Farget spotプランで最大70OFFの価格で利用できる
- CPUとメモリの割り当ては、タスク単位かコンテナ単位かを選ぶことができる
- タスク単位の設定は必須となっている
- 揮発性の書き込み可能なレイヤイメージを利用できる。
  タスクごとに10GBが利用でき、コンテナ間で独立して利用する事ができる。
- 揮発性の書き込み可能なコンテナイメージも利用できる。
  タスクごとに4GBが利用でき、コンテナ間で共用する事ができる。
- ネットワークはawsvpcネットワークモードだけが利用できる・
  タスクごとにENIを自動的に割り当てる。
- タスクごとにセキュリティグル－プを設定することができる。
- 負荷分散はALBを使うか、Route53を使う。
- Route53の更新は、Amazon ECS Serive DiscoveryからAWS Cloud Mapに登録することで更新する
- EC2と異なり、Spotインスタンスを利用することはできない。
- docker execを使ってインタラクティブなデバッグはできない
- Farget Spotを使ってスケールインする場合は、終了イベントが通知されるので、120秒以内に終了する必要がある。
  また、ALBからの登録解除も自分で行う必要がある（EC2のSpotインスタンスの場合は不要）。
  登録解除は、EventBridgeからのイベントで駆動するLambdaで登録解除を行う方式になる。



### ECS Capacity Provider

- AutoScaling GroupやFargetを１つのCapacity Providerとして定義して、クラスタに紐づけて使用する。

## データベース:
### Amazon DynamoDB
### Amazon ElastiCache
### Amazon RDS


## デベロッパーツール:
### AWS CodeArtifact
[https://www.youtube.com/watch?v=rqy_wluHDe0&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=rqy_wluHDe0&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- ソフトウェアパッケージを安全に保存、公開、共有できるようにするプライベートリポジトリサービス
- 以下のパブリックなリポジトリから登録することができる
    - Maven Central store
    - google android store
    - gradle plugin store
    - npm store
    - nuget store
    - pypi store
- 上記のパブリックなリポジトリから取得したパッケージをキャッシュしておくことができる
- また独自のパッケージも登録しておくことができる

### AWS CodeBuild
[https://www.youtube.com/watch?v=Zzv1_ztf-B0&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=Zzv1_ztf-B0&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- AWS CodeBuild はクラウドで動作する、完全マネージド型のビルドサービスです。
- CodeBuild はソースコードをコンパイルし、単体テストを実行して、すぐにデプロイできるアーティファクトを生成します。
- ビルド対象のソース
    + S3
    + Code Commit
    + GitHub
    + GitHub Enterprise
    + Bitbucket
- ビルドの開始方法は
    + 定期的なビルド
    + 手動ビルド
    
- ビルド方法は、buildspec.ymlに記述する
    ```yaml
    version: 0.2

    phases:
      install:
        runtime-versions:
          java: corretto8
      build:
        commands:
          - echo Build started on `date`
          - mvn test 
      post_build:
        commands:
          - echo Build completed on `date`
          - mvn package
    artifacts:
      files:
        - target/test-1.0-SNAPSHOT.jar
      discard-paths: yes
    ```
- `buildspec.yml`の構造
    - `version:` buildspecのバージョン
    - `run-as:` コマンドを実行するLinuxユーザ
    - `env:`    環境変数
    - `proxy:`  プロキシサーバ
    - `batch:`  バッチ設定
    - `phase:`  実行するコマンド
    - `reports:`    テキストレポート作成
    - `artifacts:`  CodeBuildの出力
    - `cache:`  キャッシュ設定

- Commit等のタイミングでSNSトピック又はAWS Chatbotに通知することができる
- `AWS CodeBuild エージェント`を用いてローカルでも同様のビルドができる
- ビルドの成果物(Artifact)はS3に保存することができる

### AWS CodeCommit
[https://www.youtube.com/watch?v=rqy_wluHDe0&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=rqy_wluHDe0&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)
- Gitベースのリポジトリを提供するサービス
- HTTPS又はSSHでリポジトリにアクセスできる
- CodeCommitのユーザ管理はIAMと統合されている
  つまり、IAMユーザ毎にGITへのアクセス権を設定する
- Commit等のタイミングでSNSトピック又はAWS Chatbotに通知することができる
- Lambdaのイベントトリガーとしても指定することができる

### AWS CodeDeploy
[https://www.youtube.com/watch?v=cXa2S2kS0TU&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=cXa2S2kS0TU&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- アプリケーションをデプロイするためのサービス
- **デプロイ先は**
    + Amazon EC2 インスタンス
    + オンプレミスインスタンス
    + サーバーレス Lambda 関数
    + Amazon ECS サービス
- デプロイするリソースは以下に保存されているアプリケーション
    + Amazon S3 バケット
    + GitHub リポジトリ
    + Bitbucket リポジトリ
- デプロイ方法はAppSpecファイル(yaml/json)に記述する
- デプロイタイプ
    + In-Place  稼働中の環境を新しいアプリケーションで更新する。
    + Blue-Green 現バージョンとは別の新しい実行環境を用意してリクエスト送信先を切り替える。
- デプロイ設定 EC2/オンプレミス
    + One-at-a-time 一台ずつ入れ替えていく
    + Custom    一度に指定した割合まで入れ替えて、次に100％入れ替える
    + Half-at-a-time    一度に50％入れ替えて、次に100％入れ替える
    + All-at-once   一度に全て入れ替える
- デプロイ設定 Lambda/EC2
    + Canary 最初は10％で、数分後に全てなど、2段階でトラフィックを変更する
    + Linear 毎分同じ割合でトラフィックを増やしていく
    + All at once すべてのトラフィックを一度に切り替える

### Amazon CodeGuru
[https://www.youtube.com/watch?v=OF-NfrcgR-o&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=OF-NfrcgR-o&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- CodeGrue ReviewerとCodeGrue Profilerから構成される。
- CodeGrue Reviewerはソースコードのレビューを行う。
- CodeGrue Profilerは実行時のプロファイリングを行う。
- 対象はJavaのみである。
- CodeGrue ProfilerはJVMに組み込まれる形でプロファイリングが実行される。
- CodeGrue ReviewerはCodeCommitやGitHub等のリポジトリと関連付けされる。
- リポジトリのPullRequestが発行されると、CodeGrue Reviewerが分析を行い、レコメンドを行う。

### AWS CodePipeline
[https://www.youtube.com/watch?v=31-w23SdOAs&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=31-w23SdOAs&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- CI/CDのフローを作成するツール
- フローはパイプラインと呼ぶ
- パイプラインの各中身は、CodeCommit、CodeBuild、CodeDeployなどであるが、その他のAWSのサービスやJenkins等の3rdパーティのサービスも使える。
- 各パイプライン内のアクションで、Lambdaを呼び出すこともできる。

### AWS CodeStar
[https://www.youtube.com/watch?v=31-w23SdOAs&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=31-w23SdOAs&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- CI/CDのフローやリポジトリ、ビルド方法などをウィザード形式で簡単に作るためのサービス
- フローはCodePipelineが使われる。
- CodePipelineと同様に複雑のフローを作成することができる。

### AWS Fault Injection Simulator
[https://www.youtube.com/watch?v=_yQzKkQzG9c&ab_channel=CloudDeepDive](https://www.youtube.com/watch?v=_yQzKkQzG9c&ab_channel=CloudDeepDive)

- カオスエンジニアに関連したサービスである。
- EC2インスタンスを停止したり、CPUやメモリ使用量を急増させたり、ネットワークを遮断したり、非常に過酷な状況を発生させて、システムの動作を確認するためのシミュレータである。
- どのような事象を発生させるかは、テンプレートに記述することで指定する。
- FISを通じて、パフォーマンスの問題や未知の問題を発見し解決する。

### AWS X-Ray
[https://www.youtube.com/watch?v=oVVTS1NgWSQ&t=623s&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=oVVTS1NgWSQ&t=623s&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- 複数のマイクロサービス連携させた処理をトレースして解析するためのサービス。
- 複数のマイクロサービスの呼び出しをトレースログで管理する。
- 呼び出しのつながりをトレースに残すために、特殊なIDをログに埋め込む。
- LambdaやEC2などのサービスでは、プログラムを改造してX-Ray用のログを取得するためのAPIを呼び出す必要がある。
- かなり難易度が高そうなサービスである。

### AWS Cloud9
[https://www.youtube.com/watch?v=3Sl6Hzcw7Bk&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F](https://www.youtube.com/watch?v=3Sl6Hzcw7Bk&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)

- 統合開発環境をWebブラウザを通して利用できるサービス
- 

## マネジメントとガバナンス:
### AWS CloudFormation


### Amazon CloudWatch


## ネットワークとコンテンツ配信:
### Amazon API Gateway
### Amazon CloudFront
### Elastic Load Balancing

## セキュリティ、アイデンティティ、コンプライアンス:
### Amazon Cognito
### AWS Identity and Access Management (IAM)
### AWS Key Management Service (AWS KMS)

## ストレージ:

### Amazon S3
[Amazon S3/Glacier](https://www.youtube.com/watch?v=oFG5kMZjKtc&t=183s&ab_channel=AmazonWebServicesJapan%E5%85%AC%E5%BC%8F)


