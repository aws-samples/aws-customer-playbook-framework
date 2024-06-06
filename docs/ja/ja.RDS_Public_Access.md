# インシデントレスポンス Playbook: パブリックリソースエクスポージャー-RDS
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2024 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`\
承認者:`承認者名`\
最終承認日:

## エグゼクティブサマリー
この Playbook では、パブリックリソースの所有者を特定し、公開中にそれらのリソースにアクセスした可能性のあるユーザーの特定、リソースへのアクセスの取り消しの影響の判定、およびパブリックアクセシビリティの根本原因を特定するプロセスの概要を説明します。

## 妥協の潜在的な指標
-AWS サービスダッシュボードからのパブリックアクセスの警告
-CloudTrail イベント名「パブリックアクセス可能」
-リソースへのパブリックアクセスに関するセキュリティ研究者からの通知
-パブリックインターネットプロトコル (IP) アドレスからのリソースの削除

### 目標
Playbook の実行全体を通して、インシデント対応機能の強化についてメモを取って、_***望ましい結果***_ に焦点を当てます。

#### 決定:
* **脆弱性が悪用された**
* **エクスプロイトとツールが観察された**
* **俳優の意図**
* **俳優の帰属**
* **環境とビジネスに与えたダメージ**

#### 回復:
* **オリジナルで強化した構成に戻す**

#### CAF セキュリティパースペクティブの強化コンポーネント:
[AWS Cloud Adoption Framework セキュリティの視点] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **指令**
* **探偵**
* **レスポンシブ**
* **予防**

! [画像] (/images/aws_caf.png)
* * *

### レスポンスステップ
1. [**準備**] 資産インベントリを作成する
2. [**準備**] RDS インスタンスインベントリを作成する
3. [**準備**] RDS セキュリティおよびロギングチェックの確立
4. [**準備**] RDS のアクセスとログ分析を特定して評価するためのトレーニングプログラムを実装する
5. [**準備**] エスカレーション手順の特定、文書化、およびテストのエスカレーション手順 [**検出と分析**]
6. [**検出と分析**] インスタンスチェックの実行
7. [**検出と分析**] RDS パブリックリソースの CloudTrail を確認する
8. [**検出と分析**] VPC Flow Logs の確認
９。 [**検出と分析**] RDS エンドポイント/ホストベースのログを確認する
10. [**封じ込め**] RDS パブリックエクスポージャーを含める
11. [**ERADICATION**] 認識されていない、または許可されていないパブリックスナップショットまたはデータベースをすべて削除する
12. [**準備**] 追加の予防措置:RDS セキュリティチェック
13. [**準備**] 追加の予防措置:セキュリティコントロールポリシー-RDS 暗号化
14. [**準備**] 追加の予防措置:AWS Config
15. [**準備**] 追加の予防措置:全体的なセキュリティ姿勢

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: AWS サービスパブリックアクセス
* **カテゴリ**: パブリックアクセス
* **リソース**: RDS
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス
* **ログソース**: RDS Database Log Files, CloudTrail, CloudWatch
* **チーム**: セキュリティオペレーションセンター (SOC)、フォレンジック調査官、クラウドエンジニアリング

## インシデント処理プロセス
### インシデント対応プロセスには、次の段階があります。
* 準備
* 検出と分析
* 封じ込めと根絶
* 回復
* インシデント後の活動

## 準備
この Playbook では、可能な場合は、AWS のセキュリティ評価、監査、強化、インシデント対応に役立つコマンドラインツールである [Prowler] (https://github.com/toniblyx/prowler) を参照し、統合します。

CIS Amazon Web Services Foundations Benchmark（49チェック）のガイドラインに従い、GDPR、HIPAA、PCI-DSS、ISO-27001、FFIEC、SOC2などに関連する100以上の追加のチェックがあります。

このツールは、お客様の環境内の現在のセキュリティ状態のスナップショットを迅速に提供します。 代わりに、[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) はコンプライアンススキャンを自動化し、[Prowler と統合] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### アセットインベントリ
既存のリソースをすべて特定し、資産インベントリリストとそれぞれの所有者を組み合わせて更新する

#### RDS インスタンスインベントリ
-RDS インスタンスをインベントリするには、AWS API [describe-db-instances] (https://docs.aws.amazon.com/cli/latest/reference/rds/describe-db-instances.html) を使用して、特定のリージョン内のすべてのインスタンスの名前を一覧表示します。`aws rds describe-db-instances —リージョン us-east-1 —query 'dbInstances [*]。 [dbInstance識別子、ReadReplicadbInstanceIdentifiers] '`

#### RDS のセキュリティとログのチェック
-RDS インスタンスのストレージが暗号化されているかどうかを確認します:`. /prowler-c extra735`
-RDS インスタンスでバックアップが有効になっているかどうかを確認します:`. /prowler-c extra739`
-RDS インスタンスが CloudWatch ログと統合されているかどうかを確認する:`。 /prowler-c extra747`
-RDS インスタンスでマイナーバージョンのアップグレードが有効になっていることを確認します:`。 /prowler-c extra7131`
-RDS インスタンスで [拡張モニタリング] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_Monitoring.OS.html) が有効になっているかどうかを確認します:`。 /prowler-c extra7132`
-RDS セキュリティチェック:`. /prowler-g group13`

### トレーニング
-「社内のアナリストが AWS API（コマンドライン環境）、S3、RDS、その他の AWS サービスに慣れるためのトレーニングはどのようなものですか？ `
>>>
脅威検出とインシデント対応の機会には、次のものがあります。\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-「アカウント内のサービスを変更できるロールはどれですか？ `
-`それらのロールが割り当てられているのはどのユーザーですか？ 最小特権は守られていますか、それともスーパー管理者ユーザーが存在しますか？ `
-「セキュリティアセスメントが環境に対して実行されていますか。「新しい」または「疑わしい」ものを検出するための既知のベースラインはありますか？ `

### 通信技術
-「チーム/会社内で問題を伝えるためにどのような技術が使われていますか？ 自動化されたものはありますか？ `
>>>
電話\
電子メール\
AWS SES\
AWS SNS\
スラック\
チャイム\
その他？
>>>

## 検出

### RDS インスタンスチェック

#### AWS Config
AWS Config には、いくつかの [RDS インスタンスを評価するためのマネージドルール] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* rds-automatic-minor-version-upgrade-enabled
* rds-cluster-deletion-protection-enabled
* rds-cluster-iam-authentication-enabled
* rds-cluster-multi-az-enabled
* rds-enhanced-monitoring-enabled
* rds-instance-deletion-protection-enabled
* rds-instance-iam-authentication-enabled
* rds-instance-public-access-check
* rds-in-backup-Plan
* rds-logging-enabled
* rds-multi-az-support
* rds-resources-protected-by-backup-plan
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted
#### Prowler
-パブリックアクセス可能な RDS インスタンスがないことを確認します:`. /prowler-c extra78`
-RDS スナップショットとクラスタースナップショットがパブリックであるかどうかを確認する:`. /prowler-c extra723`
-外部エンティティと共有されている Amazon S3 バケットや IAM ロールなど、組織内のリソースとアカウントを特定します:`。 /prowler-c extra769`
-インターネットに公開されているリソースを見つける:`. /prowler-g group17`

## エスカレーション手順
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 分析
セキュリティインシデントイベント管理（SIEM）ソリューション（Splunk、ELK stack など）にログをエクスポートして、より完全な攻撃タイムライン分析のためにさまざまなログを表示および分析することを強く推奨します。

### CloudTrail: RDS Public
1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail ログで「ModifyDBInstance」、「ModifyDbSnapshotAttribute」、または「ModifyDBClusterSnapshotAttribute」のイベント名を確認し、パブリック IP アドレスから `publilyAccessible` イベントの値を探してください

### VPC Flow Logs
VPC Flow Logs は、VPC 内のネットワークインターフェイスとの間で送受信される IP トラフィックに関する情報をキャプチャできる機能です。 これは、CloudTrail 内で検出された IP アドレスがパブリックリソースへの外部接続のタイプを判断する場合に便利です。

Athena でのクエリを含む詳細な情報と手順については、[VPC Flow Logs 用の AWS ドキュメント] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html) を参照してください。 Athena 分析は別のプレイブックに含まれ、他の関連項目にリンクすることをお勧めします。

### エンドポイント/ホストベース
-EC2 オペレーティングシステムとアプリケーションログで、不適切なログイン、不明なソフトウェアのインストール、または認識されないファイルの存在を確認します。

-サードパーティのホストベースの侵入検知システム（HIDS）ソリューション（OSSEC、Tripwire、Wazuh、[Amazon Inspector] (https://aws.amazon.com/inspector/)、その他) を使用することを強く推奨します。

## 封じ込め

### RDS パブリックエクスポージャー
VPC 内で DB インスタンスを起動すると、DB インスタンスには VPC 内のトラフィック用のプライベート IP アドレスが割り当てられます。 このプライベート IP アドレスはパブリックにアクセスできません。 パブリックアクセスオプションを使用して、DB インスタンスにプライベート IP アドレスに加えてパブリック IP アドレスがあるかどうかを指定できます。

次の図は、[追加の接続設定] セクションの [パブリックアクセス] オプションを示しています。 オプションを設定するには、[接続] セクションの [追加の接続設定] セクションを開きます。

! [画像] (/images/VPC-example.png)

## 撲滅

### RDS 未承認/認識されないリソース
認識されていない、または許可されていないパブリックスナップショットまたはデータベースを削除する

1. AWS マネジメントコンソールにサインインし、https://console.aws.amazon.com/rds/ で Amazon RDS コンソールを開きます。
1. ナビゲーションペインで、[スナップショット] を選択します。
1. [手動スナップショット] リストが表示されます。
1. 削除する DB スナップショットを選択します。
1. [アクション] で、[スナップショットの削除] を選択します。
1. 確認ページで [Delete] を選択します。

## リカバリ
撲滅のために記載されている手順と同じ手順

## 予防措置

### RDS セキュリティチェック
-RDS セキュリティチェック:`. /prowler-g group13`

### セキュリティコントロールポリシー:RDS 暗号化
[必須の RDS 暗号化を強制する] (https://medium.com/@cbchhaya/aws-scp-to-mandate-rds-encryption-6b4dc8b036a)

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) には、[rds-snapshots-パブリック禁止] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html) を含むパブリックアクセスから保護するための複数の自動化されたルールがあります

### 全体的なセキュリティポスチャ
環境に対して [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) を実行して、この Playbook では特定されない他のリスクおよび潜在的に他のパブリックエクスポージャーをさらに特定します。

## 学んだ教訓
「これは、必ず「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要な、貴社固有のアイテムを追加する場所です。 `

## アドレス指定バックログ項目
-インシデントレスポンダーとして、誤って公開されたリソースを緩和する方法に関するRunbookが必要です
-インシデントレスポンダーとして、パブリックリソース（AMI、EBS ボリューム、ECR リポジトリなど）を検出できる必要があります
-インシデントレスポンダーとして、AWS 内で重要な変更を加えることができるロールを知る必要があります
-インシデントレスポンダーとして、すべてのスナップショット（RDS、EBS、ECR）に暗号化が必要であることを確認する必要があります

## 現在のバックログアイテム