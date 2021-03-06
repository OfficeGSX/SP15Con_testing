---
title: SharePoint アドインと SharePoint ソリューションの比較
ms.prod: SHAREPOINT
ms.assetid: 0e9efadb-aaf2-4c0d-afd5-d6cf25c4e7a8
---


# SharePoint アドインと SharePoint ソリューションの比較
SharePoint 2013 拡張機能を SharePoint アドイン として開発する場合と SharePoint ファーム ソリューション またはコードなし セキュリティで保護されたソリューション として開発する場合について説明します。
 * **適用対象:*** 
  
    
    

この記事では、SharePoint アドイン、ファーム ソリューション、およびコードなし セキュリティで保護されたソリューション (NCSS) のユース ケースを比較します。
- 新しい SharePoint アドイン は自己完結型の拡張機能で、クラウドベースのロジックとデータ、SharePoint コンポーネント、およびクライアント側のスクリプトが含まれることがありますが、SharePoint サーバーで動作するカスタム マネージ コードではありません。Office ストア または組織のアドイン カタログのどちらからもインストールされ、オンプレミスファームまたは Microsoft SharePoint Online のいずれかにインストールできます。SharePoint アドイン の詳細については、「 [SharePoint アドイン](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)」を参照してください。
    
  
- SharePointファーム ソリューション はファーム全体のギャラリーにアップロードされる SharePoint コンポーネントのパッケージであり、ここからインストールすることができます。 Office ストア 経由で配布することはできず、SharePoint Online にインストールすることはできません。SharePoint ファーム サーバー上で動作するカスタム マネージ コードが含まれることがあります。ファーム ソリューション の基本について詳しくは、「 [ソリューションの概要](http://msdn.microsoft.com/library/1983cab9-4b29-494a-a62a-0f8e83908744%28Office.15%29.aspx)」と「 [ファーム ソリューション](http://msdn.microsoft.com/library/845f7524-b9ff-412b-aa29-3afacda91100%28Office.15%29.aspx)」を参照してください。
    
  
- NCSS も SharePoint コンポーネントのパッケージですが、NCSS はサイト コレクション ギャラリーにアップロードされ、そこからインストールすることができます。NCSS はオンプレミス ファームまたは SharePoint Online のいずれかにインストールできますが、Office ストア を介して配布することはできません。NCSS には SharePoint アドイン とほとんど同じ種類の説明コンポーネントを含めることができます。またアドインと同様、JavaScript を含めることができますが、SharePoint サーバーで実行されるカスタム マネージ コードは含まれません。アドインと NCSS の展開システムの違いにより、一部のシナリオでは NCSS の方がより優れた開発オプションになります。セキュリティで保護されたソリューション の詳細については、「 [サンドボックス ソリューションの概要 (SharePoint Server 2010)](https://technet.microsoft.com/ja-jp/library/ee721992%28v=office.14%29.aspx)」を参照してください。
    
  

> **重要**
> 宣言型マークアップと JavaScript だけを含んでいる セキュリティで保護されたソリューション (これを NCSS (No-Code Sandboxed Solution) と呼びます) の開発は依然有望視されていますが、セキュリティで保護されたソリューション 内ではカスタム マネージ コードの使用を廃止しました。マネージ コードの使用が必要なシナリオでの代替案として、新しい SharePoint アドイン モデルを導入しています。アドイン モデルでは、SharePoint コア プロダクトがアドインのランタイムから切り離されているので、柔軟性を大幅に向上させることができ、任意の環境でコードを実行することができます。コード化された セキュリティで保護されたソリューション へ投資をされたお客様がいることは理解していますが、これについては責任を持って段階的に廃止して参ります。既存のコード化された セキュリティで保護されたソリューション は当分の間、オンプレミス SharePoint ファームでも引き続き機能します。オンライン サービスのダイナミックな性質を考慮して、お客様のニーズに基づいて、SharePoint Online でのコード化された セキュリティで保護されたソリューション をサポートの必要性を判断いたします。将来のすべての投資は新しい SharePoint アドイン モデルをより豊かにより強力にするために行われます。したがって、新しい開発では新しいアドイン モデルをできる限り使用することをお勧めします。ファーム ソリューション またはコード化された セキュリティで保護されたソリューション を開発する必要がある場合は、より緩やかに結合された開発モデルに簡単に変更できるような設計にすることをお勧めします。 
  
    
    


## 可能な場合はいつでもアドインを開発する
<a name="AppWhenYouCan"> </a>

最も重要な点として、できる限り ファーム ソリューションまたは NCSS ではなく、SharePoint アドイン を開発することをお勧めします。SharePoint アドインには、従来のソリューションと比較した場合、次の利点があります。
  
    
    

- ユーザーに最も簡単な検出、購入、およびインストールのプロセスを提供します。
    
  
- 管理者に最も安全な SharePoint 拡張機能を提供します。
    
  
- Microsoft のオンライン アドイン ストアに基づいた最もシンプルなマーケティングおよび営業システムを提供します。
    
  
- 将来のアップグレード開発における柔軟性を最大化します。
    
  
- SharePoint以外の既存のプログラミング スキルを最大限に活用できるようにします。
    
  
- クラウドベースのリソースをより円滑かつ柔軟性のある方法で統合します。
    
  
- アドインを実行しているユーザーのアクセス許可とは異なるアクセス許可を拡張機能に与えます。
    
  
- HTML、REST、OData、JavaScript、OAuth などのクロスプラットフォーム標準を使用できるようにします。
    
  
- SharePoint クロス ドメイン JavaScript ライブラリを利用して、SharePoint データにアクセスできるようにします。または、Microsoft のセキュリティ保護された OAuth 準拠のトークン サービス、またはデジタル証明書を使用して、SharePoint データへの認証を取得できます。
    
  

## エンド ユーザー向けにアドインまたは NCSS を設計し、管理者向けにファーム ソリューションを設計する
<a name="SPAppVsClassic_Overview"> </a>

SharePoint アドイン および NCSS は、SharePoint クライアント オブジェクト モデルの 1 つ、または REST エンドポイントを使用して、SharePoint のコンテンツおよびコンポーネントにアクセスします。これらのクライアント API により、エンド ユーザー向けに設計されている SharePoint 拡張機能が有効にされます (このコンテキストでは、エンド ユーザーは、サイトコレクション管理者、Web サイト所有者、および Web サイト メンバーです)。 サーバー オブジェクト モデルには、SharePoint の管理、構成、およびセキュリティをプログラムで拡張することができる追加 API があります。サーバーの全体管理、Windows PowerShell のカスタム コマンド、タイマー ジョブ、カスタム バックアップなどの拡張が含まれます。開発できる管理拡張機能について詳しくは、「 [Windows SharePoint Services の管理](http://msdn.microsoft.com/library/cdcc1b8a-4144-446f-b471-03d4a754a8ab%28Office.15%29.aspx)」を参照してください。これらの管理拡張機能は、ファーム、Web アプリケーション、またはサイトコレクションのスコープを持つ SharePoint 機能に展開されます。また、SharePointファーム ソリューション はファーム管理者によってもインストールされますが、アドインと NCSS はテナントおよびサイト コレクションの管理者がインストールできます。
  
    
    
サーバー オブジェクト モデルには、リスト、ライブラリ、Web サイトで作成/読み取り/更新/削除 (CRUD) 操作を行い、他の SharePoint コンポーネントで操作を行うための API もあります。つまり、サーバー オブジェクト モデルは、エンド ユーザー向けの拡張機能に使用することができますが、前のセクションで述べた理由により、通常、ファーム ソリューションはこうした拡張機能に最適ではありません。したがって、ファーム ソリューションは Microsoft SharePoint Online にインストールすることができません。Microsoft は SharePoint Online のすべての管理を扱うので、管理拡張機能は必要ありません。SharePoint のさまざまな API とこれらが重複する箇所に関する詳細については、「 [SharePoint 2013 での適切な API セットの選択](choose-the-right-api-set-in-sharepoint-2013.md)」を参照してください。
  
    
    
クライアント オブジェクト モデルと REST エンドポイントは、サーバー オブジェクト モデルの管理指向の API を複製しません。さらに、SharePoint アドイン および NCSS には、SharePoint サーバーで実行するカスタム コードを含めることができないので、これらの管理 API を呼び出すことができません。また、SharePoint アドインのすべての機能は Web サイト スコープを持つ必要があります。また NCSS の機能はサイト コレクションまたは Web サイトのスコープを持つ必要があります。したがって、SharePoint アドイン または NCSS では、管理指向の SharePoint 拡張を行うことができません。したがって、絶対的な規則ではありませんが、基本的に、アドインおよび NCSS はエンド ユーザー向けであり、ファーム ソリューションは管理者向けであるということが言えます。
  
    
    

## ブランド用およびテンプレートタイプの拡張機能用の NCSS の設計
<a name="SPAppVsClassic_Overview"> </a>

アドイン モデルが適切でなく、ファーム ソリューション を使っても実装できない SharePoint 開発シナリオがいくつかあります。その原因としては、ソリューションを SharePoint Online 上にインストールする必要があるか、サイト コレクションの管理者がインストールする必要があるかのいずれかが考えられます。このようなシナリオには、広い範囲で重なり合うカテゴリが 2 つ存在します。
  
    
    

- **ブランド:**SharePoint ユーザーはしばしば、自分の SharePoint サイト (SharePoint Online サイトを含みます) に、独自の色、スタイル、レイアウト、ロゴを設定したカスタムな外観を設定することを要望します。これは一般的に、SharePoint アドイン よりも NCSS を使った方が簡単に実行できます。SharePoint アドイン が備えているのは、独自のアドイン Web の外観に関する宣言型コントロールだけです。ホスト Web の場合、リボン ボタンとメニュー項目 (およびアドイン パーツ) だけを宣言して追加できます。ホスト Web またはその親サイト コレクション、テナント、またはオンプレミス SharePoint Web アプリケーションに対するその他の変更は、SharePoint のクライアント オブジェクト モデルのいずれかを使用するコードまたはスクリプトを使って実行されます。たとえば、新しいアイコンや CSS ファイルは、プログラムを使って展開する必要があります。このようなコードは、アドインをインストールした後、アドイン自体から実行することができます。また、アドインのインストール イベント ハンドラで実行することもできます。ただし、このコードを作成するには、かなりの作業が必要になります。さらに、このアドインでアドイン Web とホスト Web 以外の Web サイトを変更するには、サイト コレクション スコープの権限が必要になります。また、親サイト コレクション以外の変更を行う場合、テナント スコープの権限が必要になります。ただし、ブランド NCSS は、任意のサイト コレクションに展開してアクティブ化できます。また、少数の完全な宣言型コンポーネントによって構成されている場合もあります。 
    
    > **メモ**
      > SharePointファーム ソリューション はブランド化に関して、NCSS や SharePoint アドイン よりも潜在的に強力ですが、SharePoint Online では使用できません。 
- **「テンプレートタイプ」の拡張機能:** ビジネス クライシスの共同の分析やソリューション用に、SharePoint のクライシス管理拡張機能が必要と仮定します。この拡張機能には、複数のカスタム リスト タイプ、コーディング不要のワークフロー、その他の SharePoint コンポーネントが含まれていて、すべてを組み合わせて、1 つのカスタム WebTemplate が構築されます。この拡張機能を NCSS としてパッケージ化して導入する場合、ソリューションの機能をアクティブ化すると、クライシスが発生した場合にいつでも、ユーザーは独自の SharePoint Web サイトのクライシス管理サブ Web を作成できます。Web サイトには、クライシスに固有のデータを入力できます。一方、まったく同じ SharePoint コンポーネント セットを使って、同じ拡張機能を SharePoint アドイン として実装することもできます。アドインをチーム サイトにインストールすると、サブ Web (「アドイン Web」と呼ばれています) が直ちに作成されます。この Web サイトでも、ユーザーは関連データを入力できます。
    
    ここで、2 つ目のクライシスが発生すると、何が起こるでしょう。拡張機能を NCSS として実装した場合、ユーザーがカスタム WebTemplate から別のサブ Web を作成することはほとんどありません。しかし、拡張機能を SharePoint アドイン として実装した場合、ユーザーにはある問題が発生します。それは、親 Web サイトにアドインの 2 番目のインスタンスをインストールできないということです。ホスト Web にインストールできるアドインのインスタンスは 1 つだけです。最善の対処方法は、親 Web サイトのサブサイトを作成し、それをアドインの別のインタンスをインストールする場所にすることです。アドインをインストールすると、アドインのコンポーネントに対して新しいアドイン Web (親 Web サイトのサブサブ Web になります) が作成されます。この比較からわかることは一般的なことで、絶対的な原則というわけではありませんが、「テンプレートタイプ」の特徴 (コンポーネントの再利用可能なコレクションは固有のユース ケースごとに構成する必要がありますが、一般に同じ SharePoint Web サイトには複数のインスタンスが関連付けられています) を持つ SharePoint 拡張機能は、アドイン モデルよりも、NCSS 展開モデルとの方が相性がいいということです。この例では、可変要素はクライシスですが、インスタンスが地域、日付、その他のさまざまな特性によってインスタンスが変化するテンプレートタイプの SharePoint 拡張機能を想像することは簡単です。
    
  
SharePoint アドイン の可能性を拡張する方法については、「 [アドイン イベント ハンドラの使用を最小限に抑える](#AppEventHandlers)」と「 [拡張機能を作成するアドイン](#ExtensionFactories)」を参照してください。
  
    
    

## アドイン向けの方法で行う
<a name="Questions"> </a>

前述のとおり、SharePoint アドインでは、SharePoint サーバーで実行するカスタム コードが許可されていません。これは重大な制限では *ありません*  。単に、カスタム ビジネス ロジックがクライアント デバイスに"落とされるか"、またはクラウドに"アップされるか"というだけです。どちらの場合も、 [SharePoint REST/OData サービス](http://msdn.microsoft.com/library/f60ed19e-9840-4f39-911e-4676751a2f6b%28Office.15%29.aspx) を使用して、SharePoint サイト、リスト、およびその他のデータにアクセスできます。また、 [SharePoint JavaScript、Silverlight、または .NET Framework クライアント オブジェクト モデル](http://msdn.microsoft.com/library/8c086b11-2b8b-41ec-82ae-cd4fef0aeac6%28Office.15%29.aspx)を介して SharePoint データにリモートでアクセスできます。最後に、Windows Phone では、SharePointWindows Phone オブジェクト モデルを介して SharePoint にアクセスできます。SharePoint 2013 のさまざまな API のセットについて詳しくは、「 [SharePoint 2013 での適切な API セットの選択](choose-the-right-api-set-in-sharepoint-2013.md)」を参照してください。
  
    
    
同様に、SharePoint アドインの機能には、サイト コレクション、Web アプリケーション、またはファームのスコープを含めることができません。しかし、ユーザー インターフェイス (UI) の一部の要素および機能をあきらめる必要はありません。つまり、コンポーネントの実装が SharePoint から離れて、クライアント、リモート Web アプリケーション、またはリモート データベースに移行したことを意味します。
  
    
    
次の表に、SharePoint アドインに展開できない SharePoint コンポーネントを示します。また、同じ機能を使用するための「アドイン向けの方法」を説明します。
  
    
    


|**必要な機能**|**使用する方法**|
|:-----|:-----|
|カスタム Web パーツ  <br/> |SharePoint アドインは、カスタム Web パーツを含むリモート ページを持つことができます。また、リモート Web アプリケーションからページを SharePoint サイト ページのアドイン パーツに公開することもできます。リモート ページには、Web パーツと本質的に同じ UI コントロールと機能があります。詳細については、「 [アドイン パーツを作成して SharePoint アドインと共にインストールする](http://msdn.microsoft.com/library/a2664289-6c56-4cb1-987a-22367fad55eb%28Office.15%29.aspx)」を参照してください。  <br/> |
|イベント レシーバーと機能レシーバー  <br/> |SharePoint アドインには、機能的に同等のリモート イベント ハンドラーを含めることができます。詳細については、「 [SharePoint アドインのイベントの処理](http://msdn.microsoft.com/library/c050d056-8548-4496-a053-016779d723d9%28Office.15%29.aspx)」を参照してください。  <br/> |
|カスタム フィールド (列) 型  <br/> |アドインは、 [既存のフィールド型](http://msdn.microsoft.com/ja-jp/library/microsoft.sharepoint.spfieldtype.aspx%28Office.15%29.aspx)の 1 つに基づく新しいフィールド (列) を展開することができます。 [集計](http://msdn.microsoft.com/library/8703373d-bb55-4132-8c5f-37a41c383f81%28Office.15%29.aspx)フィールド型と [計算](http://msdn.microsoft.com/ja-jp/library/microsoft.sharepoint.spfieldcomputed.aspx%28Office.15%29.aspx)フィールド型には、本質的に柔軟性があります。また、カスタマイズしたコントロールまたはグリッドを使用して、データをリモート Web ページに表示することもできます。  <br/> |
|SharePoint  [Service Application Framework](http://msdn.microsoft.com/library/6d0300d2-5b5c-4477-a9e2-17594aea5a7d%28Office.15%29.aspx) に構築されたカスタム Web サービス <br/> |カスタム Web サービスをリモート サービスとして開発できます。  <br/> |
|アプリケーション ページ  <br/> |SharePoint アドインには、アドインがインストールされるすべての Web サイトから利用できるリモート Web ページを含めることができます。また、アドインは、サイト ページで組み込み SharePoint Web パーツを使用することができます。  <br/> |
   
以下に示す一部の SharePoint コンポーネントは、エンドユーザー シナリオで使用されますが、SharePoint アドイン モデルには、同等のコンポーネントがなく、NCSS に展開することはできません。これらのコンポーネントが必要な場合は、ファーム ソリューション を使用する必要があります。
  
    
    

- **カスタム サイトの定義** しかし、カスタム WebTemplates (機能的にはサイト定義に似ています) は、NCSS と SharePoint アドイン の両方で使用できます。詳細については、「 [テンプレートおよび定義を使用する](http://msdn.microsoft.com/library/1edf6d4d-eddb-4cb5-9034-ed394e8a3e01%28Office.15%29.aspx)」を参照してください。
    
  
- **委任コントロール** 詳細については、「 [委任コントロール (コントロールのテンプレート化)](http://msdn.microsoft.com/library/e979328d-4985-4ed6-9085-7ff32a998dfc%28Office.15%29.aspx)」を参照してください。
    
  
- **ユーザー設定のテーマ**
    
  
- **カスタム アクション グループとカスタム アクションの非表示**
    
  
- **ユーザー コントロール (.ascx ファイル)** シナリオには必要ありません。
    
  

### アドイン イベント ハンドラの使用を最小限に抑える
<a name="AppEventHandlers"> </a>

アドインのインストール、アドインの更新、アドインのアンインストール イベント用のハンドラを作成することで、SharePoint アドイン の制限事項の一部を解決することができます。このようなハンドラは、SharePoint ファームの外側 (通常はクラウド) の Web サーバー上でホストされる Web サービスです。ハンドラは SharePoint クライアント オブジェクト モデル、または REST API を使って、ホスト Web のコンポーネントなどの SharePoint コンポーネント上で CRUD 操作を実行します。理論的には、このようなハンドラを使って、前に説明した「 **ブランド化** 」と「 **テンプレートタイプの拡張機能** 」の項目に関する展開時の制限事項を一部解決できます。ただし、このようなハンドラは、ユース ケースが必要とする機能をお客様に提供する手段が他にない場合の最後の手段としてのみ使うことをお勧めします。ハンドラを作成するかどうかを決定する場合、以下の項目を検討してください。
  
    
    

- プログラムを使ってホスト Web に展開するには、アドインでホスト Web へのフル コントロール権限が必要かどうか。このレベルの権限が必要なアドインは、Office ストア 経由では販売できません。
    
  
- ホスト Web にコンポーネントを展開すると、独自のドメインを備えたアドイン Web に SharePoint コンポーネントを配置するというセキュリティ上のメリットが損なわれる可能性があります。
    
  
- 拡張展開コードは、アドイン Web コンポーネントや NCSS で使用可能な説明型の展開マークアップと比較して、作成とデバッグに多くの工数が必要です。
    
  
- アドイン イベント ハンドラの作成では、従う必要がある重大なベスト プラクティスが存在します。エラーが発生した場合に、実行されたすべての処理を取り消すロールバック ロジックをコードに含める必要があります。また、実行した処理をインフラストラクチャがすべてロールバックできるように、エラーが発生した SharePoint インフラストラクチャに警告を発行する必要もあります。
    
  
- アドインのイベント ハンドラは、SharePoint ホスト型と呼ばれているタイプの SharePoint アドイン を処理できません。
    
  
アドイン イベント ハンドラの詳細については、SDK ノード  [SharePoint アドインのイベントの処理](http://msdn.microsoft.com/library/c050d056-8548-4496-a053-016779d723d9%28Office.15%29.aspx) をご覧ください。ロールバック ロジックについて詳しくは、「 [SharePoint アドインで更新イベント用のハンドラーを作成する](http://msdn.microsoft.com/library/0fa088c5-54c6-482c-84ed-51c4f77c4127%28Office.15%29.aspx) 」というトピックの「 **ハンドラへのロールバック ロジックの追加** 」セクションをご覧ください。後者のトピックは、アドインの更新イベントというコンテキストで記述されていますが、基本的な原則はすべてのアドイン イベント ハンドラに適用されます。
  
    
    

### 拡張機能を作成するアドイン
<a name="ExtensionFactories"> </a>

SharePoint クライアント オブジェクト モデル (またはその REST API) を使って、SharePoint アドイン に関するコンポーネント展開問題を解決するもう 1 つの方法は、アドイン イベント ハンドラにではなく、アドイン自体に CRUD コードを組み込むことです。次にアドインはカスタム拡張機能の一種のファクトリになります。たとえば、SharePoint ホスト型アドインは SharePointJavaScript オブジェクト モデルを使って、ホスト Web 上、テナントや Web アプリケーションなどで展開や他の CRUD 操作を実行することができます。別の例としては、「 [SharePoint 2013 サイトのプロビジョニング テクニックとリモート プロビジョニング](http://blogs.msdn.com/b/vesku/archive/2013/08/23/site-provisioning-techniques-and-remote-provisioning-in-sharepoint-2013.aspx)」の「 **リモート プロビジョニングへの簡単な紹介** 」セクションを参照してください。プロバイダ ホスト型の SharePoint アドイン を使って、インザボックス サブ Web プロビジョニングに非常によく似た SharePoint のサブ Web プロビジョニングを提供する方法が記載されています。ただし、無駄な労力が大量に発生するため、ファクトリ SharePoint アドイン の作成に大量の作業が必要です。さらに、このような種類のアドインは、ホスト Web のフル コントロールを必要とするため、Office ストア では販売できません。
  
    
    

## その他の技術情報
<a name="bk_addresources"> </a>


-  [SharePoint 2013 でのプログラミング モデル](programming-models-in-sharepoint-2013.md)
    
  
-  [ソリューションの使用](http://msdn.microsoft.com/library/0da0518c-24eb-48e0-89bd-21282fdeef94%28Office.15%29.aspx)
    
  

