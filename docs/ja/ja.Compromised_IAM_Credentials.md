# インシデントレスポンス Playbook: 侵害された IAM 認証情報
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2021 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`\
承認者:`承認者名`\
最終承認日:

## エグゼクティブサマリー
この Playbook では、AWS アカウント内の不正なアクティビティを観察した場合、または権限のない当事者がお客様のアカウントにアクセスしたと思われる場合に対応するためのプロセスの概要を説明します。

## 妥協の潜在的な指標
-新規または認識されない IAM ユーザー
-認識されないリソース、または許可されていないリソース (EC2、Lambda など)
-異例の課金が増える
-セキュリティ研究者からの通知
-AWS リソースまたはアカウントが侵害される可能性があるという通知

## AWS GuardDuty の潜在的な調査結果
-CredentialAccess: IAMUser/AnomalousBehavior ior
-DefenseEvasion: IAMUser/AnomalousBehavior or
-Discovery: IAMUser/AnomalousBehavior av
-Exfiltration: IAMUser/AnomalousBehavior
-Impact: IAMUser/AnomalousBehavior ior
-InitialAccess: IAMUser/AnomalousBehavior ior
-PenTest: IAMUser/KaliLinux
-PenTest: IAMUser/ParrotLinux
-PenTest: IAMUser/PentooLinux
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
1. [**準備**] 資産インベントリの実行
2. [**準備**] 公開された IAM 認証情報を特定して対応するためのトレーニングプランを実装する
3. [**準備**] インシデント対応のためのコミュニケーション戦略を実装する
4. [**検出**] Root アカウントアクセスを特定する (承認済みでない)
5. [**検出**] 新規または認識されない IAM ユーザーを特定する
6. [**検出**] 認識されていないリソースまたは許可されていないリソース (例:EC2、Lambda) を特定する
7. [**検出**] 露出した秘密を特定して見つける
8. [**検出**] 異常な請求の増加を特定する
9. [**検出**] AWS リソースまたはアカウントが侵害される可能性があることを示す AWS またはサードパーティからの通知に応答する
10. [**検出**] 不正の可能性がある IAM ユーザー認証情報を特定する
11. [**準備**] エスカレーション手順を特定する
12. [**検出と分析**] CloudTrail ログを確認する
13. [**検出と分析**] VPC Flow Logs の確認
14. [**検出と分析**] エンドポイント/ホストベースのログを確認する
15. [**CONTAINMENT**] 適切な封じ込めアクションを実行する
16. [**ERADICATION**] 侵害されたアクセスキーによるアクティビティの CloudTrail イベント履歴のレビューの結果を確認します
17だ [**根絶**] 予期せぬ請求を回避するのを見直す
18日。 [**リカバリ**] 適切なリカバリアクションを実行する
19日。 [**準備**] Prowler IAM スキャンを実行する
20日。 [**準備**] MFAを有効にする
21。 [**準備**] アカウント情報を確認する
22. [**準備**] AWS Git プロジェクトを使用して不正使用の証拠をスキャンする
23. [**準備**] 日常的な操作に root ユーザーを使用することは避ける
24だ [**準備**] 全体的なセキュリティ姿勢を評価する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: 資格の露出
* **カテゴリ**: IAM クレデンシャルエクスポージャー
* **リソース**: IAM
* **指標**: サイバー脅威インテリジェンス、第三者通知
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

このツールは、お客様の環境内の現在のセキュリティ状態のスナップショットを迅速に提供します。 代わりに、[AWS Security Hub] (https://aws.amazon.com/security-hub/?aws-security-hub-blogs.sort-by=item.additionalFields.createdDate&aws-security-hub-blogs.sort-order=desc) はコンプライアンススキャンを自動化し、[Prowler と統合] (https://github.com/toniblyx/prowler/blob/b0fd6ce60f815d99bb8461bb67c6d91b6607ae63/README.md#security-hub-integration)

### アセットインベントリ
既存のユーザーをすべて特定し、各アカウントの目的のリストを更新する

1. [AWS コンソール] (https://console.aws.amazon.com/) に移動します
1. 「サービス」に移動し、「IAM」を選択します。
1. 左側のメニューで、[資格情報レポート] を選択します。
1. 「レポートをダウンロード」を選択します。
1. アカウントが作成された日時、最後に使用されたパスワード、およびパスワードの最終変更列に特に注意してください。
* **注意** ユーザーが複数のアクセスキーを持っている場合は、それぞれの目的を検証してください

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

### Root アカウントアクセス
root アカウントに関連付けられているすべてのアクセスキーを削除することをお勧めします:`. /prowler-c check_112`

### 新規または認識されない IAM ユーザー
[アセットインベントリ] (. /commromised_iam_credentials.md/ #asset-inventory)
IAM ユーザーに 2 つのアクティブなアクセスキー (`) があるかどうかを確認します。 /prowler-c check_extra712`
完全な\ "*: *\」管理者権限を許可する IAM ポリシーが作成されていないことを確認します:`。 /prowler-c check_122`
IAM アクセスアナライザーが有効になっていて、その結果:`. /prowler-c check_extra769`

### 認識されないリソースまたは権限のないリソース (EC2、Lambda など)
aws ec2 describe-instances
aws lambda list-functions

### 秘密を探して
EC2 インスタンスのユーザーデータで見つかった潜在的な秘密:`。 /prowler-c check_extra741`
Lambda 関数変数で見つかった潜在的な秘密:`. /prowler-c check_extra759`
ECS タスク定義変数で見つかった潜在的な秘密:`. /prowler-c check_extra768`
自動スケーリング設定で見つかった潜在的な秘密:`. /prowler-c check_extra775`

### 異例の課金が増える
AWS の請求書を表示するには、[請求とコスト管理] コンソールの [Bills] (https://console.aws.amazon.com/billing/home #) ペインを開き、ドロップダウンメニューから表示する月を選択します。

AWS の支払いの履歴は、請求とコスト管理コンソールの [注文と請求書] (https://console.aws.amazon.com/billing/home?/paymenthistory) ペインで確認できます。

動的モニタリングとアラートには [AWS Cost Anomaly Detection] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-ad.html) を使用することもできます。

請求書の次の点を確認してください。
* 通常使用しない AWS サービス
* 通常使用しない AWS リージョンのリソース
* お札の大きさが大幅に変更されました

この情報を使用して、保持したくないリソースを削除または終了することができます。

### AWS リソースまたはアカウントが侵害される可能性があるという通知
AWS からアカウントに関する通知を受け取った場合は、AWS サポートセンターにサインインし、通知に応答します。

アカウントにサインインできない場合は、[お問い合わせ] フォームを使用して AWS サポートにヘルプをリクエストしてください。

質問や懸念がある場合は、AWS サポートセンターで新しい AWS サポートケースを作成してください。
* **注意**: AWS アクセスキー、パスワード、クレジットカード情報などの機密情報をやり取りに含めないでください。
AWS Git プロジェクトを使用して不正使用の証拠をスキャンする

### 不正の可能性がある IAM ユーザー認証情報を特定する
1. IAM コンソールを開きます。
1. ナビゲーションペインで [Users] を選択します。
1. リストから各 IAM ユーザーを選択し、[アクセス権限ポリシー] で AWSExposedCredentialPolicy_DO_NOT_REMOVE という名前のポリシーを確認します。 ユーザーがこのポリシーをアタッチしている場合は、そのユーザーのアクセスキーをローテーションする必要があります。

## エスカレーション手順
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 分析
セキュリティインシデントイベント管理（SIEM）ソリューション（Splunk、ELK stack など）にログをエクスポートして、より完全な攻撃タイムライン分析のためにさまざまなログを表示および分析することを強く推奨します。

### CloudTrail
異常なログインアクティビティを探す
1. [CloudTrail ダッシュボード] (https://console.aws.amazon.com/cloudtrail) に移動します
1. 左側の余白で [イベント履歴] を選択します。
1. ドロップダウンで「読み取り専用」から「イベント名」に変更します。
1. 検索フィールドに「ConsoleLogin」または「AssumeRole」または「GetFederationToken」または「GetCredentialReport」または「generateCredentialReport」と入力し、疑わしいアクティビティがないか利用可能なイベントを確認します。
* **注** userIdentity は `"type」: rootの場合は「Root" `、または`" type」:「iamUser"` と表示されます

疑わしい EC2 インスタンスの起動に使用された IAM アクセスキー ID とユーザー名を見つけます。
1. CloudTrail コンソールを開き、[イベント履歴] を選択します。
1. [フィルタ] ドロップダウンメニューを選択し、[リソース名] を選択します。
1. [リソース名の入力] フィールドに EC2 インスタンス ID を貼り付け、デバイスで [Enter] を選択します。
1. RunInstances のイベント名を展開します。
1. AWS アクセスキーをコピーし、ユーザー名をメモします。

侵害されたアクセスキーによるアクティビティの CloudTrail イベント履歴を確認する
1. CloudTrail コンソールを開き、ナビゲーションペインから [イベント履歴] を選択します。
1. [フィルタ] ドロップダウンメニューを選択し、[AWS アクセスキーフィルター] を選択します。
1. [AWS アクセスキーの入力] フィールドに、侵害された IAM アクセスキー ID を入力します。
1. API 呼び出しの RunInstances のイベント名を展開します。
* **注**: S3 バケットに保存する証跡を以前に設定していない限り、過去 90 日間のイベント履歴のみ表示できます。

### VPC Flow Logs
VPC Flow Logs は、VPC 内のネットワークインターフェイスとの間で送受信される IP トラフィックに関する情報をキャプチャできる機能です。 これは、CloudTrail 内で検出された IP アドレスがパブリックリソースへの外部接続のタイプを判断する場合に便利です。

Athena でのクエリを含む詳細な情報と手順については、[VPC Flow Logs 用の AWS ドキュメント] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs-athena.html) を参照してください。 Athena 分析は別のプレイブックに含まれ、他の関連項目にリンクすることをお勧めします。

### エンドポイント/ホストベース
* **特定のインスタンス名の最終作成時刻を特定**: `aws ec2 describe-instances —query '予約 [] .Instances []。 {ip: publicipAddress, tm: LaunchTime} '—フィルタ '名前=タグ:名前, 値= myInstanceName' | jq 'sort_by (.tm) | 逆 |. [0] '`

* **ラムダ関数の最終更新時刻を特定**: `aws lambda list-functions | grep「lastModified" `

## 封じ込め
1. IAM ユーザーを無効にし、バックアップ IAM アクセスキーを作成し、侵害されたアクセスキーを無効にします。
1. [IAM コンソール] (https://console.aws.amazon.com/iam/) を開き、IAM アクセスキー ID を検索 IAM バーに貼り付けます。
1. ユーザー名を選択し、[セキュリティ認証情報] タブを選択します。
1. [コンソールパスワード] で、[管理] を選択します。
* **注**: AWS マネジメントコンソールのパスワードが [無効] に設定されている場合は、この手順をスキップできます。
1. コンソールアクセスで、[無効] を選択し、[適用] を選択します。
1. 重要:アカウントが無効になっているユーザーは AWS マネジメントコンソールにアクセスできません。 ただし、ユーザーがアクティブなアクセスキーを持っている場合でも、API 呼び出しを使用して AWS サービスにアクセスできます。
1. 侵害された IAM アクセスキーについては、[非アクティブにする] を選択します。
1. アプリケーション IAM キーを無効にし、バックアップ IAM アクセスキーを作成し、侵害されたアクセスキーを無効にします。
1. まず、2 番目のキーを作成します。 次に、新しいキーを使用するようにアプリケーションを変更します。
1. 最初のキーを無効にする (ただし、削除しない)。
1. アプリケーションに何か問題がある場合は、キーを一時的に再度アクティブ化してください。 アプリケーションが完全に機能し、最初のキーが無効状態になったら、最初のキーを削除します。
1. 非アクティブ/侵害された AWS アクセスキーをすべてローテーションして削除する
* **！！ CloudTrail で引き続き検索できるように、削除されたアクセスキーをすべて記録しておいてください。 **

## 撲滅
### [侵害されたアクセスキーによるアクティビティの CloudTrail イベント履歴の確認] (. /#cloudtrail)
侵害されたキーによって作成されたリソースをすべて削除します。 AWS リソースを起動したことがないリージョンでも、すべての AWS リージョンを確認してください。
* **重要**: 調査のためにリソースを保持する必要がある場合は、それらをバックアップすることを検討してください。 たとえば、EC2 インスタンスを保持する必要がある規制、コンプライアンス、または法的なニーズがある場合は、インスタンスを終了する前に EBS スナップショットを作成します。

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

作成していない IAM ユーザーを削除する
1. AWS マネジメントコンソールにサインインし、[IAM コンソール] (https://console.aws.amazon.com/iam/) を開きます。
1. ナビゲーションペインで、[Users] を選択し、名前や行自体ではなく、削除するユーザー名の横にあるチェックボックスをオンにします。
1. ページの上部で、[ユーザーの削除] を選択します。
1. 確認ダイアログボックスで、最後にアクセスした情報が読み込まれるのを待ってから、データを確認します。 ダイアログボックスには、選択した各ユーザーが AWS サービスに最後にアクセスした日時が表示されます。 過去 30 日以内にアクティブであったユーザーを削除しようとすると、追加のチェックボックスをオンにして、アクティブなユーザーを削除することを確認する必要があります。 続行する場合は、[はい、削除] を選択します。

他のサービスの削除に関するドキュメントは、[すべてのリソースを終了する] (https://aws.amazon.com/premiumsupport/knowledge-center/terminate-resources-account-closure/) ページにあります。

## リカバリ
AWS には [AWS アカウントでの不正なアクティビティに気付いた場合はどうすればよいですか？] のガイドが公開されています。 (https://aws.amazon.com/premiumsupport/knowledge-center/potential-account-compromise/)

## 予防措置
### Prowler IAM スキャン
`。 /prowler-g check122、check111、check110、check19、check18、check17、check16、check15、check11、check116、check12、check114、check115、check13、check112、check119、extra71、extra7100、extra7123、extra7125、extra769、extra774`

### MFA を有効にする
セキュリティを強化するには、AWS リソースを保護するために MFA を設定することがベストプラクティスです。 [IAM ユーザーの MFA] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) または [AWS アカウントのルートユーザー] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa_enable_virtual.html) を有効にできます。 root ユーザーの MFA を有効にすると、root ユーザーのクレデンシャルにのみ影響します。 アカウント内の IAM ユーザーは、独自の認証情報を持つ個別のアイデンティティであり、各 ID には独自の MFA 設定があります。

### アカウント情報を確認する
AWS では、お客様に連絡し、アカウントの問題を解決するために、正確なアカウント情報が必要です。 アカウントの情報が正しいことを確認してください。
* アカウント名とメールアドレス。
* あなたの連絡先情報、特にあなたの電話番号。
* アカウントの代替連絡先。

### AWS Git プロジェクトを使用して不正使用の証拠をスキャンする
AWS では、アカウントを保護するためにインストールできる Git プロジェクトを提供しています。
* [Git Secrets] (https://github.com/awslabs/git-secrets) では、マージ、コミット、コミットメッセージをスキャンして、秘密情報 (つまりアクセスキー) を調べることができます。 Git Secrets は禁止されている正規表現を検出すると、それらのコミットがパブリックリポジトリに投稿されないように拒否できます。
* [AWS ステップ関数と AWS Lambda を使用して Amazon CloudWatch イベントを生成する] (https://aws.amazon.com/step-functions) を AWS ヘルスから、または AWS トラステッドアドバイザーで生成する。 アクセスキーが公開されているという証拠がある場合、プロジェクトは自動的にイベントを検出し、ログに記録し、軽減するのに役立ちます。

### 日常的な操作に root ユーザーを使用しないでください
AWS アカウントのルートユーザーのアクセスキーは、請求情報を含むすべての AWS リソースへの完全なアクセスを提供します。 AWS アカウントのルートユーザーアクセスキーに関連付けられているアクセス許可を減らすことはできません。 絶対に必要な場合を除き、root ユーザーアクセスを使用しないことがベストプラクティスです。

AWS アカウントのルートユーザーのアクセスキーをまだ持っていない場合は、絶対に必要な場合を除き、アクセスキーを作成しないでください。 代わりに、管理者権限を持つ自分の IAM ユーザーを作成します。 AWS アカウントの E メールアドレスとパスワードを使用して AWS マネジメントコンソールにサインインして IAM ユーザーを作成できます。

### 全体的なセキュリティポスチャ
環境に対して [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) を実行して、この Playbook では特定されない他のリスクおよび潜在的に他のパブリックエクスポージャーをさらに特定します。

## 学んだ教訓
「これは、必ず「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要な、貴社固有のアイテムを追加する場所です。 `

## アドレス指定バックログ項目
-インシデントレスポンダーとして、インシデント後のセッショントークンとアクセスキーの処理方法に関するプレイブックが必要です
-インシデントレスポンダーとして、パブリックコードリポジトリからキーをスキャンして削除する方法が必要です
-インシデントレスポンダーとして、いつ環境を壊すか継続的な露出を許可するかについて、明確な意思決定者が必要です
## 現在のバックログアイテム