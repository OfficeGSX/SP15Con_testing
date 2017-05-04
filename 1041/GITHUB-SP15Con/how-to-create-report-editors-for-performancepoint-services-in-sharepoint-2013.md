---
title: [方法] SharePoint 2013 の PerformancePoint Services 用のレポート エディタを作成する
ms.prod: SHAREPOINT
ms.assetid: b42b4452-90f8-464c-828f-d3abac40670c
---


# [方法] SharePoint 2013 の PerformancePoint Services 用のレポート エディタを作成する
PerformancePoint サービス 用のカスタム レポート拡張機能のコンポーネントを作成する方法について説明します。
## PerformancePoint サービス 用カスタム レポート エディターとは何か。
<a name="bi_intro"> </a>

PerformancePoint サービス では、カスタム レポート エディターにより、ユーザーはカスタム レポートのプロパティを設定できます。レポート エディターは、レポート エンドポイントを初期化します。レポート エンドポイントは、スコアカードとフィルター プロバイダーからパラメーター値を受け取ります。エディターの要件と機能の詳細については、「 [カスタム PerformancePoint Services オブジェクトのエディター](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)」を参照してください。
  
    
    
以下の手順と例は、 [カスタム オブジェクト サンプル](http://msdn.microsoft.com/library/af021d52-7562-4e7a-9de4-e1fc5784a59d%28Office.15%29.aspx) の **SampleReportViewEditor** クラスに基づいています。 エディターは、ユーザーがレポートの名前と詳細を変更できるシン Web アプリケーションです。クラスの完全なコードについては、「 [コードの例: SharePoint Server 2013のカスタム PerformancePoint サービス レポートの作成、取得、更新](#bk_example)」を参照してください。
  
    
    
サンプル エディターをテンプレートとして使用することをお勧めします。サンプルは、PerformancePoint サービス API でオブジェクトを呼び出す方法、(オブジェクトの作成、更新などの) リポジトリ操作のためにヘルパー オブジェクトによる呼び出しを単純化する方法、PerformancePoint サービス 開発のためのベスト プラクティスを示します。
  
    
    

## カスタム PerformancePoint サービス レポート用エディターの作成
<a name="BKMK_ConfigREditor"> </a>


  
    
    

1. PerformancePoint サービス をインストールするか、拡張機能が使用する (手順 3. で示した) DLL をコンピューターにコピーします。詳細については、「 [開発シナリオで使用される PerformancePoint Services DLL](http://msdn.microsoft.com/library/41e92619-8253-481d-82f9-35b6a6abc477%28Office.15%29.aspx)」を参照してください。
    
  
2. Visual Studio で、C# クラス ライブラリを作成します。拡張機能のためのクラス ライブラリを既に作成している場合、新しい C# クラスを追加します。
    
    DLL には厳密な名前で署名する必要があります。さらに、DLL によって参照されたすべてのアセンブリが厳密な名前を持つことを確認してください。厳密な名前を使用してアセンブリに署名する方法の詳細、および公開/秘密キーのペアを作成する方法の詳細については、「 [How to: Create a public/private key pair](http://msdn.microsoft.com/library/05026813-f3bd-4d7c-9e0b-fc588eb3d114.aspx)」を参照してください。
    
  
3. プロジェクトに、アセンブリ参照として以下の DLL を追加します。
    
  - Microsoft.PerformancePoint.Scorecards.Client.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerCommon.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.Store.dll (ヘルパー クラスが使用)
    
  
  - Microsoft.SharePoint.dll (ヘルパー クラスが使用)
    
  

    サンプル エディターには、System.Web.dll と System.Web.Services.dll へのアセンブリ参照も含まれます。拡張機能の機能によっては、その他のプロジェクト参照が必要になることがあります。
    
  
4. サンプルから以下のクラスをプロジェクトに追加します。エディターは、PerformancePoint サービス リポジトリを操作するためにこれらのヘルパークラスを使用します。
    
  - DataSourceConsumerHelper.cs
    
  
  - ExtensionRepositoryHelper.cs
    
  
  - ReportViewRepositoryHelper.cs
    
  
  - IDataSourceConsumer.cs
    
  

    > **メモ**
      > サンプル レポートでは、データをフィルターから取得します。このため、 **DataSourceConsumerHelper** または **IDataSourceConsumer** オブジェクトは使用しません。ただし、ユーザーのレポートでデータを PerformancePoint サービス データ ソースから取得する場合は、 **DataSourceConsumerHelper** クラスによって公開されるメソッドを使用して、データ ソースを取得できます。詳細については、「 [[方法] SharePoint 2013 の PerformancePoint Services 用のフィルター エディタを作成する](how-to-create-filter-editors-for-performancepoint-services-in-sharepoint-2013.md)」を参照してください。 
5. エディター クラスで、以下の PerformancePoint サービス 名前空間のための **using** ディレクティブを追加します。
    
  - **Microsoft.PerformancePoint.Scorecards**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerCommon**
    
  

    拡張機能の機能によっては、その他の **using** ディレクティブが必要になることがあります。
    
  
6. エディターの実装をサポートする基本クラスから継承します。サンプル レポート エディターは Web アプリケーションなので、 [Page](https://msdn.microsoft.com/library/System.Web.UI.Page.aspx) クラスから継承します。他の実装では、 [UserControl](https://msdn.microsoft.com/library/System.Windows.Forms.UserControl.aspx) 、 [WebPart](https://msdn.microsoft.com/library/System.Web.UI.WebControls.WebParts.WebPart.aspx) クラスなどの基本クラスから派生できます。
    
  
7. ユーザーが表示または変更できるプロパティを表示するコントロールの変数を宣言します。サンプル レポート エディターでは、最初にユーザー インターフェイス コンポーネント (ASPX ページ) で定義されている Web サーバー コントロールの変数を宣言します。また、サンプル エディターは、変更をユーザーが送信できるようにするボタン コントロールも定義します。次に、エディターは  [CreateChildControls()](https://msdn.microsoft.com/library/System.Web.UI.Control.CreateChildControls.aspx) メソッドを呼び出して、コントロールをページで使用できるようにします。
    
    > **メモ**
      > エディターは、ユーザー インターフェイスとは別のプログラミング ロジックを定義します。エディターのユーザー インターフェイス コンポーネントを作成するための説明は、この文書の範囲外となります。 

    サンプル レポート エディターでは、 **Page_Load** メソッドで手順 8. ～ 12. を実行します。 **Page_Load** は、変数とコントロールの初期化と検証、コントロールの事前設定、およびカスタム レポートとヘルパー オブジェクトの状態情報の保存を実行するためにも使用されます。
    
  
8.  [AllowUnsafeUpdates](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ServerCommon.ServerUtils.AllowUnsafeUpdates.aspx) プロパティを **true** に設定します。これにより、レポート エディターはフォーム **POST** 操作を使用せずにデータをリポジトリに書き込むことができます。
    
  
9. クエリ文字列からパラメーターを取得し、それらを以下のコード例で示すローカル変数のための値として設定します。
    
  ```cs
  
// The URL of the site collection that contains the PerformancePoint Services repository.
string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];

// The location of the report in the repository.
string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];

// The operation to perform: OpenItem or CreateItem.
string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];
  ```


クエリ文字列パラメーターの詳細については、「 [カスタム PerformancePoint Services オブジェクトのエディター](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)」を参照してください。
    
  
10. 以下のコード例に示すように、リポジトリに呼び出しをするための **ReportViewRepositoryHelper** オブジェクトを取得します。
    
  ```cs
  
reportviewRepositoryHelper = new ReportViewRepositoryHelper();
  ```

11. 以下のコード例に示すように、クエリ文字列パラメーターに基づいてレポートの場所を設定します。
    
  ```cs
  RepositoryLocation repositoryReportViewLocation = RepositoryLocation.CreateFromUriString(itemLocation);
  ```

12. クエリ文字列から実行する操作 ( _OpenItem_ または _CreateItem_) を取得し、カスタム レポートを取得または作成します。
    
  - カスタム レポートを取得するには、 **ReportViewRepositoryHelper.Get** メソッドを使用します。
    
  
  - カスタム レポートを作成するには、 **ReportView()** コンストラクターを使用し、レポートの [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.RendererClassName.aspx) 、および [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.SubTypeId.aspx) プロパティを定義します。
    
     [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.SubTypeId.aspx) はレポートの一意の識別子であり、PerformancePoint サービス web.config ファイルのカスタム レポートに指定する **subType** 属性と一致している必要があります。 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.RendererClassName.aspx) は、レンダラーの Web サーバー コントロールを定義するクラスの完全修飾名です。これをエディターに定義しない場合、この値は、web.config ファイルに指定されたレンダラー クラスに既定設定されます。
    
  

  ```cs
  if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
    {

        // Use the repository-helper object to retrieve the report.
        reportview = reportviewRepositoryHelper.Get(repositoryReportViewLocation);
        if (reportview == null)
        {
            displayError("Could not retrieve the report view for editing.");
            return;
        }
    }
    else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
    {
        reportview = new ReportView
        {
            RendererClassName = typeof(SampleReportRenderer).AssemblyQualifiedName, 
            SubTypeId = "SampleReportView"
        };
    }
    else
    {
        displayError("Invalid Action.");
        return;
    }
  ```


    > **メモ**
      > 既定では、ユーザーは PerformancePoint ダッシュボード デザイナーからのみカスタム オブジェクトを作成できます。ユーザーにダッシュボード デザイナーの外でカスタム オブジェクトを作成できるようにするには、リポジトリのコンテンツ タイプから  _CreateItem_ 要求をエディターに送るメニュー項目を追加する必要があります。詳細については、「 [カスタム PerformancePoint Services オブジェクトのエディター](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)」を参照してください。 
13. レポートのエンドポイントを定義します。エンドポイントによってレポートは、フィルターとスコアカードからデータを取得できます。サンプル レポート エディターでは、エンドポイントに必要なプロパティを以下のコード例のように定義します。
    
  ```cs
  
if (0 == reportview.EndPoints.Count)
{
    EndPoint endpoint = new EndPoint
    {
        Category = EndPointCategory.None,
        UniqueName = "SampleReportView_EndPoint",

        // The display name is shown to users in Dashboard Designer.
        // It represents the endpoint that can be connected 
        // to a filter or scorecard.
        DisplayName = "Sample Report View EndPoint"
    };

    reportview.EndPoints.Add(endpoint);
}
  ```


サンプル エディターでは、 **VerifyReportView** メソッドでエンドポイントを定義します。また、 **VerifyReportView** を使用して、必要なプロパティが設定されていることを確認し、オプションの [CustomData](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.CustomData.aspx) プロパティを定義します。このプロパティを使用してレポートの情報を格納できます。
    
  
14. レポートをユーザー定義の変更で更新します。サンプル レポート エディターの **buttonOK_Click** メソッドは、 **ReportViewRepositoryHelper.Update** メソッドを呼び出して、リポジトリでレポートの [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) と [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) プロパティを更新します。 **buttonOK_Click** は、コントロールの内容を検証したり、カスタム レポートとヘルパー オブジェクトの状態情報を取得するときにも使用されます。
    
    > **メモ**
      > ユーザーは、カスタム オブジェクトの  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 、および [Owner](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Owner.aspx) ([ **責任者**]) プロパティを設定して、ダッシュボード デザイナー および PerformancePoint サービス リポジトリからカスタム オブジェクトを直接削除することができます。 

## コードの例: SharePoint Server 2013のカスタム PerformancePoint サービス レポートの作成、取得、更新
<a name="bk_example"> </a>

次のコード例では、カスタム レポートを作成、取得、および更新します。このコードはエディターの分離コードクラスからのもので、ASPX ページで定義されているコントロールのプログラミング ロジックとして使用できます。
  
    
    
このコードの例をコンパイルできるようにするには、「 [カスタム PerformancePoint サービス レポート用エディターの作成](#BKMK_ConfigREditor)」で説明しているとおりに開発環境を設定する必要があります。
  
    
    



```cs

using System;
using System.Web.UI;
using System.Web.UI.WebControls;
using Microsoft.PerformancePoint.Scorecards;
using Microsoft.PerformancePoint.Scorecards.ServerCommon;

namespace Microsoft.PerformancePoint.SDK.Samples.SampleReport
{

    // Represents the class that defines the sample report editor. 
    public class SampleReportViewEditor : Page
    {

        // Declare private variables for the ASP.NET controls defined in the user interface.
        // The sample's user interface is an ASPX page that defines the controls in HTML.
        private TextBox textboxName;
        private TextBox textboxDescription;
        private Label labelErrorMessage;
        private Button buttonOK;

        // Make the controls available to this class.
        protected override void CreateChildControls()
        {
            base.CreateChildControls();

            if (null == textboxName)
                textboxName = FindControl("textboxName") as TextBox;
            if (null == textboxDescription)
                textboxDescription = FindControl("textboxDescription") as TextBox;
            if (null == labelErrorMessage)
                labelErrorMessage = FindControl("labelErrorMessage") as Label;
            if (null == buttonOK)
                buttonOK = FindControl("buttonOK") as Button;
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
                ReportViewRepositoryHelper reportviewRepositoryHelper = null;
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
                    reportviewRepositoryHelper =
                        new ReportViewRepositoryHelper();

                    // Set the report location by using the location from the query string.
                    RepositoryLocation repositoryReportViewLocation = RepositoryLocation.CreateFromUriString(itemLocation);

                    ReportView reportview;

                    // Retrieve or create the report object, depending on the operation
                    // passed in the query string (OpenItem or CreateItem).
                    if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Retrieve the report object by using the repository-helper object.
                        reportview = reportviewRepositoryHelper.Get(repositoryReportViewLocation);
                        if (reportview == null)
                        {
                            displayError("Could not retrieve the report view for editing.");
                            return;
                        }
                    }
                    else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Create a report view.
                        // CreateItem requests can be sent from a SharePoint list, but
                        // you must create a custom menu item to send the request.
                        // Dashboard Designer can send edit requests only.
                        reportview = new ReportView
                        {
                            RendererClassName = typeof(SampleReportRenderer).AssemblyQualifiedName, 
                            SubTypeId = "SampleReportView"
                        };
                    }
                    else
                    {
                        displayError("Invalid Action.");
                        return;
                    }

                    VerifyReportView(reportview);

                    // Save the original report and helper objects across page postbacks.
                    ViewState["action"] = action;
                    ViewState["reportview"] = reportview;
                    ViewState["reportviewrepositoryhelper"] = reportviewRepositoryHelper;
                    ViewState["itemlocation"] = itemLocation;

                    // Populate the child controls.
                    textboxName.Text = reportview.Name.ToString();
                    textboxDescription.Text = reportview.Description.ToString();
                }
                catch (Exception ex)
                {
                    displayError("An error has occurred. Please contact your administrator for more information.");
                    if (reportviewRepositoryHelper != null)
                    {

                        // Add the exception detail to the server event log.
                        reportviewRepositoryHelper.HandleException(ex);
                    }
                }
            }
        }

        // Handles the Click event of the buttonOK control.
        protected void buttonOK_Click(object sender, EventArgs e)
        {
            EnsureChildControls();

            // Verify that the textboxName control contains a value.
            if (string.IsNullOrEmpty(textboxName.Text))
            {
                labelErrorMessage.Text = "A report view name is required.";
                return;
            }

            // Clear any pre-existing error message.
            labelErrorMessage.Text = string.Empty;

            // Retrieve the report and helper objects from view state.
            string action = (string)ViewState["action"];
            string itemLocation = (string) ViewState["itemlocation"];
            ReportView reportview = (ReportView)ViewState["reportview"];
            ReportViewRepositoryHelper reportviewRepositoryHelper = (ReportViewRepositoryHelper)ViewState["reportviewrepositoryhelper"];

            // Update the report object with form changes.
            reportview.Name.Text = textboxName.Text;
            reportview.Description.Text = textboxDescription.Text;

            // Save the report object to the PerformancePoint Services repository.
            try
            {
                reportview.Validate();
                if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                {
                    reportview.CreatedDate = DateTime.Now;
                    ReportView newReportView = reportviewRepositoryHelper.Create(
                        string.IsNullOrEmpty(reportview.Location.ItemUrl) ? itemLocation : reportview.Location.ItemUrl, 
                        reportview);
                    ViewState["reportview"] = newReportView;
                    ViewState["action"] = ClickOnceLaunchValues.OpenItem;
                }
                else
                {
                    reportviewRepositoryHelper.Update(reportview);
                }
            }
            catch (Exception ex)
            {
                displayError("An error has occurred. Please contact your administrator for more information.");
                if (reportviewRepositoryHelper != null)
                {
                    // Add the exception detail to the server event log.
                    reportviewRepositoryHelper.HandleException(ex);
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

        // Verifies that the properties for the report object are set.
        static void VerifyReportView(ReportView reportview)
        {

            // Verify that all required properties are set. 
            if (string.IsNullOrEmpty(reportview.SubTypeId))
            {

                // This value must match the subType attribute specified
                // in the web.config file.
                reportview.SubTypeId = "SampleReportView";
            }

            if (string.IsNullOrEmpty(reportview.RendererClassName))
            {
                reportview.RendererClassName = typeof (SampleReportRenderer).AssemblyQualifiedName;
            }

            // Reports are consumers and do not use provider endpoints.
           reportview.BeginPoints.Clear();

            // If there are no consumer endpoints, create one so we can connect a filter or a scorecard to the report.
            if (0 == reportview.EndPoints.Count)
            {
                EndPoint endpoint = new EndPoint
                {
                    Category = EndPointCategory.None,
                    UniqueName = "SampleReportView_EndPoint",

                    // The display name is shown to users in Dashboard
                    // Designer to represent the endpoint that can be 
                    // connected to a filter or scorecard.
                    DisplayName = "Sample Report View EndPoint"
                };

                reportview.EndPoints.Add(endpoint);
            }
            
            // Set optional properties for reports.
            reportview.CustomData = "You can use this property to store custom information for this filter as a string or a serialized object.";
        }
    }
}
```


## 次の手順
<a name="bk_next"> </a>

レポート エディター (必要な場合は、そのユーザー インターフェイスも含む) とレポート レンダラーを作成した後、「 [[方法] PerformancePoint Services の拡張機能を手動で登録する](http://msdn.microsoft.com/library/3aa6d340-4b05-46b3-9648-2b6e18e04e09%28Office.15%29.aspx)」に説明されているとおりに拡張機能を展開します。
  
    
    

## その他の技術情報
<a name="bk_addResources"> </a>


-  [[方法] SharePoint 2013 の PerformancePoint Services 用のレポート レンダラーを作成する](how-to-create-report-renderers-for-performancepoint-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の PerformancePoint Services](performancepoint-services-in-sharepoint-2013.md)
    
  

