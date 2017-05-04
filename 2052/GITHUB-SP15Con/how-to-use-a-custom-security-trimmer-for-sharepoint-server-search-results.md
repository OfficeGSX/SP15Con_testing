---
title: 如何：对 SharePoint Server 搜索结果使用自定义安全修整程序
ms.prod: SHAREPOINT
ms.assetid: e1a8664e-fb43-45c2-83aa-9635fe1efc99
---


# 如何：对 SharePoint Server 搜索结果使用自定义安全修整程序
本操作方法指导您完成要实现的步骤，即使用 Microsoft Visual Studio 2010 为 SharePoint 2013 中的搜索功能创建、部署和注册自定义安全修整程序。
SharePoint 2013 中的搜索功能 会对搜索结果执行查询时安全修整。但是，有些情况下您可能希望执行自定义安全修整。SharePoint 2013 中的搜索功能 通过  [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) 命名空间中的 [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) 、 [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) 和 [ISecurityTrimmer2](https://msdn.microsoft.com/library/Microsft.Office.Server.Search.Query.ISecurityTrimmer2.aspx) （已弃用）接口为这些情况提供支持。
  
    
    

自定义安全修整有两种类型：前期修整和后期修整。前期修整是指查询前评估，即在将查询与搜索索引匹配之前先重写该查询以添加安全信息。后期修整是指查询后评估，即在将搜索结果返回给用户之前先对搜索结果进行修剪。
我们建议使用前期修整提高性能和进行一般更正；使用后期修整防止精简程序数据和命中次数实例的信息泄漏。
  
    
    

利用安全修整程序注册过程，可以指定自定义安全修整程序的配置属性。
## 概述

SharePoint 2013 中的搜索功能中的自定义安全修整包含两个接口，可用于对搜索结果执行前期修整或后期修整。本操作方法重点介绍这两个接口，描述创建和注册您自己的安全修整程序所需的步骤。
  
    
    

## 先决条件
<a name="PreReq"> </a>

若要完成本操作方法，必须在开发环境中安装以下各项：
  
    
    

- Microsoft SharePoint 2013 中的搜索功能
    
  
- Microsoft Visual Studio 2010 或类似的与 Microsoft .NET Framework 兼容的开发工具
    
  

## 设置自定义安全修整程序项目
<a name="SetUp"> </a>

在此步骤中，您将创建自定义安全修整程序项目，然后添加对该项目的必需引用。
  
    
    

### 创建用于自定义安全修整程序的项目


1. 打开 Visual Studio，依次选择"文件"、"新建"、"项目"。
    
  
2. 在"项目类型"中的"C#"下，选择"SharePoint"。
    
  
3. 在"模板"下，选择"空白 SharePoint 项目"。在"名称"字段中，键入"CustomSecurityTrimmerSample"，然后选择"确定"按钮。
    
  
4. 在"SharePoint 自定义向导"中，选择"部署为场解决方案"，然后选择"完成"。
    
  

### 添加对自定义安全修整程序项目的引用


1. 在"项目"菜单上，选择"添加引用"。
    
  
2. 在".NET"选项卡上，选择具有以下组件名称的引用，然后选择"确定"按钮：
    
  - **Microsoft Search 组件**
    
    您应在带组件名称"Microsoft Search 组件"的".NET"选项卡上看到两个项。选择其中的"路径"列为 \\ISAPI\\Microsoft.Office.Server.Search.dll 的项。如果"添加引用"对话框的".NET"选项卡中缺少此项，则必须使用到 Microsoft.Office.Server.Search.dll 的路径从"浏览"选项卡添加引用。
    
  
  - **Microsoft.IdentityModel**
    
    如果"Microsoft.IdentityModel"未在".NET"选项卡上列出，则必须使用以下路径从"浏览"选项卡添加对 Microsoft.IdentityModel.dll 的引用：
    
     `%ProgramFiles%\\Reference Assemblies\\Microsoft\\Windows Identity Foundation\\v4.0.`
    
  

## 创建自定义前期安全修整程序
<a name="CreatePreTrimmer"> </a>


### 为前期安全修整程序创建类文件


1. 在"项目"菜单上，选择"添加新项"。
    
  
2. 在"已安装的模板"中的"Visual C# 项"下方，选择"代码"，然后选择"类"。
    
  
3. 键入"CustomSecurityPreTrimmer.cs"，然后选择"添加"。
    
  

### 编写自定义前期安全修整程序代码

您的自定义安全修整程序必须实现 **ISecurityTrimmerPre** 接口。以下代码示例是此接口的基本实现。
  
    
    

### 修改 CustomSecurityPreTrimmer 中的默认代码


1. 在类的开头添加以下 **using** 指令。
    
  ```cs
  
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Security.Principal;
using Microsoft.IdentityModel.Claims;
using Microsoft.Office.Server.Search.Query;
using Microsoft.Office.Server.Search.Administration;

  ```

2. 在类声明中指定 **CustomSecurityPreTrimmer** 类实现 [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) 接口，如以下代码所示。
    
  ```cs
  
public class CustomSecurityPreTrimmer : ISecurityTrimmerPre
  ```


### 实现 ISecurityTrimmerPre 接口方法


1. 为  [Initialize()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.Initialize.aspx) 方法声明添加下面的代码。
    
  ```cs
  public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{

}
  ```


该示例的基本版本未在 **Initialize** 方法中包含任何代码。
    
  
2. 为  [AddAccess()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.AddAccess.aspx) 方法声明添加下面的代码。
    
  ```cs
  
public IEnumerable<Tuple<Claim, bool>> AddAccess(
                IDictionary<string, object> sessionProperties, IIdentity passedUserIdentity)
{
//AddAccess method implementation, see steps 3-5.
}
  ```

3. 在 **AddAccess** 方法实现的第一部分中，我们通过查看 _passedUserIdentity_ 查明用户身份。
    
  ```cs
  
if (passedUserIdentity == null)
{
   throw new ArgumentException("AddAccess method is called with invalid user identity
   parameter", "passedUserIdentity");
}
            
   String strUser = null;
   var claimsIdentity = (IClaimsIdentity)passedUserIdentity;
   if (claimsIdentity != null)
   {
      foreach (var claim in claimsIdentity.Claims)
      {
         if (claim == null) 
         {
            continue;
         }
         
         // strUser is "domain\\\\user" format when web app is in Claims Windows Mode
         if (SPClaimTypes.Equals(claim.ClaimType, SPClaimTypes.UserLogonName))
         {
            strUser = claim.Value;
            break;
         }

         // strUser2 is "S-1-5-21-2127521184-1604012920-1887927527-66602" when web app is   
            in Legacy Windows Mode
         // In this case we need to convert it into NT user format.
         if (SPClaimTypes.Equals(claim.ClaimType, ClaimTypes.PrimarySid))
         {
            strUser = claim.Value;
            SecurityIdentifier sid = new SecurityIdentifier(strUser);
            strUser = sid.Translate(typeof(NTAccount)).Value;
            break;
         }
      }
}            

  ```

4. 创建列表，使用声明填充该列表并返回该列表，如以下代码所示。
    
  ```cs
  
var claims = new LinkedList<Tuple<Claim, bool>>();
if (!string.IsNullOrEmpty(strUser))
   {
      var groupMembership = GetMembership(strUser);
      if (!string.IsNullOrEmpty(groupMembership))
      {
         var groups = groupMembership.Split(new[] {';'},
         StringSplitOptions.RemoveEmptyEntries);
         foreach (var group in groups)
         {
            claims.AddLast(new Tuple<Claim, bool>(
            new Claim("http://schemas.happy.bdc.microsoft.com/claims/acl", group), false));
         }
      }
   }
   return claims;
}

  ```


**GetMembership** 方法包含修整程序的自定义逻辑。
    
  

## 创建自定义后期安全修整程序
<a name="CreatePostTrimmer"> </a>


### 为后期安全修整程序创建类文件


1. 在"项目"菜单上，选择"添加新项"。
    
  
2. 在"已安装的模板"中的"Visual C# 项"下方，选择"代码"，然后选择"类"。
    
  
3. 键入"CustomSecurityPostTrimmer.cs"，然后选择"添加"。
    
  

### 编写自定义后期安全修整程序代码

您的自定义安全修整程序必须实现 **ISecurityTrimmerPost** 接口。本节中的代码示例是此接口的基本实现。
  
    
    

### 修改 CustomSecurityPostTrimmer 中的默认代码


1. 在类的开头添加以下 **using** 指令：
    
  ```cs
  
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Security.Principal;
using Microsoft.IdentityModel.Claims;
using Microsoft.Office.Server.Search.Query;
using Microsoft.Office.Server.Search.Administration;

  ```

2. 在类声明中指定 **CustomSecurityPostTrimmer** 类实现 [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) 接口，如下所示：
    
  ```cs
  
public class CustomSecurityPostTrimmer : ISecurityTrimmerPost
  ```


### 实现 ISecurityTrimmerPost 接口方法


1. 为  [Initialize()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.Initialize.aspx) 方法声明添加下面的代码。
    
  ```cs
  public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{

}
  ```


该示例的基本版本未在 Initialize 方法中包含任何代码。
    
  
2. 为  [CheckAccess()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.CheckAccess.aspx) 方法声明添加下面的代码。
    
  ```cs
  
public BitArray CheckAccess(IList<string> documentCrawlUrls, IList<string> documentAcls, IDictionary<string, object> sessionProperties, IIdentity passedUserIdentity)
{
//CheckAccess method implementation, see steps 3-5.
}
  ```

3. 在 **CheckAccess** 方法实现的第一部分中，声明并初始化 **BitArray** 变量以便在 **documentCrawlUrls** 集合中存储每个 URL 的访问检查结果，并检索提交该查询的用户，如以下代码所示。
    
  ```cs
  
if (documentCrawlUrls == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid URL list",
   "documentCrawlUrls");
}
if (documentAcls == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid documentAcls   
   list", "documentAcls");
}
if (passedUserIdentity == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid user identity
   parameter", "passedUserIdentity");
}

  ```

4. 循环集合中的每个爬网 URL 并执行访问检查，以确定提交查询的用户是否可以访问爬网 URL 的相关内容项，如以下代码所示。 
    
  ```cs
  
// Initialize the bit array with TRUE value which means all results are visible by default.
var urlStatusArray = new BitArray(documentCrawlUrls.Count, true);
var claimsIdentity = (IClaimsIdentity)passedUserIdentity;
if (claimsIdentity != null)
{
   var userGroups = GetGroupList(claimsIdentity.Claims);
   var numberDocs = documentCrawlUrls.Count;
   for (var i = 0; i < numberDocs; ++i)
   {
      if (!string.IsNullOrEmpty(documentAcls[i]))
      {
         urlStatusArray[i] = VerifyAccess(documentAcls[i], userGroups);
      }
   }
}

  ```


如果用户具有访问该内容项的权限，请将位于该索引 ( **urlStatusArray[i]**) 的 **BitArray** 项的值设为 **true**；否则，将其设为 **false**。
    
  
5. 将 **CheckAccess** 方法的返回值设为 **urlStatusArray**，如以下代码所示。
    
  ```cs
  
return urlStatusArray;
  ```


## 注册自定义安全修整程序
<a name="RegisterTrimmer"> </a>

此步骤介绍如何配置自定义安全修整程序，并包括以下任务：
  
    
    

- 使用 Windows PowerShell cmdlet 为搜索服务应用程序注册自定义安全修整程序。
    
  
- 注册 SharePoint 搜索主机控制器 (SPSearchHostController)。
    
  

### 注册自定义安全修整程序

使用 SharePoint Management Shell 注册具有  _ClassName_ 的自定义安全修整程序。在我们的示例中， _ClassName_ 可以是 **CustomSecurityPreTrimmer** 或 **CustomSecurityPostTrimmer**。以下过程将演示如何注册自定义安全修整程序，并将搜索服务应用程序的 ID 设置为 1。
  
    
    

### 注册自定义安全修整程序


1. 在 Windows 资源管理器中找到  _<Local_Drive>_:\\WINDOWS\\assembly 路径中的 CustomSecurityTrimmerSample.dll。
    
  
2. 打开文件的快捷菜单，然后选择"属性"。
    
  
3. 在"属性"对话框中的"常规"选项卡上，选择并复制标记。
    
  
4. 打开 SharePoint Management Shell。有关使用此工具的信息，请参阅 [使用 SharePoint 2010 Management Shell 管理服务应用程序](http://msdn.microsoft.com/library/aff64855-7377-4e4a-b3a9-b620c9047076%28Office.15%29.aspx)
    
  
5. 在命令提示符处，键入以下命令。
    
  ```
  New-SPEnterpriseSearchSecurityTrimmer -SearchApplication "Search Service Application"
-typeName "CustomSecurityTrimmerSample.ClassName, CustomSecurityTrimmerSample, 
Version=1.0.0.0, Culture=neutral, PublicKeyToken=token" -RulePath "xmldoc://*"
  ```


在该命令中，将  _ClassName_ 替换为 **CustomSecurityPreTrimmer** 或 **CustomSecurityPostTrimmer**；将  _token_ 替换为 CustomSecurityTrimmerSample.dll 文件的公钥标记。您必须将所有后修整程序与 _"xmldoc://*"_ 爬网规则关联，但此操作是预修整程序的可选操作。
    
> **注释**
> 如果您有多个前端 Web 服务器，则必须将安全修整程序部署到服务器场中所有前端 Web 服务器上的全局程序集缓存。 
6. 确认您的安全修整程序是使用以下 PowerShell cmdlet 注册的。
    
  ```
  
$searchApp = Get-SPEnterpriseSearchServiceApplication
$searchApp | Get-SPEnterpriseSearchSecurityTrimmer
  ```


安全修整程序在结果中必须是可见的。
    
  

### 重启 SharePoint 搜索主机控制器


- 您可以键入下列 PowerShell cmdlet 来重启搜索服务
    
  ```
  
  net restart sphostcontrollerservice

  ```


## 其他资源
<a name="bk_sectrimmer_addlresources"> </a>


-  [SharePoint Server 2013 中的搜索的自定义安全修整](custom-security-trimming-for-search-in-sharepoint-server-2013.md)
    
  
-  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx)
    
  
-  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx)
    
  
-  [Microsoft.Office.Server.Search.Query.PluggableAccessCheckException](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.PluggableAccessCheckException.aspx)
    
  

