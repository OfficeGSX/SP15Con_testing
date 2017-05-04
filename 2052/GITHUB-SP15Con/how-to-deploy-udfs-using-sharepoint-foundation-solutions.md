---
title: 如何：使用 SharePoint Foundation 解决方案部署 UDF
keywords: how to,howdoi,howto,udf list,WSS Solutions
f1_keywords:
- how to,howdoi,howto,udf list,WSS Solutions
ms.prod: SHAREPOINT
ms.assetid: 97751a6c-ef73-4d95-a3c4-98014d84ba48
---


# 如何：使用 SharePoint Foundation 解决方案部署 UDF

本示例说明如何使用 Microsoft SharePoint Foundation 解决方案框架部署用户定义函数 (UDF) DLL。
  
    
    

SharePoint Foundation 解决方案框架使您可以将所有组件打包，以将 SharePoint Foundation 扩展到一个名为解决方案文件的新文件（基于 CAB 的格式，扩展名为 .wsp）。解决方案是一个可部署且可重复使用的程序包，它包含一组应用于网站的功能、网站定义和程序集，并且可以单独启用或禁用。此外，您可以使用解决方案文件部署 Web 部件软件包的内容，包括程序集、类资源, .dwp 文件和其他软件包组件。有关 SharePoint Foundation 解决方案框架的详细信息，请参阅  [开始针对 SharePoint Foundation 进行开发](http://msdn.microsoft.com/library/ef1187aa-e007-4490-8191-db36a50b3ae4%28Office.15%29.aspx) (http://msdn.microsoft.com/zh-cn/library/ee539432(office.14).aspx) 中的 SharePoint Foundation 节点。
使用 SharePoint Foundation 解决方案框架创建和部署 UDF 程序集的过程如下所示：
  
    
    


1. 创建解决方案清单文件 Manifest.xml。
    
    解决方案清单（通常称为 Manifest.xml）存储在解决方案文件的根目录。该文件列出了功能、网站定义、资源文件、Web 部件文件以及要处理的程序集。它不定义文件结构；如果文件包含在解决方案中，但未在清单 XML 文件中列出，则不会进行处理。
    
    > **注释**
      > 有关清单 XML 文件的结构的详细信息，请参阅 SharePoint Foundation 文档。 
2. 将 UDF 程序集和 Manifest.xml 打包到 CAB 文件中。
    
  
3. 确保 SharePoint Foundation 管理服务在服务器上运行。
    
  
4. 使用 stsadm.exe 将解决方案添加到服务器。
    
  
5. 使用 stsadm.exe 部署解决方案。
    
  
每个 Excel Services 受信任位置都有一个 **AllowUdfs** 标记。
> **注释**
> **AllowUdfs** 标记在 Excel Services"受信任文件位置"页面上使用 **User-defined functions allowed**选项进行标记。要了解如何导航到"受信任文件位置"页面，请参阅 [步骤 3：部署和启用 UDF](step-3-deploying-and-enabling-udfs.md)。 
  
    
    

要允许从特定受信任位置调用 UDF，您必须：
- 将 **AllowUdfs** 值设置为 **true**。默认值为 **false**。 
    
  
- 将 UDF 程序集添加到受信任的 UDF 列表，以允许从工作簿中调用 UDF。
    
  
有关如何启用 UDF 以及如何将 UDF 添加到受信任 UDF 列表的详细信息，请参阅 [如何：启用 UDF](how-to-enable-udfs.md)。
> **注释**
> 为避免名称冲突，请为您的 UDF 程序集及其依赖项指定强名称，并尽可能对其进行唯一命名。有关详细信息，请参阅  [Excel Services 最佳实践](excel-services-best-practices.md)和  [Excel Services 已知问题和提示](excel-services-known-issues-and-tips.md)。 
  
    
    


## 过程


### 创建 Manifest.xml 文件


1. 在"解决方案资源管理器"中右键单击您的项目，指向"添加"，然后单击"新建项目"。
    
  
2. 选择"XML 文件"，然后将文件命名为 Manifest.xml。
    
  
3. 单击"添加"。
    
  
4. 向文件中添加以下内容：
    
  ```XML
  
<?xml version="1.0" encoding="utf-8" ?>
<Solution xmlns="http://schemas.microsoft.com/sharepoint/" SolutionId="{57568687-2CC0-45bf-B66A-2D50D57108CA}" DeploymentServerType="ApplicationServer">
  <Assemblies>
    <Assembly DeploymentTarget="GlobalAssemblyCache" Location="EcsUdfsCommonSet.dll"/>
  </Assemblies>
</Solution>
  ```


> **注释**
> 您应该为每个解决方案生成一个唯一的 GUID。有关 **Solution**元素的详细信息，请参阅SharePoint Foundation [解决方案和 Web 部件包](http://msdn.microsoft.com/library/a145a5eb-fbb6-4328-b5b3-96bf5ce89a19%28Office.15%29.aspx) (http://msdn.microsoft.com/zh-cn/library/ms413687.aspx)。

### 创建解决方案包


- 有关如何创建解决方案文件的信息，请参阅 SharePoint Foundation SDK 中"解决方案和 Web 部件软件包"节点下的"创建解决方案"主题。 
    
  

### 验证 SharePoint Foundation Administration 是否正常运行


1. 单击"开始"，指向"管理工具"，然后双击"服务"。 
    
    将显示"服务"对话框。 
    
  
2. 确保 SharePoint Foundation Administration 服务的状态显示为"已启动"，否则请右键单击 SharePoint Foundation Administration，然后选择"启动"。
    
  

### 添加解决方案


1. 依次单击"开始"、"运行"，然后键入 cmd。 
    
    将显示命令提示符控制台。
    
  
2. 运行以下脚本，将该解决方案添加到 SharePoint 服务器： 
    
    stsadm.exe -o addsolution -filename <pathtoCAB>
    
    > **注释**
      > 您可以在以下位置找到 Stsadm.exe： <br/> C:\\Program Files\\Common Files\\Microsoft Shared\\web server extensions\\12\\BIN。 

    > **注释**
      > 有关 Stsadm.exe 命令选项的详细信息，请参阅  [Stsadm 到 Windows PowerShell 的映射 (SharePoint Foundation 2010)](http://technet.microsoft.com/zh-cn/library/ff621081.aspx) (http://technet.microsoft.com/zh-cn/library/ff621081.aspx)。

  
    
    

### 部署解决方案


1. 依次单击"开始"、"运行"，然后键入 cmd。 
    
    将显示命令提示符控制台。
    
  
2. 运行以下脚本，将该解决方案部署到 SharePoint 服务器： 
    
    stsadm.exe -o deploysolution -name <filename of the CAB> -immediate -allowGacDeployment
    
    现在您应该会在全局程序集缓存中看到您的 UDF DLL。
    
  

## 另请参阅


#### 任务


  
    
    
 [如何：创建调用 Web 服务的 UDF](how-to-create-a-udf-that-calls-a-web-service.md)
  
    
    
 [如何：启用 UDF](how-to-enable-udfs.md)
  
    
    
 [如何：限制 UDF 代码访问安全权限](how-to-restrict-udf-code-access-security-permissions.md)
#### 概念


  
    
    
 [演练：开发托管代码 UDF](walkthrough-developing-a-managed-code-udf.md)
  
    
    
 [关于 Excel Services UDF 的常见问题](frequently-asked-questions-about-excel-services-udfs.md)
  
    
    
 [了解 Excel Services UDF](understanding-excel-services-udfs.md)
