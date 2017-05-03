---
title: [方法] 外部イベント レシーバーの作成
ms.prod: SHAREPOINT
ms.assetid: c6d5f486-6247-47f9-9876-fab12f13342f
---


# [方法] 外部イベント レシーバーの作成
Business Connectivity Services (BCS) 外部リストの社内インストール用に外部イベント レシーバーを作成する手順について説明します。
外部イベント レシーバーは、リストやリスト アイテムなどの SharePoint アイテムに対して発生するイベントに応答するために SharePoint アドインを有効にするクラスです。たとえば、フィールドの追加や削除などを行うリスト イベント、リスト アイテムや添付ファイルの追加や削除などを行うリスト アイテム イベント、またはサイトやサイト コレクションの追加や削除などを行う Web イベントに応答できます。リモート イベント レシーバーは、SharePoint アドインを含む任意の既存の Visual Studio ソリューションに追加できます。
  
    
    

この記事には、「 [SharePoint 2013: 外部データのリモート イベント レシーバーを作成する](http://code.msdn.microsoft.com/SharePoint-2013-Create-a-095c594c)」のサンプル コードが添付されています。このコードは、外部システム イベント通知を構成して使用するために必要なコンポーネントの作成方法が示されています。
この例では、次の操作を実行します。
  
    
    


1. Northwind サンプル データベースに基づいた外部システムの作成
    
  
2. Windows Communication Foundation (WCF) サービスの作成による、OData を介したサンプル データの公開
    
  
3. データの変更を監視するポーリング サービスの作成と、SharePoint 2013 への変更の通知
    
  
4. アイテムが外部データに追加され、その結果、通知リストに新しいリスト アイテムが作成された場合に実行される、外部イベント レシーバーの作成
    
  

## 前提条件とシステム セットアップ
<a name="bkmk_Prerequisites"> </a>

この例を実行するには、次の前提条件が必要です。
  
    
    

- Visual Studio 2012
    
  
- Office Developer Tools for Visual Studio 2013
    
  
- SQL Server
    
  
- SharePoint 2013
    
  
- Internet Information Services 7.0
    
  
- Northwind サンプル データベース
    
  

## 外部システム用のコンポーネントの作成
<a name="BuildingTheComponents"> </a>

設定の多くの部分は、実際には外部システム上で実行されます。外部イベント レシーバーが正しく動作するには、外部システムで次のコンポーネントが存在し、動作している必要があります。
  
    
    

- **サブスクリプション ストア:**外部システムへの変更の通知を受信するサブスクライバーに関する情報が含まれているテーブル。
    
  
- **変更ストア:** データ アイテムへの変更の保存に使用されるテーブル。通知が SharePoint のサブスクライバーに送り返されると、ポーリング サービスによってテーブル内のアイテムが削除されるため、一時的なストレージとしてのみ機能します。
    
  
- **ポーリング サービス:** いつデータが変更されたか、いつ変更テーブルに送信されたかをチェックする方法として、このシナリオで必要です。ポーリング サービスは変更テーブルをクエリし、サブスクリプション ストアに保存されている SharePoint 配信アドレス (REST エンドポイント) に送信される通知パッケージを作成します。
    
  
- **OData WCF データ サービス:** 外部システムのデータベースからのデータを公開するには、WCF サービスを作成する必要があります。Internet Information Services (IIS) で実行されているこのサービスは、REST 対応のインターフェイスと、このシナリオに必要な OData フィードを提供します。
    
  

## 外部システムのセットアップ
<a name="bkmk_SetUpTheExternalSystem"> </a>

最初のタスクは、外部システムのセットアップです。
  
    
    

### Northwind サンプル データベースの接続

バックエンド システムを準備するための最初の作業は、Northwind サンプル データベースを SQL Server の実行中インスタンスに追加することです。すでに Northwind サンプル データベースがインストールされている場合は、次のセクションのスクリプトを実行して、外部イベント通知が動作するために必要な追加オブジェクトを作成できます。
  
    
    
ただし、Northwind をインストールしていない場合は、「 [Northwind サンプル データベースのインストール](http://msdn.microsoft.com/library/2f92cfc3-6310-4327-b2f2-8610f7385c86%28Office.15%29.aspx)」を参照してください。
  
    
    
データベースには、「 [SharePoint 2013: 外部データのリモート イベント レシーバーを作成する](http://code.msdn.microsoft.com/SharePoint-2013-Create-a-095c594c)」のサンプル コードも含まれています。
  
    
    

### サブスクリプション ストアの作成

Northwind データベースをインストールしたら、新しいクエリ ウィンドウを開いて、次のスクリプトを実行します。
  
    
    

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

これにより、EntitySubscribe という名前のテーブルが Northwind データベースに作成されます。このテーブルは前述したサブスクリプション ストアとして機能します。
  
    
    

### 変更ストアの作成

次のスクリプトを実行して、顧客テーブルのデータに加えられた変更を記録する変更テーブルを作成します。
  
    
    

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

これにより、顧客テーブルに追加されたレコードに関する情報を保存する、Customers_Updates という名前のテーブルが追加されます。
  
    
    

### 変更トリガーの作成

顧客テーブルが変更されると、変更トリガーが起動します。レコードが顧客テーブルに追加されると、SQL Server は必ずトリガーを起動します。これにより、Customers_Updates テーブルに、レコードに関する情報を持つ新しいレコードが挿入されます。
  
    
    
トリガーを作成するには、次のクエリを実行します。
  
    
    



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


> **メモ**
> BDC モデルで定義されている、独自のカスタム ストアド プロシージャを使用している場合、削除トリガーと更新トリガーも作成することをお勧めします。追加のトリガーについては、このシナリオ内では説明しません。 
  
    
    


### ストアド プロシージャの作成

Business Connectivity Service を使って、直接テーブル アクセスを使用している場合、以下の手順は不要です。ただし、外部システム上で独自のプロシージャとカスタム コードを定義している場合、以下の手順を追加して、SQL Server からテーブルにアクセスできるようにすることをお勧めします。
  
    
    

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

 **SubscribeEntity** ストアド プロシージャは、サブスクリプション ストアにレコードを作成します。
  
    
    



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


## OData サービスの作成
<a name="bkmk_CreatingTheODataService"> </a>

OData フィードは、 **WCF データ サービス** 内でホストされます。この WCF サービスは、Web アプリケーションで IIS によってホストされます。
  
    
    
以下の手順では、新しい ASP.NET WCF データ サービスを作成します。
  
    
    

### WCF データ サービス アプリケーションを作成するには


1. Visual Studio 2012 で、[ **ファイル**]、[ **新規作成**]、[ **プロジェクト**] の順に選択します。
    
  
2. [ **新しいプロジェクト**] ダイアログ ボックスの Visual C# ノードで [ **Web**] テンプレート、[ **ASP.NET Web アプリケーション**] の順に選択します。
    
  
3. プロジェクトの名前として「NorthwindService」と入力し、[ **OK**] を選択します。
    
  
次に Visual Studio ウィザードを使用して、データ ソースのスキーマを検出し、それを使って ADO.NET エンティティ データ モデルを作成します。
  
    
    

### データ モデルを定義するには


1. [ **ソリューション エクスプローラー**] で ASP.NET プロジェクトのショートカット メニューを開き、[ **新しいアイテムの追加**] を選択します。
    
  
2. [ **新しいアイテムの追加**] ダイアログ ボックスで [ **データ**] テンプレートを選択し、[ **ADO.NET エンティティ データ モデル**] を選択します。
    
  
3. データ モデルの名前には、「Northwind.edmx」と入力します。
    
  
4. エンティティ データ モデル ウィザードで、[ **データベースから生成**] を選択し、[ **次へ**] を選択します。
    
  
5. 次のいずれかのステップを実行して、データ モデルをデータベースに接続します。
    
1. まだデータベース接続を設定していない場合、[ **新しい接続**] を選択して、新しい接続を作成します。詳細については、「 [SQL Server データベースへの接続を作成する方法](http://msdn.microsoft.com/library/360c340d-e5a6-4a7e-a569-e95d500be43d%28Office.15%29.aspx)」を参照してください。この SQL Server インスタンスには、Northwind サンプル データベースが接続されている必要があります。[ **次へ**] を選択します。
    
    または
    
  
2. Northwind データベースに接続するようにデータベース接続が構成済みの場合、接続リストからその接続を選択し、[ **次へ**] を選択します。
    
  
6. ウィザードの最後のページで、データベースのすべてのテーブルに対してチェック ボックスをオンに、ビューとストアド プロシージャに対するチェック ボックスをオフにします。
    
  
7. [ **完了**] ボタンをクリックして、ウィザードを終了します。
    
  
次のステップでは、IIS がホストする実際のサービスを作成します。このサービスは、Representational State Transfer (REST) を介して、外部データにアクセスする方法を提供します。
  
    
    

### WCF データ サービスを作成するには


1. [ **ソリューション エクスプローラー**] で ASP.NET プロジェクトのショートカット メニューを開き、[ **新しいアイテムの追加**] を選択します。
    
  
2. [ **新しいアイテムの追加**] ダイアログ ボックスで [ **WCF データ サービス**] を選択します。
    
  
3. サービスの名前には、「Northwind」を入力します。
    
  
4. データ サービスのコードでは、データ サービスを定義するクラスの定義で、コマンド  `/* TODO: put your data source class name here */` をデータ モデルのエンティティ コンテナーであるタイプに置き換えます。この場合、これは `NorthwindEntities` になります。クラス定義は次のようになります。
    
  ```cs
  
public class Northwind : DataService<NorthwindEntities>

  ```


### サービスにセキュリティを設定するには


- 外部の消費者が OData フィードからのデータにアクセスできるように、セキュリティを変更する必要があります。WCF サービスを作成すると、既定ではすべてのアクセスが拒否されます。先ほど作成したクラスを次のように変更します。
    
  ```cs
  
config.SetEntitySetAccessRule("*", EntitySetRights.All);
config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


### サブスクライブ サービス操作の作成 (オプション)

SharePoint からのサブスクライブおよびサブスクライブ取り消し要求を処理する方法を使って、WCF サービスをコーディングすることができます。アプリケーション用にカスタムのストアド プロシージャを作成する場合、各ストアド プロシージャはサービス操作によって処理されます。
  
    
    
 **サブスクライブ**: サブスクライブ操作は SharePoint によって送信される要求を受け取り、各タイプとエンティティの配信アドレスを取得します。また、 **subscriptionId** を生成し、これらのすべてをデータベース テーブルに記録します。
  
    
    



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


> **メモ**
> SQL Server を Windows 認証用にセットアップすると、アプリ プール ID を持つ要求を認証しようとします。アプリ プール内で構成されたアカウントに、データベースの読み取りおよび書き込み権限があることを確認してください。 
  
    
    


## ポーリング サービスの作成
<a name="bkmk_CreateThePollingService"> </a>

ポーリング サービスは、変更テーブルをクエリして通知を作成し、特定の配信アドレスの SharePoint 2013 または SharePoint Online にその通知を送信する Windows サービスです。
  
    
    
次に、新しい Windows サービス プロジェクトを作成し、WCF ホスト マシンに登録します。プロジェクトを登録した後、Microsoft Management Console (MMC) の実行中サービスを確認できます。
  
    
    

### 新規プロジェクトを作成するには


1. Visual Studio 2012 を開きます。
    
  
2. Windows サービス テンプレートを使って新規プロジェクトを作成し、プロジェクトに「PollingService」という名前を付けて、[ **OK**] ボタンをクリックします。
    
  
3. プロジェクトを作成したら、コード ビューで PollingService.cs ファイルを開きます。
    
  
4. 以下のコードを新しく作成したクラスに追加します。
    
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

次の手順は、OData コンピューターの実行中サービスに追加できる実行可能ファイルを作成することです。
  
    
    

### ポーリング サービスのコンパイルと展開を行うには


1. F5 を押して、プロジェクトをビルドします。
    
  
2. [ **VS2012 のコマンド プロンプト**] を開きます。
    
  
3. プロンプトで、プロジェクトを出力する場所に移動します。
    
  
4. Bin ディレクトリーで、PollingService.exe ファイルを探します。
    
  
5. 「installutil PollingService.exe」と入力し、Enter キーを押します。
    
  
6. Services MMC を実行して、サービスが実行中であることを確認します。
    
  

## 必要な SharePoint コンポーネント
<a name="bkmk_SharePointComponents"> </a>

システム全体を完成するには、SharePoint 2013 を実行しているサーバー上に、次のコンポーネントが必要です。
  
    
    

- **外部コンテンツ タイプ:** 外部コンテンツ タイプは基本的に、外部データ ソースの XML 定義です。このシナリオの場合、Visual Studio 2012 の新しい自動生成ツールを使用して、データ ソースを検出し、外部コンテンツ タイプを自動生成します。
    
  
- **外部イベント レシーバー:** リモートまたは外部イベント レシーバーは、外部データの変更に対する操作を、SharePoint 2013 で実行できるようにします。外部リストとエンティティに対して、イベント レシーバーを作成できます。
    
    外部リスト用に作成されたイベント レシーバーは、SharePoint リスト用の他のイベント レシーバーと似ています。コードを作成し、それをリストに接続します。リストで表示されているデータに対して操作を実行すると、イベント レシーバーが実行されます。
    
    エンティティ用のイベント レシーバーは、リストベースのイベント レシーバーと同じように実行されますが、リストに接続する必要がないという点が異なります。このレシーバーは同じように通知を受信しますが、リストベースの例に関連付けられているインタフェースは必要ありません。このレシーバーのメリットは、通知をプログラムで受信し、特定のアクションを実行するコードを作成することができるということです。このシナリオでは、通知リストでの新規レコードの作成がそのアクションに相当します。 
    
  
- **通知リスト:** 通知リストはシンプルな SharePoint リストで、外部システムから受信した通知を記録するために使われます。外部システムに新しいレコードが追加されるたびに、通知リストでレコードが作成されます。
    
  

## SharePoint コンポーネントのセットアップ
<a name="bkmk_SettingUpTheSharePointComponents"> </a>

これで外部システムのセットアップが終了しました。残り半分の設定に移ります。ここでは SharePoint でホストするコンポーネントを作成します。
  
    
    

### 新しい Visual Studio 2012 プロジェクトを作成するには


1. Visual Studio 2012 で [ **新しいプロジェクト**] を選択します。
    
  
2. SharePoint アプリ プロジェクトのテンプレートを選択します。
    
  
Office Developer Tools for Visual Studio 2013 が自動生成ウィザードを追加しました。このウィザードはデータ ソースのスキーマを検出し、そこから外部コンテンツ タイプを作成します。
  
    
    

### 新しい外部コンテンツ タイプを追加するには


- [ **ソリューション エクスプローラー**] でプロジェクトのショートカット メニューを開き、[ **追加**]、[ **外部データ ソース用のコンテンツ タイプ**] の順に選択します。
    
    これにより、 **SharePoint カスタマイズ ウィザード** が起動します。このウィザードは外部コンテンツ タイプの自動作成に使われます。
    
    外部コンテンツ タイプの作成方法の詳細については、「 [[方法] SharePoint 2013 で OData ソースから外部コンテンツ タイプを作成する](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md)」を参照してください。
    
  
前の手順 Y で作成した XML を変更して、サブスクライブのメソッドを追加します。これにより、外部データの変更について通知されるようにユーザーがサブスクライブするときに、BCS が外部システムと通信できるようになります。
  
    
    

### 外部コンテンツ タイプ XML にサブスクライブのメソッドを追加するには


1. [ **ソリューション エクスプローラー**] で [ **外部コンテンツ タイプ**] という新規ノードを検索します。
    
  
2. **Customers.ect** ファイルを見つけ、それを XML エディターで開きます。
    
  
3. ページの下までスクロールして、次のメソッドを  `<Methods>` セクションに貼り付けます。
    
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

新しいクライアント コードを追加すると、リストでイベント通知をサブスクライブできるようになります。
  
    
    

### コードを App.js ファイルに追加して、サブスクリプションを開始するには


- SharePoint アドイン プロジェクトの [スクリプト] フォルダーで、 **App.js** ファイルを開きます。以下のメソッドをファイルに貼り付けます。
    
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

このスクリプトを使用してイベント レシーバーを登録するには、プロジェクトの Default.aspx ページにボタンを作成し、 **onclick()** メソッドから **SubscribeEntity()** を呼び出す必要があります。
  
    
    

- Default.aspx ページを開き、次の HTML を追加します。
    
  ```HTML
  
  <input type="button" value="Subscribe" onclick="SubscribeEntity();"/>
  ```

イベントを動作させるには、SharePoint リストを有効にして、外部イベントを受け取れるようにすることも必要です。これは、外部イベント機能をオンにすることで可能になります。
  
    
    
クライアント コードを使用して、この機能をオンにするスクリプトを次に示します。
  
    
    



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

 **Subscribe** と同様に、この機能をオンにするボタンをページに追加します。
  
    
    
Default.aspx ページの [ **サブスクライブ**] ボタンのすぐ下に、次の HTML を追加します。
  
    
    



```HTML

<input type="button" value="EnableEventing" onclick="EnableEventing_Clicked();"" />
```

このシナリオでは次に、外部システムが送信した通知を表示する通知リストを追加する必要があります。
  
    
    

### 通知リストを追加するには


1. [ **ソリューション エクスプローラー**] で、プロジェクト名のショートカット メニューを開き、[ **追加**] を選択します。
    
  
2. [ **リスト**] テンプレートを選択し、リストに「NotificationList」という名前を付けます。
    
  
グローバル アセンブリ キャッシュで SharePoint で登録されているイベント レシーバーを模倣するため、このサンプルでは、変更のリッスンを開始するコンソール アプリケーションを提供します。通知を受信すると、リスト アイテムが **NotificationList** に追加されます。
  
    
    

### コマンドライン イベント レシーバーを開始するには


1. [ **RemoteEventReceiverConsoleApp**] を開きます。
    
  
2. [ **Program.cs**] ファイルを開き、ローカル コンピューター (またはイベント レシーバーを実行中のコンピューター) の名前を変更します。
    
  
3. Visual Studio の [ **スタート**] ボタンをクリックし、コンソール アプリケーションを実行します。
    
    コマンド ウィンドウが開きます。このウィンドウは、アプリケーションが正常にコンパイルされたことを示します。アプリケーションは外部システムからの変更通知をリッスンしています。このウィンドウは、このテスト中、開いたままにしておいてください。
    
  

### アプリを SharePoint に展開するには


- Visual Studio で F5 キーを押して、ビルドと展開を行います。
    
  

## アプリのテスト
<a name="bkmk_testApp"> </a>

これでアプリが実行できるようになりました。
  
    
    

1. アプリを色々と動かし、外部システム内のデータを表すさまざまなリストを表示します。
    
  
2. [ **顧客**] リストをクリックします。
    
  
3. 新しい顧客を追加します。
    
  
4. 先ほど作成した新しいリスト アイテムを表示します。
    
  
5. 通知リストに移動して、外部システムが顧客テーブルの変更を SharePoint に通知したことを確認します。
    
    1 分ごとに通知を送信するようにタイマーが設定されているということに注意してください。テスト中、通知が送信されるまでに、少し時間がかかることがあります。
    
  

## その他の技術情報
<a name="bkmk_additionalresources"> </a>


-  [SharePoint 2013 の外部イベントおよびアラート](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [方法: アドイン スコープの外部コンテンツ タイプを作成する (SharePoint 2013 )](how-to-create-an-add-in-scoped-external-content-type-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 でクライアント オブジェクト モデルと外部データを使用する方法の概要](get-started-using-the-client-object-model-with-external-data-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の Business Connectivity Services](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の Business Connectivity Services の新機能](what-s-new-in-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の Business Connectivity Services の概要](get-started-with-business-connectivity-services-in-sharepoint-2013.md)
    
  

