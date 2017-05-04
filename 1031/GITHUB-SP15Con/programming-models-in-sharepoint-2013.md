---
title: Programmiermodelle in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 061985ec-6129-4e91-991b-a72488ce1d34
---



# Programmiermodelle in SharePoint 2013
In diesem Artikel erhalten Sie einen schnellen Überblick über die unterschiedlichen SharePoint-Entwicklungsprojekte.
  
    
    


|||
|:-----|:-----|
|**In diesem Artikel**<br/>          [Add-Ins für SharePoint](#Apps)<br/>           [SharePoint-Veröffentlichungssites](#ECM)<br/>           [SharePoint-Farmlösungen](#Solutions)<br/>           [Mobile Add-Ins für SharePoint](#Mobile)<br/>           [Wiederverwendbare Komponenten für SharePoint](#Reuse)<br/>           [Zusätzliche Ressourcen](#SP15devinSP_addlresources)|**Video anschauen: Entwicklungsoptionen für Office 2013 und SharePoint 2013**<br/>![Videos](images/mod_icon_video.png)<br/>|
   

Sie können auf vielfältige Weise Anwendungen für die SharePoint 2013-Plattform entwickeln. Diese Anwendungen können - je nach deren Erstellungstools, nach den verwendeten Programmierungsmodellen, nach dem Methoden der Paketerstellung und Bereitstellung, den Methoden der Vermarktung und schließlich nach den Geräten, auf denen sie ausgeführt werden - sinnvoll in folgenden Gruppen eingeteilt werden.
  
    
    


- SharePoint-Add-Ins
    
  
- SharePoint-Veröffentlichungssites
    
  
- SharePoint-Farmlösungen
    
  
- Mobile Add-Ins für SharePoint
    
  
- Wiederverwendbare Komponenten für SharePoint
    
  
Diese Kategorien schließen sich gegenseitig  *nicht*  aus. Beispiel: Sie entwickeln eine Veröffentlichungssite als eine SharePoint-Add-In. In den folgenden Abschnitten werden diese Kategorien erläutert, und Sie werden jeweils durch die Dokumentation geführt.
## Add-Ins für SharePoint
<a name="Apps"> </a>

Ein SharePoint-Add-In entspricht einem Add-In auf einem mobilen Gerät. Es ist eine eigenständige Produktivitätslösung, die eine Reihe von verbundenen Aufgaben ausführt, leicht zu installieren ist und sauber deinstalliert werden kann. Benutzer finden SharePoint-Add-Ins in einem öffentlichen SharePoint-Add-In Store oder im Add-In-Katalog ihres Unternehmens und können sie dort auch herunterladen. Ein SharePoint-Add-In kann die üblichen SharePoint-Komponenten beinhalten, wie Listen, benutzerdefinierte Websiteseiten, Webparts, Workflows und Inhaltstypen. Aber ein SharePoint-Add-In kann auch eine Remote-Webanwendung und Remote-Daten in SharePoint darstellen. Ein SharePoint-Add-In kann ebenfalls sowohl SharePoint- als auch Remote-Komponenten enthalten. SharePoint-Add-Ins sind sehr sichere Anwendungen, deren benutzerdefinierte Logik immer in die Cloud oder auf Clientcomputer verschoben wird. Sie werden nie auf SharePoint-Servern ausgeführt.
  
    
    
Eine Einführung in Modell für SharePoint-Add-Ins finden Sie unter  [SharePoint-Add-Ins](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx).Weitere Informationen finden Sie unter  [SharePoint-Add-Ins im Vergleich zu SharePoint-Lösungen](sharepoint-add-ins-compared-with-sharepoint-solutions.md), and  [Auswählen des richtigen API-Satzes in SharePoint 2013](choose-the-right-api-set-in-sharepoint-2013.md).
  
    
    

## SharePoint-Veröffentlichungssites
<a name="ECM"> </a>

SharePoint-Veröffentlichungssites bieten reichhaltige Inhaltsveröffentlichung mit einem hohen Maß an Wartbarkeit und Regelkonformität. Sie stellen außerdem Dokument-, Datensatz, Taxonomie- und Inhaltstypverwaltung bereit. Weitere Informationen finden Sie unter  [Erstellen von Websites für SharePoint](build-sites-for-sharepoint.md).
  
    
    

## SharePoint-Farmlösungen
<a name="Solutions"> </a>

SharePoint Farmlösungen sind vertrauenswürdige SharePoint-Erweiterungen, deren benutzerdefinierte Logik das SharePoint-Serverobjektmodell mit vollem Vertrauen in die SharePoint-Server aufruft und ausführt. Diese Lösungen dienen in erster Linie benutzerdefinierten administrativen SharePoint-Erweiterungen, wie Zeitgeberaufträge, benutzerdefinierte Windows PowerShell-Befehle und Erweiterungen der Zentraladministration. Farmlösungen werden als SharePoint-Lösungspakete bereitgestellt, die Farmadministratoren an einen farmübergreifende Speicherort laden und von dort aus bereitstellen. Komponenten in Farmlösungen können in der Farm, in der Webanwendung, in der Websitesammlung oder auf der Website gelten. Weitere Informationen finden Sie unter  [Erstellen von Farmlösungen in SharePoint 2013](build-farm-solutions-in-sharepoint-2013.md).
  
    
    

## Mobile Add-Ins für SharePoint
<a name="Mobile"> </a>

Windows Phone-Apps und Apps, die auf mobilen Nicht-Microsoft-Plattformen basieren, können auf SharePoint-Websites und -Daten zugreifen. Tools zum Erstellen von Windows Phone-Apps, die mit SharePoint 2013 interagieren, sind für eine Installation unter Visual Studio 2010 (und Visual Studio 2012) verfügbar. Eine clientverwaltete SharePoint-API für eine Verwendung auf Windows Phone-Geräten ist erhältlich. Mobile Geräte, einschließlich Nicht-Microsoft-Geräte, können ebenfalls über REST/OData-Endpunkte auf SharePoint-Daten zugreifen. Weitere Informationen finden Sie unter  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md).
  
    
    

## Wiederverwendbare Komponenten für SharePoint
<a name="Reuse"> </a>

Die SharePoint 2013-Plattform und Visual Studio 2012 ermöglichen eine Kapselung und Wiederverwendung von Anwendungselementen, auch derjenigen Elemente, die mit Code, Skripten und XML-Markup erstellt wurden. Weitere Informationen finden Sie unter  [Erstellen von wiederverwendbaren Komponenten für SharePoint 2013](build-reusable-components-for-sharepoint-2013.md).
  
    
    

## In diesem Abschnitt
<a name="Reuse"> </a>


-  [Erstellen von Websites für SharePoint](build-sites-for-sharepoint.md)
    
  
-  [Erstellen von Farmlösungen in SharePoint 2013](build-farm-solutions-in-sharepoint-2013.md)
    
  
-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Erstellen von wiederverwendbaren Komponenten für SharePoint 2013](build-reusable-components-for-sharepoint-2013.md)
    
  

## Zusätzliche Ressourcen
<a name="SP15devinSP_addlresources"> </a>


-  [Einrichten einer allgemeinen Entwicklungsumgebung für SharePoint 2013](set-up-a-general-development-environment-for-sharepoint-2013.md)
    
  
-  [Hinzufügen von SharePoint 2013-Funktionen](add-sharepoint-2013-capabilities.md)
    
  
-  [Barrierefreiheit in SharePoint 2013](accessibility-in-sharepoint-2013.md)
    
  
