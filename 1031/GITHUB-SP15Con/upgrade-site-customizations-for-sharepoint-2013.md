---
title: Aktualisieren von Anpassungen für SharePoint 2013-Websites
ms.prod: SHAREPOINT
ms.assetid: 4b515860-69ae-4af8-8013-cd071c0ddca6
---


# Aktualisieren von Anpassungen für SharePoint 2013-Websites
Informationen zu potenziellen Problemen und Empfehlungen für das Upgrade Ihrer SharePoint 2010 websiteanpassungen auf SharePoint 2013 abrufen.
SharePoint 2013 führt wurden keine bedeutende Änderungen in der Benutzeroberfläche (UI), mit die Sie Websites mithilfe von schneller und flexiblere Komponenten anpassen kann. Aber wenn Sie von SharePoint 2010 aktualisieren, treten möglicherweise einige Probleme mit den neuen Komponenten, die in SharePoint 2013 zum Verarbeiten der Verbesserungen der Benutzeroberfläche erstellt wurden.
  
    
    

Dieser Artikel hilft SharePoint-Entwickler und die Verantwortlichen für die Verwaltung des Upgradeprozesses von SharePoint 2010 zur Identifizierung potenzieller Probleme nach dem Upgrade.
## Allgemeine Empfehlungen für das Upgrade

Im Allgemeinen wird empfohlen, dass Sie eine neue "Test" Website erstellen, können Sie Ihre Anpassungen testen und sie für eine Umgebung SharePoint 2013 Umgestalten. Weitere Informationen zum Erstellen einer auswertungswebsitesammlung finden Sie unter  [Upgrade einer Websitesammlung](http://office.microsoft.com/en-us/office365-sharepoint-online-enterprise-help/upgrade-a-site-collection-HA102865473.aspx?CTT=5&amp;origin=HA104034491).
  
    
    

### Aktualisieren von benutzerdefinierten Komponenten

Tabelle 1 werden die möglichen Vorgängen Upgrade Probleme für benutzerdefinierte Komponenten und Informationen zu deren Behebung aufgelistet.
  
    
    

**In Tabelle 1. Durchführen eines Upgrades auf SharePoint 2013 auswirkt benutzerdefinierten Komponenten**


|**Wenn Sie angepasst haben**|**Auftreten können**|
|:-----|:-----|
|CSS-Formate <br/> |Während des Upgrades wird Ihre Website auf die CSS-Dateien in die Seite default.master genannten zurückgesetzt. Daher ändert sich Anpassungen, die Sie an die Darstellung der Seiten vorgenommen haben, und Seiten mit der Standardarten gefunden in einer Standardinstallation rendert. <br/> |
|Designs <br/> |Für SharePoint 2010 können Sie benutzerdefinierte Designs erstellt haben, die eine corporate branding für eine vollständige Website definieren. Sie erstellen diese als thmx-Dateien, die in der Websitesammlung hochgeladen und vom Administrator angewendet werden können. <br/> In SharePoint 2013 wurde die Designmodul vollständig flexibler und leistungsfähiger sein und für zukünftige Updates einfacher bereitstellen überarbeitet. Als Ergebnis dieser Umgestalten Aktualisieren von SharePoint 2010 auf SharePoint 2013 Designs wird nicht unterstützt und erfordert, dass Sie diese SharePoint 2013 neu erstellen. <br/> |
|Webvorlagen <br/> |Webvorlagen wurden in SharePoint 2010 zu bieten die Möglichkeit, Paket Features, Layouts, Stile und anderer Komponenten, die Sie dann verwenden können, erstellen Sie neue Webs eingeführt. Sie sind ähnlich wie in Funktion, die beim Erstellen von Websitedefinitionen, aber mit Webvorlagen, können Sie Veröffentlichungsfeatures hinzufügen. <br/> Aufgrund der Änderungen in SharePoint 2013 funktionieren Ihre benutzerdefinierte Webvorlagen nicht unmittelbar nach dem Upgrade. <br/> Um festzustellen, welche Änderungen an Webvorlagen vorgenommen wurden und dadurch Probleme zu beheben, finden Sie unter  [Aktualisieren von Vorlagen für SharePoint 2013](upgrade-web-templates-for-sharepoint-2013.md) Empfehlungen und bewährte Methoden. <br/> |
|Gestaltungsvorlagen <br/> |Während des Upgrades werden alle Verweise auf benutzerdefinierte Masterseiten, die Sie erstellt haben, wieder auf der Seite default.master zurückgesetzt. Dies kann Fehler aufgrund von Verweisen auf die benutzerdefinierten Seite auf fehlenden Komponenten in SharePoint führen. <br/> Um dieses Problem zu beheben, müssen Sie die Verweise auf alle benutzerdefinierten Gestaltungsvorlagen ändern. <br/> |
   

## Weitere Ressourcen
<a name="bk_addresources"> </a>


-  [Planen eines Upgrades eine Websitesammlung](https://technet.microsoft.com/en-us/library/ff191199.aspx)
    
  
-  [Branding-Probleme, die beim Upgrade auf SharePoint 2013 auftreten können](http://office.microsoft.com/en-us/office365-sharepoint-online-enterprise-help/branding-issues-that-may-occur-when-upgrading-to-sharepoint-2013-HA104052656.aspx?CTT=5&amp;origin=HA104034491)
    
  
-  [Upgrade einer Websitesammlung](http://office.microsoft.com/en-us/office365-sharepoint-online-enterprise-help/upgrade-a-site-collection-HA102865473.aspx?CTT=5&amp;origin=HA104034491)
    
  
-  [Upgrade von Websitesammlungen Troubleshoot Probleme](http://office.microsoft.com/en-us/office365-sharepoint-online-enterprise-help/troubleshoot-site-collection-upgrade-issues-HA104037311.aspx?CTT=5&amp;origin=HA104034491)
    
  

