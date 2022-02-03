# インシデントレスポンス Playbook: 単純な電子メールサービスイベントへの対応

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

### 目標
Playbook の実行全体を通して、インシデント対応機能の強化についてメモを取って、_***望ましい結果***_ に焦点を当てます。

#### 決定する:
* **脆弱性が悪用された**
* **エクスプロイトとツールが観察された**
* **俳優の意図**
* **俳優の帰属**
* **環境とビジネスに与えたダメージ**

#### 回復:
* **オリジナルで強化された構成に戻す**

#### CAF セキュリティパースペクティブの強化コンポーネント:
[AWS クラウド導入フレームワークセキュリティの視点] (https://d0.awsstatic.com/whitepapers/AWS_CAF_Security_Perspective.pdf)
* **指令**
* **探偵**
* **レスポンシブ**
* **予防**

### レスポンスステップ
1. [**準備**] IAM に AWS GuardDuty 検出を使用する
2. [**準備**] エスカレーション手順の特定、文書化、およびテストを行う
3. [**検出と分析**] 検出を実行し、CloudTrail で認識されない API イベントについて分析する
4. [**検出と分析**] 検出を実行し、CloudWatch で認識されないイベントについて分析する
3. [**封じ込めと撲滅**] IAM ユーザーキーを削除またはローテーションする
4. [**封じ込めと撲滅**] 認識されないリソースを削除またはローテーションする
5. [**リカバリ**] 必要に応じてリカバリ手順を実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

### インシデントの分類と処理
* **戦術、テクニック、手順**:
* **カテゴリー**:
* **リソース**: SES
* **指標**:
* **ログソース**:
* **チーム**: セキュリティオペレーションセンター (SOC)、フォレンジック調査官、クラウドエンジニアリング

## インシデント処理プロセス
### インシデント対応プロセスには、次の段階があります。
* 準備
* 検出と分析
* 封じ込めと根絶
* 回復
* インシデント後の活動

## エグゼクティブサマリー
このプレイブックでは、AWS シンプルな E メールサービス (SES) に対する攻撃に対する応答のプロセスの概要を説明します。 このガイドと組み合わせて、[Amazon SES 送信レビュープロセスに関するよくある質問] (https://docs.aws.amazon.com/ses/latest/DeveloperGuide/faqs-enforcement.html) で、強制措置に対する回答と SES の使用に対する回答を確認してください。

詳細については、[AWS セキュリティインシデント対応ガイド] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html) を参照してください。

## 準備-一般
* セキュリティ体制を評価して、セキュリティギャップを特定して修正する
* AWS は新しいオープンソース [セルフサービスセキュリティ評価] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント。
* サーバー、ネットワークデバイス、ネットワーク/ファイル共有、開発者マシンなど、すべてのリソースの完全な資産インベントリを維持する
* Amazon SES に保存されている AWS アカウント、ワークロード、データを保護するために [AWS GuardDuty] (https://aws.amazon.com/guardduty/) を実装して、悪意のあるアクティビティや不正な動作を継続的に監視することを検討する
* アカウントの有効期限と必須の資格情報のローテーションを含む [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) を実装する
* **多要素認証 (MFA) を強制**
* パスワードの複雑さの要件を強制し、有効期限を設定する
* [IAM 認証情報レポート] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) を実行して、アカウント内のすべてのユーザーと、パスワード、アクセスキー、MFA デバイスなどのさまざまな認証情報のステータスを一覧表示します。
* [AWS IAM アクセスアナライザー] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) を使用して、外部エンティティと共有される IAM ロールなど、組織内のリソースとアカウントを特定します。 これにより、セキュリティ上のリスクであるリソースとデータへの意図しないアクセスを特定できます。

## 準備-SES 固有
* 送信承認ポリシーを使用して、誰がメールを送信できるのか、どこから送信できるかを制御する
* https://docs.aws.amazon.com/ses/latest/dg/sending-authorization.html
* 設定セットを使用して、メッセージの送信を許可する IP アドレスと ID を制御する
* https://docs.aws.amazon.com/ses/latest/dg/using-configuration-sets.html
* Amazon SES に専用 IP アドレスを使用することを検討する
* https://docs.aws.amazon.com/ses/latest/dg/dedicated-ip.html
* Amazon SES でのメール送信と購読の他、メールの抑制のために独自のリストを管理する
* https://docs.aws.amazon.com/ses/latest/dg/lists-and-subscriptions.html
* リアルタイム通知のために Amazon SES イベントパブリッシングをセットアップする
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-using-event-publishing-setup.html
* Amazon SES で最低権限の ID およびアクセス管理を使用する
* https://docs.aws.amazon.com/ses/latest/dg/control-user-access.html
* セキュリティとアクセスに焦点を当て、SES のベストプラクティスを確認します。
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/best-practices.html (クラシック)
* https://docs.aws.amazon.com/ses/latest/DeveloperGuide/control-user-access.html (クラシック)
* 自分のドメインに SPF、DKIM、および DMARC を設定して、フィッシングやなりすましを防止する
* https://docs.aws.amazon.com/ses/latest/dg/email-authentication-methods.html

### AWS GuardDuty 検出の可能性
次の結果は IAM エンティティとアクセスキーに固有であり、常に [Resource Type] が AccessKey になります。 調査結果の重要度と詳細は、発見のタイプによって異なります。 各検索タイプの詳細については、[GuardDuty IAM 検索タイプ] (https://docs.aws.amazon.com/guardduty/latest/ug/guardduty_finding-types-iam.html) ウェブページを参照してください。

* クレデンシャルアクセス:iamuser/anomalousBehavior
* defenseeVasion: iamuser/anomalousBehavior
* Discovery: iamuser/anomalousBehav
* exfiltration: iamuser/anomalousBehav
* インパクト:iamuser/anomalousBehavior
* initialAccess/anomalousBehavior
* pentest: iamuser/Kalilinux
* pentest: iamuser/ParrotLinux
* pentest: iamuser/PentoolLinux
* 永続性:iamuser/anomalousBehavior
* ポリシー:iamuser/rootCredentialUsage
* PrivilegeEscalation: iamuser/anomalousBeh
* recon: iMUSER/maliciousipCaller
* recon: iamuser/maliciousipCaller.Custom
* recon: iamuser/toripCaller
* ステルス:iamuser/CloudTrailloggingDisabled
* ステルス:iamuser/passwordPolicyChange
* UnauthorizedAccess: iamuser/consoleLoginSuccess.B
* UnauthorizedAccess: iamuser/instanceCreentiExfiltation. outsideAWS
* 無許可アクセス:iamuser/maliciousipCaller
* 不正アクセス:iamuser/maliciousipCaller.Custom
* UnauthorizedAccess: iamuser/toripCaller

## エスカレーション手順
-「EC2フォレンジックを実施すべきかについてビジネス上の決定が必要です`
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 検出と分析
### CloudTrail
Amazon SES は AWS CloudTrail と統合されています。AWS CloudTrail は、Amazon SES のユーザー、ロール、または AWS サービスによって実行されるアクションの記録を提供するサービスです。 CloudTrail は、Amazon SES の API 呼び出しをイベントとしてキャプチャします。 キャプチャされた呼び出しには、Amazon SES コンソールからの呼び出しと Amazon SES API オペレーションへのコード呼び出しが含まれます。

SES に関連するアカウントの変更を検索するための特定の CloudTrail `EventName` イベントを次に示します。

* DeleteIDentity
* アイデンティティポリシーの削除
* DeleteReceiptFilter
* deleteReceipTrule
* DeleteReceipTruleSet
* DeleteVerifiedEmailAddress
* getSendQuota
* PutidentityPolicy
* updateReceipTrule
* verifyDomaindKIM
* VerifyDomainID
* メールアドレスの確認
* 確認メールアイデンティティ

[CloudTrail イベント履歴でイベントを表示する] (https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)

### その他の探偵コントロール
* SES イベント（送信、バウンス、苦情など）を記録して監視する
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* 送信者評価メトリックを監視する
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* SES メトリクスの適切な CloudWatch アラームまたはダッシュボードをセットアップする
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## 封じ込めと根絶
* [IAM ユーザーキーの削除またはローテーション] (https://console.aws.amazon.com/iam/home#users) と [ルートユーザーキー] (https://console.aws.amazon.com/iam/home#security_credential)。公開されている特定のキーを特定できない場合、アカウント内のすべてのキーをローテーションすることもできます。
* [許可されていない IAM ユーザーを削除する] (https://console.aws.amazon.com/iam/home#users。)
* [不正なポリシーを削除する] (https://console.aws.amazon.com/iam/home#/policies)
* [権限のない役割を削除する] (https://console.aws.amazon.com/iam/home#/roles)
* [一時的な資格情報を取り消す] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 一時的な認証情報は、IAM ユーザーを削除することで取り消すこともできます。
* 注:IAM ユーザーの削除は実稼働ワークロードに影響を与える可能性があるため、注意して行う必要があります。

## リカバリ
* 最小権限のアクセスポリシーで新しい IAM ユーザーを作成する
*「準備-SES 特定」のセクションにあるステップとリソースを実装する
* SES イベント（送信、バウンス、苦情など）を記録して監視する
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sending-activity.html
* https://aws.amazon.com/blogs/messaging-and-targeting/handling-bounces-and-complaints/
* 送信者評価メトリックを監視する
* https://docs.aws.amazon.com/ses/latest/dg/monitor-sender-reputation.html
* SES メトリクスの適切な CloudWatch アラームまたはダッシュボードをセットアップする
* https://docs.aws.amazon.com/ses/latest/dg/security-monitoring-overview.html

## 学んだ教訓
「これは、必ず「修正」を必要としないが、このプレイブックを運用上およびビジネス上の要件と並行して実行するときに知っておくべき重要なあなたの会社固有のアイテムを追加する場所です。 `

## アドレス指定バックログ項目

## 現在のバックログアイテム