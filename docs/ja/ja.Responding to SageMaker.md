# インシデント対応プレイブック:SageMaker セキュリティイベントへの対応
この文書は情報提供のみを目的としています。 本書の発行日現在のAmazon ウェブサービス (AWS) が提供している製品および慣行を表しており、予告なしに変更されることがあります。 お客様は、本書に記載された情報および AWS の製品またはサービスの使用について、お客様自身で評価する責任を負うものとします。これらの評価は、明示的か黙示的かを問わず、いかなる種類の保証もなく「現状のまま」提供されます。 この文書は、AWS、その関連会社、サプライヤー、またはライセンサーからの保証、表明、契約上のコミットメント、条件、または保証をするものではありません。 お客様に対する AWS の責任と責任は AWS 契約によって管理されており、この文書は AWS と顧客間の契約の一部ではなく、また変更されるものでもありません。

© 2024 Amazon ウェブサービス株式会社またはその関連会社 全著作権所有。 この作品はクリエイティブ・コモンズ表示 4.0 国際ライセンスの下に提供されています。

本 AWS コンテンツは、http://aws.amazon.com/agreement で入手可能な AWS カスタマー契約の条件、またはお客様とアマゾンウェブサービス株式会社、Amazon ウェブサービス、EMEA、SARL、あるいはその両方との間で締結されたその他の書面による契約に従って提供されます。


## 連絡窓口

作成者:`著者名`\
承認者:`承認者名`\
最終承認日:

## エグゼクティブサマリー
お客様への継続的な取り組みの一環として、AWS はこのセキュリティインシデント対応プレイブックを提供しています。このプレイブックには、Amazon SageMaker が AWS アカウント内での不正使用の発生元または対象となっているセキュリティイベントの調査に必要な手順が記載されています。 このドキュメントの目的は、セキュリティイベントが発生した疑いがある場合に取るべき措置に関する規範的なガイダンスを提供することです。

! [画像] (sagemaker_images/nist_life_cycle.png)

*AWS インシデント対応の側面*


### Amazon セージメーカー

Amazon SageMaker はフルマネージド型の機械学習 (ML) サービスです。 SageMaker を使用すると、データサイエンティストと開発者は ML モデルを迅速かつ自信を持って構築、トレーニング、本番環境に対応したホスト環境にデプロイできます。 ML ワークフローを実行するための UI エクスペリエンスが提供されるため、SageMaker ML ツールを複数の統合開発環境 (IDE) で利用できるようになります。

SageMaker を使用すると、独自のサーバーを構築して管理しなくても、データを保存して共有できます。 独自のアルゴリズムとフレームワークのサポートが組み込まれているため、SageMaker は特定のワークフローに合わせて調整できる柔軟な分散型トレーニングオプションを提供しています。 追加情報については、Amazon SageMaker の開発者ガイドをこちらでご覧ください。https://docs.aws.amazon.com/sagemaker/latest/dg/whatis.html



## 準備
予防的コントロール ([サービスコントロールポリシー] (https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)) と検出コントロール (設定ルールの「検出」セクションを参照) を実装して、環境をプロアクティブに準備します。


**予防策 (SCP) **

VPC CIDC CIDC F
* たとえば、次の SCP では、VPC が指定されていない限り、ユーザーはノートブック、トレーニング、または処理ジョブを起動できなくなります。

```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「VPC デプロイ」、
「効果」:「拒否」、
「アクション」: [
「SageMaker: ハイパーパラメータチューニングジョブの作成」,
「セージメーカー:モデル作成」,
「セージメーカー:ノートブックインスタンスの作成」,
「セージメーカー:処理ジョブの作成」,
「セージメーカー:トレーニングジョブを作成」
],
「リソース」: [
「*」
],
「条件」: {
「NULL」: {
「セージメーカー:VPC セキュリティグループ ID」:「true」,
「セージメーカー:VPC サブネット」:「true」
}
}
}
]
}
```


* ジョブの暗号化を強制

```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「暗号化されていないボリュームを拒否」,
「効果」:「拒否」、
「アクション」: [
「SageMaker: ハイパーパラメータチューニングジョブの作成」,
「SageMaker: トレーニングジョブを作成」,
「SageMaker: エンドポイントコンフィグの作成」,
「セージメーカー:トランスフォームジョブの作成」
],
「リソース」: [
「*」
],
「条件」: {
「NULL」: {
「セージメーカー:ボリューム KMS キー」: [
「true」
]
}
}
}
]
}
```
* コンテナ間のトラフィック暗号化を強制
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「暗号化されていないトラフィックを拒否」,
「効果」:「拒否」、
「アクション」: [
「セージメーカー:トレーニングジョブの作成」,
「SageMaker: ハイパーパラメータチューニングジョブの作成」
],
「リソース」: [
「*」
],
「条件」: {
「ブール」: {
「セージメーカー:コンテナ間トラフィック暗号化」:「false」
}
}
}
]
}
```

* ネットワーク分離を強制
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「分離されていないことを拒否」,
「効果」:「拒否」、
「アクション」: [
「セージメーカー:トレーニングジョブの作成」,
「SageMaker: ハイパーパラメータチューニングジョブの作成」,
「セージメーカー:モデル作成」
],
「リソース」:「*」,
「条件」: {
「ブール」: {
「セージメーカー:ネットワーク分離」:「false」
}
}
}
]
}
```
* ノートブックの事前署名 URL を IP に限定する
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「URL を IP に制限」,
「効果」:「拒否」、
「アクション」:「SageMaker: 署名済みノートブックインスタンス URL の作成」,
「リソース」:「*」、
「条件」: {
「すべての値:IP アドレス以外」: {
「AWS: ソース IP」: [
「[パブリック IP アドレスを入力]」
]
}
}
}
]
}
```
* インターネットアクセスを無効にする
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「ダイレクトインターネットを拒否」,
「効果」:「拒否」、
「アクション」:「SageMaker: ノートブックインスタンスの作成」,
「リソース」:「*」、
「条件」: {
「文字列と等しい」: {
「SageMaker: インターネットへの直接アクセス」: [
「有効」
]
}
}
}
]
}
```

* SageMaker ノートブックのRoot アクセスを無効にする
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「セージメーカーがルートアクセスを拒否」,
「効果」:「拒否」、
「アクション」: [
「セージメーカー:ノートブックインスタンスの作成」,
「SageMaker: ノートブックインスタンスの更新」
],
「リソース」:「*」,
「条件」: {
「文字列と等しい」: {
「セージメーカー:ルートアクセス」: [
「有効」
]
}
}
}
]
}
```
* ユーザーが起動できるインスタンスタイプを制限する
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「SageMaker リミットインスタンスタイプ数」,
「効果」:「拒否」、
「アクション」:「SageMaker: ノートブックインスタンスの作成」,
「リソース」:「*」、
「条件」: {
「任意の値:文字列が似ていない」: {
「SageMaker: インスタンスタイプ」: [
「[インスタンスタイプ例]」,
「ml.c5.xlarge」,
「ml.m5.xlarge」,
「ml.t3. ミディアム」
]
}
}
}
]
}
```

* Studio についても同様に、以下のサンプルポリシーを参照してください。 管理者はデフォルトの Jupyter Server アプリのシステムインスタンスを許可する必要があることに注意してください。
```
{
「バージョン」:「2012-10-17"、
「ステートメント」: [
{
「Sid」:「SageMaker で許可されているインスタンスタイプ」,
「効果」:「拒否」、
「アクション」: [
「セージメーカー:アプリ作成」
],
「リソース」:「*」,
「条件」: {
「任意の値:文字列が似ていない」: {
「SageMaker: インスタンスタイプ」: [
「ml.c5. ラージ」,
「ml.m5. ラージ」,
「ml.t3. ミディアム」,
「システム」
]
}
}
}
]
}
```


## 検出

### セージメーカーチェック

### AWS Config

AWS Config にはいくつかの [SageMaker を評価するためのマネージドルール] (https://docs.aws.amazon.com/config/latest/developerguide/managed-rules-by-aws-config.html)
* sagemaker-endpoint-configuration-kms-key-configure
* sagemaker-endpoint-config-prod-instance-count
* sagemaker-ノートブック-インスタンス-内部-VPC
* SageMaker ノートブックインスタンス KMS キー設定
VPC CIPC CIDC CIDC CISCF CIDC CIDC CIDCF CIDC CIDCF
* SageMaker ノートブック-インターネットへの直接アクセスなし

### CloudTrail イベント

環境への不正アクセスが発生した場合のクイックリファレンスとして、関連する SageMaker API 呼び出しに関連するシナリオを以下で確認してください (これは SageMaker API 呼び出しの完全なリストではないことに注意してください)。


**データ/モデルの流出:**
-SageMaker データストアまたはモデルアーティファクトから機密データをコピーまたはダウンロードする。
-モデルパラメータまたはトレーニングデータを抽出すると、知的財産や個人情報が漏えいする可能性がある。

<ins>API 呼び出しの例:</ins>
-`DescribeModelPackage` を使用してモデルパッケージに関する情報を取得します。
-「DescribeTrainingJob」を使用すると、トレーニングジョブの詳細とその出力データにアクセスできます。
-「GetModelPackageModelMetrics`」を使用すると、モデルメトリクスや機密性の高いデータを取得できます。



**データポイズニング:**
* 悪意のあるデータや敵対的な例を注入して、トレーニング済みのモデルを変更したり、ポイズニングしたりする。
* 侵害されたモデルを SageMaker エンドポイントに展開すると、誤った予測や悪意のある出力につながる。

<ins>API 呼び出しの例:</ins>
-「CreateModelPackage」または「UpdateModelPackage」を使用して、侵害を受けたモデルパッケージをデプロイします。
-「CreateTransformJob」を使用して、ポイズニングされたモデルを使用してトランスフォームジョブを実行します。
-「CreateEndpointConfig」と「CreateEndpoint」を使用して、悪意のあるモデルをエンドポイントにデプロイします。
-「CreateTrainingJob」または「UpdateTrainingJob」を使用して、悪意のあるデータをトレーニングジョブに注入します。



**リソースの誤用:**
* ?$#@$プトマイニングやその他の悪意のあるアクティビティを目的として、無許可の SageMaker ノートブックまたはインスタンスを起動する。
* SageMaker リソースを AWS 環境内でのラテラルムーブメントのエントリーポイントまたはピボットポイントとして使用する。

<ins>API 呼び出しの例:</ins>
-「CreatebookInstance」または「UpdateNotebookInstance」を使用して、権限のないノートブックインスタンスを起動します。
-「CreateTrainingJob」または「CreateHyperParameterTuningJob」を使用して過剰なトレーニングジョブを開始する。
-「CreateEndpoint」または「UpdateEndpoint」を使用して、不正な目的でエンドポイントを作成または変更します。

**サービス拒否 (DoS): **
* 過剰なトレーニングジョブまたはエンドポイントを起動して SageMaker リソース (コンピュートインスタンス、ストレージなど) を使い果たす。
* 大量のリクエストで SageMaker API またはサービスに負荷がかかり、サービスの中断につながる。

<ins>API 呼び出しの例:</ins>
-「CreateTrainingJob」または「CreateHyperParameterTuningJob」を使用して多数のトレーニングジョブを起動し、リソースを使い果たします。
-「CreateEndpoint」または「UpdateEndpoint」を使用して複数のエンドポイントを作成し、過剰なコンピューティングリソースを消費します。

**設定の変更:**
* SageMaker の役割、ポリシー、または権限を変更して権限を昇格させたり、不正アクセスを許可したりする。
* SageMaker VPC 設定、セキュリティグループ、またはネットワーク設定を変更してセキュリティ制御を回避すること。

<ins>API 呼び出しの例:</ins>
-SageMaker のロールと権限を変更するには「ロールを作成」または「ロールを更新」します。
-「CreatebookInstanceLifeCycleConfig」または「UpdateNotebookInstanceLifeCycleConfig」を使用してノートブックインスタンスの設定を変更します。
-「CreateEndpointConfig」または「UpdateEndpointConfig」を使用して、エンドポイント構成またはセキュリティ設定を変更します。

**ログ改ざん:**
* SageMaker のログや監査記録を修正または削除して、痕跡を隠したりインシデント調査を妨げたりする。
* セキュリティアナリストの誤解を招いたり、悪意のあるアクティビティを隠したりするために、誤ったログエントリを挿入すること。

<ins>API 呼び出しの例:</ins>
-「PutModelPackageModelMetrics」を使って偽のモデルメトリクスをログに注入する。
-「StopTrainingJob」または「StopTransformJob」を使用すると、ログデータを変更または削除できる可能性があります。

**マルウェアのデプロイ:**
* SageMaker ノートブックまたはインスタンスにマルウェアやバックドアを配備して、永続的なアクセスやデータ盗難を目的とする行為。
* SageMaker のリソースを利用してマルウェアを配布したり、他のシステムやネットワークに攻撃を仕掛けたりする。

<ins>API 呼び出しの例:</ins>
-「CreateNotebookInstance」または「UpdateNotebookInstance」を使用して、マルウェアを含むノートブックインスタンスを起動します。
-「CreateModelPackage」または「UpdateModelPackage」を使用して、悪意のあるコードを含むモデルパッケージをデプロイします。

**認証情報の盗難:**
* ノートブックまたはインスタンスに保存されている AWS 認証情報または SageMaker API キーを盗むこと。
* 盗んだ認証情報を使用して、他の AWS リソースまたはサービスにさらに不正にアクセスする。

<ins>API 呼び出しの例:</ins>
-「DescribeNotebookInstance」または「DescribeTrainingJob」を使用すると、保存されている認証情報または API キーにアクセスする可能性があります。
-「GetModelPackageModelMetrics`」または「DescribeModelPackage」を使用して、機密情報または認証情報を取得します。

** ?$#@$プトジャッキング:**
* SageMaker コンピュートリソース (インスタンス、エンドポイントなど) をハイジャックして、不正な暗号通貨マイニング活動を行うこと。
* コンピュートリソースを過剰に消費し、サービスの中断やコストの増加につながる可能性がある。

<ins>API 呼び出しの例:</ins>
-「CreateNotebookInstance」または「UpdateNotebookInstance」を使用して、暗号通貨マイニング用のインスタンスを起動します。
-「CreateTrainingJob」または「CreateHyperParameterTuningJob」を使用して、マイニング目的で計算量の多いジョブを開始します。


**注:これらの API 呼び出しは正当な目的でも使用できますが、不正アクセスの場合は、危険なアクションを実行するために悪用される可能性があることに注意してください。 SageMaker API やリソースの不正使用を検出して防止するには、強固なアクセス制御、監視、監査メカニズムを実装することが不可欠です。 **

### SageMaker のログエントリについて

以下のスクリーンショットは、インシデントレスポンダーが調査中に見つかったイベントの解釈を視覚的にわかりやすくするためのものです。 以下の各画像は、記録されたイベント名と一致するアクションを示しています。

---

** ドメインの作成**
<details>
<summary>スクリーンショットを展開</summary>
-
ドメインを作成します。 ドメインは、関連する Amazon Elastic File System ボリューム、承認済みユーザーのリスト、さまざまなセキュリティ、アプリケーション、ポリシー、および Amazon 仮想プライベートクラウド (VPC) 設定で構成されます。 ドメイン内のユーザーは、ノートブックファイルやその他のアーティファクトを相互に共有できます。

! [ドメインを作成] (/sagemaker_images/sagemaker-01.png)
</details>

---

** SageMaker ドメインの詳細**
<details>
<summary>スクリーンショットを展開</summary>

Amazon SageMaker ドメインは SageMaker 機械学習 (ML) 環境をサポートしています。 SageMaker ドメインは、ドメイン、ユーザープロファイル、共有スペース、アプリケーションのエンティティで構成されます。


! [ドメイン詳細] (/sagemaker_images/sagemaker-02.png)
! [ドメイン詳細 2] (/sagemaker_images/sagemaker-03.png)

</details>

---


**ドメイン作成時の CloudTrail イベント**

<details>
<summary>スクリーンショットを展開</summary>

Cloudtrail の「CreateDomain」イベントには、VPC、サブネット、実行ロール、アプリケーションなどの情報がすべて含まれていることに注意してください。

! [VPC、実行ロール、アプリケーションなどが関連付けられた SageMaker ドメイン] (/sagemaker_images/sagemaker-04.png)


</details>

---

**エンドポイント作成用のCloudTrail イベント**

<details>
<summary>スクリーンショットを拡大</summary>

CloudTrail の SageMaker の「CreateEndpoint」イベントは「SageMaker-ExecutionRole」サービスロールによって呼び出されることに注意してください

! [エンドポイントの例を作成] (/sagemaker_images/sagemaker-05.png)

</details>

---



## 分析

インシデントが発生した場合、侵害の兆候、脅威アクター、時間枠などを調査することに加えて、これがSageMakerのリソースに関連するインシデントであることが確認されたら、考慮すべきその他の質問がいくつかあります。

1. SageMaker リソースに無断でアクセスされたのはどれですか? (ノートブック、モデル、エンドポイント、データストアなど)
2. 不正アクセスはどのようにして得られましたか。 (認証情報の侵害、権限の設定ミス、脆弱性の悪用など)
3. 不正アクセスの際に、影響を受けた SageMaker リソースに対してどのようなアクションが実行されましたか?
4. モデルやデータが盗まれたり改ざんされたりしたことはありますか？
5. モデルにアクセスした場合、モデルポイズニングや敵対的攻撃のリスクはありますか？
6. 不正アクセス中に新しいリソース (ノートブック、エンドポイントなど) が作成または変更されたことはありますか?
7. 不正アクセス中に SageMaker API または SDK が使用されましたか。また、それらを通じてどのようなアクションが実行されたか。
8. SageMaker のログやオーディットトレイルは、トラックをカバーするように変更または削除されましたか?
9. インシデント中に SageMaker のロールや IAM ポリシーが変更されたり、悪用されたりしたことはありますか?
10. SageMaker VPC の設定やネットワーク設定が変更されたことはありますか?
11. SageMaker のノートブックまたはインスタンスが、さらなる不正アクセスの入り口または要点として使用されたことはありましたか?
12. 他の AWS リソースや外部システムに対する攻撃や悪意のあるアクティビティを仕掛けるために SageMaker リソースが使用されたことはありますか?
13. 不正アクセスが SageMaker リソースと関連データの機密性、完全性、可用性にどのような影響を与える可能性があるか?
14. 影響を受ける SageMaker リソースを安全に分離してバックアップし、復旧や再構築を行うにはどうすればよいか？
15. SageMaker のセキュリティ上のベストプラクティスや構成が守られず、不正アクセスにつながったのはどれですか?

## 封じ込めと根絶

権限のないユーザーによってリソースが作成された場合や、作成を許可されていないリソースがある場合は、作成したリソースまたは権限を削除または変更する方法について、以下の手順に従ってください。

-[SageMaker ドメインを削除する方法 (コンソール)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-studio)
-[SageMaker ドメインを削除する方法 (CLI)] (https://docs.aws.amazon.com/sagemaker/latest/dg/gs-studio-delete-domain.html#gs-studio-delete-domain-cli)
-[SageMaker エンドポイントを削除する方法] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint)
-[SageMaker エンドポイント設定を削除する方法] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-endpoint-config)
-[SageMaker モデルを削除する方法] (https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-delete-resources.html#realtime-endpoints-delete-model)
-[SageMaker ノートブックからRoot アクセスを削除] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html) (目的のノートブックインスタンスに移動/インスタンスを停止/完了したら、[編集] をクリック/ [権限] で [ノートブックインスタンスの無効化/更新]/[インスタンスの実行] を選択します)


## 対処済みのバックログ項目
-インシデントレスポンダーとして、SageMaker の重要なイベントをすべて監視できる必要がある
-インシデントレスポンダーとして、SageMaker Cloudtrail イベントの大規模なクエリに関するプレイブックが必要だ

## 現在のバックログ項目

## 付録-ベストプラクティス

**強く推奨**

-[隔離された VPC にデプロイ] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
-VPC エンドポイントを使用してリソースにアクセスする
-[Sagemaker ノートブックは VPC 内からの接続へのアクセスを制限すべき] (https://docs.aws.amazon.com/sagemaker/latest/dg/infrastructure-connect-to-resources.html)
-セキュリティグループと NACL を使用して、環境に出入りするトラフィックを制御します。
-[複数のコンピュートインスタンスによるトレーニングジョブのコンテナ間トラフィック暗号化] (https://docs.aws.amazon.com/sagemaker/latest/dg/train-encrypt.html)
-[KMS を使用して保存時の暗号化を有効にする] (https://docs.aws.amazon.com/sagemaker/latest/dg/encryption-at-rest.html)
-[ノートブックへのルートアクセスが不要な場合は無効にする] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-root-access.html)
-[ライフサイクル設定のベストプラクティス] (https://docs.aws.amazon.com/sagemaker/latest/dg/nbi-lifecycle-config-install.html)
-チームが悪意のあるコードが実行されるリスクを軽減するために使用できるパッケージの許可リストを作成する
-[IAM ロールとリソースベースのポリシー (S3 バケットデータにアクセスするためのバケットポリシーなど) を使用する最小権限、ML ガバナンスを活用] (https://docs.aws.amazon.com/sagemaker/latest/dg/governance.html)
-[IAM ID センターを使用] (https://docs.aws.amazon.com/sagemaker/latest/dg/domain-user-profile-add-remove.html)
-[シークレットマネージャーに認証情報を保存してローテーションする] (https://docs.aws.amazon.com/secretsmanager/latest/userguide/integrating-sagemaker.html)
-[SageMaker モデルモニターを使用してモデルの入力と出力を監視する] (https://docs.aws.amazon.com/sagemaker/latest/dg/model-monitor-faqs.html)
-[S3 データおよびモデルアーティファクト監査の CloudTrail S3 データイベントロギングを有効にする] (https://docs.aws.amazon.com/AmazonS3/latest/userguide/enable-cloudtrail-logging-for-s3.html)
-[SageMaker Experiments を有効にしてモデルアーティファクトのバージョンコントロールを活用] (https://docs.aws.amazon.com/sagemaker/latest/dg/experiments.html)
-[VPC Flow Logs を有効にして VPC 内のネットワークトラフィックを監視する] (https://docs.aws.amazon.com/vpc/latest/userguide/flow-logs.html)
-[CodeArtifact を使って必要なライブラリ/パッケージをインターネットからダウンロードする] (https://aws.amazon.com/blogs/machine-learning/secure-aws-codeartifact-access-for-isolated-amazon-sagemaker-notebook-instances/)
-[CloudWatch は SageMaker の監視にも使える] (https://docs.aws.amazon.com/sagemaker/latest/dg/monitoring-cloudwatch.html)

** 奨励**

-[Amazon Macie を使って機密な S3 データを保護しよう] (https://docs.aws.amazon.com/macie/latest/user/data-classification.html)
-[Service Catalog を使用して、インスタンスサイズを制限し、安全な構成の使用を強制するために、事前に精査された SageMaker リソース (ノートブックなど) を販売する] (https://aws.amazon.com/blogs/machine-learning/automate-a-centralized-deployment-of-amazon-sagemaker-studio-with-aws-service-catalog/)