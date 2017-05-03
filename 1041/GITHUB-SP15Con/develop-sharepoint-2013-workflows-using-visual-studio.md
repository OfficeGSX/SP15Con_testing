---
title: Visual Studio を使用した SharePoint 2013 ワークフローの開発
ms.prod: SHAREPOINT
ms.assetid: 28f5d3b1-6fe8-4b1f-8c4e-b11105fe6f46
---



# Visual Studio を使用した SharePoint 2013 ワークフローの開発
SharePoint 2013 では、ワークフローを作成するために、SharePoint Designer と Visual Studio の 2 つの主要なワークフロー開発環境をサポートしています。この記事では、この 2 つのツールの概要とそれぞれの利点と欠点について説明します。
## SharePoint ワークフロー作成の基本事項
<a name="bkm_AuthoringBasics"> </a>


> **メモ**
> Microsoft SharePoint Server 2013 および ワークフロー マネージャー クライアント 1.0 サーバーのセットアップと構成方法については、「 [SharePoint 2013 ワークフロー マネージャーをセットアップおよび構成する](set-up-and-configure-sharepoint-2013-workflow-manager.md)」を参照してください。 
  
    
    

以前のバージョンと同様に、Microsoft SharePoint 2013 には、ワークフロー作成のための Microsoft SharePoint Designer と Microsoft Visual Studio の 2 つの主なワークフロー開発環境が用意されています。ただし、Visual Studio の使用においてコード ベースの作成戦略が提供されていないところが以前のバーションとは異なります。代わりに、選択する開発ツールを問わず SharePoint Designer と Visual Studio の両方に完全に宣言型のコード不要の作成環境が用意されています。
  
    
    

> **メモ**
> SharePoint Designer でのワークフローの作成の補完として、Microsoft Visio 2013 で Visio 2013 の図形を使用してワークフロー ロジックを構造化し、ロジックを SharePoint Designer 2013 にインポートできます。Visio 2013 を使用してワークフロー ロジックを作成する方法については、「 [SharePoint Designer および Visio でのワークフロー開発](workflow-development-in-sharepoint-designer-and-visio.md)」を参照してください。 
  
    
    


### 宣言型ワークフロー

まず "宣言型" ワークフローとは何かを明確にします。この用語は、ワークフローをコードで作成しマネージ アセンブリにコンパイルする代わりに、ワークフローを XAML で (逐語的に) 記述し、実行時に解釈しながら実行することを意味しています。
  
    
    
XAML は、ワークフロー ​​デザイナー (Visual Studio を使用した場合) または SharePoint Designer のワークフロー デザイン画面 (または Visio、後で詳しく説明) で操作するワークフロー構成要素から派生 (推定) します。構成要素自体はデザイナー ツールボックス内の視覚的なワークフロー デザイン オブジェクト (ステージ、条件、アクション、イベントなど) です。それぞれのツールボックス (Visual Studio または SharePoint Designer) のツールのセットは若干異なりますが、宣言型のワークフローの概念は同じです。
  
    
    

## 決定ツリー: SharePoint Designer と Visual Studio
<a name="bkm_DecisionTree"> </a>

SharePoint 2013 のワークフロー フレームワークの最も大きい利点は、インフォメーション ワーカーが SharePoint Designer のコード不要な環境を使用して豊富で強力なワークフローを簡単に作成できることです。さらに、Visual Studio などの宣言型の作成環境で柔軟性に富んだカスタマイズが可能です。
  
    
    
これらのワークフロー作成環境 (SharePoint Designer および Visual Studio) のいずれも特定の利点と欠点があります。このセクションでは、ワークフロー開発ニーズに最も適した作成環境がどちらであるかを判断する方法について説明します。
  
    
    

### SharePoint Designer の使用


- **対象ユーザー:** インフォメーション ワーカー、ビジネス アナリスト、SharePoint 開発者
    
  
- **難易度:** ステージ、ゲート、アクション、条件、ループなどのコア ワークフロー コンポーネントを含む SharePoint Designer の知識。
    
  
SharePoint Designer を使用すると、ユーザーはコード不要のテキストベース デザイナーを使用してリスト、ライブラリ、またはサイトに付加されたワークフローを作成できます。または、ビジネス プロセスの論理フローを表すためにデザイン画面にグラフィカル要素が並べられた新しい視覚的な設計環境を使用することができます。SharePoint Designer は非技術者のワークフロー開発を高速に行えるようにできる点で優れています。
  
    
    

### Visual Studio の使用


- **対象ユーザー:** 中級または上級のソフトウェア開発者。
    
  
- **難易度:** イベント レシーバー、パッケージ化と展開、セキュリティなどのソフトウェア開発概念を含む Visual Studio の知識。
    
  
Visual Studio でワークフローを作成すると、複雑さにかかわらずビジネス プロセスを実質的にサポートするワークフローを作成できる柔軟性が得られ、ワークフロー定義のデバッグや再利用ができるようになります。最も重要と考えられる Visual Studio を使用して、開発者は幅広い SharePoint ソリューションまたは SharePoint アドイン の一部として SharePoint ワークフローを含めることができます。
  
    
    
Visual Studio では、開発者は SharePoint Designer で使用されるカスタム アクションを作成でき、カスタム ロジックを実行する手段が提供されます。Visual Studio では、開発者は複数のサイトに展開できるワークフロー テンプレートを作成することもできます。
  
    
    

## SharePoint Designer と Visual Studio の比較
<a name="bkm_Comparing"> </a>

次の表に、SharePoint ワークフローの作成に SharePoint Designer と Visual Studio を使用した場合の機能と要件の対照比較を示します。
  
    
    

**表 1. ワークフロー作成ツールの比較**


|**機能/要件**|**SharePoint Designer**|**Visual Studio**|
|:-----|:-----|:-----|
|迅速なワークフロー開発が可能  <br/> |はい  <br/> |はい  <br/> |
|ワークフローの再利用が可能  <br/> |ワークフローは、開発されたリストまたはライブラリによってのみ使用できます。ただし、SharePoint Designer では同じサイト内で複数回使用できる再利用可能なワークフローが提供されます。  <br/> |ワークフローは、展開された後に再利用され、リストまたはライブラリと関連付けができるように、テンプレートとして記述できます。  <br/> |
|ワークフローを SharePoint ソリューションまたは SharePoint アドイン の一部として含めることが可能  <br/> |いいえ  <br/> |はい  <br/> |
|カスタム アクションの作成が可能  <br/> |いいえ。ただし、SharePoint Designer は Visual Studio を使用して作成および展開されたカスタム アクションを使用および実装できます。  <br/> |はい。ただし、Visual Studio では、対応するアクションではなく、基になるアクティビティが使用されます。  <br/> |
|カスタム コードの記述が可能  <br/> |いいえ  <br/> |いいえ  <br/> <br/> **メモ**<br/> これは以前のバージョンから変更されています。SharePoint 2013 では、ワークフローは宣言型のみで、Visual Studio はワークフロー開発用の視覚的なデザイン画面に依存します。           |
|ワークフロー ロジックの作成に Visio Professional を使用可能  <br/> |はい  <br/> |いいえ  <br/> |
|展開  <br/> |作成されたリスト、ライブラリ、サイトに自動的に展開されます。  <br/> |SharePoint ソリューション パッケージ (.wsp) ファイルを作成し、このソリューション パッケージをサイト (SPWeb) に展開します。  <br/> |
|ワークフローのワンクリック発行が可能  <br/> |はい  <br/> |はい  <br/> |
|ワークフローをパッケージ化してリモート サーバーへ展開可能  <br/> |はい  <br/> |はい  <br/> |
|デバッグ  <br/> |デバッグはできません。  <br/> |ワークフローは Visual Studio を使用してデバッグできます。  <br/> |
|サイト管理者に承認されたアクションのみ使用可能  <br/> |はい  <br/> |はい  <br/> <br/> **メモ**<br/> これは以前のバージョンから変更されています。以前は、Visual Studio を使用して作成されたワークフローおよびアクションがコードベースであり、ファーム スコープで展開されていたため、管理者の承認は不要でした。           |
   

## Visual Studio を使用したワークフローの開発
<a name="bkm_Developing"> </a>

以前のバージョンとは異なり、SharePoint 2013 のワークフローは完全に宣言型です。今回、Windows Workflow Foundation 4 上に構築された Visual Studio には、カスタム ワークフロー、ワークフロー テンプレート、フォーム、カスタム ワークフロー アクティビティ全体をデザイナー環境で作成できる視覚的なワークフロー デザイナー画面が用意されています。ワークフローは、その後パッケージ化され SharePoint フィーチャーとして展開されます。フィーチャーのパッケージ化については、「 [SharePoint Foundation でのフィーチャーの使用](http://msdn.microsoft.com/ja-jp/library/ms461324.aspx)」を参照してください。
  
    
    
おそらく、Visual Studio 開発者にとって最も大きな変更点は、カスタム ワークフローが .NET Framework アセンブリとしてコンパイルおよび展開されなくなったことです。また、SharePoint 2013は Microsoft InfoPath フォームを使用しなくなり、代わりにフォーム生成は Microsoft ASP.NET フォームに依存しています。 
  
    
    
さらに、Visual Studio のワークフロー プロジェクト テンプレートが変更されています。以前はステート マシンおよびシーケンシャル ワークフロー用のテンプレートが用意されていましたが、これらの区別にはもう意味がありません。代わりに、仮想マシン (VM) 上に用意された Visual Studio ビルドで Visual Studio プロジェクト テンプレートを使用できます。
  
    
    

## 社内ワークフロー デバッグを有効にする
<a name="bkm_Enabling"> </a>

社内ワークフローを Visual Studio でデバッグするには、一時的にワークフロー マネージャー ツールがファイアウォールを通過してシステムにアクセスすることを許可する必要があります。
  
    
    

1. [ **コントロール パネル**] で、[ **システムとセキュリティ**]、[ **Windows ファイアウォール**] の順に選択します。
    
  
2. [ **コントロール パネル ホーム**] リストにある [ **詳細設定**] リンクを選択します。
    
  
3. Windows ファイアウォールの左ウィンドウにある [ **受信の規則**] を選択します。
    
  
4. [ **受信の規則**] リストで、[ **Workflow Manager Tools 1.0 for Visual Studio 2012 - Test Service Host**] を選択します。
    
  
5. [ **操作**] リストで、[ **規則の有効化**] を選択します。
    
  
6. SharePoint プロジェクトのプロパティ ページで、[ **SharePoint**] タブを選択し、[ **ワークフロー デバッグの有効化**] チェック ボックスを選択します。
    
  

## Visual Studio を使用して SharePoint Online ワークフローをデバッグする
<a name="bkm_Debug"> </a>

Visual Studio で SharePoint Online ワークフローをデバッグするには、以下のステップを実行します。
  
    
    

1. ユーザーがファイアウォールの内側にいる場合、会社のネットワーク トポロジによっては、プロキシ クライアント ( [Forefront Threat Management Gateway (TMG) クライアント](http://www.microsoft.com/ja-jp/download/details.aspx?displaylang=en&amp;id=10504)など) のインストールが必要になる場合があります。
    
  
2. まだ登録を行っていない場合は Microsoft Azure アカウントに登録してから、そのアカウントにサインインします。
    
    Microsoft Azure アカウント登録の詳細については、「 [Microsoft Azure](http://www.windowsazure.com)」を参照してください。
    
  
3. Microsoft Azure Service Bus 名前空間を作成します。これを使用して、リモート ワークフローをデバッグできます。この作業は  [Microsoft Azure 管理ポータル](http://manage.windowsazure.com)上で行えます。
    
    Microsoft Azure Service Bus の詳細については、「 [Service Bus Service 名前空間の管理](http://msdn.microsoft.com/ja-jp/library/windowsazure/hh690928.aspx)」をご覧ください。
    
    > **メモ**
      > SharePoint Online ワークフローのデバッグでは Microsoft Azure Service Bus の Relay Service コンポーネントを使用するので、Service Bus の使用料金がかかります。「 [Service Bus の料金に関する FAQ](http://msdn.microsoft.com/library/hh667438.aspx)」を参照してください。Visual Studio Professional、Visual Studio Premium、または Visual Studio Ultimate を MSDN でサブスクライブしている月は、Microsoft Azure に無料でアクセスできます。このアクセスでは、MSDN のサブスクリプションに応じて、Service Bus リレーを 1,500、3,000、または 3,000 時間使用できます。「 [一部の Microsoft Azure サービスを毎月追加料金なしで利用する](http://www.windowsazure.com/ja-jp/pricing/member-offers/msdn-benefits/)」を参照してください。 
4. Microsoft Azure で、サービス名前空間を選択し、[ **アクセス キー**] リンクを選択し、[ **接続文字列**] ボックスのテキストをコピーします。
    
  
5. SharePoint アドイン プロジェクトのプロパティ ページで、[ **SharePoint**] タブを選択し、[ **ワークフロー デバッグの有効化**] チェック ボックスを選択します。
    
    SharePoint Online のワークフローをデバッグするには、この機能を有効にする必要があります。このプロパティは、Visual Studio 内にある自分のすべての SharePoint プロジェクトに適用されます。Office ストアで配布できるようにアプリをパッケージ化すると、Visual Studio はワークフローのデバッグを自動でオフにします。
    
  
6. [ **Microsoft Azure Service Bus によるデバッグを有効にする**] チェック ボックスを選択します。次いで、[ **Microsoft Azure Service Bus 接続文字列**] ボックスに、先ほどコピーした接続文字列を貼り付けます。
    
  
ワークフロー デバッグを有効にして、Microsoft Azure Service Bus に有効な接続文字列を指定した後、SharePoint Online ワークフローをデバッグできます。
  
    
    

> **メモ**
> ワークフロー デバッグを無効にしておらず、プロジェクトにワークフローが含まれている間は通知を受信しないようにする場合は、[ **Microsoft Azure Service Bus デバッグが構成されていない場合は通知する**] チェック ボックスのチェックを解除します。 
  
    
    


## その他の技術情報
<a name="workflowbk_addres"> </a>

SharePoint ワークフローの大部分は、Visual Studio 開発者に対しては変更されていません。SharePoint 2010 のドキュメントの主要なセクションはまだ該当します。
  
    
    

- Visual Studioワークフロー ​​デザイナー の使用方法については、「 [Visual Studio Designer for Windows Workflow Foundation の概要](http://msdn.microsoft.com/ja-jp/library/ms441543.aspx)」を参照してください。
    
  
- Microsoft ASP.NET を使用したフォームの作成方法については、「 [ワークフロー フォームの概要](http://msdn.microsoft.com/ja-jp/library/ms457061.aspx)」を参照してください。
    
  
- フィーチャーのパッケージ化については、「 [SharePoint Foundation でのフィーチャーの使用](http://msdn.microsoft.com/ja-jp/library/ms461324.aspx)」を参照してください。
    
  
