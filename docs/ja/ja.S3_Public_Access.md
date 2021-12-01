# インシデント対応 Playbook: パブリックリソースエクスポージャー-S3
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるアマゾンウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2021 アマゾンウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、アマゾンウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

## エグゼクティブサマリー
この Playbook では、パブリックリソースの所有者を特定し、公開中にそれらのリソースにアクセスした可能性のあるユーザーの特定、リソースへのアクセスの取り消しの影響の判定、およびパブリックアクセシビリティの根本原因を特定するプロセスの概要を説明します。

## 妥協の潜在的な指標
-AWS サービスダッシュボードからのパブリックアクセスの警告
-CloudTrail `GetPublicAccessBlock`, `deletePublicAccessBlock `, `getObjectacl`, `putObjectacl`
-リソースへのパブリックアクセスに関するセキュリティ研究者からの通知
-パブリックインターネットプロトコル (IP) アドレスからのリソースの削除

## AWS GuardDuty の潜在的な調査結果
-Discovery: s3/maliciousipCaller
-Discovery: s3/maliciousipCaller.Custom
-Discovery: s3/toripCaller
-exfiltration: s3/maliciousipCaller
-exfiltration: s3/objectRead。珍しい
-影響:s3/maliciousipCaller
-pentest: s3/kalilinux
-pentest: s3/ParrotLinux
-pentest: s3/pentoolLinux
-ポリシー:s3/accountBlockパブリックアクセス無効
-ポリシー:s3/bucket匿名アクセス付与済み
-ポリシー:s3/bucketBlockPublic Access無効
-ポリシー:S3/bucketPublic Access付与
-ステルス:S3/サーバーアクセスロギング無効
-不正アクセス:s3/maliciousipCaller.Custom
-不正アクセス:s3/toripCaller

### 目標
Playbook の実行全体を通して、インシデント対応機能の強化についてメモを取って、_***望ましい結果***_ に焦点を当てます。

#### 決定する:
* **脆弱性が悪用された**
* **エクスプロイトとツールが観察された**
* **俳優の意図**
* **俳優の帰属**
* **環境とビジネスに与えたダメージ**

#### リカバリ:
* **オリジナルで強化した構成に戻す**

#### CAF セキュリティパースペクティブの強化コンポーネント:
[AWS クラウド導入フレームワークセキュリティの視点] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **指令**
* **探偵**
* **レスポンシブ**
* **予防**

! [画像] (/images/aws_caf.png)
* * *

### レスポンスステップ
1. [**準備**] アカウントの作成資産インベントリ
2. [**準備**] S3 バケットインベントリを作成する
3. [**準備**] 適宜ロギングを有効にする
4. [**準備**] 各バケットに含まれるデータの種類を特定する
5. [**準備**] エスカレーション手順を特定、文書化、およびテストする
6. [**準備**] DOS/DDoS攻撃に対処するためのトレーニングを実装する
7. [**検出と分析**] S3 バケットチェックの実行
8. [**検出と分析**] CloudTrail: パブリック S3 バケットのレビュー
9. [**検出と分析**] CloudTrail: パブリック S3 オブジェクトのレビュー
10. [**検出と分析**] VPC フローログの確認
11. [**検出と分析**] エンドポイント/ホストベースのレビュー
12. [**包含**] S3 ブロックパブリックアクセス
13. [**ERADICATION**] S3 は認識されない/許可されていないオブジェクトを削除
14. [**リカバリ**] 必要に応じてリカバリ手順を実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: AWS サービスパブリックアクセス
* **カテゴリ**: パブリックアクセス
* **リソース**: S3
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス
* **ログソース**: S3 サーバーログ、S3 アクセスログ、CloudTrail、CloudWatch
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

CISアマゾンウェブサービス財団ベンチマーク（49チェック）のガイドラインに従い、GDPR、HIPAA、PCI-DSS、ISO-27001、FFIEC、SOC2などに関連する100以上の追加のチェックがあります。

このツールは、お客様の環境内の現在のセキュリティ状態のスナップショットを迅速に提供します。 さらに、[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) はコンプライアンススキャンを自動化し、[Prowler と統合] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### アセットインベントリ
既存のリソースをすべて特定し、資産インベントリリストとそれぞれの所有者を組み合わせて、更新された資産インベントリリストを作成します。

#### S3 バケットインベントリ
-AWS API [list-buckets] (https://docs.aws.amazon.com/cli/latest/reference/s3api/list-buckets.html) を使用して、すべての Amazon S3 バケットの名前を（すべてのリージョンにわたって）表示します。`aws s3api list-buckets —クエリ「バケット [] .Name" `

#### ログを有効にする
-CloudTrail: `で S3 バケットでサーバーレベルのログが有効になっているかどうかを確認します。 /prowler-c extra718`
-S3 バケットで CloudTrail: `でオブジェクトレベルのログが有効になっているかどうかを確認します。 /prowler-c extra725`

#### 各バケットに含まれるデータの種類を特定する
**オプションA**
-S3 バケットのすべてのファイルを表示するには、コマンド `aws S3 ls s3: //your_bucket_name —recursive`
-**注** この API 呼び出しは最初の 1000 個の一致に制限され、そのしきい値を超えるオブジェクトは含まれません
-会社にとって重要なバケットとオブジェクトを手動で分類する
-重要なバケットにタグを追加して、メタデータが将来参照できるようにする

**オプションB**
-[Amazon Macie] (https://aws.amazon.com/macie/) は完全マネージド型のデータセキュリティおよびデータプライバシーサービスで、機械学習とパターンマッチングを使用して AWS の機密データを検出して保護します。 Amazon Macie は、機械学習とパターンマッチングを使用して、大規模な機密データをコスト効率よく検出します。

### トレーニング
-「社内のアナリストが AWS API（コマンドライン環境）、S3、RDS、その他の AWS サービスに精通するためのトレーニングはどのようなものですか？ `
>>>
脅威検出とインシデント対応の機会には、次のものがあります。\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[セルフサービスセキュリティ評価] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
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

### S3 バケットチェック
-CloudTrail S3 バケットで S3 バケットアクセスログが有効になっていることを確認します:`。 /prowler-c check26`
-Everyone または Any AWS ユーザーに開かれている S3 バケットがないことを確認します:`. /prowler-c extra73`
-外部エンティティと共有されている Amazon S3 バケットや IAM ロールなど、組織内のリソースとアカウントを特定します:`。 /prowler-c extra769`
-インターネットに公開されているリソースを見つける:`. /prowler-g group17`

## エスカレーション手順
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 分析
セキュリティインシデントイベント管理（SIEM）ソリューション（Splunk、ELK スタックなど）にログをエクスポートして、より完全な攻撃タイムライン分析のためにさまざまなログを表示および分析することを強く推奨します。

### CloudTrail: パブリック S3 バケット
デフォルトでは、CloudTrail は過去 90 日間に行われた API 呼び出しを記録しますが、オブジェクトに対して行われたリクエストはログに記録されません。 CloudTrail コンソールでバケットレベルのイベントを確認できます。 ただし、そこでデータイベント（Amazon S3 オブジェクトレベルの呼び出し）を表示することはできません。CloudTrail ログを解析またはクエリする必要があります。

1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail ログでイベント名 `GetPublicAccessBlock` と `DeletePublicAccessBlock `を確認します

### CloudTrail: パブリック S3 オブジェクト
オブジェクトレベルの Amazon S3 アクションの CloudTrail ログを取得することもできます。 これを行うには、S3 バケットまたはアカウント内のすべてのバケットのデータイベントを有効にします。 アカウントでオブジェクトレベルのアクションが発生すると、CloudTrail は証跡の設定を評価します。 イベントが証跡で指定したオブジェクトと一致する場合、イベントはログに記録されます。

1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail ログでイベント名 `getObjectacl `と `putObjectacl` を確認します。

### VPC フローログ
VPC フローログは、VPC 内のネットワークインターフェイスとの間で送受信される IP トラフィックに関する情報をキャプチャできる機能です。 これは、CloudTrail 内で検出された IP アドレスがパブリックリソースへの外部接続のタイプを判断する場合に便利です。

Athena でのクエリを含む詳細な情報と手順については、[VPC フローログ用の AWS ドキュメント] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html) を参照してください。 Athena 分析は別のプレイブックに含まれ、他の関連項目にリンクすることをお勧めします。

### エンドポイント/ホストベース
1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail でパブリック IP アドレスからの `PutObject` および `DeleteObject `リクエストについて確認します

-EC2 オペレーティングシステムとアプリケーションログで、不適切なログイン、不明なソフトウェアのインストール、または認識されないファイルの存在を確認します。

-サードパーティのホストベースの侵入検知システム（HIDS）ソリューション（OSSEC、Tripwire、Wazuh、[Amazon Inspector] (https://aws.amazon.com/inspector/)、その他) を使用することを強く推奨します。

## 封じ込め

### S3 ブロックパブリックアクセス
aws s3api put-public-access-block —bucket-name-here —public-access-block-configuration「blockPublicacls=true, ignorePublicacls=true, blockPublickets=true, blockPublickets=

また、アカウント全体のパブリック S3 アクセスをブロックする方法の詳細については、[Amazon S3 ストレージへのパブリックアクセスのブロック] (https://aws.amazon.com/s3/features/block-public-access/) をご覧ください。

## 撲滅

### S3 認識されない/許可されていないオブジェクトの削除
認識されないオブジェクトをバケットから削除する

1. AWS マネジメントコンソールにサインインし、https://console.aws.amazon.com/s3/ で Amazon S3 コンソールを開きます。
1. [バケット名] リストで、オブジェクトを削除するバケットの名前を選択します。
1. 削除するオブジェクトの名前を選択します。
1. オブジェクトの現在のバージョンを削除するには、[最新バージョン] を選択し、ごみ箱アイコンを選択します。
1. オブジェクトの以前のバージョンを削除するには、[最新バージョン] を選択し、削除するバージョンの横にあるゴミ箱アイコンを選択します。

## リカバリ
撲滅のために記載されている手順と同じ手順

## 予防措置

### S3 Prowler チェック
**暗号化**
-S3 バケットでデフォルトの暗号化 (SSE) が有効になっているかどうかを確認します:`。 /prowler-c extra734`

**災害復旧**
-S3 バケットでオブジェクトのバージョニングが有効になっているかどうかを確認します:`. /prowler-c extra763`

### S3 サービスアクション
[ユーザーが S3 ブロックパブリックアクセス設定を変更できないようにする] (https://asecure.cloud/a/scp_s3_block_public_access/)

毎月バケットのアクセスとポリシーを定期的に確認し、[CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) またはセキュリティハブを使用して自動検出を行います

[S3 バケットでのバージョニングの使用] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) トップレベルオブジェクトの偶発的または意図的な削除を軽減

[ACL によるアクセスの管理] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) バケットおよびオブジェクトレベルのリソースへの不正アクセスを制限する

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) には、[s3-bucket-level-public-access-禁止] など、パブリックアクセスから保護するための複数の自動化されたルールがあります。https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)。

### 全体的なセキュリティポスチャ
環境に対して [セルフサービスセキュリティ評価] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) を実行して、この Playbook では特定されない他のリスクおよび潜在的に他のパブリックエクスポージャーをさらに特定します。

## 学んだ教訓
「これは、「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要なアイテムを追加するための場所です。 `

## アドレスバックログ項目
-インシデントレスポンダーとして、誤って公開されたリソースを緩和する方法に関するRunbookが必要です
-インシデントレスポンダーとして、パブリックリソース（AMI、EBS ボリューム、ECR リポジトリなど）を検出できる必要があります
-インシデントレスポンダーとして、AWS 内で重要な変更を加えることができるロールを知る必要があります
-インシデントレスポンダーとして、パブリックバケットの露出を軽減するためのプレイブックが必要で、エスカレーションポイントが必要でした
-インシデントレスポンダーとして、さまざまなバケット分類に必要なログに関するドキュメントが必要です

## 現在のバックログアイテム