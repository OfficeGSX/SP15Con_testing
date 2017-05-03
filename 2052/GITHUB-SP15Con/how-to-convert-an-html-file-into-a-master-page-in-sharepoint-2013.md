---
title: 如何：在 SharePoint 2013 中将 HTML 文件转换为母版页
ms.prod: SHAREPOINT
ms.assetid: a76ab289-3256-45de-ac63-d5112a74e3c7
---


# 如何：在 SharePoint 2013 中将 HTML 文件转换为母版页
借助设计管理器，您可以将 .html 文件转换为 SharePoint 2013 母版页（.master 文件）。转换后，HTML 文件和母版页相关联，因此在您编辑并保存 HTML 文件后，这些更改内容会同步到关联母版页中。
## 转换母版页简介
<a name="Introduction"> </a>

借助设计管理器，您可以将 .html 文件转换为 SharePoint 2013 母版页（.master 文件）。转换后，HTML 文件和母版页相关联，因此在您编辑并保存 HTML 文件后，这些更改内容会同步到关联母版页中。
  
    
    
为什么要转换 HTML 文件，而不是从头开始创建 .master 文件？在 SharePoint 2013 中，母版页就像在 ASP.NET 中一样工作，但 SharePoint 还要求特定于 SharePoint 的某些元素（如控件和内容占位符）必须存在于该页面上，SharePoint 才能正确呈现该母版页。使用设计管理器将 HTML 文件转换为完整功能的 SharePoint 母版页，您就不必知道 ASP.NET 或 SharePoint 特定标记，而将精力集中于用 HTML、CSS 和 JavaScript 设计您的网站。
  
    
    
在将 HTML 文件转换为母版页时：
  
    
    

- 已在母版页样式库中创建与 HTML 文件同名的 .master 文件。
    
  
- SharePoint 2013 需要的所有标记均已添加到 .master 文件中，因此该母版页可以正确呈现。
    
  
- 标记（如注释、 **<div>** 标记、代码段和内容占位符）均已添加到您的原始 HTML 文件中。
    
  
- HTML 文件和母版页相关联，因此在保存 HTML 文件后，对 HTML 文件做出的任何后续编辑都会同步到 .master 文件中。
    
  

> **注释**
> 仅单向进行同步。对 HTML 母版页做出的更改会同步到关联 .master 文件中，但如果选择直接编辑 .master 文件，则更改内容不会同步到 HTML 文件中。每个 HTML 母版页（和每个 HTML 页面布局）都有一个名为"关联的文件"的属性，默认情况下，该属性设置为"True"，用于在文件之间创建关联和同步。 
  
    
    

如果有一对关联的文件（HTML 和 .master），在不中断关联的情况下编辑 .master 文件，虽然可以保存对 .master 文件做出的更改，但不能签入或发布该 .master 文件，因此无法有意义地保存这些更改。对 HTML 文件做出的任何更改都会覆盖 .master 文件。如果签入或发布该 HTML 文件，则 HTML 文件的更改内容会覆盖对 .master 文件做出的任何更改。对 .master 文件做出的更改则会丢失。
  
    
    
如果您是习惯使用 ASP.NET 的开发人员，则可以选择中断这两个文件之间的关联，仅使用 .master 文件。若要中断 HTML 文件和 .master 文件之间的关联，请在设计管理器中，对 HTML 文件选择"编辑属性"，然后清除"关联的文件"复选框。您稍后可通过编辑这些属性并选中此复选框来重新关联这些文件，在此情况下，HTML 文件会重新覆盖 .master 文件，对 .master 文件做出的更改会丢失。
  
    
    

## 准备要转换的 HTML 文件
<a name="Prepare"> </a>

在转换 HTML 文件之前，要考虑一些最佳实践和指导原则：
  
    
    

- 考虑 SharePoint 页面模型。有关详细信息，请参阅  [SharePoint 2013 页面模型概述](overview-of-the-sharepoint-2013-page-model.md)。在设计网站的 HTML 模型时，您可能有几个不同页面类型的 HTML 文件，如文章页面或类别页面，后者包含显示目录中项目类别的 Web 部件。但只能将一个 HTML 文件转换为母版页。虽然可以将 HTML 文件转换为母版页，但不能将 HTML 文件直接转换为页面布局，因为页面布局需要页字段。
    
  
- 确保您的 HTML 文件与 XML 兼容。要使转换成功，HTML 文件必须与 XML 兼容。很遗憾，此要求会使某些 HTML 5 标准无效，例如，在 HTML 5 中，可以使用小写形式指定 **doctype**，但在 XML 中， **doctype** 必须大写。此外，您还应从 HTML 文件中删除所有 **<form>** 标记。在转换之前，考虑通过外部 XML 验证器运行 HTML 文件来确定 XML 错误。
    
  
- 考虑关于 CSS 引用的以下重要指导原则：
    
  - 不要在 **<head>** 标记中放置 **<style>** 块。在转换过程中，系统会删除这些样式。而是从 HTML 文件中链接到外部 CSS 文件。
    
  
  - 如果您使用的是 Web 字体，则将  `ms-design-css-conversion="no"` 添加到 **<CSS link>** 标记中。
    
  
  - 在对一般 HTML 标记（例如 **<body>**、 **<div>** 和 **< img>**）应用样式时要谨慎。SharePoint 设计中的所有内容（包括功能区）都在 **<body>** 标记内。对于经常应用到 **<body>** 标记的样式，请考虑将它们应用到 **<div id="s4-bodyContainer">**，此标记是 SharePoint 2013 用于页面正文的标记。此外，SharePoint 2013 还会使用许多图像，这些图像会受您应用到 **<img>** 标记的样式的影响。
    
  
  - 许多设计者都会设计导航样式，方法是将类应用到 **<ul>** 和 **<li>** 元素中。但是，SharePoint 2013 使用动态导航控件，您可以在代码段库中将此控件添加到母版页中。默认情况下，SharePoint 2013 导航控件已应用样式，您必须改写它们。
    
  
- 考虑以下有关文件命名的潜在问题：
    
  - 如果您有 Index.html 和 Index.htm，则这些文件将具有同一 .master 文件。
    
  
  - 如果您有 Design/Index.html 和 Design/SubDesign/Index.html，则这两个文件均可用于转换为其自己单独的 .master 文件，但它们在设计管理器的母版页列表中都显示为 Index.html。若要消除其岐义，请单击或选择各个文件的省略号按钮，查看完整路径。
    
  
- 如果是添加 JavaScript 小组件，则确保 **<script>** 起始标记位于其各自的行中。
    
  ```
  
  <script>
  (function( …

  ```


    不要将它们放在同一行中，如下所示。
    


  ```
  
<Script> (function( …
  ```

- 对 JQuery 库的引用（外部引用）应位于 **</head>** 标记前面。
    
  

## 将 HTML 文件转换为母版页
<a name="Convert"> </a>

在转换 HTML 文件之前，必须先上载所有设计文件，包括 HTML 文件。有关详细信息，请参阅 [如何：将网络驱动器映射到 SharePoint 2013 母版页样式库](how-to-map-a-network-drive-to-the-sharepoint-2013-master-page-gallery.md)。
  
    
    

### 将 HTML 文件转换为 .master 文件


1. 浏览到您的发布网站。
    
  
2. 在页面右上角，选择"设置"，然后选择"设计管理器"\\。
    
  
3. 在设计管理器的左侧导航窗格中选择"编辑母版页"。
    
  
4. 选择"将 HTML 文件转换为 SharePoint 母版页"。
    
  
5. 在"选择资产"对话框中，浏览到要转换的 HTML 文件，并选择该文件。
    
    > **注释**
      > 在上载设计文件时，应将与单个设计相关的所有文件保存在母版页样式库中的其自己的文件夹中。在将设计文件夹复制到映射网络驱动器中时，母版页样式库会保持您创建的文件夹结构。 
6. 选择"插入"。
    
    此时，SharePoint 2013 会使用相同的名称将 HTML 文件转换为 .master 文件。
    
    在设计管理器中，现在将显示带有"状态"列的 HTML 文件，该列显示两个可能的状态之一：
    
  - 错误
    
  
  - **转换成功**
    
  
7. 使用"状态"列中的链接预览文件，并查看有关母版页的任何错误或警告。
    
    错误
    
    有关解决错误和警告问题的详细信息，请参阅 [如何：在 SharePoint 2013 中预览页面时解决错误和警告](how-to-resolve-errors-and-warnings-when-previewing-a-page-in-sharepoint-2013.md)。
    
    有关使用不同页面预览母版页的详细信息，请参阅 [如何：在 SharePoint 2013 设计管理器中更改预览页面](how-to-change-the-preview-page-in-sharepoint-2013-design-manager.md)。
    
    预览页面的右上角还包含一个"代码段"链接。此链接用于打开代码段库，您可在其中开始使用动态 SharePoint 控件开始替换您的设计中的静态或模型控件。有关详细信息，请参阅 [SharePoint 2013 设计管理器代码段](sharepoint-2013-design-manager-snippets.md)。
    
  
8. 若要修复任何错误，请编辑直接驻留在服务器上的 HTML 文件，方法是使用 HTML 编辑器打开并编辑映射驱动器中的该 HTML 文件。每次保存 HTML 文件后，所有更改都会同步到关联的 .master 文件中。
    
  
9. 在母版页预览成功后，您将看到添加到 HTML 文件中的 **<div>** 标记。您可能必须滚动到页面底部才能看到 **<div>** 标记。
    
    此 **<div>** 是主内容块。它驻留在名为 **ContentPlaceHolderMain** 的内容占位符中。在运行时，如果访问者浏览您的网站并请求某页面，此内容占位符会使用页面布局中包含匹配内容区域中的内容的相应内容进行填充。您应将此 **<div>** 置于您希望页面布局在母版页上显示的位置。
    
    如果 HTML 文件的页面正文中包含静态或模型内容，现在可以开始以下过程：从 HTML 母版页删除该静态内容并将这些样式应用到 SharePoint 页面模型的其他元素中，例如页面布局、页字段控件、代码段和显示模板。有关示例，请参阅 [如何：在 SharePoint 2013 中创建页面布局](how-to-create-a-page-layout-in-sharepoint-2013.md)。
    
  

## 了解转换后的 HTML 文件
<a name="Understand"> </a>

在将 HTML 文件转换为母版页后，HTML 文件中添加了多行标记。您可以放心地忽略大多数标记，在浏览器中查看源代码时，大多数标记都不会显示在网站的最终标记位置，但此类标记对将 HTML 文件转换为 SharePoint 实际使用的 .master 文件至关重要。每次保存 HTML 文件的更改内容后，使用此类 SharePoint 标记，可在后台对关联的 .master 文件做出相同的更改。
  
    
    
添加的标记包括 **<head>** 标记、代码段和内容占位符之前和之中的标记。大多数标记包含在注释标记内：每次保存 HTML 文件的更改内容时，转换过程都会删除这些注释，以便使用其中的 ASP.NET 标记。
  
    
    

### 标记类型

添加到 HTML 文件中的标记的类型细分如下：
  
    
    

- **文档属性** **<mso>** 标记包含 SharePoint 元数据，其中包括有关文件本身和成功转换为 .master 文件所需的某些属性的信息。
    
  ```HTML
  
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
  ```

- **SharePoint 命名空间注册** **<SPM>** 标记（"SharePoint 标记"）用于提供注册 SharePoint 命名空间的数据行。
    
  ```HTML
  
<!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
  ```

- **注释** 在转换过程中将忽略 **<CS>** 和 **<CE>**（"注释开始"和"注释结尾"）标记。这些标记可帮助您解析标记行。
    
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

- **代码段** **<MS>** 和 **<ME>**（"标记开始"和"标记结尾"）标记用于指示 SharePoint 控件或代码段的开始和结尾部分。代码段是将 SharePoint 功能添加到您的页面的 SharePoint 控件。您可以使用代码段库自己添加代码段。有关详细信息，请参阅 [SharePoint 2013 设计管理器代码段](sharepoint-2013-design-manager-snippets.md)。
    
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

- **预览块** **<PS>** 和 **<PE>**（"预览开始"和"预览结尾"）标记包围着一节不能编辑的 HTML 代码，因为这节代码仅影响设计时预览。这些预览节是向其插入代码段时 SharePoint 控件的快照。利用预览，您可以在客户端 HTML 编辑器中更有意义地处理 HTML 文件。但是，在该预览中更改内容或样式不会对 SharePoint 最终使用的 .master 文件产生永久影响。若要设计代码段的样式，必须标识 SharePoint 样式并用自己的自定义 CSS 覆盖这些样式。
    
  ```HTML
  
<!--PS: Start of READ-ONLY PREVIEW (do not modify) -->
<div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div>
<!--PE: End of READ-ONLY PREVIEW -->
  ```

- **SharePoint ID** 在转换过程中添加到 HTML 文件中的两个代码段（页眉内容代码段和 SharePoint 功能区）都有关联的 SharePoint ID 或 SID（分别为 00 和 02）。利用这些 ID，可以缩短这些代码段，使页面中的 HTML 更便于阅读。
    
  ```HTML
  
<!--SID:00 -->

<!--SID:02 {Ribbon}-->
  ```


### 添加的代码段

知道添加到 HTML 文件中的两个代码段很重要。这些代码段是在转换过程中自动添加的，您不能在代码段库中添加它们。
  
    
    

- **功能区** 为使内容作者能够在 SharePoint 网站上创建页面和创作内容，您的母版页需要功能区和"套件导航"（SharePoint 2013 的新增功能）。功能区已包含在安全修整代码段中，因此当访问者浏览您的网站时，功能区仅对通过身份验证的用户显示，而不对匿名用户显示。您可以将功能区移到页面上的其他位置或通过改写默认 CSS 类来设计其样式，但我们建议不要移动或重新排序功能区内包含的组件（例如"网站操作"菜单）。
    
  ```HTML
  
<!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
<!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
<!--ME:</wssucw:Welcome>-->
<!--ME:</SharePoint:SPSecurityTrimmedControl>-->
  ```

- **ContentPlaceHolderMain** 在 **<div id="s4-bodyContainer">** 标记底部、结尾 **</body>** 标记前面，转换过程插入了一个名为 **PlaceHolderMain** 的内容占位符。此代码段内包含黑框黄色的 **<div>**，它显示在 HTML 编辑器的设计视图中，或设计管理器的服务器端预览中。
    
    此 **<div>** 代表将放入页面布局和页面指定的内容的区域。您应将 **PlaceHolderMain** 代码段移到母版页中将由页面布局填充的位置，即在您网站的所有页面中不相同的网站设计的区域。
    


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


## 示例
<a name="Reference"> </a>

下面是 HTML 文件转换为母版页后添加到该文件中的标记的示例。
  
    
    

### 添加到 <head> 标记中的标记


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


### 在开始 <body> 标记后添加的标记


#### 功能区代码段


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


#### 两个 SharePoint <div> 标记


```HTML

<div id="s4-workspace">
            <div id="s4-bodyContainer">

```


### 在结尾 </body> 标记和两个结尾 </div> 标记前添加的标记


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


## 其他资源
<a name="Additional"> </a>


-  [SharePoint 2013 中的设计管理器概述](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中创建页面布局](how-to-create-a-page-layout-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 设计管理器代码段](sharepoint-2013-design-manager-snippets.md)
    
  

