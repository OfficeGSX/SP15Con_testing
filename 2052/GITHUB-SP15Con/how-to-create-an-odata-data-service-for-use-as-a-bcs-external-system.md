---
title: 如何：创建用作 BCS 外部系统的 OData 数据服务
ms.prod: SHAREPOINT
ms.assetid: 7d7b3aa6-85b7-400d-8ea5-50bebac56a1d
---


# 如何：创建用作 BCS 外部系统的 OData 数据服务
了解如何创建 Internet 寻址 WCF 服务，该服务在基本数据更改时使用 OData 向 SharePoint 2013 发送通知。这些通知用于触发连接到外部列表的事件。
本文介绍如何创建 ASP.NET Windows Communication Foundation (WCF) 数据服务以显示 AdventureWorks 2012 LT 示例数据库。这使您能够通过开放式数据协议 (OData)访问数据。通过 OData 建立访问之后，则可以配置 Business Connectivity Services (BCS) 外部内容类型，使 SharePoint 2013 能够使用外部数据库中的数据。为进一步增强此 OData 源，您可以向 WCF 服务添加服务合同，使 BCS可以订阅显示外部数据变化的通知。
  
    
    


## 创建的 OData 服务的先决条件
<a name="bkmk_Prerequisites"> </a>

以下是本文中创建 OData 服务所需条件：
  
    
    

- 一台承载 Internet Information Services (IIS) 的服务器
    
  
- .NET Framework 3.5 或更高版本
    
  
- SharePoint 2013
    
  
-  [AdventureWorks 20012 LT 脚本](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
-  [AdventureWorks 2012 LT 数据](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
- Visual Studio 2008
    
  
- Visual Studio 2012 Office 开发人员工具
    
  
有关设置开发环境的信息，请参阅  [设置 SharePoint 2013 的常规开发环境](set-up-a-general-development-environment-for-sharepoint-2013.md)。
  
    
    

### 创建 OData 服务的核心概念

表 1 列出了帮助您了解使用 OData 和外部内容构建 WCF 服务的核心概念的文章。
  
    
    

**表 1. 创建 OData 服务的核心概念**


|**资源**|**说明**|
|:-----|:-----|
| [将 OData 源与 SharePoint 2013 中的 Business Connectivity Services 结合使用](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md) <br/> |提供帮助您开始创建基于 OData 源并使用 SharePoint 2013 或 Office 2013 组件中的数据的外部内容类型的信息。  <br/> |
| [如何：在 SharePoint 2013 中从 OData 源创建外部内容类型](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md) <br/> |了解如何使用 Visual Studio 2008 来发现已发布的 OData 源并创建可重用的外部内容类型以在 SharePoint 2013 中的 BCS 中使用。  <br/> |
| [开放式数据协议](http://www.odata.org)（http://www.odata.org）  <br/> |提供有关开放式数据协议的信息，包括协议定义、结构信息和用法示例。  <br/> |
| [WCF 数据服务概述](http://msdn.microsoft.com/zh-cn/library/cc668794.aspx) <br/> |WCF 数据服务能够使用 OData 创建和使用 Web 和局域网上的数据服务。OData 使您能够将您的数据公开为 URI 可寻址的资源。  <br/> |
| [开发和部署 WCF 数据服务](http://msdn.microsoft.com/zh-cn/library/gg258442.aspx) <br/> |提供有关开发和部署 WCF 数据服务的信息。  <br/> |
   

### 与创建外部系统有关的步骤

需要完成以下步骤
  
    
    

- 安装 AdventureWorks 2012 LT 示例数据库
    
  
- 创建 OData 服务
    
  
- 设置访问权限
    
  
- 测试服务
    
  
- 为其他 BCS 功能添加功能
    
  

## 安装示例数据库
<a name="bkmk_Prerequisites"> </a>

外部系统通常是一个数据库，由此，此示例显示如何使用 AdventureWorks 2012 LT 示例数据库来表示一个专有数据源。
  
    
    
以下步骤将
  
    
    

## 如何创建 WCF OData 服务
<a name="bkmk_CreatingTheService"> </a>

创建可以通过 OData 协议访问的 Windows Communication Foundation (WCF) 服务相对比较简单。Visual Studio 2008 提供了几种能够从各种数据源自动发现和建模数据的工具。这使您可以创建到 SQL Server 数据库中和其他类型的数据商店中的数据的连接和接口，然后可以以编程方式使用扩展对象模型。
  
    
    
但是，当基本数据发生更改时，SharePoint 2013 启用 BCS 接收远程系统的通知，您必须修改 WCF 服务以响应这些更改。
  
    
    

### 创建新的 WCF 项目


1. 在 Visual Studio 中的"文件"菜单上，选择"新建"，"项目"。
    
  
2. 在"新建项目"对话框中，选择"Web"模板，然后选择"ASP.NET Web 应用程序"。
    
  
3. 请输入项目名称 **AdventureWorksService**，并选择"确定"。
    
  
4. 在"解决方案资源管理器"，中打开您刚刚创建的 ASP.NET 项目的快捷菜单，并选择的"属性"。
    
  
5. 请选择"Web"选项卡，并将"特定的端口"文本框的值设置为 8008。
    
  

### 定义数据模型


1. 在"解决方案资源管理器"中打开 ASP.NET 项目的快捷菜单，然后选择"添加新项"。
    
  
2. 在"添加新项目"对话框中，选择数据模板，然后选择"ADO.NET实体数据模型"。
    
  
3. 请输入数据模型的名称 **AdventureWorks.edmx**。
    
  
4. 在"实体数据模型向导"中，选择"从数据库生成"，然后选择"下一步"。
    
  
5. 通过执行下列步骤之一将数据模型连接到数据库，然后选择"下一步"。
    
  - 如果您没有已配置的数据库连接，请选择"新建连接"，并创建一个新的连接。
    
  
  - 如果您有连接到罗斯文数据库的已配置的连接，则在连接列表中选择该连接。
    
  
  - 在向导的最后一页，选择数据库中所有表的复选框，并清除所有视图和存储程序的复选框。
    
  
6. 选择"完成"关闭该向导。
    
  

### 创建数据服务


1. 在"解决方案资源管理器"中打开您的 ASP.NET 项目的快捷菜单，然后选择"添加新项目"。
    
  
2. 在"添加新项目"对话框中，选择"WCF 数据服务"。
    
  
3. 请输入服务的名称 **AdventureWorks**。
    
    Visual Studio 为新服务创建 XML 的标记和代码文件。默认情况下，代码编辑器窗口处于打开状态。在"解决方案资源管理器"中，服务将拥有名称，"AdventureWorks"的扩展名为 .svc.cs 或 .svc.vb 扩展名。
    
  
4. 将用于定义数据服务的类的定义中的注释  `/* TODO: put your data source class name here */` 替换为数据模型的实体容器的类型，在本例中是"AdventureWorksEntities" **。类定义应如下所示：**
    
  ```cs
  
public class AdventureWorks : DataService<AdventureWorksEntities>
  ```

默认情况下，在创建 WCF 服务后，由于安全配置原因，是不可访问的。下一步您将指定谁可以对其进行访问，以及他们拥有哪些权限。
  
    
    

### 启用对数据服务资源的访问


- 在数据服务代码中，用以下内容替换 **InitializeService** 函数中的占位符的代码。
    
  ```cs
  
      config.SetEntitySetAccessRule("*", EntitySetRights.All);
      config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


    这使授权的客户端对指定的资源的实体集具有读和写访问权限。
    
    > **注释**
      > 任何客户端都可以访问 ASP.NET 应用程序，也可以访问数据服务公开的资源。在生产数据服务中，为避免对资源进行未经授权的访问，您还应保护应用程序本身。有关详细信息，请参阅  [WCF 数据服务的安全](http://msdn.microsoft.com/zh-cn/library/dd728284.aspx)。 
如果 BCS 要接收通知，后端数据源必须有一种接受添加和删除订阅通知的请求的机制。
  
    
    
创建服务的最后一个步骤是为在 BDC 模型中定义的 **Subscribe**和 **Unsubscribe**构造型添加服务操作。
  
    
    

### 为订阅和取消订阅的构造型添加服务操作


- 在 AdventureWorks.cs 页面中，添加以下字符串变量声明。
    
  ```cs
  
public string subscriptionStorePath = @"\\\\[SHARE_NAME]\\SubscriptionStore\\SubscriptionStore.xml";
  ```


    > **注释**
      > 此文件是一个使用新订阅更新的 XML 文件，由服务器处理对此文件的访问，因此，请确保您被授予了访问此文件的足够的权限。 <br/> 您可能还希望创建一个用于存储订阅信息的数据库解决方案。 

    那么，将以下两个 **WebGet** 方法添加到订阅处理程序。
    


  ```cs
  [WebGet]
        public string Subscribe(string deliveryUrl, string eventType)
        {
            string subscriptionId = Guid.NewGuid().ToString();
            
            XmlDocument subscriptionStore = new XmlDocument();
            
            subscriptionStore.Load(subscriptionStorePath);

            // Add a new subscription element.
            XmlNode newSubNode = subscriptionStore.CreateElement("Subscription");

            // Add subscription ID element to the subscription element.
            XmlNode subscriptionIdStart = subscriptionStore.CreateElement("SubscriptionID");
            subscriptionIdStart.InnerText = subscriptionId;
            newSubNode.AppendChild(subscriptionIdStart);

            // Add delivery URL element to the subscription element.
            XmlNode deliveryAddressStart = subscriptionStore.CreateElement("DeliveryAddress");
            deliveryAddressStart.InnerText = deliveryUrl;
            newSubNode.AppendChild(deliveryAddressStart);

            // Add event type element to the subscription element.
            XmlNode eventTypeStart = subscriptionStore.CreateElement("EventType");
            eventTypeStart.InnerText = eventType;
            newSubNode.AppendChild(eventTypeStart);

            // Add the subscription element to the root element. 
            subscriptionStore.AppendChild(newSubNode);

            
            subscriptionStore.Save(subscriptionStorePath);

            return subscriptionId;
        }

        [WebGet]
        public void Unsubscribe(string subscriptionId)
        {
            XmlDocument subscriptionStore = new XmlDocument();
            subscriptionStore.Load(subscriptionStorePath);

            XmlNodeList subscriptions = subscriptionStore.DocumentElement.ChildNodes;
            foreach (XmlNode subscription in subscriptions)
            {
                XmlNodeList subscriptionList = subscription.ChildNodes;
                if (subscriptionList.Item(0).InnerText == subscriptionId)
                {
                    subscriptionStore.DocumentElement.RemoveChild(subscription);
                    break;
                }
            }

            subscriptionStore.Save(subscriptionStorePath);
        }

  ```


## 后续步骤
<a name="bkmk_Next"> </a>

若要将所作的更改通知 SharePoint，您还需要创建一个查询数据源更改的服务，然后向所有订阅通知的人发送通知。
  
    
    

## 其他资源
<a name="bkmk_Addresources"> </a>


-  [SharePoint 2013 中的 Business Connectivity Services](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [将 OData 源与 SharePoint 2013 中的 Business Connectivity Services 结合使用](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的外部内容类型](external-content-types-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的外部事件和警告](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中从 OData 源创建外部内容类型](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 的 Business Connectivity Services 程序员参考](business-connectivity-services-programmers-reference-for-sharepoint-2013.md)
    
  

