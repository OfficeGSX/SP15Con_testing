---
title: SharePoint 2013 でカスタム テーマを展開する方法
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# SharePoint 2013 でカスタム テーマを展開する方法
ユーザー インターフェイスを使用するか、機能レシーバーを実装することによって、SharePoint 2013 サイトにカスタム テーマを展開する方法について説明します。
SharePoint 2013 には事前にインストールされているテーマがあります。追加のカラー パレット、フォント パターン、およびマスター ページを作成して、カスタム テーマを作成できます。ファイルがテーマ ギャラリーおよびマスター ページ ギャラリーにアップロードされた後、ユーザー インターフェイスまたはコードを使用してサイトにテーマを展開できます。カラー パレットおよびフォント パターンについては、「 [SharePoint 2013 のカラー パレットとフォント](color-palettes-and-fonts-in-sharepoint-2013.md)」を参照してください。
  
    
    


## テーマを展開するために知っておく必要がある中心概念
<a name="core"> </a>

表 1 に、テーマの展開に関する中心概念を理解するために役立つ記事を示します。
  
    
    

**表 1. テーマの展開に関する中心概念**


|**記事のタイトル**|**説明**|
|:-----|:-----|
| [SharePoint 2013 のテーマの概要](themes-overview-for-sharepoint-2013.md) <br/> |SharePoint 2013 のテーマ エクスペリエンスについて説明します。  <br/> |
| [フィーチャー イベント](http://msdn.microsoft.com/ja-jp/library/ms469501.aspx) <br/> |フィーチャーがサーバー ファームにインストールされる際に発生するイベントの受信および応答を可能にするフィーチャー イベントについて説明します。  <br/> |
   

## テーマ ギャラリーおよびマスター ページ ギャラリーへのファイルのアップロード
<a name="section1"> </a>

追加のカラー パレットおよびフォント パターンを作成し、テーマ ギャラリーにアップロードすることによって、カスタム テーマを作成できます。その後、テーマ エクスペリエンスのデザインを変更した場合、およびプログラムでテーマを適用した場合に、新しいカラー パレットおよびフォント パターンが適用されます。同様に、選択元の追加のサイト レイアウトが必要な場合、追加のマスター ページ、および対応するプレビュー ファイルをマスター ページ ギャラリーにアップロードできます。次の一覧では、ファイルを配置する場所について説明します。
  
    
    

- **マスター ページ ギャラリー** マスター ページ ファイル、および対応するプレビュー ファイル (.preview files) を表示します。マスター ページを [ **外観の変更**] ウィザードで使用できるようにする場合、マスター ページ プレビュー ファイルが必要です。また、JavaScript ファイルおよび他のデザイン アセットも、マスター ページ ギャラリーにアップロードできます。
    
    SharePoint ユーザー インターフェイスからマスター ページ ギャラリーにアクセスするには、[ **Web デザイナー ギャラリー**] の [ **サイトの設定**] で、[ **マスター ページ**] を選択します。サイト (http://  _SiteName_/_catalogs/masterpage/) に直接移動することもできます。
    
  
- **テーマ ギャラリー** テーマ エクスペリエンスで使用できるカラー パレットおよびフォント パターンを表示します。SharePoint は [ **15**] フォルダーを検索し、使用可能なカラー パレットおよびフォント パターンを判断します。
    
    SharePoint ユーザー インターフェイスからテーマ ギャラリーにアクセスするには、[ **Web デザイナー ギャラリー**] の [ **サイトの設定**] で、[ **テーマ**] を選択します。サイト (http:// _SiteCollectionName_/_catalogs/theme/15/) に直接移動することもできます。
    
  
- **スタイル ライブラリ** テーマ エクスペリエンスを使用するカスタム CSS ファイルを表示します。スタイル ライブラリに直接移動することもできます (次の URL の _SiteCollectionName_ および _language_ を置換します。http:// _SiteCollectionName_/Style Library/ _language_/Themable/)。
    
> **メモ**
> マスター ページ ギャラリーの Themable フォルダーではなく、スタイル ライブラリの Themable フォルダーにカスタム CSS ファイルを配置します。スタイル ライブラリの Themable フォルダーに保存された CSS ファイルのみがテーマ設定エンジンに認識されます。 

> **メモ**
> マスター ページ ギャラリーおよびテーマ ギャラリーで有効なバージョン管理機能がある場合、テーマ設定エンジンで使用できるようにする前に、デザイン ファイルも発行する必要があります。 
  
    
    


## ユーザー インターフェイスを使用したテーマの展開
<a name="section2"> </a>

構成された外観 (デザイン) とは、サイトの外観を決定するカラー パレット、フォント スパターン、背景イメージ、およびマスター ページのことです。構成された外観リストには、デザイン ギャラリーで使用できる構成済みの外観が含まれます。構成された外観リストにリスト アイテムを追加し、使用するマスター ページ、カラー パレット、フォント パターン、および背景イメージを指定して、デザインを作成します。
  
    
    

> **メモ**
> マスター ページをデザイン ギャラリーで使用できるようにする場合、マスター ページ プレビュー ファイルが必要です。 
  
    
    


### 構成された外観を追加するには


1. [ **設定**] アイコンを選択して、[ **サイトの設定**] を選択します。
    
  
2. [ **Web デザイナー ギャラリー**] で [ **構成された外観**] を選択します。
    
  
3. [ **構成された外観**] リストで [ **新しいアイテム**] を選択します。
    
  
4. [ **タイトル**] テキスト ボックスにデザインのタイトルを入力します。
    
  
5. [ **名前**] テキスト ボックスに、デザインの名前を入力します。名前は構成された外観リストおよびデザイン ギャラリーに表示されます。
    
  
6. [ **マスター ページ URL**] テキスト ボックスに、マスター ページの URL を入力します。URL は相対 URL を使用できます。
    
  
7. [ **テーマ URL**] テキスト ボックスに、カラー パレットの URL (.spcolor ファイルの URL) を入力します。URL は相対 URL を使用できます。
    
  
8. [ **イメージ URL**] テキスト ボックスに、背景イメージの URL を入力します。これは省略可能です。URL は相対 URL を使用できます。
    
  
9. [ **フォント パターン URL**] テキスト ボックスに、フォント パターンの URL (.spfont ファイルの URL) を入力します。これは省略可能です。URL は相対 URL を使用できます。
    
  
10. [ **表示の順序**] テキスト ボックスに、表示の順序の番号を入力します。これはデザインがデザイン ギャラリーに表示される位置を決定します。
    
  
11. [ **保存**] を選択します。
    
    > **メモ**
      > 構成された外観の値に問題がある場合、構成された外観はデザイン ギャラリーに追加されません。またメッセージはログ ファイルに記録されません。構成された外観を追加できない可能性のある理由を次に示します: ファイルが見つからない、いずれかのファイルにフォーマットの問題がある、または SharePoint がファイルにアクセスできない。 
デザイン ギャラリーを使用し、新しいデザインをサイトに適用できるようになりました。詳細については、Office.com の「 [発行サイトのテーマを選択する](http://office.microsoft.com/ja-jp/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx)」を参照してください。
  
    
    

## コードによるテーマの展開
<a name="section3"> </a>

機能レシーバーを実装してテーマを展開できます。
  
    
    

### 機能レシーバーを使用してテーマを展開するには


1.  [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) クラスから継承する機能レシーバー クラスを作成します。
    
  
2.  [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) メソッドで、カラー パレットおよびフォント パターンを使用する [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) オブジェクトを作成し、次にテーマをサイトに適用します。
    
    次のコードの例は、カスタム カラー パレットおよびフォント パターンをサイトに展開する方法を示します。
    


  ```cs
  
// Get the SPColor file. Replace with the path to your SPColor file.
SPFile colorPaletteFile = Web.GetFile("path to .spcolor file");
if (null == colorPaletteFile || !colorPaletteFile.Exists)
{
    // TODO: handle the error.
    return;
}

// Get the SPFont file. Replace with the path to your SPFont file.
SPFile fontSchemeFile = Web.GetFile("path to .spfont file");
if (null == fontSchemeFile || !fontSchemeFile.Exists)
{
    // TODO: handle the error.
    return;
}

// Open an SPTheme with the two files. Replace NewTheme with the name for your theme.
// Note: If you have a background image, you can specify the following:
// SPTheme.Open("NewTheme", colorPaletteFile, fontSchemeFile, backgroundURI)
SPTheme theme = SPTheme.Open("NewTheme", colorPaletteFile, fontSchemeFile);


// Now apply your theme to the site.
// The themed CSS output files are stored in the Themed folder of the Theme Gallery of the root web
// of the site collection. To specify that the files should be stored in the _themes folder within the root 
// web, pass false to the ApplyTo method.
theme.ApplyTo(Web, true);
  ```


> **メモ**
> **ApplyTo** メソッドの _shareGenerated_ パラメーターは、テーマ設定されたファイルをサイト コレクションのサイト間で共有できるかどうかを示します。一般に、SharePoint Server および SharePoint Online サイトに **true** が設定され、SharePoint Foundation サイトに **false** が設定されます。テーマ設定されたファイルを共有しようとする場合、 _shareGenerated_ パラメーターを **true** に設定する必要があります。詳細については、 [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) を参照してください。

ユーザーが [ **外観の変更**] ウィザードにテーマを適用する場合、ウィザードは構成された外観のリストおよびデザイン ギャラリーの Current という名前のテーマも更新します。プログラムでテーマを適用する場合、現在のテーマは手動で更新する必要があります。次の例は、現在のテーマを更新する方法を示します。
    


  ```cs
  
SPList designGallery = Web.GetCatalog(SPListTemplateType.DesignCatalog);
if (null == designGallery)
{
    // TODO: Handle the error.
    return;
}

SPQuery q = new SPQuery();
q.RowLimit = 1;
q.Query = "<Where><Eq><FieldRef Name='DisplayOrder'/><Value Type='Number'>0</Value></Eq></Where>";
q.ViewFields = "<FieldRef Name='DisplayOrder'/>";
q.ViewFieldsOnly = true;

SPListItemCollection currentItems = designGallery.GetItems(q);

If (currentItems.Count == 1)
{
    // Remove the old Current item.
    currentItems[0].Delete();
}

SPListItem currentItem = designGallery.AddItem();

currentItem["Name"] = SPResource.GetString(CultureInfo.CurrentUICulture, Strings.DesignGalleryCurrentItemName);
currentItem["Title"] = SPResource.GetString(CultureInfo.CurrentUICulture, Strings.DesignGalleryCurrentItemName);

// Change this line if you want to specify a different master page.
currentItem["MasterPageUrl"] = Web.MasterUrl;

// Replace with the path to your SPColor file.
currentItem["ThemeUrl"] = "path to .spcolor file";

// Delete the following line if you do not have a background image. Otherwise, replace with the path to
// the background image.
currentItem["ImageUrl"] = "path to background image"; 

// Replace with the path to your SPFont file. Or, you can delete this line if you want to use
// the default font scheme of the selected master page.
currentItem["FontSchemeUrl"] = "path to .spfont file"; 

currentItem["DisplayOrder"] = 0;
currentItem.Update();

  ```


## その他の技術情報
<a name="bk_addresources"> </a>


-  [SharePoint 2013 のサイト デザインの開発](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 のカラー パレットとフォント](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 でのマスター ページ プレビュー ファイルを作成する方法](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [SharePoint チームのブログ: SharePoint テーマでスタイルを際立たせる](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

