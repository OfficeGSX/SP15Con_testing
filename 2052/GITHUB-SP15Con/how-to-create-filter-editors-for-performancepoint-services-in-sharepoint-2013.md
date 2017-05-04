---
title: 如何：为 SharePoint 2013 中的 PerformancePoint Services 创建筛选器编辑器
ms.prod: SHAREPOINT
ms.assetid: 51354c88-4330-40f0-bffa-1ce73f9ff298
---


# 如何：为 SharePoint 2013 中的 PerformancePoint Services 创建筛选器编辑器
了解如何创建 PerformancePoint Services 的自定义筛选器扩展的编辑器组件。
## 什么是 PerformancePoint Services 的自定义筛选器编辑器？
<a name="bk_intro"> </a>

在 PerformancePoint Services 中，自定义筛选器编辑器可让用户设置自定义筛选器的属性。筛选器编辑器还必须初始化定义包含记分卡参数值的筛选器起点的筛选器的  [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) 属性并向使用方报告。有关编辑器要求和功能的详细信息，请参阅 [用于自定义 PerformancePoint Services 对象的编辑器](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)。
  
    
    
以下过程和示例基于 **SampleFilterEditor** 类（ [自定义对象示例](http://msdn.microsoft.com/library/af021d52-7562-4e7a-9de4-e1fc5784a59d%28Office.15%29.aspx) 中）。 该编辑器是精简 Web 应用程序，此应用程序可让用户修改筛选器的名称和描述，并且可让用户选择基础数据源。对于该类的完整代码，请参阅 [代码示例：创建、检索和更新自定义 PerformancePoint Services 筛选器（SharePoint Server 2013 中）](#bk_example)。
  
    
    
我们建议您将示例编辑器用作模板。该示例演示了如何调用 PerformancePoint Services API 中的对象，提供了简化库操作（例如创建和更新对象）调用的帮助程序对象，并演示了 PerformancePoint Services 开发的最佳实践。
  
    
    

## 创建和配置 PerformancePoint Services 筛选器的编辑器类
<a name="bk_intro"> </a>


  
    
    

1. 安装 PerformancePoint Services，或者将您的扩展使用的 DLL（请参阅步骤 3）复制到您的计算机上。有关详细信息，请参阅  [开发方案中使用的 PerformancePoint Services DLL](http://msdn.microsoft.com/library/41e92619-8253-481d-82f9-35b6a6abc477%28Office.15%29.aspx)。
    
  
2. 在 Visual Studio 中，创建一个 C# 类库。如果您已为扩展创建了一个类库，请添加新 C# 类。
    
    您必须用强名称对您的 DLL 进行签名。此外，请确保您的 DLL 所引用的所有程序集都具有强名称。有关如何使用强名称对程序集进行签名以及如何创建公钥/私钥对的信息，请参阅  [How to: Create a public/private key pair](http://msdn.microsoft.com/library/05026813-f3bd-4d7c-9e0b-fc588eb3d114.aspx)。
    
  
3. 将以下 DLL 作为程序集引用添加到项目：
    
  - Microsoft.PerformancePoint.Scorecards.Client.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerCommon.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerRendering.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.Store.dll（由帮助程序类使用）
    
  
  - Microsoft.SharePoint.dll（由帮助程序类使用）
    
  

    示例编辑器还包含对 System.Web.dll 和 System.Web.Services.dll 的程序集引用。根据您的扩展的功能，可能需要其他项目引用。
    
  
4. 将示例中的以下类添加到项目中。编辑器使用这些帮助程序类来与 PerformancePoint Services 存储库进行交互：
    
  - DataSourceConsumerHelper.cs
    
  
  - ExtensionRepositoryHelper.cs
    
  
  - FilterRepositoryHelper.cs
    
  
  - IDataSourceConsumer.cs
    
  
5. 在编辑器类中，为以下 PerformancePoint Services 命名空间添加 **using** 指令：
    
  - **Microsoft.PerformancePoint.Scorecards**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerCommon**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerRendering**
    
  

    根据您的扩展的功能，可能需要其他 **using** 指令。
    
  
6. 从支持您的编辑器实现的基类继承。由于示例筛选器编辑器是一个 Web 应用程序，因此它从  [Page](https://msdn.microsoft.com/library/System.Web.UI.Page.aspx) 类继承。其他实现可从基类（如 [UserControl](https://msdn.microsoft.com/library/System.Windows.Forms.UserControl.aspx) 类或 [WebPart](https://msdn.microsoft.com/library/System.Web.UI.WebControls.WebParts.WebPart.aspx) 类）派生。
    
  
7. 定义公开您希望用户查看或修改的属性的控件。示例筛选器编辑器首先声明在用户界面组件（即 ASPX 页）中定义的 Web 服务器控件的变量。示例编辑器还定义使用户能够提交更改的按钮控件。然后，编辑器调用  [CreateChildControls()](https://msdn.microsoft.com/library/System.Web.UI.Control.CreateChildControls.aspx) 方法使控件可在该页上使用。
    
    > **注释**
      > 编辑器独立于用户界面定义编程逻辑。有关创建编辑器的用户界面组件的说明不在本文档的讨论范围内。 

    示例筛选器编辑器在 **Page_Load** 方法中执行步骤 8 到 12。 **Page_Load** 还可用于初始化并验证变量和控件、填充控件并保存自定义筛选器和帮助程序对象的状态信息。
    
  
8. 将  [AllowUnsafeUpdates](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ServerCommon.ServerUtils.AllowUnsafeUpdates.aspx) 属性设置为 **true**。这样一来，筛选器编辑器无需使用表单 POST 操作即可将数据写入存储库。
    
  
9. 从查询字符串中检索参数并将它们设置为本地变量的值，如以下代码示例中所示。
    
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


有关查询字符串参数的信息，请参阅  [用于自定义 PerformancePoint Services 对象的编辑器](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)。
    
  
10. 检索 **FilterRepositoryHelper** 对象，它用于调用库，如以下代码示例中所示。
    
  ```cs
  
filterRepositoryHelper = new FilterRepositoryHelper();
  ```


  ```VB.net
  filterRepositoryHelper = New FilterRepositoryHelper()
  ```

11. 基于查询字符串参数设置筛选器位置，如以下代码示例所示。
    
  ```cs
  RepositoryLocation repositoryFilterLocation = RepositoryLocation.CreateFromUriString(itemLocation);
  ```


  ```VB.net
  Dim repositoryFilterLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(itemLocation)
  ```

12. 从查询字符串检索要执行的操作（ _OpenItem_ 或 _CreateItem_） ，然后检索或创建自定义筛选器。
    
  - 若要检索自定义筛选器，请使用 **FilterRepositoryHelper.Get** 方法。
    
  
  - 若要创建自定义筛选器，请使用 **Filter()** 构造函数，并确定该筛选器的 [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) , [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.RendererClassName.aspx) 和 [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.SubTypeId.aspx) 属性。 [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.SubTypeId.aspx) 是此筛选器的唯一标识符，必须匹配为您的自定义筛选器指定的 **subType** 属性（PerformancePoint Services web.config 文件中）。 [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.RendererClassName.aspx) 是确定呈现器 Web 服务器控件的类的完全限定名称。如果在编辑器上未对其进行确定，则此值默认为 web.config 文件中指定的呈现器类。
    
  

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


> **注释**
> 默认情况下，用户只能从 PerformancePoint 仪表板设计器 创建自定义对象。若要使用户能够在 仪表板设计器 之外创建自定义对象，您必须添加一个菜单项以将  _CreateItem_ 请求从库中的内容类型发送到编辑器。有关详细信息，请参阅 [用于自定义 PerformancePoint Services 对象的编辑器](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx)。 
13. 从存储库检索筛选器的基础数据源。该示例筛选器编辑器使用 **FilterRepositoryHelper.DataSourceHelper** 属性调用 **DataSourceConsumerHelper.GetDataSource** 方法，该方法用于根据数据源在存储库中的位置来检索数据源。如以下代码示例所示。
    
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

14. 若要使用户能够选择筛选器的数据源，请使用 PerformancePoint Services 数据源填充选择控件。本示例筛选器编辑器中的 **PopulateDataSourceDropDown** 方法调用 **DataSourceConsumerHelper.GetDataSourcesBySourceNames** 方法以检索数据源。如以下代码示例所示。
    
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


    本示例筛选器编辑器仅检索两类数据源，但您可修改此方法以支持其他数据源类型或提示用户要检索的数据源的类型。若要引用特定类型的本机数据源，请使用  [SourceName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSource.SourceName.aspx) 属性，该属性会从 [DataSourceNames](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSourceNames.aspx) 类返回一个字段。若要引用一个自定义数据源，请使用该数据源的 [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSource.SubTypeId.aspx) 属性，该属性的值与在 PerformancePoint Services web.config 文件中为数据源扩展注册的 **subType** 属性的值相同。
    
    如果修改此方法，则必须在示例筛选器的数据提供程序中的  [GetDisplayDataInternal](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetDisplayDataInternal.aspx) 方法中进行相应的更改。
    
  
15. 定义由  [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) 属性表示的筛选器起点。这将定义筛选值的源，并且筛选器必须使用它才能将数据发送到记分卡和报告。
    
1. 创建一个  [ParameterDefinition](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.aspx) 对象。 [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) 返回一个仅包含一个 [ParameterDefinition](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.aspx) 对象的 [ParameterDefinitionCollection](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinitionCollection.aspx) 对象。
    
  
2. 若要指定筛选器的数据提供程序，请将  [ParameterProviderId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.ParameterProviderId.aspx) 属性设置为该数据提供程序的唯一标识符。此值必须与该数据提供程序的 [GetId()](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetId.aspx) 方法所返回的值匹配。
    
  
3. 若要指定筛选值的关键标识符源，请将  [KeyColumn](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.KeyColumn.aspx) 属性设置为显示模拟运算表中包含关键标识符的列。示例筛选器编辑器将此属性定义为"Symbol"列。
    
  
4. 若要指定筛选器控件的显示值源，请将  [DisplayColumn](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.DisplayColumn.aspx) 属性设置为显示模拟运算表中包含显示值的列。示例筛选器编辑器将此属性定义为"Symbol"列。
    
  

    > **注释**
      > 显示模拟运算表由  [DisplayValues](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.DisplayValues.aspx) 属性返回，并在筛选器数据提供程序调用 [GetDisplayDataInternal](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetDisplayDataInternal.aspx) 方法时初始化。如果该模拟运算表包含其他列，则可定义其他列映射以提供附加功能。

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


    示例编辑器在 **VerifyFilter** 方法中定义其起点，它还使用 **VerifyFilter** 来验证是否已设置所需属性，并定义选择模型（它是一个可选属性）。
    
  
16. 通过运行筛选器的查询并检索数据源中的数据来初始化筛选器。示例筛选器编辑器中的 **buttonOK_Click** 方法调用 **FilterRepositoryHelper.GetParameterDisplayData** 方法来初始化筛选器。
    
    > **注释**
      > 在更新筛选器对象之前，编辑器必须至少调用 **FilterRepositoryHelper.GetParameterDisplayData** 一次。
17. 使用用户定义的更改来更新筛选器。示例筛选器编辑器中的 **buttonOK_Click** 方法会调用 **FilterRepositoryHelper.Update** 方法以在存储库中更新筛选器的 [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 和 [DataSourceLocation](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.DataSourceLocation.aspx) 属性。此外，可使用 **buttonOK_Click** 验证控件的内容，并检索自定义筛选器和帮助程序对象的状态信息。
    
    > **注释**
      > 用户可设置自定义对象的  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) 、 [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) 和 [Owner](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Owner.aspx) （"负责人"）属性，并直接从 仪表板设计器 和 PerformancePoint Services 存储库中删除自定义对象。

## 代码示例：创建、检索和更新自定义 PerformancePoint Services 筛选器（SharePoint Server 2013 中）
<a name="bk_example"> </a>

以下代码创建、检索和更新自定义筛选器。此代码来自为 ASPX 页中定义的控件提供编程逻辑的代码隐藏类。
  
    
    
在编译此代码示例之前，您必须配置开发环境，如  [在 PerformancePoint Services 中创建和配置筛选器编辑器的编辑器类](#BKMK_ConfigFEd)。
  
    
    



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


## 后续步骤
<a name="bk_example"> </a>

在创建筛选器编辑器（如果需要，包括其用户界面）和数据提供程序后，部署如  [如何：手动注册 PerformancePoint Services 扩展](http://msdn.microsoft.com/library/3aa6d340-4b05-46b3-9648-2b6e18e04e09%28Office.15%29.aspx) 中所述的扩展。
  
    
    

## 其他资源
<a name="bk_addResources"> </a>


-  [如何：为 SharePoint 2013 中的 PerformancePoint Services 创建筛选器数据提供程序](how-to-create-filter-data-providers-for-performancepoint-services-in-sharepoint.md)
    
  
-  [SharePoint 2013 中的 PerformancePoint Services](performancepoint-services-in-sharepoint-2013.md)
    
  

