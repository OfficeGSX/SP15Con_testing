---
title: Excel Services の開発ロードマップ
keywords: roadmap
f1_keywords:
- roadmap
ms.prod: OFFICE365
ms.assetid: 5c789f58-9cdb-4601-9047-9c6f83f2fbba
---


# Excel Services の開発ロードマップ

Excel Services の重要な側面は、ソリューション開発者がその能力をアプリケーションからプログラムで使えることです。これらのアプリケーションは、基幹業務 (LOB) 製品の場合もあれば、組織の内部で開発するカスタム企業ソリューションの場合もあります。 
  
    
    

これらのアプリケーションの例を次に示します。 
- プレゼンテーション層が Excel Web Services を呼び出す Web アプリケーションとして実装された多層アプリケーション (例: ASP.NET アプリケーション)
    
  
- Microsoft SharePoint Server 2010 内の、または LOB 製品と統合されたアプリケーション
    
  
Excel Services を使用して行える開発は 5 種類あります。
- Excel Web Services を使用してソリューションを開発する
    
  
- ユーザー定義関数 (UDF) を使用して Excel Services で Microsoft Excel 関数ライブラリを拡張する 
    
  
- Excel Web Access Web パーツをカスタマイズする
    
  
- ECMAScript (JavaScript, JScript) を使用してソリューションを開発する
    
  
- REST API を使用して Excel ブックの操作を行う
    
  

## Excel Web サービス

以下は、Excel Web Services の主なシナリオです。
  
    
    

- **Server-side Excel calculation**
    
    このシナリオはアプリケーション中心です。このシナリオでは、Excel ブックで定義され、アプリケーション ロジックの一部としてサーバー上で計算されるモデルを使用します。
    
  
- **Automating workbook updates on the server**
    
    このシナリオはファイル中心です。このシナリオでは、Excel Web Services がブックを処理し、ブックまたはスナップショットのコピーを保存します。
    
  
- **Opening workbooks in edit sessions**
    
    Excel Web Services は、SharePoint Server 2010 の編集セッションでブックを開くことをサポートしています。このシナリオでは、コードを使用してブックを編集します。
    
  

### サーバー側の Excel の計算

サーバー側の Excel の計算のため、一般に、カスタム アプリケーションはそのロジックの一部として Excel モデルを使用します。プログラミング言語で Excel ブックのビジネス ロジックを再コーディングする必要がある代わりに、ビジネス ユーザーは、サーバーの場所にある Excel でモデルを保守することができます。開発者は、ビジネス ユーザーが作成したモデルを使用するアプリケーション内のコード行を変更する必要はありません。
  
    
    
このシナリオでは、カスタム アプリケーションは、バックエンド計算サービスに呼び出しを送信する Excel Web Services を繰り返し呼び出します。Excel Calculation Services は以下を実行します。
  
    
    

- 指定された Excel ブックを読み込む 
    
  
- 入力を受け取る
    
  
- ブックを処理する (例: データの更新または計算の実行)
    
  
- 結果をカスタム アプリケーションに送信する
    
  

### サーバー上のブック更新を自動化する

開発者がサーバー上での Excel ブックの更新を自動化する際、多くの場合、2 つの目的があります。
  
    
    

- Open XML ファイル形式 を使用して Excel ファイルを生成したり、Excel テンプレートを変更してから、生成された Excel ファイルの計算を行います。
    
  
- Excel ファイルを定期的に開き、外部データを更新します (1 ユーザーあたり 1 回または複数回の場合もあります)。その後、結果となるブックを計算し、保存するか、電子メール メッセージに含めてさまざまなユーザーに送信します。
    
  
このシナリオでは、カスタム アプリケーションは Excel Web Services を使用して以下を行います。
  
    
    

- 指定された Excel ブックを読み込む 
    
  
- パラメーターを入力する
    
  
- ブックを処理する (例: データの更新または計算の実行) 
    
  
カスタム アプリケーションは、ブックやスナップショットのライブ バージョンを取得してから、Excel Web Services を使用してブックやスナップショットを保存します。
  
    
    

> **メモ**
> ブックの変更を行う (たとえば、Excel Web Services を使用して値を範囲に対して設定する) 場合、ブックに対する変更はその特定のセッションでのみ保持されます。保存されたり、元のブックに保存されることはありません。現在のブックのセッションが終了する際 (例: **CloseWorkbook** メソッドを呼び出したとき、またはセッションがタイム アウトになったとき)、行われた変更は失われます。<br/> ブックに対して行った変更を保存する場合は、 **GetWorkbook** メソッドを使用してから、 **SaveWorkbook** メソッドまたは **SaveWorkbookCopy** メソッドを使用してブックを保存します。Excel Web Services API について、詳細は「 [Microsoft.Office.Excel.Server.WebServices](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.WebServices.aspx) 」を参照してください。
  
    
    


### Excel Web Services の使用

Excel Web Services は以下に使用できます。
  
    
    

- 通常の Web サービス。SOAP over HTTP を介して Web メソッドを呼び出します。
    
  
- ローカル アセンブリ。 **Microsoft.Office.Excel.Server.Webservices.dll** と直接リンクします。
    
  
 **Microsoft.Office.Excel.Server.Webservices.dll** に直接リンクする必要がある場合について、詳細は「 [SOAP 呼び出しのループバックおよび直接リンク](loop-back-soap-calls-and-direct-linking.md)」を参照してください。 
  
    
    
Excel Web Services API について、詳細は  [Microsoft.Office.Excel.Server.Webservices](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.Webservices.aspx) 名前空間のリファレンス ドキュメントを参照してください。Excel Web Services を使用したカスタム アプリケーションの開発方法の例については、「 [チュートリアル: Excel Web Services を使用してカスタム アプリケーションを開発する](walkthrough-developing-a-custom-application-using-excel-web-services.md)」を参照してください。
  
    
    

## ユーザー定義関数 (UDF)

Excel Services は、マネージド コード UDF をサポートしています。Excel Services UDF は、セル内の数式を使用して、マネージド コードで記述され、SharePoint Server 2010 に配置されたカスタム関数を呼び出す機能を提供します。UDF は次の目的で作成できます。
  
    
    

- カスタムの数学関数を呼び出す
    
  
- カスタム データ ソースからデータを取得してワークシートに表示する
    
  
- UDF から Web サービスを呼び出す
    
  
- 既存のネイティブ コード ライブラリの関数の呼び出しをラップする (既存の Excel UDF など)
    
  
Excel Services UDF について、詳細は「 [Excel Services UDF の理解](understanding-excel-services-udfs.md)」を参照してください。 
  
    
    

### UDF の使用

Excel Services UDF の定義について、詳細は  [Microsoft.Office.Excel.Server.Udf](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.Udf.aspx) 名前空間のリファレンス ドキュメントを参照してください。
  
    
    
マネージド コード UDF の作成方法の例については、「 [チュートリアル: マネージ コード UDF を開発する](walkthrough-developing-a-managed-code-udf.md)」を参照してください。
  
    
    

## Excel Web Access

Excel Web Access Web パーツの拡張可能なプロパティは、次の目的に使用できます。
  
    
    

- プログラムで Excel Web Access を構成する
    
  
- プログラムで Excel Web Access のプロパティを変更する
    
  
- テーマを適用したり、カスケード スタイル シート (CSS) を使用して Web パーツ ページをブランド化する
    
  

### Excel Web Access の Web Part 機能拡張の使用

以下について、詳細はそれぞれの参照先を参照してください。
  
    
    

- Excel Web Access の拡張プロパティについては、 [Microsoft.Office.Excel.Server.WebUI](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.WebUI.aspx) 名前空間のリファレンス ドキュメントを参照してください。
    
  
- Excel Web Access CSS については、CSS のリファレンス ドキュメントを参照してください。
    
  
- プログラムで Web パーツを構成する方法は、SharePoint Foundation SDK を参照してください。 
    
  

## ECMAScript (JavaScript、JScript)

SharePoint Server 2010 において、Excel Services では JavaScript のサポートが追加されました。Excel Services の JavaScript オブジェクト モデルにより、開発者は、ページ上で Excel Web Access Web パーツのコントロールの自動化、カスタマイズ、操作ができるようになります。JavaScript オブジェクト モデルを使用すると、ページ上で 1 つ以上の Excel Web Access Web パーツのコントロールと対話するマッシュアップおよびその他の統合ソリューションを構築できます。また、ブックとその周囲のコードに多くの機能を追加することもできます。
  
    
    
Excel Services の JavaScript オブジェクト モデルについて、詳細は  [Ewa](http://msdn.microsoft.com/library/6fe73191-3213-b986-1ad6-2c3b918a2241%28Office.15%29.aspx) 名前空間のリファレンス ドキュメントを参照してください。
  
    
    

### ECMAScript (JavaScript、JScript) の使用

JavaScript について、詳細は次のリンクを参照してください。
  
    
    

- Excel Services の JavaScript オブジェクト モデルについて、詳細は  [Ewa](http://msdn.microsoft.com/library/6fe73191-3213-b986-1ad6-2c3b918a2241%28Office.15%29.aspx) 名前空間のリファレンス ドキュメントを参照してください。
    
  
- コンテンツ エディター Web パーツを使用して Excel Services で JavaScript オブジェクト モデルを操作する方法の例については、「 [チュートリアル: コンテンツ エディターと Web パーツを使用した開発](walkthrough-developing-using-the-content-editor-web-part.md)」を参照してください。
    
  

## REST API

Excel Services の REST API は SharePoint Server 2010 の新機能です。REST API を使用すると、URL を介して直接ブックのパーツや要素にアクセスできます。 
  
    
    
また、Excel Services の REST API に組み込まれた探索メカニズムでは、開発者とユーザーは、特定のブックに存在する要素に関する情報を含む Atom フィードを供給することで、手動またはプログラム上でブックの内容を探索できます。REST API を介してアクセスできるリソースは、範囲、図、表、ピボットテーブルです。 
  
    
    
REST API が提供する Atom フィードを使用すると、関心があるデータを簡単に取得できるようになります。フィードには、ブックにどのような要素が存在するかをコードが検出できるようにするスキャン可能な要素が含まれます。 
  
    
    
詳細は、「 [Excel Services の REST API](excel-services-rest-api.md)」を参照してください。
  
    
    

### REST API の使用

以下について、詳細はそれぞれの参照先を参照してください。
  
    
    

- REST サービスへのアクセス、および Excel Services における REST サービスのサンプル URI の表示については、「 [Excel Services REST API へのアクセス](sample-uri-for-excel-services-rest-api.md)」を参照してください。
    
  
- Excel Services における REST サービスのスキーマへのアクセスについては、「 [スキーマへのアクセス](accessing-a-schema.md)」を参照してください。
    
  

## 関連項目


#### タスク


  
    
    
 [プログラムを使用して Excel Web Access の Web パーツをページに追加する方法](how-to-programmatically-add-an-excel-web-access-web-part-to-a-page.md)
#### 概念


  
    
    
 [Excel Services の概要](excel-services-overview.md)
  
    
    
 [Excel Services のアーキテクチャ](excel-services-architecture.md)
  
    
    
 [サポートされる機能とサポートされない機能](supported-and-unsupported-features.md)
  
    
    
 [Excel Services ブログ、フォーラム、リソース](excel-services-blogs-forums-and-resources.md)
#### その他の技術情報


  
    
    
 [チュートリアル: Excel Web Services を使用してカスタム アプリケーションを開発する](walkthrough-developing-a-custom-application-using-excel-web-services.md)
