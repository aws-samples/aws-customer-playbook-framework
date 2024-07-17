# インシデントレスポンスのプレイブック:EC2 Microsoft Windows 身代金レスポンス
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2024 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

## エグゼクティブサマリー
この Playbook では、EC2 インスタンスに対する身代金攻撃に対する応答のプロセスの概要を説明します。

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
4. [**検出と分析**] Microsoft 365 Defender Threat Intelligence Team に記載されている手順を使用する
5. [**検出と分析**] CloudWatch メトリクスを使用して、データが漏洩しているかどうかを判断する
6. [**検出と分析**] vpcFlowLogs を使用して外部 IP アドレスからの不適切なデータベースアクセスを特定する
7. [**封じ込めと根絶**] ネットワークから侵害されたシステムをすべて削除します。
8. [**封じ込めと根絶**] ネットワーク IOC に基づいて NACL を適用して、さらなるトラフィックを防止する
9. [**封じ込めと根絶**] その他の関心項目
10. [**リカバリ**] 必要に応じてリカバリ手順を実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: 身代金とデータ破壊
* **カテゴリー**: 身代金攻撃
* **リソース**: EC2
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス
* **ログソース**: CloudTrail, CloudWatch, AWS Config
* **チーム**: セキュリティオペレーションセンター (SOC)、フォレンジック調査官、クラウドエンジニアリング

## インシデント処理プロセス
### インシデント対応プロセスには、次の段階があります。
* 準備
* 検出と分析
* 封じ込めと根絶
* 回復
* インシデント後の活動

## 準備
* アカウントのセキュリティ体制を評価し、セキュリティギャップを特定して修正する
* AWS はオープンソースのSelf-Service Security Assessment（https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/）ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント
* 組織を保護するために、Microsoft は [人間が運営するランサムウェア緩和プロジェクト計画] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) PowerPoint プレゼンテーションに記載されている情報を使用することをお勧めします。これには、[セキュリティ保護] が含まれています。特権アクセス] (https://aka.ms/spa)
* ドメインコントローラー、Microsoft Windows EC2 インスタンス、Microsoft Windows サーバーとデータベース、および外部 ID プロバイダーとの統合など、すべてのリソースの完全な資産インベントリを維持する
* [Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/) などのユーティリティを使用して、ホストの定期的な脆弱性分析を実行する
* Windows Defender Antivirus をオンにして更新する-商用、サブスクリプション有料エンドポイント検出と応答（EDR）ソリューションが好ましい
* EC2 インスタンスのバックアップを実行する
* [AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) または [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/) の使用を検討する
* [ファイル履歴でファイルをバックアップする] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
* バックアップを確認し、感染が蔓延していないことを確認します
* 重要なファイルを定期的にバックアップする。 3-2-1 ルールを使用します。 データのバックアップを 2 つの異なるストレージタイプに保持し、少なくとも 1 つのバックアップをオフサイトに保存します。
* オペレーティングシステムとアプリに最新のアップデートを適用する
* ソーシャルエンジニアリング攻撃やスピアフィッシング攻撃を特定できるように従業員を教育する
* [制御されたフォルダアクセス] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders) を実装します。 ランサムウェアがファイルを暗号化し、身代金のためにファイルを保持するのを阻止できます。
* [Office ドキュメント内のマクロをブロックする] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
* 既知のランサムウェアのファイルタイプをブロックする
* Windows 10では、ランサムウェアやその他のマルウェアなどの不正なプログラムから重要なローカルフォルダを保護するために、[制御されたフォルダアクセス] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) をオンにします。
* [Active Directory サービスのセキュリティ保護に関するベストプラクティス] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/) に従ってください
* [セキュリティ侵害を防止するための最も重要なグループポリシー設定の上位10位] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
* Microsoft 365 Defender Threat Intelligence Team [包括的なレポート] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/)、ランサムウェアイベントが発生する前にMicrosoft のリソースを保護するためのアクションを特定しました。
* インターネット向けアセットを強化し、最新のセキュリティアップデートを入手してください。 脅威と脆弱性の管理を使用して、脆弱性、構成ミス、および疑わしいアクティビティについて定期的にこれらの資産を監査します。
* Azure 多要素認証 (MFA) などのソリューションを使用して、リモートデスクトップゲートウェイをセキュリティで保護します。 MFA ゲートウェイがない場合は、ネットワークレベル認証 (NLA) を有効にします。
* 最低特権の原則を実践し、資格の衛生を維持する。 ドメイン全体の管理者レベルのサービスアカウントの使用は避けてください。 強力なランダム化されたジャストインタイムのローカル管理者パスワードを強制する
* ブルートフォースの試みを監視する。 過度の失敗した認証の確認 (Windows セキュリティイベント ID 4625)
* イベントログ、特にセキュリティイベントログと PowerShell 操作ログのクリアを監視します。 Microsoft Defender ATP は「Event log was cleared」というアラートを発生させ、Windows はイベント ID 1102 を生成します。
* タンパープロテクション機能をオンにして、攻撃者がセキュリティサービスを停止するのを防ぐ
* 特権の高いアカウントがログオンし、認証情報を公開している場所を特定する。 ログオンタイプの属性について、ログオンイベント (イベント ID 4624) を監視および調査します。 ドメイン管理者アカウントおよび高い権限を持つ他のアカウントは、ワークステーション上に存在しないでください。
* Windows Defender アンチウイルスのクラウド配信保護とサンプル自動送信を有効にします。 これらの機能は、人工知能と機械学習を使用して、新しい脅威や未知の脅威を迅速に特定して阻止します。
* 資格情報の盗難、ランサムウェアのアクティビティ、PsExec とWMI の疑わしい使用をブロックするルールを含む、攻撃面削減ルールを有効にします。 武器化された Office ドキュメントによって開始される悪意のあるアクティビティに対処するには、Office アプリケーションによって開始される高度なマクロアクティビティ、実行可能コンテンツ、プロセスの作成、プロセス注入をブロックするルールを使用します。 これらのルールの影響を評価するには、監査モードで展開します。
* Office Office 365 をお持ちの場合、Office VBA用AMSIをオンにする
* Windows Defender Firewall とネットワークファイアウォールを利用して、可能な限りエンドポイント間の RPC および SMB 通信を防止します。 これにより、横方向の移動やその他の攻撃アクティビティが制限されます。
* [Systems Manager と Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) を使用して、Microsoft Windows を実行している EC2 インスタンスに、一般的な脆弱性およびエクスポージャー (CVE) が含まれているかどうかを確認します。

### AWS Config を使用して設定のコンプライアンスを表示します。
1. AWS マネジメントコンソールにサインインし、https://console.aws.amazon.com/config/ で AWS Config コンソールを開きます。
1. AWS マネジメントコンソールメニューで、リージョンセレクターが AWS Config ルールをサポートするリージョンに設定されていることを確認します。 サポートされているリージョンのリストについては、Amazon ウェブサービス全般のリファレンスの「AWS Config リージョンとエンドポイント」を参照してください。
1. ナビゲーションペインで、[リソース] を選択します。 [リソースインベントリ] ページでは、リソースカテゴリ、リソースタイプ、およびコンプライアンスステータスでフィルタリングできます。 必要に応じて、[削除されたリソースを含める] を選択します。 テーブルには、リソースタイプのリソース識別子と、そのリソースのリソースコンプライアンスステータスが表示されます。 リソース識別子は、リソース ID またはリソース名です。
1. [リソース識別子] 列からリソースを選択します。
1. [リソースタイムライン] ボタンを選択します。 設定イベント、コンプライアンスイベント、または CloudTrail イベントでフィルタリングできます。
1. 具体的には、次のイベントに焦点を当てます。
* ebs-in-backup-plan
* ebs-optimized-instance
* ebs-snapshot-public-restorable-check
* ec2-ebs-encryption-by-default
* ec2-imdsv2-check
* ec2-instance-detailed-monitoring-enabled
* ec2-instance-managed-by-systems-manager
* ec2-instance-multiple-eni-check
* ec2-instance-no-public-ip
* ec2-instance-profile-attached
* ec2-managedinstance-applications-blacklisted っている
* ec2-managedinstance-applications-required
* ec2-managedinstance-association-compliance-status-check
* ec2-managedinstance-inventory-blacklisted っている
* ec2-managedinstance-patch-compliance-status-check
* ec2-managedinstance-platform-check
* ec2-security-group-attached-to-eni
* ec2-stopped-instance
* ec2-volume-inuse-check

## エスカレーション手順
-「EC2 フォレンジックをいつ実施すべきかについてビジネス上の決定が必要です`
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 検出と分析
### [CloudWatch メトリクスを使う] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)
データ漏洩の「スパイク」を探します。 攻撃者がデータ破壊を行い、身代金メモを残した可能性があります。このような場合、悪意のあるアクターと協力してデータ復旧の機会はありません。
1. CloudWatch コンソールを https://console.aws.amazon.com/cloudwatch/ で開く
1. ナビゲーションペインで、[メトリック]、[すべてのメトリック] の順に選択します。
1. [すべてのメトリクス] タブで、インスタンスをデプロイするリージョンを選択します。
1. [すべてのメトリクス] タブで、検索語句 `networkPacketsOut `を入力し、Enter キーを押します。
1. 検索の結果の 1 つを選択して、メトリックスを表示します。
1. 1 つ以上のメトリックをグラフ化するには、各メトリックスの横にあるチェックボックスをオンにします。 すべてのメトリックを選択するには、テーブルの見出し行のチェックボックスをオンにします。
1. (オプション) グラフのタイプを変更するには、[グラフオプション] を選択します。 その後、折れ線グラフ、積み上げ面グラフ、棒グラフ、円グラフ、または数値を選択できます。
1. （オプション）メトリックの期待値を示す異常検出バンドを追加するには、メトリックの横にある「アクション」の下の異常検出アイコンを選択します。

### Microsoft 365 Defender Threat Intelligence Team
チームは、複数のランサムウェアバリアントに対して探すべきものを特定する包括的なレポート（https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/）を提供しました。

### vpcFlowLogs を使用して外部 IP アドレスからの不適切なデータベースアクセスを特定する
1. [AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) を使用してログデータを分析する
1. 特定の IP への、または特定の IP からのすべてのレコードの src_ip、src_port、dst_ip、dst_port クワッドの各バイト数のサマリーを取得します。
```
vpcflow から byte_count として送信元アドレス、宛先アドレス、送信元ポート、宛先ポート、合計 (数バイト) を選択します。
WHERE (送信元アドレス = '192.0.2.1' または宛先アドレス = '192.0.2.1')
そして date_partition >= '2020/07/01'
と date_partition <= '2020/07/31'
そして account_partition = '111122223333'
そして region_partition in ('us-east-1', 'us-east-2', 'us-west-2', 'us-west-2')
送信元アドレス、宛先アドレス、送信元ポート、宛先ポートによるグループ化
バイト数順 DESC
```
1. その他のクエリ例は、[vpcflow_demo_queries.sql] (https://github.com/awslabs/aws-security-analytics-bootstrap/blob/main/AWSSecurityAnalyticsBootstrap/sql/dml/analytics/vpcflow/vpcflow_demo_queries.sql) で提供されています。

## 封じ込め
### 影響を受けるリソースをすぐに隔離する
**注**: リソースを分離するためのエスカレーションと承認を要求するプロセスがあることを確認し、分離が現在のオペレーションと収益源にどのような影響を与えるかについて、ビジネスへの影響分析を最初に実行するようにします。
1. インスタンスが Auto Scaling グループの一部であるか、ロードバランサーにアタッチされているかを判断する
* Auto Scaling Group: グループからインスタンスをデタッチする
* Elastic Load Balancer: ELB からインスタンスを登録解除し、ターゲットグループからインスタンスを削除します。
1. すべての入出力トラフィックをブロックする*new* セキュリティグループを作成します。出力トラフィックのデフォルトの `allow all` ルールを削除してください
1. 影響を受けるインスタンスに*new* セキュリティグループをアタッチします。

### ネットワーク IOC に基づいて NACL を適用して、さらなるトラフィックを防止する
1. [Amazon VPC コンソール] (https://console.aws.amazon.com/vpc/) を開きます
1. ナビゲーションペインで、[Network ACLs] を選択します。
1. [Network ACL の作成] を選択します。
1. [Network ACL の作成] ダイアログボックスで、必要に応じてネットワーク ACL の名前を指定し、VPC リストから VPC の ID を選択します。 次に、[はい、作成] を選択します。
1. 詳細ウィンドウで、追加するルールのタイプに応じて [受信ルール] タブまたは [アウトバウンドルール] タブを選択し、[編集] を選択します。
1. [ルール #] に、ルール番号 (100 など) を入力します。 ルール番号は、ネットワーク ACL でまだ使用されていない必要があります。 ルールを順番に処理し、最小の数字から順に処理します。
* 連番（101、102、103）を使用するよりも、ルール番号（100、200、300 など）の間にギャップを残すことをお勧めします。 これにより、既存のルールを再番号付けしなくても、新しいルールを簡単に追加できます。
1. [タイプ] リストからルールを選択します。 たとえば、HTTP のルールを追加するには、[HTTP] を選択します。 すべての TCP トラフィックを許可するルールを追加するには、[All TCP] を選択します。 これらのオプションの一部 (HTTP など) については、ポートを記入します。 一覧にないプロトコルを使用するには、[カスタムプロトコルルール] を選択します。
1. (オプション) カスタムプロトコルルールを作成する場合は、[プロトコル] リストからプロトコルの番号と名前を選択します。 詳細については、IANA プロトコル番号のリストを参照してください。
1. （オプション）選択したプロトコルにポート番号が必要な場合は、ポート番号またはポート範囲をハイフンで区切って入力します（たとえば、49152-65535）。
1. [送信元] または [宛先] フィールドに（これがインバウンドルールかアウトバウンドルールかに応じて）、ルールが適用される CIDR 範囲を入力します。
1. [許可/拒否] リストから、[許可] を選択して指定したトラフィックを許可するか、[拒否] を選択して指定したトラフィックを拒否します。
1. （オプション）別のルールを追加するには、[別のルールを追加] を選択し、必要に応じて前の手順を繰り返します。
1. 完了したら、[保存] を選択します。
1. ナビゲーションペインで、[Network ACLs] を選択し、ネットワーク ACL を選択します。
1. 詳細ペインの [サブネットの関連付け] タブで、[編集] を選択します。 ネットワーク ACL に関連付けるサブネットの [Associate] チェックボックスをオンにし、[Save] を選択します。

## 撲滅
### ネットワークから侵害されたシステムをすべて削除します。
* 規制要件または社内ポリシーに従って、EC2 インスタンスのフォレンジックが必要かどうかを判断する
* インスタンスのフォレンジックが必要な場合、またはデータを復元する必要がある場合は、[Playbook: EC2 フォレンジック] (. /docs/ec2_forensics.md)

### その他の関心のある項目
* [侵害されたドメインコントローラーのメタデータをドメインから削除する] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
* 感染の可能性についてバックアップを検査する

## リカバリ
* 身代金を払わないことをお勧めします
* 身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです。
* データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を比較検討する必要があります。
* 身代金を支払うことを選択した場合、攻撃者が自分の会社または他のユーザーに対して操作を継続できるようにする
* https://www.nomoreransom.org/ にアクセスして、データが感染したマルウェアバリアントに対して復号化が可能かどうかを確認してください。
* [IAM ユーザーキーの削除またはローテーション] (https://console.aws.amazon.com/iam/home#users) と [Root ユーザーキー] (https://console.aws.amazon.com/iam/home#security_credential)。公開されている特定のキーを特定できない場合、アカウント内のすべてのキーをローテーションすることもできます。
* [許可されていない IAM ユーザーを削除する] (https://console.aws.amazon.com/iam/home#users。)
* [不正なポリシーを削除する] (https://console.aws.amazon.com/iam/home#/policies)
* [権限のない役割を削除する] (https://console.aws.amazon.com/iam/home#/roles)
* [一時的な資格情報を取り消す] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 一時的な認証情報は、IAM ユーザーを削除することで取り消すこともできます。 注:IAM ユーザーの削除は実稼働ワークロードに影響を与える可能性があるため、注意して行う必要があります。* CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
* 代替データバックアップ戦略を使用する場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前にスケジュールされた最後のイベントから復元します。
* 信頼できる AMI から新しい EC2 インスタンスを作成する
* CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
* 代替データバックアップ戦略を使用する場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前にスケジュールされた最後のイベントから復元します。

## 学んだ教訓
「これは、必ず「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要な、貴社固有のアイテムを追加する場所です。 `

## アドレスバックログ項目
-インシデントレスポンダーとして EC2 フォレンジックを実施するには Runbook が必要です
-インシデントレスポンダーとして、EC2 フォレンジックをいつ実施すべきかについてのビジネス上の決定が必要です。
-インシデントレスポンダーとして、使用意図に関係なく有効になっているすべてのリージョンでログを有効にする必要があります。
-インシデントレスポンダーとして、既存の EC2 インスタンスで暗号マイニングを検出できる必要があります。

## 現在のバックログアイテム