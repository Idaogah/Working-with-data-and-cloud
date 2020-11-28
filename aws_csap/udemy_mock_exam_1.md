---
title: Udemy Mock Exam 1
url: https://www.udemy.com/course/aws-53225/learn/quiz/4723906#overview
tags: aws-csap, amazon-web-services
---

# 1st time
## 1. ソリューションアーキテクチャ
フロー
- 子プロセスを作成
- 画像識別による一時的な評価と人の目による二次的評価を実施
- 親プロセスに評価結果を返信するプロセスが必要
=> AWS Step Functions

## 2. 移行方法とライセンス管理方法
- DB2、SAP、Windowsオペレーティングシステムサーバーなどのライセンスを使用
- これらのライセンスごとAWSに移行する予定
=> RDS, Windows AMI, AWS License Manager

## 3. 権限
- サードパーティのWEBアプリケーションを使用したい
- アカウント内で実行されているEC2インスタンスへのAPIコマンドを発行するアクセス権が必要
- 第三者による目的外利用ができない形式で権限を付与することが必須
=> AWS IAM, Amazon API Gateway

## 4. EC2のアクセス制限
- 社内用業務WEBアプリケーション
- パブリックサブネットに設置されたWebサーバー、EC2インスタンスで構成
- オープンなインターネットから誰でもアクセスできるようにはしたくはない
- パッチ更新のために
  - インバウンドリクエストを受ける
  - 特定のURLへのアウトバウンドリクエストのみに限定
=> Security Group

## 5. 最もコスト効率がよく最適なアーキテクチャ
ニュースメディア配信アプリケーションの構成
- 東京リージョン
- EC2インスタンス
  - ワイヤレスセンサーの管理サーバー
    - Javaフロントエンド
  - Linux バックエンドサーバー ... AZ障害により停止
	- Javaバックエンド
	- MySQLデータベース
    - 1つのAZしか利用していない（NATゲートウェイも同じ）
	- インターネットに接続してパッチをダウンロードすることが必要
	  - セキュリティ上の理由からジャンプホストへのSSHポートのみを開くことが要件
=> Multi-AZ, NAT Gateway, ELB, Bastion host, Security Gropu

## 6. IPv6の設定
WEBサービスの構成
- IPv4 CIDR（10.0.0.0/16）のVPC内に設置
  - 2つのパブリックサブネット
    - プライベートサブネットからのインターネットへの返信処理のためにNATゲートウェイを配置
  - 2つのプライベートサブネット
- データベース用のEC2インスタンス
  - NATゲートウェイに関連付けたルートテーブルを持つプライベートサブネットに配置
社内方針
- IPv6によるIP管理が実施
- 上記アプリケーションでも今後はIPv6を利用した構成に変更することが求められている
=> Egress-only Internet Gateway, Amazon VPC

## 7. AWSへの移行
現状
- WEBアプリケーション
  - オンプレミス環境
  - VMware環境内の高度にカスタマイズされたWindows VM上でアプリケーション実行
  - オンプレミスサーバーの老朽化
期待
- 新サーバーに切り替えるタイミングでAWSに移行
制約
- この移行は週末の土日2日間で実施
- 効率的で迅速な対応が不可欠
=> AWS Server Migration Service

## 8. データベース移行方法
現状
- ニュースメディア配信アプリケーション
  - EC2インスタンス
  - ELB
  - Auto-Scalingグループ
  - MySQL
期待
- MySQLからPosgreSQLへとデータベースを移行
=> AWS Database Migration Service

## 9. AWS Lambda
新規事業用アプリケーション
- RDS MySQL
  - 多数の顧客の基本情報
  - これまでの売買記録
  - 分析などに利用予定
- Lambda
  - RDSのコネクション接続
=> AWS Lambda, RDS Proxy

## 10. ロケーションベースのアラート機能
グローバルな国際決済サービス
- iOSおよびAndroidモバイル
- 飲食店クーポンモバイルアプリ
期待
- ロケーションベースのアラート機能を追加
  - GPSを利用して近隣店舗に近づいた際に
    - その店舗を紹介するチャットボットによるレコメンデーション
    - クーポン提示
=> Amazon DynamoDB, Amazon EC2, Amazon SQS, AutoScaling, AWS Lambda, Amazon Lex 

## 11. AWS Storage GatewayとCHAP認証
AWSとオンプレミスサイトを併用したハイブリッドアーキテクチャ
- AWS Storage Gatewayのボリュームゲートウェイ
- iSCSIを介したハイブリッド構成のデータ共有システム
- セキュリティ上の問題
  - データ共有システムのネットワークに対するDDoS攻撃
  - 不正アクセス
  - 不正傍受
=> AWS Storage Gateway, CHAP

## 12. AWS LambdaとAmazon Rekognition API
動物画像検索アプリ (画像識別)
- ユーザーが動物写真などをアップロード、類似した動物を画像検索
- 一連の写真をアップロード、特定の画像が利用された時間を検索
期待
- アプリケーションの開発と運用を低コストに実施
=> AWS Lambda, Amazon Rekognition API, Lambda -> S3

## 13. Amazon Transit Gatewayをつかったセキュリティ構成
AWS上の社内システム
- セキュリティ強化
  - 利用するVPCに侵入検知・防止システムを実装
- システム要件
  - VPC内で実行されている数百にも及ぶインスタンスを拡張できる機能が必要
  - 現在VPCは12個起動
期待
- まとめてモニタリングする効率的な方法
=> AWS Transit Gateway, Amazon VPC, IDS/IPS

## 14. AWS Snowball Edge Storage Optimizedをつかったデータ移行
要件
- オンプレのインフラとアプリケーション全般をAWSクラウドに移行
  - 合計150TBのデータ
    - タイムリーかつ費用対効果の高い方法でS3バケットに移動する必要がある
  - 既存のインターネット接続の空き容量を使用
    - データをAWSにアップロードするのには1週間以上かかると予測
=> AWS Snowball Edge Storage Optimized


## 15. AWSでスケーラビリティと弾力性を高める
3層ウェブアプリケーションとなっているニュースサイト
- オンプレミスでデプロイ
要件
- スケーラビリティと弾力性を高めるためにAWSに移行
  - トラフィック変動に自動的に対応する必要あり
    - Web層とアプリケーション層を組み合わせた読み取り専用のニュースレポートサイト
    - 予測不可能な大規模なトラフィック要求を受け取るデータベース層
=> ELB, AutoScaling, ElastiCache Redis, CloudWatch, RDS Read/Replica

## 16. AWS Security Token Service
モバイルアプリケーション
- ユーザーがS3バケット内のデータを利用する際に一時認証を利用
  - STSを利用して一時的な認証情報を取得しユーザーに渡す
  - ただし一部の一時認証情報のアクセス権限が間違っているため必要なリソースへのアクセスが提供されていないことが判明
要件
- 一時認証によって付与されたアクセス権を取り消す方法
=> AWS IAM, AWS Security Token Service

## 17. DDos攻撃対策
社内アプリケーションに対するDDoS攻撃によって大規模なシステム障害が発生
- DDoS攻撃などの外部攻撃を軽減
- 具体的に防止するべき攻撃リスト
  - DDoS攻撃
  - SYNフラッド
  - UDPリフレクション攻撃
  - SQLインジェクション
  - クロスサイトスクリプティング
  - 不正IP取得によるアカウントアクセス
  - ネットワーク情報の取得
=> AWS Shield Standard/Advanced, Amazon Route 53 (shuffle sharding, anycast striping)

## 18. AWS CloudFormation
S3とRDSを利用したデータ共有アプリケーション
- CloudFormationテンプレートを利用
- 画像をS3バケットに保存
- RDSに顧客データを記録
要件
- サービスを停止
  - いつでも再開できるように準備が必要
  - インフラを終了
  - 同時にデータを保持
=> AWS CloudFormation DeletionPolicy (S3 retain), RDS (snapshot)

## 19. 誤操作対策
AWS上にエンタープライズシステム
- システムが突如停止するという障害が発生
  - 一人のエンジニアが本番環境のEC2インスタンスを誤って終了
  - 実稼働するアプリケーションにアクセスできる開発者が多数存在
=> AWS IAM, Amazon EC2, Amazon VPC

## 20. Amazon CloudFrontの配置
美術鑑賞向けSNSサービス「PINTORアプリケーション」
- CloudFrontディストリビューションを使用
  - コンテンツ読み込み時間を短縮
- 配信されるデータは個人情報も多い
要件
- アプリケーションからユーザーへのCloudFront配信においてHTTPS通信を実施
- CloudFrontのビューアリクエストの割合を増やすことにより
  - パフォーマンスを改善
  - コストを抑える
=> AWS Certificate Manager, Amazon CloudFront (Cache-control max-age directive)

## 21. Amazon EC2のパフォチュー
顧客管理向けのJavaアプリケーション
- WEBサーバーにEC2
  - 約40％のCPU使用率に相当する一定のワークロード
  - オンデマンドEC2インスタンスのフリート
  - インスタンスは複数利用
    - 通信を最適化することが求められている
- RDSに顧客の構成情報データ
=> T3, プレイスメントグループ, 拡張ネットワーク

## 22. SSL証明書の適用
モバイルアプリケーション
- EC2インスタンス
  - AutoScalingグループ
  - ELB
- セキュリティポリシー
  - インスタンスからVPC内の他のサービスへの全てのアウトバウンド接続について
    - インスタンスアクセス時に一意のSSL認証が利用される必要がある
=> AWS Certificate Manager, Elastic Load Balancer

## 23. VPCエンドポイントをつかったS3との連携
顧客管理システム
- AWSパブリッククラウド
- ２層アプリケーション
- EC2
  - データ処理サーバー
  - S3
    - データ保存と管理
    - S3との間において毎秒5 Gbpsを超えるデータを送信
    - プライベートサブネットのアプリケーションレイヤーからS3にデータを転送
  - インスタンスの処理にはサードパーティーのソフトウェアが利用
    - 定期的にソフトウェアに対するパッチ更新が必要
=> Amazon EC2, Amazon S3 (bucket policy), VPC Endpoint

## 24. データ共有システム
社内データ共有システム
- データセンターにホストして運用
- 社内データ
  - データセンターのストレージに保存
  - 中長期保存用のため迅速なデータ抽出は必要ない
  - データ処理のためにOSSメッセージングシステムを利用したジョブ管理を行っている
  - データはテープライブラリによってアーカイブされる構成をオンプレミスで実施
要件
- これらのシステムをAWSに移行
=> EC2 (Auto Scaling, Spot Instance), Amazon SQS, Amazon S3 Glacier

## 25. IDフェデレーション
エンタープライズシステムのデータセンター
- AWSクラウドに拡張するハイブリッドクラウドインフラストラクチャ
  - オンプレミス側とクラウド側で2つの個別のログインアカウントを持つ
    - 複数の資格情報を保存することを避ける必要がある
要件
- AWSリソースを管理する構成
  - 社内アカウントを使用して既にサインインしているオンプレミスユーザーが個別のIAMユーザーを作成しないこと
=> AWS IAM (Id Provider OpenID), AWS Security Token Service (AssumeRoleWithWebIdentity)

## 26. ECSのオートスケーリング
エンタープライズアプリケーション
- Amazon ECSを使用したDockerベースの
- マルチAZ構成でリードレプリカを持つRDS MySQL
  - 高可用でスケーラブル
要件
- アプリケーション層にスケーラビリティを確保
  - ECSクラスターに対するオートスケーリング設定を実施
=> AWS CloudWatch (Capacity Provider Reservation), AWS Auto Scaling, Amazon ECS (AWS ECS Cluster Auto Scaling)

## 27. AWS Resource Access Manager
社内の統合管理のために全社共通のIT運用部門
- AWS Organizationsを使用
  - マルチアカウントおよびマルチリージョンを管理
  - AWSアカウントAとAWSアカウントBとAWSアカウントC
要件
- クロスアカウント処理が必要となる定期タスクの自動化設定
  - アカウントAのユーザーがアカウントBのEC2インスタンスへのアクセスを定期的に実施
=> AWS Resource Access Manager (enable-sharing-with-aws-organizations), AWS Organizations (trusted access)

## 28. 安全なECS環境
CI/CD環境
- 開発環境などは全てDocker
- Fargate起動タイプを使用するAmazon ECSクラスター
要件
- 社内製品を販売するためのECサイトを構築
- ECSでの実装
  - 環境変数を使用
- 顧客データベースの資格情報をECサイトに提供する必要があり
  - セキュリティを徹底
    - 資格情報がデータ保持とイメージ転送が安全であることが保障
    - かつクラスター自体で表示できないように
=> Systems Manager (parameter store), KMS, ECS, Secrets Manager, IAM

## 29. VPN
ウェブベースの会計アプリケーション
- フロントサーバー群はAWSのパブリックサブネット上で利用
- 社内のネットワークからのみAWSサイト間VPN接続によって利用
要件
- 会社ではSOHOを推進
  - 外部Wifiがある環境であればどこからでもリモートで接続して作業ができる機能を実装
	- 下記事案に関してセキュリティ性能をできる限り高める
      - 外部からのアクセスが頻繁に発生すること
      - 機密性の高いデータを扱っていること
=> Amazon VPC (Private Subnet <- NAT Gateway <- Public Subnet), VPN (OpenVPN)