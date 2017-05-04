---
title: [方法] SharePoint 2013 の PerformancePoint Services 用のフィルター エディタを作成する
ms.prod: SHAREPOINT
ms.assetid: 51354c88-4330-40f0-bffa-1ce73f9ff298
---


# [方法] SharePoint 2013 の PerformancePoint Services 用のフィルター エディタを作成する
PerformancePoint サービス のカスタム フィルター拡張機能のエディター コンポーネントを作成する方法について説明します。
## PerformancePoint サービス のカスタム フィルター エディターとは
<a name="bk_intro"> </a>

PerformancePoint サービス では、カスタム フィルター エディターを使用することにより、ユーザーがカスタム フィルターのプロパティを設定できます。フィルター エディターは、スコアカードとレポートのコンシューマーのパラメーター値を含んだフィルター開始ポイントを定義する、フィルターの  [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) プロパティを初期化する必要がります。エディターの要件と機能の詳細については、「 [カスタム PerformancePoint Services オブジェクトのエディター](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)」を参照してください。
  
    
    
次の手順と例は、 [カスタム オブジェクト サンプル](http://msdn.microsoft.com/library/af021d52-7562-4e7a-9de4-e1fc5784a59d%28Office.15%29.aspx) からの **SampleFilterEditor** クラスに基づいています。エディターはシン Web アプリケーションで、これによりユーザーは、フィルターの名前と説明を変更し、基となるデータ ソースを選択することができます。クラスの完全なコードについては、「 [コード サンプル: Create, retrieve, and update custom filters in SharePoint Server 2013 でのカスタム PerformancePoint サービス フィルターの作成、取得、および更新](#bk_example)」を参照してください。
  
    
    
サンプル エディターをテンプレートとして使用するようお勧めします。このサンプルは、PerformancePoint サービス API でオブジェクトを呼び出す方法を示し、リポジトリ操作 (オブジェクトの作成や更新など) の呼び出しを簡単にするヘルパー オブジェクトを提供し、PerformancePoint サービス 開発のベスト プラクティスを示しています。
  
    
    

## カスタム PerformancePoint サービス フィルターのエディター クラスの作成と構成
<a name="bk_intro"> </a>


  
    
    

1. コンピューターに、PerformancePoint サービス をインストールするか、拡張機能で使用する DLL (手順 3 を参照) をコピーします。詳細については、「 [開発シナリオで使用される PerformancePoint Services DLL](http://msdn.microsoft.com/library/41e92619-8253-481d-82f9-35b6a6abc477%28Office.15%29.aspx)」を参照してください。
    
  
2. Visual Studio で、C# クラス ライブラリを作成します。拡張機能のクラス ライブラリを作成してある場合は、新しい C# クラスを追加します。
    
    DLL には、厳密な名前で署名する必要があります。さらに、DLL によって参照されるすべてのアセンブリが厳密な名前を持つことを確認してください。厳密な名前を使用してアセンブリに署名する方法の詳細、および公開/秘密キーのペアを作成する方法の詳細については、「 [How to: Create a public/private key pair](http://msdn.microsoft.com/library/05026813-f3bd-4d7c-9e0b-fc588eb3d114.aspx)」を参照してください。
    
  
3. プロジェクトに、アセンブリ参照として以下の DLL を追加します。
    
  - Microsoft.PerformancePoint.Scorecards.Client.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerCommon.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerRendering.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.Store.dll (ヘルパー クラスが使用)
    
  
  - Microsoft.SharePoint.dll (ヘルパー クラスが使用)
    
  

    サンプル エディターには、System.Web.dll と System.Web.Services.dll へのアセンブリ参照も含まれます。拡張機能の機能によっては、その他のプロジェクト参照が必要になることがあります。
    
  
4. サンプルから以下のクラスをプロジェクトに追加します。エディターは、PerformancePoint サービスリポジトリの操作でこれらのヘルパークラスを使用します。
    
  - DataSourceConsumerHelper.cs
    
  
  - ExtensionRepositoryHelper.cs
    
  
  - FilterRepositoryHelper.cs
    
  
  - IDataSourceConsumer.cs
    
  
5. エディター クラスで、以下の PerformancePoint サービス 名前空間の **using** ディレクティブを追加します。
    
  - **Microsoft.PerformancePoint.Scorecards**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerCommon**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerRendering**
    
  

    拡張機能の機能によっては、他の **using** ディレクティブが必要になることがあります。
    
  
6. エディターの実装をサポートする基本クラスから継承します。サンプルのフィルター エディターは Web アプリケーションであるため、 [Page](https://msdn.microsoft.com/library/System.Web.UI.Page.aspx) クラスから継承されます。他の実装では、 [UserControl](https://msdn.microsoft.com/library/System.Windows.Forms.UserControl.aspx) クラス、 [WebPart](https://msdn.microsoft.com/library/System.Web.UI.WebControls.WebParts.WebPart.aspx) クラスなどの基底クラスから派生できます。
    
  
7. ユーザーが参照または編集するプロパティを公開するコントロールを定義します。サンプルのフィルター エディターは、ユーザー インターフェイス コンポーネント (ASPX ページ) に定義された Web サーバー コントロールの変数を最初に宣言します。また、サンプル エディターは、変更をユーザーが送信できるようにするボタン コントロールも定義します。次に、エディターは  [CreateChildControls()](https://msdn.microsoft.com/library/System.Web.UI.Control.CreateChildControls.aspx) メソッドを呼び出して、コントロールをページで使用できるようにします。
    
    > **メモ**
      > エディターは、ユーザー インターフェイスとは別のプログラミング ロジックを定義します。エディターのユーザー インターフェイス コンポーネントを作成するための説明は、この文書の範囲外となります。 

    サンプルのフィルター エディターは、 **Page_Load** メソッドで手順 8 ～ 12 を実行します。 **Page_Load** は、変数とコントロールの初期化と検証、コントロールへのデータの挿入、カスタム フィルターとヘルパー オブジェクトの状態情報の保存にも使用されます。
    
  
8.  [AllowUnsafeUpdates](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ServerCommon.ServerUtils.AllowUnsafeUpdates.aspx) プロパティを **true** に設定します。これにより、フォーム POST 操作を使用しなくても、フィルター エディターでデータをリポジトリに書き込むことができます。
    
  
9. クエリ文字列からパラメーターを取得し、それらを以下のコード サンプルで示すローカル変数のための値として設定します。
    
  ```cs
  
// The URL of the site collection that contains the PerformancePoint Services repository.
string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];

// The location of the filter in the repository.
string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];

// The operation to perform: OpenItem or CreateItem.
string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];
  ```


  ```VB.net
  
' The URL of the site collection that contains the PerformancePoint Services repository.
Dim server As String = Request.QueryString(ClickOnceLaunchKeys.SiteCollectionUrl)

' The location of the filter in the repository.
Dim itemLocation As String = Request.QueryString(ClickOnceLaunchKeys.ItemLocation)

' The operation to perform: OpenItem or CreateItem.
Dim action As String = Request.QueryString(ClickOnceLaunchKeys.LaunchOperation)
  ```


  クエリ文字列パラメーターの詳細については、「 [カスタム PerformancePoint Services オブジェクトのエディター](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)」を参照してください。
    
  
10. 以下のコード サンプルに示すように、リポジトリに呼び出しをするための **FilterRepositoryHelper** オブジェクトを取得します。
    
  ```cs
  
filterRepositoryHelper = new FilterRepositoryHelper();
  ```


  ```VB.net
  filterRepositoryHelper = New FilterRepositoryHelper()
  ```

11. 以下のコード サンプルのように、クエリ文字列パラメーターに基づいてフィルターの場所を設定します。
    
  ```cs
  RepositoryLocation repositoryFilterLocation = RepositoryLocation.CreateFromUriString(itemLocation);
  ```


  ```VB.net
  Dim repositoryFilterLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(itemLocation)
  ```

12. クエリ文字列から実行する操作 ( _OpenItem_ や _CreateItem_) を取得して、カスタム フィルターを取得または作成します。
    
  - カスタム フィルターを取得するには、 **FilterRepositoryHelper.Get** メソッドを使用します。
    
  
  - カスタム フィルターを作成するには、 **Filter()** コンストラクターを使用して、フィルターの [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.RendererClassName.aspx) 、および [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.SubTypeId.aspx) プロパティを定義します。 [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.SubTypeId.aspx) は、フィルターの一意の識別子であり、PerformancePoint サービス web.config ファイルでカスタム フィルターに指定する **subType** 属性と一致する必要があります。 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.RendererClassName.aspx) は、レンダラーの Web サーバー コントロールを定義するクラスの完全修飾名です。これをエディターに定義しない場合、この値は、web.config ファイルに指定されたレンダラー クラスに既定設定されます。
    
  

  ```cs
  if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
{

                // Use the repository-helper object to retrieve the filter.
                filter = filterRepositoryHelper.Get(repositoryFilterLocation);
                if (filter == null)
                {
                    displayError("Could not retrieve the filter for editing.");
                    return;
                }
 }
else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
{
                filter = new Filter
                {
                    RendererClassName = typeof(MultiSelectTreeViewControl).AssemblyQualifiedName,
                    SubTypeId = "SampleFilter"
                };
}
  ```


  ```VB.net
  
If ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then

' Use the repository-helper object to retrieve the filter.
      filter = filterRepositoryHelper.Get(repositoryFilterLocation)
      If filter Is Nothing Then
           displayError("Could not retrieve the filter for editing.")
           Return
      End If

ElseIf ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then
      filter = New Filter With {.RendererClassName = GetType(MultiSelectTreeViewControl).AssemblyQualifiedName, .SubTypeId = "SampleFilter"}
End If
  ```


> **メモ**
> 既定では、ユーザーは PerformancePoint ダッシュボード デザイナー からのみカスタム オブジェクトを作成できます。ユーザーが ダッシュボード デザイナー の外部でカスタム オブジェクトを作成できるようにするには、リポジトリのコンテンツ タイプから  _CreateItem_ 要求をエディターに送るメニュー項目を追加する必要があります。詳細については、「 [カスタム PerformancePoint Services オブジェクトのエディター](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)」を参照してください。 
13. フィルターの基盤となるデータ ソースをリポジトリから取得します。サンプル フィルター エディターは、 **FilterRepositoryHelper.DataSourceHelper** プロパティを使用して、 **DataSourceConsumerHelper.GetDataSource** メソッドを呼び出します。このメソッドは、リポジトリ内の場所によってデータ ソースを取得するために使用されます。次のコード サンプルにその例を示します。
    
  ```cs
  
if (!string.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl))
    {
        RepositoryLocation repositoryDatasourceLocation =
        RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl);
        datasource =
        filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation);
    }
  ```


  ```VB.net
  
If Not String.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl) Then
      Dim repositoryDatasourceLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl)
      datasource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation)
End If
  ```

14. ユーザーがフィルターのデータ ソースを選択できるようにするには、PerformancePoint サービス データ ソースを使用して選択コントロールにデータを挿入します。サンプル フィルター エディター内の **PopulateDataSourceDropDown** メソッドは、 **DataSourceConsumerHelper.GetDataSourcesBySourceNames** メソッドを呼び出して、データ ソースを取得します。次のコード サンプルにその例を示します。
    
  ```cs
  
// The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
// Edit this value if you are not using the default path. A leading forward slash may not be needed.
ICollection dataSourceCollection =
    
filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames
    ("/BICenter/Data%20Connections%20for%20PerformancePoint/",
         new[] { "WSTabularDataSource", DataSourceNames.ExcelWorkbook });
  ```


  ```VB.net
  
' The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
' Edit this value if you are not using the default path. A leading forward slash may not be needed.
Dim dataSourceCollection As ICollection = filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames ("("/BICenter/Data%20Connections%20for%20PerformancePoint/", { "WSTabularDataSource", DataSourceNames.ExcelWorkbook })
  ```


    サンプル フィルター エディターでは 2 種類のデータ ソースのみが取得されますが、このメソッドを変更して、他のデータ ソースの種類をサポートしたり、取得するデータ ソースの種類をユーザーに要求したりできます。特定の種類のネイティブ データ ソースを参照するには、 [DataSourceNames](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSourceNames.aspx) クラスからフィールドを返す [SourceName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSource.SourceName.aspx) プロパティを使用します。カスタム データ ソースを参照するには、データ ソースの [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSource.SubTypeId.aspx) プロパティを使用します。この値は、PerformancePoint サービス web.config ファイルに登録された、データ ソース拡張機能の **subType** 属性と同じです。
    
    このメソッドを変更する場合、サンプル フィルターのデータ プロバイダー内の  [GetDisplayDataInternal](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetDisplayDataInternal.aspx) メソッドに、対応する変更を加える必要があります。
    
  
15.  [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) プロパティで表される、フィルターの開始ポイントを定義します。これは、フィルター値のソースを定義し、フィルターでデータをスコアカードおよびレポートに送信できるようにするために必要とされます。
    
1.  [ParameterDefinition](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.aspx) オブジェクトを作成します。 [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) は、 [ParameterDefinition](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.aspx) オブジェクトを 1 つだけ含んだ [ParameterDefinitionCollection](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinitionCollection.aspx) オブジェクトを返します。
    
  
2. フィルターのデータ プロバイダーを指定するには、 [ParameterProviderId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.ParameterProviderId.aspx) プロパティを、データ プロバイダーの一意の識別子に設定します。この値は、データ プロバイダーの [GetId()](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetId.aspx) メソッドから返される値に一致する必要があります。
    
  
3. フィルター値のキー識別子のソースを指定するには、 [KeyColumn](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.KeyColumn.aspx) プロパティを、キー識別子を含む表示データ テーブル内の列に設定します。サンプル フィルター エディターでは、このプロパティは、"Symbol" 列として定義されています。
    
  
4. フィルター コントロールの表示値のソースを指定するには、 [DisplayColumn](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.DisplayColumn.aspx) プロパティを、表示値を含む表示データ テーブル内の列に設定します。サンプル フィルター エディターでは、このプロパティは、"Symbol" 列として定義されています。
    
  

    > **メモ**
      > 表示データ テーブルは、 [DisplayValues](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.DisplayValues.aspx) プロパティによって返され、フィルター データ プロバイダーが [GetDisplayDataInternal](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetDisplayDataInternal.aspx) メソッドを呼び出すときに初期化されます。データ テーブルに他の列が含まれる場合、他の列マッピングを定義して、追加の機能を提供できます。

  ```cs
  
if (0 == filter.BeginPoints.Count)
{
    ParameterDefinition paramDef = new ParameterDefinition();

    // Reference the data provider.
    paramDef.ParameterProviderId = "SampleFilterDataProvider";
    paramDef.DefaultPostFormula = string.Empty;

    // Specify the column that contains the key identifiers and the column
    // that contains the display values. The sample uses the same column
    // for both purposes.
    // These values must match the structure of the data table that is
    // returned by the ParameterDefinition.DisplayValues property.

    paramDef.KeyColumn = "Symbol";
    paramDef.DisplayColumn = "Symbol";

    // You can use this property to store custom information for this filter.
    paramDef.CustomDefinition = string.Empty;
    
    filter.BeginPoints.Add(paramDef);
}
  ```


  ```VB.net
  
If 0 = filter.BeginPoints.Count Then
        Dim paramDef As New ParameterDefinition()

        ' Reference the data provider.
      paramDef.ParameterProviderId = "SampleFilterDataProvider"
      paramDef.DefaultPostFormula = String.Empty

        ' Specify the column that contains the key identifiers and the column
        ' that contains the display values. The sample uses the same column
        ' for both purposes.
        ' These values must match the structure of the data table that is
        ' returned by the ParameterDefinition.DisplayValues property.

      paramDef.KeyColumn = "Symbol"
      paramDef.DisplayColumn = "Symbol"

        ' You can use this property to store custom information for this filter.
      paramDef.CustomDefinition = String.Empty

      filter.BeginPoints.Add(paramDef)
End If
  ```


    サンプル エディターでは、その開始ポイントが **VerifyFilter** メソッドに定義されています。また、 **VerifyFilter** を使用して、必要なプロパティが設定されていることが検証され、選択モード (オプションのプロパティ) が定義されています。
    
  
16. フィルターのクエリを実行し、データ ソースからデータを取得して、フィルターを初期化します。サンプル フィルター エディター内のメソッドが **buttonOK_Click** メソッドが **FilterRepositoryHelper.GetParameterDisplayData** メソッドを呼び出して、フィルターを初期化します。
    
    > **メモ**
      > エディターは、フィルター オブジェクトを更新する前に、 **FilterRepositoryHelper.GetParameterDisplayData** を 1 回以上呼び出す必要があります。
17. ユーザー定義の変更を使用してフィルターを更新します。サンプル フィルター エディター内の **buttonOK_Click** メソッドは、 **FilterRepositoryHelper.Update** メソッドを呼び出して、リポジトリ内にあるフィルターの [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 、および [DataSourceLocation](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.DataSourceLocation.aspx) プロパティを更新します。また、 **buttonOK_Click** を使用して、コントロールのコンテンツを検証し、カスタム フィルターとヘルパー オブジェクトの状態情報を取得します。
    
    > **メモ**
      > ユーザーは、カスタム オブジェクトの  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 、および [Owner](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Owner.aspx) ([ **責任者**]) プロパティを設定し、ダッシュボード デザイナー および PerformancePoint サービス リポジトリからカスタム オブジェクトを直接削除することができます。 

## コード サンプル: Create, retrieve, and update custom filters in SharePoint Server 2013 でのカスタム PerformancePoint サービス フィルターの作成、取得、および更新
<a name="bk_example"> </a>

以下のコード サンプルでは、カスタム フィルターを作成、取得、および更新します。このコードは、エディターの分離コード クラスから取られたもので、ASPX ページで定義されるコントロールのプログラミング ロジックとして使用できます。
  
    
    
このコード サンプルをコンパイルできるようにするには、「 [PerformancePoint Services でのフィルター エディターのエディター クラスの作成と構成](#BKMK_ConfigFEd)」の説明に従って、開発環境を構成する必要があります。
  
    
    



```cs

using System;
using System.Collections;
using System.Collections.Generic;
using System.Data;
using System.Web.UI;
using System.Web.UI.WebControls;
using Microsoft.PerformancePoint.Scorecards; 
using Microsoft.PerformancePoint.Scorecards.ServerCommon;
using Microsoft.PerformancePoint.Scorecards.ServerRendering;

namespace Microsoft.PerformancePoint.SDK.Samples.SampleFilter
{

    // Represents the class that defines the sample filter editor.
    public class SampleFilterEditor : Page
    {

        // Declare private variables for the ASP.NET controls defined in the user interface.
        // The sample's user interface is an ASPX page that defines the controls in HTML.
        private TextBox textboxName;
        private TextBox textboxDescription;
        private Label labelErrorMessage;
        private DropDownList dropdownlistDataSource;
        private Button buttonOK;
        private ListBox listboxStocks;

        // Make the controls available to this class.
        protected override void CreateChildControls()
        {
            base.CreateChildControls();

            if (null == textboxName) 
                textboxName = FindControl("textboxName") as TextBox;
            if (null == textboxDescription) 
                textboxDescription = FindControl("textboxDescription") as TextBox;
            if (null == dropdownlistDataSource) 
                dropdownlistDataSource = FindControl("dropdownlistDataSource") as DropDownList;
            if (null == labelErrorMessage)
                labelErrorMessage = FindControl("labelErrorMessage") as Label;
            if (null==buttonOK)
                buttonOK = FindControl("buttonOK") as Button;
            if (null==listboxStocks)
                listboxStocks = FindControl("listboxStocks") as ListBox;
        }

        // Handles the Load event of the Page control.
        // Methods that use a control variable should call the Control.EnsureChildControls
        // method before accessing the variable for the first time.
        protected void Page_Load(object sender, EventArgs e)
        {

            // Required to enable custom report and filter editors to
            // write data to the repository.
            ServerUtils.AllowUnsafeUpdates = true;

            // Initialize controls the first time the page loads only.
            if (!IsPostBack)
            {
                EnsureChildControls();
                FilterRepositoryHelper filterRepositoryHelper = null;
                try
                {

                    // Get information from the query string parameters.
                    string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];
                    string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];
                    string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];

                    // Validate the query string parameters.
                    if (string.IsNullOrEmpty(server) ||
                        string.IsNullOrEmpty(itemLocation) ||
                        string.IsNullOrEmpty(action))
                    {
                        displayError("Invalid URL.");
                        return;
                    }

                    // Retrieve the repository-helper object.
                    filterRepositoryHelper =
                        new FilterRepositoryHelper();

                    // Set the filter location.
                    RepositoryLocation repositoryFilterLocation = RepositoryLocation.CreateFromUriString(itemLocation);

                    Filter filter;
                    DataSource datasource = null;

                    // Retrieve or create the filter object, depending on the operation
                    // passed in the query string (OpenItem or CreateItem).
                    if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Retrieve the filter object by using the repository-helper object.
                        filter = filterRepositoryHelper.Get(repositoryFilterLocation);
                        if (filter == null)
                        {
                            displayError("Could not retrieve the filter for editing.");
                            return;
                        }

                    }
                    else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Create a filter.
                        // CreateItem requests can be sent from a SharePoint list, but
                        // you must create a custom menu item to send the request.
                        // Dashboard Designer can send edit requests only.
                        filter = new Filter
                            {

                                // Specify the class that defines the renderer
                                // web server control. The sample filter uses a native
                                // PerformancePoint Services renderer.
                                // Defaults to the value specified in the web.config file
                                RendererClassName = typeof(MultiSelectTreeViewControl).AssemblyQualifiedName,

                                // Specify the unique identifier for the filter.
                                // The SubTypeId property must match the
                                // subType attribute in the web.config file.
                                SubTypeId = "SampleFilter"
                            };
                    }
                    else
                    {
                        displayError("Invalid Action.");
                        return;
                    }

                    VerifyFilter(filter);

                    // Retrieve filter's underlying data source.
                    if (!string.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl))
                    {
                        RepositoryLocation repositoryDatasourceLocation =
                            RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl);
                        datasource =

                            // Gets a PerformancePoint Services data source by using the
                            // DataSourceHelper property to call the
                            // DataSourceConsumerHelper.GetDataSource method. 
                            filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation);
                    }

                    // Save the original filter and helper objects across page postbacks.
                    ViewState["action"] = action;
                    ViewState["filter"] = filter;
                    ViewState["filterrepositoryhelper"] = filterRepositoryHelper;
                    ViewState["itemlocation"] = itemLocation;

                    // Populate the child controls.
                    textboxName.Text = filter.Name.ToString();
                    textboxDescription.Text = filter.Description.ToString();

                    // Populate the dropdownlistDataSource control with data sources of specific
                    // types from the PerformancePoint Services repository.
                    // This method looks up the passed data source in the data sources
                    // that are registered in the web.config file. 
                    // Although the sample retrieves data sources of two specific types,
                    // you can modify it to prompt the user for a data source type.
                    PopulateDataSourceDropDown(datasource);

                    // Call the SelectedIndexChanged event directly to populate the 
                    // listbox control with preview data.
                    dropdownlistDataSource_SelectedIndexChanged(null, null);
                }
                catch (Exception ex)
                {
                    displayError("An error has occurred. Please contact your administrator for more information.");
                    if (filterRepositoryHelper != null)
                    {
                        // Add the exception detail to the server event log.
                        filterRepositoryHelper.HandleException(ex);
                    }
                }
            }
        }

        // Handles the SelectedIndexChanged event of the dropdownlistDataSource control.
        protected void dropdownlistDataSource_SelectedIndexChanged(object sender, EventArgs e)
        {
            EnsureChildControls();

            // Check if a valid data source is selected.
            if (null != dropdownlistDataSource.SelectedItem &amp;&amp;
                !string.IsNullOrEmpty(dropdownlistDataSource.SelectedItem.Text))
            {
                // Retrieve the data source object.
                FilterRepositoryHelper filterRepositoryHelper =
                    (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];
                string selectedDataSourceItemUrl = dropdownlistDataSource.SelectedItem.Value;

                RepositoryLocation repositoryDatasourceLocation =
                    RepositoryLocation.CreateFromUriString(selectedDataSourceItemUrl);
                DataSource datasource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation);
                ViewState["datasource"] = datasource;

                // Populate the listboxStocks control with the preview data for the selected
                // data source.
                PopulateListBoxData(datasource);
            }
            else
            {
                ClearStocksListBox();
            }
        }

        // Clears the listboxStocks control.
        // The sample filter works with a web service that provides stock information.
        private void ClearStocksListBox()
        {
            listboxStocks.DataSource = null;
            listboxStocks.DataBind();
            listboxStocks.Items.Clear();
        }

        // Handles the Click event of the buttonOK control.
        protected void buttonOK_Click(object sender, EventArgs e)
        {
            EnsureChildControls();

            // Verify that the controls contain values.
            if (string.IsNullOrEmpty(textboxName.Text))
            {
                labelErrorMessage.Text = "A filter name is required.";
                return;
            }
            if (dropdownlistDataSource.SelectedIndex == 0)
            {
                labelErrorMessage.Text = "A data source is required.";
                return;
            }

            // Clear any pre-existing error message.
            labelErrorMessage.Text = string.Empty;

            // Retrieve the filter, data source, and helper objects from view state.
            string action = (string)ViewState["action"];
            string itemLocation = (string) ViewState["itemlocation"];
            Filter filter = (Filter)ViewState["filter"];
            DataSource datasource = (DataSource)ViewState["datasource"];
            FilterRepositoryHelper filterRepositoryHelper = (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];

            // Update the filter object with form changes.
            filter.Name.Text = textboxName.Text;
            filter.Description.Text = textboxDescription.Text;
            filter.DataSourceLocation = datasource.Location;
            foreach (ParameterDefinition parameterDefinition in filter.BeginPoints)
            {
                parameterDefinition.DisplayName = filter.Name.Text;
            }

            // Initialize the filter. This method runs the filter's query and retrieves preview data.
            filterRepositoryHelper.GetParameterDisplayData(ref filter);

            // Save the filter object to the PerformancePoint Services repository.
            try
            {
                filter.Validate();

                if (ClickOnceLaunchValues.CreateItem.Equals(action,StringComparison.OrdinalIgnoreCase))
                {
                    Filter newFilter = filterRepositoryHelper.Create(
                        string.IsNullOrEmpty(filter.Location.ItemUrl) ? itemLocation : filter.Location.ItemUrl, filter);
                    ViewState["filter"] = newFilter;
                    ViewState["action"] = ClickOnceLaunchValues.OpenItem;
                }
                else
                {
                    filterRepositoryHelper.Update(filter);
                }
            }
            catch (Exception ex)
            {
                displayError("An error has occurred. Please contact your administrator for more information.");
                if (filterRepositoryHelper != null)
                {

                    // Add the exception detail to the server event log.
                    filterRepositoryHelper.HandleException(ex);
                }
            }
        }

        // Displays the error string in the labelErrorMessage label.
        void displayError(string msg)
        {
            EnsureChildControls();
            
            labelErrorMessage.Text = msg;

            // Disable the OK button because the page is in an error state.
            buttonOK.Enabled = false;
            return;
        }

        // Verifies that the properties for the filter object are set.
        static void VerifyFilter(Filter filter)
        {

            if (null != filter)
            {

                // Verify that all required properties are set.
                if (string.IsNullOrEmpty(filter.SubTypeId)) 
                {

                    // This value must match the subType attribute specified
                    // in the web.config file.
                    filter.SubTypeId = "SampleFilter";
                }

                if (string.IsNullOrEmpty(filter.RendererClassName)) 
                {
                    filter.RendererClassName = typeof (MultiSelectTreeViewControl).AssemblyQualifiedName; 
                }

                // Define the BeginPoints property so the filter can send a parameter value to
                // scorecards and reports.
                // The value must be from the KeyColumn of the display
                // DataTable object, which is defined in the data provider. The data table is
                // returned by the FilterRepositoryHelper.GetParameterDisplayData method.
                // A filter has one beginpoint only, and it is represented by a
                // ParameterDefinition object. The ParameterDefinition object defines how
                // the filter accesses the data.
                if (0 == filter.BeginPoints.Count)
                {
                    ParameterDefinition paramDef = new ParameterDefinition
                                                       {
                                                           // This value must match the value returned 
                                                           // by the data provider's GetId method.
                                                           ParameterProviderId = "SampleFilterDataProvider",

                                                           // Reference the data provider.
                                                           DefaultPostFormula = string.Empty,

                                                           // Specify the column that contains
                                                           // the key identifiers and the column
                                                           // that contains the display values.
                                                           // The sample uses the same column
                                                           // for both purposes.
                                                           // These values must match the structure
                                                           // of the data table that is returned
                                                           // by the ParameterDefinition.DisplayValues property.
                                                           KeyColumn = "Symbol",
                                                           DisplayColumn = "Symbol",

                                                           // You can use this property to store
                                                           // extra information for this filter.
                                                           CustomDefinition = string.Empty
                                                       };
                    filter.BeginPoints.Add(paramDef);
                }

                // Set optional properties. The renderer can return multiple values. 
                filter.SelectionMode = FilterSelectionMode.MultiSelect;
            }
        }

        // Populates the dropdownlistDataSource control.
        void PopulateDataSourceDropDown(DataSource filterDataSource)
        {
            EnsureChildControls();
            
            FilterRepositoryHelper filterRepositoryHelper =
                (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];

            // Retrieve data sources from the repository by using the DataSourceHelper
            // property to call the DataSourceConsumerHelper object.
            // If you modify the types of data source to retrieve, you must make the corresponding
            // change in the filter's data provider.
            // The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
            // Edit this value if you are not using the default path. A leading forward slash may not be needed.
            ICollection dataSourceCollection = filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames("/BICenter/Data%20Connections%20for%20PerformancePoint/",
                new[] { "WSTabularDataSource", DataSourceNames.ExcelWorkbook });
            if (null == dataSourceCollection)
            {
                displayError("No available data sources were found.");
                return;
            }

            // Create a list of name/value pairs for the dropdownlistDataSource control.
            var dataSources = new List<KeyValuePair<string, string>>();
            int selectedIndex = 0;
            int i = 1;
            dataSources.Add(new KeyValuePair<string, string>(string.Empty, string.Empty));

            foreach (DataSource ds in dataSourceCollection)
            {
                dataSources.Add(new KeyValuePair<string, string>(ds.Name.Text, ds.Location.ItemUrl));

                // Check if the entry is the originally selected data source.
                if ((filterDataSource != null) &amp;&amp;
                    (string.Compare(ds.Name.Text, filterDataSource.Name.Text) == 0))
                {
                    selectedIndex = i;
                }
                ++i;
            }

            dropdownlistDataSource.DataSource = dataSources;
            dropdownlistDataSource.DataTextField = "Key";
            dropdownlistDataSource.DataValueField = "Value";
            dropdownlistDataSource.DataBind();
            dropdownlistDataSource.SelectedIndex = selectedIndex;
        }


        // Populate the list box data.
        void PopulateListBoxData(DataSource datasource)
        {
            EnsureChildControls();
            
            ClearStocksListBox();
            
            FilterRepositoryHelper filterRepositoryHelper =
                (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];

            // Retrieve the first 100 rows of the preview data from the data source
            DataSet dataSet = filterRepositoryHelper.DataSourceHelper.GetDataSet(100, datasource);

            if (null != dataSet &amp;&amp; null != dataSet.Tables[0])
            {
                listboxStocks.DataTextField = "Symbol";
                listboxStocks.DataValueField = "Value";
                listboxStocks.DataSource = dataSet.Tables[0];
                listboxStocks.DataBind();
            }
        }
    }
}
```




```VB.net

'INSTANT VB NOTE: This code snippet uses implicit typing. You will need to set 'Option Infer On' in the VB file or set 'Option Infer' at the project level:

Imports System
Imports System.Collections
Imports System.Collections.Generic
Imports System.Data
Imports System.Web.UI
Imports System.Web.UI.WebControls
Imports Microsoft.PerformancePoint.Scorecards
Imports Microsoft.PerformancePoint.Scorecards.ServerCommon
Imports Microsoft.PerformancePoint.Scorecards.ServerRendering

Namespace Microsoft.PerformancePoint.SDK.Samples.SampleFilter

    ' Represents the class that defines the sample filter editor.
    Public Class SampleFilterEditor
        Inherits Page

        ' Declare private variables for the ASP.NET controls defined in the user interface.
        ' The sample's user interface is an ASPX page that defines the controls in HTML.
        Private textboxName As TextBox
        Private textboxDescription As TextBox
        Private labelErrorMessage As Label
        Private dropdownlistDataSource As DropDownList
        Private buttonOK As Button
        Private listboxStocks As ListBox

        ' Make the controls available to this class.
        Protected Overrides Sub CreateChildControls()
            MyBase.CreateChildControls()

            If Nothing Is textboxName Then
                textboxName = TryCast(FindControl("textboxName"), TextBox)
            End If
            If Nothing Is textboxDescription Then
                textboxDescription = TryCast(FindControl("textboxDescription"), TextBox)
            End If
            If Nothing Is dropdownlistDataSource Then
                dropdownlistDataSource = TryCast(FindControl("dropdownlistDataSource"), DropDownList)
            End If
            If Nothing Is labelErrorMessage Then
                labelErrorMessage = TryCast(FindControl("labelErrorMessage"), Label)
            End If
            If Nothing Is buttonOK Then
                buttonOK = TryCast(FindControl("buttonOK"), Button)
            End If
            If Nothing Is listboxStocks Then
                listboxStocks = TryCast(FindControl("listboxStocks"), ListBox)
            End If
        End Sub

        ' Handles the Load event of the Page control.
        ' Methods that use a control variable should call the Control.EnsureChildControls
        ' method before accessing the variable for the first time.
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As EventArgs)

            ' Required to enable custom report and filter editors to
            ' write data to the repository.
            ServerUtils.AllowUnsafeUpdates = True

            ' Initialize controls the first time the page loads only.
            If Not IsPostBack Then
                EnsureChildControls()
                Dim filterRepositoryHelper As FilterRepositoryHelper = Nothing
                Try

                    ' Get information from the query string parameters.
                    Dim server As String = Request.QueryString(ClickOnceLaunchKeys.SiteCollectionUrl)
                    Dim itemLocation As String = Request.QueryString(ClickOnceLaunchKeys.ItemLocation)
                    Dim action As String = Request.QueryString(ClickOnceLaunchKeys.LaunchOperation)

                    ' Validate the query string parameters.
                    If String.IsNullOrEmpty(server) OrElse String.IsNullOrEmpty(itemLocation) OrElse String.IsNullOrEmpty(action) Then
                        displayError("Invalid URL.")
                        Return
                    End If

                    ' Retrieve the repository-helper object.
                    filterRepositoryHelper = New FilterRepositoryHelper()

                    ' Set the filter location.
                    Dim repositoryFilterLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(itemLocation)

                    Dim filter As Filter
                    Dim datasource As DataSource = Nothing

                    ' Retrieve or create the filter object, depending on the operation
                    ' passed in the query string (OpenItem or CreateItem).
                    If ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then

                        ' Retrieve the filter object by using the repository-helper object.
                        filter = filterRepositoryHelper.Get(repositoryFilterLocation)
                        If filter Is Nothing Then
                            displayError("Could not retrieve the filter for editing.")
                            Return
                        End If

                    ElseIf ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then

                        ' Create a filter.
                        ' CreateItem requests can be sent from a SharePoint list, but
                        ' you must create a custom menu item to send the request.
                        ' Dashboard Designer can send edit requests only.
                        filter = New Filter With {.RendererClassName = GetType(MultiSelectTreeViewControl).AssemblyQualifiedName, .SubTypeId = "SampleFilter"}
                        ' Specify the class that defines the renderer
                        ' web server control. The sample filter uses a native
                        ' PerformancePoint Services renderer.
                        ' Defaults to the value specified in the web.config file
                        ' Specify the unique identifier for the filter.
                        ' The SubTypeId property must match the
                        ' subType attribute in the web.config file.
                    Else
                        displayError("Invalid Action.")
                        Return
                    End If

                    VerifyFilter(filter)

                    ' Retrieve filter's underlying data source.
                    If Not String.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl) Then
                        Dim repositoryDatasourceLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl)
                        ' Gets a PerformancePoint Services data source by using the
                        ' DataSourceHelper property to call the
                        ' DataSourceConsumerHelper.GetDataSource method. 
                        datasource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation)
                    End If

                    ' Save the original filter and helper objects across page postbacks.
                    ViewState("action") = action
                    ViewState("filter") = filter
                    ViewState("filterrepositoryhelper") = filterRepositoryHelper
                    ViewState("itemlocation") = itemLocation

                    ' Populate the child controls.
                    textboxName.Text = filter.Name.ToString()
                    textboxDescription.Text = filter.Description.ToString()

                    ' Populate the dropdownlistDataSource control with data sources of specific
                    ' types from the PerformancePoint Services repository.
                    ' This method looks up the passed data source in the data sources
                    ' that are registered in the web.config file. 
                    ' Although the sample retrieves data sources of two specific types,
                    ' you can modify it to prompt the user for a data source type.
                    PopulateDataSourceDropDown(datasource)

                    ' Call the SelectedIndexChanged event directly to populate the 
                    ' listbox control with preview data.
                    dropdownlistDataSource_SelectedIndexChanged(Nothing, Nothing)
                Catch ex As Exception
                    displayError("An error has occurred. Please contact your administrator for more information.")
                    If filterRepositoryHelper IsNot Nothing Then
                        ' Add the exception detail to the server event log.
                        filterRepositoryHelper.HandleException(ex)
                    End If
                End Try
            End If
        End Sub

        ' Handles the SelectedIndexChanged event of the dropdownlistDataSource control.
        Protected Sub dropdownlistDataSource_SelectedIndexChanged(ByVal sender As Object, ByVal e As EventArgs)
            EnsureChildControls()

            ' Check if a valid data source is selected.
            If Nothing IsNot dropdownlistDataSource.SelectedItem AndAlso (Not String.IsNullOrEmpty(dropdownlistDataSource.SelectedItem.Text)) Then
                ' Retrieve the data source object.
                Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)
                Dim selectedDataSourceItemUrl As String = dropdownlistDataSource.SelectedItem.Value

                Dim repositoryDatasourceLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(selectedDataSourceItemUrl)
                Dim datasource As DataSource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation)
                ViewState("datasource") = datasource

                ' Populate the listboxStocks control with the preview data for the selected
                ' data source.
                PopulateListBoxData(datasource)
            Else
                ClearStocksListBox()
            End If
        End Sub

        ' Clears the listboxStocks control.
        ' The sample filter works with a web service that provides stock information.
        Private Sub ClearStocksListBox()
            listboxStocks.DataSource = Nothing
            listboxStocks.DataBind()
            listboxStocks.Items.Clear()
        End Sub

        ' Handles the Click event of the buttonOK control.
        Protected Sub buttonOK_Click(ByVal sender As Object, ByVal e As EventArgs)
            EnsureChildControls()

            ' Verify that the controls contain values.
            If String.IsNullOrEmpty(textboxName.Text) Then
                labelErrorMessage.Text = "A filter name is required."
                Return
            End If
            If dropdownlistDataSource.SelectedIndex = 0 Then
                labelErrorMessage.Text = "A data source is required."
                Return
            End If

            ' Clear any pre-existing error message.
            labelErrorMessage.Text = String.Empty

            ' Retrieve the filter, data source, and helper objects from view state.
            Dim action As String = CStr(ViewState("action"))
            Dim itemLocation As String = CStr(ViewState("itemlocation"))
            Dim filter As Filter = CType(ViewState("filter"), Filter)
            Dim datasource As DataSource = CType(ViewState("datasource"), DataSource)
            Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)

            ' Update the filter object with form changes.
            filter.Name.Text = textboxName.Text
            filter.Description.Text = textboxDescription.Text
            filter.DataSourceLocation = datasource.Location
            For Each parameterDefinition As ParameterDefinition In filter.BeginPoints
                parameterDefinition.DisplayName = filter.Name.Text
            Next parameterDefinition

            ' Initialize the filter. This method runs the filter's query and retrieves preview data.
            filterRepositoryHelper.GetParameterDisplayData(filter)

            ' Save the filter object to the PerformancePoint Services repository.
            Try
                filter.Validate()

                If ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then
                    Dim newFilter As Filter = filterRepositoryHelper.Create(If(String.IsNullOrEmpty(filter.Location.ItemUrl), itemLocation, filter.Location.ItemUrl), filter)
                    ViewState("filter") = newFilter
                    ViewState("action") = ClickOnceLaunchValues.OpenItem
                Else
                    filterRepositoryHelper.Update(filter)
                End If
            Catch ex As Exception
                displayError("An error has occurred. Please contact your administrator for more information.")
                If filterRepositoryHelper IsNot Nothing Then

                    ' Add the exception detail to the server event log.
                    filterRepositoryHelper.HandleException(ex)
                End If
            End Try
        End Sub

        ' Displays the error string in the labelErrorMessage label.
        Private Sub displayError(ByVal msg As String)
            EnsureChildControls()

            labelErrorMessage.Text = msg

            ' Disable the OK button because the page is in an error state.
            buttonOK.Enabled = False
            Return
        End Sub

        ' Verifies that the properties for the filter object are set.
        Private Shared Sub VerifyFilter(ByVal filter As Filter)

            If Nothing IsNot filter Then

                ' Verify that all required properties are set.
                If String.IsNullOrEmpty(filter.SubTypeId) Then

                    ' This value must match the subType attribute specified
                    ' in the web.config file.
                    filter.SubTypeId = "SampleFilter"
                End If

                If String.IsNullOrEmpty(filter.RendererClassName) Then
                    filter.RendererClassName = GetType(MultiSelectTreeViewControl).AssemblyQualifiedName
                End If

                ' Define the BeginPoints property so the filter can send a parameter value to
                ' scorecards and reports.
                ' The value must be from the KeyColumn of the display
                ' DataTable object, which is defined in the data provider. The data table is
                ' returned by the FilterRepositoryHelper.GetParameterDisplayData method.
                ' A filter has one beginpoint only, and it is represented by a
                ' ParameterDefinition object. The ParameterDefinition object defines how
                ' the filter accesses the data.
                If 0 = filter.BeginPoints.Count Then
                    Dim paramDef As ParameterDefinition = New ParameterDefinition With {.ParameterProviderId = "SampleFilterDataProvider", .DefaultPostFormula = String.Empty, .KeyColumn = "Symbol", .DisplayColumn = "Symbol", .CustomDefinition = String.Empty}
                    ' This value must match the value returned 
                    ' by the data provider's GetId method.
                    ' Reference the data provider.
                    ' Specify the column that contains
                    ' the key identifiers and the column
                    ' that contains the display values.
                    ' The sample uses the same column
                    ' for both purposes.
                    ' These values must match the structure
                    ' of the data table that is returned
                    ' by the ParameterDefinition.DisplayValues property.
                    ' You can use this property to store
                    ' extra information for this filter.
                    filter.BeginPoints.Add(paramDef)
                End If

                ' Set optional properties. The renderer can return multiple values. 
                filter.SelectionMode = FilterSelectionMode.MultiSelect
            End If
        End Sub

        ' Populates the dropdownlistDataSource control.
        Private Sub PopulateDataSourceDropDown(ByVal filterDataSource As DataSource)
            EnsureChildControls()

            Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)

            ' Retrieve data sources from the repository by using the DataSourceHelper
            ' property to call the DataSourceConsumerHelper object.
            ' If you modify the types of data source to retrieve, you must make the corresponding
            ' change in the filter's data provider.
            ' The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
            ' Edit this value if you are not using the default path. A leading forward slash may not be needed.
            Dim dataSourceCollection As ICollection = filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames("/BICenter/Data%20Connections%20for%20PerformancePoint/", {"WSTabularDataSource", DataSourceNames.ExcelWorkbook})
            If Nothing Is dataSourceCollection Then
                displayError("No available data sources were found.")
                Return
            End If

            ' Create a list of name/value pairs for the dropdownlistDataSource control.
            Dim dataSources = New List(Of KeyValuePair(Of String, String))()
            Dim selectedIndex As Integer = 0
            Dim i As Integer = 1
            dataSources.Add(New KeyValuePair(Of String, String)(String.Empty, String.Empty))

            For Each ds As DataSource In dataSourceCollection
                dataSources.Add(New KeyValuePair(Of String, String)(ds.Name.Text, ds.Location.ItemUrl))

                ' Check if the entry is the originally selected data source.
                If (filterDataSource IsNot Nothing) AndAlso (String.Compare(ds.Name.Text, filterDataSource.Name.Text) = 0) Then
                    selectedIndex = i
                End If
                i += 1
            Next ds

            dropdownlistDataSource.DataSource = dataSources
            dropdownlistDataSource.DataTextField = "Key"
            dropdownlistDataSource.DataValueField = "Value"
            dropdownlistDataSource.DataBind()
            dropdownlistDataSource.SelectedIndex = selectedIndex
        End Sub


        ' Populate the list box data.
        Private Sub PopulateListBoxData(ByVal datasource As DataSource)
            EnsureChildControls()

            ClearStocksListBox()

            Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)

            ' Retrieve the first 100 rows of the preview data from the data source
            Dim dataSet As DataSet = filterRepositoryHelper.DataSourceHelper.GetDataSet(100, datasource)

            If Nothing IsNot dataSet AndAlso Nothing IsNot dataSet.Tables(0) Then
                listboxStocks.DataTextField = "Symbol"
                listboxStocks.DataValueField = "Value"
                listboxStocks.DataSource = dataSet.Tables(0)
                listboxStocks.DataBind()
            End If
        End Sub
    End Class
End Namespace
```


## 次の手順
<a name="bk_example"> </a>

フィルター エディター (必要な場合は、そのユーザー インターフェイスも含む) とデータ プロバイダーを作成した後は、「 [[方法] PerformancePoint Services の拡張機能を手動で登録する](http://msdn.microsoft.com/library/3aa6d340-4b05-46b3-9648-2b6e18e04e09%28Office.15%29.aspx)」に説明されているとおりに拡張機能を展開します。
  
    
    

## その他の技術情報
<a name="bk_addResources"> </a>


-  [[方法] SharePoint 2013 の PerformancePoint Services 用にフィルター データ プロバイダーを作成する](how-to-create-filter-data-providers-for-performancepoint-services-in-sharepoint.md)
    
  
-  [SharePoint 2013 の PerformancePoint Services](performancepoint-services-in-sharepoint-2013.md)
    
  

