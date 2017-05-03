---
title: 如何：在 SharePoint 2013 中添加安全修整代码段
ms.prod: SHAREPOINT
ms.assetid: 4beaab08-760b-408a-b768-906312779379
---


# 如何：在 SharePoint 2013 中添加安全修整代码段
您可以使用安全修整代码段，根据特定用户必须拥有的特定权限以及他们是通过身份验证的用户还是匿名用户，来仅向这些用户显示内容。
## 安全修整代码段简介
<a name="Introduction"> </a>

您可以使用安全修整代码段，根据特定用户必须拥有的特定权限以及他们是通过身份验证的用户还是匿名用户，来仅向这些用户显示内容。您可以将安全修整面板添加到母版页或页面布局中。安全修整面板是一种容器，除静态内容之外，还可以包含 Web 部件等其他组件或代码段。
  
    
    
例如，您可以使用安全修整面板对指定用户显示以下内容：
  
    
    

- 一个"搜索的内容"Web 部件，它显示经过身份验证的用户当前正在使用的文档。
    
  
- 最近已修改文档的列表视图，以便经过身份验证的用户看到网站上的新增内容。
    
  
- 一个"搜索的内容"Web 部件，它可基于当前文章对未经身份验证的访客显示推荐链接列表。这个推荐列表可能会对正在网站上操作且经过身份验证的内容作者造成干扰，但对于未经身份验证的访客而言却非常重要。
    
  
- 一个与功能区分离的登录链接，面向未经身份验证的用户或尚待验证的用户。
    
    > **注释**
      <br/> 这个登录链接会自动插入到使用设计管理器创建的母版页中，但如果没有必要，也可以删除此链接。 
安全修整面板有两个重要的属性设置，一个用于身份验证，另一个用于权限（或授权）。例如，您可以使用安全修整面板对指定用户显示以下内容：
  
    
    

- **AuthenticationRestrictions**：通过此属性，您可以将面板限制为仅面向经过身份验证的用户或匿名用户显示，也可以选择所有用户（所有用户是默认设置）。
    
  
- **Permissions**：通过此属性，您可以选择用户查看面板内容时必须具有的一个特定权限。 
    
    > **注释**
      <br/> 您可以选择单个权限，而不是一个权限级别。（权限级别是一组已授予的权限。） 
当然，如果您限制为只对匿名用户进行身份验证，那么通常不必指定特定权限，因为匿名用户一般不会获得任何 SharePoint 2013 权限。仅对所有用户或所有经过身份验证的用户使用权限才具有意义。
  
    
    
安全修整面板的功能区中有三个选项，如表 1 的左列所示。表 1 显示了这些设置如何确定用户需要具备的特定权限、包含相应特定权限的最低默认权限级别，以及默认链接到该权限级别的组。
  
    
    

> **注释**
> 这些是默认设置，可以在任意给定范围内进行更改，如网站集、网站、列表或项。 
  
    
    

例如，如果您将安全修整面板设置为"向作者显示"，默认情况下，面板内部的内容会对成员组用户和所有者组中的用户可见。
  
    
    

**表 1. 面板选项到默认权限级别和组的映射**


|**安全修整面板选项**|**权限属性**|**权限**|**权限级别**|**组**|
|:-----|:-----|:-----|:-----|:-----|
|向作者显示  <br/> |**AddAndCustomizePages** <br/> |添加和自定义页面  <br/> |参与（或更高）  <br/> |成员  <br/> |
|对经过身份验证的用户显示  <br/> |**ViewPages** <br/> |查看页面  <br/> |读取（或更高）  <br/> |访问者  <br/> |
|向管理员显示  <br/> |**FullMask** <br/> |选择所有  <br/> |完全控制  <br/> |所有者  <br/> |
   

### 插入安全修整面板
<a name="InsertSnippet"> </a>

与所有代码段类似，您可以从代码段库添加安全修整代码段。若要导航到代码段库，必须先选择一个要编辑的母版页或页面布局。
  
    
    

### 插入安全修整面板


1. 浏览到您的发布网站。
    
  
2. 在页面右上角，选择"设置"齿轮，然后选择"设计管理器"。
    
  
3. 在设计管理器的左导航窗格中，选择"编辑母版页"或"编辑页面布局"，具体取决于您所编辑的文件类型。
    
  
4. 选择您要向其中添加代码段的母版页或页面布局的名称。
    
  
5. 若要打开代码段库，请选择服务器端预览右上角的"代码段"。
    
  
6. 在功能区的"设计"选项卡上，选择"安全修剪"。
    
    （可选）在"安全修剪"按钮上的下拉列表中，可以选择可看见面板的用户，或者可以通过配置面板的重要属性值查看更多选项。
    
  
7. 在代码段库右侧的"关于此组件"下，单击或选择节标题以展开或折叠属性组，然后配置所需的任何自定义设置。
    
  
8. 配置任何属性后，选择"更新"。这将更新页面左侧的 HTML 代码段，以使标记反映出您的自定义设置。您可以随时选择"重置"将所有属性恢复为其默认设置。
    
  
9. 在代码段库左侧的"HTML 代码段"下，选择"复制到剪贴板"。
    
  
10. 在 HTML 编辑器中，打开您计算机上的映射网络驱动器，然后打开您要向其添加代码段的母版页或页面布局对应的 HTML 文件。
    
  
11. 在 HTML 文件中，将代码段粘贴到希望显示标记的位置。
    
    如果您将代码段添加到页面布局，请确保将代码段粘贴到 **PlaceHolderMain** 内部。
    
  
12. 将  `class="DefaultContentBlock"` 的 **<div>** 替换为您自己的特定内容。
    
  
13. 保存该页面，然后刷新设计管理器中的服务器端预览，以确保安全修整面板按预期方式显示。
    
  

## 理解代码段标记
<a name="UnderstandMarkup"> </a>

安全修整代码段最重要的部分是 **AuthenticationRestrictions** 属性和 **Permissions** 属性以及下方加粗的 **<div>**。默认情况下， **AuthenticationRestrictions** 仅当从 **AllUsers** 更改时，才会出现在标记中。如果您选择在代码段库中对代码段进行"重置"，就会将 **AuthenticationRestrictions** 从标记中删除，这意味着代码段将使用默认值 **AllUsers**。
  
    
    
 **<div>**（其中  `class="DefaultContentBlock"`）是使用自己的内容替换的内容，其中可包含其他代码段和控件。
  
    
    



```HTML

<div data-name="SecurityTrimmedAuthors">
    <!--CS: Start Security Trim Snippet-->
    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
    <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AuthenticatedUsersOnly" Permissions="AddAndCustomizePages" PermissionContext="RootSite">-->
        <!--PS: Start of READ-ONLY PREVIEW (do not modify)--><span><!--PE: End of READ-ONLY PREVIEW-->
        <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
        You should replace this div with content that renders based on your Security Trim Properties.
        </div>
        <!--PS: Start of READ-ONLY PREVIEW (do not modify)--></span><!--PE: End of READ-ONLY PREVIEW-->
    <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
    <!--CE: End Security Trim Snippet-->
</div>
```


## 其他资源
<a name="AdditionalResources"> </a>


-  [简介：控制用户访问权限](http://office.microsoft.com/zh-cn/sharepoint-foundation-help/introduction-control-user-access-with-permissions-HA102771919.aspx?CTT=1)
    
  
-  [了解权限级别](http://office.microsoft.com/zh-cn/products/default-permission-levels-HA102772313.aspx?CTT=5&amp;origin=HA102771919)
    
  
-  [SharePoint 2013 设计管理器代码段](sharepoint-2013-design-manager-snippets.md)
    
  
-  [为 SharePoint 构建网站](build-sites-for-sharepoint.md)
    
  
-  [在 SharePoint 2013 中开发网站设计](develop-the-site-design-in-sharepoint-2013.md)
    
  

