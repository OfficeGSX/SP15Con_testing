---
title: Excel Services 开发路线图
keywords: roadmap
f1_keywords:
- roadmap
ms.prod: OFFICE365
ms.assetid: 5c789f58-9cdb-4601-9047-9c6f83f2fbba
---


# Excel Services 开发路线图

Excel Services 的一个重要方面是解决方案开发人员可以通过编程方式利用应用程序的力量。这些应用程序可能是业务线 (LOB) 产品，也可能是组织内部开发的自定义企业级解决方案。 
  
    
    

以下是这些应用程序的示例： 
- 多层应用程序，其中表示层作为调用 Excel Web Services 的 Web 应用程序（如 ASP.NET 应用程序）实现。
    
  
- Microsoft SharePoint Server 2010 中的应用程序，或与 LOB 产品集成在一起。
    
  
使用 Excel Services 可以执行以下五种类型的开发：
- 使用 Excel Web Services 开发解决方案
    
  
- 使用用户定义函数 (UDF) 扩展 Excel Services 中的 Microsoft Excel 函数库 
    
  
- 自定义 Excel Web Access Web 部件
    
  
- 使用 ECMAScript (JavaScript, JScript) 开发解决方案
    
  
- 使用 REST API 对 Excel 工作簿执行操作
    
  

## Excel Web Service

下面是 Excel Web Services 的主要方案：
  
    
    

- **Server-side Excel calculation**
    
    此方案以应用程序为中心。在此方案中，您使用在 Excel 工作簿中定义、作为应用程序逻辑的一部分在服务器上计算的模型。
    
  
- **Automating workbook updates on the server**
    
    此方案以文件为中心。在此方案中，Excel Web Services 处理工作簿，并保存工作簿或快照的副本。
    
  
- **Opening workbooks in edit sessions**
    
    Excel Web Services 支持在 SharePoint Server 2010 中在编辑会话中打开工作簿。在此方案中，您可以使用代码来编辑工作簿。
    
  

### 服务器端 Excel 计算

对于服务器端 Excel 计算，自定义应用程序通常使用 Excel 模型作为其逻辑的一部分。业务用户无需使用编程语言重新编码 Excel 工作簿业务逻辑，而可以在服务器位置维护 Excel 模型。开发人员永远不需要更改使用业务用户创建的模型的应用程序中的任何一行代码。
  
    
    
在此方案中，自定义应用程序重复调用 Excel Web Services，后者会将调用发送到后端计算服务。Excel Calculation Services 执行以下操作：
  
    
    

- 加载指定的 Excel 工作簿 
    
  
- 接收输入
    
  
- 处理工作簿（例如，刷新数据或执行计算）
    
  
- 将结果发送到自定义应用程序
    
  

### 在服务器上自动执行工作簿更新

当开发人员将服务器上的 Excel 工作簿更新自动化时，他们通常有两个目标：
  
    
    

- 使用 Open XML 文件格式 生成 Excel 文件或修改 Excel 模板，然后计算生成的 Excel 文件。
    
  
- 定期打开 Excel 文件以刷新外部数据（每个用户一个，也可能多次），然后计算生成的工作簿并保存，或使用电子邮件发送给不同用户。
    
  
在此方案中，自定义应用程序使用 Excel Web Services 执行以下操作：
  
    
    

- 加载指定的 Excel 工作簿 
    
  
- 输入参数
    
  
- 处理工作簿（例如，刷新数据或执行计算） 
    
  
自定义应用程序使用 Excel Web Services 检索工作簿或快照的在线版本，然后保存工作簿或快照。
  
    
    

> **注释**
> 对工作簿进行更改时例如，使用 Excel Web Services 将值设置为范围对工作簿的更改将仅为该特定会话保存。更改不会保存到原始工作簿中。当前工作簿会话结束时（例如，当您调用 **CloseWorkbook** 方法时，或当会话超时时），您所做的更改将丢失。<br/> 如果您想保存对工作簿所做的更改，您可以使用 **GetWorkbook** 方法，然后使用 **SaveWorkbook** 方法或 **SaveWorkbookCopy** 方法保存工作簿。有关 Excel Web Services API 的详细信息，请参阅 [Microsoft.Office.Excel.Server.WebServices](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.WebServices.aspx) 。
  
    
    


### 使用 Excel Web Services

您可以将 Excel Web Services 作为：
  
    
    

- 常规 Web 服务，方法是通过 SOAP over HTTP 调用 Web 方法。
    
  
- 本地程序集，方法是直接链接到 **Microsoft.Office.Excel.Server.Webservices.dll**。
    
  
有关您应何时直接链接到 **Microsoft.Office.Excel.Server.Webservices.dll** 的详细信息，请参阅 [环回 SOAP 调用和直接链接](loop-back-soap-calls-and-direct-linking.md)。 
  
    
    
有关 Excel Web Services API 的信息，请参阅  [Microsoft.Office.Excel.Server.Webservices](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.Webservices.aspx) 命名空间参考文档。有关如何使用 Excel Web Services 开发自定义应用程序的示例，请参阅 [演练：使用 Excel Web Services 开发自定义应用程序](walkthrough-developing-a-custom-application-using-excel-web-services.md)。
  
    
    

## 用户定义函数 (UDF)

Excel Services 支持托管代码 UDF。Excel Services UDF 使您可以使用单元格中的公式来调用使用托管代码编写并部署到 SharePoint Server 2010 的自定义函数。您可以创建 UDF 以执行以下操作：
  
    
    

- 调用自定义数学函数。
    
  
- 从自定义数据源获取数据并插入到工作表中。
    
  
- 从 UDF 调用 Web 服务。
    
  
- 将调用打包到现有的本机代码库函数例如，现有的 Excel UDF。
    
  
有关 Excel Services UDF 的详细信息，请参阅 [了解 Excel Services UDF](understanding-excel-services-udfs.md)。 
  
    
    

### 使用 UDF

有关 Excel Services UDF 定义的信息，请参阅  [Microsoft.Office.Excel.Server.Udf](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.Udf.aspx) 命名空间参考文档。
  
    
    
有关如何创建托管代码 UDF 的示例，请参阅 [演练：开发托管代码 UDF](walkthrough-developing-a-managed-code-udf.md)。
  
    
    

## Excel Web Access

您可以使用 Excel Web Access Web 部件的可扩展属性执行以下操作：
  
    
    

- 以编程方式配置 Excel Web Access。
    
  
- 以编程方式更改 Excel Web Access 属性。
    
  
- 使用级联样式表 (CSS) 应用主题或标记 Web 部件页面。
    
  

### 使用 Excel Web Access Web 部件可扩展性

相关信息：
  
    
    

- 有关 Excel Web Access 可扩展属性，请参阅  [Microsoft.Office.Excel.Server.WebUI](https://msdn.microsoft.com/library/Microsoft.Office.Excel.Server.WebUI.aspx) 命名空间参考文档。
    
  
- 有关 Excel Web Access CSS，请参阅 CSS 参考文档。
    
  
- 有关如何以编程方式配置 Web 部件，请参阅 SharePoint Foundation SDK。 
    
  

## ECMAScript (JavaScript, JScript)

在 SharePoint Server 2010 中，Excel Services 增加了对 JavaScript 的支持。Excel Services 中的 JavaScript 对象模型使开发人员可以自动执行和自定义页面上的 Excel Web Access Web 部件控件并与其交互。通过 JavaScript 对象模型，您可以构建与页面上的一个或多个 Excel Web Access Web 部件控件交互的混合解决方案及其他集成解决方案。它还允许您向工作簿及相关代码中添加更多功能。
  
    
    
有关 Excel Services 中的 JavaScript 对象模型的详细信息，请参阅  [Ewa](http://msdn.microsoft.com/library/6fe73191-3213-b986-1ad6-2c3b918a2241%28Office.15%29.aspx) 命名空间参考文档。
  
    
    

### 使用 ECMAScript（JavaScript、JScript）

有关 JavaScript 的详细信息，请参阅以下链接：
  
    
    

- 有关 Excel Services 中的 JavaScript 对象模型的详细信息，请参阅  [Ewa](http://msdn.microsoft.com/library/6fe73191-3213-b986-1ad6-2c3b918a2241%28Office.15%29.aspx) 命名空间参考文档。
    
  
- 有关如何使用内容编辑器 Web 部件与 Excel Services 中的 JavaScript 对象模型交互的示例，请参阅 [演练：使用内容编辑器 Web 部件进行开发](walkthrough-developing-using-the-content-editor-web-part.md)。
    
  

## REST API

Excel Services 中的 REST API 是 SharePoint Server 2010 中的新增功能。通过使用 REST API，您可以直接通过 URL 访问工作簿的各个部分或元素。 
  
    
    
Excel Services REST API 的内置发现机制还使开发人员和用户可以手动或以编程方式浏览工作簿的内容，方法是提供包含特定工作簿中的元素相关信息的 Atom 馈送。您可以通过 REST API 访问的资源包括范围、图表、数据透视表和表。 
  
    
    
使用 REST API 提供的 Atom 馈送是获取所需数据一种更简单的方式。馈送包含可遍历的元素，这些元素使任何代码段均可发现工作簿中存在的元素。 
  
    
    
有关详细信息，请参阅  [Excel Services REST API](excel-services-rest-api.md)。
  
    
    

### 使用 REST API

相关信息：
  
    
    

- 有关访问 REST 服务的信息，或者要查看 Excel Services 中 REST 服务的示例 URI，请参阅 [访问 Excel Services REST API](sample-uri-for-excel-services-rest-api.md)。
    
  
- 有关访问 Excel Services 中 REST 服务的架构的信息，请参阅 [访问架构](accessing-a-schema.md)。
    
  

## 另请参阅


#### 任务


  
    
    
 [如何：以编程方式将 Excel Web Access Web 部件添加到页面](how-to-programmatically-add-an-excel-web-access-web-part-to-a-page.md)
#### 概念


  
    
    
 [Excel Services 概述](excel-services-overview.md)
  
    
    
 [Excel Services 体系结构](excel-services-architecture.md)
  
    
    
 [受支持和不受支持的功能](supported-and-unsupported-features.md)
  
    
    
 [Excel Services 博客、论坛和资源](excel-services-blogs-forums-and-resources.md)
#### 其他资源


  
    
    
 [演练：使用 Excel Web Services 开发自定义应用程序](walkthrough-developing-a-custom-application-using-excel-web-services.md)
