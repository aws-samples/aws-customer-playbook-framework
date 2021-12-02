# AWS 内の身代金攻撃への対応

## 通知

このドキュメントは、情報提供のみを目的として提供されています。 それは表す
Amazon ウェブサービスの現在の製品提供とプラクティス
(AWS) 本文書の発行日現在。
予告なしに変更してください。 お客様は、自分で作る責任があります。
この文書に記載されている情報の独立した評価および使用について
AWS 製品またはサービスのうち、それぞれが「現状のまま」提供されます。
明示または黙示を問わず、あらゆる種類の保証。 このドキュメントはそうではありません
保証、表明、契約上のコミットメントを作成し、
AWS、その関連会社、サプライヤー、または AWS からの条件、または保証
ライセンサー。 AWS の顧客に対する責任と責任
AWS 契約によって管理されており、このドキュメントはの一部でもなく、
AWS とその顧客との間の契約は変更されますか。 \
\
© 2021、Amazon ウェブサービス株式会社またはその関連会社。 すべての権利
予約済み。

## ランサムウェア

1989年に最初に記録されたランサムウェア攻撃がPCサイボーグと呼ばれて以来、
ランサムウェアは犯罪者によって使用される著名な収益化戦略となっている
インターネット上の組織と脅威アクター。 その他の例
ランサムウェアには以下が含まれます。

-CryptoLocker\ [2014\]
-Petya\ [2016\]
-WannaCry\ [2017\]
-NotPetya\ [2017\]
-Ryuk\ [2019\])

脅威アクターは、顧客全体の問題と弱点を活用
インフラストラクチャ、脆弱なエンドポイント、安全でないサービスの悪用、
社会工学的な従業員。

ランサムウェア攻撃は、政府、非営利団体、および企業にコストがかかる
数十億ドルと業務の中断。 NotPetya 強制
巨大なマースクを4,000台のサーバーと45,000台のPCを再インストールするために出荷する
\「重大な業務中断」により300万ドル。 ランサムウェア攻撃
ボルチモア市は\ $18M以上の費用がかかり、地方自治体は
リビエラビーチとフロリダ州レイクシティは、ハッカーに総額1万ドルを支払うことになる
システムとデータを取り戻します。 アメリカ合衆国連邦捜査局
(FBI) は、ランサムウェアの脅威が「よりターゲットを絞られ、
洗練された、そしてコストがかかる」近い将来に。 これらの警告に達する
米国国境を越えて、ユーロポールはランサムウェアを「最も」
広範かつ財政的に損害を与えるサイバー攻撃の形態です。」

## 身代金攻撃って何ですか

身代金攻撃は収益化戦略であり、特定の技術ではありません。
攻撃は、多くの場合、特定の悪意のあるソフトウェア、またはランサムウェアを活用します。
身代金まで被害者情報を公開したり、アクセスをブロックしたりすると脅迫する
が支払われます。

理論的には、身代金が割り当てられた時間内に支払われれば、システムと
データは復号化され、再び利用可能になり、通常の操作
続けて。 ただし、身代金が満たされない場合、組織はリスクを冒します
恒久的な破壊または一般向けデータ漏洩は、
攻撃者。 攻撃者は、顧客をさらに強要して
第三者または一般向けの顧客機密データおよびシステム。

## ランサムウェアは気にしない

多くの身代金攻撃は本質的に日和見的です。つまり、ランサムウェア
人間および/またはを通じてアクセス可能なネットワークに無差別に感染する
マシンベクトル。 教育機関、州、およびセキュリティチーム
地方自治体、および医療機関が対策を強化している
ランサムウェア攻撃の増加からデータを安全に保つこと。 脅威
アクターは、業界業界における弱点を特定する方法を理解しています。 多数
教育や政府組織はランサムウェアの傾向が強い
予算の縮小、セキュリティリソースのギャップ、
パッチが適用されていない既知の脆弱性を持つレガシー IT システム。 同様に、
身代金攻撃は、ダウンタイムに対する不耐性を有する業界を標的とする可能性がある。
病院、支払いの確率を高めることを期待して。

## 身代金攻撃はなぜ効果的ですか

-従業員のセキュリティ意識が低い
-組織がデータをバックアップしていない、または既存のバックアップのテストに失敗する
-攻撃にはほとんどスキルが必要ではなく、大幅な支払いが発生します
-組織は重大な一般的な脆弱性およびエクスポージャー（CVE）へのパッチ適用が遅い
-過度の負担をかけた技術スタッフは、すべてのセキュリティギャップに対処または予測できない
-単一の攻撃で複数のベクトルまたはチャネルが使用されている
-データの盗難（データの漏出、不正コピー）はビジネスプロセスを停止しない可能性があり、情報の暗号化や削除などが行われるまで顧客は反応しない可能性があります。

## 払うか払わないか？

サイバーセキュリティの専門家の間では、
身代金の支払いを決定。 FBIを含む多くの専門家が助言する
身代金を払わない組織は、支払うことはないと主張して
ロックされたシステムまたは暗号化されたデータが利用可能になることを保証する
そして、悪質な行動を動機づけ続けるだけです。 とはいえ
身代金を支払った後、システムとデータへのアクセスは保証されません。
組織は、通常を再開することを期待して、計算されたリスクを負う
オペレーション。 そうすることで、潜在的な補助コストを削減したいと考えています。
生産性の低下を含む攻撃のうち、経時的に収益が減少し、
機密データの暴露、風評被害。 ランサムウェア
脅威は深刻ですが、賢い準備と継続的な警戒は
それに対する効果的なカウンター。 データセキュリティの完全な防具は次のとおりです。
人的統制と技術統制の両方がありますが、AWS の機能があります
ランサムウェア攻撃の軽減に役立つクラウド。 AWS は
High を支援するツール、ベストプラクティス、サービスを提供
上の悪役者に対処するための可用性、セキュリティ、および回復力
インターネット。

## AWS のシステムとデータのセキュリティ保護は [共有責任] (https://aws.amazon.com/compliance/shared-responsibility-model)

AWS クラウドでアプリケーションとインフラストラクチャをデプロイすると、AWS
セキュリティ上の責任をお客様と共有することで役立ちます。 AWS エンジニア
セキュアな設計原則を使用した基盤となるクラウドインフラストラクチャ
お客様は、ワークロード用に独自のセキュリティアーキテクチャを実装する必要があります。
AWS にデプロイされました。 AWS はインフラストラクチャを保護する責任があります
AWS クラウドで提供されるすべてのサービスを実行します。 このインフラ
ハードウェア、ソフトウェア、ネットワーキング、およびファシリティで構成されています。
AWS クラウドサービスを実行する。 お客様が選択する AWS クラウドサービス
お客様が実行する必要がある構成作業の量を決定する
彼らのセキュリティ責任の一部。 顧客はいつも
データを管理および保護し、分類する責任
アセット、および AWS ID アクセス管理 (IAM) を使用して
適切なパーミッション。

## AWS サポート

アカウントが有効な身代金を下回っていると思われる場合
攻撃、影響を受けたお客様は AWS サポートからケースを作成する必要があります
影響を受けるアカウント内から中央に配置します。 root ユーザーになれない場合
アクセスされた、または侵害された場合、お客様は新しい AWS を開く必要があります
アカウントを作成し、そこからサポートチケットを開きます。 このプロセスにより、AWS は
お客様と本人確認を行い、エンゲージメントを開始する
イベントの修復に最も役立つ内部リソース。

## AWS Security Reference Architecture (AWS SRA)

[Amazon ウェブサービス (AWS) セキュリティリファレンスアーキテクチャ (AWS)
SRA)] (https://docs.aws.amazon.com/prescriptive-guidance/latest/security-reference-architecture/welcome.html)
は、AWS の完全な補完をデプロイするための包括的なガイドラインです
マルチアカウント環境のセキュリティサービス。 それは助けるために使うことができます
AWS セキュリティサービスを設計、実装、管理して、それらが連携するように
AWS のベストプラクティスを使用します。

## 米国サイバーセキュリティおよびインフラストラクチャセキュリティ機関

[ランサムウェアガイド (Sept.
2020)] (https://www.cisa.gov/sites/default/files/publications/CISA_MS-ISAC_Ransomware%20Guide_S508C.pdf)
がもたらすリスクの管理に役立つベストプラクティスと参考資料を提供します。
ランサムウェアと組織の協調的かつ効率的なサポート
ランサムウェアインシデントへの対応。

## 欧州連合サイバーセキュリティ機関 (ENISA)

[ENISA 脅威ランドスケープ 2020]-
ランサムウェア] (https://www.enisa.europa.eu/publications/ransomware)
ランサムウェアに関する調査結果の概要を説明し、説明と分析を提供します。
ドメインのうち、関連する最近のインシデントを一覧表示します。 提案された一連の提案
緩和のためのアクションが提供されます。

## オーストラリアサイバーセキュリティセンター (ACSC)

ACSC は、[ランサムウェア] への応答を含むいくつかのガイドを提供しています。
オーストラリア] (https://www.cyber.gov.au/sites/default/files/2020-10/Ransomware%20in%20Australia%20%28October%202020%29.pdf), [ランサムウェア
攻撃緊急対応
ガイド] (https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Emergency-Response-Guide_Accessible_08.12.20.pdf)、
[ランサムウェア攻撃の防止と保護]
ガイド] (https://www.cyber.gov.au/sites/default/files/2021-07/11515_ACSC_Prevention-And-Protection-Guide_Accessible_08.12.20.pdf)。

## NISTサイバーセキュリティフレームワーク

ガイド、NIST サイバーセキュリティフレームワーク (CSF): NIST CSF への調整
AWS クラウドでは、は商業部門および公共部門を支援するように設計されています
世界のあらゆる規模のエンティティは、次のように CSF と整列します。
AWS のサービスとリソースを使用する。

## ランサムウェアの緩和:保護と回復準備アクションの上位 5

AWS は、[ランサムウェアの軽減策] に関するパブリックセキュリティブログを提供しました。
トップ5の保護と回復準備
アクション。] (https://aws.amazon.com/blogs/security/ransomware-mitigation-top-5-protections-and-recovery-preparation-actions/)
これらには以下が含まれます。

-アプリとデータを回復する機能を設定する
-データを暗号化する
-重要なパッチを適用する
-セキュリティ標準に従う
-レスポンスを監視して自動化していることを確認する

エクスポーズされたリモートアクセスにも強力な認証を使用する必要があります。
サービス、特にリモートデスクトッププロトコルとセキュアシェル。 [多要素
認証とシングルとの組み合わせ
サインオン] (https://docs.aws.amazon.com/singlesignon/latest/userguide/mfa-how-to.html)
リモート接続を保護する技術的なメカニズムです。
およびリソースリクエスト。

## 開発、セキュリティ、運用 (DevSecOps)

*背景*: DevSecOps の目的は、お客様を安全に支援することです
顧客 (エンドユーザー) とのフィードバックループを加速します。 By
これらのフィードバックループを加速することで、顧客はより多くのソフトウェアを発送できる
より高い品質、セキュリティ、安定性を備えた生産への変化。 By
開発プロセスにセキュリティを組み込むことで、顧客は防ぐことができます
本番環境のエンドユーザーに脆弱性をデプロイする。
NISTによると、セキュリティ上の欠陥を一度修正するコスト
それを生産にすることは、その間に比べて最大60倍高価になる可能性があります
開発サイクル
\ [[出典] (https://securityboulevard.com/2020/09/the-importance-of-fixing-and-finding-vulnerabilities-in-development/)\]。

AWS カスタマーインシデント対応チームからのデータに基づいて、ほとんど
顧客の身代金攻撃は、1/A 悪い 3 つのカテゴリのいずれかに該当します
アクターは、AWS アクセスキーについてソースコードリポジトリ (GitHub など) をスキャンします。
これらの長期アクセスキーを使用すると、次の AWS リソースにアクセスできます。
顧客のアカウント。 アクセスレベルに基づいてリスクが増加する
アクセスキーで許可されています。2/ パブリック S3 バケットをスキャンして、
オブジェクトの場合、不正なアクターがアクセスをブロックし、S3 バケットを暗号化することができます
所有者がデータにアクセスできない。3/ 悪いアクターがウェブをスキャンして活用
アプリケーションの脆弱性 (コードインジェクションやクロスサイトなど)
scripting）をパブリックポート上で使用して、顧客データへのアクセスを取得します。

AWS がこれらの安全対策の多くを提供しているとお客様は誤って信じています。
AWS アカウントを持つことの一環として自動的に。 AWS が提供している間
デフォルトでプライベート機能を含む何千ものセキュリティ機能
(EC2 セキュリティグループと S3 バケットなど)、[shared] の一部として
責任
モデル] (https://aws.amazon.com/compliance/shared-responsibility-model/)、
お客様は、自社のセキュリティ脆弱性を検出して修復する必要があります。
AWS 環境でのコード化または両方。 AWS のサービスとツールがあります
これらの非準拠リソースを検出して修正するためですが
お客様は、自動でのプラクティスの適用に関する知識を持っている必要があります。
AWS リソース全体に渡ってみましょう。

デプロイパイプラインは、ビルド、テスト、デプロイ、リリースアクションを分割します。
一連のステージに。 各ステージは自信を高め、
通常、余分な時間を犠牲にして。 初期段階ではほとんどの問題が見つかる
より速いフィードバックが得られる一方、後のステージではより遅く、さらに多くの結果が得られます。
徹底的なプロービング。 ビルダーは、これらのアクションの多くを自動化します。
デプロイパイプライン; デプロイメントパイプラインは主要なアーキテクチャです
[連続] の構成
配信] (https://aws.amazon.com/devops/continuous-delivery/)。 ザ・
デプロイパイプラインの仕事は、原因となる変更を検出することです
プロダクションの問題。 これには、パフォーマンス、セキュリティ、または
ユーザビリティの問題
\ [[出典] (https://martinfowler.com/bliki/DeploymentPipeline.html)\]。
この文書の目的のために、建築の実践を参照します。
セキュリティテストとデプロイパイプラインをチェックインして、
継続的なセキュリティとして身代金攻撃を緩和します。

継続的セキュリティを使用して予防する原則
身代金攻撃の軽減は次のとおりです。1/ ソースにシークレットをコミットするのを防ぐ
不正なアクターが AWS にアクセスするためのキーを持たないように、リポジトリをコード化する
リソース。2/ ウェブを作る一般的な開発者のミスを防ぐ
アプリケーションの脆弱性 (静的およびランタイム)。3/
危険にさらされた AWS リソースのバックアップにより、不良アクターに
リソースへのアクセスをブロックすることから多くの利益を得る。4/ すべてを暗号化する
関連するデータ。悪いアクターがデータにアクセスした場合、
恐喝で得ることははるかに少ない。5/ 権限のないユーザーを検出する
AWS リソースにアクセスして、緩和策を実行できるようにします。6/ IAM を確保
アクセスキーは IAM ロールを使用して短命であるため、アクセスは制限されています。
危険にさらされたリソースまでの期間。7/ IAM 権限の使用量を最小限に抑える
悪い俳優なら本当に悪いことが起こらないようにする特権
キーへのアクセスを得ます。8/ 設定ミスを防止または検出する
AWS リソースを脆弱にする。

*ソースコードリポジトリにシークレットをコミットするときの防止と検出***: **
デベロッパー環境 (IDE など) から、お客様は
シークレット検出静的アプリケーションセキュリティテスト (SAST) ツールの実行
ソースコードリポジトリにコードをコミットする前。 さらに、最初の
自動デプロイパイプラインのステージでシークレット検出 SAST を実行する
シークレットがソースコードリポジトリにコミットされたタイミングを検出するツール。
シークレットを検出するときは、パイプラインが失敗するように構成し、通知します
開発者は、修正を適用するまで今後のコミットを停止します。 さらに、
自動化を実行してシークレットの git 履歴をクレンジングし、AWS を無効にする
アクセスキー。 このソリューションのツールの例としては、AWS CodePipeline、AWS があります。
CodeBuild、git-secrets、およびシークレットを修復するためのカスタムコード。

*アプリケーションを作る一般的な開発者のミスを防止し、検出する
脆弱性***: ** 開発者環境 (IDE など) から、
お客様は Web を検出する SAST ツールおよびコードレビューツールを実行する必要があります
コードインジェクション、クロスサイトスクリプティング、その他の脆弱性
安全でないコーディングの実践。 さらに、自動化の第一段階
デプロイパイプラインはこれらのツールを実行して、Web の脆弱性を検出します。
ソースコードリポジトリにコミットされます。 これらのウェブを発見するとき
脆弱性、パイプラインが失敗するように構成し、開発者に通知し、
修正を適用するまで、今後のコミットを停止します。 さらに、顧客はできること
これらの Web 脆弱性のランタイム検出と修復を構成する
およびをプロビジョニングする共有サービス展開パイプラインの一部として
は、これらのサービスをコードとして設定します。 このソリューションのツールの例は次のとおりです。
Amazon CodeGuru、AWS CloudFormation、AWS CodePipeline、AWS CodeBuild、
SonarQube/CheckMarx、および AWS WAF および WAF セキュリティ自動化機能を備えています。

*侵害された AWS リソースのバックアップ:* 実行時に、タグなしを検出する
バックアップがない関連する AWS リソースまたは関連する AWS リソース。 さらに、
お客様は、提供する AWS リソースのプロビジョニングを設定できます。
共有サービス展開パイプラインの一部としてのランタイム検出
これらのサービスをコードとしてプロビジョニングし、構成します。 このためのツールの例
解決策は AWS Backup、AWS CloudFormation、AWS CodePipeline、AWS
CodeBuild、Amazon EventBridge、[AWS Config
ルール] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
（例えば、required-tags、redshift-backup-enabled、
aurora-resources-protected-by-backup-plan、
backup-plan-min-frequency-and-min-retention-check、
backup-recovery-point-manual-deletion-disabled、
DB インスタンスバックアップ対応、dynamodb-in-backup-Plan、
ebs-in-backup-plan), Amazon SNS, and-.

*転送中および保管中のデータの暗号化***: ** 開発者から
環境 (IDE など) の場合は、次のような SAST ツールを実行する必要があります。
コードインジェクションやクロスサイトなどの Web 脆弱性を検出する
スクリプティング。 さらに、自動デプロイメントの第一段階
pipeline は iaC リンティングツールを実行して、開発者がいつ定義したかを検出します。
暗号化されていない AWS リソースをコードとして実行し、設定コードをにコミットする
ソースコードレポ。 これらの暗号化されていないリソースを検出すると、
パイプラインを失敗させ、開発者に通知し、将来停止するように構成する
修正を適用するまでコミットします。 さらに、お客様は
ランタイム検出を提供する AWS リソースのプロビジョニングと
共有の一部として暗号化されていない AWS リソースの修復
これらをプロビジョニングして構成するサービスデプロイパイプライン
コードとしてのサービス。 このソリューションのツール例としては AWS があります。
CloudFormation, AWS CodePipeline, AWS CloudFormation Guard, AWS
CodeBuild、Amazon EventBridge、[AWS Config
ルール] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
（例えば、デフォルトで ec2-ebs-encryption、dynamodb-table-encrypted-kms、
rds-snapshot-encrypted、cloudwatch-log-group-encrypted、
cloud-trail-encryption-enabled、
s3-バケットサーバー側暗号化対応、api-gw-ssl-enabled、
cloudfront-custom-ssl-certificate）、Amazon SNS、AWS KMS、AWS Lambda。

*許可されていないユーザーが AWS リソースにアクセスするタイミングを検出する:* 実行時に、
データの流出を検出する\ "スパイク\」(例:CloudWatch メトリクスの使用)
具体的に
[NetWORKOUT] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html))。
たとえば、攻撃者がデータ破壊を行い、
身代金メモを残し、このような場合、データの機会がない
悪質なアクターと協力して回復する。 さらに、お客様
ランタイムを提供する AWS リソースのプロビジョニングを設定できます
共有サービス展開パイプラインの一部としての検出
これらのサービスをコードとしてプロビジョニングし、構成します。 このためのツールの例
解決策は AWS CloudFormation, AWS CodePipeline, AWS CodeBuild, Amazon
EventBridge、Amazon GuardDuty、Amazon SNS、Amazon CloudWatch
メトリクス。

*IAM アクセスキーの有効期間が短くなっていることを確認***: ** 実行時に、次のツールを実行します。
ローテーションを含む IAM アクセスキーおよびポリシーのさまざまな状態を検出する
長寿命のアクセスキー。 お客様は AWS のプロビジョニングを設定できます。
共有サービスの一部としてランタイム検出を提供するリソース
これらのサービスを次のようにプロビジョニングして構成するデプロイパイプライン
コード。 このソリューションのツールの例としては、AWS CloudFormation、AWS があります。
CodePipeline, AWS CodeBuild, Amazon EventBridge, [AWS Config
ルール] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
（例えば、iam-policy-no-statements-with-admin-access、
iam-policy-no-statements-with-full-access、iam-root-access-key-check、
iam-user-no-policies-check、iam-user-unused-credentials-check、
access-keys-rotated)、および AWS Lambda。

*IAM 権限で最低限の権限を使用していることを確認***: ** 開発者から
環境 (IDE など) の場合は、次のような SAST ツールを実行する必要があります。
コミットする前に過度に許容されるパーミッションの定義を検出する
コードをソースコードレポにまとめます。 さらに、自動化の第一段階
デプロイパイプラインは SAST ツールを実行して、過度に許容範囲を検出します。
コードがソースコードにコミットされた後のポリシー定義
レポ。 過度に許容されるポリシー定義を検出する場合は、
パイプラインが失敗し、開発者に通知し、将来のコミットを停止する
修正を適用する。 さらに、過度に寛容なポリシーを検出
AWS アカウントまたはアカウント（つまり、ソースコードリポジトリの外）。 順番に、
お客様は、提供する AWS リソースのプロビジョニングを設定できます。
共有サービス展開パイプラインの一部としてのランタイム検出
これらのサービスをコードとしてプロビジョニングし、構成します。 このためのツールの例
解決策は AWS CloudFormation、AWS CodePipeline、AWS CodeBuild、
pmapper/awspx, AWS IAM Access Analyzer, Amazon EventBridge, AWS Config
ルール、および AWS Lambda。

*AWS リソースを脆弱にする設定ミスを防ぐ*:
特定の AWS リソースの設定ミス、または有効化しなかった場合
身代金攻撃に対して脆弱なお客様。 最も注目すべきリソースは
ネットワーキング、コンピューティング、ストレージ、およびデータベースに関連するもの。 何ですか
さらに、お客様は次のような AWS リソースのプロビジョニングを設定できます。
共有サービス展開の一部としてランタイム検出を提供する
これらのサービスをコードとしてプロビジョニングおよび構成するパイプライン。 例
このソリューションのツールは、AWS CloudFormation、AWS CodePipeline、AWS です。
CodeBuild、Amazon EventBridge、AWS Config Rules（例：
s3-account-level-public-access-blocks、s3-bucket-default-lock-enabled、
s3-bucket-level-public-access-prohibited、s3-bucket-logging-enabled、
s3-bucket-policy-not-more-permissive、s3-bucket-public-read-prohibited、
s3-bucket-public-write-prohibited、s3-bucket-replication-enabled、
cloud-trail-log-file-validation-enabled、
ec2-セキュリティグループ-eniにアタッチされた、vpc-default-security-group-closed、
vpc-flow-logs-enabled、guardduty-enabled-centralized)、Amazon
インスペクターネットワーク到達可能性、Amazon VPC Reachability Analyzer、Amazon
S3、および AWS Lambda。

## Amazon Elastic Compute Cloud (*Amazon EC2*)-Microsoft Windows

### 識別する
-アカウントのセキュリティ体制を評価し、セキュリティギャップを特定して修正する
-AWS は新しいオープンソース [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント。
-ドメインコントローラー、Microsoft Windows EC2 インスタンス、Microsoft Windows サーバーとデータベース、および外部 ID プロバイダーとの統合など、すべてのリソースの完全な資産インベントリを維持します。
-[Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/) などのユーティリティを使用して、ホストの定期的な脆弱性分析を実行します。
-組織を保護するために、Microsoft は [**人間が運営するランサムウェア緩和プロジェクトプラン**] (https://download.microsoft.com/download/7/5/1/751682ca-5aae-405b-afa0-e4832138e436/RansomwareRecommendations.pptx) PowerPoint プレゼンテーション ([セキュリティ保護]) に記載されている情報を使用することをお勧めします。特権アクセス] (https://aka.ms/spa)。
-[CloudWatch メトリクス] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)、特に NetworkPacketsOut を使用して、データの流出の「スパイク」を探します。 攻撃者がデータ破壊を行い、身代金メモを残した可能性があります。このような場合、悪意のあるアクターと協力してデータ復旧の機会はありません。

### 保護
-オペレーティングシステムとアプリに最新のアップデートを適用する
-お使いの PC の製造元がまだオンになっていない場合は、ファイル履歴でファイルをバックアップします。 [ファイル履歴について詳しく知る] (https://support.microsoft.com/en-us/windows/file-history-in-windows-5de0e203-ebae-05ab-db85-d5aa0a199255)
-重要なファイルを定期的にバックアップします。 3-2-1 ルールを使用します。 データのバックアップを 2 つの異なるストレージタイプに保持し、少なくとも 1 つのバックアップをオフサイトに保存します。
-既知のランサムウェアのファイルタイプをブロックする
-[Office ドキュメント内のマクロをブロックする] (https://support.microsoft.com/en-us/topic/enable-or-disable-macros-in-office-files-12b036fd-d140-4e74-b45e-16fed1a7e5c6)
-ソーシャルエンジニアリングやスピアフィッシング攻撃を特定できるように従業員を教育する
-[Active Directory サービスを保護するためのベストプラクティスに従う] (https://www.calcomsoftware.com/how-to-secure-your-active-directory-when-anonymous-users-must-have-access/)
-[セキュリティ侵害を防止するための最も重要なグループポリシー設定の上位 10 に従う] (https://www.lepide.com/blog/top-10-most-important-group-policy-settings-for-preventing-security-breaches/)
-[制御されたフォルダアクセスの実装] (https://docs.microsoft.com/en-us/microsoft-365/security/defender-endpoint/controlled-folders)。 ランサムウェアがファイルを暗号化し、身代金のためにファイルを保持するのを阻止できます
-EC2 インスタンスのバックアップを実行する
-[AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) または [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/) の使用を検討する
-Microsoft 365 Defender Threat Intelligence Team [包括的なレポート] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) ランサムウェアイベントが発生する前にMicrosoft リソースを保護するためのアクションを特定しました。
-インターネット向けアセットを強化し、最新のセキュリティ更新プログラムがあることを確認してください。 脅威と脆弱性の管理を使用して、脆弱性、構成ミス、および疑わしいアクティビティについて定期的にこれらの資産を監査します。
-ブルートフォースの試みを監視する。 過度の失敗した認証の確認 (Windows セキュリティイベント ID 4625)
-イベントログ、特にセキュリティイベントログと PowerShell 操作ログのクリアを監視します。 Microsoft Defender ATP は「Event log was cleared」というアラートを発生させ、Windows はイベント ID 1102 を生成します。
-最小特権の原則を実践し、資格情報の衛生を維持する。 ドメイン全体の管理者レベルのサービスアカウントの使用は避けてください。 強力なランダム化されたジャストインタイムのローカル管理者パスワードを強制します。 LAPSのようなツールを使う
-Azure 多要素認証 (MFA) などのソリューションを使用して、リモートデスクトップゲートウェイをセキュリティで保護します。 MFA ゲートウェイがない場合は、ネットワークレベル認証 (NLA) を有効にします。
-Office 365 を使用している場合は、Office VBA の AMSI をオンにする
-資格情報の盗難、ランサムウェアのアクティビティ、PsExec とWMI の疑わしい使用をブロックするルールを含む、攻撃面削減ルールを有効にします。 武器化された Office ドキュメントによって開始される悪意のあるアクティビティに対処するには、Office アプリケーションによって開始される高度なマクロアクティビティ、実行可能コンテンツ、プロセスの作成、プロセス注入をブロックするルールを使用します。 これらのルールの影響を評価するには、監査モードで展開します。
-Windows Defender アンチウイルスでクラウド配信保護と自動サンプル送信を有効にします。 これらの機能は、人工知能と機械学習を使用して、新しい脅威や未知の脅威を迅速に特定して阻止します。
-タンパープロテクション機能をオンにして、攻撃者がセキュリティサービスを停止するのを防ぐ
-Windows Defender Firewall とネットワークセキュリティシステムを使用して、可能な限りエンドポイント間の RPC および SMB 通信を防止します。 これにより、横方向の移動やその他の攻撃アクティビティが制限されます。
-Windows Defender Antivirus をオンにして更新する-商用、サブスクリプション有料のエンドポイント検出と応答（EDR）ソリューションが好ましい
-Windows 10で [制御されたフォルダアクセス] (https://support.microsoft.com/en-us/topic/ransomware-protection-in-windows-security-445039d6-537a-488a-ad53-48906f346363) をオンにして、ランサムウェアやその他のマルウェアなどの不正なプログラムから重要なローカルフォルダを保護します。
-[Systems Manager と Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) を使用して、Microsoft Windows を実行している EC2 インスタンスに、一般的な脆弱性およびエクスポージャー (CVE) が含まれているかどうかを確認します。
-バックアップを確認し、感染が蔓延していないことを確認します

### 検出
-Microsoft 365 Defender Threat Intelligence Team が [包括的なレポート] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) を提供し、複数のランサムウェアバリアントに対して探すべきものを特定した
-vpcFlowLogs を使用して、外部 IP アドレスからの不適切なデータベースアクセスを特定する
-[Amazon Detective でVPC フローを調査する] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) または [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### 応答
-ネットワーク IOC に基づいて NACL を適用して、さらなるトラフィックを防止する
-感染したシステムを隔離する
-感染の可能性についてバックアップを検査する
-侵害されたシステムをネットワークから削除します。
-規制要件または社内ポリシーに従って、EC2 インスタンスのフォレンジックが必要かどうかを判断する
-インスタンスのフォレンジックが必要な場合、またはデータを復元する必要がある場合は、[EC2 インスタンスを隔離する] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)
-[侵害されたドメインコントローラーのメタデータをドメインから削除する] (https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/deploy/ad-ds-metadata-cleanup)
-フローログを使用してエクスフィルトレーション IP を特定し、CloudTrail からデータがエクスフィルトされた IP と通信している他のインスタンスを探します。

### リカバリ
-身代金を払わないことをお勧めします
-身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです
-データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を評価する必要があります。
-身代金を支払うことを選択した場合、攻撃者が自分の会社または他のユーザーに対して操作を継続できるようにする
-信頼できる AMI から新しい EC2 インスタンスを作成する
-CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
-代替データバックアップ戦略を使用している場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前に最後にスケジュールされたイベントから復元します。
-< https://www.nomoreransom.org/ > アクセスして、データが感染したマルウェアバリアントで復号化が可能かどうかを特定する

## Amazon Elastic Compute Cloud (*Amazon EC2*)-Unix/Linux

### 識別する
-セキュリティ体制を評価して、セキュリティギャップを特定して修正する
-AWS は、新しいオープンソース [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) ツールを開発しました。このツールは、ポイントインタイム評価をお客様に提供し、AWS アカウント。
-サーバー、ネットワークデバイス、ネットワーク/ファイル共有、開発者マシンなど、すべてのリソースの完全な資産インベントリを維持する
-[Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-visualize-multi-account-amazon-inspector-findings-with-amazon-elasticsearch-service/) などのユーティリティを使用して、ホストの定期的な脆弱性分析を実行します。
-[CloudWatch メトリクス] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/viewing_metrics_with_cloudwatch.html)、特に NetWORUT を使用して、データの流出の「スパイク」を探します。 攻撃者がデータ破壊を行い、身代金メモを残した可能性があります。このような場合、悪意のあるアクターと協力してデータ復旧の機会はありません。

### 保護
-オペレーティングシステムとアプリに最新のアップデートを適用する
-重要なファイルを定期的にバックアップします。 3-2-1 ルールを使用します。 データのバックアップを 2 つの異なるストレージタイプに保持し、少なくとも 1 つのバックアップをオフサイトに保存します。
-既知のランサムウェアのファイルタイプをブロックする
-AWS Config を使用して、AWS サービスの変更を監視するルールを作成することを検討してください。 AWS Config には、EBS と EC2 をモニタリングするために、次のようないくつかのマネージドルールが事前に構築されています。
-[ebs-in-backup-Plan] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-in-backup-plan.html)
-[ebs-optimized-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-optimized-instance.html)
-[ebs-snapshot-public-restorable-check] (https://docs.aws.amazon.com/config/latest/developerguide/ebs-snapshot-public-restorable-check.html)
-[ec2-ebs-encryption-by-default] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-ebs-encryption-by-default.html)
-[ec2-imdsv2-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-imdsv2-check.html)
-[ec2-instance-detailed-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-detailed-monitoring-enabled.html)
-[ec2-instance-managed-by-systems-manager] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-managed-by-systems-manager.html)
-[ec2-instance-multiple-eni-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-multiple-eni-check.html)
-[ec2-instance-no-public-ip] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-no-public-ip.html)
-[ec2-instance-profile-attached] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-instance-profile-attached.html)
-[ec2-managedinstance-applications-blacklisted] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-blacklisted.html)
-[ec2-managedinstance-applications-required] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-applications-required.html)
-[ec2-managedinstance-association-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-association-compliance-status-check.html)
-[ec2-managedinstance-inventory-blacklisted ed] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-inventory-blacklisted.html)
-[ec2-managedinstance-patch-compliance-status-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-patch-compliance-status-check.html)
-[ec2-managedinstance-platform-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-managedinstance-platform-check.html)
-[ec2-security-group-attached-to-eni] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-security-group-attached-to-eni.html)
-[ec2-stopped-instance] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-stopped-instance.html)
-[ec2-volume-inuse-check] (https://docs.aws.amazon.com/config/latest/developerguide/ec2-volume-inuse-check.html)
-ソーシャルエンジニアリングやスピアフィッシング攻撃を特定できるように従業員を教育する
-すべてのシステムでマルウェア検出とアンチウイルスを有効にする-商用、サブスクリプション有料のエンドポイント検出と応答（EDR）ソリューションが推奨されます。
-サンプルガイドは、[ClamAVをアンチウイルスエンジンとしてLinuxマルウェア検出 (LMD) をインストールして使用する方法] (https://www.tecmint.com/install-linux-malware-detect-lmd-in-rhel-centos-and-fedora/) を参照してください。
-EC2 インスタンスのバックアップを実行する
-[AWS Backup] (https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html) または [AWS CloudEndure] (https://aws.amazon.com/cloudendure-disaster-recovery/) の使用を検討する
-Microsoft 365 Defender Threat Intelligence Team が [包括的なレポート] (https://www.microsoft.com/security/blog/2020/03/05/human-operated-ransomware-attacks-a-preventable-disaster/) を提供し、ランサムウェアイベントが発生する前にMicrosoft リソースを保護するためのアクションを特定しました。 これらの推奨事項の多くは、Unix および Linux に対応できます。
-特権の高いアカウントがログオンし、認証情報を公開している場所を特定します。
-インターネット向けアセットを強化し、最新のセキュリティ更新プログラムがあることを確認してください。 脅威と脆弱性の管理を使用して、脆弱性、構成ミス、および疑わしいアクティビティについて定期的にこれらの資産を監査します。
-ブルートフォースの試みを監視する。 過度に失敗した認証試行のチェック (/var/log/auth.log または /var/log/secure) /var/log/auth.log
-イベントログのクリアを監視する
-最小特権の原則を実践し、資格情報の衛生を維持する。 ドメイン全体の管理者レベルのサービスアカウントの使用は避けてください。 強力なランダム化されたジャストインタイムのローカル管理者パスワードを強制します。
-タンパープロテクション機能をオンにして、攻撃者がセキュリティサービスを停止するのを防ぐ
-[Wazuh] (https://documentation.wazuh.com/current/getting-started/components/index.html) などのエンドポイントセキュリティエージェントを使用する
-[Tripwire] (https://github.com/Tripwire/tripwire-open-source) などのファイル整合性モニタを使用して、重要なファイルの変更を検出する
-[Systems Manager と Amazon Inspector] (https://aws.amazon.com/blogs/security/how-to-patch-inspect-and-protect-microsoft-windows-workloads-on-aws-part-1/) を使用して、EC2 インスタンスに一般的な脆弱性およびエクスポージャー (CVE) が含まれているかどうかを確認します。
-バックアップを確認し、感染が蔓延していないことを確認します

### 検出
-リチャード・ホーン著 [No Strings on Me: Linux and Ransomware] (https://www.sans.org/reading-room/whitepapers/tools/strings-me-linux-ransomware-39870) の表1は、以下を含むために監視する複数の指標を特定しています。
-外部ネットワーク接続要求がある以前にエンドポイントで見られたことがない新しいプロセス
-エントロピーが見つかったDNS名または裸のIPで直接通信を試みた
-ディストリビューション Yum または Apt リポジトリの変更
-ユーザーベース以外のホームディレクトリ内に.sh ファイルを作成する
-共有 Web、データベース、またはファイルストレージディレクトリツリーの列挙
-特権のエスカレーションまたは sudo アクセスの試み
-外部および内部ネットワーク通信
-エントロピーで生成されたファイル名、または連続するファイル名
-同じディレクトリツリー内で複数回名前が変更されるファイル
-oddファイル拡張子で作成されたファイル
-通常の日常業務の外れ値やイベントにフォーカスを当てる
-短い期間または持続的に高いメモリ使用量
-子プロセスの前に親プロセスが終了する大きなプロセストレス
-ブートオプションの変更
-複数のディレクトリにある同じファイルの複数のコピー
-1 つのディレクトリ内で複数の変更
-暗号化ライブラリの呼び出しの可能性
-/tmp ディレクトリ内のプロセスの作成と実行が可能
-すばやく連続したディレクトリ列挙
-ワイルドカードを使用して、または確認なしでディレクトリからファイルを削除する
-chmod をワイルドカードで使う (または chmod 777)
-wget またはcurl cmdsの使用
-chmod cmd を使用して、実行可能ファイルを過度に許可された権限に変更する
-感染前または感染中に通信をエンコードしようとする文字列 cmd の使用
-vpcFlowLogs を使用して、外部 IP アドレスからの不適切なデータベースアクセスを特定する
-[Amazon Detective でVPC フローを調査する] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) または [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)

### 応答
-感染の可能性についてバックアップを検査する
-感染したシステムを隔離する
-侵害されたシステムをネットワークから削除します。
-規制要件または社内ポリシーに従って、EC2 インスタンスのフォレンジックが必要かどうかを判断する
-インスタンスのフォレンジックが必要な場合、またはデータを復元する必要がある場合は、[EC2 インスタンスを隔離する] (https://support.alertlogic.com/hc/en-us/articles/115005534366-Quarantine-an-EC2-Instance)

### リカバリ
-身代金を払わないことをお勧めします
-身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです
-データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を評価する必要があります。
-身代金を支払うことを選択した場合、攻撃者が自分の会社または他のユーザーに対して操作を継続できるようにする
-信頼できる AMI から新しい EC2 インスタンスを作成する
-規制要件または社内ポリシーに従って、EC2 インスタンスのフォレンジックが必要かどうかを判断する
-CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
-代替データバックアップ戦略を使用している場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前に最後にスケジュールされたイベントから復元します。
-< https://www.nomoreransom.org/ > アクセスして、データが感染したマルウェアバリアントで復号化が可能かどうかを特定する

## Amazon シンプルストレージサービス (S3)

### 識別する
-セキュリティ体制を評価して、セキュリティギャップを特定して修正する
-AWS は新しいオープンソース [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント。
-Amazon S3 に保存されている AWS アカウント、ワークロード、データを保護するために [AWS GuardDuty] (https://aws.amazon.com/guardduty/) を実装して、悪意のあるアクティビティや不正な動作を継続的に監視することを検討してください。
-AWS リソースの設定を評価、監査、評価できるサービスである [AWS Config] (https://aws.amazon.com/config/) の使用を検討してください。
-Darkbit は、オブジェクトの不正コピーを識別するために使用できる [AWS S3 データ損失防止] (https://darkbit.io/blog/simple-dlp-for-amazon-s3) の例を提供します。 ビジネスユースケースに基づいて、他の特定のオペレーションをパターンに追加することもできます。
-サーバー、ネットワークデバイス、ネットワーク/ファイル共有、開発者マシンなど、すべてのリソースの完全な資産インベントリを維持する

### 保護
-[レプリケーションの有効化] (http://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html)-同じリージョンまたはクロスリージョンを検討してください。 クロスリージョンレプリケーションは、別のリージョンに2番目のコピーを保持することにより、アプリケーションの欠陥やオペレータのエラーからデータを保護
-AWS Config を使用して、AWS サービスの変更を監視するルールを作成することを検討してください。 AWS Config には、EBS と EC2 をモニタリングするために、次のようないくつかのマネージドルールが事前に構築されています。
-[s3-account-level-public-access-blocks s] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks.html)
-[s3-account-level-public-access-blocks-periodic] (https://docs.aws.amazon.com/config/latest/developerguide/s3-account-level-public-access-blocks-periodic.html)
-[s3-bucket-blacklisted-actions-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-blacklisted-actions-prohibited.html)
-[s3-bucket-default-lock-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-default-lock-enabled.html)
-[s3-bucket-level-public-access-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-level-public-access-prohibited.html)
-[s3-bucket-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-logging-enabled.html)
-[s3-bucket-policy-grantee-check] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-grantee-check.html)
-[s3-bucket-policy-not-more-permissive] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-policy-not-more-permissive.html)
-[s3-bucket-public-read-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-read-prohibited.html)
-[s3-bucket-public-write-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-public-write-prohibited.html)
-[s3-bucket-replication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-replication-enabled.html)
-[s3-bucket-server-side-encryption-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-server-side-encryption-enabled.html)
-[s3-bucket-ssl-requests-only] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-ssl-requests-only.html)
-[s3-bucket-versioning-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/s3-bucket-versioning-enabled.html)
-[s3-default-encryption-kms] (https://docs.aws.amazon.com/config/latest/developerguide/s3-default-encryption-kms.html)
-[AWS Key Management Service (KMS)] (https://docs.aws.amazon.com/kms/latest/developerguide/overview.html) キーを使用してすべてのオブジェクトを暗号化し、攻撃者が暗号化キーを適用するのを防ぐことを検討してください。
-[AWS S3 ブロックパブリックアクセス機能] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html) を使用して、オブジェクトの意図しない露出を軽減することを検討してください。
-オブジェクトのバックアップとコスト最適化のために [S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) の使用を検討する
-*write-once-read-many* (WORM) モデルを使用してオブジェクトを格納できるように、[S3 Object Lock] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) の使用を検討してください。 オブジェクトロックは、一定時間または無期限にオブジェクトが削除または上書きされるのを防ぐのに役立ちます。
-オブジェクトロックコンプライアンスモード** では、保護されたオブジェクトのバージョンを AWS アカウントのルートユーザーを含むすべてのユーザーによって上書きまたは削除することはできません。 オブジェクトがコンプライアンスモードでロックされている場合、その保持モードは変更できず、保持期間を短縮することはできません。 コンプライアンスモード ***は、保持期間中はオブジェクトバージョンを上書きまたは削除できないことを保証します。
-機密情報または重要な情報を含む [S3 バケットおよびオブジェクトの CloudTrail イベントログ] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html) を有効にします。 デフォルトでは、CloudTrail 証跡はデータイベントをログに記録しませんが、指定した S3 バケットのデータイベントをログに記録したり、AWS アカウント内のすべての Amazon S3 バケットのデータイベントをログに記録するように証跡を設定できます。
-[S3 バケットの CloudTrail サーバーレベルのログ記録] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerLogs.html) および機密情報または重要な情報を含むオブジェクトを有効にします。 サーバーアクセスログは、バケットに対して行われたリクエストの詳細なレコードを提供します。
-[S3 Object Versioning] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/Versioning.html) を有効にして、変更されたオブジェクトの復元を許可する
-保持するバージョンの数を設定するには、[ライフサイクルポリシーを設定] (http://docs.aws.amazon.com/AmazonS3/latest/dev/intro-lifecycle-rules.html) で最新でないバージョンに適用します。
-[S3 MFA delete] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/MultiFactorAuthenticationDelete.html) を有効にして、攻撃者がバージョニングを無効にし、バケット内のすべてのオブジェクトを上書きすることを防ぎます。
-多要素認証 (MFA) を強制する
-パスワードの複雑さの要件を適用し、有効期限を設定する
-アカウントの有効期限と必須の認証情報のローテーションを含む [CIS AWS Foundations] (https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-cis-controls.html) を実装する
-[IAM Credential Report] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html) を実行して、アカウント内のすべてのユーザーと、パスワード、アクセスキー、MFA デバイスなどのさまざまな認証情報のステータスを一覧表示します。
-[新しい認証情報が公開されないようにする] (http://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)
-[AWS IAM Access Analyzer] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html) を使用して、外部エンティティと共有される Amazon S3 バケットや IAM ロールなど、組織内のリソースとアカウントを特定します。 これにより、セキュリティ上のリスクであるリソースとデータへの意図しないアクセスを特定できます。
-IAM ロールを使用してアクセス権限を管理する
-最小権限を実装し、s3:\ * 権限を許可しない
-バケットポリシーを使用し、定期的に監査する
-必要なものだけを公開し、すべてのオブジェクトがプライベートであることによって保護されるようにする
-特定のサードパーティソリューションをお勧めすることはできませんが、Veritas や CommVault などのパートナー製品や、S3 内のオブジェクトを S3 の内部または外部の管理対象バックアップストレージの場所にバックアップするネイティブ機能を持つ他のバックアップソフトウェアプロバイダからの製品もあります。

### 検出

-CloudTrail ログで、許可されていない IAM ユーザー、ポリシー、ロール、一時的なセキュリティ認証情報の作成など、認可されていないアクティビティがないかどうかを確認します。
-CloudTrail ログをチェックして、不正な EC2 インスタンス、Lambda 関数、EC2 スポット入札など、不正な AWS の使用状況について AWS アカウントを確認します。 AWS マネジメントコンソールにログインし、各サービスページを確認することで、使用状況を確認することもできます。 請求コンソールの\ "Bills\」ページでも、予期しない使用状況をチェックできます
-不正な使用はどのリージョンでも発生する可能性があり、コンソールに一度に表示されるリージョンは1つだけであることに注意してください。 リージョンを切り替えるには、コンソール画面の右上隅にあるドロップダウンを使用します。
-バケット内のオブジェクトとして、または顧客への電子メールを介して提供される身代金メモ
-S3 オブジェクトが削除されるか、S3 バケット全体が削除されます
-注：データ破壊イベントでは、身代金メモが提供される場合と提供されない場合があります。 また、CloudWatch メトリクスと CloudTrail S3 イベントをチェックして、データ漏洩が発生したかどうかを検証して、身代金攻撃またはデータ破壊攻撃の間を明確にするかどうかを確認してください。
-S3 オブジェクトは、お客様が所有していないアカウントのキーを使用して暗号化される

### 応答
-[IAM ユーザーキー] (https://console.aws.amazon.com/iam/home#users) と [Root ユーザーキー] (https://console.aws.amazon.com/iam/home#security_credential) を削除またはローテーション。公開されている特定のキーを特定できない場合、アカウント内のすべてのキーをローテーションすることもできます。
-権限のない削除 [IAM ユーザー] (https://console.aws.amazon.com/iam/home#users。)
-権限のない削除 [ポリシー] (https://console.aws.amazon.com/iam/home#/policies)
-権限のない削除 [ロール] (https://console.aws.amazon.com/iam/home#/roles)
-アプリケーションが公開されているアクセスキーを使用している場合は、キーを置き換える必要があります。 キーを置き換えるには、まず 2 番目のキーを作成し (その時点で両方のキーがアクティブになります)、新しいキーを使用するようにアプリケーションを修正します。 次に、コンソールの [非アクティブにする] オプションをクリックして、公開したキーを無効にします（ただし、削除しないでください）。 アプリケーションに何か問題がある場合は、公開されているキーを再度アクティブ化できます。 新しいキーを使用してアプリケーションが完全に機能する場合は、公開されているキーを削除してください
-[一時的な資格情報の取り消し] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 一時的な認証情報は、IAM ユーザーを削除することで取り消すこともできます。 *注:* IAM ユーザーの削除は実稼働ワークロードに影響する可能性があるため、注意して行う必要があります

### リカバリ
-身代金を払わないことをお勧めします
-身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです
-データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を評価する必要があります。
-身代金を支払うことを選択した場合、攻撃者が自分の会社または他のユーザーに対して操作を継続できるようにする
-データまたはオブジェクトの復元を試みる前に、[保護] で手順を実装する
-バージョン管理されたオブジェクトの [マーカーを削除] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/RemDelMarker.html) を削除する
-削除されたバケットを再作成する
-[S3 Intelligent Tiering] (https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/) オブジェクトバックアップまたはレプリケートされたリージョンバケットを使用してオブジェクトを復元する
-*注:* S3 には現在、「元に戻す」機能はありません。AWS には削除されたデータを復元する機能がありません。 [GDPR] (https://gdpr-info.eu/) や [CCPA] (https://oag.ca.gov/privacy/ccpa) などのデータストレージコンプライアンスおよび規制の現在の時代において、Amazon S3 はお客様のアカウントから明示的に削除された顧客データを引き続き保存することはできません。 オブジェクトが一度削除されると、意図しない削除が AWS に報告される速さに関係なく、AWS によって復元できなくなります。

## リレーショナルデータベースサービス (RDS)

### 識別する
-セキュリティ体制を評価して、セキュリティギャップを特定して修正する
-AWS は新しいオープンソース [Self-Service Security Assessment] (https://aws.amazon.com/blogs/publicsector/assess-your-security-posture-identify-remediate-security-gaps-ransomware/) ツールを開発しました。このツールは、お客様の AWS のセキュリティ状況に関する貴重な洞察を得るためのポイントインタイム評価を提供します。アカウント。
-AWS リソースの設定を評価、監査、評価できるサービスである [AWS Config] (https://aws.amazon.com/config/) の使用を検討してください。
-Amazon RDS に保存されている AWS アカウント、ワークロード、データを保護するために [AWS GuardDuty] (https://aws.amazon.com/guardduty/) を実装して、悪意のあるアクティビティや不正な動作を継続的に監視することを検討してください。
-サーバー、ネットワークデバイス、ネットワーク/ファイル共有、開発者マシンなど、すべてのリソースの完全な資産インベントリを維持する

### 保護
-[Amazon RDS のセキュリティ] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.html) で追加のリファレンスと手順を参照できます
-AWS Config を使用して、AWS サービスの変更を監視するルールを作成することを検討してください。 AWS Config には、RDS を監視するための次のようないくつかのマネージドルールが事前に構築されています。
-[rds-automatic-minor-version-upgrade-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-automatic-minor-version-upgrade-enabled.html)
-[rds-cluster-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-deletion-protection-enabled.html)
-[rds-cluster-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-iam-authentication-enabled.html)
-[rds-cluster-multi-az-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-cluster-multi-az-enabled.html)
-[rds-Enhanced-monitoring-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-enhanced-monitoring-enabled.html)
-[rds-instance-deletion-protection-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-deletion-protection-enabled.html)
-[rds-instance-iam-authentication-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-iam-authentication-enabled.html)
-[rds-instance-public-access-check] (https://docs.aws.amazon.com/config/latest/developerguide/rds-instance-public-access-check.html)
-[rds-in-backup-Plan] (https://docs.aws.amazon.com/config/latest/developerguide/rds-in-backup-plan.html)
-[rds-logging-enabled] (https://docs.aws.amazon.com/config/latest/developerguide/rds-logging-enabled.html)
-[rds-multi-az-support] (https://docs.aws.amazon.com/config/latest/developerguide/rds-multi-az-support.html)
-[rds-snapshots-public-prohibited] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshots-public-prohibited.html)
-[rds-snapshot-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-snapshot-encrypted.html)
-[rds-storage-encrypted] (https://docs.aws.amazon.com/config/latest/developerguide/rds-storage-encrypted.html)
-サードパーティのホストベースの侵入検知システム（HIDS）ソリューション（OSSEC、Tripwire、Wazuh、[Amazon Inspector] (https://aws.amazon.com/inspector/) など)
-Amazon VPC サービスに基づく仮想プライベートクラウド (VPC) で DB インスタンスを実行し、最大限のネットワークアクセスコントロールを実現します。 VPC で DB インスタンスを作成する方法の詳細については、[Amazon 仮想プライベートクラウド VPC と Amazon RDS] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.html) を参照してください。
-AWS ID およびアクセス管理（IAM）ポリシーを使用して、Amazon RDS リソースを管理できるユーザーを決定するアクセス権限を割り当てます。 たとえば、IAM を使用して、DB インスタンスの作成、説明、変更、削除、リソースへのタグ付け、セキュリティグループの変更ができるユーザーを決定できます。
-Amazon RDS 暗号化を使用して、保管中の DB インスタンスとスナップショットをセキュリティで保護します。 Amazon RDS 暗号化では、業界標準の AES-256 暗号化アルゴリズムを使用して、DB インスタンスをホストするサーバー上のデータを暗号化します。 詳細については、「Amazon RDS リソースの暗号化」(https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Overview.Encryption.html) を参照してください。
-MySQL、MariaDB、PostgreSQL、Oracle、または Microsoft SQL Server データベースエンジンを実行する DB インスタンスで、セキュアソケットレイヤー (SSL) またはトランスポートレイヤーセキュリティ (TLS) 接続を使用します。 DB インスタンスで SSL/TLS を使用する方法の詳細については、[SSL/TLS を使用して DB インスタンスへの接続を暗号化する] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/UsingWithRDS.SSL.html) を参照してください。
-セキュリティグループを使用して、DB インスタンス上のデータベースに接続できる IP アドレスまたは Amazon EC2 インスタンスを制御します。 DB インスタンスを最初に作成すると、そのセキュリティシステムは、関連付けられたセキュリティグループによって指定されたルール以外のデータベースアクセスを禁止します。
-Oracle DB インスタンスでネットワーク暗号化と透過データ暗号化を使用します。詳細については、[Oracle ネイティブネットワーク暗号化] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.NetworkEncryption.html) および [Oracle 透過データ暗号化] (https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.Oracle.Options.AdvSecurity.html)
-DB エンジンのセキュリティ機能を使用して、DB インスタンスのデータベースにログインできるユーザーを制御します。 これらの機能は、データベースがローカルネットワーク上にあるかのように機能します。

### 検出
-AWS GuardDuty 機械学習は、データ漏えいの試みの一環として Amazon リレーショナルデータベースサービス (Amazon RDS) のスナップショットを生成するなど、疑わしい動作を検出できます。 AWS セキュリティブログ [Amazon GuardDuty を使用して AWS アカウント内の疑わしいアクティビティを検出する方法] (https://aws.amazon.com/blogs/security/how-you-can-use-amazon-guardduty-to-detect-suspicious-activity-within-your-aws-account/) に例が記載されています。
-EC2 オペレーティングシステムとアプリケーションログで、不適切なログイン、不明なソフトウェアのインストール、または認識されないファイルの存在を確認します。
-vpcFlowLogs を使用して、外部 IP アドレスからの不適切なデータベースアクセスを特定する
-[Amazon Detective でVPC フローを調査する] (https://aws.amazon.com/blogs/security/investigate-vpc-flow-with-amazon-detective/) または [Amazon Athena] (https://docs.aws.amazon.com/athena/latest/ug/vpc-flow-logs.html)
-[AWS Security Analytics Bootstrap] (https://github.com/awslabs/aws-security-analytics-bootstrap) を使用すると、迅速なデプロイ、すぐに使用でき、メンテナンスが容易な Amazon Athena 分析環境を提供することで、AWS サービスログでセキュリティ調査を実行できます。

### 応答
-[IAM ユーザーキー] (https://console.aws.amazon.com/iam/home#users) と [Root ユーザーキー] (https://console.aws.amazon.com/iam/home#security_credential) を削除またはローテーション。公開されている特定のキーを特定できない場合、アカウント内のすべてのキーをローテーションすることもできます。
-権限のない削除 [IAM ユーザー] (https://console.aws.amazon.com/iam/home#users。)
-権限のない削除 [ポリシー] (https://console.aws.amazon.com/iam/home#/policies)
-権限のない削除 [ロール] (https://console.aws.amazon.com/iam/home#/roles)
-認識されていない、または許可されていないパブリックスナップショットまたはデータベースの削除
-データベースへのパーミッションアクセスを持つ EC2 インスタンスを特定し、それらも調査する
-アプリケーションが公開されているアクセスキーを使用している場合は、キーを置き換える必要があります。 キーを置き換えるには、まず 2 番目のキーを作成し (その時点で両方のキーがアクティブになります)、新しいキーを使用するようにアプリケーションを修正します。 次に、コンソールの [非アクティブにする] オプションをクリックして、公開したキーを無効にします（ただし、削除しないでください）。 アプリケーションに何か問題がある場合は、公開されているキーを再度アクティブ化できます。 新しいキーを使用してアプリケーションが完全に機能する場合は、公開されているキーを削除してください
-[一時的な資格情報の取り消し] (https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_temp_control-access_disable-perms.html#denying-access-to-credentials-by-issue-time)。 一時的な認証情報は、IAM ユーザーを削除することで取り消すこともできます。 *注:* IAM ユーザーの削除は実稼働ワークロードに影響する可能性があるため、注意して行う必要があります

### リカバリ
-身代金を払わないことをお勧めします
-身代金を支払うことは、犯罪者が支払いを受けた後に取引を尊重するかどうかについてのギャンブルです
-データのバックアップが存在しない場合は、コストメリット分析を行い、攻撃者への支払いに対するデータ/評判の侵害の価値を評価する必要があります。
-身代金を支払うことを選択した場合、攻撃者が自分の会社または他のユーザーに対して操作を継続できるようにする
-侵害されたデータベースを削除し、新しい RDS データベースを作成する
-規制要件または社内ポリシーに従って、RDS データベースのフォレンジックが必要かどうかを判断する
-< https://www.nomoreransom.org/ > アクセスして、データが感染したマルウェアバリアントで復号化が可能かどうかを特定する
-CloudEndure ディザスタリカバリを使用して、ランサムウェア攻撃またはデータ破損の前に最新の復旧ポイントを選択し、AWS でワークロードを復元します。
-代替データバックアップ戦略を使用している場合は、バックアップが感染していないことを検証し、ランサムウェアイベントの前に最後にスケジュールされたイベントから復元します。

### インシデントに追従する

### 身代金攻撃を当局に報告する
FBIは、身代金事件を法執行機関に報告することを組織に奨励している。 インターネット犯罪苦情センター（IC3）は、実際の被害者または第三者から苦情申立人へのオンラインインターネット犯罪の苦情を受け入れ、今後の最善の行動方針を決定するために協力します。 以下を共有する準備をしてください。
-苦情をサポートするために必要なと思われる関連情報
-メールヘッダー
-金融取引情報（口座情報、取引日と金額、受取人の詳細）
-件名の名前、住所、電話、電子メール、ウェブサイト、IPアドレス
-被害を受けた方法に関する具体的な詳細
-被害者の名前、住所、電話、電子メール

### 根本原因分析を実行する
ほとんどの場合、既存のフォレンジックツールは AWS 環境で動作します。 フォレンジックチームは、AWS リージョン全体でのツールの自動デプロイと、ビジネスクリティカルなアプリケーションが構築されているのと同じ堅牢でスケーラブルなサービスを使用して、低摩擦で大量のデータを迅速に収集できるというメリットがあります。

[Amazon Detective] (https://aws.amazon.com/detective/%20) を使用すると、潜在的なセキュリティ問題や疑わしいアクティビティの根本原因を簡単に分析、調査、迅速に特定できます。 Amazon Detective は、AWS リソースから AWS サービスログデータを自動的に収集し、機械学習、統計分析、グラフ理論を使用して、より迅速かつ効率的なセキュリティ調査を実施できるリンクされた一連のデータを構築します。

### 学んだ教訓でセキュリティプログラムを更新する
シミュレーションやライブインシデントで学んだ教訓を「新しい通常の」プロセスと手順に戻すことで、組織が脆弱な場所、自動化が失敗した可能性がある場所、可視性が欠けている場所など、違反がどのように発生したかをよりよく理解できます。全体的なセキュリティ体制を強化する機会。

## 従業員を訓練する
効果的なセキュリティ意識向上プログラムを通じて従業員をトレーニングします。 これは面白くて魅力的なものにする必要があります。 フィッシングの報告、通知手続き、「悪い」ことを報告する方法に焦点を当てて、その行動を授けることは非常に効果的です。

## 結論
身代金攻撃は進化していますが、セキュリティ意識と備えも進化しています。 世界中の政府機関、非営利団体、および企業は、インフラストラクチャを強化し、システムとデータを安全に保つために AWS を信頼しています。 この Playbook で共有されている AWS のサービスとベストプラクティスを使用して、AWS 環境における身代金の可能性と影響を軽減するために、プロアクティブな対策を講じることができます。

## 付録 A: 身代金攻撃を防止または検出するためのサービスとツール
[Amazon CloudWatch Synthetics] (https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html)
[Amazon CloudWatch] (https://aws.amazon.com/cloudwatch)
[Amazon CodeGuru] (https://aws.amazon.com/codeguru/)
[Amazon EventBridge] (https://aws.amazon.com/eventbridge/)
[Amazon GuardDuty] (https://aws.amazon.com/guardduty/)
[Amazon Inspector Network Reachability] (https://docs.aws.amazon.com/inspector/latest/userguide/inspector_network-reachability.html)
[Amazonインスペクター] (https://aws.amazon.com/inspector/)
[Amazon Macie] (https://aws.amazon.com/macie/)
[Amazon VPC 到達可能性アナライザー] (https://docs.aws.amazon.com/vpc/latest/reachability/what-is-reachability-analyzer.html)
[AWS CDK] (https://aws.amazon.com/cdk/)
[AWS Cloud9] (https://aws.amazon.com/cloud9/)
[AWS CloudFormation Guard] (https://github.com/aws-cloudformation/cloudformation-guard)
[AWS CloudFormation] (https://aws.amazon.com/cloudformation/)
[AWS CodeArtifact] (https://aws.amazon.com/codeartifact/)
[AWS CodeBuild] (https://aws.amazon.com/codebuild/)
[AWS CodeCommit] (https://aws.amazon.com/codecommit/)
[AWS CodeDeploy] (https://aws.amazon.com/codedeploy/)
[AWS CodePipeline] (https://aws.amazon.com/codepipeline/)
[AWS Config Rules] (https://aws.amazon.com/blogs/aws/aws-config-rules-dynamic-compliance-checking-for-cloud-resources/)
[AWS コントロールタワー] (https://aws.amazon.com/controltower/)
[AWS IAM アクセスアナライザー] (https://docs.aws.amazon.com/IAM/latest/UserGuide/what-is-access-analyzer.html)
[AWS IAM] (https://aws.amazon.com/iam/)
[AWS KMS] (https://aws.amazon.com/kms/)
[AWS Lambda] (https://aws.amazon.com/lambda/)
[AWS Organizations] (https://aws.amazon.com/organizations/)
[AWS Secrets Manager] (https://aws.amazon.com/secrets-manager/)
[AWS セキュリティハブ] (https://aws.amazon.com/security-hub/)
[AWS Step Functions] (https://aws.amazon.com/step-functions/)
[AWS システムマネージャー] (https://aws.amazon.com/systems-manager/)
[AWS WAF] (https://aws.amazon.com/waf/)
[AWSPX] (https://github.com/FSecureLABS/awspx)
[checkMarx] (https://checkmarx.com/)
[EC2 Image Builder] (https://aws.amazon.com/image-builder/)
[ECR ネイティブコンテナイメージスキャン] (https://aws.amazon.com/blogs/containers/amazon-ecr-native-container-image-scanning/)
[git-secrets] (https://github.com/awslabs/git-secrets)
[pMapper] (https://github.com/nccgroup/PMapper)
[Prowler] (https://github.com/toniblyx/prowler)
[sonarQube] (https://www.sonarqube.org/)