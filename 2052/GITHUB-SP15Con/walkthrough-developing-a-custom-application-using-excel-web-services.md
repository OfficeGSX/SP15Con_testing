---
title: 演练：使用 Excel Web Services 开发自定义应用程序
ms.prod: OFFICE365
ms.assetid: 2f9bf243-281a-4d70-917e-9eaf0b867631
---


# 演练：使用 Excel Web Services 开发自定义应用程序

本节中的演练说明从使用 Microsoft Visual C# 创建的应用程序访问 Excel Web Services 的过程。
  
    
    

在本演练中，您将学习如何：
- 使用 Visual Studio 控制台应用程序项目模板创建客户端应用程序。
    
  
- 添加对 Excel Web Services 的 Web 引用。
    
  
- 编写访问 Web 服务的代码。您将了解如何打开工作簿、获取会话 ID、传递默认凭据、获取 Web 服务版本信息、定义范围协调对象、获取使用范围协调对象的范围、关闭工作簿以及捕获 SOAP 异常。
    
  
- 在调试模式下测试和运行控制台应用程序。
    
  
客户端控制台应用程序只是访问 Web 服务的一种方式。更常见的方式是使用服务器应用程序，例如 Microsoft ASP.NET 应用程序。为简单起见，本演练使用控制台应用程序，重点关注 Excel Web Services API 方面。
## 先决条件

要完成本演练，您将需要： 
  
    
    

- Microsoft SharePoint Server 2010。
    
  
- Visual Studio 或与 Microsoft .NET Framework 兼容的相似开发工具。
    
  
- 能够访问 SharePoint Server 2010 所在的计算机上的 Excel Web Services 的足够权限（至少为"查看"权限）。 
    
    > **注释**
    > 有关工作簿权限的详细信息，请参阅下一节"工作簿权限"。 
- 安装在本地驱动器或本地 SharePoint 文档库中的示例工作簿。 
    
  
- 存储您想使用 Excel Web Services 访问的工作簿的受信任位置。如果工作簿未存储在受信任位置，打开工作簿的 Excel Web Services 调用将失败。本演练假定工作簿存在于本地计算机上。 
    
    > **注释**
    > 有关如何信任某个位置的信息，请参阅 [如何：信任位置](how-to-trust-a-location.md)和 [如何：使用脚本信任工作簿位置](http://msdn.microsoft.com/library/79ab6ced-7a0c-4275-b852-bb246fc6be57%28Office.15%29.aspx)。 
- 使用 Excel 创建工作簿。
    
  
- 将工作簿另存为 .xlsx 或 .xlsb 文件。
    
  
本示例中使用的工作簿具有一个名为"Sheet1"的工作表。该工作表中有 11 列和 19 行。从 A1 到 K19 每个单元格均包含一个数值例如，4245.955、6960.673，等等。
  
    
    

## 工作簿权限


- 要获取整个工作簿（例如，通过调用 **GetWorkbook** 方法），调用者需要具备工作簿的"打开"权限。
    
  
- 调用 **GetApiVersion** 方法无需权限。
    
  
- 对于其余 Excel Web Services 方法，调用者需要具备工作簿的"查看"权限（在 Microsoft SharePoint Foundation 中）或"读取"权限（在文件共享中）。
    
    > **注释**
    > 有关设置权限的详细信息，请参阅 SharePoint Foundation 文档。 

## 另请参阅


#### 概念


  
    
    
 [访问 SOAP API](accessing-the-soap-api.md)
  
    
    
 [Excel Services 警告](excel-services-alerts.md)
  
    
    
 [Excel Services 已知问题和提示](excel-services-known-issues-and-tips.md)
  
    
    
 [环回 SOAP 调用和直接链接](loop-back-soap-calls-and-direct-linking.md)
#### 其他资源


  
    
    
 [步骤 1：创建 Web 服务客户端项目](step-1-creating-the-web-service-client-project.md)
  
    
    
 [步骤 2：添加 Web 引用](step-2-adding-a-web-reference.md)
  
    
    
 [步骤 3：访问 Web 服务](step-3-accessing-the-web-service.md)
  
    
    
 [步骤 4：构建和测试应用程序](step-4-building-and-testing-the-application.md)
