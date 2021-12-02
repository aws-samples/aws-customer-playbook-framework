# インシデント対応 Playbook: 不正なネットワーク変更
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2021 Amazon ウェブサービス株式会社またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

## エグゼクティブサマリー
この Playbook では、AWS 環境内のネットワーク資産に対する不正または疑わしい変更に対応するためのプロセスの概要を説明します。

## 妥協の潜在的な指標
-新規または認識されないサービスリソース
-認識されないリソース、または許可されていないリソース (EC2、Lambda など)
-異例の課金が増える
-セキュリティ研究者からの通知
-AWS リソースまたはアカウントが侵害される可能性があるという通知

## AWS GuardDuty の潜在的な調査結果
-Backdoor: EC2/C&CActivity.B
-Backdoor: EC2/C&CActivity.B！ DNS
-Backdoor: EC2/Spambot
-Behavior: EC2/NetworkPortUnusual
-Behavior: EC2/TrafficVolumeUnusual
-CryptoCurrency: EC2/BitcoinTool.B
-CryptoCurrency: EC2/BitcoinTool.B！ DNS
-Impact: EC2/AbusedDomainRequest.Reputation
-Impact: EC2/BitcoinDomainRequest.Reputation
-Impact: EC2/MaliciousDomainRequest.Reputation
-Impact: EC2/SuspiciousDomainRequest.Reputation
-Impact: EC2/WinRMBruteForce
-Recon: EC2/PortProbeEMRUnprotectedPort
-Recon: EC2/PortProbeUnprotectedPort
-Trojan: EC2/BlackholeTraffic
-Trojan: EC2/BlackholeTraffic！ DNS
-Trojan: ec2/dgadomainRequest.B
-trojan: ec2/dgadomainRequest.C！ DNS
-Trojan: EC2/DNSDataExfiltration
-Trojan: EC2/driveBySourceTraffic！ DNS
-Trojan: EC2/DropPoint
-Trojan: EC2/DropPoint！ DNS
-トロイの木馬:EC2/フィッシングドメインリクエスト！ DNS
-UnauthorizedAccess: EC2/MaliciousIPCaller.Custom
-UnauthorizedAccess: EC2/MetadataDNSRebind
-UnauthorizedAccess: EC2/RDPBruteForce
-UnauthorizedAccess: EC2/SSHBruteForce
-UnauthorizedAccess: EC2/TorClient
-UnauthorizedAccess: EC2/TorRelay
-Discovery: S3/MaliciousIPCaller
-CredentialAccess: IAMUser/AnomalousBehavior ior
-DefenseEvasion: IAMUser/AnomalousBehavior or
-Discovery: IAMUser/AnomalousBehavior av
-Exfiltration: IAMUser/AnomalousBehavior
-Impact: IAMUser/AnomalousBehavior ior
-InitialAccess: IAMUser/AnomalousBehavior ior
-Persistence: IAMUser/AnomalousBehavior ior
-Policy: IAMUser/RootCredentialUsage
-PrivilegeEscalation: IAMUser/AnomalousBehavior ior
-Recon: IAMUser/MaliciousIPCaller
-Recon: IAMUser/MaliciousIPCaller.Custom
-Recon: IAMUser/TorIPCaller
-Stealth: IAMUser/CloudTrailLoggingDisabled
-Stealth: IAMUser/PasswordPolicyChange
-UnauthorizedAccess: IAMUser/ConsoleLoginSuccess.B
-UnauthorizedAccess: IAMUser/InstanceCredentialExfiltration
-UnauthorizedAccess: IAMUser/MaliciousIPCaller
-UnauthorizedAccess: IAMUser/MaliciousIPCaller.Custom
-UnauthorizedAccess: IAMUser/TorIPCaller

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

### レスポンスステップ
1. [**準備**] 公開されている資産インベントリ
2. [**準備**] セットアップロギング
3. [**準備**] クラウドフォレンジック機能を特定して評価するためのトレーニングプログラムを実装する
4. [**準備**] エスカレーション手順の特定、文書化、およびテストを行う
5. [**検出と分析**] CloudTrail のレビュー
6. [**検出と分析**] CloudWatch をレビューする
7. [**検出と分析**] AWS Config を使用してセキュリティグループの設定を確認する
8. [**検出と分析**] コンソールで VPC Flow Logs を表示する
9. [**検出と分析**] Amazon Athena を使用したフローログのクエリ
10. [**CONTAINMENT**] 不正なネットワーク変更を実行したユーザーまたはロールを特定する
11. [**ERADICATION**] 侵害されたアクセスキーによるアクティビティの CloudTrail イベント履歴のレビューの結果を確認します。
12. [**根絶**] 予期せぬ請求を回避するのを見直す
13. [**リカバリ**] ネットワークリソースに発生した変更を元に戻す
14. [**準備**] 追加の予防措置:AWS Config と AWS システムマネージャー
15. [**準備**] 追加の予防措置:AWS Security Hub
16. [**準備**] 追加の予防措置:全体的なセキュリティ姿勢

***対応手順は、NIST 特別刊行物 800-61r2 のインシデント対応ライフサイクルに従います。
[NIST コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf)
! [画像] (images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: ツール:フォレンジック
* **カテゴリー**: フォレンジック
* **リソース**: EC2、VPC
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス
* **ログソース**: エンドポイント
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

Prowler は、インターネットに公開されている多くのリソースを見つけるのにも役立ちます:`. /prowler-g group17`

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
-たとえば、my-logs という名前のサブフォルダーを my-bucket というバケットに指定するには、次の ARN を使用します。`arn: aws: S3።: my-bucket/my-logs/`
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
-たとえば、my-logs という名前のサブフォルダーを my-bucket というバケットに指定するには、次の ARN を使用します。`arn: aws: S3።: my-bucket/my-logs/`
-バケットを所有している場合は、リソースポリシーが自動的に作成され、バケットにアタッチされます。 詳細については、「フローログの Amazon S3 バケットのアクセス許可」を参照してください。
1. [ログレコード形式] で、フローログレコードの形式を選択します。
* デフォルトの形式を使用するには、AWS default format を選択します。
* カスタムフォーマットを使用するには、[カスタムフォーマット] を選択し、[ログ形式] からフィールドを選択します。
1. [Create flow log] を選択します。

** NAT ゲートウェイの監視:** NAT ゲートウェイから情報を収集し、読み取り可能なほぼリアルタイムのメトリクスを作成する CloudWatch を使用して NAT ゲートウェイをモニタリングできます。
1. NAT ゲートウェイメトリックスは、1 分間隔で CloudWatch に送信されます。
1. NAT ゲートウェイのメトリックは、次のように表示できます。
* CloudWatch コンソールを https://console.aws.amazon.com/cloudwatch/ で開く
* ナビゲーションペインで、[Metrics] を選択します。
* [すべてのメトリクス] で、NAT ゲートウェイメトリック名前空間を選択します。
* 指標を表示するには、指標ディメンションを選択します。
1. NAT ゲートウェイを経由するアウトバウンドトラフィックのアラームを作成するには、次の手順を実行します。
* CloudWatch コンソールを https://console.aws.amazon.com/cloudwatch/ で開く
* ナビゲーションペインで、[アラーム]、[アラームの作成] の順に選択します。
* NAT ゲートウェイを選択します。
* NAT ゲートウェイと bytesoutToDestination メトリックを選択し、[次へ] を選択します。
* アラームを次のように設定し、完了したら [Create Alarm] を選択します。
* [アラームのしきい値] に、アラームの名前と説明を入力します。 [いつでも] に [>=] を選択し、5000000 と入力します。 連続する期間に 1 を入力します。
* [アクション] で、既存の通知リストを選択するか、[新規リスト] を選択して新しい通知リストを作成します。
* [アラームプレビュー] で、15 分の期間を選択し、[合計] の統計を指定します。
1. ポート割り当てエラーを監視するアラームを作成するには
* CloudWatch コンソールを https://console.aws.amazon.com/cloudwatch/ で開く
* ナビゲーションペインで、[アラーム]、[アラームの作成] の順に選択します。
* NAT ゲートウェイを選択します。
* NAT ゲートウェイと errorPortAllocation メトリックを選択し、[次へ] を選択します。
* アラームを次のように設定し、完了したら [Create Alarm] を選択します。
* [アラームのしきい値] に、アラームの名前と説明を入力します。 [いつでも] で、[>] を選択し、0 と入力します。 連続する期間に 3 を入力します。
* [アクション] で、既存の通知リストを選択するか、[新規リスト] を選択して新しい通知リストを作成します。
* [アラームプレビュー] で、5 分の期間を選択し、[最大] の統計情報を指定します。

### トレーニング
-「社内のアナリストが AWS API（コマンドライン環境）、S3、RDS、その他の AWS サービスに精通するためのトレーニングはどのようなものですか？ `
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

### CloudTrail
1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. CloudTrail ログで、イベント名 `セキュリティグループの承認`、`revokeSecurityGroupingress`、`revokeSecurityGroupegress`、`revokeSecurityGroupegress`、`revokeSecurityGroupegress`を確認して

### CloudWatch
CloudWatch コンソールを使用してイベントでトリガーする CloudWatch イベントルールを作成します。

1. CloudWatch コンソールを開きます。
1. ナビゲーションペインで、[イベント] の [ルール] を選択し、[ルールの作成] を選択します。
1. イベントパターンを選択します。
1. [サービス名] で、[EC2] を選択します。
1. [イベントタイプ] で、[CloudTrail 経由の AWS API コール] を選択します。
1. [特定の操作] を選択し、次の API 呼び出しを指定します。 これらの API 呼び出しは、セキュリティグループルールを追加または削除するために使用されます。
-AuthorizeSecurityGroupIngress
-AuthorizeSecurityGroupEgress
-RevokeSecurityGroupIngress
-RevokeSecurityGroupEgress
1. これらの設定により、次のイベントパターンが作成されます。
```json
{
「ソース」: [
「aws.ec2"
]、
「詳細タイプ」: [
「CloudTrail 経由の AWS API コール」
]、
「詳細」: {
「EventSource」: [
「ec2.amazonaws.com」
]、
「イベント名」: [
「AuthorizeSecurityGroupIngress」,
「AuthorizeSecurityGroupEgress」,
「RevokeSecurityGroupIngress」,
「RevokeSecurityGroupEgress」
]
}
}
```
1. [ターゲットの追加] を選択します。
1. ターゲットのリストで、[SNS トピック] を選択します。
1. [トピック] で、既存のトピックを選択し、新しいトピックを作成します。
1. [詳細を設定] を選択します。
1. [ルールの詳細の構成] ページで、名前とオプションの説明を入力します。
-[状態] で、[有効] ボックスを選択したままにします。
1. [ルールの作成] を選択します。

### AWS Config を使用してセキュリティグループの設定を確認する
AWS Config と AWS Config ルールを使用する計画を立てるときは、まずサービスをリアクティブまたはプロアクティブに使用するかを決定します。
-AWS Config をリアクティブに使用するには、サービスを有効にすると、AWS リソースのインベントリと変更履歴の作成が開始されます。
-プロアクティブな変更通知に AWS Config を使用するには、次のタスクを実行します。
1. Amazon SNS トピックを作成して、設定変更通知を受信します。
1. AWS Lambda 関数または EC2 ベースの SQS メッセージプロセッサを開発して、ネットワーク固有の変更を特定し、適切な通知を送信します。
1. 変更通知には、少なくとも 2 人の異なる受信者を検討してください。
-すぐに人的介入を必要とする通知を受信するためのネットワークセキュリティメールまたはページング配布リスト（グローバルにすべてのアクセスを許可するようにセキュリティグループを変更したり、インターネットゲートウェイを内部専用 VPC に接続するなど）。
-承認された変更要求と実際の構成の変更を調整するために使用できる、中央ロギングまたは変更管理システム。
1. AWS Lambda 関数または SQS キューを変更通知 SNS トピックにサブスクライブします。
1. 変更通知 SNS トピックに変更通知を発行するように AWS Config を設定します。

次に、AWS Config を Lambda 関数と組み合わせて使用する例を示します。

1. [Lambda 実行ロール] (http://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) と [AWS Config ロール] (http://docs.aws.amazon.com/config/latest/developerguide/iamrole-permissions.html) を作成します。
1. AWS Config を有効にして、セキュリティグループの設定を記録して、AWS Config がセキュリティグループの設定の変更を監視できるようにします。 次のスクリーンショットは、設定の例を示しています。
構成設定の例を示すスクリーンショット
1. [セキュリティグループを作成する] (http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_SecurityGroups.html) で、ビジネス要件に関連するポートを有効にします（例：HTTP の場合は 80/443、SMTP の場合は 25）
1. [GitHub の AWS Config ルールライブラリのコード] (https://github.com/awslabs/aws-config-rules/blob/master/python/ec2_security_group_ingress.py) を使用して Python Lambda 関数を作成します。
-Lambda 関数コードは、プロトコル、ポート範囲、および IP 範囲を表す要素を使用して REQUIRED_PERMISSIONS という名前のリストを定義します。 この場合、ポート 80 と 443 のみが許可されます。
-ビジネスニーズに必要なポートと IP 範囲を使用して、この Lambda 関数をカスタマイズします。
1. [AWS Config ルールを作成する] (https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config_develop-rules_nodejs.html#creating-a-custom-rule-with-the-AWS-Config-console) は、ステップ 4 で作成した Lambda 関数を使用します。
-[トリガータイプ] で、[構成の変更] を選択します。
-[変更の範囲] で、[EC2: SecurityGroup] を選択し、ステップ 3 で作成したセキュリティグループの ID を入力します。
1. Config ルールを実行します。 これにより、ルールが実行のためにキューに入れられ、ルールは約 10 分で完了するまで実行されます。
1. ステップ 3 で作成したセキュリティグループを確認します。

## エスカレーション手順
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 分析
### コンソールでの VPC Flow Logs の表示
**ネットワークインターフェイスのフローログに関する情報を表示するには**
1. Amazon [EC2 コンソール] を開きます (https://console.aws.amazon.com/ec2/)
1. ナビゲーションペインで、[ネットワークインターフェイス] を選択します。
1. ネットワークインターフェイスを選択し、[フローログ] を選択します。 フローログに関する情報がタブに表示されます。 [Destination type] 列には、フローログがパブリッシュされる宛先が示されます。

**VPC またはサブネットのフローログに関する情報を表示するには**
1. https://console.aws.amazon.com/vpc/ で Amazon VPC コンソールを開きます。
1. ナビゲーションペインで、[VPC] または [Subnets] を選択します。
1. VPC またはサブネットを選択し、[フローログ] を選択します。
-フローログに関する情報がタブに表示されます。
-[宛先タイプ] 列には、フローログがパブリッシュされる宛先が示されます。

**Amazon S3 に発行されたフローログレコードを表示するには**
1. https://console.aws.amazon.com/s3/ で Amazon S3 コンソールを開きます。
1. [Bucket name] で、フローログを公開するバケットを選択します。
1. [名前] で、ログファイルの横にあるチェックボックスをオンにします。 オブジェクトの概要パネルで、[ダウンロード] を選択します。

### Amazon Athena を使用したフローログのクエリ
Amazon Athena は、標準 SQL を使用して、フローログなどの Amazon S3 のデータを分析できる対話型クエリサービスです。 VPC Flow Logs で Athena を使用すると、VPC を流れるトラフィックに関する実用的なインサイトをすばやく得ることができます。 たとえば、仮想プライベートクラウド（VPC）内のどのリソースがトップトーカーであるかを特定したり、最も拒否された TCP 接続を持つ IP アドレスを特定したりできます。

VPC フローログと Athena との統合を合理化および自動化するには、必要な AWS リソースと事前定義されたクエリを作成する CloudFormation テンプレートを生成し、VPC を流れるトラフィックに関するインサイトを取得できます。

1. CloudFormation テンプレートは次のリソースを作成します。
-Athena データベース。 データベース名は vpcflowlogsathenadatabase <flow-logs-subscription-id><flow-logs-subscription-id>です。
-アAthena のワークグループ。 <flow-log-subscription-id><partition-load-frequency><start-date><end-date>workgroup <flow-log-subscription-id><partition-load-frequency><start-date><end-date>名はワークグループです。
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
-Amazon VPC コンソールを開きます。 ナビゲーションペインで、[VPC] を選択し、VPC を選択します。
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
1. 成功メッセージで [CloudFormation スタックの作成] を選択し、AWS CloudFormation コンソールでスタックの作成ウィザードを開きます。
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
* VpcFlowLogsTopTalkingSubnets s — トラフィックが最も多く記録されている 50 のサブネットの ID。
* VpcFlowLogsTopTCPTraffic — 送信元 IP アドレスについて記録されたすべての TCP トラフィック。
* VpcFlowLogsTotalBytesTransferred — 記録されたバイト数が最も多い送信元と宛先 IP アドレスのペアが50組。
* VpcFlowLogsTotalBytesTransferredPacketLevel — 記録されたバイト数が最も多いパケットレベルの送信元および宛先 IP アドレスの 50 のペア。
* VpcFlowLogsTrafficFrmSrcAddr — 特定の送信元 IP アドレスについて記録されたトラフィック。
* VpcFlowLogsTrafficToDstAddr — 特定の宛先 IP アドレスについて記録されたトラフィック

## 封じ込め
### ユーザー識別
不正なネットワーク変更を実行したユーザーまたはロールを特定する

次に、EC2 インスタンスの作成の背後にあるユーザーまたはプロファイルを特定する例を示します。

1. ステップ 1 で疑わしい EC2 インスタンスのインスタンス ID の 1 つを特定します。
1. AWS コンソールを開く
1. [サービス]、[CloudTrail]
1. 左側の余白で、[Event History] を選択します。
1. 「false」の右側にある「X」を選択して、現在のフィルタをクリアします。
1. 右端 ([カスタム] の下) で、歯車アイコンをクリックします。
1. 下にスクロールして「AWS アクセスキー」を有効にします。
1. 中央で、フィルタのドロップダウンを「リソース名」に変更します。
1. 検索ボックスに EC2 インスタンス ID を入力し、Enter キーを押します。
1. RunInstances イベントが表示されるはずです。これをクリックします。
1. 検出したユーザー名を記録します。
-CloudFormation テンプレートから作成されたイベントなど、元のイベントを見つけるために複数のイベントがある場合があります。
1. Event History に戻り、ドロップダウンを「ユーザー名」に変更し、前の手順のエントリを検索に投稿し、Enter キーを押します。
1. 疑わしいユーザーまたは侵害されたユーザーからの他のイベントを特定する
1. それでは、Services、IAM（画面の左上）に移動しましょう
1. [IAM リソース] で、[ユーザー] を選択します。
1. CloudTrail で特定したアカウントを探して選択します。
1. セキュリティ認証情報の選択
1. アクセスキーまでスクロールして「x」を選択してキーを削除します。
1. チャレンジダッシュボードに移動し、[進捗状況を確認] ボタンを選択します。

## 撲滅
### [侵害されたアクセスキーによるアクティビティの CloudTrail イベント履歴の確認] (. /#cloudtrail)
侵害されたキーによって作成されたリソースをすべて削除します。 AWS リソースを起動したことがないリージョンでも、すべての AWS リージョンを確認してください。
* **重要**: 調査のためにリソースを保持する必要がある場合は、それらをバックアップすることを検討してください。 たとえば、EC2 インスタンスを保持する必要がある規制、コンプライアンス、または法的要件がある場合は、インスタンスを終了する前に EBS スナップショットを作成します。

### [予期せぬ請求を避ける] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/checklistforunwantedcharges.html)
アカウント内で認識されているサービスを確認し、削除します。 次のリソースに特に注意してください。
* EC2 インスタンスと AMI（停止状態のインスタンスを含む）
* EBS ボリュームとスナップショット
* AWS Lambda 関数とレイヤー

Lambda 関数とレイヤーを削除するには、次の手順を実行します。
1. Lambda コンソールを開きます。
1. ナビゲーションペインで、[関数] を選択します。
1. 削除する関数を選択します。
1. [アクション] で、[削除] を選択します。
1. ナビゲーションペインで、[レイヤー] を選択します。
1. 削除するレイヤーを選択します。
1. [削除] を選択します。

## リカバリ
ネットワークリソースに対して発生した変更を元に戻す

インターネットに公開されているリソースを特定し、保護する:`. /prowler-g group17`

## 予防措置
### AWS Config と AWS システムマネージャー
[AWS Config と AWS システムマネージャーでインターネットアクセス可能なポートを自動修復する方法] (https://aws.amazon.com/blogs/security/how-to-auto-remediate-internet-accessible-ports-with-aws-config-and-aws-system-manager/)

### AWS Security Hub
[新しい AWS 基礎セキュリティベストプラクティスのセキュリティ標準を有効にする] (https://aws.amazon.com/blogs/security/aws-foundational-security-best-practices-standard-now-available-security-hub/?secd_dir1)

### 全体的なセキュリティポスチャ
環境に対して [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) を実行して、この Playbook では特定されない他のリスクおよび潜在的に他のパブリックエクスポージャーをさらに特定します。

## 学んだ教訓
「これは、「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要なアイテムを追加するための場所です。 `

## アドレス指定バックログ項目
-インシデントレスポンダーとして、すべての重要な VPC 環境を監視できる必要があります
-インシデントレスポンダーとして、ビジネスチーム、インフラストラクチャチーム、セキュリティチームと調整する方法が必要です。
-インシデントレスポンダーとして VPC FlowLogs を大規模に照会するためのプレイブックが必要です
-インシデントレスポンダーとして、すべてのアカウントで Config を適切に使用できるようにする必要があります

## 現在のバックログアイテム