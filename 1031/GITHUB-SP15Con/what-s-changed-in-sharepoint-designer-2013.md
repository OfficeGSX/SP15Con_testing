---
title: Änderungen in SharePoint Designer 2013
ms.prod: SHAREPOINT
ms.assetid: f1cd30c9-9a73-428d-9151-f1813b43b020
---


# Änderungen in SharePoint Designer 2013
Hier erhalten Sie Informationen zu Features, die veraltet sind oder aus SharePoint Designer 2013 entfernt wurden. Veraltete Features sind in SharePoint 2013 aus Kompatibilitätsgründen mit der vorherigen Produktversion weiterhin vorhanden, werden aber in zukünftigen Versionen entfernt.
## Eingestellte Features in SharePoint Designer 2013
<a name="WhatsChangedSharePointDesigner2013_DiscontinuedFeatures"> </a>

Die folgenden Features wurden in SharePoint Designer 2013 eingestellt oder entfernt.
  
    
    

### SharePoint 2010 Workflow-Plattform
<a name="WhatsChangedSharePointDesigner2013_WorkflowPlatform"> </a>

 **Beschreibung der Änderung.** Einige Features der SharePoint 2010-Workflow-Plattform, die von der Windows Workflow Foundation 3.0 abhängig sind, wurden in SharePoint 2013 eingestellt.
  
    
    
 **Änderungsgrund.** SharePoint 2013 führt eine neue SharePoint 2013-Workflow-Plattform ein, die auf der Windows Workflow Foundation 4.0 aufsetzt und mit Workflow-Manager 1.0 integriert wird.
  
    
    
 **Migrationspfad.** In SharePoint Designer 2013 können Sie weiterhin einen SharePoint 2010-Workflow erstellen und alle Workflow-Features von SharePoint 2010 verwenden, indem Sie die SharePoint 2010-Workflow-Plattform auswählen.
  
    
    
Sie können auch Features der SharePoint 2010-Workflow-Plattform in die neue SharePoint 2013-Workflow-Plattform integrieren. Erstellen Sie dazu einen SharePoint 2010-Workflow, indem Sie die SharePoint 2010-Workflow-Plattform auswählen. Erstellen Sie einen SharePoint 2013-Workflow, indem Sie die SharePoint 2013-Workflow-Plattform auswählen. Dann verwenden Sie die Aktionen **Listenworkflow starten** und **Website-Workflow starten** im SharePoint 2013-Workflow, um den SharePoint 2010-Workflow aufzurufen.
  
    
    
Die folgenden Features stehen nur für die SharePoint 2010-Workflow-Plattform zur Verfügung:
  
    
    

- Aktionen:
    
  - Workflow beenden
    
  
  - Version der Dokumentenmappe erfassen
    
  
  - Dokumentenmappe an Repository senden
    
  
  - Inhaltsgenehmigungsstatus für die Dokumentenmappe festlegen
    
  
  - Genehmigungsvorgang für Dokumentenmappen starten
    
  
  - Datensatz deklarieren
    
  
  - Status für die Genehmigung von Inhalten festlegen
    
  
  - Deklaration des Datensatzes aufheben
    
  
  - Listenelement hinzufügen 
    
  
  - Übergeordnete Listenelementberechtigungen erben
    
  
  - Listenelementberechtigungen entfernen
    
  
  - Listenelementberechtigungen ersetzen
    
  
  - Vorgesetzten eines Benutzers nachschlagen
    
  
  - Formular einer Gruppe zuordnen
    
  
  - Aufgabe zuordnen
    
  
  - Daten von einem Benutzer sammeln
    
  
  - Genehmigungsvorgang starten
    
  
  - Benutzerdefinierten Aufgabenvorgang starten
    
  
  - Feedbackprozess starten
    
  
  - Listenelement kopieren (SharePoint Designer 2013 unterstützt nur das Kopieren des Dokuments)
    
  
- Bedingungen:
    
  - Wenn das aktuelle Elementfeld gleich Wert ist
    
  
  - Listenelement-Berechtigungsstufen überprüfen
    
  
  - Listenelementberechtigungen überprüfen
    
  
- Schritte:
    
  - Identitätswechselschritt:
    
  
- Datenquellen:
    
  - Benutzerprofilsuche
    
  
- Sonstige Features:
    
  - Integration von Visio
    
  
  - Zuordnungsspalte
    
  
  - Inhaltstypzuordnung für wiederverwendbare Workflows
    
  
  - Feature "Berechtigungen zum Verwalten von Listen/Websites anfordern" für Listen-/Website-Workflow
    
  
  - Global wiederverwendbarer Workflowtyp
    
  
  - Option "Workflowvisualisierung"
    
  

### SharePoint-Features für den Seitenentwurf
<a name="WhatsChangedSharePointDesigner2013_PageDesignFeatures"> </a>

Das folgende Feature ist in SharePoint 2013 nicht verfügbar.
  
    
    

#### Entwurfsansicht und geteilte Ansicht
<a name="WhatsChangedSharePointDesigner2013_DesignViewSplitView"> </a>

 **Beschreibung der Änderung.** SharePoint Designer 2010 verfügt über drei Ansichten zum Bearbeiten von HTML- und ASPX-Seiten: Codeansicht, Entwurfsansicht und geteilte Ansicht. Die Entwurfsansicht und geteilte Ansicht wurden in SharePoint Designer 2013 entfernt. Das Entfernen der Entwurfsansicht und der geteilten Ansicht wirkt sich auf die Features von SharePoint Designer 2013 aus, die zum Bearbeiten von Webparts und Gestaltungsvorlagen verwendet werden. Wenn Sie Seiten in SharePoint Designer 2013 bearbeiten, müssen Sie die Codeansicht verwenden.
  
    
    
 **Änderungsgrund.** Im Vergleich zu aktuellen Versionen von Internet Explorer ist die Entwurfsansicht eine ältere Technologie, die viele neue HTML5- und CSS-Tags nicht unterstützt.
  
    
    
 **Migrationspfad.** Wenn Sie Seiten in der Codeansicht bearbeiten, können Sie **F12** drücken, um eine Vorschau der Seite im Browser anzuzeigen. Alternativ können Sie Visual Studio verwenden, um Seiten zu bearbeiten.
  
    
    
Wenn Sie Ihre Website visuell entwerfen oder kennzeichnen möchten, und eine WYSIWYG-Seitenbearbeitung ("What You See Is What You Get") wünschen, können Sie einen beliebigen professionellen HTML-Editor wie Microsoft Expression Web verwenden. Dann können Sie Ihre HTML-Dateien mithilfe des Entwurfs-Managers in SharePoint Server 2013 importieren. Der Entwurfs-Manager ist ein beim Veröffentlichen von Websites einbezogenes Feature wie die Websitevorlage zur Veröffentlichung der Portalwebsitesammlung.
  
    
    

## Zusätzliche Ressourcen
<a name="WhatsChangedSharePointDesigner2013_AdditionalResources"> </a>


-  [Kurzübersicht zu Workflowaktionen (SharePoint 2013-Workflowplattform)](workflow-actions-quick-reference-sharepoint-2013-workflow-platform.md)
    
  
-  [Änderungen zwischen SharePoint 2010 und SharePoint 2013](http://technet.microsoft.com/de-de/library/ff607742%28v=office.15%29.aspx)
    
  
-  [Neuigkeiten im Workflow in SharePoint Server 2013](http://technet.microsoft.com/de-de/library/jj219638%28v=office.15%29.aspx)
    
  

  
    
    

