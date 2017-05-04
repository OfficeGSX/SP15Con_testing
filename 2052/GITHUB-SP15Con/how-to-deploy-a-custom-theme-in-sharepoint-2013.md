---
title: 如何：在 SharePoint 2013 中部署自定义主题
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# 如何：在 SharePoint 2013 中部署自定义主题
了解如何使用用户界面或通过实施功能接收器将自定义主题部署到 SharePoint 2013 网站。
SharePoint 2013 包括预先安装的主题。您可以通过创建更多调色板、字体方案和母版页，创建自定义主题。将文件上载到主题库和母版页样式库之后，您即可使用用户界面或代码将主题部署到网站。有关调色板和字体方案的详细信息，请参阅 [SharePoint 2013 中的调色板和字体](color-palettes-and-fonts-in-sharepoint-2013.md)。
  
    
    


## 要部署主题需了解的核心概念
<a name="core"> </a>

表 1 列出了帮助您了解部署主题的核心概念的文章。
  
    
    

**表 1. 要部署主题需了解的核心概念**


|**文章标题**|**说明**|
|:-----|:-----|
| [SharePoint 2013 主题概述](themes-overview-for-sharepoint-2013.md) <br/> |了解 SharePoint 2013 中的主题设置体验。  <br/> |
| [功能事件](http://msdn.microsoft.com/zh-cn/library/ms469501.aspx) <br/> |了解功能事件，以便捕获并响应在服务器场中安装功能时发生的事件。  <br/> |
   

## 将文件上载到主题库和母版页样式库
<a name="section1"> </a>

您可以通过创建更多调色板和字体方案并将其上载到主题库，来创建自定义主题。当您修改主题设置体验设计或以编程方式应用主题时，您即可使用新的调色板和字体方案。同样，如果您希望具有更多可供选择的网站布局，您可以将更多母版页及相应的预览文件上载到母版页样式库。以下列表说明了将文件放置在何处：
  
    
    

- **母版页样式库** 列出了母版页文件及相应的预览文件（.preview 文件）。如果您希望母版页在"更改外观"向导中可用，则需要母版页预览文件。JavaScript 文件和其他设计资产也可以上载到母版页样式库。
    
    要从 SharePoint 用户界面访问母版页样式库，请在"网站设置"页面上的"Web 设计器库"中选择"母版页"。您也可以直接导航到该网站 (http://  _SiteName_/_catalogs/masterpage/)。
    
  
- **主题库** 列出了可用于主题设置体验的调色板和字体方案。SharePoint 将查找"15"文件夹来确定可用的调色板和字体方案。
    
    要从 SharePoint 用户界面访问主题库，请在"网站设置"页面上的"Web 设计器库"中选择"主题"。您也可以直接导航到该网站 (http://  _SiteName__catalogs/theme/15/)。
    
  
- **样式库** 列出了您希望在主题设置体验中使用的自定义 CSS 文件。您可以直接导航到样式库（替换此 URL 中的 _SiteCollectionName_ 和 _language_：http:// _SiteCollectionName_/Style Library/ _language_/Themable/）。
    
> **注释**
>将自定义 CSS 文件放置在主题库的 Themable 文件夹中，而不是母版页样式库的 Themable 文件夹中。主题引擎仅可识别存储在样式库的 Themable 文件夹中的 CSS 文件。 

> **注释**
> 如果您在母版页样式库和主题库上启用了版本控制，您必须先发布设计文件，然后才可供主题引擎使用。 
  
    
    


## 使用用户界面部署主题
<a name="section2"> </a>

组合外观或设计包括决定网站外观的调色板、字体方案、背景图像与母版页。"组合外观"列表中包含设计库中提供的组合外观。您可以通过在"组合外观"列表中添加列表项并指定要使用的母版页、调色板、字体方案和背景图像来创建设计。
  
    
    

> **注释**
> 如果您希望母版页在设计库中可用，则需要母版页预览文件。 
  
    
    


### 添加组合外观


1. 选择"设置"图标，然后选择"网站设置"。
    
  
2. 在"Web 设计器库"下，选择"组合外观"。
    
  
3. 在"组合外观"列表中，选择"新建项目"。
    
  
4. 在"标题"文本框中，输入设计的标题。
    
  
5. 在"名称"文本框中，输入设计的名称。名称将显示在"组合外观"列表和设计库中。
    
  
6. 在"母版页 URL"文本框中，输入母版页的 URL。URL 可以是相对 URL。
    
  
7. 在"主题 URL"文本框中，输入调色板的 URL（.spcolor 文件的 URL）。URL 可以是相对 URL。
    
  
8. 在"图像 URL"文本框中，输入背景图像的 URL。这是可选项。URL 可以是相对 URL。
    
  
9. 在"字体方案 URL"文本框中，输入字体方案的 URL（.spfont 文件的 URL）。这是可选项。URL 可以是相对 URL。
    
  
10. 在"显示顺序"文本框中，输入显示顺序编号。这确定了设计在设计库中的位置。
    
  
11. 选择"保存"。
    
    > **注释**
      > 如果组合外观的值有问题，该组合外观将不会添加到设计库中，且不会在日志文件中记录消息。下面列出了组合外观无法添加的可能原因：找不到文件、其中一个文件格式有问题或者 SharePoint 无法访问文件。 
现在，您可以使用设计库将新设计应用到您的网站。有关详细信息，请参阅 Office.com 上的 [为发布网站选择主题](http://office.microsoft.com/zh-cn/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx)。
  
    
    

## 使用代码部署主题
<a name="section3"> </a>

您可以通过实施功能接收器来部署主题。
  
    
    

### 使用功能接收器部署主题


1. 创建继承自  [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) 类的功能接收器类。
    
  
2. 在  [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) 方法中，创建使用调色板和字体方案的 [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) 对象，然后将主题应用到您的网站。
    
    以下代码示例说明如何将自定义调色板和字体方案部署到网站。
    


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


> **注释**
> **ApplyTo** 方法中的 _shareGenerated_ 参数指定是否可以在网站集中的网站之间共享主题文件。通常，对于 SharePoint Server 和 SharePoint Online 网站，设置为 **true**；对于 SharePoint Foundation 网站，设置为 **false**。如果您希望共享主题文件，必须将  _shareGenerated_ 参数设置为 **true**。有关详细信息，请参阅  [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) 。

当用户在"更改外观"向导中应用主题时，该向导还会更新"组合外观"列表和设计库中名为"当前"的主题。当您以编程方式应用主题时，必须手动更新"当前"主题。以下示例说明如何更新"当前"主题。
    


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


## 其他资源
<a name="bk_addresources"> </a>


-  [在 SharePoint 2013 中开发网站设计](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的调色板和字体](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中创建母版页预览文件](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [SharePoint 团队博客：通过 SharePoint 主题设置秀出自己的风格](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

