---
title: 方法 SharePoint 2013 で HTML ファイルをマスター ページに変換する
ms.prod: SHAREPOINT
ms.assetid: a76ab289-3256-45de-ac63-d5112a74e3c7
---


# 方法: SharePoint 2013 で HTML ファイルをマスター ページに変換する
デザイン マネージャーでは, .html ファイルを SharePoint 2013 のマスター ページ (.master ファイル) に変換できます。変換後は、HTML ファイルとマスター ページが関連付けられるので、HTML ファイルを編集して保存すると、変更内容は関連付けられたマスター ページと同期されます。
## マスター ページへの変換の概要
<a name="Introduction"> </a>

デザイン マネージャーでは, .html ファイルを SharePoint 2013 のマスター ページ (.master ファイル) に変換できます。変換後は、HTML ファイルとマスター ページが関連付けられるので、HTML ファイルを編集して保存すると、変更内容は関連付けられたマスター ページと同期されます。
  
    
    
.master ファイルをゼロから作成するのではなく、HTML ファイルを変換するのはなぜでしょうか。SharePoint 2013 では、マスター ページは ASP.NET での動作とまったく同様に動作しますが、SharePoint でマスター ページを正しくレンダリングするには、SharePoint 固有のコントロールやコンテンツ プレースホルダーなど特定の要素がそのページ上に存在している必要があります。デザイン マネージャーを使用して HTML ファイルを完全に機能する SharePoint のマスター ページに変換すれば、ASP.NET や SharePoint 固有のマークアップに関する知識は不要で、HTML、CSS、JavaScript でのサイトのデザインに重点を置くことができます。
  
    
    
HTML ファイルをマスター ページに変換すると、次の処理が行われます。
  
    
    

- HTML ファイルと同じ名前の .master ファイルがマスター ページ ギャラリーに作成されます。
    
  
- SharePoint 2013 でマスター ページを正しくレンダリングするために必要なすべてのマークアップが .master ファイルに追加されます。
    
  
- コメント、 **<div>** タグ、スニペット、コンテンツ プレースホルダーなどのマークアップが元の HTML ファイルに追加されます。
    
  
- HTML ファイルとマスター ページが関連付けられるので、その後で HTML ファイルに対して行われた編集は、HTML ファイルを保存すると .master ファイルと同期されます。
    
  

> **メモ**
> 同期は一方向にのみ実行されます。HTML マスター ページに対する変更は、関連付けられた .master ファイルと同期されますが, .master ファイルを直接編集しても、変更内容は HTML ファイルと同期されません。すべての HTML マスター ページ (およびすべての HTML ページ レイアウト) には、既定で **True** に設定されている [ **関連付けられているファイル**] というプロパティがあります。このプロパティによって、ファイル間の関連付けと同期が作成されます。 
  
    
    

関連付けられたファイルのペア (HTML と .master) があり、関連付けを解除することなく .master ファイルを編集した場合, .master ファイルに対する変更は保存されますが, .master ファイルをチェックインまたは発行することはできません。そのため、これらの変更内容の保存は意味を持ちません。HTML ファイルに対する変更内容で .master ファイルが上書きされます。HTML ファイルをチェックインまたは発行した場合、HTML ファイルの変更内容で .master ファイルに対して行われたあらゆる変更が上書きされます。.master ファイルに対する変更は失われます。
  
    
    
ASP.NET を使い慣れている開発者であれば、ファイル間の関連付けを解除することによって, .master ファイルのみを操作できます。デザイン マネージャーで HTML ファイルと .master ファイルの関連付けを解除するには、HTML ファイルの [ **プロパティの編集**] を選択し、[ **関連付けられたファイル**] チェック ボックスをオフにします。プロパティを編集し、このチェック ボックスをオンにすることで、後でファイルを再度関連付けることができます。その場合、HTML ファイルによって再度 .master ファイルが上書きされ, .master ファイルに対して行われた変更は失われます。
  
    
    

## HTML ファイルの変換の準備
<a name="Prepare"> </a>

HTML ファイルを変換する前に、検討すべきベスト プラクティスとガイダンスを以下に示します。
  
    
    

- SharePoint ページ モデルについて検討します。詳細については、「 [SharePoint 2013 ページ モデルの概要](overview-of-the-sharepoint-2013-page-model.md)」を参照してください。サイトの HTML 模擬表示をデザインする際、記事ページや、カタログのアイテムのカテゴリを表示する Web パーツを含むカテゴリ ページなど、さまざまなタイプのページ用に複数の HTML ファイルを作成することがあります。しかし、マスター ページに変換される HTML ファイルは 1 つだけです。マスター ページに変換できる HTML ファイルは 1 つですが、1 つのページ レイアウトには複数のページ フィールドが必要なため、HTML ファイルをページ レイアウトに直接変換することはできません。
    
  
- HTML ファイルが XML 準拠であることを確認します。変換を正しく行うには、HTML ファイルが XML 準拠である必要があります。残念ながら、この要件は一部の HTML 5 標準に優先します。たとえば、HTML 5 では **doctype** を小文字で指定できますが、XML では **doctype** は大文字でなければなりません。また、HTML ファイルからすべての **<form>** タグを削除する必要があります。変換前に XML エラーを特定するため、外部の XML 検証ツールから HTML ファイルを実行することを検討してください。
    
  
- CSS リファレンスとして、次の重要なガイドラインについて検討します。
    
  - **<style>** ブロックを **<head>** タグ内に配置しないでください。これらのスタイルは変換時に削除されます。代わりに、HTML ファイルから外部の CSS ファイルにリンクしてください。
    
  
  - Web フォントを使用しない場合は、 `ms-design-css-conversion="no"` を **<CSS link>** タグに追加します。
    
  
  - **<body>**、 **<div>**、 **< img>** などの一般的な HTML タグへのスタイルの適用は慎重に行ってください。リボンを含む、SharePoint デザイン内のすべてのものが **<body>** タグ内に含まれます。通常、 **<body>** タグに適用するスタイルには、代わりに **<div id="s4-bodyContainer">** に適用することを検討してください。これは、SharePoint 2013 でページの本体に使用されるタグです。また、SharePoint 2013 では、使用する多くの画像が **<img>** タグに適用されるすべてのスタイルによって影響を受けます。
    
  
  - 多くのデザイナーは、クラスを **<ul>** および **<li>** 要素に適用することによって、ナビゲーションのスタイルを設定します。しかし、SharePoint 2013 では動的ナビゲーション コントロールが使用され、ユーザーはそれをスニペット ギャラリーからマスター ページに追加できます。SharePoint 2013 のナビゲーション コントロールに既定で適用されるスタイルは、上書きする必要があります。
    
  
- ファイルの命名に関する次の潜在的な問題について検討します。
    
  - Index.html と Index.htm がある場合、これらのファイルの .master ファイルは同じ名前になります。
    
  
  - Design/Index.html と Design/SubDesign/Index.html がある場合、これらのファイルはどちらもそれぞれ別個の .master ファイルに変換できますが、デザイン マネージャーのマスター ページの一覧ではどちらも Index.html として表示されます。両者を明確にするには、各ファイルの省略記号ボタンをクリックまたは選択して、フル パスを表示します。
    
  
- JavaScript ウィジェットを追加する場合は、 **<script>** 開始タグを単独で行に配置します。
    
  ```
  
  <script>
  (function( …

  ```


    次のように、同じ行に配置しないでください。
    


  ```
  
  <Script> (function( …
  ```

- JQuery ライブラリへの参照 (外部参照) は **</head>** タグの前に配置する必要があります。
    
  

## HTML ファイルをマスター ページに変換する
<a name="Convert"> </a>

HTML ファイルを変換する前に、まず、HTML ファイルを含む、すべてのデザイン ファイルをアップロードする必要があります。詳細については、「 [[方法]: SharePoint 2013 マスター ページ ギャラリーへのネットワーク ドライブの割り当て](how-to-map-a-network-drive-to-the-sharepoint-2013-master-page-gallery.md)」を参照してください。
  
    
    

### HTML ファイルを .master ファイルに変換するには


1. 発行サイトに移動します。
    
  
2. ページの右上隅で [ **設定**] を選択し、[ **デザイン マネージャー**] を選択します。
    
  
3. デザイン マネージャーの左側のナビゲーション ウィンドウで、[ **マスター ページの編集**] を選択します。
    
  
4. [ **HTML ファイルを SharePoint マスター ページに変換**] を選択します。
    
  
5. [ **メディアの選択**] ダイアログ ボックスで、変換する HTML ファイルを参照して選択します。
    
    > **メモ**
      > デザイン ファイルをアップロードする場合は、1 つのデザインに関連付けられたすべてのファイルをマスター ページ ギャラリー内のそれぞれのフォルダーに保持する必要があります。デザイン フォルダーをマッピングされたネットワーク ドライブにコピーすると、マスター ページ ギャラリーには作成したすべてのフォルダー構造が維持されます。 
6. [ **挿入**] を選択します。
    
    ここで、SharePoint 2013 によって、HTML ファイルが同じ名前の .master ファイルに変換されます。
    
    デザイン マネージャーに HTML ファイルが表示され、[状態] 列には次の 2 つの状態のどちらかが表示されます。
    
  - エラー
    
  
  - **正常に変換されました**
    
  
7. [状態] 列のリンクをクリックして、ファイルをプレビューするか、マスター ページに関するエラーまたは警告を表示します。
    
    エラー
    
    エラーと警告への対応の詳細については、「 [SharePoint 2013 でページをプレビューしているときのエラーと警告を解決する方法](how-to-resolve-errors-and-warnings-when-previewing-a-page-in-sharepoint-2013.md)」を参照してください。
    
    さまざまなページを持つマスター ページのプレビューの詳細については、「 [方法: SharePoint 2013 デザイン マネージャーでプレビュー ページを変更する](how-to-change-the-preview-page-in-sharepoint-2013-design-manager.md)」を参照してください。
    
    プレビュー ページの右上隅には、[スニペット] リンクもあります。このリンクをクリックすると、スニペット ギャラリーが開きます。スニペット ギャラリーでは、デザインの静的コントロールまたは模擬表示コントロールを動的 SharePoint コントロールと置き換えることができます。詳細については、「 [SharePoint 2013 デザイン マネージャー スニペット](sharepoint-2013-design-manager-snippets.md)」を参照してください。
    
  
8. エラーを修正するには、HTML エディターを使用して、マッピングされたドライブ内の HTML ファイルを開いて編集することにより、サーバー上に直接置かれている HTML ファイルを編集します。HTML ファイルを保存するたびに、すべての変更内容が関連付けられた .master ファイルと同期されます。
    
  
9. マスター ページが正常にプレビューされた後は、HTML ファイルに **<div>** タグが追加されています。 **<div>** タグを表示するには、ページ下部までスクロールしなければならないことがあります。
    
    この **<div>** はメイン コンテンツ ブロックで、 **ContentPlaceHolderMain** というコンテンツ プレースホルダー内に置かれます。実行時に閲覧者がサイトを参照してページを要求すると、このコンテンツ プレースホルダーには、一致するコンテンツ領域内にコンテンツを含むページ レイアウトのコンテンツが設定されます。この **<div>** は、マスター ページ上にページ レイアウトを表示させる場所に位置付ける必要があります。
    
    HTML ファイルにページ本体の静的コンテンツまたは模擬表示コンテンツが含まれる場合、ここでその静的コンテンツを HTML マスター ページから削除して、ページ レイアウト、ページ フィールド コントロール、スニペット、表示テンプレートなど、SharePoint ページ モデルの他の要素にこれらのスタイルを適用するプロセスを開始します。たとえば、「 [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](how-to-create-a-page-layout-in-sharepoint-2013.md)」を参照してください。
    
  

## 変換後の HTML ファイルの理解
<a name="Understand"> </a>

HTML ファイルをマスター ページに変換すると、多数のマークアップ行が HTML ファイルに追加されます。このマークアップの大部分は無視しても問題はなく、ブラウザーでソースを表示したときにサイトの最終的なマークアップに表示されませんが、HTML ファイルを SharePoint が実際に使用する .master ファイルに変換する上では、このマークアップは重要です。HTML ファイルに対する変更を保存するたびに、この SharePoint マークアップにより、関連付けられた .master ファイルに対して同じ変更をバックグラウンドで行うことができます。
  
    
    
追加されるマークアップには、 **<head>** タグの前または中に含まれるタグ、スニペット、コンテンツ プレースホルダーなどがあります。大部分のマークアップは、コメント タグに囲まれていますが、HTML ファイルに対する変更を保存するたびに、その中の ASP.NET マークアップを使用できるよう、変換プロセスによってコメントが取り除かれます。
  
    
    

### マークアップの種類

HTML ファイルに追加されるマークアップの種類の分類を以下に示します。
  
    
    

- **ドキュメント プロパティ** - **<mso>** タグには、ファイル自体と, .master ファイルへの変換を正常に行うために必要ないくつかのプロパティに関する情報などの SharePoint メタデータが含まれます。
    
  ```HTML
  
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
  ```

- **SharePoint 名前空間登録** - **<SPM>** タグ ("SharePoint マークアップ") により、SharePoint 名前空間を登録する行が追加されます。
    
  ```HTML
  
<!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
  ```

- **コメント** - **<CS>** および **<CE>** ("コメント開始" と "コメント終了") タグは、変換プロセスで無視されます。これらのタグは、マークアップ行の解析に役立ちます。
    
  ```HTML
  
<!--CS: Start Page Head Contents Snippet-->
…
<!--CE: End Page Head Contents Snippet-->

  <!--CS: Start Ribbon Snippet-->
…
<!--CE: End Ribbon Snippet-->

<!--CS: Start PlaceHolderMain Snippet-->
…
<!--CE: End PlaceHolderMain Snippet-->
  ```

- **スニペット** - **<MS>** および **<ME>** ("マークアップ開始" と "マークアップ終了") タグには、SharePoint コントロールまたはスニペットの開始と終了が含まれます。スニペットとは、SharePoint 機能をページに追加する SharePoint コントロールです。スニペット ギャラリーを使用して、ユーザー自身でスニペットを追加できます。詳細については、「 [SharePoint 2013 デザイン マネージャー スニペット](sharepoint-2013-design-manager-snippets.md)」を参照してください。
    
  ```HTML
  
<!--MS:<SharePoint:RobotsMetaTag runat="server">-->
        <!--ME:</SharePoint:RobotsMetaTag>-->
        <!--MS:<SharePoint:PageTitle runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server">-->
                <!--MS:<SharePoint:ProjectProperty Property="Title" runat="server">-->
                <!--ME:</SharePoint:ProjectProperty>-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:PageTitle>-->
        <!--MS:<SharePoint:StartScript runat="server">-->
        <!--ME:</SharePoint:StartScript>-->
        <!--MS:<SharePoint:CssLink runat="server" Version="15">-->
        <!--ME:</SharePoint:CssLink>-->
        <!--MS:<SharePoint:CacheManifestLink runat="server">-->
        <!--ME:</SharePoint:CacheManifestLink>-->
        <!--MS:<SharePoint:PageRenderMode runat="server" RenderModeType="Standard">-->
        <!--ME:</SharePoint:PageRenderMode>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="core.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="menu.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="callout.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="sharing.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="suitelinks.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:CustomJSUrl runat="server">-->
        <!--ME:</SharePoint:CustomJSUrl>-->
        <!--MS:<SharePoint:SoapDiscoveryLink runat="server">-->
        <!--ME:</SharePoint:SoapDiscoveryLink>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaPlaceHolderAdditionalPageHead" Container="false" runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderAdditionalPageHead" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
            <!--MS:<SharePoint:DelegateControl runat="server" ControlId="AdditionalPageHead" AllowMultipleControls="true">-->
            <!--ME:</SharePoint:DelegateControl>-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderBodyAreaClass" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--MS:<SharePoint:CssRegistration Name="Themable/corev15.css" runat="server">-->
        <!--ME:</SharePoint:CssRegistration>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaSPWebPartManager" runat="server">-->
            <!--MS:<WebPartPages:SPWebPartManager runat="server">-->
            <!--ME:</WebPartPages:SPWebPartManager>-->
        <!--ME:</SharePoint:AjaxDelta>-->
  ```

- **プレビュー ブロック** - **<PS>** および **<PE>** ("プレビュー開始" と "プレビュー終了") タグは、設計時のプレビューにのみ影響を与えるため編集する必要がない HTML コードのセクションを囲みます。これらのプレビュー セクションは、SharePoint コントロールまたはスニペットの挿入時のスナップショットです。プレビューを使用することによって、クライアント側の HTML エディターで HTML ファイルに対してより有効に作業できるようになります。ただし、そのプレビューでコンテンツまたはスタイル設定を変更しても、SharePoint が最終的に使用する .master ファイルに対する効果は長続きしません。スニペットにスタイルを設定するには、SharePoint スタイルを指定し、独自のカスタム CSS で上書きする必要があります。
    
  ```HTML
  
<!--PS: Start of READ-ONLY PREVIEW (do not modify) -->
<div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div>
<!--PE: End of READ-ONLY PREVIEW -->
  ```

- **SharePoint ID** - 変換時に HTML ファイルに追加されるスニペットのうち 2 つ (Page Head Contents スニペットと SharePoint Ribbon) には、SharePoint ID、すなわち SID (それぞれ 00 と 02) が関連付けられています。これらの ID を使用して、スニペットを短縮し、ページ内の HTML を読みやすくすることができます。
    
  ```HTML
  
<!--SID:00 -->

<!--SID:02 {Ribbon}-->
  ```


### 追加されるスニペット

HTML ファイルに追加されるスニペットのうち 2 つについて理解することが重要です。これらのスニペットは変換時に自動的に追加されますが、スニペット ギャラリーから追加することはできません。
  
    
    

- **Ribbon** - コンテンツ作成者がページを作成し、SharePoint サイトにコンテンツを作成できるようにするには、マスター ページにリボンと SharePoint 2013 で新たに導入された "スィート ナビゲーション" が必要です。リボンはセキュリティトリミング スニペットに含まれるため、閲覧者がサイトを参照すると、リボンは認証されたユーザーにのみ表示され、匿名ユーザーには表示されません。リボンをページ上のさまざまな位置に移動したり、既定の CSS クラスを上書きしてスタイルを設定したりできますが、リボン内に含まれる ([サイト操作] メニューなどの) コンポーネントを移動したり、並べ替えたりすることはお勧めしません。
    
  ```HTML
  
<!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
<!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
<!--ME:</wssucw:Welcome>-->
<!--ME:</SharePoint:SPSecurityTrimmedControl>-->
  ```

- **ContentPlaceHolderMain** - 変換プロセスによって、 **<div id="s4-bodyContainer">** タグの一番下、終了 **</body>** タグの前に、 **PlaceHolderMain** というコンテンツ プレースホルダーが挿入されます。このスニペットの内側は境界線が黒く、黄色い **<div>** で、HTML エディターのデザイン ビューまたはデザイン マネージャーのサーバー側プレビューに表示されます。
    
    この **<div>** は、ページ レイアウトとページで指定されたコンテンツが表示される領域を表します。 **PlaceHolderMain** スニペットをページ レイアウトによって設定されるマスター ページ内の場所、つまりサイトのすべてのページで同じというわけではないサイト デザインの領域に移動する必要があります。
    


  ```HTML
  
<!--CS: Start PlaceHolderMain Snippet-->
                    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
                    <!--MS:<SharePoint:AjaxDelta ID="DeltaPlaceHolderMain" IsMainContent="true" runat="server">-->
                        <!--MS:<asp:ContentPlaceHolder ID="PlaceHolderMain" runat="server">-->
                            <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
            This div, which you should delete, represents the content area that your Page Layouts and pages will fill. Design your Master Page around this content placeholder.
        
                            </div>
                        <!--ME:</asp:ContentPlaceHolder>-->
                    <!--ME:</SharePoint:AjaxDelta>-->
                    <!--CE: End PlaceHolderMain Snippet-->
  ```


## 例
<a name="Reference"> </a>

マスター ページへの変換後に HTML ファイルに追加されるマークアップの例を以下に示します。
  
    
    

### <head> タグに追加されるマークアップ


```HTML

<head>
        <meta http-equiv="X-UA-Compatible" content="IE=10" />
        <!--CS: Start Page Head Contents Snippet-->
        <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SID:00 -->
        <meta name="GENERATOR" content="Microsoft SharePoint" />
        <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
        <meta http-equiv="Expires" content="0" />
        <!--MS:<SharePoint:RobotsMetaTag runat="server">-->
        <!--ME:</SharePoint:RobotsMetaTag>-->
        <!--MS:<SharePoint:PageTitle runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server">-->
                <!--MS:<SharePoint:ProjectProperty Property="Title" runat="server">-->
                <!--ME:</SharePoint:ProjectProperty>-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:PageTitle>-->
        <!--MS:<SharePoint:StartScript runat="server">-->
        <!--ME:</SharePoint:StartScript>-->
        <!--MS:<SharePoint:CssLink runat="server" Version="15">-->
        <!--ME:</SharePoint:CssLink>-->
        <!--MS:<SharePoint:CacheManifestLink runat="server">-->
        <!--ME:</SharePoint:CacheManifestLink>-->
        <!--MS:<SharePoint:PageRenderMode runat="server" RenderModeType="Standard">-->
        <!--ME:</SharePoint:PageRenderMode>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="core.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="menu.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="callout.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="sharing.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="suitelinks.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:CustomJSUrl runat="server">-->
        <!--ME:</SharePoint:CustomJSUrl>-->
        <!--MS:<SharePoint:SoapDiscoveryLink runat="server">-->
        <!--ME:</SharePoint:SoapDiscoveryLink>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaPlaceHolderAdditionalPageHead" Container="false" runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderAdditionalPageHead" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
            <!--MS:<SharePoint:DelegateControl runat="server" ControlId="AdditionalPageHead" AllowMultipleControls="true">-->
            <!--ME:</SharePoint:DelegateControl>-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderBodyAreaClass" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--MS:<SharePoint:CssRegistration Name="Themable/corev15.css" runat="server">-->
        <!--ME:</SharePoint:CssRegistration>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaSPWebPartManager" runat="server">-->
            <!--MS:<WebPartPages:SPWebPartManager runat="server">-->
            <!--ME:</WebPartPages:SPWebPartManager>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--CE: End Page Head Contents Snippet-->
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <!--DC:Business Solutions-->
        <link rel="stylesheet" href="css/style.css" type="text/css" charset="utf-8" />
        <!--[if lte IE 7]>
  <link rel="stylesheet" href="css/ie.css" type="text/css" charset="utf-8"/> 
 <![endif]-->
        <!--[if gte mso 9]><xml>
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
</xml><![endif]-->
    </head>

```


### 開始 <body> タグの後に追加されるマークアップ


#### Ribbon スニペット


```HTML

<!--CS: Start Ribbon Snippet-->
        <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="wssucw" TagName="Welcome" Src="~/_controltemplates/15/Welcome.ascx"%>-->
        <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" HideFromSearchCrawler="true" EmitDiv="true">-->
            <div id="TurnOnAccessibility" style="display:none" class="s4-notdlg noindex">
                <a id="linkTurnOnAcc" href="#" class="ms-accessible ms-acc-button" onclick="SetIsAccessibilityFeatureEnabled(true);UpdateAccessibilityUI();document.getElementById('linkTurnOffAcc').focus();return false;">
                    <!--MS:<SharePoint:EncodedLiteral runat="server" text="&amp;lt;%$Resources:wss,master_turnonaccessibility%&amp;gt;" EncodeMethod="HtmlEncode">-->
                    <!--ME:</SharePoint:EncodedLiteral>-->
                </a>
            </div>
            <div id="TurnOffAccessibility" style="display:none" class="s4-notdlg noindex">
                <a id="linkTurnOffAcc" href="#" class="ms-accessible ms-acc-button" onclick="SetIsAccessibilityFeatureEnabled(false);UpdateAccessibilityUI();document.getElementById('linkTurnOnAcc').focus();return false;">
                    <!--MS:<SharePoint:EncodedLiteral runat="server" text="&amp;lt;%$Resources:wss,master_turnoffaccessibility%&amp;gt;" EncodeMethod="HtmlEncode">-->
                    <!--ME:</SharePoint:EncodedLiteral>-->
                </a>
            </div>
        <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
        <div id="ms-designer-ribbon">
            <!--SID:02 {Ribbon}-->
            <!--PS: Start of READ-ONLY PREVIEW (do not modify) --><div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div><!--PE: End of READ-ONLY PREVIEW -->
        </div>
        <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
            <!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
            <!--ME:</wssucw:Welcome>-->
        <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
        <!--CE: End Ribbon Snippet-->

```


#### 2 つの SharePoint <div> タグ


```HTML

<div id="s4-workspace">
            <div id="s4-bodyContainer">

```


### 終了 </body> タグと 2 つの終了 </div> タグの前に追加されるマークアップ


```HTML

<div data-name="ContentPlaceHolderMain">
                    <!--CS: Start PlaceHolderMain Snippet-->
                    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
                    <!--MS:<SharePoint:AjaxDelta ID="DeltaPlaceHolderMain" IsMainContent="true" runat="server">-->
                        <!--MS:<asp:ContentPlaceHolder ID="PlaceHolderMain" runat="server">-->
                            <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
            This div, which you should delete, represents the content area that your Page Layouts and pages will fill. Design your Master Page around this content placeholder.
        
                            </div>
                        <!--ME:</asp:ContentPlaceHolder>-->
                    <!--ME:</SharePoint:AjaxDelta>-->
                    <!--CE: End PlaceHolderMain Snippet-->
                </div>

```


## その他の技術情報
<a name="Additional"> </a>


-  [SharePoint 2013 のデザイン マネージャーの概要](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [[方法]: SharePoint 2013 でページ レイアウトを作成する方法](how-to-create-a-page-layout-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 デザイン マネージャー スニペット](sharepoint-2013-design-manager-snippets.md)
    
  

