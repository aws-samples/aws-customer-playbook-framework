このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2021 Amazon ウェブサービス株式会社またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

## エグゼクティブサマリー
この Playbook では、コードリソースの所有者を特定し、公開されたコードにどのようにアクセスしたかを判断し、エクスポージャーの影響を判断し、必要な修復アクション、およびコードエクスポージャーの根本原因を特定するプロセスの概要を説明します。

## 妥協の潜在的な指標
-情報漏えい対策 (DLP) アラート
-既知のコード公開または露出
-疑わしい CloudTrail ログ

## AWS GuardDuty の潜在的な調査結果
-Behavior: EC2/NetworkPortUnusual
-Behavior: EC2/TrafficVolumeUnusual
-CredentialAccess: IAMUser/AnomalousBehavior ior
-DefenseEvasion: IAMUser/AnomalousBehavior or
-Discovery: IAMUser/AnomalousBehavior av
-Exfiltration: IAMUser/AnomalousBehavior
-Exfiltration: S3/MaliciousIPCaller
-Exfiltration: S3/ObjectRead.Unusual
-Impact: IAMUser/AnomalousBehavior ior
-InitialAccess: IAMUser/AnomalousBehavior ior
-Persistence: IAMUser/AnomalousBehavior ior
-PrivilegeEscalation: IAMUser/AnomalousBehavior ior
-UnauthorizedAccess: IAMUser/InstanceCredentialExfiltration.InsideAWS
-UnauthorizedAccess: IAMUser/InstanceCredentialExfiltration.OutsideAWS

### 目標
Playbook の実行全体を通して、インシデント対応機能の強化についてメモを取って、_***望ましい結果***_ に焦点を当てます。

#### 決定する:
* **コピー、移転、出版をコード化**
* **クレデンシャル露出**
* **脆弱性が暴露された**
* **環境インテリジェンスが暴露**
* **使用されるツール**
* **構成情報**
* **俳優の意図**
* **俳優の帰属**
* **その他の環境やビジネスに与えた被害**
* **環境とビジネスに生じるリスク**

#### リカバリ:
* **公開された認証資格情報を期限切れまたはリセット**
* **暴露された環境インテリジェンスに基づいて、潜在的な攻撃ベクトルのリスクを列挙する**
* **エクスポーズされたコードのリスクを最小化または排除**

#### CAF セキュリティパースペクティブの強化コンポーネント:
[AWS Cloud Adoption Framework セキュリティの視点] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **指令**
* **探偵**
* **レスポンシブ**
* **予防的**

! [画像] (/images/aws_caf.png)
* * *

### レスポンスステップ
1. [**準備**] アカウント資産インベントリを作成する
2. [**準備**] リポジトリインベントリを作成する
3. [**準備**] 必要に応じてロギングを有効にする
4. [**準備**] 各リポジトリにあるデータの種類を特定する
5. [**準備**] エスカレーション手順の特定、文書化、およびテストを行う
6. [**準備**] 侵入攻撃に対処するためのトレーニングを実施する
7. [**検出と分析**] エクスフィルトレーションと DLP チェックを実行する
8. [**検出と分析**] リポジトリの確認 (CodeCommit) の読み取りと書き込みアクション
9. [**検出と分析**] DNS ログを確認する
10. [**検出と分析**] VPC フローログを確認する
第11回。 [**検出と分析**] エンドポイント/ホストベースのログを確認する
12. [**CONTAINMENT**] 影響を受けるアカウントのアクセスをブロックする
13. [**ERADICATION**] リポジトリ内の認識されていないオブジェクトや権限のないオブジェクトを削除する
14. [**リカバリ**] 必要に応じてリカバリ手順を実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: データの漏洩、AWS サービスの不正使用
* **カテゴリー**: データ損失
* **リソース**: CodeCommit, S3, EC2
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、CloudWatch Metrics
* **ログソース**: DNS ログ、VPC Flow Logs、CloudTrail、CloudWatch
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

このツールは、お客様の環境内の現在のセキュリティ状態のスナップショットを迅速に提供します。 さらに、[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) はコンプライアンススキャンを自動化し、[Prowler と統合] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### アセットインベントリ
既存のリソースをすべて特定し、資産インベントリリストと各リソースの所有者を組み合わせて更新します。 ソースコードは、次のいずれかのアセットに格納できます。
-(CodeCommit) [https://aws.amazon.com/codecommit/] リポジトリ
-(S3) [https://aws.amazon.com/s3/] バックアップコードストレージ
-(EC2) [https://aws.amazon.com/ec2/] セルフホスト型コードストレージメカニズム

#### 各アセットから抽出されたデータを特定する
-S3 に保存されている CloudWatch および CloudTrail ログは、(Amazon Athena) [https://aws.amazon.com/athena/] を使用してクエリを実行でき、望ましくないアクションを特定できます。
-(Amazon GuardDuty) [https://aws.amazon.com/guardduty/] は異常トラフィックを自動的に検出することがあります。 これは、(AWS Security Hub) [https://aws.amazon.com/security-hub/] または (Amazon Detective) [https://aws.amazon.com/detective/] でアクセスできます
-S3 に保存されているアプリケーションおよびインスタンスログは、Athena を使用してクエリすることもできます。

### トレーニング
-「社内のアナリストが AWS API（コマンドライン環境）、CodeCommit、S3、RDS、その他の AWS サービスに精通するためのトレーニングはどのようなものですか？ `
>>>
脅威の検出とインシデント対応の機会は次のとおりです。\
[AWS RE: INFORCE] (https://reinforce.awsevents.com/faq/)\
[Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/)
>>>

-「アカウント内のサービスを変更できるロールはどれですか？ `
-`それらのロールが割り当てられているのはどのユーザーですか？ 最小特権は守られていますか、それともスーパー管理者ユーザーが存在しますか？ `
-`環境に対してセキュリティ評価が行われていますか？ 「新しい」または「疑わしい」ものを検出するための既知のベースラインはありますか？ `

### 通信技術
-「チーム/会社内で問題を伝えるためにどのような技術が使われていますか？ 自動化されたものはありますか？ `
>>>
電話\
電子メール\
SMS\
AWS SES\
AWS SNS\
スラック\
チャイム\
チーム\
その他？
>>>

### DLP 実装

情報漏えい対策 (DLP) ソリューションを実装すると、追加の検出機能とアラートが提供される場合があります。 DLP ソリューションは、EC2 環境やネットワークトラフィックの評価において最大の価値を提供する場合があります。 DLP ソリューションは [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) にあります。

## 検出

### ロギングと S3 バケットチェック
-CloudTrail がすべてのリージョンで有効になっていることを確認します:`。 /prowler-c check21`
-CloudTrail ログファイルの検証が有効になっていることを確認します:`。 /prowler-c check22`
-CloudTrail S3 バケットで S3 バケットアクセスログが有効になっていることを確認します:`。 /prowler-c check26`
-Everyone または Any AWS ユーザーに開かれている S3 バケットがないことを確認します:`. /prowler-c extra73`
-外部エンティティと共有されている Amazon S3 バケットや IAM ロールなど、組織内のリソースとアカウントを特定します:`。 /prowler-c extra769`
-インターネットに公開されているリソースを見つける:`. /prowler-g group17`

### ロギングおよび CodeCommit イベント
-[EventBridge で AWS CodeCommit イベントを監視する] (https://docs.aws.amazon.com/codecommit/latest/userguide/monitoring-events.html)。リアルタイムデータのストリームを配信します。 これらのイベントは、Amazon CloudWatch イベントに表示されるイベントと同じです。Amazon CloudWatch イベントでは、AWS リソースの変更を記述するほぼリアルタイムのシステムイベントストリームが配信されます。
-[CloudTrail 証跡を作成して S3 バケットに CodeCommit イベントを継続的に配信できるようにする] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)。 CloudTrail は CodeCommit のすべての API 呼び出しをイベントとしてキャプチャします。

### DLP アラート

DLP ソリューションを実装すると、追加の検出機能とアラートが提供される場合があります。 詳細については、DLP ソリューションプロバイダーから入手できます。

## エスカレーション手順
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 分析
セキュリティインシデントイベント管理（SIEM）ソリューション（Splunk、ELK stack など）にログをエクスポートして、より完全な攻撃タイムライン分析のためにさまざまなログを表示および分析することを強くお勧めします。

### CloudTrail
CloudTrail は、すべての AWS API 呼び出しに対して最大 90 日間のイベントログを提供します。 この情報は、悪意のあるアクションや異常なアクションを特定して追跡するために使用できます。 詳細については、[CloudTrail ログイベントリファレンス] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-event-reference.html) を参照してください。

### CloudTrail: パブリック S3 バケット
デフォルトでは、CloudTrail は過去 90 日間に行われた API 呼び出しを記録しますが、オブジェクトに対して行われたリクエストはログに記録されません。 CloudTrail コンソールでバケットレベルのイベントを確認できます。 ただし、デフォルトでは、データイベント（Amazon S3 オブジェクトレベルの呼び出し）を表示することはできません。これらのイベントが CloudTrail に表示される前に、オブジェクトレベルのログを有効にする必要があります。

1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します。
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail ログでイベント名 `GetPublicAccessBlock` と `DeletePublicAccessBlock `を確認します

### CloudTrail: パブリック S3 オブジェクト
オブジェクトレベルの Amazon S3 アクションの CloudTrail ログを取得することもできます。 これを行うには、[S3 バケットのデータイベントを有効にする] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/logging-data-events-with-cloudtrail.html) またはアカウント内のすべてのバケット。 アカウントでオブジェクトレベルのアクションが発生すると、CloudTrail は証跡の設定を評価します。 イベントが証跡で指定したオブジェクトと一致する場合、イベントはログに記録されます。

1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します。
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail ログでイベント名 `getObjectacl `と `putObjectacl` を確認します。

### VPC Flow Logs
VPC Flow Logs は、VPC 内のネットワークインターフェイスとの間で送受信される IP トラフィックに関する情報をキャプチャできる機能です。 これは、CloudTrail 内で検出された IP アドレスがパブリックリソースへの外部接続のタイプを判断する場合に便利です。

Athena でのクエリを含む詳細な情報と手順については、[VPC Flow Logs 用の AWS ドキュメント] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html) を参照してください。 Athena 分析は別のプレイブックに含まれ、他の関連項目にリンクすることをお勧めします。

### DNS ログ
DNS ログは、VPC 内のネットワークインターフェイスとの間で送受信される DNS トラフィックに関する情報をキャプチャできる機能です。 これは、異常または高リスクドメインの特定に役立ちます。

### CloudWatch
EC2 インスタンスやその他のソースからのデータは [CloudWatch に取り込み] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Install-CloudWatch-Agent.html) される可能性があります。 このデータは、アラームのトリガーや分析の実行に使用できます。 CloudWatch では [異常検出] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Anomaly_Detection.html) も有効になっています。

### エンドポイント/ホストベース
1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します。
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail でパブリック IP アドレスからの `PutObject` リクエストと `DeleteObject `リクエストについて確認します

-EC2 オペレーティングシステムとアプリケーションログで、不適切なログイン、不明なソフトウェアのインストール、または認識されないファイルの存在を確認します。

-サードパーティのホストベースの侵入検知システム（HIDS）ソリューション（OSSEC、Tripwire、Wazuh、[Amazon Inspector] (https://aws.amazon.com/inspector/)、その他) を使用することを強く推奨します。

### DLP

DLP ソリューションは、設定されたとおりに検出してアラートを出す場合があります。 DLP ソリューションは [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) で入手できます。

## 封じ込め

### S3 ブロックパブリックアクセス
aws s3api put-public-access-block —bucket bucket-name-here —public-access-block-configuration「BlockPublicAcls=true, IgnorePublicAcls=true, BlockPublicPolicy=true, RestrictPublicBuckets=true」

また、アカウント全体のパブリック S3 アクセスをブロックする方法の詳細については、[Amazon S3 ストレージへのパブリックアクセスのブロック] (https://aws.amazon.com/s3/features/block-public-access/) をご覧ください。

### CodeCommit

[AWS ID およびアクセス管理 (IAM) サービスと CodeCommit] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control.html) を使用して、すべてのユーザーのアクセス制御を監査します。

権限は、[CodeCommit 権限リファレンス] (https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-permissions-reference.html) で変更または制限することもできます。

## 撲滅

### S3 認識されない/許可されていないオブジェクトの削除
バケットから認識されないオブジェクトを削除します。

1. AWS マネジメントコンソールにサインインし、https://console.aws.amazon.com/s3/ で Amazon S3 コンソールを開きます。
1. [バケット名] リストで、オブジェクトを削除するバケットの名前を選択します。
1. 削除するオブジェクトの名前を選択します。
1. オブジェクトの現在のバージョンを削除するには、[最新バージョン] を選択し、ごみ箱アイコンを選択します。
1. オブジェクトの以前のバージョンを削除するには、[最新バージョン] を選択し、削除するバージョンの横にあるゴミ箱アイコンを選択します。

### AWS CodeCommit

[アイデンティティとCodeCommit へのアクセスを監査します。] (https://docs.aws.amazon.com/codecommit/latest/userguide/security-iam.html)

### Amazon EC2

可能な場合:

1. [EBS スナップショット] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html) または [Amazon マシンイメージ (AMI) バックアップ] () を使用して [置き換え EC2 インスタンスを起動する] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-launch-snapshot.html) https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.htmlソースから作成された
1. [EBS ボリュームをアタッチする] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-attaching-volume.html) 終了したインスタンスから新しい EC2 インスタンスに。

## リカバリ
撲滅のために記載されている手順と同じ手順

## 予防措置

### 認証Prowler チェック
-多要素認証 (MFA) が有効になっていることを確認します。
`。 /prowler-c check12`

### S3 Prowler チェック
**暗号化**
-S3 バケットでデフォルトの暗号化 (SSE) が有効になっているかどうかを確認します:`。 /prowler-c extra734`

**災害復旧**
-S3 バケットでオブジェクトのバージョニングが有効になっているかどうかを確認します:`. /prowler-c extra763`

### S3 サービスアクション
[ユーザーが S3 ブロックパブリックアクセス設定を変更できないようにする] (https://asecure.cloud/a/scp_s3_block_public_access/)

毎月バケットのアクセスとポリシーを定期的に確認し、[CloudWatch Events] (https://docs.aws.amazon.com/codepipeline/latest/userguide/create-cloudtrail-S3-source-console.html) またはセキュリティハブを使用して自動検出を行います

[S3 バケットでバージョニングを使用する] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) トップレベルオブジェクトの偶発的または意図的な削除を軽減

[ACL によるアクセスの管理] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/acls.html) バケットおよびオブジェクトレベルのリソースへの不正アクセスを制限する

### Amazon EC2

[各アカウントで多要素認証 (MFA) を使用する。] (https://aws.amazon.com/iam/features/mfa/)

TLS を使用して AWS リソースと通信します。 TLS 1.2 以降をお勧めします。 一部のサービスはデフォルトで有効になっており、他のサービスは実装する必要があります ([JavaScript SDK] (https://docs.aws.amazon.com/sdk-for-script/v2/developer-guide/enforcing-tls.html) など)。

[AWS CloudTrail で API とユーザーアクティビティのログをセットアップします。] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitor-with-cloudtrail.html)

[KMS などの AWS 暗号化ソリューションを使用する] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html) と、AWS サービス内のすべてのデフォルトのセキュリティコントロールを使用します。

[EBS スナップショットを有効にする] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSSnapshots.html)

### AWS CodeCommit

[各アカウントで多要素認証 (MFA) を使用する。] (https://aws.amazon.com/iam/features/mfa/)

TLS を使用して AWS リソースと通信します。 TLS 1.2 以降をお勧めします。 一部のサービスはデフォルトで有効になっており、他のサービスは実装する必要があります ([JavaScript SDK] (https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/enforcing-tls.html) など)。

[AWS CloudTrail で API とユーザーアクティビティのログをセットアップします。] (https://docs.aws.amazon.com/codecommit/latest/userguide/integ-cloudtrail.html)

[KMS などの AWS 暗号化ソリューションを使用する] (https://docs.aws.amazon.com/codecommit/latest/userguide/encryption.html) と、AWS サービス内のすべてのデフォルトのセキュリティコントロールを使用します。

[Amazon Macie などの高度なマネージドセキュリティサービスを使用する] (https://aws.amazon.com/blogs/compute/discovering-sensitive-data-in-aws-codecommit-with-aws-lambda-2/) は、Amazon S3 に保存されている個人データの検出と保護を支援します。

### Amazon Macie
[Amazon Macie] (https://aws.amazon.com/macie/) は、[管理データ識別子を使用] (https://docs.aws.amazon.com/macie/latest/user/managed-data-identifiers.html) によって、保存された認証情報、秘密キー、およびその他のアクセスデータを検出できます。

### AWS Config
[AWS Config] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html) には、[codebuild-project-envvar-awscred-check] (https://docs.aws.amazon.com/config/latest/developerguide/codebuild-project-envvar-awscred-check.html) を含む、コードの露出を管理する複数の自動化されたルールがあります認証情報がコードに保存されているかどうかを確認します。

### 全体的なセキュリティポスチャ
環境に対して [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) を実行して、この Playbook では特定されない他のリスクおよび潜在的に他のパブリックエクスポージャーをさらに特定します。

### DLP 実装

情報漏えい対策 (DLP) ソリューションを実装すると、追加の検出機能とアラートが提供される場合があります。 DLP ソリューションは [AWS Marketplace] (https://aws.amazon.com/marketplace/search/results?searchTerms=dlp) にあり、規定に従って設定する必要があります。

## 学んだ教訓
「これは、「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要なアイテムを追加するための場所です。 `

## アドレス指定バックログ項目
-インシデントレスポンダーとして、コードエクスポージャーを検出する方法に関するRunbookが必要です
-インシデントレスポンダーとして、コードの漏洩を検出する方法に関するRunbookが必要です
-インシデントレスポンダーとして、パブリックリソース（AMI、EBS ボリューム、ECR リポジトリなど）を検出できる必要があります
-インシデントレスポンダーとして、AWS 内で重要な変更を加えることができるロールを知る必要があります
-インシデントレスポンダーとして、コードエクスポージャーを軽減するためのプレイブックが必要で、エスカレーションポイントが必要でした
-インシデントレスポンダーとして、さまざまなデータ分類に必要なログに関するドキュメントが必要です

## 現在のバックログアイテム