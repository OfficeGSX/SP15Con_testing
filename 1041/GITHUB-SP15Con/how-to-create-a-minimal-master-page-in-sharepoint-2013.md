---
title: SharePoint 2013 での最小限のマスター ページを作成する方法
ms.prod: SHAREPOINT
ms.assetid: 634aa471-07e1-41d6-aa80-27f7ef7e9dc8
---


# SharePoint 2013 での最小限のマスター ページを作成する方法
最小限のマスター ページには、SharePoint 2013 がブラウザー内でページを正しく表示するのに必要なページ要素のみが含まれます。デザイン マネージャーを使用すれば、最初に HTML ファイルをデザインして変換する必要がなく、最小限のマスター ページを迅速に作成できます。
## 最小限のマスター ページの概要
<a name="Introduction"> </a>

デザイン マネージャーを使用すると、一般的な HTML ファイルを SharePoint 2013 マスター ページに変換できます。ただし、事前に組み込まれたモックアップがない場合は、最小限のマスター ページを作成することにより、ゼロからでも迅速に開始することができます。最小限のマスター ページには、SharePoint がブラウザー内でページを表示するのに必要なページ要素のみが含まれます。
  
    
    
最小限のマスター ページを作成すると、デザイン マネージャーによって .master ファイルと、関連付けされた HTML ファイルの両方が作成されます。したがって、引き続き HTML ファイルのみを操作することもできます。最小限のマスター ページを操作することは、HTML ファイルから変換されたマスター ページを操作するのとまったく同じことです。HTML ファイルとマスター ページが関連付けられるので、HTML ファイルを編集して保存すると必ず、変更内容は関連付けられたマスター ページと同期されます。 さらに、HTML ファイルには, .master ファイルとの同期を可能にする特殊な種類のマークアップが含まれます。この関連付けと、マップアップの該当する種類の詳細については、「 [方法: SharePoint 2013 で HTML ファイルをマスター ページに変換する](how-to-convert-an-html-file-into-a-master-page-in-sharepoint-2013.md)」を参照してください。
  
    
    
以下の場合は、最小限のマスター ページから始めるのが便利です。
  
    
    

- モックアップ HTML ファイルから始めるのでなく、ゼロからすばやく開始し、最小限のマスター ページに関連付けられた HTML ファイルでデザインを構築する場合。
    
  
- 作業用の SharePoint マスター ページが必要なデザイン要素を迅速にテストまたはプロトタイプ化する場合。たとえば、最小限のマスター ページを作成すれば、変換用の HTML を用意したり、HTML ファイル内の有効でないマークアップが原因で発生したプレビュー エラーを解決したりする必要はありません。つまり、サーバー側のプレビューまたはスニペット ギャラリー をすぐに操作できるということです。
    
  
- .master ファイルを直接操作する場合。ASP.NET 開発者または SharePoint 開発者は、最小限のマスター ページを作成し、HTML ファイルのプロパティの [ **関連付けられているファイル**] チェック ボックスをオフにして HTML ファイルと .master ファイルとの関連付けを削除し, .master ファイルを直接操作することができます。
    
  

## 最小限のマスター ページを作成する
<a name="CreateMinimalMaster"> </a>


  
    
    

### 最小限のマスター ページを作成するには


1. 発行サイトに移動します。
    
  
2. ページの右上隅で [ **設定**] を選択し、[ **デザイン マネージャー**] を選択します。
    
  
3. デザイン マネージャーの左側のナビゲーション ウィンドウで、[ **マスター ページの編集**] を選択します。
    
  
4. [ **最小限のマスター ページの作成**] を選択します。
    
  
5. [ **マスター ページの作成**] ダイアログ ボックスで、マスター ページの名前を入力し、[ **OK**] をクリックします。
    
    この時点で, .master ファイルと、同じ名前を持つ関連付けられた HTML ファイルの両方が SharePoint によってマスター ページ ギャラリーに作成されます。
    
    デザイン マネージャーに、HTML ファイルが表示され、[状態] 列に [ **正常に変換されました**] と表示されます。
    
  
6. [状態] 列をクリックして、ファイルをプレビューします。
    
    プレビュー ページは、マスター ページの現在のサーバー側プレビューです。
    
    さまざまなページを持つマスター ページのプレビューの詳細については、「 [方法: SharePoint 2013 デザイン マネージャーでプレビュー ページを変更する](how-to-change-the-preview-page-in-sharepoint-2013-design-manager.md)」を参照してください。
    
    プレビュー ページの右上隅には [ **スニペット**] リンクもあります。このリンクをクリックすると、スニペット ギャラリーが開きます。ここで、デザイン内の静的なコントロールまたはモックアップ コントロールを動的な SharePoint コントロールに置換できます。詳細については、「 [SharePoint 2013 デザイン マネージャー スニペット](sharepoint-2013-design-manager-snippets.md)」を参照してください。
    
    マスター ページが正常にプレビューされた後は、HTML ファイルに **<div>** タグが追加されています。 **<div>** タグを表示するには、ページ下部までスクロールしなければならないことがあります。
    
    この **<div>** はメイン コンテンツ ブロックで、 **ContentPlaceHolderMain** というコンテンツ プレースホルダー内に置かれます。実行時に閲覧者がサイトを参照してページを要求すると、このコンテンツ プレースホルダーには、一致するコンテンツ領域内にコンテンツを含むページ レイアウトのコンテンツが設定されます。この **<div>** は、マスター ページ上にページ レイアウトを表示させる場所に位置付ける必要があります。
    
  
7. サーバー上に直接置かれている HTML ファイルは編集できます。それには、HTML エディターを使用して、マッピングされたドライブ内の HTML ファイルを開いて編集します。HTML ファイルを保存するたびに、変更内容が関連付けられた .master ファイルと同期されます。詳細については、「 [[方法]: SharePoint 2013 マスター ページ ギャラリーへのネットワーク ドライブの割り当て](how-to-map-a-network-drive-to-the-sharepoint-2013-master-page-gallery.md)」を参照してください。
    
  
8. HTML ファイルは操作せずに, .master ファイルのみを操作するには、2 つのファイル間の関連付けを削除する必要があります。デザイン マネージャーの [マスター ページを編集する] ページで、HTML ファイルを選択し、[ **プロパティ**] メニューを開き、[ **プロパティの編集**] を選択します。[ **編集**] タブで、[ **関連付けられているファイル**] チェック ボックスをオフにし、[ **保存**] を選択します。
    
    関連付けを削除すれば, .master ファイルを直接操作して変更内容を保存することができ、HTML ファイルに変更を加えてもその内容は上書きされません。この関連付けはいつでも復元できます。関連付けを復元すると、関連付けられた HTML ファイルが .master ファイルに同期してそれを上書きします。
    
  

## 関連付けられた HTML ファイルを理解する
<a name="UnderstandHTML"> </a>

最小限のマスター ページを作成すると, .master ファイルに関連付けられた HTML ファイルが作成されます。この HTML ファイルには SharePoint 2013 の仕組みに固有のマークアップ行が多数含まれます。このマークアップの大部分は無視しても問題はなく、ブラウザーでソースを表示したときにサイトの最終的なマークアップには表示されませんが、HTML ファイルを SharePoint 2013 が実際に使用する .master ファイルに変換する上では、このマークアップは重要です。HTML ファイルに対する変更を保存するたびに、この SharePoint マークアップにより、関連付けられた .master ファイルに対してバックグラウンドで同じ変更を行うことができます。 詳細については、「 [方法: SharePoint 2013 で HTML ファイルをマスター ページに変換する](how-to-convert-an-html-file-into-a-master-page-in-sharepoint-2013.md)」に示されているマークアップのサンプルを参照してください。
  
    
    

## その他の技術情報
<a name="Additional"> </a>


-  [SharePoint 2013 のデザイン マネージャーの概要](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](how-to-create-a-page-layout-in-sharepoint-2013.md)
    
  
-  [方法: SharePoint 2013 で HTML ファイルをマスター ページに変換する](how-to-convert-an-html-file-into-a-master-page-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 デザイン マネージャー スニペット](sharepoint-2013-design-manager-snippets.md)
    
  

