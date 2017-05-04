---
title: Vorgehensweise Hinzufügen einer Geolocation-Spalte einer Liste in SharePoint 2013 programmgesteuert
ms.prod: SHAREPOINT
ms.assetid: f31a3594-c328-4731-b8eb-5da6b85103ad
---



# Vorgehensweise: Hinzufügen einer Geolocation-Spalte einer Liste in SharePoint 2013 programmgesteuert
In diesem Artikel erfahren Sie, wie Sie programmgesteuert eine Geolocation-Spalte einer Liste in SharePoint 2013 hinzufügen. Sie können Standortinformationen und Karten in SharePoint-Listen und standortbasierten Websites mithilfe des neuen Geolocation-Felds hinzufügen, indem Sie Ihren eigenen Geolocation-basierten Feldtyp erstellen.
 * **Gilt für:*** 
  
    
    


|||
|:-----|:-----|
|**In diesem Artikel**          [Voraussetzungen für das Hinzufügen einer Geolocation-Spalteninhalts](#SP15addgeo_prereq)           [Codebeispiel: Programmgesteuertes Hinzufügen eine Geolocation-Spalte zu einer Liste](#SP15addgeo_addcolumn)           [Programmgesteuertes Hinzufügen eines Listenelements mit dem Wert des Geolocation-Feld zu einer SharePoint-Liste](#SP15addgeo_addlistitem)           [Zusätzliche Ressourcen](#SP15addgeo_addlresources) <br/> |
  
    
    
![Verwandte Codeausschnitte und Beispiel-Apps](images/mod_icon_links_samples.png)
  
    
    

  
    
    
 [SharePoint 2013: Add a GeoLocation column to a list programmatically](http://code.msdn.microsoft.com/SharePoint-2013-Add-a-d3fa8288) <br/> |
   

SharePoint 2013 introduces a new field type named Geolocation that enables you to annotate SharePoint lists with location information. In columns of type Geolocation, you can enter location information as a pair of latitude and longitude coordinates in decimal degrees or retrieve the coordinates of the user's current location from the browser if it implements the W3C Geolocation API. For more information about the Geolocation column, see [Integrieren von Standort- und Kartenfunktionen in SharePoint 2013](integrating-location-and-map-functionality-in-sharepoint-2013.md). The Geolocation column is not available by default in SharePoint lists. To add the column to a SharePoint list, you have to write code. In this article, learn how to add the Geolocation field to a list programmatically by using the SharePoint client object model.
  
    
    

Ein MSI-Paket mit dem Namen SQLSysClrTypes.msi muss auf jedem Front-End-Webserver SharePoint auf den Wert des Felds Geolocation oder die Daten in einer Liste sehen installiert werden. Dieses Paket installiert die Komponenten, die die neuen Geometrie, Geografie und Hierarchie-ID-Typen in SQL Server 2008 zu implementieren. Standardmäßig ist diese Datei für SharePoint Online installiert. Es ist jedoch nicht für eine lokale Bereitstellung von SharePoint Server 2013. Sie müssen ein Mitglied der Gruppe "Farmadministratoren" zum Ausführen dieses Vorgangs sein. Informationen zum Herunterladen von SQLSysClrTypes.msi finden Sie unter  [Microsoft SQL Server 2008 R2 SP1 Feature Pack](http://www.microsoft.com/en-us/download/details.aspx?id=26728) für SQL Server 2008 oder [Microsoft SQL Server 2012 Feature Pack](http://www.microsoft.com/en-us/download/details.aspx?id=29065)für SQL Server 2012 im Microsoft Download Center.
## Voraussetzungen für das Hinzufügen einer Geolocation-Spalteninhalts
<a name="SP15addgeo_prereq"> </a>


  
    
    

- Zugriff auf eine Liste von SharePoint 2013, mit der ausreichenden Berechtigungen, um eine Spalte hinzuzufügen.
    
  
- Ein gültiger Bing Maps-Schlüssel festgelegt Farm oder Web-Ebene, die aus der  [Bing Maps-Kontocenter](https://www.bingmapsportal.com/)abgerufen werden können.
    
    > **WICHTIG**
      <br/> Beachten Sie, dass Sie für die Einhaltung von Bedingungen, die von den Bing Maps-Schlüssel und alle erforderlichen Angaben für Ihre Verwendung gelten für Benutzer von der Anwendung verantwortlich sind Bezug Daten an den Bing Maps-Dienst weitergeleitet.
- Visual Studio 2010.
    
  

## Codebeispiel: Programmgesteuertes Hinzufügen eine Geolocation-Spalte zu einer Liste
<a name="SP15addgeo_addcolumn"> </a>

Gehen folgendermaßen Sie vor, um die Geolocation-Spalte zu einer Liste mithilfe des Clientobjektmodells SharePoint 2013 hinzuzufügen.
  
    
    

### So fügen Sie der Geolocation-Spalte zu einer Liste mithilfe des Clientobjektmodells hinzu


1. Starten Sie Visual Studio.
    
  
2. Wählen Sie auf der Menüleiste die Optionen Sie **Datei, neues Projekt** aus. Das Dialogfeld **Neues Projekt** wird geöffnet.
    
  
3. Klicken Sie im Dialogfeld **Neues Projekt** Wählen Sie **c#** im Feld **Installierte Vorlagen**, und wählen Sie dann die Vorlage **Konsolenanwendung** aus.
    
  
4. Benennen Sie dem Projekt, und wählen Sie dann auf die Schaltfläche **OK**.
    
  
5. Visual Studio erstellt das Projekt. Fügen Sie einen Verweis auf die folgenden Assemblys hinzu, und wählen Sie **OK** aus.
    
    Microsoft.SharePoint.Client.dll
    
    Microsoft.SharePoint.Client.Runtime.dll
    
  
6. Fügen Sie eine Richtlinie **using** in der Standard-CS-Datei wie folgt.
    
     `using Microsoft.SharePoint.Client;`
    
  
7. Fügen Sie den folgenden Code, um die **Main** -Methode in der Datei.
    
  ```cs
  
class Program
    {
        static void Main(string[] args)
        {
            AddGeolocationField();
            Console.WriteLine("Location field added successfully");
        }
        private static void AddGeolocationField()
        { 
         // Replace site URL and List Title with Valid values.
            ClientContext context = new ClientContext("<Site Url>"); 
            List oList = context.Web.Lists.GetByTitle("<List Title>");
            oList.Fields.AddFieldAsXml("<Field Type='Geolocation' DisplayName='Location'/>",true, AddFieldOptions.AddToAllContentTypes);                                        
            oList.Update();
            context.ExecuteQuery();
        } 
    }
  ```

8. Ersetzen Sie < Website-Url > und < Listentitel > durch gültige Werte.
    
  
9. Legen Sie das Zielframework in den Projekteigenschaften als .NET Framework 4.0 oder 3.5, und führen Sie das Beispiel.
    
  
10. Navigieren Sie zu der Liste. Sie sollten eine Spalte mit dem Namen **Speicherort** vom Typ Geolocation in der Liste anzeigen können. Sie können nun einige Werte und erleben. Abbildung 1 zeigt den Standardspeicherort und Map-Features, die Sie erwarten können, finden in der Liste.
    
   **Abbildung 1. Übersicht der standardmäßige Standort- und Karten-features**

  

![Standardmäßiges Geolocation- und Karten-Feature](images/SP15Con_HowToAddGeolocationColumnUpdated_Fig1.png)
  

  

  

## Programmgesteuertes Hinzufügen eines Listenelements mit dem Wert des Geolocation-Feld zu einer SharePoint-Liste
<a name="SP15addgeo_addlistitem"> </a>

Nach der Geolocation wird Feld hinzugefügt, einer SharePoint-Liste der Entwickler das Listenelement programmgesteuert zur Liste hinzufügen kann. Es gibt zwei Methoden, um das Listenelement programmgesteuert hinzufügen:, indem Sie auf das Geolocation-Feld **komplexer FieldGeolocationValue**-Objekt übergeben und von Geolocation-Feld **Roh-Wert** übergeben.
  
    
    

### Methode A: übergeben Sie komplexer FieldGeolocationValue-Objekt an die Geolocation-Feld


- Die folgende Methode fügt ein Listenelement, indem Sie als ein Objekt den Geolocation-Wert übergeben.
    
  ```cs
  
  private void AddListItem()
        {   // Replace site URL and List Title with Valid values.
            ClientContext context = new ClientContext("<Site Url>");
            List oList = context.Web.Lists.GetByTitle("<List Name>");

            ListItemCreationInformation itemCreationInfo = new ListItemCreationInformation();
            ListItem oListItem = oList.AddItem(itemCreationInfo);

            oListItem["Title"] = "New Title";

            FieldGeolocationValue oGeolocationValue = new FieldGeolocationValue();
            oGeolocationValue.Latitude = (double)17.4;
            oGeolocationValue.Longitude = (double)78.4;
            oListItem["location"] = oGeolocationValue;

            oListItem.Update();
            context.ExecuteQuery();
        }

  ```


### Methode B: übergeben Sie unformatierten Wert an die Geolocation-Feld


- Die folgende Methode fügt ein Listenelement zur SharePoint-Liste, indem Sie auf das Geolocation-Feld unformatierte Werte übergeben.
    
  ```cs
  
  private void AddListItem()
        {   // Replace site URL and List Title with Valid values.
            ClientContext context = new ClientContext("<Site Url>");
            List oList = context.Web.Lists.GetByTitle("<List Name>");

            ListItemCreationInformation itemCreationInfo = new ListItemCreationInformation();
            ListItem oListItem = oList.AddItem(itemCreationInfo);

            oListItem["Title"] = "New Title";
             // Data in WKT (World Known Text) format.
            oListItem["location"] = "POINT (78.4 17.4)" ; 

            oListItem.Update();
            context.ExecuteQuery();
        }

  ```


## Zusätzliche Ressourcen
<a name="SP15addgeo_addlresources"> </a>


-  [Integrieren von Standort- und Kartenfunktionen in SharePoint 2013](integrating-location-and-map-functionality-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Legen Sie die Bing Maps-Taste auf Ordnerebene Web und Farm in SharePoint 2013](how-to-set-the-bing-maps-key-at-the-web-and-farm-level-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: erweitern den Geolocation-Feldtyp verwenden clientseitiges Rendering](how-to-extend-the-geolocation-field-type-using-client-side-rendering.md)
    
  
-  [Erstellen einer Kartenansicht für Geolocation-Feld in SharePoint 2013](create-a-map-view-for-the-geolocation-field-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Integrieren von Zuordnungen in Windows Phone-Anwendungen und SharePoint 2013 aufgelistet](how-to-integrate-maps-with-windows-phone-apps-and-sharepoint-2013-lists.md)
    
  
-  [Verwenden des SharePoint 2013-standortfeldtyps in mobilen Anwendungen](http://technet.microsoft.com/en-us/library/fp161355%28v=office.15%29.aspx)
    
  
