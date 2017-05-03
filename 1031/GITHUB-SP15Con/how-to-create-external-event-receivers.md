---
title: Vorgehensweise Erstellen von externen Ereignisempfängern
ms.prod: SHAREPOINT
ms.assetid: c6d5f486-6247-47f9-9876-fab12f13342f
---


# Vorgehensweise: Erstellen von externen Ereignisempfängern
Informationen Sie zu den Schritten zum Erstellen von externen-Ereignisempfänger für lokale Installationen von Business Connectivity Services (BCS) externer Listen.
Externe Ereignisempfänger sind Klassen, mit die SharePoint-Add-Ins auf Ereignisse reagieren, die auf SharePoint-Elemente wie Listen oder Listenelemente auftreten können. Beispielsweise können Sie auf die listenereignisse, z. B. hinzufügen oder Entfernen eines Felds reagieren; Listen Sie Elementereignisse wie hinzufügen oder Entfernen eines Listenelements oder einer Anlage zu einem Listenelement auf. oder Webereignisse wie beispielsweise hinzufügen oder Löschen einer Website oder Websitesammlung. Sie können einer vorhandenen Visual Studio-Lösung einen remote-Ereignisempfänger hinzufügen, die ein SharePoint-Add-In enthält.
  
    
    

Zu diesem Artikel gehörende Codebeispiel  [SharePoint 2013: erstellen ein remoteereignissempfängers für externe Daten](http://code.msdn.microsoft.com/SharePoint-2013-Create-a-095c594c). Es veranschaulicht, wie alle Komponenten, die zum Konfigurieren und Verwenden von externen System ereignisbenachrichtigungen benötigt erstellen.
In diesem Beispiel wird Folgendes:
  
    
    


1. Erstellen Sie ein externes System basierend auf der Northwind-Beispieldatenbank
    
  
2. Verfügbar machen Sie, die über OData-Beispieldaten durch Erstellen eines Windows Communication Foundation (WCF)-Diensts.
    
  
3. Erstellen Sie einen Abruf-Dienst, der überwacht die Änderungen in den Daten und SharePoint 2013 über diese Änderungen benachrichtigen.
    
  
4. Erstellen Sie einen externen Ereignisempfänger, der ausgeführt wird, wenn die externen Daten Elemente hinzugefügt werden und daher erstellt ein neues Listenelement in einer Benachrichtigungsliste.
    
  

## Erforderliche Komponenten und System einrichten
<a name="bkmk_Prerequisites"> </a>

Um das Beispiel zu vervollständigen, benötigen Sie die folgenden erforderlichen Komponenten:
  
    
    

- Visual Studio 2012
    
  
- Office Developer Tools für Visual Studio 2013
    
  
- SQL Server
    
  
- SharePoint 2013
    
  
- Internetinformationsdienste 7.0
    
  
- Northwind-Beispieldatenbank
    
  

## Die Komponenten für externe Systeme erstellen
<a name="BuildingTheComponents"> </a>

Der größte Teil der Konfiguration geschieht tatsächlich auf dem externen System. Damit externe Ereignisempfängern ordnungsgemäß funktioniert muss die folgenden Komponenten vorhanden ist und auf dem externen System:
  
    
    

- **Abonnementspeicher:** Eine Tabelle mit Informationen zu den Abonnenten, die Änderungen an der externen Daten benachrichtigt werden möchten.
    
  
- **Änderungen zu speichern:** Eine Tabelle, die zum Speichern der Änderungen an der Datenelemente verwendet wird. Es fungiert als temporärer Speicher, da der Dienst Abruf löscht das Element in der Tabelle aus, wenn an Abonnenten in SharePoint-Benachrichtigungen gesendet werden.
    
  
- **Abruf Service:** In diesem Szenario erforderlich, als Mittel zur Überprüfung, wenn Daten geändert und auf die Änderungstabelle übermittelt wurde. Der Dienst Abruf fragt die Änderungstabelle und baut ein Notification-Paket, das an den SharePoint-Empfängeradresse (REST-Endpunkt) gespeichert, in dem Abonnementspeicher gesendet wird.
    
  
- **OData-WCF-Datendienste:** Um die Daten aus dem externen System-Datenbank verfügbar zu machen, müssen Sie einen WCF-Dienst zu erstellen. Dieser Dienst, ausgeführt auf Internet Information Services (IIS), bietet der Rest-Schnittstelle und OData-feed, der für dieses Szenario erforderlich ist.
    
  

## Richten Sie das externe system
<a name="bkmk_SetUpTheExternalSystem"> </a>

Die erste Aufgabe besteht darin, das externe System einrichten.
  
    
    

### Fügen Sie die Nordwind-Datenbank

Der erste Teil der Vorbereitung des Back-End-Systems ist so eine ausgeführte Instanz von SQL Server die Nordwind-Datenbank hinzu. Wenn Sie bereits die Nordwind-Datenbank installiert haben, können Sie die Skripts ausführen, im folgenden Abschnitt, um die zusätzlichen Objekte benötigt für externe Ereignisdienst Benachrichtigungen zu erstellen.
  
    
    
Jedoch, wenn Sie keine Northwind installiert haben, finden Sie unter  [Installieren der Northwind-Beispieldatenbank](http://msdn.microsoft.com/library/2f92cfc3-6310-4327-b2f2-8610f7385c86%28Office.15%29.aspx).
  
    
    
Die Datenbank ist auch in das Codebeispiel enthalten:  [SharePoint 2013: erstellen ein remoteereignissempfängers für externe Daten](http://code.msdn.microsoft.com/SharePoint-2013-Create-a-095c594c).
  
    
    

### Erstellen von dem Abonnementspeicher

Wenn die Nordwind-Datenbank installiert ist, öffnen Sie ein neues Abfragefenster, und führen Sie das folgende Skript aus.
  
    
    

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

Dadurch wird eine Tabelle in der Nordwind-Datenbank mit dem Namen EntitySubscribe erstellt. Dies dient dem Abonnementspeicher weiter oben genannten.
  
    
    

### Erstellen Sie den Speicher ändern

Führen Sie das folgende Skript aus, um die Änderungstabelle erstellen, die Änderungen an den Daten in der Tabelle Customers aufgezeichnet werden.
  
    
    

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

Dadurch wird eine Tabelle mit dem Namen Customers_Updates, die die Informationen zu den Datensatz speichert, das der Customers-Tabelle hinzugefügt wurde, hinzugefügt.
  
    
    

### Erstellen Sie den Auslöser ändern

Der Änderung Trigger wird ausgelöst, wenn Änderungen auf die Tabelle Kunden erfolgen. Bei jedem Kunden ein Datensatz hinzugefügt wird, führt SQL Server den Auslöser, der einen neuen Datensatz in der Tabelle Customers_Updates mit Informationen zu den Datensatz einfügt.
  
    
    
Führen Sie die folgende Abfrage aus, um den Auslöser erstellen.
  
    
    



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


> **HINWEIS**
> Wenn Sie Ihre eigenen benutzerdefinierten gespeicherten Prozeduren gemäß Definition im BDC-Modell verwenden, sollten Sie auch löschen erstellen und Aktualisieren von Triggern. Zusätzliche Trigger werden nicht im Rahmen dieses Szenarios behandelt werden.
  
    
    


### Erstellen Sie die gespeicherten Prozeduren

Wenn Sie Zugriff auf die direkte Tabelle mit Business Connectivity Services dieser Verfahren nicht erforderlich verwenden. Wenn Sie Ihre eigenen Prozeduren und benutzerdefinierter Code für das externe System definieren, sollten Sie diese Option, um die Gewährung des Zugriffs auf die Tabelle von SQL Server hinzufügen.
  
    
    

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

Die **SubscribeEntity** gespeicherte Prozedur erstellt einen Datensatz in dem Abonnementspeicher.
  
    
    



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


## Erstellen des OData-Diensts
<a name="bkmk_CreatingTheODataService"> </a>

OData-Feed wird innerhalb eines **WCF-Datendienste**gehostet. Dieser WCF-Dienst wird anschließend in einer Webanwendung von IIS gehostet.
  
    
    
Die folgenden Schritte erstellen Sie eine neue ASP.NET WCF-Datendienste.
  
    
    

### So erstellen Sie die WCF Data Service-Anwendung


1. Wählen Sie in Visual Studio 2012 klicken Sie im Menü **Datei** auf **neu**, **Projekt**.
    
  
2. Klicken Sie im Dialogfeld **Neues Projekt** unter den Knoten Visual c# wählen Sie **die Vorlage**, und wählen Sie dann **ASP.NET-Webanwendung**.
    
  
3. Geben Sie den Namen des Projekts NorthwindService , und wählen Sie dann auf **OK**.
    
  
Im nächsten Schritt mit dem Visual Studio-Assistenten, Sie verwenden, um ein ADO.NET Entitätsdatenmodell erstellen und ermitteln das Schema der Datenquelle.
  
    
    

### So definieren Sie das Datenmodell


1. Öffnen Sie im **Projektmappen-Explorer** das Kontextmenü für das Projekt ASP.NET, und wählen Sie **Neues Element hinzufügen**.
    
  
2. Klicken Sie im Dialogfeld **Neues Element hinzufügen** auswählen **der Datenvorlage**, und wählen Sie dann **ADO.NET Entitätsdatenmodell**.
    
  
3. Geben Sie den Namen des Datenmodells zu Northwind.edmx.
    
  
4. Wählen Sie im Entity Data Model-Assistenten **aus Datenbank generieren**, und wählen Sie dann auf **Weiter**.
    
  
5. Verbinden Sie das Datenmodell mit der Datenbank, führen Sie einen der folgenden Schritte aus:
    
1. Wenn Sie keine datenbankverbindung bereits konfiguriert haben, wählen Sie **Neue Verbindung** aus, und erstellen Sie eine neue Verbindung. Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen von Verbindungen mit SQL Server-Datenbanken](http://msdn.microsoft.com/library/360c340d-e5a6-4a7e-a569-e95d500be43d%28Office.15%29.aspx). Diese Instanz von SQL Server benötigen die Nordwind-Datenbank zugeordnet ist. Wählen Sie auf **Weiter**.
    
    -Oder-
    
  
2. Wenn Sie eine Verbindung zum Verbinden mit der Nordwind-Datenbank bereits konfiguriert haben, wählen Sie diese Verbindung aus der Liste der Verbindungen, und wählen Sie dann auf **Weiter**.
    
  
6. Wählen Sie auf der letzten Seite des Assistenten die Kontrollkästchen für alle Tabellen in der Datenbank, und deaktivieren Sie die Kontrollkästchen für die Ansichten und gespeicherte Prozeduren.
    
  
7. Wählen Sie die Schaltfläche **Fertig stellen** aus, um den Assistenten zu schließen.
    
  
Im nächsten Schritt erstellen Sie den eigentlichen Dienst, der von IIS gehostet wird, der die Mittel Zugriff auf externe Daten über Representational State Transfer (REST) bereitstellt.
  
    
    

### Zum Erstellen der WCF-Dienst


1. Öffnen Sie im **Projektmappen-Explorer** das Kontextmenü für Ihr Projekt ASP.NET, und wählen Sie **Neues Element hinzufügen**.
    
  
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** **WCF Data Service**.
    
  
3. Geben Sie für den Namen des Diensts Northwind.
    
  
4. Ersetzen Sie in den Code für die Data-Dienst, in der Definition der Klasse, die den Datendienst definiert den Kommentar  `/* TODO: put your data source class name here */` mit dem Typ, der Entitätscontainer des Datenmodells, die in diesem Fall `NorthwindEntities`ist. Die Definition der Klasse sollte wie folgt aussehen.
    
  ```cs
  
public class Northwind : DataService<NorthwindEntities>

  ```


### Die Sicherheit für den Dienst festlegen


- Sie haben jetzt die Sicherheit, um die Gewährung des Zugriffs auf die Daten aus der OData-feed von externen Empfängern zu ändern. Wenn ein WCF-Dienst erstellt wird, wird standardmäßig alle Zugriff verweigert. Stellen Sie die folgenden Änderungen auf die soeben erstellte Klasse.
    
  ```cs
  
config.SetEntitySetAccessRule("*", EntitySetRights.All);
config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


### Erstellen Sie den Vorgang Subscribe (optional)

WCF-Dienst kann auch eine Möglichkeit zum Behandeln der abonnieren und kündigen Anforderungen von SharePoint codiert werden. Benutzerdefinierte gespeicherte Prozeduren für die Anwendung erstellt werden soll wird jeweils durch ein Vorgang behandelt werden.
  
    
    
 **Abonnieren**: Abonnieren den Vorgang nimmt die Anforderung vom SharePoint gesendet und der Empfängeradresse den Ereignistyp und die Entität abgerufen. Es muss außerdem ein **subscriptionId** generieren und zeichnen Sie dann alle diese in der Tabelle der Datenbank.
  
    
    



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


> **HINWEIS**
> Wenn SQL Server für die Windows-Authentifizierung eingerichtet ist, versucht er, die Anforderung mit der Identität des App-Pool zu authentifizieren. Stellen Sie sicher, dass das Konto in der App-Pool konfiguriert Rechte zum Lesen und Schreiben in die Datenbank verfügt.
  
    
    


## Erstellen Sie den Dienst abrufen
<a name="bkmk_CreateThePollingService"> </a>

Der Abruf-Dienst ist ein Windowsdienst, der für die Abfrage der Änderungstabelle und erstellen und Senden von Benachrichtigungen zu SharePoint 2013 oder SharePoint Online an der angegebenen zustellungs-Adresse zuständig ist.
  
    
    
Im nächsten Schritt erstellen Sie ein neues Windows-Dienst-Projekt, das registriert wird, auf dem Hostcomputer WCF. Nachdem das Projekt registriert ist, können Sie in der Microsoft Management Console (MMC) den ausgeführten Dienst anzeigen.
  
    
    

### So erstellen Sie ein neues Projekt


1. Öffnen Sie Visual Studio 2012.
    
  
2. Erstellen eines neuen Projekts mithilfe der Windows-Dienst-Vorlage, nennen Sie das Projekt PollingService, und wählen Sie die Schaltfläche **OK**.
    
  
3. Wenn das Projekt erstellt wird, öffnen Sie die PollingService.cs-Datei in der Codeansicht.
    
  
4. Fügen Sie den folgenden Code in der neu erstellten-Klasse.
    
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

Im nächste Schritt wird eine ausführbare Datei erstellen, die die ausgeführten Dienste auf dem Computer OData hinzugefügt werden können.
  
    
    

### So kompilieren und stellen Sie den Dienst abrufen


1. Drücken Sie F5, um das Projekt zu erstellen.
    
  
2. Öffnen Sie die **Befehlszeile für VS2012**.
    
  
3. Navigieren Sie dazu aufgefordert werden zu dem Speicherort der Project-Ausgabe.
    
  
4. Suchen Sie in das Verzeichnis Bin die PollingService.exe-Datei aus.
    
  
5. Geben Sie Installutil PollingService.exe , und drücken Sie die EINGABETASTE.
    
  
6. Stellen Sie sicher, dass der Dienst ausgeführt durch Ausführung der Dienste-MMC.
    
  

## Erforderliche Komponenten für SharePoint
<a name="bkmk_SharePointComponents"> </a>

Um das gesamte System abgeschlossen haben, müssen die folgenden Komponenten auf dem Server, auf dem SharePoint 2013 ausgeführt wird.
  
    
    

- **Externer Inhaltstyp:** Der externe Inhaltstyp ist im Wesentlichen eine XML-Definition der externen Datenquelle. Für dieses Szenario verwenden Sie die neue automatische Generierung von Tools in Visual Studio 2012 zum Ermitteln der Datenquelle und den externen Inhaltstyp automatisch erstellt.
    
  
- **Externe Ereignisempfänger:** Der Remote- oder externen Ereignisempfänger ist die Sache, die Aktionen auf externe datenänderungen in SharePoint 2013 ermöglicht. Sie können Ereignisempfänger für externe Listen und für Entitäten erstellen.
    
    Ein Ereignisempfänger, der für eine externe Liste erstellt wird, ähnelt anderen-Ereignisempfänger für SharePoint-Listen. Erstellen Sie den Code, und fügen sie Sie der Liste. Wenn eine Aktion für die Daten ausgeführt wird, die durch die Liste dargestellt wird, führt der Ereignisempfänger aus.
    
    Ein Ereignisempfänger für Entitäten wird genau wie ein List-basierten Ereignisempfänger ausgeführt, jedoch nicht muss, um zu einer Liste angefügt werden soll. Er erhält Benachrichtigungen auf die gleiche Weise wie, aber sie benötigt nicht die Schnittstelle, die im Beispiel listenbasierten zugeordnet ist. Dies ist, Sie können die Benachrichtigungen programmgesteuert intercept und Erstellen von Code, um eine bestimmte Aktion ausgeführt. In diesem Szenario wird die Aktion zum Erstellen eines neuen Datensatzes in der Benachrichtigungsliste
    
  
- **Benachrichtigungsliste:** Benachrichtigungsliste ist eine einfache SharePoint-Liste, die zum Aufzeichnen von Benachrichtigungen empfangen aus dem externen System verwendet wird. Für jeden neuen Datensatz, das externe System hinzugefügt wurde wird ein Datensatz in der Benachrichtigungsliste erstellt.
    
  

## Richten Sie die SharePoint-Komponenten
<a name="bkmk_SettingUpTheSharePointComponents"> </a>

Nun, da Sie das externe System eingerichtet haben, ist es Zeit zum Wechsel auf die andere Hälfte der Formel erstellen. Erstellen Sie nun die Komponenten, die in SharePoint gehostet werden.
  
    
    

### Erstellen Sie ein neues Projekt von Visual Studio 2012


1. Wählen Sie im Visual Studio 2012 **Neues Projekt**.
    
  
2. Wählen Sie die SharePoint-app-Projektvorlage.
    
  
Office Developer Tools für Visual Studio 2013 hinzugefügt, ein automatische Generierung von-Assistent, der als entdecken Schema der Datenquelle und klicken Sie dann aus, die einen externen Inhaltstyp erstellen.
  
    
    

### So fügen Sie einen neuen externen Inhaltstyp hinzu


- Klicken Sie im **Projektmappen-Explorer** öffnen Sie das Kontextmenü für das Projekt, und wählen Sie **Hinzufügen** und **Inhaltstypen für eine externe Datenquelle**.
    
    Dies startet den **Assistenten zum Anpassen von SharePoint**, die automatisch erstellen des externen Inhaltstyps verwendet wird.
    
    Weitere Informationen zum Erstellen externer Inhaltstypen finden Sie unter  [Gewusst wie: Erstellen eines externen Inhaltstyps aus einer OData-Quelle in SharePoint 2013](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md).
    
  
Ändern Sie jetzt den XML-Code, der im vorherigen Schritt zum Hinzufügen von Methoden zum Abonnieren generiert wurde. Dadurch wird BCS mit dem externen System kommunizieren, wenn jemand abonniert externe datenänderungen informiert werden.
  
    
    

### So fügen Sie die Subscribe-Methode für den externen Inhaltstyp XML hinzu


1. Suchen Sie im **Projektmappen-Explorer** den neuen Knoten mit dem Namen **External Content Types**.
    
  
2. Suchen Sie die Datei **Customers.ect**, und öffnen Sie es mit einem XML-Editor.
    
  
3. Führen Sie einen Bildlauf nach unten zum unteren Rand der Seite, und fügen Sie die folgende Methode in den Abschnitt  `<Methods>` .
    
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

Fügen Sie jetzt Clientcode, um der Liste, um ereignisbenachrichtigungen abonnieren zu ermöglichen.
  
    
    

### Die Datei App.js, das Abonnement zu initiieren Code hinzu


- Öffnen Sie in den Ordner Skripts Ihres Projekts SharePoint-Add-In die Datei **App.js** aus. Fügen Sie die folgende Methode in die Datei.
    
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

Um den Ereignisempfänger mit diesem Skript zu registrieren, müssen Sie eine Schaltfläche auf der Seite "Default.aspx" in Ihrem Projekt erstellen, und rufen Sie die **SubscribeEntity()** die **onclick()** -Methode.
  
    
    

- Öffnen Sie die Seite "default.aspx", und fügen Sie den folgenden HTML-Code hinzu.
    
  ```HTML
  
  <input type="button" value="Subscribe" onclick="SubscribeEntity();"/>
  ```

Für Ereignisdienst funktioniert müssen Sie auch die SharePoint-Liste externe Ereignisse akzeptieren aktivieren. Dies erfolgt durch Aktivieren des Features für externe Ereignisse.
  
    
    
Es folgt ein Skript, das das Feature mithilfe der Clientcode aktiviert wird.
  
    
    



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

Genau wie mit **Subscribe**, fügen Sie eine Schaltfläche auf der Seite hinzu, die das Feature aktiviert wird.
  
    
    
Fügen Sie den folgenden HTML-Code auf der Seite Default.aspx direkt unterhalb der Schaltfläche " **Anmelden** ".
  
    
    



```HTML

<input type="button" value="EnableEventing" onclick="EnableEventing_Clicked();"" />
```

Für dieses Szenario müssen Sie als Nächstes eine Benachrichtigungsliste hinzufügen, die die Senden von Benachrichtigungen vom externen System angezeigt wird.
  
    
    

### So fügen Sie der Benachrichtigungsliste hinzu


1. Klicken Sie im **Projektmappen-Explorer** öffnen Sie das Kontextmenü für den Projektnamen, und wählen Sie **Hinzufügen**.
    
  
2. Wählen Sie die Vorlage **Liste** aus, und nennen Sie die ListeNotificationList.
    
  
Um einen Ereignisempfänger imitieren, der mit SharePoint im globalen Assemblycache registriert ist, enthält das Beispiel eine Konsolenanwendung, die überwacht werden, damit die Änderungen gestartet wird. Wenn sie eine Benachrichtigung erhält, werden die **NotificationList**ein Listenelements hinzugefügt.
  
    
    

### Starten Sie den Command-Line-Ereignisempfänger


1. Öffnen Sie die **RemoteEventReceiverConsoleApp**.
    
  
2. Öffnen Sie die Datei **Program.cs**, und ändern Sie den Namen von Ihrem lokalen Computer (oder dem Computer, auf dem der Ereignisempfänger ausgeführt wird).
    
  
3. Klicken Sie auf die Schaltfläche **Start** in Visual Studio die Konsolenanwendung ausgeführt.
    
    Ein Befehlsfenster wird geöffnet, die angibt, dass die Anwendung ordnungsgemäß kompiliert wurde, und es für Benachrichtigungen aus dem externen System ändern hört. Lassen Sie dieses Fenster geöffnet, während des Tests.
    
  

### Zum Bereitstellen der app für SharePoint


- Drücken Sie F5 mit Visual Studio zum Erstellen und Bereitstellen der app öffnen.
    
  

## Testen der App
<a name="bkmk_testApp"> </a>

Nun können Sie die app in Aktion angezeigt werden.
  
    
    

1. Durchsuchen Sie, um die app aus, um die verschiedenen Listen anzuzeigen, die Daten im externen System darstellen.
    
  
2. Klicken Sie auf die Liste der **Kunden**.
    
  
3. Hinzufügen eines neuen Kundens.
    
  
4. Zeigen Sie das neue Listenelement, den, das Sie gerade erstellt haben.
    
  
5. Wechseln Sie zu der Benachrichtigungsliste zu sehen, dass das externe System SharePoint von einer Änderung der Customers-Tabelle benachrichtigt hat.
    
    Behalten Sie im Hinterkopf, die der Timer zum Senden von Benachrichtigungen einer Minute festgelegt ist. Beim Testen, müssen Sie möglicherweise für eine kurze Zeit, bevor Sie die Benachrichtigungen gebucht finden Sie unter warten.
    
  

## Zusätzliche Ressourcen
<a name="bkmk_additionalresources"> </a>


-  [Externe Ereignisse und Warnungen in SharePoint 2013](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: erstellen ein Add-in-bezogenen externes Inhaltstyps in SharePoint 2013](how-to-create-an-add-in-scoped-external-content-type-in-sharepoint-2013.md)
    
  
-  [Erste Schritte der Verwendung des Clientobjektmodells mit externen Daten in SharePoint 2013](get-started-using-the-client-object-model-with-external-data-in-sharepoint-2013.md)
    
  
-  [Business Connectivity Services in SharePoint 2013](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Was ist neu in Business Connectivity Services in SharePoint 2013](what-s-new-in-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Erste Schritte mit den Business Connectivity Services in SharePoint 2013](get-started-with-business-connectivity-services-in-sharepoint-2013.md)
    
  

