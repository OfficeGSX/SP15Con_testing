---
title: 如何：在 Duet Enterprise 2.0 中使用 SAP 工作流
ms.prod: SHAREPOINT
ms.assetid: 816e28ed-8cea-4e33-98e5-d3d27136e2e6
---


# 如何：在 Duet Enterprise 2.0 中使用 SAP 工作流

## 简介
<a name="bkmk_Introduction"> </a>

SAP 业务工作流提供了自动化业务流程的方式，并且通过 Duet Enterprise 2.0，这些工作流可以集成在 SharePoint 外接程序中。
  
    
    
以下步骤显示了如何创建和配置应用程序，以及之后如何显示从 SAP 工作流检索的信息。
  
    
    

## 为 SharePoint 和 Duet Enterprise 2.0 创建应用程序
<a name="bkmk_CreateApp"> </a>

第一步是创建 SharePoint 外接程序，其中将包含针对 Business Connectivity Services (BCS) 使用外部内容类型的联系信息、外部列表和您可能想要用于显示数据的任何自定义项。
  
    
    

### 创建 SharePoint 相关应用程序：


1. 以管理员身份启动 Visual Studio 2008。
    
  
2. 依次选择"文件"、"新建"、"新建项目"。
    
  
3. 在"新建项目"对话框中，依次展开"Visual C#"节点和"Office/SharePoint"节点，然后选择"应用程序"节点。
    
  
4. 选择"SharePoint 2013 相关应用程序"。
    
  
5. 为项目命名并单击"确定"。
    
  
6. 在第一个"指定 SharePoint 相关应用程序设置"对话框中，为应用程序命名并选择"您希望如何托管 SharePoint 相关应用程序"下的 "SharePoint 承载"。此外，在"您希望使用哪个 SharePoint 站点对您的应用程序进行调试"下，指定想要用于调试的"Duet 工作流网站"的 URL，然后选择"完成"。
    
  
7. 在"生成"菜单中，选择"部署" *<应用程序名称>*  。
    
  
部署应用程序后，Visual Studio 会启动应用程序的默认页面。
  
    
    

## 向应用程序添加外部内容类型和外部列表
<a name="bkmk_AddECT"> </a>

必须让 BCS 知道外部数据源。为此，可使用外部内容类型。
  
    
    

### 添加外部内容类型：


1. 在"解决方案资源管理器"中，打开项目的快捷菜单，并选择"添加"、"外部数据源的内容类型"。
    
  
2. 在"指定 OData 源"页中，输入 Duet Enterprise 工作流服务的 URL。下面是 Duet 服务 URL 的一个示例：
    
     *https://<<DUETGATEWAY>>:<<PORT>>/sap/opu/odata/IWWRK/DUET_WORKFLOW_CORE;mo;c=SHAREPOINT_DE* 
    
  
3. 为 ODATA 源选择一个名称（例如，SAPWorkflows），然后选择"下一步"。
    
  
4. 指定 SAP 用户名和密码以连接到服务元数据。
    
  
5. 随即将显示 OData 服务所暴露数据实体的列表。 选择要包括在外部内容类型中的实体。在本示例中，使用 SAP 工作流服务，从以下实体中选择：
    
  - **AttachmentCollection**
    
  
  - **CommentCollection**
    
  
  - **DecisionOptionCollection**
    
  
  - **ExtensibleElementCollection**
    
  
  - **ParticipantCollection**
    
  
  - **ServiceOperations**
    
  
  - **TaskDescriptionCollection**
    
  
  - **WorkflowTaskCollection**
    
  
6. 选中"创建所选数据实体 (服务操作除外) 的列表实例"复选框。
    
  
7. 选择"完成"。
    
  

> **注释**
> 确保 SAP 工作流服务允许与 Visual Studio 中的 BDC 自动生成工具一样的基本身份验证。 
  
    
    


## 向 Duet Enterprise 2.0 工作流列表添加自定义操作功能
<a name="bkmk_AddCustomAction"> </a>

为了向用户提供一种方法以供用户使用添加到 SharePoint 列表的新功能，以下步骤将向"网站操作"菜单添加项。
  
    
    

### 若要添加自定义操作，请执行以下操作：


1. 右键单击 SharePoint 外接程序项目，并添加新的"UI 自定义操作"项。
    
  
2. 打开自定义操作主机 Web 功能的 **Elements.xml** 文件。通过将文件中的代码替换为以下 XML，自定义操作将
    
  - 声明 ECB 自定义操作及其属性。
    
  
  - 声明功能区自定义操作及其属性。
    
  
  - 声明应用程序网页目标以及通过查询字符串传递到该网页的值。 
    
    **UrlAction** 元素使用了若干标记。有关详细信息，请参阅 [SharePoint 外接程序中的 URL 字符串和标记](http://msdn.microsoft.com/library/800ec8cd-a448-46bc-b41e-d4030eeb4048%28Office.15%29.aspx)。
    
  

  ```XML
  
<?xml version="1.0" encoding="utf-8"?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
  <CustomAction Id="ViewDetailsECBCustomAction"
          RegistrationType="List"
          RegistrationId="107"
          Location="EditControlBlock"
          Sequence="1"
          ImageUrl="_layouts/15/images/placeholder16x16.png"
          Title="View Details">
     <UrlAction Url="~appWebUrl/Pages/ViewDetails.aspx?
            {StandardTokens}&amp;amp;ListId={ListId}&amp;amp;ItemId={ItemId}"/>
  </CustomAction>

  <CustomAction
          Id="ViewDetailsRibbonAction"
          RegistrationId="107"
          RegistrationType="List"
          Location="CommandUI.Ribbon"
          Title="View Details"
          HostWebDialog="false"
          HostWebDialogHeight="500"
          HostWebDialogWidth="500">
    <CommandUIExtension>
      <CommandUIDefinitions>
        <CommandUIDefinition 
              Location="Ribbon.ListItem.Workflow.Controls._children">
          <Button
              Id="Ribbon.ListItem.Workflow.ViewDetails"
              Alt="View Details"
              Sequence="25"
              Command="Invoke_ViewDetailsSAPWorkflow"
              LabelText="View Details"
              TemplateAlias="o1"
              Image32by32="_layouts/15/images/placeholder32x32.png"
              Image16by16="_layouts/15/images/placeholder16x16.png"/>
        </CommandUIDefinition>
      </CommandUIDefinitions>
      <CommandUIHandlers>
        <CommandUIHandler
          Command="Invoke_ViewDetailsSAPWorkflow"
          CommandAction="~appWebUrl/Pages/ViewDetails.aspx?
           {StandardTokens}&amp;amp;ListId={ListId}&amp;amp;ItemId={SelectedItemId}"/>
      </CommandUIHandlers>
    </CommandUIExtension>
  </CustomAction>
</Elements>
  ```

3. 将新页面 ViewDetails.aspx 添加到项目。
    
  
4. 按"F5"以构建和部署 SharePoint 应用程序。
    
  
5. 转到主机 Web 中的 **WorkItem** 列表，并在上下文菜单中选择"查看详细信息"。系统会将您重定向至 **ViewDetails.aspx**。
    
  

> **注释**
> 确保 SAP 工作流服务允许基本身份验证。Visual Studio 中的 BDC 自动生成工具目前仅支持匿名身份验证和基本身份验证。 
  
    
    

如果您部署应用程序，然后导航至应用程序中的 **Lists/WorkflowTaskCollection**，将会看到以下消息：
  
    
    
"外部系统消息：'LobSystem(外部系统)返回身份验证错误。'"。
  
    
    
若要修复此错误，您需要向应用程序添加单一登录，以向 BCS 和 SAP 后端提供身份验证凭据。
  
    
    

## 向应用程序添加安全单一登录
<a name="bkmk_AddSSO"> </a>

Duet Enterprise 单一登录允许对用户进行身份验证，以便用户通过一次登录访问 SharePoint 和 SAP 端的资源。
  
    
    

### 添加单一登录：


1. 通过在 SharePoint 命令行管理程序中执行以下命令创建 OData 连接。
    
  ```XML
  
New-SPODataConnectionSetting -Name SAPWorkflow
-ServiceContext "http://localhost" -ExtensionProvider 
"OBA.Server.Canary.ObaOdataServerExtensionProvider, OBA.Server.SSOProvider, 
Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c" 
-AuthenticationMode passthrough -ServiceAddressURL 
"https://<<DUETGATEWAY>>:<<PORT>>/sap/opu/odata/IWWRK/
DUET_WORKFLOW_CORE;mo;c=SHAREPOINT_DE"

  ```

2. 双击 **WorkflowTaskCollection.ect**。
    
  
3. 在"属性"窗口中，将  `ODataConnectionSettingId` 的值更新为 *SAPWorkflow*  。
    
  
4. 允许应用程序使用连接。
    
  
5. 打开 **AppManifest.xml**。
    
  
6. 在"权限请求"中，依次选择"范围"、"BCS"和"权限" = 读取。
    
  
7. 按"F5"部署应用程序。
    
  
8. 在应用程序的 **权限**页上，选择此应用程序将使用的 OData 连接。
    
  
9. 选择"信任它"按钮。
    
  
10. 导航到 **../Lists/WorkflowTaskCollection**。您会看到分配给您的直接来自于 SAP 系统的 SAP 任务。
    
  

## 使用 JSOM 访问工作流任务和相关项
<a name="bkmk_UseJSOM"> </a>

由于SharePoint 外接程序必须使用客户端代码与 SharePoint 通信，下面将演示如何使用 JavaScript 对象模型与 SAP 工作流任务交互。
  
    
    

### 若要创建代码，请执行以下操作：


1. 右键单击"解决方案资源管理器"中的"页面"文件夹，添加 .NET 页面并将其命名为 **ViewDetails.aspx**
    
  
2. 将以下 HTML 粘贴到  `PlaceHolderAdditionalPageHead` 部分中。此操作会设置样式表引用、加载脚本，然后返回 SAP 中的相关工作流任务。
    
  ```XML
  
<link
    rel="Stylesheet" 
    type="text/css"
    href="../Content/App.css"/>

<script 
    src="//ajax.aspnetcdn.com/ajax/4.0/1/MicrosoftAjax.js" 
    type="text/javascript">
</script>
<script 
    src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js" 
    type="text/javascript">
</script>      
<script 
    src="../Scripts/ViewDetails.js" 
    type="text/javascript">
</script>

  ```

3. 右键单击"脚本"文件夹，添加一个 JavaScript 文件。将文件命名为 **ViewDetails.js**。
    
  
4. 此标记将执行以下操作：
    
  - 检索父 Web 的  `ClientContext`。
    
  
  - 使用 Duet 工作流列表 GUID 和所选项 ID 检索所选工作流任务的  `TaskInstanceParentId`。
    
  
  - 分析  `TaskInstanceParentId` 以获取 SAP 中工作流的 SAP 源和 ID。
    
  
  - 加载外部内容类型的实体。
    
  
  - 通过以参数形式传递 SAP 源和 SAP 的工作流任务 ID，使用特定查找工具读取 SAP 的特定工作流任务。
    
  
  - 从 SAP 读取工作流任务的相关元素。
    
  
5. 下面是页面的完整 HTML 和 JavaScript。
    
  ```
  
// This code runs when the DOM is ready. It ensures the SharePoint
// script files are loaded and then executes execOperation().
$(document).ready(function () {
  SP.SOD.executeFunc("sp.js", "SP.ClientContext", execOperation);
});

function execOperation() {
  retrieveTaskFromList();
}

function retrieveTaskFromList() {
  // Retrieves the ClientContext of the parent web.
  var clientContext = 
  new SP.ClientContext.get_current();
  var appContextSite = 
  new SP.AppContextSite(clientContext, 
      decodeURIComponent(getQueryStringParameter("SPHostUrl")));
  var web = appContextSite.get_web();
  // Loads the selected workflow task from the duet workflow list.
  var listGuid = decodeURIComponent(getQueryStringParameter("ListId"));
  var itemId = decodeURIComponent(getQueryStringParameter("ItemId"))
  this.workflowItem = web.get_lists().getById(
      listGuid.substring(1, listGuid.length - 1)).getItemById(itemId);
  clientContext.load(workflowItem);

  clientContext.executeQueryAsync(
    Function.createDelegate(this, this.onLoadingTaskSucceeded),
    Function.createDelegate(this, this.onError)
  );
}

function onLoadingTaskSucceeded() {
  var sapParameters = parseParentTaskId(workflowItem.
      get_item("TaskInstanceParentId"));
  getDataFromSAP(sapParameters);
}

// Parses the TaskInstanceParentId 
// to get SAP Origin and the id for the workflow in SAP.
function parseParentTaskId(parentTaskId) {
  var idlength = parseInt(parentTaskId.substring(0, 2));
  var sapParameters = new Object();
  if (parentTaskId.length > (17 + idlength)) {
    sapParameters.workitemId = parentTaskId.substring(5, 5 + idlength);
    sapParameters.sapOrigin = parentTaskId.substring(17 + idlength);
  }
  return sapParameters;
}
    
// Retrieves the workflow task and associated elements from SAP.
function getDataFromSAP(sapParameters) {
  context = new SP.ClientContext.get_current();
  //Loads the entities for the external content types.
  wfEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "WorkflowTaskCollection");
  context.load(wfEntity);
  wfExtensibleElementEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "ExtensibleElementCollection");
  context.load(wfExtensibleElementEntity);
  wfCommentsEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "CommentCollection");
  context.load(wfCommentsEntity);
  wfDecisionsEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "DecisionOptionCollection");
  context.load(wfDecisionsEntity);
  wfParticipantEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "ParticipantCollection");
  context.load(wfParticipantEntity);
  wfDescriptionEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "TaskDescriptionCollection");
  context.load(wfDescriptionEntity);
  wfAttachmentEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "AttachmentCollection");
  context.load(wfAttachmentEntity);

  // Loads a LOB system instances.
  lobSystem = wfEntity.getLobSystem();
  lobSystemInstanceCollection = lobSystem.getLobSystemInstances();
  context.load(lobSystemInstanceCollection);

  context.executeQueryAsync(onLoadingLOBInstanceSucceeded, onError);

  function onLoadingLOBInstanceSucceeded() {
    lobSystemInstance = lobSystemInstanceCollection.get_item(0);
    // Reads the workflow task from SAP using specific finder.
    var identifierValues = [sapParameters.sapOrigin, sapParameters.workitemId];
    var entityIdentity = SP.BusinessData.Runtime.EntityIdentity.
          newObject(context, identifierValues);
    wfEntityInstance = wfEntity.findSpecific(entityIdentity, 
          "ReadSpecificWorkflowTask", lobSystemInstance);
    context.load(wfEntityInstance);

    context.executeQueryAsync(onLoadingWorkflowSucceeded, onError);
  }

  function onLoadingWorkflowSucceeded() {
    // Reads the associated elements for the workflow task.
    var exFilterCollection = wfExtensibleElementEntity.getFilters(
        "GetExtensibleElementsFromWorkflowTaskCollection");
    context.load(exFilterCollection);
    exElementsCollection = wfExtensibleElementEntity.findAssociated(
        wfEntityInstance, "GetExtensibleElementsFromWorkflowTaskCollection", 
        exFilterCollection, lobSystemInstance);
    context.load(exElementsCollection);

    var comFilterCollection = wfCommentsEntity.getFilters(
        "GetCommentsFromWorkflowTaskCollection");
    context.load(comFilterCollection);
    commentsCollection = wfCommentsEntity.findAssociated(
        wfEntityInstance, "GetCommentsFromWorkflowTaskCollection", 
        comFilterCollection, lobSystemInstance);
    context.load(commentsCollection);

    var decFilterCollection = wfDecisionsEntity.getFilters(
        "GetDecisionOptionsFromWorkflowTaskCollection");
    context.load(decFilterCollection);
    decisionsCollection = wfDecisionsEntity.findAssociated(
        wfEntityInstance, "GetDecisionOptionsFromWorkflowTaskCollection", 
        decFilterCollection, lobSystemInstance);
    context.load(decisionsCollection);

    var partFilterCollection = wfParticipantEntity.getFilters(
        "GetParticipantsFromWorkflowTaskCollection");
    context.load(partFilterCollection);
    participantsCollection = wfParticipantEntity.findAssociated(
        wfEntityInstance, "GetParticipantsFromWorkflowTaskCollection", 
        partFilterCollection, lobSystemInstance);
    context.load(participantsCollection);

    var descFilterCollection = wfDescriptionEntity.getFilters(
        "GetDescriptionFromWorkflowTaskCollection");
    context.load(descFilterCollection);
    descriptionCollection = wfDescriptionEntity.findAssociated(
        wfEntityInstance, "GetDescriptionFromWorkflowTaskCollection", 
        descFilterCollection, lobSystemInstance);
    context.load(descriptionCollection);

    var attaFilterCollection = wfAttachmentEntity.getFilters(
        "GetAttachmentsFromWorkflowTaskCollection");
    context.load(attaFilterCollection);
    attachmentCollection = wfAttachmentEntity.findAssociated(
        wfEntityInstance, "GetAttachmentsFromWorkflowTaskCollection", 
        attaFilterCollection, lobSystemInstance);
    context.load(attachmentCollection);

    context.executeQueryAsync(getUpdatedDataSucceeded, onError);
  }

  function getUpdatedDataSucceeded() {
    alert("# of extensible elements: " + exElementsCollection.get_count() + 
      "\\n# of comments: " + commentsCollection.get_count() + 
      "\\n# of decision options: " + decisionsCollection.get_count() + 
      "\\n# of attachments: " + attachmentCollection.get_count() + 
      "\\n# of participants: " + participantsCollection.get_count());
  }

  function onError(sender, e) {
    alert("Request failed. " + e.get_message() +
          "\\n" + e.get_stackTrace());
  }
}

// Retrieves a query string value.
function getQueryStringParameter(paramToRetrieve) {
  var params = document.URL.split("")[1].split("&amp;");
  var strParams = "";
  for (var i = 0; i < params.length; i = i + 1) {
    var singleParam = params[i].split("=");
    if (singleParam[0] == paramToRetrieve)
      return singleParam[1];
  }
}

  ```

6. 打开 **AppManifest.xml** 文件。
    
  
7. 在"权限请求"中，依次选择"范围"和"Web 和权限"，然后将值设置为"读取"。
    
  
8. 按"F5"将解决方案部署到 Duet 工作流网站。
    
  
9. 转到 **我的工作流任务**。从 ECB 菜单选择"查看详细信息"，或者选择一个任务并按功能区的"查看详细信息"。系统会将您重定向到 **ViewDetails.aspx**，您会在此看到包含与工作流相关的一些元素的计数的警报。
    
  

## 使用 HTML 和 JavaScript 呈现自定义 UI
<a name="bkmk_UseJSOM"> </a>

在 ViewDetails.aspx 中，将以下代码替换为您自己的 HTML 和 JavaScript 以呈现自己的自定义 UI。
  
    
    

```

alert("# of extensible elements: " + exElementsCollection.get_count() +
"\\n# of comments: " + commentsCollection.get_count() + 
"\\n# of decision options: " + decisionsCollection.get_count() + 
"\\n# of attachments: " + attachmentCollection.get_count() + 
"\\n# of participants: " + participantsCollection.get_count());

```


## 向详细信息页添加 Lync 控件
<a name="bkmk_UseJSOM"> </a>

此处有一个用于自定义用户界面的选项。添加 Lync 控件会让您能够从自定义页面与 Lync 联系人进行通信。
  
    
    

### 若要添加 Lync 控件，请执行以下操作：


1. 右键单击解决方案资源管理器中的"脚本"文件夹、添加 JavaScript 文件并将其命名为 **People.js**。
    
  
2. 以下标记将：
    
  - 为任务参与者添加状态控件。
    
  
  - 标注中的 Lync 集成供参与者对任务进行一键式合作。
    
  

    将以下代码粘贴到 **People.js** 页面。
    


  ```
  
var presenceControlCount = 0;
var appWebURL = '';

function AddPeopleControl(userName) {
  var id = "pc_participants_" + presenceControlCount++;
  var prevId = null;

  return "<div id='" + id + "' ></div>" + 
    "<script>" + 
      "LoadPeopleControl('" +id+ "', '" +prevId+ "', '" +userName+ "');" + 
    "</script>";
}

function LoadPeopleControl(elemId, prevId, user) {
  var prevElement = null;
  var element = $("#" + elemId);
  user = "fareast\\\\" + user.toLowerCase();

  GetUsersInfo(user, element, prevElement);
}

function GetUsersInfo(accountName, htmlElement, waitforElement) {
  var clientContext = SP.ClientContext.get_current();
  var website = clientContext.get_web();

  clientContext.load(website);
  clientContext.executeQueryAsync(onRequestSucceeded, onRequestFailed);

  function onRequestSucceeded() {
    appWebURL = website.get_url();
    var queryURL = appWebURL + 
    "/_api/SP.UserProfiles.PeopleManager/GetPropertiesFor(accountName=@v)?@v='"
     + accountName + "'";

    jQuery.ajax({
      url: queryURL,
      type: "GET",
      headers: {
        "ACCEPT": "application/json;odata=verbose"
      },
      success: function (data) {
        var html = [];
        var i = 0;

        var id = htmlElement[0].id + "_";

        var about_info;

        if (data.d.UserProfileProperties != null) {
          for (i = 0; i < data.d.UserProfileProperties.results.length; i++) {
            if (data.d.UserProfileProperties.results[i].Key == "AboutMe") {
              about_info = data.d.UserProfileProperties.results[i].Value;
            }
          }
        }

        var email = data.d.Email;
        var profileUrl = data.d.UserUrl;
        var pictureUrl = data.d.PictureUrl;
        var name = data.d.DisplayName;

        if (name == null) {
          name = accountName;
        }

        var isFollowed = data.d.IsFollowed;

        var html = [
          "<table>",
          "<tr>",
          "<td>",
            "<span class='ms-spimn-presenceWrapper ms-spimn-imgSize-5x48'> ",
              "<img id='imn_" + id + "_" + i++ + ",type=smtp'" ,
                "onload=\\"IMNRC('", email, "')\\" ",
                "class='ms-spimn-img ms-spimn-presence-offline-5x48x32 '", 
                "title='' name='imnmark' alt='Offline' ", 
                "src='/_layouts/15/images/spimn.png' ", 
                "sip='" + email + "' showofflinepawn='1'>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-imnSpan'>",
              "<a class='ms-imnlink' tabindex='-1'", 
                "onclick='IMNImageOnClick(event);return false;' href='#'>",
                "<img id='imn_" + id + "_" + i++ + ",type=smtp'",
                  "onload=\\"IMNRC('", email, "')\\"",
                  "class=' ms-hide' title='' name='imnmark' alt='Away' ",
                  "src='/_layouts/15/images/spimn.png' ",
                  "sip='" + email + "' showofflinepawn='1'>",
              "</a>",
              "<a class='ms-subtleLink ms-peopleux-imgUserLink' ",
                "onclick='GoToLinkOrDialogNewWindow(this);return false;' ",
                "href='" + profileUrl + "'>",
                "<span style='width: 48px; height: 48px;'" ,
                  "class='ms-peopleux-userImgWrapper'>",
                "<img style='cliptop: 0px; clipright: 48px;" ,
                  "clipbottom: 48px; clipleft: 0px; ",
                  "min-height: 48px; min-width: 48px; max-width: 48px;' ",
                  "class='ms-peopleux-userImg' alt='' src='" +pictureUrl+ "'>",
                "</span>",
              "</a>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-spimn-presenceWrapper ms-spimn-imgSize-5x48'>",
            "<img id='imn_" + id + "_" + i++ + ",type=smtp' ",
              "onload=\\"IMNRC('", email, "')\\" ",
              "class='ms-spimn-img ms-spimn-presence-offline-5x48x32' ",
              "title='' name='imnmark' alt='Offline' ",
              "src='/_layouts/15/images/spimn.png' ",
              "sip='" + email + "' showofflinepawn='1'>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-microfeed-userName ms-textLarge' ",
              "style='font-size: medium;'>" + name + "<br/>",
              "<div id='people_callout_" + id + "_" + i + "' ",
                "class='ms-microfeed-button ms-textSmall" + 
                "ms-secondaryCommandLink ms-microfeed-footerButton' ",
                "align='center' ",
                "style='cursor:pointer; font-size:small;' > more... </div>",
                  "<script>",
                    "RegisterCallOut(\\"people_callout_" + id + "_" + i + 
                      "\\",\\"" + name + "\\",\\"" + encodeURI(about_info) + 
                      "\\",\\"" + profileUrl + "\\",\\"" + isFollowed + "\\");",
                  "</script>",
            "</span>",
          "</td>",
          "</tr>",
          "</table>",
        ];

        htmlElement.html(html.join(''));
      }

    });
  }
  function onRequestFailed(sender, args) {
    alert('Error: ' + args.get_message());
  }

}

function RegisterCallOut(divId, displayName, aboutme, userUrl, isFollowed) {
  if (typeof CalloutManager !== "object" || typeof Callout !== "function" || typeof CalloutAction !== "function")
    return;

  var launchdiv = document.getElementById(divId);
  var calloutId = divId + "_callout";
  var html = [];

  html.push("<br/>");

  if (aboutme == "") {
    html.push("No Information Found about this person.");
  }
  else {
    html.push(decodeURI(aboutme));
  }

  html.push("<hr/>");

  if (isFollowed == true) {
    html.push("<div>You are <b>following</b> this person</div>");
  }
  else {
    html.push("<div >You are <b>not following</b> this person</div>");
  }

  var callout = CalloutManager.createNew({
    launchPoint: launchdiv,
    openOptions: {
      closeCalloutOnBlur: true,
      event: "click",
      showCloseButton: true
    },
    ID: calloutId,
    title: displayName,
    content: html.join("")
  });

  callout.addAction(new CalloutAction({
    text: "View Profile", onClickCallback: 
    function (calloutActionClickEvent, calloutAction) {
      window.open(userUrl);
    }
  }));
}

  ```


> **注释**
> 公司网络中参与者的用户名与 SAP 中的用户名相同。 
3. 打开 **AppManifest.xml** 页面。
    
  
4. 在"权限请求"中，依次选择"范围"和"用户配置文件和权限"，然后将值设置为"读取"。
    
  
5. 复制以下标记，然后将其粘贴到 **ViewDetails.aspx** 中的 `PlaceHolderAdditionalPageHead` 部分。
    
  ```HTML
  
<script src="../Scripts/People.js" type="text/javascript"></script>
  ```

6. 若要为参与者添加状态控件，请调用以下方法： 
    
  ```
  AddPeopleControl(userName);
  ```


## 其他资源
<a name="bk_addresources"> </a>


-  [SharePoint 外接程序](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)
    
  
-  [使用 SharePoint 2013 客户端库代码完成基本操作](http://msdn.microsoft.com/library/5a69c9e3-73bf-4ed5-bc19-182056bdb394%28Office.15%29.aspx)
    
  
-  [SAP 业务工作流简介](http://scn.sap.com/docs/DOC-31056)
    
  

