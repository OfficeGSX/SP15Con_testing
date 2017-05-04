---
title: SharePoint 2013 中的编程模型
ms.prod: SHAREPOINT
ms.assetid: 061985ec-6129-4e91-991b-a72488ce1d34
---



# SharePoint 2013 中的编程模型
获取不同类型的 SharePoint 开发项目的简单概述。
  
    
    


|||
|:-----|:-----|
|**本文内容**<br/>          [SharePoint 加载项](#Apps)<br/>           [SharePoint 发布网站](#ECM)<br/>           [SharePoint 场解决方案](#Solutions)<br/>           [SharePoint 的移动加载项](#Mobile)<br/>           [SharePoint 的可重用组件](#Reuse)<br/>           [其他资源](#SP15devinSP_addlresources)|**观看视频：Office 2013 和 SharePoint 2013 的开发选项**<br/>![视频](images/mod_icon_video.png)|
   

可以通过多种方式开发针对 SharePoint 2013 平台的应用程序。可以基于以下各项将这些应用程序划分下列组中：用于创建应用程序的工具、用于开发应用程序的编程模型、打包和部署应用程序的方法、将应用程序投入市场的方式以及运行应用程序的设备。
  
    
    


- SharePoint 外接程序
    
  
- SharePoint 发布网站
    
  
- SharePoint 场解决方案
    
  
- SharePoint 的移动加载项
    
  
- SharePoint 的可重用组件
    
  
这些类别 *不*  是互斥的。例如，您可以将发布网站作为 SharePoint 外接程序进行开发。以下各节定义了这些类别并知道您查看每个类别的文档。
## SharePoint 加载项
<a name="Apps"> </a>

SharePoint 外接程序 类似于移动设备上的加载项。它是一种独立的生产力解决方案，可执行少量相关任务、轻松安装和完全卸载。用户可以从公共 SharePoint 加载项存储区或从其组织的企业加载项目录发现和下载 SharePoint 外接程序。SharePoint 外接程序 可包含经典 SharePoint 组件，如列表、自定义网站页、Web 部件、工作流和内容类型。但是，SharePoint 外接程序还可以呈现 SharePoint 中的远程 Web 应用程序和远程组件。SharePoint 外接程序 还可以包含 SharePoint 组件和远程组件。SharePoint 外接程序是非常安全的应用程序，其自定义逻辑始终"向上"转移到云或"向下"转移到客户端计算机。它绝不会在 SharePoint 服务器上运行。
  
    
    
有关 SharePoint 外接程序的模型的简介，请参阅  [SharePoint 外接程序](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)。有关详细信息，请参阅  [SharePoint 加载项与 SharePoint 解决方案比较](sharepoint-add-ins-compared-with-sharepoint-solutions.md)和 [在 SharePoint 2013 中选择正确的 API 集](choose-the-right-api-set-in-sharepoint-2013.md)。
  
    
    

## SharePoint 发布网站
<a name="ECM"> </a>

SharePoint 发布网站提供了大规模的内容发布，并实现了很高的可维护性和法规遵从性。它们还提供了对文档、记录、分类和内容类型的管理。有关详细信息，请参阅 [为 SharePoint 构建网站](build-sites-for-sharepoint.md)。
  
    
    

## SharePoint 场解决方案
<a name="Solutions"> </a>

SharePoint 服务器场解决方案是受信任的 SharePoint 扩展，其自定义逻辑将调用 SharePoint 服务器对象模型并在 SharePoint 服务器上以完全信任方式运行。这些解决方案主要用于 SharePoint 的自定义管理扩展，如计时器作业、自定义 Windows PowerShell 命令和管理中心的扩展。场解决方案作为 SharePoint 解决方案包分发，场管理员会将其上载到可部署其的场范围的存储位置。服务器场解决方案中的组件可以具有场、Web 应用程序、网站集或网站范围。有关详细信息，请参阅 [在 SharePoint 2013 中生成场解决方案](build-farm-solutions-in-sharepoint-2013.md)。
  
    
    

## SharePoint 的移动加载项
<a name="Mobile"> </a>

Windows Phone 应用程序以及在非 Microsoft 移动平台上生成的应用程序可以访问 SharePoint 网站和数据。用于生成与 SharePoint 2013 交互的 Windows Phone 应用程序的工具可在 Visual Studio 2010 和 Visual Studio 2008 的安装中使用。仅在 Windows Phone 设备上使用的 SharePoint 客户端托管 API 可用。移动设备（包括非 Microsoft 设备）还可以通过 SharePoint REST/OData 终结点访问 SharePoint 数据。有关详细信息，请参阅 [构建访问 SharePoint 2013 的 Windows Phone 应用程序](build-windows-phone-apps-that-access-sharepoint-2013.md)。
  
    
    

## SharePoint 的可重用组件
<a name="Reuse"> </a>

SharePoint 2013 平台和 Visual Studio 2008 支持封装和重用应用程序元素，包括使用代码、脚本和 XML 标记创建的元素。有关详细信息，请参阅 [为 SharePoint 2013 生成可重用组件](build-reusable-components-for-sharepoint-2013.md)。
  
    
    

## 本节内容
<a name="Reuse"> </a>


-  [为 SharePoint 构建网站](build-sites-for-sharepoint.md)
    
  
-  [在 SharePoint 2013 中生成场解决方案](build-farm-solutions-in-sharepoint-2013.md)
    
  
-  [构建访问 SharePoint 2013 的 Windows Phone 应用程序](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [为 SharePoint 2013 生成可重用组件](build-reusable-components-for-sharepoint-2013.md)
    
  

## 其他资源
<a name="SP15devinSP_addlresources"> </a>


-  [设置 SharePoint 2013 的常规开发环境](set-up-a-general-development-environment-for-sharepoint-2013.md)
    
  
-  [添加 SharePoint 2013 功能](add-sharepoint-2013-capabilities.md)
    
  
-  [SharePoint 2013 中的辅助功能](accessibility-in-sharepoint-2013.md)
    
  
