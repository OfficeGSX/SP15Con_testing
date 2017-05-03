---
title: 如何：在 SharePoint 2013 中创建最简单的母版页
ms.prod: SHAREPOINT
ms.assetid: 634aa471-07e1-41d6-aa80-27f7ef7e9dc8
---


# 如何：在 SharePoint 2013 中创建最简单的母版页
一个内容最少的母版页仅包含 SharePoint 2013 所需的页面元素，用来在浏览器中正确呈现页面。您可以使用设计管理器快速创建一个内容最少的母版页，而不必先设计并转换 HTML 文件。
## 内容最少的母版页简介
<a name="Introduction"> </a>

使用设计管理器，您可以将典型 HTML 文件转换到 SharePoint 2013 母版页中。但是，如果您没有预建的模型，您可以通过创建一个内容最少的母版页来迅速从头开始构建。内容最少的母版页仅包含 SharePoint 所需的页面元素，用来在浏览器中呈现页面。
  
    
    
当您创建内容最少的母版页时，设计管理器会创建 .master 文件和相关联的 HTML 文件，这样，您仍可以按照需要只使用 HTML 文件。使用内容最少的母版页和使用从 HTML 文件转换来的母版页完全一样。HTML 文件和母版页已关联，因此在您编辑并保存 HTML 文件后，更改内容将同步到关联的母版页。HTML 文件包含特殊类型的标记，能实现与 .master 文件的同步。有关此关联和这些标记类型的详细信息，请参阅 [如何：在 SharePoint 2013 中将 HTML 文件转换为母版页](how-to-convert-an-html-file-into-a-master-page-in-sharepoint-2013.md)。
  
    
    
在下列情况下，以内容最少的母版页作为开始非常有用：
  
    
    

- 您想迅速从头开始，然后在与内容最少的母版页关联的 HTML 文件中构建自己的设计，而不是使用模型 HTML 文件作为开始。
    
  
- 您想快速测试或想以一个需要有效 SharePoint 母版页的设计元素为原型。例如，创建内容最少的母版页不需要准备一个 HTML 文件用于转换，也无需解决由 HTML 文件中的无效标记导致的预览错误。这意味着您可以立即使用服务器端预览或代码段库。
    
  
- 您想直接使用 .master 文件。如果您是 ASP.NET 开发人员或 SharePoint 开发人员，您可以创建一个内容最少的母版页，通过清除 HTML 文件属性中的 **关联的文件**复选框来删除 HTML 文件与 .master 文件之间的关联，然后直接使用 .master 文件。
    
  

## 创建一个内容最少的母版页
<a name="CreateMinimalMaster"> </a>


  
    
    

### 创建一个内容最少的母版页


1. 浏览到您的发布网站。
    
  
2. 在页面右上角，选择"设置"，然后选择"设计管理器"。
    
  
3. 在设计管理器的左侧导航窗格中选择"编辑母版页"。
    
  
4. 选择"创建内容最少的母版页"。
    
  
5. 在"创建母版页"对话框中，输入母版页的名称，然后选择"确定"。
    
    此时，SharePoint 在母版页样式库中创建了同名的 .master 文件和相关联的 HTML 文件。
    
    设计管理器中将显示您的 HTML 文件，状态栏中显示 **转换成功**。
    
  
6. 跟随状态栏中的链接预览文件。
    
    预览页面是母版页的实时服务器端预览。
    
    有关使用不同页面预览母版页的详细信息，请参阅 [如何：在 SharePoint 2013 设计管理器中更改预览页面](how-to-change-the-preview-page-in-sharepoint-2013-design-manager.md)。
    
    预览页面的右上角还包含一个"代码段"链接。此链接可打开代码段库，您可在其中将设计中的静态或模型控件替换为动态 SharePoint 控件。有关详细信息，请参阅 [SharePoint 2013 设计管理器代码段](sharepoint-2013-design-manager-snippets.md)。
    
    在母版页预览成功后，您将看到添加到 HTML 文件中的 **<div>**-Tag 标记。您可能必须滚动到页面底部才能看到 **<div>**-Tag 标记。
    
    此 **<div>**-Tag 是主内容块。它驻留在名为 **ContentPlaceHolderMain** 的内容占位符中。在运行时，如果访问者浏览您的网站并请求某页面，此内容占位符会使用页面布局中包含匹配内容区域中的内容的相应内容进行填充。您应将此 **<div>**-Tag 置于您希望页面布局在母版页上显示的位置。
    
  
7. 您可以编辑直接位于服务器上的 HTML 文件，方法是使用 HTML 编辑器打开和编辑映射驱动器中的 HTML 文件。每次保存 HTML 文件后，所做更改都会同步到关联的 .master 文件中。有关详细信息，请参阅 [如何：将网络驱动器映射到 SharePoint 2013 母版页样式库](how-to-map-a-network-drive-to-the-sharepoint-2013-master-page-gallery.md)。
    
  
8. 若要仅使用 .master 文件而不是 HTML 文件，您必须中断这两个文件之间的关联。在设计管理器中的"编辑母版页"页面上，选择该 HTML 文件，打开"属性"菜单，然后选择"编辑属性"。在"编辑"选项卡上，清除"关联的文件"复选框，然后选择"保存"。
    
    中断关联可使您直接使用 .master 文件，并保存更改，而这些更改不会被 HTML 文件中的更改覆盖。您可以随时还原此关联。如果您还原了关联，则关联的 HTML 文件将会与 .master 文件进行同步，并对其进行覆盖。
    
  

## 了解关联的 HTML 文件
<a name="UnderstandHTML"> </a>

当您创建内容最少的母版页时，会创建一个与 .master 文件相关联的 HTML 文件，此 HTML 文件包含特定于 SharePoint 2013 如何工作的多行标记。您可以放心地忽略大部分标记，在浏览器中查看源代码时，大多数标记都不会显示在网站的最终标记位置，但此类标记对将 HTML 文件中的更改同步到 SharePoint 2013 实际使用的 .master 文件至关重要。每次保存 HTML 文件的更改内容后，使用此类 SharePoint 标记，可在后台对关联的 .master 文件做出相同的更改。有关详细信息，请参阅 [如何：在 SharePoint 2013 中将 HTML 文件转换为母版页](how-to-convert-an-html-file-into-a-master-page-in-sharepoint-2013.md)中的标记示例。
  
    
    

## 其他资源
<a name="Additional"> </a>


-  [SharePoint 2013 中的设计管理器概述](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中创建页面布局](how-to-create-a-page-layout-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中将 HTML 文件转换为母版页](how-to-convert-an-html-file-into-a-master-page-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 设计管理器代码段](sharepoint-2013-design-manager-snippets.md)
    
  

