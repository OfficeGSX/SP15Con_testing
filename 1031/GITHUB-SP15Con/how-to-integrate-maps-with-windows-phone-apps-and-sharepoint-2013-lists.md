---
title: Vorgehensweise Integrieren von Zuordnungen in Windows Phone-Anwendungen und SharePoint 2013 aufgelistet
ms.prod: SHAREPOINT
ms.assetid: 7e0550bc-92d1-407f-b8ba-1371c63bd16e
---


# Vorgehensweise: Integrieren von Zuordnungen in Windows Phone-Anwendungen und SharePoint 2013 aufgelistet
In diesem Artikel erfahren Sie, wie Sie Standortinformationen und Karten in SharePoint-Listen sowie standortbasierte Web-Apps und mobile SharePoint-Add-Ins mithilfe des neuen Felds "Geolocation" und durch Erstellen Ihrer eigenen auf dem Geolocation-Feld basierenden Feldtypen integrieren.
SharePoint 2013 führt einen neuen Feldtyp namens Geolocation, die Ihnen das Hinzufügen von SharePoint-Listen mit Standortinformationen Anmerkungen ermöglicht. In Spalten vom Typ Geolocation können Sie Standortinformationen als ein Paar von Breiten- und Längengrad Koordinaten in decimal Grad eingeben oder die Koordinaten des aktuellen Standort des Benutzers vom Browser, abrufen, wenn im Browser die W3C Geolocation-API implementiert. Klicken Sie in der Liste zeigt SharePoint 2013 den Speicherort auf einer Karte unterstützt von Bing Maps. Zusammen Geolocation-Feld Kartenansicht ermöglichen es Ihnen, einen räumlichen Kontext für alle Informationen bereitgestellt, durch die Integration von Daten aus SharePoint in einer Zuordnung und können Benutzer ausschließlich auf neue Weise in den Webservern und mobilen apps und -Lösungen zu. Wir helfen Ihnen eine einfache Windows 7-app erstellen, die SharePoint 2013 Geolocation-Feld Typ-Funktion verwendet, um Funktionen für die Zuordnung zu verwenden, sodass Sie Karten für mobile SharePoint-Add-In Listenelemente anzeigen können.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie anstelle von Visual Studio 2010 Express Visual Studio Express 2012 verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung Erstellen von apps für Windows Phone 8 und Windows Phone 7.> Weitere Informationen finden Sie unter  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## Voraussetzungen für die Erstellung einer Zuordnung-basierten Windows Phone-app
<a name="SP15Integratemaps_prereeq"> </a>

Stellen Sie sicher, dass Folgendes installiert sein:
  
    
    

- SharePoint 2013
    
  
- Visual Studio 2012
    
  
- Visual Studio Express 2010 mit den SharePoint-phonevorlagen neu aus Microsoft SharePoint SDK for Windows Phone 7.1http://www.microsoft.com/en-us/download/details.aspx?id=30476 []()
    
  
- Zugriff auf eine Liste SharePoint 2013 mit ausreichenden Berechtigungen zum Hinzufügen einer Spalte
    
  
- Die Bing Maps-Schlüssel bereitgestellt, die auf Ihrem Server; finden Sie unter  [Vorgehensweise: Legen Sie die Bing Maps-Taste auf Ordnerebene Web und Farm in SharePoint 2013](how-to-set-the-bing-maps-key-at-the-web-and-farm-level-in-sharepoint-2013.md)
    
  

## Schritt 1: Erstellen eines SharePoint-Felds mithilfe des Geolocation-Features
<a name="HowToCreateMapBasedPhoneApp_Step1"> </a>

Geolocation-Spalte ist nicht verfügbar, die standardmäßig in SharePoint-Listen. Sie müssen zum Schreiben von Code zum Hinzufügen der Spalte zu einer SharePoint-Liste. Wir zeigen Sie Gewusst wie: Programmgesteuertes Hinzufügen von Geolocation-Feld zu einer Liste mithilfe des SharePoint-Clientobjektmodells. Nachdem Sie das Feld zur Liste hinzufügen, können Sie der Liste Geolocation-Feld als ein Feature hinzufügen.
  
    
    

### Visual Studio-Projekt erstellen


1. Melden Sie sich als Administrator auf dem Server mit SharePoint 2013.
    
  
2. Starten Sie **Visual Studio**, und wählen Sie **Datei**, **Neues Projekt**. Das Dialogfeld **Neues Projekt** wird geöffnet.
    
  
3. Klicken Sie im Dialogfeld **Neues Projekt** wählen Sie **Visual c#**, wählen Sie **SharePoint 2013**, und wählen Sie dann den Projekttyp **SharePoint 2013**.
    
  
4. Benennen Sie das Projekt. In diesem Beispiel verwenden wir **GeoList**. Wählen Sie die Schaltfläche **OK**.
    
  
5. Geben Sie im **Assistenten zum Anpassen von SharePoint** die URL für die Websitesammlung, die die gleiche SharePoint-Liste verwendet, die Sie für die Telefonentwicklung zugreifen möchten.
    
  
6. Klicken Sie im **Projektmappen-Explorer** öffnen Sie das Kontextmenü für das Projekt **GeoList**, und wählen Sie **Hinzufügen** und **Neues Element**.
    
  
7. Wählen Sie im Dialogfeld **Neues Element hinzufügen** die **Liste**. Name der Liste. In diesem Beispiel verwenden wir **ServiceCalls**.
    
  
8. Fügen Sie im Dialogfeld **Einstellungen für die Liste Wählen Sie** einen Anzeigenamen ein. In diesem Beispiel verwenden wir **-Dienstaufrufen**. Wählen Sie für **zum Anpassen der Liste basierend auf auswählen** in der **Standardeinstellung (leer)** wie in Abbildung 1 dargestellt.
    
    Wählen Sie dann **Fertig stellen**.
    

   **Abbildung 1. Hinzufügen der SharePoint-Liste mithilfe des Assistenten für SharePoint-Liste**

  

![Hinzufügen der SharePoint-Liste mithilfe des Assistenten](images/SP15Con_HowToCreateMapBasedPhoneApp_Fig1.png)
  

  

  

### So fügen Sie ein Feature zur SharePoint-Liste hinzu


1. Klicken Sie im **Projektmappen-Explorer** und klicken Sie dann den Knoten **Features**.
    
  
2. Öffnen Sie das Kontextmenü des Knotens **Feature1**, und wählen Sie **Hinzufügen** und **Ereignisempfänger hinzufügen**.
    
  
3. Kommentieren Sie die **FeatureActivated** -Methode und die **FeatureDeactivating** -Methode, und fügen Sie den folgenden Code.
    
  ```cs
  
public override void FeatureActivated(SPFeatureReceiverProperties properties)
{
    SPWeb site = properties.Feature.Parent as SPWeb;
    SPList list = site.Lists.TryGetList("Service Calls");
    if (list != null)
    {
        list.Fields.AddFieldAsXml(
            "<Field Type='Geolocation' DisplayName='Location'/>", 
            true, 
            SPAddFieldOptions.Default);
        list.Update();
    }
}
public override void FeatureDeactivating(
                     SPFeatureReceiverProperties properties)
{
    SPWeb site = properties.Feature.Parent as SPWeb;
    SPList list = site.Lists.TryGetList("Service Calls");
    if (list != null)
    {
        list.Delete();
    }
}
  ```

4. Erstellen Sie die Projektmappe, indem Sie auf die F6-Taste.
    
  

## Schritt 2: Bereitstellen der Liste, und geben Sie Daten in der SharePoint-Liste speicherortbasierte
<a name="HowToCreateMapBasedPhoneApp_Step2"> </a>

In diesem Schritt für die neu erstellte Liste Visual Studio bereitstellen und verwenden Sie das neue Feld Speicherort in SharePoint.
  
    
    

### Zum Bereitstellen von SharePoint-Liste


- Klicken Sie im **Projektmappen-Explorer** öffnen Sie das Kontextmenü für das Projekt **GeoList**, und wählen Sie **Bereitstellen**.
    
  

### Daten in die neue SharePoint-Liste mit den Geolocation-Feld eingeben.


1. Nachdem die Liste erfolgreich bereitgestellt wurde, öffnen Sie die Website für die Telefonentwicklung verwendeten.
    
  
2. Wählen Sie auf **Weitere**, und wählen Sie die Liste **-Dienstaufrufen** aus.
    
  
3. Wählen Sie **Neues Element hinzufügen**.
    
  
4. Geben Sie einen Titel für das Feld **Titel** ein. Verwenden Sie für dieses Beispiel **Neues Geolocation-Element** ein.
    
  
5. Wählen Sie **Verwenden den aktuellen Speicherort** im Feld **Speicherort**, oder Sie können wählen Sie **Den Speicherort angeben**, und geben Sie Werte für **Länge** und **Breite**.
    
  
6. Wählen Sie **Speichern** aus.
    
  

## Schritt 3: Erstellen einer Phone-app für die Liste speicherortbasierte
<a name="HowToCreateMapBasedPhoneApp_Step3"> </a>

In diesem Schritt erstellen Sie eine Phone-app, die die SharePoint-Liste verwendet, die Sie zuvor in Schritt 1 und Schritt 2 erstellt haben.
  
    
    

1. Melden Sie sich an das Telefon Entwicklungsumgebung auf dem Client.
    
  
2. Starten Sie Visual Studio 2010 Express mit den neuen SharePoint-Vorlagen.
    
  
3. Klicken Sie in der Menüleiste auf **Datei** und dann auf **Neues Projekt**.
    
    Das Dialogfeld **Neues Projekt** wird geöffnet.
    
  
4. Klicken Sie im Dialogfeld **Neues Projekt** die Option **Visual c#**, **Silverlight für Windows Phone**, **Windows Phone SharePoint List Application**.
    
  
5. Benennen Sie das Projekt. In diesem Beispiel verwenden wir GeoApp. Wählen Sie die Schaltfläche **OK**.
    
  
6. Geben Sie im **Assistenten für SharePoint Phone-Anwendung** die URL der SharePoint-Website, in dem Sie die Liste in Schritt2 **bereitgestellt haben. Bereitstellen die Liste, und geben Sie Daten in der SharePoint-Liste speicherortbasierte**, und wählen Sie dann auf **Suchen enthält**.
    
  
7. Wählen Sie die **Service-Aufrufe** aus, und wählen Sie dann auf **Weiter**.
    
  
8. Klicken Sie auf der Seite **Ansichten auswählen** wählen Sie **Alle Elemente**, und wählen Sie dann auf **Weiter**.
    
  
9. **Wählen Sie** auf der Seite **Vorgänge auswählen**, und wählen Sie dann auf **Weiter**.
    
  
10. Klicken Sie auf der Seite **Wählen Sie Felder** wählen Sie das Feld, auf Ihre Phone-app angezeigt werden soll, und wählen Sie dann auf **Weiter**.
    
  
11. Ordnen Sie auf der Seite **Reihenfolge Felder** die Felder neu an, wie Sie benötigen, und wählen Sie dann auf **Fertig stellen**.
    
  

## Schritt 4: Testen Sie und überprüfen Sie Ihre app
<a name="HowToCreateMapBasedPhoneApp_Step4"> </a>

In diesem Schritt können Sie Ihre app ausführen und zu überprüfen.
  
    
    

1. Wählen Sie in Visual Studio zu **Debuggen**, **Debuggen starten**.
    
  
2. Wenn Sie aufgefordert werden, melden Sie sich, mit den Anmeldeinformationen, die auf dem Server mit SharePoint Administratorrechte verfügen.
    
  
3. In diesem Beispiel wird wählen Sie in den ersten Eintrag, **Brian Cox**.
    
  
4. Wählen Sie den **Map It**-Link im Feld **Speicherort** gefunden.
    
  
5. Wählen Sie auf dem **Zulassen Maps zugreifen und diese verwenden Ihres Standorts für Ihre** Privatsphäre Richtlinie Bildschirm, **Zulassen**, wie in Abbildung 2 dargestellt.
    
   **Abbildung 2. Mobile app-Anforderung an Ihren jeweiligen Standort zugreifen**

  

![Datenschutzrichtlinie für Karten zulassen](images/SP15Con_HowToCreateMapBasedPhoneApp_Fig2.png)
  

Kartenansicht wird angezeigt, wie in Abbildung 3 dargestellt.
    

   **Abbildung 3. Mobile App Anzeigeort Bing Map**

  

![Kartenansicht in mobiler App](images/SP15Con_HowToCreateMapBasedPhoneApp_Fig3.png)
  

  

  

> **HINWEIS**
> Die Benutzeroberfläche des Geolocation-Felds kann auf mobilen Geräten als in Browsern abweichen. Die Option **Mit bestimmten Speicherort** im Browser, ist nicht verfügbar für mobile Geräte. Ist nur eine Option für mobile Geräte verfügbar: **Mein Standort verwenden**.
  
    
    


## Zusätzliche Ressourcen
<a name="SP15Integmaps_addlresources"> </a>


-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Integrieren von Standort- und Kartenfunktionen in SharePoint 2013](integrating-location-and-map-functionality-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: erweitern den Geolocation-Feldtyp verwenden clientseitiges Rendering](how-to-extend-the-geolocation-field-type-using-client-side-rendering.md)
    
  
-  [Vorgehensweise: Hinzufügen einer Geolocation-Spalte einer Liste in SharePoint 2013 programmgesteuert](how-to-add-a-geolocation-column-to-a-list-programmatically-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  

