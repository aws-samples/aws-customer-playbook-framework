# インシデントレスポンス Playbook: RDS の身代金応答
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について独自に評価する責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2024 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

## エグゼクティブ・サマリー
このプレイブックでは、Amazon リレーショナルデータベースサービス (RDS) に対する身代金攻撃に対する応答のプロセスの概要を説明します。

詳細については、[AWS セキュリティインシデント対応ガイド] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html) を参照してください。

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
[AWS Cloud Adoption Framework セキュリティの視点] (https://docs.aws.amazon.com/whitepapers/latest/aws-caf-security-perspective/aws-caf-security-perspective.html)
* **指令**
* **探偵**
* **レスポンシブ**
* **予防**

! [画像] (/images/aws_caf.png)
* * *

### レスポンスステップ
1. [**準備**] AWS Config を使用して設定のコンプライアンスを表示する
2. [**準備**] エスカレーション手順の特定、文書化、およびテストを行う
3. [**封じ込め**] 影響を受けるリソースをすぐに隔離する
5. [**検出と分析**] CloudWatch メトリクスを使用して、データが漏洩した可能性があるかどうかを判断する
6. [**検出と分析**] vpcFlowLogs を使用して外部 IP アドレスからの不適切なデータベースアクセスを特定する
7. [**リカバリ**] 必要に応じてリカバリ手順を実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: 身代金とデータ破壊
* **カテゴリー**: 身代金攻撃
* **リソース**: RDS
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス
* **ログソース**: RDS Database Log Files、S3 Access Logs、CloudTrail、CloudWatch、AWS Config
* **チーム**: セキュリティオペレーションセンター (SOC)、フォレンジック調査官、クラウドエンジニアリング

## インシデント処理プロセス
### インシデント対応プロセスには、次の段階があります。
* 準備
* 検出と分析
* 封じ込めと根絶
* 回復
* インシデント後の活動

## 準備
* セキュリティ体制を評価して、セキュリティギャップを特定して修正する
* AWS はオープンソースのSelf-Service Security Assessment（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/）ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント。
* サーバー、ネットワークデバイス、ネットワーク/ファイル共有、開発者マシンなど、すべてのリソースの完全な資産インベントリを維持する
* AWS リソースの設定を評価、監査、評価できるサービスである [AWS Config] (https://aws.amazon.com/config/) の使用を検討してください。
* Amazon RDS に保存されている AWS アカウント、ワークロード、データを保護するために [AWS GuardDuty] (https://aws.amazon.com/guardduty/) を実装して、悪意のあるアクティビティや不正な動作を継続的に監視することを検討する
* [DB インスタンスを仮想プライベートクラウド (VPC) で実行する] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html) Amazon VPC サービスに基づいて、ネットワークアクセスコントロールを最大限に高める
* AWS ID およびアクセス管理 (IAM) ポリシーを使用して、Amazon RDS リソースの管理を許可されるユーザーを決定するアクセス権限を割り当てます。 たとえば、IAM を使用して、DB インスタンスの作成、説明、変更、削除、リソースのタグ付け、またはセキュリティグループの変更を許可するユーザーを特定できます。
* セキュリティグループを使用して、DB インスタンス上のデータベースに接続できる IP アドレスまたは Amazon EC2 インスタンスを制御します。 DB インスタンスを最初に作成すると、そのファイアウォールは、関連付けられたセキュリティグループによって指定されたルール以外のデータベースアクセスを禁止します。
* MySQL、MariaDB、PostgreSQL、Oracle、またはMicrosoft SQL Server データベースエンジンを実行している DB インスタンスで [セキュアソケットレイヤー (SSL) またはトランスポートレイヤーセキュリティ (TLS) 接続を使用する] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html)
* [Amazon RDS 暗号化を使用する] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) は、保管中の DB インスタンスとスナップショットをセキュリティで保護します。 Amazon RDS 暗号化では、業界標準の AES-256 暗号化アルゴリズムを使用して、DB インスタンスをホストするサーバー上のデータを暗号化します。
* [ネットワーク暗号化を使用する] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) と [透過的なデータ暗号化] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html) Oracle DB インスタンスで
* DB エンジンのセキュリティ機能を使用して、DB インスタンスのデータベースにログインできるユーザーを制御します。 これらの機能は、データベースがローカルネットワーク上にあるかのように機能します。
* サードパーティ製のホストベースの侵入検知システム（HIDS）ソリューション（OSSEC、Tripwire、Wazuh、[Amazon Inspector] (https://aws.amazon.com/inspector/) など) を用意することを強く推奨
* その他の参考文献と手順については、[Amazon RDS のセキュリティ] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html)

### AWS Config を使用して設定のコンプライアンスを表示します。
1. AWS マネジメントコンソールにサインインし、https://console.aws.amazon.com/config/ で AWS Config コンソールを開きます。
1. AWS マネジメントコンソールメニューで、リージョンセレクターが AWS Config ルールをサポートするリージョンに設定されていることを確認します。 サポートされているリージョンのリストについては、Amazon ウェブサービス全般のリファレンスの「AWS Config リージョンとエンドポイント」を参照してください。
1. ナビゲーションペインで、[リソース] を選択します。 [リソースインベントリ] ページでは、リソースカテゴリ、リソースタイプ、およびコンプライアンスステータスでフィルタリングできます。 必要に応じて、[削除されたリソースを含める] を選択します。 テーブルには、リソースタイプのリソース識別子と、そのリソースのリソースコンプライアンスステータスが表示されます。 リソース識別子は、リソース ID またはリソース名です。
1. [リソース識別子] 列からリソースを選択します。
1. [リソースタイムライン] ボタンを選択します。 設定イベント、コンプライアンスイベント、または CloudTrail イベントでフィルタリングできます。
1. 特に次のイベントに焦点を当てます。
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
* rds-snapshots-public-prohibited
* rds-snapshot-encrypted
* rds-storage-encrypted

## エスカレーション手順
-「EC2 フォレンジックをいつ実施すべきかについてビジネス上の決定が必要です`
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 検出と分析
* [AWS GuardDuty 機械学習] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) は、データ漏えいの試みの一環として Amazon リレーショナルデータベースサービス (Amazon RDS) のスナップショットを生成するなど、疑わしい動作を検出できます。
* EC2 オペレーティングシステムとアプリケーションログで、不適切なログイン、不明なソフトウェアのインストール、または認識されないファイルの有無を確認します

### [CloudWatch メトリクスを使う] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
データ漏洩の「スパイク」を探します。 攻撃者がデータ破壊を行い、身代金メモを残した可能性があります。このような場合、悪意のあるアクターと協力してデータ復旧の機会はありません。
1. CloudWatch コンソールを https://console.aws.amazon.com/cloudwatch/ で開く
1. ナビゲーションペインで、[メトリック]、[すべてのメトリック] の順に選択します。
1. [すべてのメトリクス] タブで、インスタンスをデプロイするリージョンを選択します。
1. [すべてのメトリクス] タブで、検索語句 `networkPacketsOut `を入力し、Enter キーを押します。
1. 検索の結果の 1 つを選択して、指標を表示します。
1. 1 つ以上のメトリックをグラフ化するには、各メトリックスの横にあるチェックボックスをオンにします。 すべてのメトリクスを選択するには、テーブルの見出し行のチェックボックスをオンにします。
1. (オプション) グラフのタイプを変更するには、[グラフオプション] を選択します。 その後、折れ線グラフ、積み上げ面グラフ、棒グラフ、円グラフ、または数値を選択できます。
1. （オプション）メトリックの期待値を示す異常検出バンドを追加するには、メトリックの横にある「アクション」の下の異常検出アイコンを選択します。

### vpcFlowLogs を使用して外部 IP アドレスからの不適切なデータベースアクセスを特定する
1. [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) を使用してログデータを分析する
1. 特定の IP への、または特定の IP からのすべてのレコードの src_ip、src_port、dst_ip、dst_port の各クワッドのバイト数を含むサマリーを取得します。
```
vpcflow から byte_count として送信元アドレス、宛先アドレス、送信元ポート、宛先ポート、合計 (数バイト) を選択します。
WHERE (送信元アドレス = '192.0.2.1' または宛先アドレス = '192.0.2.1')
AND date_partition >= '2020/07/01'
AND date_partition <= '2020/07/31'
そして account_partition = '111122223333'
そして region_partition in ('us-east-1', 'us-east-2', 'us-west-2', 'us-west-2')
送信元アドレス、宛先アドレス、送信元ポート、宛先ポートによるグループ化
バイト数による注文 DESC
```
1. その他のクエリ例は、[vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql) で提供されています。


## リカバリ
* 身代金を払わないことをお勧めします
* 身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです
* データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を比較検討する必要があります
* 身代金を支払うことを選択した場合、攻撃者が自分の会社や他のユーザーに対して操作を継続できるように直接許可します
* https://www.nomoreransom.org/ にアクセスして、データが感染したマルウェアバリアントに対して復号化が可能かどうかを特定する
* [IAM ユーザーキーの削除またはローテーション] (https://console.aws.amazon.com/iam/home#users) と [Root ユーザーキー] (https://console.aws.amazon.com/iam/home#security_credential)。公開されている特定のキーを特定できない場合、アカウント内のすべてのキーをローテーションすることもできます。
* [許可されていない IAM ユーザーを削除する] (https://console.aws.amazon.com/iam/home#users。)
* [不正なポリシーを削除する] (https://console.aws.amazon.com/iam/home#/policies)
* [権限のない役割を削除する] (https://console.aws.amazon.com/iam/home#/roles)
* [一時的な資格情報を取り消す] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 一時的な認証情報は、IAM ユーザーを削除することで取り消すこともできます。 注:IAM ユーザーの削除は実稼働ワークロードに影響を与える可能性があるため、注意して行う必要があります。
* CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
* 代替データバックアップ戦略を使用する場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前にスケジュールされた最後のイベントから復元します。
* 認識されていない、または許可されていないパブリックスナップショットまたはデータベースを削除する
* 侵害されたデータベースを削除し、新しい RDS データベースを作成する
* データベースへのパーミッションアクセスを持つ EC2 インスタンスを特定し、それらも調査する

## 学んだ教訓
「これは、必ず「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要な、貴社固有のアイテムを追加する場所です。 `

## アドレス指定バックログ項目
-インシデントレスポンダーとして EC2 フォレンジックを実施するには Runbook が必要です
-インシデントレスポンダーとして、EC2 フォレンジックをいつ実施すべきかについてのビジネス上の決定が必要です
-インシデントレスポンダーとして、使用の意図に関係なく、有効になっているすべてのリージョンでログを有効にする必要があります
-インシデントレスポンダーとして、既存の EC2 インスタンスで暗号マイニングを検出できる必要があります

## 現在のバックログアイテム