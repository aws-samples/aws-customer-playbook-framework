# インシデントレスポンス Playbook: S3 の身代金応答
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2021 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

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
1. [**準備**] AWS Config を使用して設定のコンプライアンスを表示する
2. [**準備**] エスカレーション手順の特定、文書化、およびテストを行う
3. [**検出と分析**] 検出を実行し、CloudTrail が認識されない API について分析する
4. [**リカバリ**] 必要に応じてリカバリ手順を実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: 身代金とデータ破壊
* **カテゴリー**: 身代金攻撃
* **リソース**: S3
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス
* **ログソース**: S3 サーバーログ、S3 Access Logs、CloudTrail、CloudWatch、AWS Config
* **チーム**: セキュリティオペレーションセンター (SOC)、フォレンジック調査官、クラウドエンジニアリング

## インシデント処理プロセス
### インシデント対応プロセスには、次の段階があります。
* 準備
* 検出と分析
* 封じ込めと根絶
* 回復
* インシデント後の活動

## エグゼクティブサマリー
このプレイブックでは、AWS シンプルストレージサービス（S3）に対する身代金攻撃に対する応答のプロセスの概要を説明します。

詳細については、[AWS セキュリティインシデント対応ガイド] (https://docs.aws.amazon.com/whitepapers/latest/aws-security-incident-response-guide/welcome.html) を参照してください。

## 準備
* セキュリティ体制を評価して、セキュリティギャップを特定して修正する
* AWS は新しいオープンソース [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント。
* サーバー、ネットワークデバイス、ネットワーク/ファイル共有、開発者マシンなど、すべてのリソースの完全な資産インベントリを維持する
* AWS リソースの設定を評価、監査、評価できるサービスである [AWS Config] (https://aws.amazon.com/config/) の使用を検討してください。
* Amazon S3 に保存されている AWS アカウント、ワークロード、データを保護するために [AWS GuardDuty] (https://aws.amazon.com/guardduty/) を実装して、悪意のあるアクティビティや不正な動作を継続的に監視することを検討する
* Darkbit は、オブジェクトの不正なコピーを識別するために使用できる [AWS S3 データ損失防止] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) の例を提供します。 ビジネスユースケースに基づいて、他の特定のオペレーションをパターンに追加することもできます。
* IAM ロールを使用してアクセス権限を管理する
* 最小権限を実装し、s3を許可しない:* アクセス許可
* アカウントの有効期限と必須の資格情報のローテーションを含む [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) を実装する
* 多要素認証 (MFA) を強制する
* パスワードの複雑さの要件を強制し、有効期限を設定する
* [IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) を実行して、アカウント内のすべてのユーザーと、パスワード、アクセスキー、MFA デバイスなどのさまざまな認証情報のステータスを一覧表示します。
* [AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) を使用して、外部エンティティと共有される Amazon S3 バケットや IAM ロールなど、組織内のリソースとアカウントを特定します。 これにより、セキュリティ上のリスクであるリソースとデータへの意図しないアクセスを特定できます。
* [S3 Object Versioning を有効にする] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) 変更したオブジェクトの復元を許可する
* 保持するバージョンの数を設定するには、ライフサイクルポリシー (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) を設定して最新でないバージョンに適用します。
* [S3 MFA delete を有効にする] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) 攻撃者がバージョニングを無効にし、バケット内のすべてのオブジェクトを上書きすることを防ぐ
* 書き込み1回読んだ多数 (WORM) モデルを使用してオブジェクトを格納できるように、[S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) の使用を検討してください。 オブジェクトロックは、一定時間または無期限にオブジェクトが削除または上書きされるのを防ぐのに役立ちます。
* オブジェクトロックコンプライアンスモード* では、AWS アカウントのルートユーザーを含め、保護されたオブジェクトのバージョンをどのユーザーでも上書きまたは削除することはできません。 オブジェクトがコンプライアンスモードでロックされている場合、その保持モードは変更できず、保持期間を短縮することはできません。 コンプライアンス・モードでは、保持期間中はオブジェクト・バージョンを上書きまたは削除できないことが保証されます。
* オブジェクトのバックアップとコストの最適化に [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) を使用することを検討する
* バケットポリシーを使用し、定期的に監査する
* 必要なものだけを公開し、すべてのオブジェクトがプライベートであることによって保護されるようにする
* [AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) キーを使用して、すべてのオブジェクトを暗号化し、攻撃者が暗号化キーを適用するのを防ぐことを検討する
* [AWS S3 ブロックパブリックアクセス機能] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) を使用して、オブジェクトの意図しない露出を軽減することを検討する
* 機密情報または重要な情報を含む [S3 バケットおよびオブジェクトの CloudTrail イベントログ] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) を有効にします。 デフォルトでは、CloudTrail 証跡はデータイベントをログに記録しませんが、指定した S3 バケットのデータイベントをログに記録したり、AWS アカウント内のすべての Amazon S3 バケットのデータイベントをログに記録するように証跡を設定できます。
* [S3 バケットの CloudTrail サーバーレベルのログ記録] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) と、機密情報または重要な情報を含むオブジェクトを有効にします。 サーバーアクセスログは、バケットに対して行われたリクエストの詳細なレコードを提供します。
* ミッションクリティカルなデータのアップロードについては、[レプリケーションの有効化] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html)-同じリージョンまたはクロスリージョンを検討してください。 クロスリージョンレプリケーションは、別のリージョンに2番目のコピーを保持することにより、アプリケーションの欠陥やオペレータのエラーからデータを保護します。
* [新しい認証情報が公開されないようにする] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
* 特定のサードパーティソリューションをお勧めすることはできませんが、Veritas や CommVault などのパートナー製品や、S3 内のオブジェクトを S3 の内部または外部の管理対象バックアップストレージの場所にバックアップするネイティブ機能を持つ他のバックアップソフトウェアプロバイダからの製品もあります。

### AWS Config を使用して設定のコンプライアンスを表示します。
1. AWS マネジメントコンソールにサインインし、https://console.aws.amazon.com/config/ で AWS Config コンソールを開きます。
1. AWS マネジメントコンソールメニューで、リージョンセレクターが AWS Config ルールをサポートするリージョンに設定されていることを確認します。 サポートされているリージョンのリストについては、Amazon ウェブサービス全般のリファレンスの「AWS Config リージョンとエンドポイント」を参照してください。
1. ナビゲーションペインで、[リソース] を選択します。 [リソースインベントリ] ページでは、リソースカテゴリ、リソースタイプ、およびコンプライアンスステータスでフィルタリングできます。 必要に応じて、[削除されたリソースを含める] を選択します。 テーブルには、リソースタイプのリソース識別子と、そのリソースのリソースコンプライアンスステータスが表示されます。 リソース識別子は、リソース ID またはリソース名です。
1. [リソース識別子] 列からリソースを選択します。
1. [リソースタイムライン] ボタンを選択します。 設定イベント、コンプライアンスイベント、または CloudTrail イベントでフィルタリングできます。
1. 具体的には、次のイベントに焦点を当てます。
* s3-account-level-public-access-blocks
* s3-account-level-public-access-blocks-periodic
* s3-bucket-blacklisted-actions-prohibited
* s3-bucket-default-lock-enabled
* s3-bucket-level-public-access-prohibited
* s3-bucket-logging-enabled
* s3-bucket-policy-grantee-check
* s3-bucket-policy-not-more-permissive e
* s3-bucket-public-read-prohibited
* s3-bucket-public-write-prohibited
* s3-bucket-replication-enabled
* s3-bucket-server-side-encryption-enabled
* s3-bucket-ssl-requests-only
* s3-bucket-versioning-enabled
* s3-default-encryption-kms

## エスカレーション手順
-「SS3 フォレンジックをいつ実施すべきかについてビジネス上の決定が必要です`
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを依頼するのはいつですか？ `

## 検出と分析
* S3 オブジェクトが削除されるか、S3 バケット全体が削除されます
* 注：データ破壊イベントでは、身代金メモが提供される場合と提供されない場合があります。 また、CloudWatch メトリクスと CloudTrail S3 イベントをチェックして、データ漏洩が発生したかどうかを検証して、身代金攻撃またはデータ破壊攻撃の間を明確にするかどうかを確認してください。
* S3 オブジェクトは、お客様が所有していないアカウントのキーを使用して暗号化されます
* 身代金メモは、バケット内のオブジェクトとして、または顧客への電子メールで提供
* CloudTrail ログで、許可されていない IAM ユーザー、ポリシー、ロール、一時的なセキュリティ認証情報の作成など、認可されていないアクティビティがないかどうかを確認します。
* 認識されない API 呼び出しについては CloudTrail を確認します。 具体的には、次のイベントを探します。
* DeleteBucket
* DeleteBucketCors
* DeleteBucketEncryption
* DeleteBucketLifecycle
* DeleteBucketPolicy
* DeleteBucketReplication
* DeleteBucketTagging
* DeleteBucketPublicAccessBlock
* S3 サーバーアクセスログが有効になっている場合は、同じリモート IP とリクエスタから、高いシーケンシャル `REST.COPY.OBJECT_GET `を探します。
* CloudTrail ログをチェックして、不正な EC2 インスタンス、Lambda 関数、EC2 スポット入札など、不正な AWS の使用状況について AWS アカウントを確認します。 AWS マネジメントコンソールにログインし、各サービスページを確認することで、使用状況を確認することもできます。 請求コンソールの [請求書] ページでは、予期しない使用状況を確認することもできます。
* 許可されていない使用はどのリージョンでも発生することがあり、コンソールには一度に 1 つのリージョンしか表示されないことに注意してください。 リージョンを切り替えるには、コンソール画面の右上隅にあるドロップダウンを使用します。

## リカバリ
* 身代金を払わないことをお勧めします
* 身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです
* データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を比較検討する必要があります。
* 身代金を支払うことを選択した場合、攻撃者が自分の会社や他のユーザーに対して操作を継続できるように直接許可します
* https://www.nomoreransom.org/ にアクセスして、データが感染したマルウェアバリアントに対して復号化が可能かどうかを特定する
* [IAM ユーザーキーを削除またはローテーションする] (https://console.aws.amazon.com/iam/home#users) と [Root ユーザーキー] (https://console.aws.amazon.com/iam/home#security_credential)。公開されている特定のキーを特定できない場合、アカウント内のすべてのキーをローテーションすることもできます。
* [許可されていない IAM ユーザーを削除する] (https://console.aws.amazon.com/iam/home#users。)
* [不正なポリシーを削除する] (https://console.aws.amazon.com/iam/home#/policies)
* [権限のない役割を削除する] (https://console.aws.amazon.com/iam/home#/roles)
* [一時的な資格情報を取り消す] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 一時的な認証情報は、IAM ユーザーを削除することで取り消すこともできます。 注:IAM ユーザーの削除は実稼働ワークロードに影響を与える可能性があるため、注意して行う必要があります。* CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
* 代替データバックアップ戦略を使用する場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前にスケジュールされた最後のイベントから復元します。
* データまたはオブジェクトの復元を試みる前に [保護] で手順を実装する
* [削除マーカーを削除する] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) バージョン対応オブジェクトの場合
* 削除されたバケットを再作成する
* [S3 Intelligent Tiering を使用してオブジェクトを復元する] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) オブジェクトのバックアップまたはレプリケートされたリージョンバケット
* 注:現在 S3 には「元に戻す」機能はありません。AWS には削除されたデータを復元する機能はありません。 GDPR (https://gdpr-info.eu/) や CCPA (https://oag.ca.gov/privacy/ccpa) などのデータストレージコンプライアンスと規制の現在の時代において、Amazon S3 はお客様のアカウントから明示的に削除された顧客データを引き続き保存することはできません。 オブジェクトが一度削除されると、意図しない削除が AWS に報告される速さに関係なく、AWS によって復元できなくなります。

## 学んだ教訓
「これは、必ず「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要な、貴社固有のアイテムを追加する場所です。 `

## アドレスバックログ項目
-インシデントレスポンダーとして S3 フォレンジックを実施するには Runbook が必要です
-インシデントレスポンダーとして、SS3 フォレンジックをいつ実施すべきかについてのビジネス上の決定が必要です
-インシデントレスポンダーとして、使用の意図に関係なく、有効になっているすべてのリージョンでログを有効にする必要があります

## 現在のバックログアイテム