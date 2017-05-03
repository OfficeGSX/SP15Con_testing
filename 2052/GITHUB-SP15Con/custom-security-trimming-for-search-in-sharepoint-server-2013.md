---
title: SharePoint Server 2013 中的搜索的自定义安全修整
ms.prod: SHAREPOINT
ms.assetid: fbbf0cc4-e135-426a-9996-34eb954dbd5a
---



# SharePoint Server 2013 中的搜索的自定义安全修整
了解两种自定义安全修整程序接口 **ISecurityTrimmerPre** 和 **ISecurityTrimmerPost** 以及创建自定义安全修整程序所必须采取的步骤。
在查询时，SharePoint 2013 中的搜索功能通过使用从爬网组件中获取的安全信息，对基于提交查询的用户的标识的搜索结果执行安全修整。
  
    
    

不过，您可能具有一些如下方案：其中的内置安全修整结果不足以满足您的要求。在此类方案中，您需要实施自定义安全修整。SharePoint 2013 中的搜索功能通过  [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) 命名空间中的 [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) 接口、 [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) 接口和 [ISecurityTrimmer2](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmer2.aspx) 接口（已弃用）提供了对自定义安全修整的支持。
 **注意：** 自定义预修整程序不支持 ACL 中的 Windows SID，仅支持声明。如果有任何 SID 声明类型是从自定义预修整程序返回的，则索引中所得到的 ACE 将编码为声明，不能编码为 SID。因此它们不匹配基于 SID 的 Windows 用户标识。
  
    
    


## 实现自定义安全修整的接口
<a name="Implementing_the_interfaces"> </a>

 **ISecurityTrimmerPre** 接口执行预修整或预查询评估，在搜索查询与搜索索引匹配之前重新编写搜索查询以添加安全信息。 **ISecurityTrimmerPost** 接口执行后修整或后查询评估，在将搜索结果返回给用户之前对搜索结果进行修剪。
  
    
    
建议使用预修整以实现高性能和常规正确性；预修整可防止精简条件数据和计数实例的信息泄露。在无法使用查询筛选器准确表示安全修整的情况下，可以使用后修整程序；例如，如果需要根据发出查询的用户的本地时间（例如仅正式营业时间）筛选文档。
  
    
    

### 实现 ISecurityTrimmerPre 接口

若要为搜索结果创建自定义安全预修整程序，您必须创建实现 **ISecurityTrimmerPre** 接口的组件。
  
    
    
 **ISecurityTrimmerPre** 接口包含您必须实现的两个方法： [Initialize(NameValueCollection, SearchServiceApplication)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.Initialize.aspx) 和 [AddAccess(Boolean, Claims)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.AddAccess.aspx) 。
  
    
    

#### Initialize 方法

将安全预修整程序加载到工作进程中时，将执行 **Initialize** 方法，并且在回收工作进程之前不会再执行此方法。向此方法中传递两个参数：
  
    
    

-  _staticProperties_： [NameValueCollection](https://msdn.microsoft.com/library/System.Collections.Specialized.NameValueCollection.aspx) 对象，包含在向 Search Service 应用程序注册安全修整程序时为其指定的配置属性。
    
  
-  _searchApplication_： [SearchServiceApplication](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.aspx) 对象，表示 Search Service 应用程序。
    
  

#### AddAccess 方法

在评估每个传入查询之前，每个预修整程序为查询执行一次 **AddAccess** 方法。
  
    
    
向此方法中传递两个参数：
  
    
    

-  _sessionProperties_： **[T:System.Collections.Generic.IDictionary<String,Object>]** 对象，包含查询属性。
    
  
-  _userIdentity_： [IIdentity](https://msdn.microsoft.com/library/System.Security.Principal.IIdentity.aspx) 对象，包含用户标识。
    
  

### 实现 ISecurityTrimmerPost 接口

若要为搜索结果创建自定义安全后修整程序，您必须创建实现 **ISecurityTrimmerPost** 接口的组件。
  
    
    
 **ISecurityTrimmerPost** 接口包含您必须实现的两个方法： [Initialize(NameValueCollection, SearchServiceApplication)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.Initialize.aspx) 和 **CheckAccess(IList<String>, IList<String>, IDictionary<String, Object>, IIdentity)**。
  
    
    

#### Initialize 方法

将安全修整程序加载到工作进程中时，将执行 **Initialize** 方法，并且在回收工作进程之前不会再执行此方法。向此方法中传递两个参数：
  
    
    

-  _staticProperties_： [NameValueCollection](https://msdn.microsoft.com/library/System.Collections.Specialized.NameValueCollection.aspx) 对象，包含在向 Search Service 应用程序注册安全修整程序时为其指定的配置属性。
    
  
-  _searchApplication_： [SearchServiceApplication](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.aspx) 对象，表示 Search Service 应用程序。
    
  

#### CheckAccess 方法

在评估每个结果集查询之后，每个后修整程序为查询执行一次 **CheckAccess** 方法。
  
    
    
向此方法中传递四个参数：
  
    
    

-  _documentUrls_： [IList<T>](http://msdn2.microsoft.com/ZH-CN/library/5y536ey6) 对象，包含与爬网规则匹配的搜索结果中的每个内容项目的 URL。
    
  
-  _documentAcls_： [IList<T>](http://msdn2.microsoft.com/ZH-CN/library/5y536ey6) 对象，包含其访问权将由安全修整程序实现确定的每个内容项目的项目 ACL。
    
  
-  _sessionProperties_： [IDictionary<TKey, TValue>](http://msdn2.microsoft.com/ZH-CN/library/s4ys34ea) 对象，包含临时属性包。
    
  
-  _userIdentity_： [IIdentity](https://msdn.microsoft.com/library/System.Security.Principal.IIdentity.aspx) 对象，实现程序可从其中检索用户的标识。
    
  
 **CheckAccess** 方法返回表示 **true** 或 **false** 值的数组的 [BitArray](https://msdn.microsoft.com/library/System.Collections.BitArray.aspx) 对象，为作为此方法的第一个参数传递的 **IList** 对象中的每个内容项目 URL 返回一个。查询处理组件使用这些值来对结果执行安全后修整。如果特定内容项目的数组值为 **true**，则该项目将包括在返回的结果中；如果数组值为 **false**，则将移除该项目。
  
    
    
实现 **CheckAccess** 方法时，您可以对每个项目使用两条信息来确定为该项目返回 **true** 还是返回 **false**：提交查询的用户标识和内容项目的 URL。您也可以将自定义文档 ACL 信息从连接器传递到 **CheckAccess** 方法。
  
    
    

#### 检索安全修整程序的用户标识

您可以通过访问线程的当前主体来检索用户的标识，如下面的代码示例所示。
  
    
    

```cs

IIdentity userIdentity = System.Threading.Thread.CurrentPrincipal.Identity;
```

另外，您还必须包含以下命名空间指令。
  
    
    



```cs
using System.Security.Principal;
```

您还可从 **CheckAccess** 方法的 **passedUserIdentity** 参数检索用户标识。
  
    
    

#### 将文档 ACL 从连接器传递到您的安全修整程序

顾名思义，连接器是 SharePoint 2013 与托管外部数据的外部系统之间的通信桥梁。如果您使用的是自定义连接器，则可以通过设置 **docaclmeta** 文档属性将文档的 ACL 信息直接传递到后修整程序。只要配置的连接器和后修整程序对字段具有相同的格式和说明，您就可以免费使用它传递自定义数据。
  
    
    
如果调用了自定义安全修整程序的 **CheckAccess** 方法，那么由连接器存储在 **docaclmeta** 中的字符串将呈现在 _documentAcls_ 参数中。 **docacl** 属性中的常规文档 ACL 由基本安全修整程序进行处理，对于自定义安全修整程序来说是不可见的。同样， **docaclmeta** 属性不会对基本安全修整程序产生任何影响。
  
    
    

#### 后修整程序及其对安全修整程序的精简条件计数的影响

使用后修整程序时，一定要注意有两种类型的结果表： **RelevantResults** 和 **RefinementResults** 。后修整程序仅适用于 **RelevantResults** 中的结果命中项。因此，可能有与后修整命中项相关的精简条件，并且 **RefinementResults** 计数可能大于或等于 **RelevantResults** 。您可以采用两种方式解决此行为：
  
    
    

- 从默认 Web 部件中的精简面板中排除敏感精简条件，以便不会通过精简条件泄露任何信息。
    
  
- 在使用后修整程序时使用自定义 Web 部件来显示结果或精简条件，以便可以在 **RefinementResults** 计数超过 **RelevantResults** 计数的情况下自然地隐藏 **RefinementResults** 。
    
  

### 检索安全修整程序的单独的配置属性

可以通过使用在注册安全后修整程序时指定的属性名称来访问单独的配置属性。例如，以下代码检索名为 **CheckLimit** 的配置属性的值。
  
    
    

```cs
public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{
    if (staticProperties["CheckLimitProperty"] != null)
    {
         intCheckLimit = Convert.ToInt32(staticProperties["CheckLimitProperty"]);
    }
}
```


## 部署自定义安全修整程序组件
<a name="Deploying_the_trimmer"> </a>

创建自定义安全修整程序之后，您必须将其部署到作为查询角色的任何服务器上的全局程序集缓存。 [如何：对 SharePoint Server 搜索结果使用自定义安全修整程序](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)中的步骤 2 描述了将自定义安全修整程序部署到全局程序集缓存的过程。
  
    
    

## 注册自定义安全修整程序
<a name="Registering_the_trimmer"> </a>

对于后修整程序，您必须将自定义安全修整程序注册与特定 Search Service 应用程序和爬网规则相关联；对于预修整程序，这是可选的。
  
    
    
使用 SharePoint Management Shell 的 **SPEnterpriseSearchSecurityTrimmer** cmdlet 来注册自定义安全修整程序。
  
    
    
下表描述此 cmdlet 使用的参数。
  
    
    

**表 1. PEnterpriseSearchSecurityTrimmer cmdlet 使用的参数**


|**参数**|**描述**|
|:-----|:-----|
| _SearchApplication_ <br/> |必需。Search Service 应用程序的名称，例如"Search Service Application"。  <br/> |
| _typeName_ <br/> |必需。自定义安全修整程序程序集的强名称。  <br/> |
| _RulePath_ <br/> |对于后修整程序为必需；对于预修整程序为可选。安全修整程序的爬网规则。  <br/> <br/> **注释**<br/> 我们建议每个内容源使用一种爬网规则。           |
| _id_ <br/> |必需。安全修整程序标识符 (ID)。此值是唯一的；如果使用一个已为另一个安全修整程序注册的 ID 来注册某个安全修整程序，则将使用第二个修整程序的注册来覆盖第一个修整程序的注册。  <br/> |
| _properties_ <br/> |可选。指定配置属性的名称-值对。必须使用以下格式： `Name1~Value1~Name2~Value~…` <br/> |
   
有关注册自定义安全修整程序的基本命令的示例和指定配置属性的示例，请参阅 [如何：对 SharePoint Server 搜索结果使用自定义安全修整程序](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)。
  
    
    

## 其他资源
<a name="bk_sectrimmer_addlresources"> </a>


-  [如何：对 SharePoint Server 搜索结果使用自定义安全修整程序](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)
    
  
-  [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx)
    
  
-  [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx)
    
  
-  [SharePoint 2013 中的 Business Connectivity Services 概述](http://technet.microsoft.com/zh-cn/library/ee661740.aspx)
    
  
