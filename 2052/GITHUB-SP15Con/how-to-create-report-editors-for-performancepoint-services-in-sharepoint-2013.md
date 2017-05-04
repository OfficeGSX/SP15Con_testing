---
title: 如何：为 SharePoint 2013 中的 PerformancePoint Services 创建报表编辑器
ms.prod: SHAREPOINT
ms.assetid: b42b4452-90f8-464c-828f-d3abac40670c
---


# 如何：为 SharePoint 2013 中的 PerformancePoint Services 创建报表编辑器
了解如何为 PerformancePoint Services 创建自定义报表扩展的编辑器组件。
## 什么是 PerformancePoint Services 的自定义报表编辑器？
<a name="bi_intro"> </a>

在 PerformancePoint Services 中，自定义报表编辑器可让用户在自定义报表上设置属性。报表编辑器还会初始化报表终结点，报表终结点将从记分卡和筛选器提供程序接收参数值。有关编辑器要求和功能的详细信息，请参阅  [用于自定义 PerformancePoint Services 对象的编辑器](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)。
  
    
    
以下过程和示例基于  [自定义对象示例](http://msdn.microsoft.com/library/af021d52-7562-4e7a-9de4-e1fc5784a59d%28Office.15%29.aspx)的 **SampleReportViewEditor** 类。该编辑器是一个瘦 Web 应用程序，使用户能够修改报表的名称和说明。有关该类的完整代码，请参阅 [代码示例：在 SharePoint Server 2013 中创建、检索和更新自定义 PerformancePoint Services](#bk_example)。
  
    
    
我们建议您将示例编辑器用作模板。该示例演示了如何调用 PerformancePoint Services API 中的对象，提供了简化库操作（例如创建和更新对象）调用的帮助程序对象，并演示了 PerformancePoint Services 开发的最佳实践。
  
    
    

## 为自定义 PerformancePoint Services 报表创建编辑器
<a name="BKMK_ConfigREditor"> </a>


  
    
    

1. 安装 PerformancePoint Services ，或者将您的扩展使用的 DLL（步骤 3 中列出）复制到您的计算机上。有关详细信息，请参阅  [开发方案中使用的 PerformancePoint Services DLL](http://msdn.microsoft.com/library/41e92619-8253-481d-82f9-35b6a6abc477%28Office.15%29.aspx)。
    
  
2. 在 Visual Studio 中创建 C# 类库，如果您已为您的扩展创建类库，请添加一个新的 C# 类。
    
    您必须用强名称对您的 DLL 进行签名。此外，请确保您的 DLL 所引用的所有程序集都具有强名称。有关如何用强名称对程序集进行签名和如何创建公钥/私钥对的信息，请参阅 [How to: Create a public/private key pair](http://msdn.microsoft.com/library/05026813-f3bd-4d7c-9e0b-fc588eb3d114.aspx)。
    
  
3. 将以下 DLL 作为程序集引用添加到项目中：
    
  - Microsoft.PerformancePoint.Scorecards.Client.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerCommon.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.Store.dll（由帮助程序类所用）
    
  
  - Microsoft.SharePoint.dll（由帮助程序类所用）
    
  

    示例编辑器还包含对 System.Web.dll 和 System.Web.Services.dll 的程序集引用。可能需要其他项目引用，具体取决于您的扩展的功能。
    
  
4. 将示例中的以下类添加到项目中。编辑器使用这些帮助程序类来与 PerformancePoint Services 存储库进行交互：
    
  - DataSourceConsumerHelper.cs
    
  
  - ExtensionRepositoryHelper.cs
    
  
  - ReportViewRepositoryHelper.cs
    
  
  - IDataSourceConsumer.cs
    
  

    > **注释**
      > 示例报表从筛选器获取数据，所以它没有使用 **DataSourceConsumerHelper** 或 **IDataSourceConsumer** 对象。不过，如果报表从 PerformancePoint Services 数据源获取数据，则可以使用 **DataSourceConsumerHelper** 类公开的方法来检索数据源，如 [如何：为 SharePoint 2013 中的 PerformancePoint Services 创建筛选器编辑器](how-to-create-filter-editors-for-performancepoint-services-in-sharepoint-2013.md)中所述。 
5. 在编辑器类中，为以下 PerformancePoint Services 命名空间添加 **using** 指令：
    
  - **Microsoft.PerformancePoint.Scorecards**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerCommon**
    
  

    根据您的扩展的功能，可能需要其他 **using** 指令。
    
  
6. 从支持您的编辑器实现的基类继承。因为示例报表编辑器是 Web 应用程序，所以它从  [Page](https://msdn.microsoft.com/library/System.Web.UI.Page.aspx) 类继承。其他实现可从基类（例如 [UserControl](https://msdn.microsoft.com/library/System.Windows.Forms.UserControl.aspx) 或 [WebPart](https://msdn.microsoft.com/library/System.Web.UI.WebControls.WebParts.WebPart.aspx) 类）派生。
    
  
7. 声明公开您希望用户查看或修改的属性的控件的变量。示例报表编辑器首先声明用户界面组件（这是一个 ASPX 页）中定义的 Web 服务器控件的变量。示例编辑器还定义使用户能够提交更改的按钮控件。然后，编辑器调用  [CreateChildControls()](https://msdn.microsoft.com/library/System.Web.UI.Control.CreateChildControls.aspx) 方法使控件可在该页上使用。
    
    > **注释**
      > 编辑器独立于用户界面定义编程逻辑。有关创建编辑器的用户界面组件的说明不在本文档的讨论范围内。 

    示例报表编辑器在 **Page_Load** 方法中执行步骤 8 至 12。 **Page_Load** 还用于初始化和验证变量和控件、填充控件以及保存自定义报表和帮助程序对象的状态信息。
    
  
8. 将  [AllowUnsafeUpdates](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ServerCommon.ServerUtils.AllowUnsafeUpdates.aspx) 属性设置为 **true**。这使报表编辑器能够将数据写入存储库中，而无需使用表单 **POST** 操作。
    
  
9. 从查询字符串中检索参数并将它们设置为本地变量的值，如以下代码示例中所示。
    
  ```cs
  
// The URL of the site collection that contains the PerformancePoint Services repository.
string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];

// The location of the report in the repository.
string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];

// The operation to perform: OpenItem or CreateItem.
string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];
  ```


有关查询字符串参数的信息，请参阅 [用于自定义 PerformancePoint Services 对象的编辑器](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)。
    
  
10. 检索 **ReportViewRepositoryHelper** 对象，它用于调用存储库，如以下代码示例中所示。
    
  ```cs
  
reportviewRepositoryHelper = new ReportViewRepositoryHelper();
  ```

11. 基于查询字符串参数设置报表位置，如以下代码示例中所示。
    
  ```cs
  RepositoryLocation repositoryReportViewLocation = RepositoryLocation.CreateFromUriString(itemLocation);
  ```

12. 从查询字符串检索要执行的操作（ _OpenItem_ 或 _CreateItem_），然后检索或创建自定义报表。
    
  - 若要检索自定义报表，请使用 **ReportViewRepositoryHelper.Get** 方法。
    
  
  - 若要创建自定义报表，请使用 **ReportView()** 构造函数，然后定义报表的 [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.RendererClassName.aspx) 和 [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.SubTypeId.aspx) 属性。
    
     [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.SubTypeId.aspx) 是报表的唯一标识符，它必须与您在 PerformancePoint Services web.config 文件中为自定义报表指定的 **subType** 属性相匹配。 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.RendererClassName.aspx) 是定义呈现器 Web 服务器控件的类的完全限定名称。如果没有在编辑器中定义，则此值默认为 web.config 文件中指定的呈现器类。
    
  

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


> **注释**
> 默认情况下，用户只能从 PerformancePoint 仪表板设计器 创建自定义对象。若要使用户能够在仪表板设计器之外创建自定义对象，您必须添加一个菜单项以将  _CreateItem_ 请求从库中的内容类型发送到编辑器。有关详细信息，请参阅 [用于自定义 PerformancePoint Services 对象的编辑器](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)。 
13. 定义报表的终结点，这使报表能够从筛选器和记分卡检索数据。示例报表编辑器定义了所需的端点属性，如以下代码示例中所示。
    
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


    示例编辑器在 **VerifyReportView** 方法中定义端点。它还使用 **VerifyReportView** 来验证是否设置了所需属性并定义可选的 [CustomData](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.CustomData.aspx) 属性，后者可用来存储报表信息。
    
  
14. 使用用户定义的更改来更新报表。示例报表编辑器中的 **buttonOK_Click** 方法调用 **ReportViewRepositoryHelper.Update** 方法来更新库中报表的 [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 和 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 属性。 **buttonOK_Click** 还用于验证控件内容并检索自定义报表和帮助程序对象的状态信息。
    
    > **注释**
      > 用户可以编辑自定义对象的  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 和 [Owner](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Owner.aspx) （"负责人"）属性，并直接从仪表板设计器和 PerformancePoint Services 存储库中删除自定义对象。

## 代码示例：在 SharePoint Server 2013 中创建、检索和更新自定义 PerformancePoint Services
<a name="bk_example"> </a>

以下代码示例创建、检索和更新自定义报表。此代码来自为 ASPX 页中定义的控件提供编程逻辑的代码隐藏类。
  
    
    
您必须首先配置如 [为自定义 PerformancePoint Services 报表创建编辑器](#BKMK_ConfigREditor)中所描述的开发环境，然后才可编写此代码示例。
  
    
    



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


## 后续步骤
<a name="bk_next"> </a>

创建报表编辑器（如果需要，包括其用户界面）和报表呈现器后，部署扩展，如 [如何：手动注册 PerformancePoint Services 扩展](http://msdn.microsoft.com/library/3aa6d340-4b05-46b3-9648-2b6e18e04e09%28Office.15%29.aspx)中所述。
  
    
    

## 其他资源
<a name="bk_addResources"> </a>


-  [如何：为 SharePoint 2013 中的 PerformancePoint Services 创建报告呈现器](how-to-create-report-renderers-for-performancepoint-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的 PerformancePoint Services](performancepoint-services-in-sharepoint-2013.md)
    
  

