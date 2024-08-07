# インシデント対応プレイブック:EC2 フォレンジック
このドキュメントは、情報提供のみを目的として提供されています。 本書は、このドキュメントの発行日時点におけるAmazon ウェブサービス (AWS) の現在の製品提供および慣行を表しており、これらは予告なしに変更される場合があります。 お客様は、本書に記載されている情報、および AWS 製品またはサービスの使用について、独自の評価を行う責任を負うものとします。各製品またはサービスは、明示または黙示を問わず、いかなる種類の保証もなく「現状のまま」提供されます。 このドキュメントは、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上の約束、条件、または保証を作成するものではありません。 お客様に対する AWS の責任と責任は、AWS 契約によって管理され、このドキュメントは AWS とお客様との間のいかなる契約の一部でもなく、変更もありません。

© 2024 Amazon ウェブサービス, Inc. またはその関連会社。 すべての権利予約。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

この AWS コンテンツは、http://aws.amazon.com/agreement で提供される AWS カスタマーアグリーメントの条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス EMEA SARL、またはその両方との間のその他の書面による契約に従って提供されます。

[_TOC_]]

## 連絡ポイント

著者:`著者名`
承認者:`承認者名`
最終承認日:

## エグゼクティブ・サマリー
このプレイブックでは、悪意があると特定された EC2 インスタンスまたは追加の調査を保証する侵害の指標を持つ EC2 インスタンスでフォレンジックを実行するプロセスの概要を説明します。

また、データを別の AWS リージョンにレプリケートするのではなく、イベントが発生した同じ AWS リージョンで調査を実行することをお勧めします。 この方法は主に、リージョン間でデータを転送するのに必要な時間が増えるため、この方法をお勧めします。 お客様が運用することを選択した各 AWS リージョンについて、インシデント対応プロセスとレスポンダーの両方が、関連するデータプライバシー法に準拠していることを確認してください。 AWS リージョン間でデータを移動する必要がある場合は、法域間でデータを移動することによる法的影響を考慮してください。 一般に、データを同じ国の管轄区域内に保持することがベストプラクティスです。

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
1. [**準備**] フォレンジック手順を実行する前にベストプラクティスを特定する
2. [**準備**] フォレンジック VPC を作成する
3. [**準備**] SSM VPC Endpoint を追加する
4. [**準備**] フォレンジック VPC のネットワークアクセスコントロールリストを作成する
5. [**準備**] フォレンジックを作成する EC2 ゴールデン AMI
6. [**準備**] クラウドフォレンジック機能を特定して評価するためのトレーニングプログラムを実装する
7. [**準備**] エスカレーション手順の特定、文書化、およびテストを行う
8. [**検出と分析**] Amazon EBS Snapshot を作成する
９。 [**検出と分析**] Amazon EBS Snapshot を共有する
10. [**検出と分析**] コンソールを使用して暗号化されていないスナップショットを共有する
第11回。 [**検出と分析**] 非公開で共有された暗号化されていないスナップショットを使用する
12. [**検出と分析**] コンソールを使用して暗号化されたスナップショットを共有する
13. [**検出と分析**] 共有された暗号化されたスナップショットを使用する
第14回。 [**検出と分析**] フォレンジックスゴールデン AMI からフォレンジック EC2 を作成する
15だ [**検出と分析**] 疑わしいEBSボリュームをフォレンジック EC2 にマウントする
16. [**検出と分析**] フォレンジックプロセスを実行する
17だ [**封じ込め**] 影響を受けるリソースをすぐに隔離する
18日。 [**リカバリ**] 必要に応じてリカバリプロセスを実行する

***対応手順は、[NIST Special Publication 800-61r2 コンピュータセキュリティインシデント処理ガイド] (https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-61r2.pdf) のインシデント対応ライフサイクルに従います。

! [画像] (/images/nist_life_cycle.png) ***

### インシデントの分類と処理
* **戦術、テクニック、手順**: ツール:フォレンジック
* **カテゴリー**: フォレンジック
* **リソース**: EC2、VPC
* **指標**: サイバー脅威インテリジェンス、サードパーティ通知、Cloudwatch メトリクス、AWS Shield
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
### [ベストプラクティス] (https://d1.awsstatic.com/Marketplace/scenarios/security/SEC_11_TSB_Final.pdf)
* すべてのリージョンで AWS CloudTrail をオンにする
CloudTrail のログは、アクティブに使用する AWS リージョンに制限しないでください。 すべての AWS リージョンで CloudTrail をオンにすると、組織が使用していない AWS リージョンからプロビジョニングされる AWS サービスなど、異常な動作をより簡単に特定できるようになります。

* ログ情報を保護する
システムコンポーネントの監査証跡が有効でアクティブになっていることを確認し、定期的にバックアップします。 攻撃者が悪意のある行為の証拠を提供する可能性のあるログファイルを削除できないように、異なる資格情報を必要とする場所に保存することを検討してください。

* AWS 悪用コミュニケーションを決して無視しない
AWS は、不正使用事例が提出されると、登録済みの E メールアドレスに自動的に E メールを送信します。 すぐに返信し、専用の電子メール応答アドレスを設定することを検討してください。 セキュリティインシデントに迅速に対応すればするほど、ビジネスへの影響をより効果的に制限できます。 複数の利害関係者が構成するアカウントに [セキュリティのための代替連絡先の配布リスト] (https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/manage-account-payment.html#manage-account-payment-alternate-contacts) を用意して、メッセージが 1 つで「詰まらない」ことはない従業員の受信トレイ（休暇、退職、病気など）。

### フォレンジック VPC を作成する
**注**: 本番リソースに対してフォレンジックを実施する必要がある可能性のある各リージョン内で、フォレンジック VPC を設定することをお勧めします。

1. [Amazon VPC コンソール] (https://console.aws.amazon.com/vpc/) を開きます
1. ナビゲーションペインで、[VPC]、[VPC の作成] を選択します。
1. 必要に応じて、次の VPC の詳細を指定します。
* 名前タグ:オプションで VPC の名前を指定します。 これにより、Name のキーと指定した値を持つタグが作成されます。
* IPv4 CIDR ブロック:VPC の IPv4 CIDR ブロックを指定します。 指定できる最小の CIDR ブロックは /28 で、最大値は /16 です。 RFC 1918 で指定されたプライベート（非パブリックルーティング可能）IP アドレス範囲（10.0.0.0/16、または 192.168.0.0/16 など）から CIDR ブロックを指定することをお勧めします。
* 注:パブリックにルーティング可能な IPv4 アドレスの範囲を指定できます。 ただし、現在、VPC 内のパブリックルーティング可能な CIDR ブロックからのインターネットへの直接アクセスをサポートしていません。
* 224.0.0.0 ～ 255.255.255（クラス D およびクラス E の IP アドレス範囲）の範囲で VPC で起動した場合、Windows インスタンスは正しく起動できません。
* IPv6 CIDR ブロック:オプションで IPv6 CIDR ブロックを VPC に関連付けます。 次のいずれかのオプションを選択し、[CIDR の選択] を選択します。
* Amazon提供の IPv6 CIDR ブロック:Amazon の IPv6 アドレスプールから IPv6 CIDR ブロックをリクエストします。 [ネットワークボーダーグループ] で、AWS が IP アドレスをアドバタイズするグループを選択します。
* 私の所有する IPv6 CIDR: (BYOIP) IPv6 アドレスプールから IPv6 CIDR ブロックを割り当てます。 [プール] で、IPv6 CIDR ブロックを割り当てる IPv6 アドレスプールを選択します。
* テナンシー:テナンシオプションを選択します。
* この VPC で起動されるインスタンスが、起動時に指定されたテナンシー属性に関係なく、専有テナンシーインスタンスであることを保証するには、[Dedicated] を選択します。
* この VPC で起動されるインスタンスが、起動時に指定したテナンシー属性を使用するようにするには、[Default] を選択します。
* (オプション) タグを追加または削除します。
* [作成] を選択します。

### [SSM 用の VPC Endpoint を追加] (https://aws.amazon.com/premiumsupport/knowledge-center/ec2-systems-manager-vpc-endpoints/)

1. SSM エージェントがインスタンスにインストールされていることを確認します。
2. Systems Manager の AWS アイデンティティおよびアクセス管理 (IAM) インスタンスプロファイルを作成します。 新しいロールを作成するか、必要な権限を既存のロールに追加できます。
3. IAM ロールをプライベート EC2 インスタンスにアタッチします。
4. Amazon EC2 コンソールを開き、インスタンスを選択します。 [説明] タブで、VPC ID とSubnet ID を書き留めます。
5. Systems Manager の VPC エンドポイントを作成します。
* [サービス名] で [com.amazonaws] を選択します。 [地域] .ssm (例:com.amazonaws.us-east-1.ssm)。 リージョンコードの完全なリストについては、「使用可能なリージョン」を参照してください。
* VPC の場合は、インスタンスの VPC ID を選択します。
* [Subnets] で、VPC でSubnet ID を選択します。 高可用性を実現するには、リージョン内の異なるアベイラビリティーゾーンから少なくとも 2 つのサブネットを選択します。
* 注:同じアベイラビリティーゾーンに複数のサブネットがある場合、追加のサブネットVPC endpoints を作成する必要はありません。 同じアベイラビリティーゾーン内の他のサブネットは、インターフェイスにアクセスして使用できます。
* [DNS 名を有効にする] で、[このエンドポイントで Enable] を選択します。 詳細については、「インターフェイスエンドポイントのプライベート DNS」を参照してください。
* [セキュリティグループ] で、既存のセキュリティグループを選択するか、新しいセキュリティグループを作成します。 セキュリティグループは、サービスと通信する VPC 内のリソースからのインバウンド HTTPS（ポート 443）トラフィックを許可する必要があります。
* 新しいセキュリティグループを作成した場合は、VPC コンソールを開き、[Security Groups] を選択し、新しいセキュリティグループを選択します。 [受信ルール] タブで、[受信ルールの編集] を選択します。 次の詳細を含むルールを追加し、[ルールの保存] を選択します。
* [タイプ] で [HTTPS] を選択します。
* [ソース] で VPC CIDR を選択します。 高度な設定の場合、EC2 インスタンスで使用される特定のサブネットの CIDR を許可できます。
* セキュリティグループ ID に注意してください。 この ID は他のエンドポイントと一緒に使用します。
6. AWS Systems Manager の VPC インターフェイスエンドポイントのポリシーを作成します。
* 次の変更を加えてステップ 5 を繰り返します。[サービス名] で [com.amazonaws] を選択します。 [地域] .ec2messages。
* 次の変更を加えてステップ 5 を繰り返します。[サービス名] で [com.amazonaws] を選択します。 [地域] .ssmmessages。 Session Manager を使用する場合は、この操作を行う必要があります。

### フォレンジック VPC のネットワークアクセスコントロールリストを作成する
このプロセスによって VPC が分離され、内部に含まれるインスタンスからのトラフィックの出入りができません。

1. [Amazon VPC コンソール] (https://console.aws.amazon.com/vpc/) を開きます
1. ナビゲーションペインで、[セキュリティ]、[Network ACLs] の順に選択します。
1. フォレンジック VPC の横にあるチェックボックスをオンにします。
1. 右上にあるドロップダウンの [アクション] と [インバウンドルールの編集] を選択します。
1. ルール 100 を「すべてのトラフィック」から `SSH` に変更する
1. [変更を保存] を選択します
1. アウトバウンドルールについてこれらの手順を繰り返します。

### フォレンジックを作成する EC2 ゴールデン AMI
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. 「インスタンスの起動」を選択します。
1. 検索バーに「Ubuntu」と入力し、Enter キーを押します。
1. `Ubuntu Server 18.04 LTS`を選択
1. インスタンスタイプの選択
* 注:フォレンジックの目的で、M5 タイプのインスタンスを選択することをお勧めします。
1. 「次へ:インスタンスの詳細の設定」を選択します。
* ネットワーク:フォレンジック VPC ネットワークを選択します。
* パブリック IP の自動割り当て:無効にする
* IAM ロール:フォレンジック目的で適切なロールを選択します。
* 終了保護を有効にする:このボックスがオンになっていることを確認します
1. 「次へ:ストレージを追加」を選択します。
*「新しいボリュームを追加」を選択し、キャプチャするフォレンジックデータを処理するのに十分なサイズのボリュームを作成します。 適切な開始値は 100 GB ですが、この値は組織のニーズとエクスペリエンスに基づきます。
1. 「次へ:タグを追加」を選択します。
1. 「レビューして起動」を選択します。
1. 「起動」を選択
1. EC2 Session Manager を使用して EC2 インスタンスにログインします。
1. インスタンスにフォレンジックツールをインストールします。 オープンソースフォレンジックツールの例を次に示します。
1. SANS SIFT Toolkit インストール
* sudo su-ubuntu
* sudo curl-Lo /usr/local/bin/sift https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux https://github.com/teamdfir/sift-cli/releases/download/v1.10.0-rc5/sift-cli-linux
* sudo chmod +x /usr/local/bin/sift
* sudo /usr/local/bin/sift install —mode=server —user=ubuntu
* sudo /usr/local/bin/sift upgrade
1. Google Rapid Response インストール
* sudo apt install mysql-server
* sudo mysql_secure_installation
* create database grr; grant all privileges on grr.* to grr @localhost identified by 'password'; flush privileges; @localhost
* wget https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb https://storage.googleapis.com/releases.grr-response.com/grr-server_3.2.4-6_amd64.deb
* sudo apt インストール。 /grr-server_3.2.4-6_amd64.deb
* sudo systemctl restart grr-server
* sudo systemctl status grr-server
1. 詳細については、[Ubuntu 18.04 で GRR クライアントをインストールしてセットアップする方法] (https://kifarunix.com/how-to-install-and-setup-grr-clients-on-ubuntu-18-04-debian-9/) を参照してください。
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. [インスタンス] に移動します。
1. AMI のベースとして使用するインスタンスを右クリックし、コンテキストメニューから [Create Image] を選択します。
1. [イメージを作成] ダイアログボックスで、一意の名前と説明を入力し、[イメージの作成] を選択します。
* デフォルトでは、Amazon EC2 はインスタンスをシャットダウンし、アタッチされたボリュームのスナップショットを取得し、AMI を作成して登録し、インスタンスを再起動します。
* インスタンスをシャットダウンしたくない場合は、[再起動しない] を選択します。
* 再起動しない] を選択した場合、作成されたイメージのファイルシステムの整合性は保証されません。

AMI が作成されるまでに数分かかる場合があります。 作成後、AWS Explorer の AMI ビューに表示されます。 このビューを表示するには、AWS エクスプローラーで Amazon EC2 | AMI ノードをダブルクリックします。 AMI を表示するには、[表示] ドロップダウンリストから [所有者] を選択します。 AMI を表示するには、[Refresh] を選択する必要がある場合があります。 AMI が最初に表示されると、保留状態にある可能性がありますが、しばらくすると、使用可能な状態に移行します。

### トレーニング
-「フォレンジックの責任を持つ人材に対する知識の期待は何ですか？ (知識/スキル/能力) `
-「私の担当者はどのようなフォレンジックトレーニングと手順を受けますか？ `
-「私の会社に適用されるフォレンジックの規制要件は何ですか？ `
-「社内のアナリストが AWS API（コマンドライン環境）、S3、RDS、その他の AWS サービスに精通するためのトレーニングはどのようなものですか？ `

**フォレンジックは単なるプレイブックではなくスキルなので、ここにもっとコンテキストとアイテムを追加する必要があります

## エスカレーション手順
-「EC2 フォレンジックをいつ実施すべきかについてビジネス上の決定が必要です`
-「ログ/アラートを監視し、それらを受け取り、それぞれに対して行動しているのは誰ですか？ `
-「アラートが検出されたときに通知されるのは誰ですか？ `
-「広報と法律がプロセスに関与するのはいつですか？ `
-「AWS サポートにヘルプを連絡するのはいつですか？ `

## 検出と分析
### アラートの可能性のあるソース
* 課金
* CloudWatch (EC2 メトリクス/イベントルール)
* コンフィグ
* GuardDuty
* 検査官
* マシー
* セキュリティハブ
* 信頼されたアドバイザー
* 外部通知

### データ保護
**常に**:
* 証拠を読み取り専用にする
* 証拠は**決して**改変すべきだ
* 証拠のコピーを作成し、それらからのみ働く
* **決してない** 分析のために（直接アクセス）ソース証拠から作業する
* すべての証拠をマウント読み取り専用
* 誰が何時アクセスしたかを追跡する
* 以下について多量の調査メモを取る。
* 分析したもの（どのデータ）
* 分析した時 (日付/時刻)
* 実行した内容（特定のコマンド/手順/プロセス）
* 見つかったもの（結果）

### メタデータの取得
調査に役立つかもしれないインスタンスに関連する情報をキャプチャする
* インスタンスタイプ/ID
* IP アドレス
* セキュリティグループ
* VPC/Subnet ID
* 地域
* AMI ID
* 起動時間

### メモリ獲得
サードパーティのユーティリティを使用して、実行中のインスタンスからメモリを取得することをお勧めします。
使用可能な揮発性（および貴重な）データをすべてキャプチャするには、システムの分離/シャットダウン前に**メモリをキャプチャすることが不可欠です

### [Amazon EBS Snapshot を作成する] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-modifying-snapshot-permissions.html)
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. ナビゲーションペインの [Elastic Block Store] の下の [スナップショット] を選択します。
1. [スナップショットの作成] を選択します。
1. [リソースタイプの選択] で、[ボリューム] を選択します。
1. [ボリューム] で、ボリュームを選択します。
1. （オプション）スナップショットの説明を入力します。
1. （オプション）[Add Tag] を選択して、スナップショットにタグを追加します。 タグごとに、タグキーとタグ値を指定します。
1. [スナップショットの作成] を選択します。

### [Amazon EBS Snapshot 共有] (https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf)
#### コンソールを使用して暗号化されていないスナップショットを共有する
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. ナビゲーションペインで [Snapshots] を選択します。
1. スナップショットを選択し、[アクション]、[権限の変更] の順に選択します。
1. スナップショットをパブリックにするか、次のように特定の AWS アカウントと共有します。
* スナップショットを公開するには、[Public] を選択します。
* このオプションは、暗号化されたスナップショットや AWS Marketplace 製品コードを使用したスナップショットでは無効です。
* スナップショットを 1 つ以上の AWS アカウントと共有するには、[プライベート] を選択し、[AWS アカウント番号] に AWS アカウント ID（ハイフンなし）を入力し、[Add Permission] を選択します。 追加の AWS アカウントについても同じ手順を繰り返します。
1. [保存] を選択します。
#### プライベートで共有された暗号化されていないスナップショットを使用するには
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. ナビゲーションペインで [Snapshots] を選択します。
1. [プライベートスナップショット] フィルタを選択します。
1. ID または説明でスナップショットを見つけます。 このスナップショットは、他のスナップショットと同様に使用できます。たとえば、スナップショットからボリュームを作成したり、スナップショットを別のリージョンにコピーしたりできます。

#### コンソールを使用して暗号化されたスナップショットを共有する
1. [AWS KMS コンソール] (https://console.aws.amazon.com/kms) を開きます
1. AWS リージョンを変更するには、ページの右上隅にあるリージョンセレクターを使用します。
1. ナビゲーションペインで [カスタマー管理キー] を選択します。
1. [Alias] 列で、スナップショットの暗号化に使用したカスタマー管理キーのエイリアス (テキストリンク) を選択します。 主要な詳細が新しいページで開きます。
1. [Key policy] セクションには、ポリシービューまたはデフォルトビューが表示されます。 ポリシービューには、キーポリシードキュメントが表示されます。 デフォルトのビューには、キー管理者、キーの削除、キーの使用、その他の AWS アカウントのセクションが表示されます。 コンソールでポリシーを作成し、カスタマイズしていない場合は、デフォルトのビューが表示されます。 デフォルトビューが使用できない場合は、ポリシービューでポリシーを手動で編集する必要があります。 詳細については、AWS キーマネジメントサービス開発者ガイドの「キーポリシー (コンソール) を表示する」を参照してください。
アクセス可能なビューに応じて、ポリシービューまたはデフォルトビューのいずれかを使用して、ポリシーに 1 つ以上の AWS アカウント ID を追加します。
* (デフォルトビュー) [他の AWS アカウント] までスクロールします。 [他の AWS アカウントを追加] を選択し、プロンプトに従って AWS アカウント ID を入力します。 別のアカウントを追加するには、[別の AWS アカウントを追加] を選択し、AWS アカウント ID を入力します。 すべての AWS アカウントを追加したら、[変更を保存] を選択します。
* (ポリシービュー) [編集] を選択します。 1 つ以上の AWS アカウント ID を次のステートメントに追加します。「キーの使用を許可する」および「永続リソースの添付を許可」。 [変更を保存] を選択します。 次の例では、AWS アカウント ID 444455556666 がポリシーに追加されます。
```
{
「Sid」:「キーの使用を許可する」,
「効果」:「許可」,
「プリンシパル」: {"AWS」: [
「arn: aws: iam። 111122223333: user/cmkUser」,
「arn: aws: iam። 44445555666: root」
]},
「アクション」: [
「KMS: encrypt」,
「KMS: 復号化」,
「kms: reencrypt*」,
「KMS: 生成された DataKey *」,
「kms: describeKey」
]、
「リソース」:「*」
}、
{
「Sid」:「永続リソースの添付を許可する」,
「効果」:「許可」,
「プリンシパル」: {"AWS」: [
「arn: aws: iam። 111122223333: user/cmkUser」,
「arn: aws: iam። 44445555666: root」
]},
「アクション」: [
「KMS: CreateGrant」,
「kms: listGrants」,
「kms: revokeGrant」
]、
「リソース」:「*」,
「条件」: {"ブール」: {"kms: grantisforawsResource」: true}}
}
```
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. ナビゲーションペインで [Snapshots] を選択します。
1. スナップショットを選択し、[アクション]、[権限の変更] の順に選択します。
1. AWS アカウントごとに、AWS アカウント番号に AWS アカウント ID を入力し、[アクセス許可の追加] を選択します。 すべての AWS アカウントを追加したら、[Save] を選択します。

#### 共有された暗号化されたスナップショットを使用するには
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. ナビゲーションペインで [Snapshots] を選択します。
1. [プライベートスナップショット] フィルタを選択します。 必要に応じて、[暗号化] フィルタを追加します。
1. ID または説明でスナップショットを見つけます。
1. スナップショットを選択し、[アクション]、[コピー] の順に選択します。
1. (オプション) 宛先リージョンを選択します。
1. スナップショットのコピーは、マスターキーに表示されるキーによって暗号化されます。 デフォルトでは、選択したキーはアカウントのデフォルトの CMK です。 カスタマー管理の CMK を選択するには、入力ボックス内をクリックして、使用可能なキーのリストを表示します。
1. [コピー] を選択します。

### フォレンジックスゴールデン AMI からフォレンジック EC2 を作成する
以前に作成したフォレンジックスゴールデン AMI を使用して、新しいフォレンジックインスタンスを作成します。

### 疑わしいEBS ボリュームをフォレンジック EC2 にマウントする
1. [Amazon EC2 コンソール] (https://console.aws.amazon.com/ec2/) を開きます
1. ナビゲーションペインで、[Elastic Block Store]、[ボリューム] を選択します。
1. 使用可能なボリュームを選択し、[アクション]、[ボリュームのアタッチ] の順に選択します。
1. [Instance] で、インスタンスの名前または ID の入力を開始します。
1. オプションのリストからインスタンスを選択します（ボリュームと同じアベイラビリティーゾーンにあるインスタンスのみが表示されます）。
1. [デバイス] では、推奨されるデバイス名を保持するか、サポートされている別のデバイス名を入力できます。
* 詳細については、[Linux インスタンスのデバイス名] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/device_naming.html) を参照してください。
1. [アタッチ] を選択します。
1. インスタンスに接続し、ボリュームをマウントします。
* 詳細については、[Linux で Amazon EBS ボリュームを使用できるようにする] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html) を参照してください。

### フォレンジックプロセスの実行
* `会社が求めているフォレンジックのアウトプットは何ですか？ `
* `最終報告書には何が含まれますか？ `
* `レポートは誰に行きますか？ `
* `法医学調査から必要な情報を特定する法的要件や規制上の要件はありますか？ `
* `法医学を行うために、適切に訓練/ライセンス/認定された人材が会社内にいますか？ `
* `ログ、データ、エビデンスの保持要件はありますか？ 潜在的にGlacier Vaultsを調べてサポート`
* [Amazon Linux EC2 インスタンスのデジタルフォレンジック分析] (https://www.sans.org/reading-room/whitepapers/cloud/digital-forensic-analysis-amazon-linux-ec2-instances-38235) の例はSANS研究所から入手できます。

## 封じ込め
### 影響を受けるリソースをすぐに隔離する
**注**: リソースを分離するためのエスカレーションと承認を要求するプロセスがあることを確認し、分離が現在のオペレーションと収益源にどのような影響を与えるかについて、ビジネスへの影響分析を最初に実行するようにします。

### インスタンスの廃止
終了保護を有効にする
* [インスタンスの終了を無効にする (Console/API/CLI 経由)] (https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html)
1. インスタンスのシャットダウン動作を停止（対終了）に設定する
1. すべてのインスタンスボリュームで「DeleteOnTermination」を無効にする
* [Auto-Scaling group から削除 (デタッチ)] (https://docs.aws.amazon.com/autoscaling/ec2/userguide/detach-instance-asg.html)
1. https://console.aws.amazon.com/ec2autoscaling/ で Amazon EC2 Auto Scaling コンソールを開きます。
1. Auto Scaling グループの横にあるチェックボックスをオンにします。
1. Auto Scaling グループページの下部に分割ペインが開き、選択したグループに関する情報が表示されます。
1. [インスタンス管理] タブの [インスタンス] でインスタンスを選択し、[アクション]、[デタッチ] の順に選択します。
1. [Detach instance] ダイアログボックスで、チェックボックスを選択して置換インスタンスを起動するか、またはオフのままにして、目的の容量を減らします。 [インスタンスのデタッチ] を選択します。
* [ロードバランサーからの登録解除] (https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-deregister-register-instances.html)
1. https://console.aws.amazon.com/ec2/ で Amazon EC2 コンソールを開きます。
1. ナビゲーションペインの [負荷分散] で、[ロードバランサー] を選択します。
1. ロードバランサーを選択します。
1. 下部ペインで、[Instances] タブを選択します。
1. [インスタンスの編集] を選択します。
1. ロードバランサーに登録するインスタンスを選択します。
1. [保存] を選択します。
1. すべての入出力トラフィックをブロックする*new* セキュリティグループを作成します。出力トラフィックのデフォルトの `allow all` ルールを削除してください
* 影響を受けるインスタンスに*new* セキュリティグループをアタッチする

## 撲滅
この Runbook に適用されるこのプロセスの手順はありません。

## リカバリ
フォレンジックアクティビティが終了し、最終レポートの発行後、データを保持するための法的または規制上の要件がない限り、フォレンジック EC2 インスタンスを終了し、EBS スナップショットを削除する必要があります。

## 学んだ教訓
「これは、必ず「修正」を必要としないが、運用上およびビジネス上の要件と並行してこのプレイブックを実行する際に知っておくべき重要な、貴社固有のアイテムを追加する場所です。 `

## アドレスバックログ項目
-インシデントレスポンダーとして EC2 フォレンジックを実施するには Runbook が必要です
-インシデントレスポンダーとして、EC2 フォレンジックをいつ実施すべきかについてのビジネス上の決定が必要です
-インシデントレスポンダーとして、使用の意図に関係なく、有効になっているすべてのリージョンでログを有効にする必要があります
-インシデントレスポンダーとして、承認された AMI のみが使用されていることを確認する必要があります
-インシデントレスポンダーとして、既存の EC2 インスタンスで暗号マイニングを検出できる必要があります
-インシデントレスポンダーとして、大量のインスタンスを削除するための準備された方法が必要です

## 現在のバックログアイテム