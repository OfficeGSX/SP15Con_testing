---
title: 如何：创建外部事件接收器
ms.prod: SHAREPOINT
ms.assetid: c6d5f486-6247-47f9-9876-fab12f13342f
---


# 如何：创建外部事件接收器
了解针对 Business Connectivity Services (BCS) 外部列表的本地安装创建外部事件接收器的步骤。
外部事件接收器指允许 SharePoint 外接程序对 SharePoint 项（如列表或列表项）所发生事件作出响应的类。例如，您可以对列表事件（如添加或删除字段）、列表项事件（如添加或删除列表项或列表项附件）或 Web 事件（如添加或删除网站或网站集）作出响应。您可以将远程事件接收器添加到包含 SharePoint 外接程序的现有 Visual Studio 解决方案。
  
    
    

本文包含代码示例  [SharePoint 2013：为外部数据创建远程事件接收器](http://code.msdn.microsoft.com/SharePoint-2013-Create-a-095c594c)。该示例演示了如何创建配置和使用外部系统事件通知所需的所有组件。
在该示例中，您将执行以下操作：
  
    
    


1. 创建基于 Northwind 示例数据库的外部系统
    
  
2. 创建 Windows Communication Foundation (WCF) 服务后，通过 OData 显示示例数据。
    
  
3. 创建轮询服务，该服务将监视数据中的变化，并将这些变化通知给 SharePoint 2013。
    
  
4. 创建一个外部事件接收器，该接收器在外部数据中添加了项时执行，并且由此在通知列表中创建一个新列表项。
    
  

## 必备组件和系统设置
<a name="bkmk_Prerequisites"> </a>

要完成该示例，您需要具备以下必备组件：
  
    
    

- Visual Studio 2008
    
  
- Visual Studio 2013 Office 开发人员工具
    
  
- SQL Server
    
  
- SharePoint 2013
    
  
- Internet Information Services 7.0
    
  
- Northwind 示例数据库
    
  

## 为外部系统构建组件
<a name="BuildingTheComponents"> </a>

最大的配置部分实际上出现在外部系统中。为了使外部事件接收器正确运行，外部系统必须安装并运行以下组件：
  
    
    

- **订阅存储：**包含希望在外部数据出现更改时收到通知的订阅服务器信息的表。
    
  
- **更改存储：**用于存储对数据项所作更改的表。它只会起到临时存储的功能，因为轮询服务会在通知发送回 SharePoint 中的订阅服务器时删除表中的项。
    
  
- **轮询服务：**在此方案中需要该服务作为检查数据何时发生更改以及何时提交到更改表的方法。轮询服务查询更改表，并汇编发送到订阅存储中存储的 SharePoint 传递地址（REST 端点）的通知数据包。
    
  
- **OData WCF 数据服务：**若要显示来自外部系统数据库的数据，您必须创建 WCF 服务。该服务运行于 Internet Information Services (IIS) 中，提供了本方案中需要的 RESTful 接口和 OData 源。
    
  

## 设置外部系统
<a name="bkmk_SetUpTheExternalSystem"> </a>

第一个任务是设置外部系统。
  
    
    

### 附加 Northwind 示例数据库

准备后端系统的第一部分为将 Northwind 示例数据库添加到 SQL Server 的运行实例。如果您已安装 Northwind 示例数据库，则可以在以下部分运行脚本，以创建运行外部事件通知所需的其他对象。
  
    
    
但是，如果您未安装 Northwind，请参阅 [安装 Northwind 示例数据库](http://msdn.microsoft.com/library/2f92cfc3-6310-4327-b2f2-8610f7385c86%28Office.15%29.aspx)。
  
    
    
该数据库还包含代码示例： [SharePoint 2013：为外部数据创建远程事件接收器](http://code.msdn.microsoft.com/SharePoint-2013-Create-a-095c594c)。
  
    
    

### 创建订阅存储

安装 Northwind 数据库后，打开一个新的查询窗口并执行以下脚本。
  
    
    

```

USE [Northwind]
GO
SET ANSI_NULLS ON
GO
SET QUOTED_IDENTIFIER ON
GO
CREATE TABLE [dbo].[EntitySubscribe](
    [SubscriptionId] [int] IDENTITY(1,1) NOT NULL,
    [EntityName] [nvarchar](250) NULL,
    [DeliveryURL] [nvarchar](250) NULL,
    [EventType] [int] NULL,
    [UserId] [nvarchar](50) NULL,
    [SubscribeTime] [timestamp] NULL,
    [SelectColumns] [nvarchar](10) NULL,
 CONSTRAINT [PK_Subscribe] PRIMARY KEY CLUSTERED 
(
    [SubscriptionId] ASC
)WITH (PAD_INDEX  = OFF, STATISTICS_NORECOMPUTE  = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS  = ON, ALLOW_PAGE_LOCKS  = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```

该操作会在 Northwind 数据库中创建一个表，名称为 EntitySubscribe。该表充当之前提到的订阅存储。
  
    
    

### 创建更改存储

运行以下脚本以创建更改表，该表格将记录对客户表中数据进行的更改。
  
    
    

```

USE [Northwind]
GO

SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE TABLE [dbo].[Customers_Updates](
    [CustomerID] [nchar](5) NOT NULL,
    [CompanyName] [nvarchar](40) NOT NULL,
    [ContactName] [nvarchar](30) NULL,
    [ContactTitle] [nvarchar](30) NULL,
    [Address] [nvarchar](60) NULL,
    [City] [nvarchar](15) NULL,
    [Region] [nvarchar](15) NULL,
    [PostalCode] [nvarchar](10) NULL,
    [Country] [nvarchar](15) NULL,
    [Phone] [nvarchar](24) NULL,
    [Fax] [nvarchar](24) NULL,
    [TimeAdded] [datetime] NULL,
    [EventType] [int] NULL,
 CONSTRAINT [PK_Customers_Updates] PRIMARY KEY CLUSTERED 
(
    [CustomerID] ASC
)WITH (PAD_INDEX  = OFF, STATISTICS_NORECOMPUTE  = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS  = ON, ALLOW_PAGE_LOCKS  = ON) ON [PRIMARY]
) ON [PRIMARY]

GO
```

该操作会添加一个名为 Customers_Updates 的表，其中存储了关于添加到客户表的记录的信息。
  
    
    

### 创建更改触发器

当客户表出现更改时，更改触发器将被触发。只要向客户表添加了记录，SQL Server 就会执行触发器，向包含记录信息的 Customers_Updates 表插入一条新纪录。
  
    
    
若要创建触发器，请执行以下查询。
  
    
    



```

USE [Northwind]
GO

SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE trigger [dbo].[Customer_insupd] on [dbo].[Customers] for
INSERT
AS
DECLARE 
    @CustomerID nchar(5),
    @CompanyName nvarchar(40),
    @ContactName nvarchar(30) ,
    @ContactTitle nvarchar(30) ,
    @Address nvarchar(60) ,
    @City nvarchar(15) ,
    @Region nvarchar(15) ,
    @PostalCode nvarchar(10) ,
    @Country nvarchar(15) ,
    @Phone nvarchar(24) ,
    @Fax nvarchar(24), 
    @TimeAdded datetime,
    @EventType int
    
    Select @CustomerID = CustomerId, @CompanyName=CompanyName,
    @ContactName=ContactName, @ContactTitle=ContactTitle,
    @Address=Address, @City=City, @Region=Region,
    @PostalCode =PostalCode, @Country=Country,
    @Phone=Phone,@Fax=Fax,@EventType=1 
    from inserted
    
    insert into Customers_Updates 
    (CustomerId,CompanyName,
    ContactName, ContactTitle,
    Address, City, Region,
    PostalCode,Country,
    Phone,Fax,TimeAdded,EventType) values
    (@CustomerId,@CompanyName,
    @ContactName, @ContactTitle,
    @Address, @City, @Region,
    @PostalCode,@Country,
    @Phone,@Fax,SYSDATETIME(),@EventType)
    
GO

```


> **注释**
> 如果您正在使用 BDC 模型中定义的您自己的自定义存储过程，则建议您另外创建删除触发器和更新触发器。其他触发器未涵盖在本方案中。 
  
    
    


### 创建存储过程

如果您对 Business Connectivity Services 使用直接表访问，则不需要这些过程。但是，如果您在外部系统定义自己的过程和自定义代码，则建议您添加这些过程，以允许从 SQL Server 访问表格。
  
    
    

```

// DeleteEventRecords stored procedure
USE [Northwind]
GO
SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO


CREATE PROCEDURE [dbo].[proc_DeleteEventRecords]
    @CustomerID nchar(5), @EventType int
    AS
    Delete from Customers_Updates 
    WHERE  CustomerID like @CustomerID AND EventType=@EventType

GO

```

 **SubscribeEntity** 存储过程将在订阅存储中创建一条记录。
  
    
    



```

// SubscribeEntity 
USE [Northwind]
GO

SET ANSI_NULLS ON
GO

SET QUOTED_IDENTIFIER ON
GO

CREATE PROCEDURE [dbo].[SubscribeEntity] 
    @EntityName Varchar(255),
    @EventType Integer,
    @DeliveryAddress Varchar(255),
    @SelectColumns Varchar(10)

AS
BEGIN
    -- SET NOCOUNT ON added to prevent extra result sets from
    -- interfering with SELECT statements.
    SET NOCOUNT ON;

    -- Insert statements for procedure here
    
    Insert into EntitySubscribe(EntityName,DeliveryURL,EventType,SelectColumns)
    values (@EntityName,@DeliveryAddress,@EventType,@SelectColumns)


END

GO

```


## 创建 OData 服务
<a name="bkmk_CreatingTheODataService"> </a>

OData 源托管在 **WCF 数据服务**中。然后，该 WCF 服务由 IIS 托管在一个 Web 应用程序中。
  
    
    
下面是新建 ASP.NET WCF 数据服务的步骤。
  
    
    

### 创建 WCF 数据服务应用程序


1. 在 Visual Studio 2012 中，选择"文件"菜单中的"新建"、"项目"。
    
  
2. 在"新建项目"对话框中，选择 Visual C# 节点下的"Web"模板，然后选择"ASP.NET Web 应用程序"。
    
  
3. 输入 NorthwindService 作为项目名称，然后选择"确定"。
    
  
下一步，通过使用 Visual Studio 向导发现数据源的架构，并使用该架构创建 ADO.NET 实体数据模型。
  
    
    

### 定义数据模型


1. 在"解决方案资源管理器"中，打开 ASP.NET 项目的快捷菜单，并选择"添加新项目"。
    
  
2. 在"添加新项"对话框中，选择"数据"模板，然后选择"ADO.NET 实体数据模型"。
    
  
3. 输入 Northwind.edmx 作为数据模型的名称。
    
  
4. 在实体数据模型向导中，选择"从数据库生成"，然后选择"下一步"。
    
  
5. 通过执行以下步骤之一将数据模型连接到数据库：
    
1. 如果您尚未配置数据库连接，请选择"新建连接"并创建一个新连接。有关详细信息，请参阅 [如何：创建与 SQL Server 数据库的连接](http://msdn.microsoft.com/library/360c340d-e5a6-4a7e-a569-e95d500be43d%28Office.15%29.aspx)。该 SQL Server 实例必须附加 Northwind 示例数据库。选择"下一步"。
    
    -或者-
    
  
2. 如果您已将数据库连接配置为连接到 Northwind 数据库，请从连接列表选择该连接，然后选择"下一步"
    
  
6. 在向导的最后一页，选择数据库中所有表的复选框，并清除所有视图和存储程序的复选框。
    
  
7. 选择"完成"按钮以关闭向导。
    
  
在下一步中，您会创建由 IIS 托管的实际服务，将提供通过代表性状态转移 (REST) 访问外部数据的方法。
  
    
    

### 创建 WCF 数据服务


1. 在"解决方案资源管理器"中，打开 ASP.NET 项目的快捷菜单，并选择"添加新项目"。
    
  
2. 在"添加新项目"对话框中，选择"WCF 数据服务"。
    
  
3. 输入 Northwind 作为服务名称。
    
  
4. 在数据服务代码中用于定义数据服务的类的定义中，将注释  `/* TODO: put your data source class name here */` 替换为数据模型实体容器的类型，在本示例中为 `NorthwindEntities`。类定义应如下所示。
    
  ```cs
  
public class Northwind : DataService<NorthwindEntities>

  ```


### 设置服务的安全


- 现在，您必须修改安全设置以允许外部使用者访问来自 OData 源的数据。创建 WCF 服务后，所有访问都默认被拒绝。请针对您刚才创建的类进行以下更改。
    
  ```cs
  
  config.SetEntitySetAccessRule("*", EntitySetRights.All);
  config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


### 创建订阅服务操作（可选）

WCF 服务也可以编码方式来处理 SharePoint 的订阅和取消订阅请求。如果您选择为应用程序创建自定义存储过程，服务操作会处理每个请求。
  
    
    
 **订阅**：订阅操作接收 SharePoint 发送的请求，并检索传递地址、事件类型和实体。此外，还必须生成 **subscriptionId**，然后将所有这些信息记录到数据库表中。
  
    
    



```cs

/// The Subscribe service operation maps directly to the Subscribe stereotype
/// found in the BDC model

[WebGet]
public string Subscribe(string deliveryUrl, string eventType)
{
   // Generate a new Guid that will function as the subscriptionId.
            string subscriptionId = new Guid().ToString();

            // This sproc will be used to create the subscription in the database.
            string subscribeSproc = "SubscribeEntity";

            // Create connection to database.
            using (SqlConnection conn = new SqlConnection(sqlConn))
            {
                SqlCommand cmd = new SqlCommand(subscribeSproc, conn);
                cmd.Parameters.Add(new SqlParameter("SubscriptionId", subscriptionId));
                cmd.Parameters.Add(new SqlParameter("EntityName", entityName));
                cmd.Parameters.Add(new SqlParameter("EventType", eventType));
                cmd.Parameters.Add(new SqlParameter("DeliveryAddress", deliveryUrl));
                cmd.Parameters.Add(new SqlParameter("SelectColumns", selectColumns));

                try
                {
                    conn.Open();
                    cmd.ExecuteNonQuery();
                }
                catch (Exception e)
                {
                    throw e;
                }
                finally
                {
                    conn.Close();
                }

                return subscriptionId;
            }
```


> **注释**
> 如果 SQL Server 是针对 Windows 身份验证设置的，则将尝试通过应用程序池标识对请求进行身份验证。请确保应用程序池中配置的帐户具有读取和写入数据库的权限。 
  
    
    


## 创建轮询服务
<a name="bkmk_CreateThePollingService"> </a>

轮询服务是一项 Windows 服务，负责查询更改表及创建通知并将其发送到特定传递地址的 SharePoint 2013 或 SharePoint Online。
  
    
    
下一步，新建 Windows 服务项目，该项目将在 WCF 主机中注册。注册项目后，您可以在 Microsoft 管理控制台 (MMC) 中查看运行的服务。
  
    
    

### 创建新的项目


1. 打开 Visual Studio 2008。
    
  
2. 使用 Windows 服务模板创建新项目、将项目命名为 PollingService，并选择"确定"按钮。
    
  
3. 创建项目后，在代码视图中打开 PollingService.cs 文件。
    
  
4. 将以下代码添加到新建的类中。
    
  ```cs
  
public partial class PollingService : ServiceBase
{
   string subscriptionStorePath = string.Empty;

   public PollingService()
   {
      InitializeComponent();
   }

   protected override void OnStart(string[] args)
   {

      // This is the timer which fires every minute.           
      System.Timers.Timer aTimer = new System.Timers.Timer();
      aTimer.Elapsed += new System.Timers.ElapsedEventHandler(SendEventNotification);
      aTimer.Interval = 60000;
      aTimer.Enabled = true;
   }
   protected override void OnStop()
   {}

   private void SendEventNotification(object sender, EventArgs e)
   {
      try
      {
         List<ItemChange> events = itemChangeLookUp();             
         triggerEventPerSubscription(events);
      }
      catch (Exception ex)
      {
         EventLog.Log = "Application";
         EventLog.Source = ServiceName;
         EventLog.WriteEntry("PollingService" + ex.Message, EventLogEntryType.Error);
      }
   }

   private void triggerEventPerSubscription(List<ItemChange> events)
   {           
      foreach (ItemChange itemChangeEvent in events)
      {                        
         SendNotification(itemChangeEvent, itemChangeEvent.DeliveryAddress);
         string message = string.Format("PollingService.TriggerEventPerSubscription: Notification sent for item {0} of eventType 
                          {1}", itemChangeEvent.CustomerId, itemChangeEvent.EventType);
         EventLog.Log = "Application";
         EventLog.Source = ServiceName;
         EventLog.WriteEntry(message);                   
      }
   }

   private List<ItemChange> itemChangeLookUp()
   {
      EventLog.Log = "Application";
      EventLog.Source = ServiceName;
      EventLog.WriteEntry("Polling for Item Change");
      List<ItemChange> itemChangeList = new List<ItemChange>();          
      string connectionString = "Data Source=.;Initial Catalog=Northwind;Integrated Security=true";

      // Provide the query string with a parameter placeholder.
      string queryString = "Proc_RetrieveEventRecords";

      // Specify the parameter value.
      int paramValue = -50;

      using (SqlConnection connection = new SqlConnection(connectionString))
      {
         SqlCommand command = new SqlCommand(queryString, connection);
         command.CommandType = CommandType.StoredProcedure;
         command.Parameters.AddWithValue("@TimeSince", paramValue);
         try
         {
            connection.Open();
            SqlDataReader reader = command.ExecuteReader();
            while (reader.Read())
            {
               ItemChange item = new ItemChange(reader["CustomerID"].ToString(), Int32.Parse(reader["EventType"].ToString()), 
               reader[14].ToString(), reader["DeliveryUrl"].ToString(), reader["CompanyName"].ToString(), 
               reader["ContactName"].ToString(),reader["ContactTitle"].ToString(), reader["Address"].ToString(), 
               reader["City"].ToString(), reader["Region"].ToString(), reader["Country"].ToString(), reader["PostalCode"].ToString(), 
               reader["Phone"].ToString(), reader["Fax"].ToString());
               itemChangeList.Add(item);
            }
            reader.Close();
         }
         catch (Exception ex)
         {
            EventLog.Log = "Application";
            EventLog.Source = ServiceName;
            EventLog.WriteEntry("PollingService : ItemChangeLookup " + ex.Message, EventLogEntryType.Error);
         }
      } 
      string message = string.Format("{0} items changes", itemChangeList.Count);
      EventLog.Log = "Application";
      EventLog.Source = ServiceName;
      EventLog.WriteEntry(message);

      return itemChangeList;
   }

  ```

下一步是创建可执行文件，该文件可以添加到 OData 计算机上运行的服务中。
  
    
    

### 编译和部署轮询服务


1. 按 F5 构建项目。
    
  
2. 打开"VS2012 命令提示"。
    
  
3. 在提示符中，导航至项目输出位置。
    
  
4. 在 Bin 目录中，找到 PollingService.exe 文件。
    
  
5. 输入 installutil PollingService.exe 并按 Enter。
    
  
6. 通过运行服务 MMC 验证服务是否正在运行。
    
  

## 必需的 SharePoint 组件
<a name="bkmk_SharePointComponents"> </a>

若要完成整个系统，运行 SharePoint 2013 的服务器上需要安装以下组件。
  
    
    

- **外部内容类型：**从根本上说，外部内容类型是外部数据源的 XML 定义。在此方案中，您将使用 Visual Studio 2012 中新的自动生成工具来发现数据源并自动创建外部内容类型。
    
  
- **外部事件接收器：**远程或外部事件接收器能够让外部数据更改的操作在 SharePoint 2013 中执行。您可以为外部列表和实体创建事件接收器。
    
    为外部列表创建的事件接收器类似于 SharePoint 列表的其他事件接收器。您可以创建代码并将其附加到列表。当对由列表表示的数据执行操作时，事件接收器即会执行。
    
    实体相关事件接收器的执行方式与基于列表的事件接收器类似，但它不需要附加到列表。它以相同的方式接收通知，但不需要与基于列表的示例关联的界面。这一点的优势在于，您可以编程方式截获通知并创建代码，来执行一些操作。在本方案中，执行的操作为在通知列表中创建新记录 
    
  
- **通知列表：**通知列表是简单的 SharePoint 列表，用于记录从外部系统收到的通知。对于添加到外部系统的每条新记录，通知列表中将创建一条记录。
    
  

## 设置 SharePoint 组件
<a name="bkmk_SettingUpTheSharePointComponents"> </a>

现在您已设置外部系统，那么现在应该继续创建另一半。现在您将创建托管在 SharePoint 中的组件。
  
    
    

### 新建 Visual Studio 2012 项目


1. 在 Visual Studio 2008 中，选择"新建项目"。
    
  
2. 选择 SharePoint 应用程序项目模板。
    
  
Visual Studio 2013 Office 开发人员工具添加了一个自动生成向导，该向导将发现数据源的架构，然后通过该架构创建外部内容类型。
  
    
    

### 添加新的外部内容类型


- 在"解决方案资源管理器"中，打开项目的快捷菜单并选择"添加"、"外部数据源的内容类型"。
    
    该操作将启动"SharePoint 自定义向导"，该向导用于自动构建外部内容类型。
    
    有关如何创建外部内容类型的详细信息，请参阅 [如何：在 SharePoint 2013 中从 OData 源创建外部内容类型](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md)。
    
  
此时，修改之前步骤中生成的 XML，以为订阅添加一个方法。这将允许 BCS 在有人订阅外部数据更改通知时与外部系统通信。
  
    
    

### 将 Subscribe 方法添加到外部内容类型 XML


1. 在"解决方案资源管理器"中，找到名为"外部内容类型"的新节点。
    
  
2. 找到"Customers.ect" 文件，并通过 XML 编辑器打开。
    
  
3. 向下滚动到页面底部，并将以下方法粘贴到  `<Methods>` 部分。
    
  ```XML
  
<Method Name="SubscribeCustomer" DefaultDisplayName="Customer Subscribe" IsStatic="true">
              <Properties>
                <Property Name="ODataEntityUrl" Type="System.String">/EntitySubscribes</Property>
                <Property Name="ODataHttpMethod" Type="System.String">POST</Property>
                <Property Name="ODataPayloadKind" Type="System.String">Entry</Property>
                <Property Name="ODataFormat" Type="System.String">application/atom+xml</Property>
                <Property Name="ODataServiceOperation" Type="System.Boolean">false</Property>
              </Properties>
              <AccessControlList>
                <AccessControlEntry Principal="NT Authority\\Authenticated Users">
                  <Right BdcRight="Edit" />
                  <Right BdcRight="Execute" />
                  <Right BdcRight="SetPermissions" />
                  <Right BdcRight="SelectableInClients" />
                </AccessControlEntry>
              </AccessControlList>
              <Parameters>
                <Parameter Direction="In" Name="@DeliveryURL">
                  <TypeDescriptor TypeName="System.String" Name="DeliveryURL" >
                    <Properties>
                      <Property Name="IsDeliveryAddress" Type="System.Boolean">true</Property>
                    </Properties>
                  </TypeDescriptor>
                </Parameter>
                <Parameter Direction="In" Name="@EventType">
                  <TypeDescriptor TypeName="System.Int32" Name="EventType" >
                    <Properties>
                      <Property Name="IsEventType" Type="System.Boolean">true</Property>
                    </Properties>
                  </TypeDescriptor>
                </Parameter>
                <Parameter Direction="In" Name="@EntityName">
                  <TypeDescriptor TypeName="System.String" Name="EntityName" >
                    <DefaultValues>
                      <DefaultValue MethodInstanceName="SubscribeCustomer" Type="System.String">Customers</DefaultValue>
                    </DefaultValues>
                  </TypeDescriptor>
                </Parameter>
                <Parameter Direction="In" Name="@SelectColumns">
                  <TypeDescriptor TypeName="System.String" Name="SelectColumns" >
                    <DefaultValues>
                      <DefaultValue MethodInstanceName="SubscribeCustomer" Type="System.String">*</DefaultValue>
                    </DefaultValues>
                  </TypeDescriptor>
                </Parameter>
                <Parameter Direction="Return" Name="SubscribeReturn">
                  <TypeDescriptor Name="SubscribeReturnRootTd" TypeName="Microsoft.BusinessData.Runtime.DynamicType">
                    <TypeDescriptors>
                      <TypeDescriptor Name="SubscriptionId" TypeName="System.String" >
                        <Properties>
                          <Property Name="SubscriptionIdName" Type="System.String">Default</Property>
                        </Properties>
                        <Interpretation>
                          <ConvertType LOBType="System.Int32" BDCType="System.String"/>
                        </Interpretation>
                      </TypeDescriptor>
                      <TypeDescriptor Name="DeliveryURL" TypeName="System.String" />
                      <TypeDescriptor Name="SelectColumns" TypeName="System.String" >
                      </TypeDescriptor>
                      <TypeDescriptor Name="EntityName" TypeName="System.String" />
                      <TypeDescriptor Name="EventType" TypeName="System.Int32" />
                      <TypeDescriptor Name="UserId" TypeName="System.String" />
                      <!--TypeDescriptor Name="SubscribeTime" TypeName="System." /-->
                    </TypeDescriptors>
                  </TypeDescriptor>
                </Parameter>
              </Parameters>
              <MethodInstances>
                <MethodInstance Type="EventSubscriber" ReturnParameterName="SubscribeReturn" ReturnTypeDescriptorPath="SubscribeReturnRootTd" Default="true" Name="SubscribeCustomer" DefaultDisplayName="Customer Subscribe">
                  <AccessControlList>
                    <AccessControlEntry Principal="NT Authority\\Authenticated Users">
                      <Right BdcRight="Edit" />
                      <Right BdcRight="Execute" />
                      <Right BdcRight="SetPermissions" />
                      <Right BdcRight="SelectableInClients" />
                    </AccessControlEntry>
                  </AccessControlList>
                </MethodInstance>
              </MethodInstances>
            </Method>
  ```

您现在需要添加客户端代码以允许您的列表订阅事件通知。
  
    
    

### 向 App.js 文件添加代码以启动订阅


- 在 SharePoint 外接程序项目的脚本文件夹中，打开"App.js" 文件。将以下方法粘贴到文件中。
    
  ```
  
  function SubscribeEntity()
  {
    var notificationCallback = new SP.BusinessData.Runtime.NotificationCallback(context, "http://[MACHINE NAME]:8585");
    var url = myweb.get_url();
    notificationCallback.set_notificationContext(url);
    context.load(notificationCallback);
    var subscription = entity.subscribe(1, notificationCallback, "", "SubscribeCustomer", lobSystemInstance);
    context.load(subscription);
    context.executeQueryAsync(OnSubscribeSuccess, failmethod);
  }
  ```

若要通过此脚本注册事件接收器，您必须在项目的 Default.aspx 页上创建一个按钮，并调用 **onclick()** 方法的 **SubscribeEntity()**。
  
    
    

- 打开 Default.aspx 页，然后添加以下 HTML。
    
  ```HTML
  
  <input type="button" value="Subscribe" onclick="SubscribeEntity();"/>
  ```

为了让事件运行，您还必须让 SharePoint 列表接受外部事件。该操作通过启用外部事件功能完成。
  
    
    
以下脚本将使用客户端代码启用该功能。
  
    
    



```cs
function EnableEventing_Clicked()
{
    var clientContext = SP.ClientContext.get_current();
    var web = clientContext.get_web();
    var features = web.get_features();

    clientContext.load(features);

    // The GUID provided here is the feature that allows external events and alerts.
    var eventingFeatureId = new SP.Guid('5B10D113-2D0D-43BD-A2FD-F8BC879F5ABD');

    var eventingFeature = features.add(eventingFeatureId, true, SP.FeatureDefinitionScope.farm);

    clientContext.load(eventingFeature);
    var onEventingFeatureActivated = function () {
        alert("eventing feature activated");
    };
    clientContext.executeQueryAsync(Function.createDelegate(this,
    onEventingFeatureActivated));
}
```

与针对 **Subscribe** 执行的操作类似，向页面添加一个启用功能的按钮。
  
    
    
将以下 HTML 添加到 Default.aspx 页中"订阅"按钮正下方。
  
    
    



```HTML

<input type="button" value="EnableEventing" onclick="EnableEventing_Clicked();"" />
```

接下来，在此方案中，您必须添加一个显示外部系统发送的通知的通知列表。
  
    
    

### 添加通知列表


1. 在"解决方案资源管理器"中，打开项目名称的快捷菜单并选择"添加"。
    
  
2. 选择"列表" 模板，并将列表命名为 NotificationList。
    
  
为了模仿全局程序集缓存中注册到 SharePoint 的事件接收器，该示例提供了一个将开始监听更改的控制台应用程序。当应用程序接收到通知时，将添加一个列表项到 **NotificationList**。
  
    
    

### 启动命令行事件接收器


1. 打开"RemoteEventReceiverConsoleApp"。
    
  
2. 打开"Program.cs"文件，更改本地计算机（或将运行事件接收器的计算机）的名称。
    
  
3. 单击 Visual Studio 中的"开始"按钮以运行控制台应用程序。
    
    此时命令窗口打开，表明应用程序已成功编译，并且正在监听来自外部系统的更改通知。在测试过程中，让该窗口保持打开。
    
  

### 将应用程序部署到 SharePoint


- 在 Visual Studio 打开时，按 F5 生成并部署应用程序。
    
  

## 测试应用程序
<a name="bkmk_testApp"> </a>

现在您可以看到运行中的应用程序。
  
    
    

1. 浏览应用程序以查看代表外部系统中数据的不同列表。
    
  
2. 单击"客户"列表。
    
  
3. 添加新客户。
    
  
4. 查看您刚刚创建的新列表项。
    
  
5. 转到通知列表，查看外部系统是否已向 SharePoint 通知客户表的更改。
    
    请记住，计时器设置为每隔一分钟发送一次通知。进行测试时，您可能必须稍等一会才能看到通知发布。
    
  

## 其他资源
<a name="bkmk_additionalresources"> </a>


-  [SharePoint 2013 中的外部事件和警告](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中创建外接程序范围的外部内容类型](how-to-create-an-add-in-scoped-external-content-type-in-sharepoint-2013.md)
    
  
-  [在 SharePoint 2013 中使用具有外部数据的客户端对象模型入门](get-started-using-the-client-object-model-with-external-data-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的 Business Connectivity Services](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的 Business Connectivity Services 的新增功能](what-s-new-in-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的业务连续性服务入门](get-started-with-business-connectivity-services-in-sharepoint-2013.md)
    
  

