---
title: Bewährte Methoden für die SharePoint-Workflowentwicklung
ms.prod: SHAREPOINT
ms.assetid: 63d9c867-0c5e-4f89-bc61-eeefb0320844
---


# Bewährte Methoden für die SharePoint-Workflowentwicklung
Stellt eine Sammlung von bewährten Methoden für Entwickler bereit, die Visual Studio zum Erstellen von Workflows in SharePoint 2013 verwenden.
## Bewährte Methoden für die Workflowentwicklung

Für die Entwicklung fehlerfreier Workflows für SharePoint ist es empfehlenswert, einige allgemeine Richtlinien oder „bewährte Methoden" zu befolgen, unabhängig davon, ob Sie SharePoint Designer 2013 oder Visual Studio 2012 für die Workflowentwicklung verwenden.
  
    
    

-  [Apps für SharePoint, die integrierte Workflows enthalten, müssen ein Tag in der Datei workflowmanifest.xml bearbeiten.](sharepoint-workflow-development-best-practices.md#bkm_00)
    
  
-  [Wenn Sie die Aktion „In Verlaufsliste protokollieren" verwenden, sind mehr Informationen besser.](sharepoint-workflow-development-best-practices.md#bkm_01)
    
  
-  [Schreiben Sie den Wert jeder Zeichenfolge und Variable, die Sie erstellen, in die Verlaufsliste.](sharepoint-workflow-development-best-practices.md#bkm_02)
    
  
-  [Geben Sie vor und nach jedem Schritt oder jeder wichtigen Arbeitseinheit im Workflow ein Ablaufverfolgungsprotokoll aus.](sharepoint-workflow-development-best-practices.md#bkm_03)
    
  
-  [Stellen Sie sicher, dass die Variablen nicht Null sind und erwartete Werte enthalten.](sharepoint-workflow-development-best-practices.md#bkm_04)
    
  
-  [Stellen Sie sicher, dass Zeichenfolgen in Textfeldern des Workflows nicht länger als 255 Zeichen sind.](sharepoint-workflow-development-best-practices.md#bkm_05)
    
  
-  [Verwenden Sie erhöhte Berechtigungen für ein neutrales Konto bei Verwendung eines Identitätswechsels.](sharepoint-workflow-development-best-practices.md#bkm_06)
    
  
-  [Verwenden Sie in wieder verwendbaren Workflows Zuordnungsspalten, um fehlerfreie Listenfelder sicherzustellen.](sharepoint-workflow-development-best-practices.md#bkm_07)
    
  
-  [Workflowdesign: Modellieren eines Geschäftsprozesses in einem einzigen Workflow](sharepoint-workflow-development-best-practices.md#bkm_08)
    
  
-  [Workflowdesign: Effektive Verwendung der Genehmigungsaktion](sharepoint-workflow-development-best-practices.md#bkm_09)
    
  

### Apps für SharePoint, die integrierte Workflows enthalten, müssen ein Tag in der Datei workflowmanifest.xml bearbeiten.
<a name="bkm_00"> </a>

SharePoint-Add-Ins mit integrierten Workflows (die Listen im übergeordneten Web zugeordnet sein können) werden von normalen Workflow-Apps unterschieden, indem das folgende Tag in der Datei  `workflowmanifest.xml` im App-Paket in **true** geändert wird:
  
    
    

```XML

<SPIntegratedWorkflow xmlns="http://schemas.microsoft.com/sharepoint/2014/app/integratedworkflow">
    <IntegratedApp>true</IntegratedApp>
</SPIntegratedWorkflow>

```


### Wenn Sie die Aktion „In Verlaufsliste protokollieren" verwenden, sind mehr Informationen besser.
<a name="bkm_01"> </a>

Die Aktion **In Verlaufsliste protokollieren** (oder die Klasse [LogToHistoryListActivity](https://msdn.microsoft.com/library/Microsoft.SharePoint.WorkflowActions.LogToHistoryListActivity.aspx) bei Verwendung von Visual Studio) ermöglicht Ihnen das Aufzeichnen von Informationen dazu, was ein Workflow zu einem bestimmten Punkt im Lebenszyklus des Workflows ausgeführt hat. Damit wird sie zu einem der wichtigsten Tools für die Problembehandlung, das Ihnen zur Verfügung steht. Je mehr Informationen Sie an wichtigen Punkten des Workflows bereitstellen, umso einfacher ist es, unerwartete Ereignisse zu behandeln.
  
    
    
Hier finden Sie weitere Informationen: 
  
    
    

-  [Workflowaktionen in SharePoint Designer 2010: eine Kurzübersicht](http://office.microsoft.com/de-de/sharepoint-designer-help/workflow-actions-in-sharepoint-designer-2010-a-quick-reference-guide-HA010376961.aspx#_Toc260924447)
    
  
-  [LogToHistoryListActivity](https://msdn.microsoft.com/library/Microsoft.SharePoint.WorkflowActions.LogToHistoryListActivity.aspx) -Klasse
    
  
-  [Gewusst wie: Protokollieren in die Verlaufsliste von einer Workflowaktivität](http://msdn.microsoft.com/de-de/library/ff798337.aspx)
    
  
-  [Verwenden der SharePoint-Workflowaktion „In Verlaufsliste protokollieren" für das Debugging](http://www.documentmanagementworkflowinfo.com/sample-sharepoint-workflows/use-log-to-history-list-sharepoint-designer-workflow-action-debug.md)
    
  

### Schreiben Sie den Wert jeder Zeichenfolge und Variable, die Sie erstellen, in die Verlaufsliste.
<a name="bkm_02"> </a>

Das Debuggen von Workflows, die mit SharePoint Designer erstellt wurden, ist viel einfacher, wenn Sie Zeichenfolgen und Variablen in mithilfe der Aktion **Log to History List** in die Verlaufsliste schreiben.
  
    
    
Hier finden Sie weitere Informationen: 
  
    
    

-  [Workflowaktionen in SharePoint Designer 2010: eine Kurzübersicht](http://office.microsoft.com/de-de/sharepoint-designer-help/workflow-actions-in-sharepoint-designer-2010-a-quick-reference-guide-HA010376961.aspx#_Toc260924447)
    
  
-  [LogToHistoryListActivity](https://msdn.microsoft.com/library/Microsoft.SharePoint.WorkflowActions.LogToHistoryListActivity.aspx) -Klasse
    
  
-  [Gewusst wie: Protokollieren in die Verlaufsliste von einer Workflowaktivität](http://msdn.microsoft.com/de-de/library/ff798337.aspx)
    
  
-  [Verwenden der SharePoint-Workflowaktion „In Verlaufsliste protokollieren" für das Debugging](http://www.documentmanagementworkflowinfo.com/sample-sharepoint-workflows/use-log-to-history-list-sharepoint-designer-workflow-action-debug.md)
    
  

### Geben Sie vor und nach jedem Schritt oder jeder wichtigen Arbeitseinheit im Workflow ein Ablaufverfolgungsprotokoll aus.
<a name="bkm_03"> </a>

Zur Unterstützung von Debuggingworkflows ist es wichtig, dass Sie vor und nach jeder wichtigen Arbeitseinheit aussagekräftige Informationen erfassen. Diese Informationen sollten an Ablaufverfolgungsprotokolle übermittelt werden. Hier finden Sie weitere Informationen:
  
    
    

-  [Schreiben an das Ablaufverfolgungsprotokoll](http://msdn.microsoft.com/de-de/library/aa979595.aspx)
    
  
-  [Verwenden von Ereignis- und Ablaufverfolgungsprotokollen in SharePoint](http://msdn.microsoft.com/de-de/library/ff647362.aspx)
    
  

### Stellen Sie sicher, dass die Variablen nicht Null sind und erwartete Werte enthalten.
<a name="bkm_04"> </a>

Stellen Sie vor der Verwendung von Variablen in Ihren Workflows sicher, dass keine Null-Variablen vorhanden sind. Stellen Sie außerdem sicher, dass Variablen erwartete Werte enthalten und dem richtigen Datentyp entsprechen. Weitere Informationen finden Sie unter  [Variablen und Argumente](http://msdn.microsoft.com/de-de/library/dd489456.aspx).
  
    
    

### Stellen Sie sicher, dass Zeichenfolgen in Textfeldern des Workflows nicht länger als 255 Zeichen sind.
<a name="bkm_05"> </a>

Die maximale zulässige Länge für Zeichenfolgen in Textfeldern von Workflows beträgt 255 Zeichen. Wenn Sie das Textfeld so festlegen, dass dieser Grenzwert überschritten wird, wird dessen Inhalt auf 255 Zeichen abgeschnitten.
  
    
    

### Verwenden Sie erhöhte Berechtigungen für ein neutrales Konto bei Verwendung eines Identitätswechsels.
<a name="bkm_06"> </a>

Wenn Sie Identitätswechselschritte in einem Workflow verwenden, sollten Sie den Workflow mit einem neutralen Konto (d. h. einem Konto, das nicht an einen bestimmten Benutzer gebunden ist) erstellen. Dadurch wird verhindert, dass Ihre Workflows unterbrochen werden, wenn das Konto des Autors aus irgendeinem Grund veraltet ist.
  
    
    
Weitere Informationen finden Sie unter  [Erstellen eines Workflows mit erweiterten Berechtigungen mithilfe der SharePoint 2013-Workflow-Plattform](create-a-workflow-with-elevated-permissions-by-using-the-sharepoint-2013-workflo.md).
  
    
    

### Verwenden Sie in wieder verwendbaren Workflows Zuordnungsspalten, um fehlerfreie Listenfelder sicherzustellen.
<a name="bkm_07"> </a>

Bei der Erstellung eines wieder verwendbaren Workflows, der darauf basiert, dass seine Liste über ein bestimmtes Feld verfügt, können Sie (1) den Workflow auf einen Inhaltstyp mit dem angegebenen Feld beschränken oder (2) das Feld zu einer Zuordnungsspalte machen. Option 2 wird empfohlen, da es möglich ist, dass sich ein Inhaltstyp ändert und dazu führt, dass der Workflow unterbrochen wird.
  
    
    

### Workflowdesign: Modellieren eines Geschäftsprozesses in einem einzigen Workflow
<a name="bkm_08"> </a>

Wenn möglich, ist es viel besser, einen Geschäftsprozess in einem einzigen Workflow zu modellieren als die Workflowlogik in mehrere kleinere Workflows zu unterteilen.
  
    
    

### Workflowdesign: Effektive Verwendung der Genehmigungsaktion
<a name="bkm_09"> </a>

Wenn möglich, ist es statt der Erstellung mehrerer **Approval**-Aktionen effektiver, das **Stages**-Feature innerhalb einer **Approval**-Aktion zu verwenden.
  
    
    

## Zusätzliche Ressourcen
<a name="bk_addresources"> </a>


-  [Workflows in SharePoint 2013](workflows-in-sharepoint-2013.md)
    
  
-  [Grundlegendes zu SharePoint 2013-Workflows](sharepoint-2013-workflow-fundamentals.md)
    
  
-  [Entwickeln von SharePoint 2013-Workflows mit Visual Studio](develop-sharepoint-2013-workflows-using-visual-studio.md)
    
  

