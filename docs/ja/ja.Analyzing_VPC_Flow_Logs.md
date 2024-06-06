# インシデントレスポンス Playbook: VPC Flow Logs の分析
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について独自に評価する責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2024 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`\
承認者:`承認者名`\
最終承認日:

## エグゼクティブサマリー
このプレイブックでは、AWS VPC Flow Logs を分析するためのプロセスとクエリの例について説明します。

## 準備
この Playbook では、可能な場合は、AWS のセキュリティ評価、監査、強化、インシデント対応に役立つコマンドラインツールである [Prowler] (https://github.com/toniblyx/prowler) を参照し、統合します。

CIS Amazon Web Services Foundations Benchmark（49チェック）のガイドラインに従い、GDPR、HIPAA、PCI-DSS、ISO-27001、FFIEC、SOC2などに関連する100以上の追加のチェックがあります。

このツールは、お客様の環境内の現在のセキュリティ状態のスナップショットを迅速に提供します。 さらに、[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) はコンプライアンススキャンを自動化し、[Prowler と統合] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

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
[AWS Cloud Adoption Framework セキュリティの視点] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **指令**
* **探偵**
* **レスポンシブ**
* **予防**

! [画像] (/images/aws_caf.png)
* * *

### インシデントの分類と処理
* **戦術、テクニック、手順**: Tool: Athena
* **カテゴリ**: ログ分析
* **リソース**: Athena
* **指標**: サイバー脅威インテリジェンス、第三者通知
* **ログソース**: VPC Flow Logs
* **チーム**: セキュリティオペレーションセンター (SOC)、フォレンジック調査官、クラウドエンジニアリング

## インシデント処理プロセス
### インシデント対応プロセスには、次の段階があります。
* 準備
* 検出と分析
* 封じ込めと根絶
* 回復
* インシデント後の活動

### レスポンスステップ
1. [**準備**] 公開されているアセットインベントリを作成する
2. [**準備**] ログの設定
3. [**準備**] ログ分析機能を特定して評価するためのトレーニングプログラムを実施する
4. [**検出と分析**] コンソールで VPC Flow Logs を表示する
5. [**検出と分析**] Amazon Athena を使用したフローログのクエリ
6. [**検出と分析**] DDL として VPC Flow Logs テーブルを作成します。
7. [**検出と分析**] 例 Athena クエリ:新しいログテーブルを DDL として作成する
8. [**検出と分析**] 例 Athena Queries: ロードされているかどうか、およびファイルに何行あるかを確認する
9. [**検出と分析**] 例 Athena クエリ:表示可能なデータと使用可能なデータが返されるかどうかを確認します。
10. [**検出と分析**] Athena クエリーの例:上位 IP をチェックする
11. [**検出と分析**] 例 Athena クエリ:調査を実行するための標準クエリ
12. [**検出と分析**] Athena クエリーの例:ソース/宛先 IP のデータ比のルックアップ
13. [**検出と分析**] Athena クエリーの例:既知の IOC の検索
14. [**検出と分析**] 例 Athena クエリ:脅威テーブル
15. [**検出と分析**] 例 Athena クエリ:ターゲット脅威タイプへのルックアップビュー

## 準備
### 公開されているアセットインベントリ
[VPC ダッシュボード] (https://console.aws.amazon.com/vpc/home) と [ネットワークマネージャーダッシュボード] (https://console.aws.amazon.com/networkmanager/home) を使用して、ネットワークリソースを視覚化して監視できます。 ネットワークマネージャーダッシュボードには、グローバルネットワーク内のリソース、その地理的位置、ネットワークトポロジ、Amazon CloudWatch メトリクスとイベントに関する情報が含まれており、ルート分析を実行できます。

次のリソースのインベントリを維持し、変更を監視する必要があります。
-仮想プライベートクラウド (VPC)
-VPC ネットワークアクセスコントロールリスト (NACL)
-VPC セキュリティグループ
-VPC ルートテーブル
-VPC エラスティックネットワークインターフェイス (ENI)
-VPC インターネットゲートウェイ
-VPC ピアリング接続
-VPC NAT ゲートウェイ
-VPC エンドポイント
-VPN カスタマーゲートウェイ
-VPC エラスティック IP アドレス (EIP)

Prowler は、インターネットに公開されている多くのリソースを見つけるのにも役立ちます:」。 /prowler-g group17"

### ログの設定
**フローログ:**
-フローログは、VPC 内のネットワークインターフェイスとの間で送受信される IP トラフィックに関する情報をキャプチャします。
-VPC、サブネット、または個々のネットワークインターフェイスのフローログを作成できます。
-フローログデータは CloudWatch Logs または Amazon S3 に公開され、過度に制限された、または過度に許容されるセキュリティグループおよびネットワーク ACL ルールを診断するのに役立ちます。

**コンソールを使用してネットワークインターフェイスのフローログを作成するには**
1. Amazon [EC2 コンソール] を開きます (https://console.aws.amazon.com/ec2/)
1. ナビゲーションペインで、[ネットワークインターフェイス] を選択します。
1. 1 つ以上のネットワークインターフェイスのチェックボックスをオンにし、[アクション]、[Create flow log] の順に選択します。
1. [Filter] で、ログに記録するトラフィックのタイプを指定します。
-受け入れられたトラフィックと拒否されたトラフィックを記録する場合は [すべて]、拒否されたトラフィックのみをログに記録するには [拒否]、受け入れられたトラフィックのみをログに記録するには [Accept] を選択します。
1. [最大集約間隔] で、フローがキャプチャされ、1 つのフローログレコードに集約される最大期間を選択します。
1. [送信先] で、[Send to an Amazon S3 bucket] を選択します。
1. S3 バケット ARN の場合、既存の Amazon S3 バケットの Amazon Resource Name (ARN) を指定します。
-バケット ARN にサブフォルダーを含めることができます。 バケットは AWSLogs をサブフォルダー名として使用できません。これは予約用語です。
-たとえば、my-logs という名前のサブフォルダーを my-bucket というバケットに指定するには、次の ARN を使用します。arn: arn: aws: s3።: my-bucket/my-logs/
-バケットを所有している場合は、リソースポリシーが自動的に作成され、バケットにアタッチされます。 詳細については、「フローログの Amazon S3 バケットのアクセス許可」を参照してください。
1. [ログレコード形式] で、フローログレコードの形式を選択します。
* デフォルトの形式を使用するには、AWS default format を選択します。
* カスタムフォーマットを使用するには、[カスタムフォーマット] を選択し、[ログ形式] からフィールドを選択します。
1. [Create flow log] を選択します。

**コンソールを使用して VPC またはサブネットのフローログを作成するには**
1. Amazon [VPC コンソール] を開きます (https://console.aws.amazon.com/vpc/)
1. ナビゲーションペインで、[VPC] を選択するか、[Subnets] を選択します。
1. 1 つ以上の VPC またはサブネットのチェックボックスをオンにし、[アクション]、[Create flow log] の順に選択します。
1. [Filter] で、ログに記録するトラフィックのタイプを指定します。
-受け入れられたトラフィックと拒否されたトラフィックを記録する場合は [すべて]、拒否されたトラフィックのみをログに記録するには [拒否]、受け入れられたトラフィックのみをログに記録するには [Accept] を選択します。
1. [最大集約間隔] で、フローがキャプチャされ、1 つのフローログレコードに集約される最大期間を選択します。
1. [送信先] で、[Send to an Amazon S3 bucket] を選択します。
1. S3 バケット ARN の場合、既存の Amazon S3 バケットの Amazon Resource Name (ARN) を指定します。
-バケット ARN にサブフォルダーを含めることができます。 バケットは AWSLogs をサブフォルダー名として使用できません。これは予約用語です。
-たとえば、my-logs という名前のサブフォルダーを my-bucket というバケットに指定するには、次の ARN を使用します。「arn: arn: aws: s3።: my-bucket/my-logs/
-バケットを所有している場合は、リソースポリシーが自動的に作成され、バケットにアタッチされます。 詳細については、「フローログの Amazon S3 バケットのアクセス許可」を参照してください。
1. [ログレコード形式] で、フローログレコードの形式を選択します。
* デフォルトの形式を使用するには、AWS default format を選択します。
* カスタムフォーマットを使用するには、[カスタムフォーマット] を選択し、[ログ形式] からフィールドを選択します。
1. [Create flow log] を選択します。

** NAT ゲートウェイの監視:** NAT ゲートウェイから情報を収集し、読み取り可能なほぼリアルタイムのメトリクスを作成する CloudWatch を使用して NAT ゲートウェイをモニタリングできます。
1. NAT ゲートウェイメトリックスは、1 分間隔で CloudWatch に送信されます。
1. NAT ゲートウェイのメトリックは、次のように表示できます。
* [Amazon CloudWatch コンソール] (https://console.aws.amazon.com/cloudwatch/) を開きます
* ナビゲーションペインで、[Metrics] を選択します。
* [すべてのメトリクス] で、NAT ゲートウェイメトリック名前空間を選択します。
* 指標を表示するには、指標ディメンションを選択します。
1. NAT ゲートウェイを経由するアウトバウンドトラフィックのアラームを作成するには、次の手順を実行します。
* [Amazon CloudWatch コンソール] (https://console.aws.amazon.com/cloudwatch/) を開きます
* ナビゲーションペインで、[アラーム]、[アラームの作成] の順に選択します。
* NAT ゲートウェイを選択します。
* NAT ゲートウェイと「bytesOuttoDestination」メトリックを選択し、[次へ] を選択します。
* アラームを次のように設定し、完了したら [Create Alarm] を選択します。
* [アラームのしきい値] に、アラームの名前と説明を入力します。 [いつでも] に [>=] を選択し、5000000 と入力します。 連続する期間に 1 を入力します。
* [アクション] で、既存の通知リストを選択するか、[新規リスト] を選択して新しい通知リストを作成します。
* [アラームプレビュー] で、15 分の期間を選択し、[合計] の統計を指定します。
1. ポート割り当てエラーを監視するアラームを作成するには
* [Amazon CloudWatch コンソール] (https://console.aws.amazon.com/cloudwatch/) を開きます
* ナビゲーションペインで、[アラーム]、[アラームの作成] の順に選択します。
* NAT ゲートウェイを選択します。
* NAT ゲートウェイと「ErrorPortAllocation」メトリックを選択し、[次へ] を選択します。
* アラームを次のように設定し、完了したら [Create Alarm] を選択します。
* [アラームのしきい値] に、アラームの名前と説明を入力します。 [いつでも] で、[>] を選択し、0 と入力します。 連続する期間に 3 を入力します。
* [アクション] で、既存の通知リストを選択するか、[新規リスト] を選択して新しい通知リストを作成します。
* [アラームプレビュー] で、5 分の期間を選択し、[最大] の統計情報を指定します。

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

## 検出と分析
### コンソールでの VPC Flow Logs の表示
**ネットワークインターフェイスのフローログに関する情報を表示するには**
1. Amazon [EC2 コンソール] を開きます (https://console.aws.amazon.com/ec2/)
1. ナビゲーションペインで、[ネットワークインターフェイス] を選択します。
1. ネットワークインターフェイスを選択し、[フローログ] を選択します。 フローログに関する情報がタブに表示されます。 [Destination type] 列には、フローログがパブリッシュされる宛先が示されます。

**VPC またはサブネットのフローログに関する情報を表示するには**
1. [Amazon VPC コンソール] (https://console.aws.amazon.com/vpc/) を開きます
1. ナビゲーションペインで、[VPC] または [Subnets] を選択します。
1. VPC またはサブネットを選択し、[フローログ] を選択します。
-フローログに関する情報がタブに表示されます。
-[Destination type] 列には、フローログがパブリッシュされる宛先が示されます。

**Amazon S3 に発行されたフローログレコードを表示するには**
1. [Amazon S3 コンソール] (https://console.aws.amazon.com/s3/) を開きます
1. [Bucket name] で、フローログを公開するバケットを選択します。
1. [名前] で、ログファイルの横にあるチェックボックスをオンにします。 オブジェクトの概要パネルで、[ダウンロード] を選択します。

### Amazon Athena を使用したフローログのクエリ
Amazon Athena は、標準 SQL を使用して、フローログなどの Amazon S3 のデータを分析できる対話型クエリサービスです。 VPC Flow Logs で Athena を使用すると、VPC を流れるトラフィックに関する実用的なインサイトをすばやく得ることができます。 たとえば、仮想プライベートクラウド（VPC）内のどのリソースがトップトーカーであるかを特定したり、最も拒否された TCP 接続を持つ IP アドレスを特定したりできます。

VPC フローログと Athena との統合を合理化および自動化するには、必要な AWS リソースと事前定義されたクエリを作成する CloudFormation テンプレートを生成し、VPC を流れるトラフィックに関するインサイトを取得できます。

1. CloudFormation テンプレートは次のリソースを作成します。
-Athena データベース。 データベース名は vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>です。
-アAthena のワークグループ。 <flow-log-subscription-id><partition-load-frequency><start-date><end-date>workgroup <flow-log-subscription-id><partition-load-frequency><start-date><end-date>名はワークグループです
-フローログレコードに対応するパーティション化された Athena テーブル。 テーブル名はです<flow-log-subscription-id><partition-load-frequency><start-date><end-date>。
-Athena という名前のクエリのセット。 詳細については、「定義済みクエリ」を参照してください。
-指定されたスケジュール（毎日、毎週、または毎月）で新しいパーティションをテーブルにロードする Lambda 関数。
-Lambda 関数を実行するアクセス権限を付与する IAM ロール。
1. 要件
-AWS Lambda と Amazon Athena をサポートするリージョンを選択する必要があります。
-Amazon S3 バケットは、選択したリージョンに存在する必要があります。
1. 価格設定
-クエリを実行する場合、Amazon Athena の標準料金が発生します。
-定期的なスケジュールで新しいパーティションをロードする Lambda 関数に対して標準の AWS Lambda 料金が発生します（パーティションの読み込み頻度を指定したが、開始日と終了日を指定しない場合）。
1. 次のいずれかを実行します。
-Amazon VPC コンソールを開きます。 ナビゲーションペインで、[Your VPC] を選択し、VPC を選択します。
-Amazon VPC コンソールを開きます。 ナビゲーションペインで、[Subnets] を選択し、サブネットを選択します。
-Amazon EC2 コンソールを開きます。 ナビゲーションペインで、[ネットワークインターフェイス] を選択し、ネットワークインターフェイスを選択します。
1. [フローログ] タブで、Amazon S3 にパブリッシュするフローログを選択し、[アクション]、[Athena 統合の生成] の順に選択します。
1. パーティションの負荷頻度を指定します。
-「なし」を選択した場合は、過去の日付を使用して、パーティションの開始日と終了日を指定する必要があります。
-[毎日]、[毎週]、または [月次] を選択すると、パーティションの開始日と終了日はオプションです。
-開始日と終了日を指定しない場合、CloudFormation テンプレートは定期的なスケジュールで新しいパーティションをロードする Lambda 関数を作成します。
1. 生成されたテンプレートの S3 バケットを選択し、クエリ結果用の S3 バケットを選択または作成します。
1. [Athena 統合を生成] を選択します。
1. （オプション）成功メッセージで、CloudFormation テンプレートに指定したバケットに移動するためのリンクを選択し、テンプレートをカスタマイズします。
1. 成功メッセージで、[CloudFormation スタックの作成] を選択し、AWS CloudFormation コンソールでスタックの作成ウィザードを開きます。
-生成された CloudFormation テンプレートの URL は、[テンプレート] セクションで指定されます。
-ウィザードを完了して、テンプレートで指定されたリソースを作成します。

**定義済みクエリを実行します**

生成された CloudFormation テンプレートには、AWS ネットワークのトラフィックに関する有意義な洞察をすばやく得るために実行できる、一連の定義済みクエリが用意されています。 スタックを作成し、すべてのリソースが正しく作成されたことを確認したら、事前定義されたクエリの 1 つを実行できます。

1. コンソールを使用して定義済みのクエリを実行するには
-Athena コンソールを開きます。 [ワークグループ] パネルで、CloudFormation テンプレートによって作成されたワークグループを選択します。
-定義済みのクエリの 1 つを選択し、必要に応じてパラメータを変更し、クエリを実行します。
-Amazon S3 コンソールを開きます。 クエリ結果に指定したバケットに移動し、クエリの結果を表示します。

生成された CloudFormation テンプレートによって提供される Athena 名前付きクエリを次に示します。
* vpcFlowLogsAcceptedTraffic — セキュリティグループとネットワーク ACL に基づいて許可された TCP 接続。
* VpcFlowLogsAdminPortTraffic — 管理用ウェブアプリポートに記録されたトラフィック。
* VpcFlowLogsIPv4Traffic — 記録された IPv4 トラフィックの総バイト数。
* VpcFlowLogsIPv6Traffic — 記録された IPv6 トラフィックの総バイト数。
* vpcFlowLogsRejectedTCPTraffic — セキュリティグループまたはネットワーク ACL に基づいて拒否された TCP 接続。
* vpcFlowLogsRejectedTraffic — セキュリティグループまたはネットワーク ACL に基づいて拒否されたトラフィック。
* VpcFlowLogsSshRdpTraffic — SSH および RDP トラフィック。
* VpcFlowLogsTopTalkers — 最も多くのトラフィックが記録されている 50 の IP アドレス。
* VpcFlowLogsTopTalkersPacketLevel — トラフィックが最も多く記録されている 50 個のパケットレベルの IP アドレス。
* VpcFlowLogsTopTalkingInstances — 記録されたトラフィックが最も多くある 50 のインスタンスの ID。
* VpcFlowLogsTopTalkingSubnets s — 記録されたトラフィックが最も多い50のサブネットの ID。
* VpcFlowLogsTopTCPTraffic — 送信元 IP アドレスについて記録されたすべての TCP トラフィック。
* VpcFlowLogsTotalBytesTransferred — 記録されたバイト数が最も多い送信元と宛先 IP アドレスのペアが50組。
* VpcFlowLogsTotalBytesTransferredPacketLevel Level — パケットレベルの送信元および宛先 IP アドレスの 50 組のバイト数が記録されます。
* VpcFlowLogsTrafficFrmSrcAddr — 特定の送信元 IP アドレスについて記録されたトラフィック。
* VpcFlowLogsTrafficToDstAddr — 特定の宛先 IP アドレスについて記録されたトラフィック

## ディープダイブ分析:

参考:https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html

### VPC Flow Logs テーブルを DDL として作成する
次のコードは、検証される VPC Flow Logs の DDL としてテーブルを作成するために使用されます。 (TODO 項目はインラインで注意してください):
```
/*
S3 バケットに直接配信される VPC フローログ用の VPC フローテーブルを作成します
マルチアカウント、マルチリージョンの展開、および YYYYY/MM/DD 形式の日付に関するパーティション設定が含まれます。
デフォルト以外の v3 および v4 フィールドを含む、使用可能なすべてのフィールドが含まれます。
注:VPC フローフィールドは、設定時にリストされた順序で設定する必要があります。別の順序で設定されている場合は、以下の DDL に記載されている順序を調整する必要があります。
*/

/*
TODO: 必要に応じて、テーブル名「vpcflow」を VPC フローログテーブルに使用する名前に更新します。
*/
存在しない場合は外部テーブルを作成する vpcflow (
/*
TODO: VPC フローログがこの順序で生成されるように設定されていることを確認します。そうでない場合は、生成された順序に合わせて次の順序を並べ替える必要があります。
ヒント:サンプルをダウンロードし、各ログファイルの最初の行でフィールドの順序を確認します。
フィールド名がログの一番上の行の名前と正確に一致しない場合でも、Athena は順番に基づいてインポートします。
注:これらの v2 フィールドはデフォルトの形式であり、通常は調整する必要はありません。これらのフィールドのデフォルトの順序がないため、以下の v3/v4/v5 フィールドの順序に注意してください。 フィールドと説明は https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html で確認できます。
*/
バージョンint、
アカウント文字列、
インターフェイス ID 文字列、
ソースアドレス文字列、
宛先アドレス文字列、
sourceport int、
宛先ポート整数、
プロトコル int、
numpackets int
大きな数字、
開始時間整数、
終了時間整数、
アクション文字列、
ログステータス文字列、
/*
注:デフォルト以外の v3 および v4 フィールドの開始
ソースログにこれらのフィールドがすべて含まれていなくても心配しないでください。空白として表示されます。
TODO: VPC フローログにこれらのフィールドが含まれている場合は、以下にリストされている順序がフローログフォーマットで設定されている順序と同じであることを確認してください。
*/
vpcid 文字列、
文字列をタイプし、
tcpflags smallint、
サブネティッド文字列、
サブロケーションタイプの文字列、
サブロケーション ID 文字列、
リージョン文字列、
pktsrcaddr 文字列、
pktdstaddr 文字列、
インスタンス ID 文字列、
アジッドストリング、
/*
注:デフォルト以外の v5 フィールドの開始
ソースログにこれらのフィールドがすべて含まれていなくても心配しないでください。空白として表示されます。
TODO: VPC フローログにこれらのフィールドが含まれている場合は、以下にリストされている順序がフローログフォーマットで設定されている順序と同じであることを確認してください。
*/
pkt-src-aws-サービス文字列、
pkt-dst-aws-サービス文字列、
流れ方向文字列、
トラフィックパス int
)
によってパーティショニング
(
date_partition 文字列、
region_partition 文字列
account_partition 文字列
)
行形式区切り
'' で終わるフィールド
/*
TODO: 場所の値のバケット名とオプション接頭辞を置き換え、
プレフィックスがない場合は、追加の/を削除します。
例:s3: //my_central_log_bucket/awsLogs/ または s3: //my_central_log_bucket/prod/awsLogs/
*/
場所 's3: <bucket_name><optional_prefix>////awsLogs/ '
TBLPROPERTIES
(
「skip.header.line.count" = "1",
「プロジェクション.enabled」=「真」,
「projection.date_partition.type」=「日付」,
/* TODO: <YYYY><MM>/<DD>をログの最初の日付に置き換えます。例:2020/11/30 */
「projection.date_partition.Range」= "<YYYY><MM>/<DD>, NOW」,
「projection.date_partition.format」=「yyyy/mm/dd」,
「projection.date_partition.interval」=「1",
「projection.date_partition.interval.unit」=「DAYS」,
「projection.region_partition.type」=「列挙型」,
「projection.region_partition.values」=「us-east-2, us-east-1, us-west-1, us-west-2, af-south-1, ap-southeast-1, ap-northeast-3, ap-southeast-2, ap-southeast-2, ap-northeast-1, ca-central-1, cn-north-1, cn-northwest-1, eu-central-1, eu-west-1, eu-west-2, eu-west-3, me-south-1, me-south-1, sa-east-1",
「projection.account_partition.type」=「列挙型」,
/*
TODO: projection.account_partition.values の値を、このテーブルに含める AWS アカウント番号のリストに置き換えます
例:「0123456789,0123456788,0123456777"
注:スペースは使用しないでください。値をカンマで区切って入力してください (スペースを含むと構文エラーが発生します)
アカウントが 1 つしかない場合は、カンマなしでそれ自身に含めます。例:「0123456789」
*/
「projection.account_partition.values」= "<account_num_1>,<account_num_2>, ...。 「,
/*
TODO: ロケーションと同じで、ストレージ.ロケーションテンプレートの値のバケット名とオプションプレフィックスを置き換え、
プレフィックスがない場合は、追加の/を削除します。
<> 括弧内の値のみが置換され、{} 括弧内の値は変数であり、以下で定義したままにしておく必要があることを確認します。
例:s3: //my_central_log_bucket/awsLogs/... または s3: //my_central_log_bucket/prod/awsLogs/...
*/
「storage.location.template」=「s3: <bucket_name><optional_prefix>////AWSLogs/ $ {account_partition} /vpcflowlogs/$ {region_partition} /$ {date_partition}」
);
```
### クエリの例:
#### 新しいログテーブルを DDL として作成する
以下のクエリを実行して、新しい Athena テーブルを作成します。 以下の点に留意してください。

* <bucket_name>ターゲットバケットに置き換えます。
* LOCATION をチェックして、関連するバケットプレフィックスとその末尾のスラッシュ（「/」）が含まれていることを確認します
```
存在しない場合は外部テーブルを作成する vpc_flow_logs (
rejected_packet STRING、
avg_in_packet_size INT、
vpc_id 文字列、
local_port INT、
source_dc 文字列、
IP文字列、
end_time STRING,
remote_port INT、
start_time STRING,
プロトコル INT、
in_bytes BIGINT、
source_tag 文字列,
account_id 文字列、
instance_id 文字列、
interface_public_ips 文字列、
out_bytes BIGINT、
is_skipped_rejected_packets 文字列、
source_az 文字列、
source_region 文字列、
avg_out_packet_size INT、
instance_type 文字列、
interface_local_ips 文字列
)
行フォーマットSERDE 'org.openX.data.jsonSerde.jsonSerde'
SERDEPROPERTIES ('無視.malformed.json' = '真')
場所 's3:<bucket_name>///';
```
#### ロードされているかどうか、およびファイル内に何行あるかを確認します。
```
vpc_flow_logsからカウント (*) を選択します。
```
#### 表示可能なデータおよび使用可能なデータが返されるかどうかを調べる:
```
*vpc_flow_logs制限1000から選択;
```
#### 上位IPをチェックする
```
IP、カウント (*) cnt を選択します。
vpc_flow_logsから
IP でグループ化
CNT 降順順
制限100;

いくつかのトップIPを取り、時間ごとに注文してください（下のIPを交換してください）

vpc_flow_logsから*を選択
ここで ip = '123.123.123.123'
start_timeで注文する。
```

###「S3 Gateway/VPC Interface ノート」
「VPC エンドポイント」の場合、接続はフローログにプライベートアドレスを経由するトラフィックとして表示されます。

S3 Gateway は、プレフィクスリストのパブリック範囲に接続しているプライベートアドレスとしてフローログに表示されます。

### 調査を実行するための標準クエリ

#### 送信元/宛先 IP のデータ比率のルックアップ
次のクエリは、IP、レコードカウント（同じ送信元または宛先アドレスのレコードの総数）、および合計バイト（送信元アドレスまたは宛先アドレス間の通信の合計バイト）によって VPC フローログレコードを集約します。 このクエリでは、通信あたりに送信されたバイト数（合計バイトをレコード数で割った値）であるデータ比も示します。
```
ソースアドレスを選択して、
送信先住所、
count (*) record_count として、
合計 (数バイト) 合計バイトとして、
合計 (数バイト) /カウント (*) として data_ratio,
date_format (from_unixtime (最小 (開始時間))、
'%y/%m/%d%H: %i: %s') first_seen、date_format (from_unixtime (max (endtime))、'%y/%m/%d%H: %i: %s') として last_seen
<database>「」より。 <table>」
WHERE (sourceaddress = '10.10.10'
または、宛先アドレス = '10.10.10.10')
送信元アドレス、宛先アドレスでグループ化
TOTAL_Bytes 降順で注文
```
#### 既知の IOC を検索中
次のクエリでは、IOC が外部 IP アドレスであると仮定し、ルックアップテーブルを使用してそれらを管理します。

#### 脅威テーブル
```
存在しない場合は外部テーブルを作成する incident_iocs (
threat_ip 文字列、
threat_Comment 文字列
)

行フォーマットSERDE
'org.apache.hadoop.hive.serde2.opencsVSerde'

SERDEPROPERTIES (
'separatorChar' = ',',
'quoteChar' = '\ "',
'escapeChar' = '\\'
)
テキストファイルとして保存
場所 's3: //バケット分析/iocs/iocfileここ/ '
```
#### ターゲット脅威タイプへのルックアップビュー
```
ビュー incident_vpc_flow_directional AS を作成
vpc.account を選択し、
vpc.interfaceid、
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) ソースとして、
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) デスティネーションとして、
「インバウンド」方向として、
vpc.sourceAddress connecting_ip として、
vpc.sourceport として connecting_port、
vpc.protocol、
vpc.numpackets、
vpc.numbytes、
vpc.action、
vpc.logstatus、
STARTTime としてキャスト (from_unixtime (vpc.starttime) AS TIMESTAMP)、
キャスト (from_unixtime (vpc.endtime) AS TIMESTAMP) として endTime,
date_diff ('秒', from_unixtime (vpc.starttime)、from_unixtime (vpc.endtime)) をsessionTimeとして

<database>「」より。 <table>「AS vpc

どこ
vpc.destinationaddress IN (incident_iocs から threat_ip を選択)

ユニオンすべて

vpc.account を選択し、
vpc.interfaceid、
vpc.sourceaddress || ':' || CAST (vpc.sourceport AS VARCHAR) ソースとして、
vpc.destinationaddress || ':' || CAST (vpc.destinationport AS VARCHAR) デスティネーションとして、
「アウトバウンド」方向として、
vpc.destinationaddress connecting_ip として、
vpc.destinationport として connecting_port、
vpc.protocol、
vpc.numpackets、
vpc.numbytes、
vpc.action、
vpc.logstatus、
STARTTime としてキャスト (from_unixtime (vpc.starttime) AS TIMESTAMP)、
キャスト (from_unixtime (vpc.endtime) AS TIMESTAMP) として endTime,
date_diff ('秒', from_unixtime (vpc.starttime)、from_unixtime (vpc.endtime)) をsessionTimeとして

<database>「」より。 <table>「AS vpc
どこ
vpc.sourceAddress IN (incident_iocs から threat_ip を選択)

開始時刻、終了時刻、セッション時間、インターフェイスID、connecting_ip、方向で順序
```

## アドレスバックログ項目
-インシデントレスポンダーとして、すべての重要な VPC 環境を監視できる必要があります
-インシデントレスポンダーとして VPC FlowLogs を大規模に照会するためのプレイブックが必要です

## 現在のバックログアイテム