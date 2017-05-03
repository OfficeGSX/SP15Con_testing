---
title: Vorgehensweise Erstellen einer OData-Datendiensts zur Verwendung als einer externen BCS-System
ms.prod: SHAREPOINT
ms.assetid: 7d7b3aa6-85b7-400d-8ea5-50bebac56a1d
---


# Vorgehensweise: Erstellen einer OData-Datendiensts zur Verwendung als einer externen BCS-System
In diesem Artikel erfahren sie, wie Sie einen im Internet aufrufbaren WCF-Dienst erstellen, der OData zum Senden von Benachrichtigungen an SharePoint 2013 verwendet, wenn die zugrunde liegenden Daten geändert werden. Mithilfe dieser Benachrichtigungen werden Ereignisse ausgelöst, die mit externen Listen verknüpft sind.
In diesem Artikel wird beschrieben, wie zum Erstellen einer ASP.NET Windows Communication Foundation (WCF) Data-Dienst, die 2012 LT AdventureWorks-Beispieldatenbank verfügbar zu machen. So können Sie für den Datenzugriff über die Open Data Protocol (OData). Beim Zugriff über OData hergestellt wurde, können Sie einen externen Inhaltstyp für Business Connectivity Services (BCS) konfigurieren, mit der SharePoint 2013, die Daten aus einer externen Datenbank verwenden können. Um diese OData-Quelle zu erhöhen, können Sie mit dem Dienst WCF Serviceverträge hinzufügen, mit die BCS, um Benachrichtigungen zu abonnieren, die angeben, dass die externen Daten geändert hat, kann.
  
    
    


## Voraussetzungen für die Erstellung des OData-Diensts
<a name="bkmk_Prerequisites"> </a>

Folgende sind zum Erstellen des OData-Diensts in diesem Artikel erforderlich:
  
    
    

- Ein Server mit Internet Information Services (IIS)
    
  
- .NET Framework 3.5 oder höher
    
  
- SharePoint 2013
    
  
-  [AdventureWorks 20012 LT-Skript](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
-  [AdventureWorks 2012 LT Daten](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
- Visual Studio 2012
    
  
- Office Developer Tools für Visual Studio 2012
    
  
Informationen zum Einrichten Ihrer Entwicklungsumgebung finden Sie unter  [Einrichten einer allgemeinen Entwicklungsumgebung für SharePoint 2013](set-up-a-general-development-environment-for-sharepoint-2013.md).
  
    
    

### Kernkonzepte für das Erstellen eines OData-Diensts

In Tabelle 1 sind Artikel aufgeführt, Ihnen das Verständnis der Kernkonzepte der Erstellung von einem WCF-Dienst mithilfe von OData und externen Inhalt werden.
  
    
    

**In Tabelle 1. Kernkonzepte für das Erstellen eines OData-Diensts**


|**Ressource**|**Beschreibung**|
|:-----|:-----|
| [Verwenden von OData-Quellen mit Business Connectivity Services in SharePoint 2013](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md) <br/> |Enthält Informationen, die Sie mit dem Erstellen von externer Inhaltstypen basierend auf OData-Quellen und Verwenden der Daten in SharePoint 2013 oder Office 2013-Komponenten unterstützen. <br/> |
| [Gewusst wie: Erstellen eines externen Inhaltstyps aus einer OData-Quelle in SharePoint 2013](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md) <br/> |Erfahren Sie, wie Visual Studio 2012 verwenden, um eine veröffentlichte OData-Quelle ermitteln und erstellen Sie einen wiederverwendbaren externen Inhaltstyp im BCS in SharePoint 2013 verwenden. <br/> |
| [Open Data Protocol](http://www.odata.org) <br/> |Enthält Informationen zu den Open Data Protocol, einschließlich Definitionen des Protokolls, architektonische Informationen und Beispiele für die Verwendung. <br/> |
| [WCF Data Services (Übersicht)](http://msdn.microsoft.com/en-us/library/cc668794.aspx) <br/> |WCF Data Services ermöglicht die Erstellung und Verwendung von Data Services für das Internet oder Intranet mithilfe von OData. OData können Sie Ihre Daten als Ressourcen verfügbar machen, adressierbar über URIs sind. <br/> |
| [Entwickeln und Bereitstellen von WCF Data Services](http://msdn.microsoft.com/en-us/library/gg258442) <br/> |Enthält Informationen zu entwickeln und Bereitstellen von WCF Data Services. <br/> |
   

### Schritte zum Erstellen des externen Systems

Die folgenden Schritte ausgeführt werden müssen
  
    
    

- Installieren der AdventureWorks 2012 LT-Beispieldatenbank
    
  
- Erstellen des OData-Diensts
    
  
- Festlegen von Zugriffsberechtigungen
    
  
- Testen Sie den Dienst
    
  
- Hinzufügen von Funktionen für zusätzliche BCS-Funktionen
    
  

## Installieren der Beispieldatenbank
<a name="bkmk_Prerequisites"> </a>

Ein externes System ist in der Regel eine Datenbank, und aus diesem Grund in diesem Beispiel wird veranschaulicht, wie die 2012-LT AdventureWorks-Beispieldatenbank verwenden, um eine proprietäre Datenquelle darstellen.
  
    
    
Die folgenden Schritte wird
  
    
    

## Zum Erstellen der WCF OData-Dienst
<a name="bkmk_CreatingTheService"> </a>

Erstellen einen Windows Communication Foundation (WCF)-Dienst, der über den OData-Protokoll zugegriffen werden kann, ist relativ einfach. Visual Studio 2012 bietet verschiedene Tools zum automatisch erkennen und die Daten aus verschiedenen Datenquellen modellieren. Dadurch können Sie zum Erstellen von Verbindungen und Schnittstellen mit Daten in SQL Server-Datenbanken und anderen Arten von Datenspeichern, die bei der Verwendung von eines umfangreiches Objektmodells programmgesteuert bearbeitet werden können.
  
    
    
SharePoint 2013 aktivieren BCS Benachrichtigungen von remote-Systemen ausgegeben, wenn die zugrunde liegenden Daten geändert haben, müssen Sie jedoch den WCF-Dienst, um auf diese Änderungen reagieren ändern.
  
    
    

### So erstellen Sie ein neues WCF-Projekt


1. Wählen Sie im Visual Studio, klicken Sie im Menü **Datei** auf **neu**, **Projekt**.
    
  
2. Klicken Sie im Dialogfeld **Neues Projekt** wählen Sie **die Vorlage** aus, und wählen Sie dann **ASP.NET-Webanwendung**.
    
  
3. Geben Sie **AdventureWorksService** für den Projektnamen, und wählen Sie **OK**.
    
  
4. Klicken Sie im **Projektmappen-Explorer** öffnen Sie das Kontextmenü für das ASP.NET-Projekt, das Sie gerade erstellt haben, und wählen Sie **Eigenschaften**.
    
  
5. Wählen Sie die Registerkarte **Web**, und legen Sie den Wert des Textfelds **bestimmte Ports** auf8008.
    
  

### So definieren Sie das Datenmodell


1. Öffnen Sie im **Projektmappen-Explorer** das Kontextmenü für das Projekt ASP.NET, und wählen Sie **Neues Element hinzufügen**.
    
  
2. Klicken Sie im Dialogfeld **Neues Element hinzufügen** Auswählen der Datenvorlage, und wählen Sie dann **ADO.NET Entitätsdatenmodell**.
    
  
3. Geben Sie den Namen des Datenmodells zu **AdventureWorks.edmx**aus.
    
  
4. Im **Entity Data Model Wizard** wählen Sie **aus Datenbank generieren**, und wählen Sie dann auf **Weiter**.
    
  
5. Verbinden Sie das Datenmodell mit der Datenbank führen Sie einen der folgenden Schritte aus, und wählen Sie dann auf **Weiter**.
    
  - Wenn Sie keine datenbankverbindung bereits konfiguriert haben, wählen Sie **Neue Verbindung** aus, und erstellen Sie eine neue Verbindung.
    
  
  - Wenn Sie eine Verbindung zum Verbinden mit der Nordwind-Datenbank bereits konfiguriert haben, wählen Sie diese Verbindung in der Liste der Verbindungen.
    
  
  - Wählen Sie auf der letzten Seite des Assistenten die Kontrollkästchen für alle Tabellen in der Datenbank, und deaktivieren Sie die Kontrollkästchen für die Ansichten und gespeicherte Prozeduren.
    
  
6. Wählen Sie auf **Fertig stellen**, um den Assistenten zu schließen.
    
  

### So erstellen Sie den Datendienst


1. Öffnen Sie im **Projektmappen-Explorer** das Kontextmenü für Ihr Projekt ASP.NET, und wählen Sie dann auf **Neues Element hinzufügen**.
    
  
2. Wählen Sie im Dialogfeld **Neues Element hinzufügen** **WCF Data Service**.
    
  
3. Geben Sie für den Namen des Diensts **AdventureWorks**ein.
    
    Visual Studio wird die XML-Markup und Code-Dateien für den neuen Dienst erstellt. In der Standardeinstellung öffnet das Code-Editor-Fenster. Klicken Sie im **Projektmappen-Explorer**, der Dienst hat den Namen **AdventureWorks**, mit der Erweiterung. svc.cs oder. svc.vb.
    
  
4. Ersetzen Sie den Kommentar  `/* TODO: put your data source class name here */` in der Definition der Klasse, die den Datendienst mit dem Typ definiert, die Entitäten-Container des Datenmodells, die in diesem Fall **AdventureWorksEntities** ist. Die Definition der Klasse sollte wie folgt aussehen:
    
  ```cs
  
public class AdventureWorks : DataService<AdventureWorksEntities>
  ```

Standardmäßig beim Dienst WCF erstellt wurde, kann es aufgrund der Sicherheitskonfiguration zugegriffen werden. Im nächsten Schritt können, die Sie angeben, die darauf zugreifen können und welche Berechtigungen, besitzen.
  
    
    

### Zugriff auf Daten Dienstressourcen unterstützt


- Ersetzen Sie den Platzhalter-Code in der **InitializeService** -Funktion im Code für den Datendienst durch Folgendes.
    
  ```cs
  
      config.SetEntitySetAccessRule("*", EntitySetRights.All);
      config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


    Auf diese Weise können autorisierte Clients Lese- und Schreibzugriff auf Ressourcen für die angegebene Entitätenmengen.
    
    > **HINWEIS**
      > Jeder Client, der die ASP.NET Anwendung zugreifen kann, kann auch die Ressourcen zugreifen, die vom Datendienst verfügbar gemacht werden. In einem Datendienst Produktion sollte um nicht autorisierten Zugriff auf Ressourcen zu verhindern, dass Sie auch die Anwendung selbst sichern. Weitere Informationen finden Sie unter  [Sichern von WCF Data Services](http://msdn.microsoft.com/en-us/library/dd728284.aspx).
Für BCS zum Empfangen von Benachrichtigungen, muss es einen Mechanismus für die Back-End-Datenquelle, die eine Anforderung an hinzugefügt und entfernt aus der Benachrichtigungsabonnements werden akzeptieren.
  
    
    
Der letzte Schritt beim Erstellen des Diensts ist für die **Subscribe** und **Unsubscribe** Stereotype Vorgänge des Diensts hinzufügen, die im BDC-Modell definiert sind.
  
    
    

### Hinzufügen von Dienstvorgänge zum Abonnieren und Kündigen des Abonnements Stereotype


- Fügen Sie auf der Seite AdventureWorks.cs die folgende Zeichenfolge Variablendeklaration hinzu.
    
  ```cs
  
public string subscriptionStorePath = @"\\\\[SHARE_NAME]\\SubscriptionStore\\SubscriptionStore.xml";
  ```


    > **HINWEIS**
      > Diese Datei ist eine XML-Datei, die mit der neuen Abonnements aktualisiert wird. Zugriff auf diese Datei wird von der Serverprozess vorgenommen werden, so stellen Sie sicher, dass Sie über ausreichende Rechte für den Zugriff auf diese Datei gewährt haben.<br/> Sie möchten möglicherweise auch eine Lösung Datenbank zum Speichern von Informationen zu Abonnement zu erstellen.

    Fügen Sie die folgenden zwei **WebGet** -Methoden zum Behandeln von Abonnements aufgelistet.
    


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


## Nächste Schritte
<a name="bkmk_Next"> </a>

Um SharePoint zu benachrichtigen, dass Änderungen vorgenommen wurden, müssen Sie auch einen Dienst zu erstellen, der die Datenquelle für Änderungen abgefragt und sendet dann Benachrichtigungen an alle Benachrichtigungen abonniert.
  
    
    

## Zusätzliche Ressourcen
<a name="bkmk_Addresources"> </a>


-  [Business Connectivity Services in SharePoint 2013](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Verwenden von OData-Quellen mit Business Connectivity Services in SharePoint 2013](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Externe Inhaltstypen in SharePoint 2013](external-content-types-in-sharepoint-2013.md)
    
  
-  [Externe Ereignisse und Warnungen in SharePoint 2013](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [Gewusst wie: Erstellen eines externen Inhaltstyps aus einer OData-Quelle in SharePoint 2013](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md)
    
  
-  [Business Connectivity Services-Programmierreferenz für SharePoint 2013](business-connectivity-services-programmers-reference-for-sharepoint-2013.md)
    
  

