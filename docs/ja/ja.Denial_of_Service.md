# インシデント対応 Playbook: サービス拒否/分散型サービス拒否 (DoS/DDoS)
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について独自に評価する責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2021 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`\
承認者:`承認者名`\
最終承認日:

## エグゼクティブサマリー
このプレイブックでは、AWS でホストされるリソースに対するサービス拒否/分散サービス拒否（DoS/DDoS）攻撃に応答するプロセスの概要を説明します。

## 妥協の潜在的な指標
-人気または悪意のある意図により、インターネット向けアプリケーションの負荷が重い
-単一の EC2 インスタンスがアプリケーションのホストに使用されており、トラフィックをドロップ/シェーピングしています（AWS Abuse によってお客様に通知されます）
-AWS Shield ダッシュボードに表示される結果

## AWS GuardDuty の潜在的な調査結果
-Backdoor: EC2/DenialOfService.Dns
-Backdoor: EC2/DenialOfService.Tcp
-Backdoor: EC2/DenialOfService.Udp
-Backdoor: EC2/DenialOfService.UdpOnTcpPorts
-Backdoor: EC2/DenialOfService.UnusualProtocol
-Backdoor: EC2/Spambot
-Behavior: EC2/NetworkPortUnusual
-Behavior: EC2/TrafficVolumeUnusual

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
1. [**準備**] 公開されているアセットインベントリを作成する
2. [**準備**] DoS/DDoS 攻撃に対処するためのトレーニングを実装する
3. [**準備**] イベントをエスカレートして報告するコミュニケーション戦略を策定する
4. [**準備**] ドキュメントレビューを実行して、手順が維持され、最新の状態であることを確認します
5. [**検出と分析**] AWS Shield を実装する
6. [**検出と分析**] AWS Shield でGlobal Threat Environment Dashboard を利用する
7. [**検出と分析**] AWS Shield Advanced の使用を検討する
8. [**準備**] エスカレーションと通知手順を実装する
9. [**検出と分析**] 検出と軽減を実行
10. [**検出と分析**] トップコントリビューターを特定する
11. [**封じ込め**] 封じ込めを実行する
12. [**撲滅**] 根絶を行う
13. [**インシデント後のアクティビティ**] AWS Shield Advanced でクレジットをリクエストする
14. [**準備**] 追加の予防措置：軽減手法
15. [**準備**] 追加の予防措置：攻撃面の削減
16. [**準備**] 追加の予防措置：運用テクニック
17だ [**準備**] 追加の予防措置:全体的なセキュリティ姿勢

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理

* **戦術、テクニック、手順**: サービス拒否/分散型サービス拒否攻撃
* **カテゴリ**: DoS/DDoS
* **リソース**: ネットワーク
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス、AWS Shield
* **ログソース**: AWS CloudTrail, AWS Config, VPC Flow Logs, Amazon GuardDuty
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
インターネットに公開されているリソースを見つける:`. /prowler-g group17`

セキュリティハブとファイアウォールマネージャを使用して、[DDoS イベントの一元的なモニタリングを設定し、非準拠のリソースを自動修正する] (https://aws.amazon.com/blogs/security/set-up-centralized-monitoring-for-ddos-events-and-auto-remediate-noncompliant-resources/)

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

### ドキュメントレビュー
1. [AWS Shield Documentation] (https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html) に精通している
1. 購読している場合は、[AWS Shield 入門アドバンスドガイド] (https://docs.aws.amazon.com/waf/latest/developerguide/getting-started-ddos.html) に従ってください

## 検出
### DoS/DDoS イベントを検出する Cloudwatch メトリクス
推奨される Amazon CloudWatch Metrics 表には、DDoS 攻撃の検出と対応に一般的に使用される Amazon CloudWatch メトリクスの説明が記載されています。

* AWS Shield Advanced: DDoSDetected
* 特定の Amazon Resource Name (ARN) の DDoS イベントを示します。
* AWS Shield Advanced: DDoSAttackBitsPerSecond
* 特定の ARN の DDoS イベント中に観測されたバイト数。 このメトリックは、レイヤ 3/4 DDoS イベントでのみ使用できます。
* AWS Shield Advanced: DDoSAttackPacketsPerSecond
* 特定の ARN の DDoS イベント中に観測されたパケット数。 このメトリックは、レイヤ 3/4 DDoS イベントでのみ使用できます。
* AWS Shield Advanced: DDoSAttackRequestsPerSecond
* 特定の ARN の DDoS イベント中に観察されたリクエストの数。 このメトリックは、レイヤ 7 DDoS イベントでのみ使用でき、最も重要なレイヤ 7 イベントについてのみレポートされます。
* AWS WAF: AllowedRequests
* 許可されるウェブリクエストの数。
* AWS WAF: BlockedRequests
* ブロックされたウェブリクエストの数。
* AWS WAF: CountedRequests
* カウントされたウェブリクエストの数。
* AWS WAF: PassedRequests
* 渡されたリクエストの数。 これは、ルールグループ規則のいずれとも一致せず、ルールグループ評価を通過するリクエストに対してのみ使用されます。
* CloudFront: Requests
* HTTP/S リクエストの数。
* CloudFront: TotalErrorRate
* HTTP ステータスコードが 4xx または 5xx であるすべてのリクエストの割合。
* Amazon Route 53: HealthCheckStatus
* ヘルスチェックエンドポイントのステータス。
* ALB: ActiveConnectionCount
* クライアントからロードバランサーへ、およびロードバランサーからターゲットへのアクティブな同時 TCP 接続の合計数。
* ALB: ConsumedLCUs
* ロードバランサーが使用するロードバランサーのキャパシティユニット (LCU) の数。
* ALB: HTTPCode_ELB_4XX_Count HTTPCode_ELB_5XX_Count
* ロードバランサーによって生成された HTTP 4xx または 5xx クライアントエラーコードの数。
* ALB: newConnectionCount
* クライアントからロードバランサーへ、およびロードバランサーからターゲットに確立された、新しい TCP 接続の合計数。
* ALB: processedBytes
* ロードバランサーによって処理された総バイト数。
* ALB: RejectedConnectionCount
* ロードバランサーが最大接続数に達したため、拒否された接続数。
* ALB: RequestCount
* 処理されたリクエストの数。
* ALB: TargetConnectionErrorCount
* ロードバランサーとターゲット間で正常に確立されなかった接続の数。
* ALB: targetResponseTime
* リクエストがロードバランサーを離れてから、ターゲットからの応答を受信するまでの経過時間（秒単位）。
* ALB: unhealthyHostCount
* 異常と見なされるターゲットの数。
* NLB: ActiveFlowCount
* クライアントからターゲットへの同時 TCP フロー（または接続）の総数。
* NLB: ConsumedLCUs
* ロードバランサーが使用するロードバランサーのキャパシティユニット (LCU) の数。
* NLB: newFlowCount
* 期間内にクライアントからターゲットに確立された新しい TCP フロー（または接続）の総数。
* NLB: processedBytes
* TCP/IP ヘッダーを含む、ロードバランサーによって処理された総バイト数。
* Global Accelerator NewFlowCount
* 期間内にクライアントからエンドポイントに確立された、新しい TCP および UDP フロー（または接続）の総数。
* Global Accelerator: ProcessedBytesIn
* TCP/IP ヘッダーを含む、アクセラレータによって処理された着信バイトの総数。
* Auto Scaling: GroupMaxSize
* Auto Scaling グループの最大サイズ。
* Amazon EC2: CPUUtilization
* 現在使用中の割り当てられた EC2 コンピューティングユニットの割合。
* Amazon EC2: NetworkIn
* すべてのネットワークインターフェイスでインスタンスが受信したバイト数。
### AWS Shield
[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/shieldv2) または API は、検出された攻撃に関する概要と詳細を提供します。
1. AWS マネジメントコンソールにサインインし、[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/) を開きます。
1. AWS Shield のナビゲーションバーで、[概要] または [イベント] を選択します。

### Global Threat Environment Dashboard
Global Threat Environment Dashboard には、AWS によって検出されたすべての DDoS 攻撃に関する概要情報が表示されます。
1. AWS マネジメントコンソールにサインインし、[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/) を開きます。
1. AWS Shield のナビゲーションバーで、[グローバル脅威ダッシュボード] を選択します。
1. 期間を選択します。

### AWS Shield Advanced
1. AWS マネジメントコンソールにサインインし、[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/) を開きます。
1. AWS Shield のナビゲーションバーで、[概要] または [イベント] を選択します。

アプリケーションがターゲットになっていることを示すことができる CloudWatch メトリックスへのアクセス権があります。
1. DDosDetected メトリックを設定して、攻撃が検出されたかどうかを確認できます。
1. 攻撃ボリュームに基づいてアラートを送信する場合は、ddosattackBits/秒、ddosattackPackets/秒、または dDDoSAttackRequestsPerSecond のメトリックを使用することもできます。

使用可能なメトリックスの完全なリストは、[DDoS の可視性] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html) の表内で確認できます。

## エスカレーション手順

### AWS Shield
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

### AWS Shield Advanced
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-`Shield Advanced を使用して保護する AWS リソースを指定しましたか？ `
1. AWS マネジメントコンソールにサインインし、[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/) を開きます。
1. AWS Shield ナビゲーションバーで、[保護されたリソース] を選択します。
-`DDoS レスポンス・チーム（DRT）にWAFルールとログへのアクセスを許可しましたか？ これにより、ヘルプをリクエストしたときに攻撃を緩和できます。 `
1. AWS マネジメントコンソールにサインインし、[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/) を開きます。
1. AWS Shield のナビゲーションバーで、[概要] を選択します。
1. AWS DRT サポートを設定する `DRT アクセスの編集`
-`AWS DDoS レスポンスチームのサポートが必要ですか？ `
1. AWS サポートセンターの AWS Shield でケースを開くことができます
* DDoS レスポンスチーム (DRT) のサポートを受けるには、AWS サポートセンターに連絡し、以下のオプションを選択してください。
1. ケースタイプ:テクニカルサポート
1. サービス:分散型サービス拒否 (DDoS)
1. カテゴリ:AWS へのインバウンド
* AWS Shield Advanced プロアクティブエンゲージメントでは、Shield Advanced で検出されたイベント中に、保護されたリソースに関連付けられた Amazon Route 53 ヘルスチェックが異常になった場合、DRT から直接連絡します。

## 分析
1. AWS マネジメントコンソールにサインインし、[AWS WAF & Shield console] (https://console.aws.amazon.com/wafv2/) を開きます。
1. AWS Shield のナビゲーションバーで、[イベント] を選択します。

### 検出と軽減
[Detection and Mitigation] タブには、このイベントをレポートする前に AWS Shield が評価したトラフィックと、発生した緩和策の効果を示すグラフが表示されます。 また、お客様独自の Amazon 仮想プライベートクラウド VPC および AWS グローバルアクセラレータ内のリソースのインフラストラクチャレイヤ分散サービス拒否 (DDoS) イベントのメトリックスも提供します。 Amazon CloudFront または Amazon Route 53 リソースには、検出および軽減メトリックスは含まれません。

### 上位寄稿者
[上位コントリビューター] タブでは、検出されたイベント中にトラフィックがどこから送信されるかを把握できます。 プロトコル、送信元ポート、TCP フラグなどの側面でソートされた、最上位のボリュームコントリビュータを表示できます。 情報をダウンロードして、使用するユニットと表示するコントリビュータの数を調整できます。

## 封じ込め

### [DDoS 回復力のための AWS ベストプラクティス] (https://d0.awsstatic.com/whitepapers/Security/DDoS_White_Paper.pdf)
このホワイトペーパーでは、AWS で実行されているアプリケーションの復元力を向上させるための規範的な DDoS ガイダンスを提供しています。 これには、アプリケーションの可用性を保護するためのガイドとして使用できる DDoS 復元機能のリファレンスアーキテクチャが含まれます。 このホワイトペーパーでは、インフラストラクチャ層攻撃やアプリケーション層攻撃など、さまざまな攻撃タイプについても説明します。 AWS は、各攻撃タイプの管理に最も効果的なベストプラクティスを説明しています。 さらに、DDoS 軽減戦略に適合するサービスと機能の概要と、各サービスを使用してアプリケーションを保護する方法について説明します。
封じ込め手順では、AWS ウェブアプリケーションファイアウォールの使用を前提としています。 サードパーティの WAF またはファイアウォールを使用している場合は、ユースケースに合わせてこれらの手順をここでカスタマイズしてください。

### AWS WAF
1. AWS WAF で、異常な動作に一致する条件を作成します。
1. これらの条件を 1 つ以上の AWS WAF ルールに追加します。
1. これらのルールをウェブ ACL に追加し、ルールに一致する要求をカウントするようにウェブ ACL を設定します。
* **注意** 最初にブロックではなく Count を使用してルールをテストする必要があります。 新しいルールで正しいリクエストを特定できるようになったら、ルールを変更してそれらのリクエストをブロックできます。
1. これらの数を監視して、リクエストのソースをブロックする必要があるかどうかを判断します。 リクエストの量が異常に高い場合は、ウェブ ACL を変更してそれらのリクエストをブロックします。

## 撲滅
該当なし

## リカバリ
### AWS Shield Advanced でクレジットをリクエストする
AWS Shield Advanced を購読していて、シールドアドバンストで保護されたリソースの使用率を増加させる DDoS 攻撃が発生した場合、Shield Advanced によって軽減されない限り、使用率の増加に関連する料金のクレジットをリクエストできます。

追加情報は、[AWS Shield Advanced でクレジットをリクエストする] (https://docs.aws.amazon.com/waf/latest/developerguide/request-refund.html) の AWS ドキュメントに記載されています。

## 予防措置
### 軽減テクニック
AWS では、DDoS 軽減機能が自動的に提供されます。
1. AWS Shield Standard は、ウェブサイトやアプリケーションをターゲットとする、最も一般的で頻繁に発生するネットワークおよびトランスポート層の DDoS 攻撃から保護します。 これは、すべての AWS サービスおよびすべての AWS リージョンで、追加料金なしで提供されます。
1. AWS Shield Advanced をサブスクライブすることで、DDoS 攻撃への対応と軽減の準備を改善できます。 任意の DDoS 軽減サービスは、任意の AWS リージョンでホストされているアプリケーションを保護するのに役立ちます。 このサービスは、Amazon CloudFront および Amazon Route 53 でグローバルに利用できます。 また、クラシックロードバランサー（CLB）、アプリケーションロードバランサー（ALB）、および Elastic IP アドレス（EIP）の選択 AWS リージョンでも利用できます。 EIP で AWS Shield Advanced を使用すると、ネットワークロードバランサー（NLB）または Amazon EC2 インスタンスを保護できます。
1. ボリュームメトリック DDoS 攻撃を軽減するための主な考慮事項には、十分な輸送容量と多様性が確保されていること、および Amazon EC2 インスタンスなどの AWS リソースを攻撃トラフィックから保護することが挙げられます。
1. 大規模な DDoS 攻撃は、単一の Amazon EC2 インスタンスの容量を圧倒する可能性があるため、ロードバランシングを追加すると回復力が向上します。
1. Amazon CloudFront では、静的コンテンツをキャッシュして AWS エッジロケーションから提供できるため、オリジンの負荷を軽減できます。
1. AWS WAF を使用すると、CloudFront ディストリビューションまたはアプリケーションロードバランサーでウェブアクセスコントロールリスト（ウェブ ACL）を設定して、リクエスト署名に基づいてリクエストをフィルタリングおよびブロックできます。
### 攻撃面低減
1. インスタンスの起動時にセキュリティグループを指定したり、後でインスタンスをセキュリティグループに関連付けることができます。 セキュリティグループへのすべてのインターネットトラフィックは、トラフィックを許可する許可ルールを作成しない限り、暗黙的に拒否されます。
* AWS Shield Advanced を購読している場合は、Elastic IP (EIP) を保護リソースとして登録できます。
1. VPC 内のオリジンで Amazon CloudFront を使用している場合は、AWS Lambda 関数を使用して、Amazon CloudFront トラフィックのみを許可するようにセキュリティグループルールを自動的に更新する必要があります。
1. 通常、API を一般に公開する必要がある場合、API フロントエンドが DDoS 攻撃の対象となるリスクがあります。 リスクを軽減するために、Amazon API Gateway を Amazon EC2、AWS Lambda、またはその他の場所で実行されているアプリケーションへの入り口として使用できます。
* Amazon API Gateway で Amazon CloudFront と AWS WAF を使用する場合は、次のオプションを設定します。
1. すべてのヘッダーを API Gateway リージョンエンドポイントに転送するように、ディストリビューションのキャッシュ動作を設定します。 これにより、CloudFront はコンテンツを動的として扱い、コンテンツのキャッシュをスキップします。
1. API Gateway で API keyvalue を設定して、オリジンカスタムヘッダー x-api-key を含めるようにディストリビューションを構成して、API Gateway の直接アクセスから保護します。
1. RESTAPIs の各メソッドに標準レート制限またはバーストレート制限を設定して、過剰なトラフィックからバックエンドを保護します。

### 運用テクニック
主要な運用メトリックが期待値から大幅に逸脱すると、攻撃者がアプリケーションの可用性をターゲットにしようとする可能性があります。 アプリケーションの通常の動作に精通している場合は、異常を検出したときにより迅速にアクションを実行できます。
1. DDoS 復元機能のリファレンスアーキテクチャに従ってアプリケーションを設計した場合、一般的なインフラストラクチャ層攻撃はアプリケーションに到達する前にブロックされます。
1. AWS WAF を使用している場合は、CloudWatch を使用して、WAF で設定したリクエストの増加、許可、カウント、またはブロックを監視してアラームできます。 これにより、トラフィックのレベルがアプリケーションで処理できるレベルを超えた場合に通知を受け取ることができます。

DDoS 攻撃の検出と対応に一般的に使用される Amazon CloudWatch メトリクスの説明については、[DDoS 可視性] (https://docs.aws.amazon.com/whitepapers/latest/aws-best-practices-ddos-resiliency/visibility.html) の表を参照してください。
### 全体的なセキュリティポスチャ
環境に対して [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) を実行して、この Playbook では特定されない他のリスクおよび潜在的に他のパブリックエクスポージャーをさらに特定します。

## 学んだ教訓
「これは、「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要なアイテムを追加するための場所です。 `

## アドレス指定バックログ項目
-インシデントレスポンダーとして、AWS への内部と外部の両方で文書化されたエスカレーションパスが必要です

## 現在のバックログアイテム