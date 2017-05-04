---
title: SharePoint Designer 2013 での変更点
ms.prod: SHAREPOINT
ms.assetid: f1cd30c9-9a73-428d-9151-f1813b43b020
---


# SharePoint Designer 2013 での変更点
SharePoint Designer 2013 では使用されていない機能や削除されている機能について説明します。使用されていない機能が SharePoint 2013 リリースに含まれているのは、以前の製品バージョンとの互換性のためですが、これらの機能は将来のバージョンでは削除されます。
## SharePoint Designer 2013 の廃止された機能
<a name="WhatsChangedSharePointDesigner2013_DiscontinuedFeatures"> </a>

次の機能は、SharePoint Designer 2013では使用されていないか、削除されています。
  
    
    

### SharePoint 2010 ワークフロー プラットフォーム
<a name="WhatsChangedSharePointDesigner2013_WorkflowPlatform"> </a>

 **変更の説明:** Windows Workflow Foundation 3.0 に依存している SharePoint 2010 ワークフロー プラットフォームは、SharePoint 2013では使用されていません。
  
    
    
 **変更の理由:** SharePoint 2013には、新しい SharePoint 2013 ワークフロー プラットフォームが導入されています。このプラットフォームは、Windows Workflow Foundation 4.0 上に構築され、ワークフロー マネージャー 1.0 と統合されています。
  
    
    
 **移行パス:** SharePoint Designer 2013では、SharePoint 2010 ワークフロー プラットフォームを選択することで、依然として SharePoint 2010 ワークフローを作成したり、すべての SharePoint 2010 ワークフロー機能を使用したりできます。
  
    
    
また、SharePoint 2010 ワークフロー プラットフォームの機能を新しい SharePoint 2013 ワークフロー プラットフォームに組み込むこともできます。そのためには、SharePoint 2010 ワークフロー プラットフォームを選択して、SharePoint 2010 ワークフローを作成します。続いて、SharePoint 2013 ワークフローの [ **リストのワークフローを開始する**] アクションと [ **サイトのワークフローを開始する**] アクションを使用して、SharePoint 2010 ワークフローを呼び出します。 
  
    
    
以下の機能は、SharePoint 2010 ワークフロー プラットフォームでのみ使用できます。
  
    
    

- アクション:
    
  - ワークフローを停止する
    
  
  - ドキュメント セットのバージョンを取得する
    
  
  - ドキュメント セットをリポジトリに送る
    
  
  - ドキュメント セットのコンテンツ承認状態を設定する
    
  
  - ドキュメント セット承認処理を開始する
    
  
  - レコードを宣言する
    
  
  - コンテンツの承認状態を設定する
    
  
  - レコード宣言を解除する
    
  
  - リスト アイテムを追加する 
    
  
  - リスト アイテムの親のアクセス許可を継承する
    
  
  - リスト アイテムの権限を削除する
    
  
  - リスト アイテムの権限を置換する
    
  
  - ユーザーの上司を検索する
    
  
  - フォームをグループに割り当てる
    
  
  - To Do アイテムを割り当てる
    
  
  - ユーザーからデータを収集する
    
  
  - 承認処理を開始する
    
  
  - ユーザー設定タスク処理を開始する
    
  
  - フィードバック処理を開始する
    
  
  - リスト アイテムをコピーする (SharePoint Designer 2013ではドキュメントをコピーするアクションのみをサポートしています)
    
  
- 条件:
    
  - 現在のアイテム フィールドが値に等しい場合
    
  
  - リスト アイテムのアクセス許可レベルをチェックする
    
  
  - リスト アイテムの権限をチェックする
    
  
- 手順:
    
  - 偽装の手順:
    
  
- データ ソース:
    
  - ユーザー プロファイル検索
    
  
- その他の機能:
    
  - Visio との統合
    
  
  - 関連付け列
    
  
  - 再利用可能なワークフロー用のコンテンツ タイプの関連付け
    
  
  - リスト/サイト ワークフロー用の 'リスト/Web の管理権限の要求' 機能
    
  
  - グローバルに再利用可能なワークフローの種類
    
  
  - ワークフロー ビジュアライゼーションのオプション
    
  

### SharePoint のページデザイン機能
<a name="WhatsChangedSharePointDesigner2013_PageDesignFeatures"> </a>

以下の機能は、SharePoint 2013では使用できません。
  
    
    

#### デザイン ビューと分割ビュー
<a name="WhatsChangedSharePointDesigner2013_DesignViewSplitView"> </a>

 **変更の説明:** SharePoint Designer 2010 には、HTML および ASPX ページを編集するためのビューとして、コード ビュー、デザイン ビュー、分割ビューの 3 つがあります。デザイン ビューと分割ビューは、SharePoint Designer 2013から削除されています。デザイン ビューと分割ビューの削除は、Web パーツやマスター ページを編集するために使用される SharePoint Designer 2013の機能に影響します。SharePoint Designer 2013でページを編集する場合は、コード ビューを使用する必要があります。
  
    
    
 **変更の理由:** 現在のバージョンの Internet Explorer に比べると、デザイン ビューは新しい HTML5 および CSS タグの多くをサポートしていない旧式のテクノロジです。
  
    
    
 **移行パス:** コード ビューでページを編集している場合は、 **F12** キーを押すと、ページのプレビューがブラウザーに表示されます。または、Visual Studio を使用してページを編集することもできます。
  
    
    
視覚的なデザインやサイトのブランド設定が必要で、WYSIWYG ("What You See Is What You Get") 方式のページ編集エクスペリエンスが求められている場合は、任意のプロフェッショナル HTML エディター (Microsoft Expression Web など) を使用できます。その後、新しいデザイン マネージャーを使用して HTML ファイルを SharePoint Server 2013にインポートできます。このデザイン マネージャーは、発行ポータル サイト コレクション サイト テンプレートのような発行サイトに含まれる機能です。
  
    
    

## その他の技術情報
<a name="WhatsChangedSharePointDesigner2013_AdditionalResources"> </a>


-  [ワークフロー アクション クイック リファレンス (SharePoint 2013 ワークフロー プラットフォーム)](workflow-actions-quick-reference-sharepoint-2013-workflow-platform.md)
    
  
-  [SharePoint 2010 から SharePoint 2013 への変更点](http://technet.microsoft.com/ja-jp/library/ff607742%28office.15%29.aspx)
    
  
-  [ワークフローの新機能 (SharePoint Server 2013)](http://technet.microsoft.com/ja-jp/library/jj219638%28office.15%29.aspx)
    
  

  
    
    

