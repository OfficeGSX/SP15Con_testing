---
title: Optimieren der Leistung in SharePoint 2013 Seite
ms.prod: SHAREPOINT
ms.assetid: 262caeef-64fd-4e02-b947-d772faf01159
---



# Optimieren der Leistung in SharePoint 2013 Seite
Informationen Sie zu Features zum Verbessern der Leistung auf den Seiten in SharePoint 2013. Diese Features können zur Verbesserung der Erfahrung in geografisch verteilten Implementierungen verwendet werden.
 * **Bereitgestellt von:*** David Crawford, Microsoft Corporation
  
    
    

Dieser Artikel enthält Anweisungen, die Optimierung der Leistung in SharePoint helfen. SharePoint 2013 enthält Funktionen zur Seite laden über einen Bereich WAN (Wide Network) zu optimieren. Entwerfen von Seiten zu als kleine und schnell wie möglich ergänzt diese Leistungssteigerungen machen.
## Minimale Downloadstrategie (MDS)
<a name="MDS"> </a>

Minimale herunterladen Strategie (MDS) nutzt die Möglichkeit, nur bestimmte Teile einer Seite herunterladen, die auf dem Server vollständig gerendert wird. Nur der bestimmte Teile herunterladen bietet ein sehr effizient Loading-Modell. Die vollständig gerenderte Seite wird nicht an den Client zurückgegeben. Der Server muss genau jene identifizieren, die sein müssen als Teil der Antwort und die, die nicht erforderlich sind. Die Teile, die möglicherweise nicht Teil der Antwort enthalten Skripts, Stile und Markup.
  
    
    
Die folgende Tabelle enthält einige Vorteile der Verwendung von MDS.
  
    
    

**In Tabelle 1. Vorteile der Verwendung von MDS**


|**Leistung**|**Visuelle Objekte**|
|:-----|:-----|
|Weniger Datenmengen pro Seitenanforderung heruntergeladen haben. <br/> |Keine Browser blinken zurückzuführen ganze Seite neu zu laden. <br/> |
|Browser muss nur die Bereiche der Seite aktualisieren, die seit der letzten Anforderung geändert. <br/> |Leicht zu identifizieren, Animationen. <br/> |
|Kleine Datenmengen Verarbeitung auf dem Client erforderlich. <br/> <br/> **HINWEIS**<br/> Die Hälfte der Client Seite Ladezeit 1 (PLT1) ist aufgrund von Chrome cascading Stylesheet (CSS) Rendering und Analysieren von JavaScript und Ausführung.          |Änderungen in der Seite wecken des Benutzers. <br/> |
   
AJAX und MDS sind Technologien, die nur Abschnitte der Seite, um Daten zu minimieren herunterladen und Verbesserung der Seite Reaktionsfähigkeit anfordern. Die folgende Abbildung zeigt die MDS-Architektur.
  
    
    

**Abbildung 1. MDS-Architektur**

  
    
    

  
    
    
![MDS-Architektur](images/OptimizePage_Fig01_MDSArchitecture.png)
  
    
    
Das MDS-Framework wird davon ausgegangen, dass eine Gestaltungsvorlage Chrom und Inhaltsbereiche definiert. MDS bedeutet SharePoint Navigieren zu einer Seite anfordern nur den Inhalt für diesen Regionen und die Ressourcen, denen auf die Seite abhängig ist. Sobald dieser Inhalte an den Browser downloads, gilt Skriptcode die Markup oder Ressourcen auf der Seite aus. Der Browser verhält sich, als ob die angeforderte Seite vollständig vom Server geladen wurde, hatte.
  
    
    

**Abbildung 2. Seitenchrom und Regionen in einer SharePoint-Seite**

  
    
    

  
    
    
![Seitenchrom und Regionen auf einer SharePoint-Seite](images/OptimizePage_Fig02_PageStructure.png)
  
    
    
Wenn Benutzer eine SharePoint-Website im Modus MDS durchsuchen, sie verursachen keine ganze Seitenpostbacks und ganze Seite neu geladen. Stattdessen die URL für die Seite ihnen besuchten erhalten bleibt, und der Fragmentbezeichner ("#"-Zeichen) ändert sich, um die Seite enthalten, die sie besuchen. Das Format der URL lautet:  `[Path to site (spweb)] + /_layouts/15/start.aspx# + [path to page] + [query string]`
  
    
    
Die folgende Tabelle enthält einige Beispiele von URLs im Modus MDS formatiert.
  
    
    

**In Tabelle 2. URLs im Modus MDS formatiert**


|**Nicht MDS-URL**|**MDS-URL**|
|:-----|:-----|
|http://server/SitePages/ <br/> |http://server/_layouts/15/start.aspx#/SitePages/ <br/> |
|http://server/subsite/SitePages/home.aspx <br/> |http://server/subsite/_layouts/15/start.aspx#/SitePages/home.aspx <br/> |
|http://server/_layouts/15/viewlsts.aspx?BaseType=0 <br/> |http://server/_layouts/15/start.aspx#/_layouts/viewlsts.aspx?BaseType=0 <br/> |
   
Das Objekt für die AJAX-Navigation verwendet wird, **AjaxNavigate**. Standardmäßig ist eine Instanz des **AjaxNavigate** für die Verwendung von benannten **ajaxNavigate**verfügbar. So verwenden Sie die Instanz **ajaxNavigate**
  
    
    



```cs

ajaxNavigate.update(serverRelativeURL, null);
```

Wenn Sie ein Steuerelement oder Webpart, um die Navigationsereignisse überwachen möchten, können Sie den **add_navigate** -Ereignishandler verwenden. Wenn der Ereignishandler aufgerufen wird, erhält Ihre Callback-Funktion einen Verweis auf das Navigationsobjekt und die analysierten Hash-Werte in einem Wörterbuch. Das Steuerelement oder Webpart kann den Wert für den Parameter von Interesse aus dem Wörterbuch abzurufen, vergleichen Sie ihn mit den aktuellen Wert und entscheiden, welche Aktion durchgeführt werden sollte. Eine allgemeine Aktion ist das Senden einer AJAX-Anforderung an den Server zum Abrufen von Daten oder Ändern der Reihenfolge der Elemente in der Ansicht. Wenn ein Steuerelement Navigation Ereignisse beendet wurde, können sie den **remove_navigate** -Ereignishandler verwenden.
  
    
    
MDS unterstützt auch mehrere Rauten in Schlüssel/Wert-Paare. MDS fügt die Rauten hinter dem URL an. Das Format der Rauten in der URL lautet: http://server/_layouts/15/start.aspx#/SitePages/page.aspx#key1=value1#key2=value2. Im folgenden Codebeispiel wird veranschaulicht, wie die Rauten aktualisieren.
  
    
    



```
var updateParts;
updateParts = {key1: "value1", key2: "value2", keyn: "valuen"}
ajaxNavigate.update(null, updateParts);

```

Wenn Sie feststellen, dass die Seiten in Ihrer Website ständig zurückgreifen, um die ganze Seite herunterladen, empfiehlt es sich, deaktivieren das Feature MDS berücksichtigt werden sollten. Sie sollten auch die MDS-Feature zu deaktivieren, wenn Sie eine andere Strategie zum Verbessern der Leistung verwenden müssen.
  
    
    
Ein bestimmtes Element in der Seite muss sicherstellen, dass die wichtigen Ressourcen für das Arbeiten mit der MDS-Infrastruktur zur Renderzeit Server bekannt sind. Um ein vorhandenes Projekt zu verwenden, MDS zu konvertieren, müssen Sie die folgenden Dateien und Komponenten aktualisieren:
  
    
    

- Gestaltungsvorlagen
    
  
- ASP.NET Seiten
    
  
- Benutzerdefinierte Masterseiten für Fehler
    
  
- JavaScript-Dateien
    
  
- Steuerelemente und Webparts
    
  

### Gestaltungsvorlagen

Die wichtigste Änderung in der Gestaltungsvorlage ist die Bereiche der HTML-Markup umgeben werden, die mit einem speziellen-Steuerelement mit dem Namen **SharePoint:AjaxDelta** an den Client gesendet wird. Die am häufigsten verwendeten Teile, die von einem Steuerelement **SharePoint:AjaxDelta** umgeben sein müssen, sind in der Gestaltungsvorlage Chrome und der Content Platzhalter eingebettet. Wenn ein Steuerelement auf allen Seiten innerhalb einer Website identisch ist, sollten sie nicht von einem Steuerelement **SharePoint:AjaxDelta** eingeschlossen werden. Das folgende Markup zeigt einen sichtbaren Inhaltsplatzhalter, umgeben von einer **SharePoint:AjaxDelta** -Steuerelement.
  
    
    

```HTML

<SharePoint:AjaxDelta id="DeltaPlaceHolderMain" IsMainContent="true" runat="server">
    <asp:ContentPlaceHolder id="PlaceHolderMain" runat="server" />
</SharePoint:AjaxDelta>
```

Steuerelemente, die abhängig von der aktuellen URL Inhalt aufweisen müssen in einem Steuerelement **SharePoint:AjaxDelta** umbrochen werden. Das folgende Markup zeigt ein Menü, umgeben von einer **SharePoint:AjaxDelta** -Steuerelement.
  
    
    



```HTML

<SharePoint:AjaxDelta id="DeltaBreadcrumbDropdown" runat="server">
    <SharePoint:PopoutMenu
        runat="server"
        ID="GlobalBreadCrumbNavPopout"
        IconUrl=IMGCLUSTER_FG_IMG
        IconAlt=LOC_ATTR_WSS(master_breadcrumbIconAlt)
        IconOffsetX=IMGCLUSTER_FG_LEFT(BREADCRUMBBUTTON)
        IconOffsetY=IMGCLUSTER_FG_TOP(BREADCRUMBBUTTON)
        IconWidth=IMGCLUSTER_FG_WIDTH(BREADCRUMBBUTTON)
        IconHeight=IMGCLUSTER_FG_HEIGHT(BREADCRUMBBUTTON)
        AnchorCss="s4-breadcrumb-anchor"
        AnchorOpenCss="s4-breadcrumb-anchor-open"
        MenuCss="s4-breadcrumb-menu">
    </SharePoint:PopoutMenu>
</SharePoint:AjaxDelta>

```


> **HINWEIS**
> Das Steuerelement **SharePoint:AjaxDelta** sollte nicht in sich selbst geschachtelt werden. Geben Sie dieses Steuerelement auf der höchsten Ebene erforderlich.
  
    
    

Wenn Sie eine Datei cascading Stylesheet (CSS) einschließen möchten, müssen Sie die Steuerelemente **SharePoint:CssLink** und **SharePoint:CssRegistration** verwenden. Diese Steuerelemente wurden aktualisiert, um MDS und nicht MDS-Modus zu arbeiten. Das folgende Markup veranschaulicht, wie die Steuerelemente **SharePoint:CssLink** und **SharePoint:CssRegistration** verwenden.
  
    
    



```HTML

<SharePoint:CssLink runat="server" Version="15"/>
<SharePoint:CssRegistration Name="my_stylesheet.css" runat="server" />
```


> **VORSICHT**
> Sie können nur ein **SharePoint:CssLink** Steuerelement pro Seite haben. In MDS-Modus erhalten Sie einen Fehler, wenn Sie über mehr als ein **SharePoint:CssLink** -Steuerelement auf einer Seite verfügen. Eine CSS-Datei mithilfe des HTML-Format Elements einschließlich ist im Modus MDS nicht unterstützt, da die Serverlogik die Datei als Ressource identifizieren kann nicht, wenn die Antwort gerendert wird.
  
    
    

Eine Datei JavaScript aufnehmen möchten, verwenden Sie das **SharePoint:ScriptLink** -Steuerelement. Das folgende Markup veranschaulicht, wie das **SharePoint:ScriptLink** -Steuerelement verwendet wird.
  
    
    



```HTML

<SharePoint:ScriptLink language="javascript" name="my_javascriptfile.js" runat="server" />
```


> **VORSICHT**
> Einschließlich einer HTML-Tags Script JavaScript Datei wird im MDS-Modus nicht unterstützt, da die Serverlogik die Datei als Ressource identifizieren kann nicht, wenn die Antwort gerendert wird.
  
    
    

Zum Rendern von Title-Elements innerhalb des Head-Elements auf der Seite verwenden wir eine spezielle Muster mithilfe des **SharePoint:PageTitle** -Steuerelements. Das folgende Markup veranschaulicht, wie das **SharePoint:PageTitle** -Steuerelement verwendet wird.
  
    
    



```HTML
<SharePoint:PageTitle runat="server">
    <asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server" />
</SharePoint:PageTitle>

```


> **HINWEIS**
> Jede einzelne Seite muss den Titel außer Kraft setzen, durch Bereitstellen von Ersatz für das **Asp: ContentPlaceHolder** -Steuerelement in das **SharePoint:PageTitle** -Steuerelement.
  
    
    


### ASP.NET Seiten

Ein JavaScript oder eine CSS-Datei aufnehmen möchten, verwenden Sie die gleichen **SharePoint:ScriptLink** und **SharePoint:CssLink** Steuerelemente, die im vorherigen Abschnitt beschrieben.
  
    
    
In früheren Versionen von SharePoint schreibt einige Seiten die Inhalte mithilfe der **Response.Output** -Eigenschaft. Wenn Sie MDS verwenden, ist dies nicht mehr zulässig. Sie müssen die Aufrufe von **Response.Output** verwendet eine neue API zu ändern. Die folgende Tabelle enthält APIs, die häufig verwendet werden in SharePoint-Seiten und der neuen MDS-kompatibles-API.
  
    
    

**Tabelle 3. Häufig verwendete APIs und die zugehörigen MDS-kompatible Alternativen**


|**Häufig verwendete API**|**MDS-kompatible alternative**|
|:-----|:-----|
| [NoEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.NoEncode.aspx) <br/> | [WriteNoEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteNoEncode.aspx) <br/> |
| [HtmlEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.HtmlEncode.aspx) <br/> | [WriteHtmlEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteHtmlEncode.aspx) <br/> |
| [EcmaScriptStringLiteralEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.EcmaScriptStringLiteralEncode.aspx) <br/> | [WriteEcmaScriptStringLiteralEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteEcmaScriptStringLiteralEncode.aspx) <br/> |
| [HtmlEncodeAllowSimpleTextFormatting()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.HtmlEncodeAllowSimpleTextFormatting.aspx) <br/> | [WriteHtmlEncodeAllowSimpleTextFormatting()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteHtmlEncodeAllowSimpleTextFormatting.aspx) <br/> |
| [HtmlUrlAttributeEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.HtmlUrlAttributeEncode.aspx) <br/> | [WriteHtmlUrlAttributeEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteHtmlUrlAttributeEncode.aspx) <br/> |
| [UrlKeyValueEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.UrlKeyValueEncode.aspx) <br/> | [WriteUrlKeyValueEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteUrlKeyValueEncode.aspx) <br/> |
| [UrlPathEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.UrlPathEncode.aspx) <br/> | [WriteUrlPathEncode()](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPHttpUtility.WriteUrlPathEncode.aspx) <br/> |
   
Wenn eine Seite, Steuerelement oder Webpart, deren Ausgabe der **Response.Output** -Eigenschaft anweist, kann dies MDS Failback. Wenn MDS wieder ein Fehler auftritt, führt er eine vollständige Navigation auf die angeforderte Seite. Mithilfe der **DeltaPage ._shipRender** -Eigenschaft während des Debuggens Serverkomponente finden Sie das betreffende Steuerelement.
  
    
    
Sie sollten mit Steuerelementen **SharePoint:ScriptBlock** HTML Inline Skriptelemente ersetzen. Die folgende Tabelle zeigt eine HTML-Inline Script-Element und ein **SharePoint:ScriptBlock** -Steuerelement.
  
    
    

**In Tabelle 4. HTML Inline Script-Element und dessen MDS-kompatible alternative**


|**HTML Inline Script-element**|**MDS-kompatible alternative**|
|:-----|:-----|
|
```

<script type="text/javascript">
    // Your JavaScript code goes here.
</script>
```

|
```

<SharePoint:ScriptBlock runat="server">
    // Your JavaScript code goes here.
</SharePoint:ScriptBlock>
```

|
   
Die Einführung der **SharePoint:ScriptBlock** auf der Seite kann den Bereich der Variablen auf der Seite ändern. Manchmal ist es erforderlich, verschieben die Deklaration von Variablen von `<% %>` zu `<script runat="server"> <script>`. Laden Sie die Seite im Browser nach Updates ausführen, um dies zu testen.
  
    
    

> **HINWEIS**
> MDS-Infrastruktur unterstützt keine VBScript, da nicht als ein Skript in ASP.NET erfasst werden. Skripts müssen in JavaScript konvertiert werden soll.
  
    
    

Links in ASP.NET Seiten müssen aktualisiert werden, um **SPUpdatePage** Typ zu verwenden. Tabelle 5 zeigt Hyperlinks in ASP.NET Seiten und die gleiche Links mit **SPUpdatePage** Typ aktualisiert.
  
    
    

**Tabelle 5. Hyperlink in ASP.NET und dessen MDS-kompatible alternative**


|**Hyperlink in ASP.NET**|**MDS-kompatible alternative**|
|:-----|:-----|
|
```cs

<a
    id=<%_STSWriteHTML("viewlist" + spList.BaseTemplate.ToString());%>
    href=<%_STSWriteURL(listViewUrl);%>
>

```

|
```cs

<a
    id=<%_STSWriteHTML("viewlist" + spList.BaseTemplate.ToString());%>
    href=<%_STSWriteURL(listViewUrl);%>
    onclick="if (typeof(SPUpdatePage) !== 'undefined') return SPUpdatePage(this.href);"
>
```

|
   

### Benutzerdefinierte Masterseiten für Fehler

Auch wenn Sie eine benutzerdefinierte Gestaltungsvorlage für die Fehler verwenden, können Sie Fehlermeldungen in MDS-Modus anzeigen. Um eine benutzerdefinierte Gestaltungsvorlage für Fehler im MDS-Modus verwenden, müssen Sie ein **AjaxDelta** in der Gestaltungsvorlage Fehler definieren, die die **Id** -Eigenschaft auf die Zeichenfolge **"DeltaPlaceHolderMain"** festgelegt wurde. Der Inhalt der Fehlermeldung muss in der **AjaxDelta** gerendert werden. Wenn die Fehlerseite vom Server an den Browser zurückgegeben wird, wird der Browser identifiziert dies als der Hauptinhalt angezeigt und zeigt es dem Benutzer.
  
    
    
Obwohl die Masterseite für die Fehlerseite und die Masterseite für **start.aspx** sind kein explizite Übereinstimmung, kann den Browser zu erkennen, dass ein Fehler aufgetreten ist. Der Browser ermöglicht MDS den entsprechenden Fehlermeldungen Nachrichteninhalt innerhalb der vorhandenen Masterseite verwenden, um eine konsistente und reibungslose benutzererfahrung verwalten.
  
    
    

### JavaScript-Dateien

JavaScript Dateien sollte nur Deklarationen enthalten. Es gibt dennoch viele legacy-Skripts, die globalen Variablen wie enthalten. Globale Variablen müssen erneut initialisiert werden, wenn eine neue Seite im Modus MDS gerendert wird. Sie können die **ExecuteAndRegisterBeginEndFunctions** verwenden, um globalen Variablen zu initialisieren. Im folgenden Codebeispiel wird veranschaulicht, wie die **ExecuteAndRegisterBeginEndFunctions**verwenden.
  
    
    

```

function ExecuteAndRegisterBeginEndFunctions(tag, beginFunc, endFunc, loadFunc)

```

Die **ExecuteAndRegisterBeginEndFunctions** hat die folgenden Parameter:
  
    
    

-  _tag_: der Name der Datei, die die Rückrufe; registriert Es wird nur zu Debuggingzwecken verwendet.
    
  
-  _beginFunc_: eine Funktion, die aufgerufen wird, bevor Sie das Seite Delta anfordern.
    
  
-  _endFunc_: eine Funktion, die aufgerufen wird, nachdem Sie das Delta abgerufen, jedoch bevor der HTML- und JavaScript für die neue Seite angewendet werden.
    
  
-  _loadFunc_: eine Funktion, die nach der HTML- und JavaScript aufgerufen wird, für die neue Seite angewendet wird.
    
  

### Steuerelemente und Webparts

Um MDS verwenden, müssen Steuerelemente und Webparts Seitenressourcen registrieren mithilfe des **SPPageContentManager** -Objekts. Die am häufigsten registrierten Ressourcen mit **SPPageContentManager** sind JavaScript Snippets (Funktionen oder JSON-Variablen) und ausgeblendeten Feldern. Die folgende Tabelle zeigt gängiger Muster für das Einfügen von ausgeblendeten Felder und JavaScript Codeausschnitte und wie Sie die gleichen mithilfe des **SPPageContentManager** -Objekts.
  
    
    

**Tabelle 6. Allgemeine Methoden für das Rendering des Inhalts und die zugehörigen MDS-kompatible Alternativen**


|**Üblich zum Rendern von Inhalt**|**MDS-kompatible alternative**|
|:-----|:-----|
|
```cs

output.Write("<input type=\\"hidden\\" name=\\"");
output.Write(SPHttpUtility.NoEncode("HiddenField));
output.Write("\\" value=\\"");
output.Write(DigestValue);
output.Write("\\" />");
```

|
```cs

SPPageContentManager.RegisterHiddenField(this, "HiddenField", DigestValue);
```

|
|
```cs
Page.ClientScript.RegisterClientScriptBlock(typeof(MyType), "MyKey", "var myvar=1", true);

```

|
```cs

SPPageContentManager.RegisterClientScriptBlock(this, typeof(MyType), "MyKey", "var myvar=1");
```

|
   

> **HINWEIS**
> Die Funktionen **RegisterHiddenField** und **RegisterClientScriptBlock** im **SPPageContentManager** -Objekt erwarten, dass ein Objekt vom Typ **Steuerelement** oder eine **Seite** im ersten Parameter.
  
    
    

Das Modul MDS verwendet den ersten Parameter, um die Skripts filtern. Hier werden die Regeln für die Filterung beim Rendern einer Seite im MDS Deltamodus:
  
    
    

- Wenn das erste Argument vom Typ **Seite** ist, werden die Skripts im Browser ausgeführt.
    
  
- Wenn das erste Argument nicht vom Typ **Seite ist** und das Steuerelement unter einem **SharePoint:AsyncDelta fällt**, werden im Browser die Skripts ausgeführt.
    
  
- Wenn das erste Argument ein Webpart ist, werden die Skripts im Browser ausgeführt.
    
  
Viele APIs in früheren Versionen von SharePoint hat das aktuelle Steuerelement als ein Argument nicht bereitgestellt. Das public-Objektmodell in SharePoint 2013 bietet eine alternative Methode, um die Ressourcen zu registrieren. Die Methoden, die das aktuelle Steuerelement als Argument keine bereitstellen sind weiterhin in der API für Abwärtskompatibilität.
  
    
    
Sie können zwischen zwei Seiten mithilfe einer neuen Funktion mit dem Namen **SPUpdatePage**navigieren. Wenn ein Steuerelement oder Webpart im MDS Deltamodus gerendert wird, müssen die Steuerelemente **HyperLink** fügen Sie den Ereignishandler **onclick** und **SPUpdatePage**aufrufen.
  
    
    
Aktualisieren Sie die XSLT von Webparts verwendet werden, da alle Ressourcen hinzugefügt werden müssen, mithilfe des **SPPageContentManager** -Objekts um zu machen MDS-kompatibel. Sie können die XSLT JavaScript Snippets mithilfe der Methods **RegisterScriptLink** und **RegisterScriptBlock** des **SPPageContentManager** -Objekts hinzufügen.
  
    
    

## Optimieren der Seite downloads
<a name="MDS"> </a>

Wenn Sie die Zusammensetzung des einer Seite verstanden haben, können Sie verschiedene Methoden verwenden, um den Download für diese Seite zu optimieren. Im Allgemeinen ist das Ziel, um die Anzahl von Roundtrips zwischen Client und Server-Computern zu minimieren und die Datenmenge zu reduzieren, die über das Netzwerk wechselt. Die Anleitung in diesem Artikel enthält Empfehlungen, die Sie im Allgemeinen gibt es für eine Vielzahl von verschiedenen Implementierungen von SharePoint 2013 anwenden können.
  
    
    
In Tabelle 7 sind Ladezeiten, die veranschaulichen, den Unterschied zwischen dem ersten Beispiel Zweitens und nachfolgenden Seite geladen wird.
  
    
    

**Tabelle 7. Wie oft Laden der Seite**

|||
|:-----|:-----|
|Laden der ersten Seite <br/> |3-4 Sekunden <br/> |
|Laden der zweiten Seite <br/> |15 Sekunden <br/> |
|Die Seite anschließend geladen <br/> |1 Sekunde <br/> |
   

> **HINWEIS**
> Ladezeiten kann in Ihrer speziellen Szenario unterschiedlich sein. Wie oft laden unterliegen viele Variablen, einschließlich, aber nicht beschränkt auf Seitengröße, der Wartezeit und Serverlast.
  
    
    

Sie sollten seitenoptimierungstechniken in zwei Kategorien unterteilt: die erste Seite Anforderung und nachfolgende Seitenanforderungen. Optimierungen für die erste Seitenanforderung (Seite des Ladens 1 oder PLT1) sind diese Arten von Optimierungen, die eine effektive erstmals die Seite angefordert wird, jedoch nicht unbedingt nachfolgende Seitenanforderungen beeinträchtigt werden. Im folgenden sind einige Optimierungen für PLT1:
  
    
    

- Optimieren Sie HTML-Markup.
    
  
- Verwenden Sie konsolidierten Bilder und Dateien. Kombinieren Sie beispielsweise mehrere CSS-Dateien in eine. Kombinieren Sie Bilder in einem Bildstreifen oder Cluster.
    
  
- Verwenden Sie Komprimierungstechniken (Crunch) Techniken. Weitere Informationen finden Sie unter  [Komprimieren Sie (crunchen) JavaScript und CSS-Dateien](optimize-page-performance-in-sharepoint-2013.md#OptimizingPagePerformance_Crunch).
    
  
- Stellen Sie sicher, dass Sie die Produktionsversion üblichen JavaScript Bibliotheken wie jQuery, anstatt die Debugversionen verweisen.
    
  
- Erwägen Sie eine bekannte Content Delivery Network (CDN) wie die  [Microsoft Ajax Content Delivery Network](http://www.asp.net/ajaxlibrary/cdn.ashx). Auf den Seiten erforderlichen Dateien möglicherweise bereits vom Clientbrowser zwischengespeichert werden sollen.
    
  
Optimierungen für nachfolgende Seitenanforderungen sind die, die die Benutzeroberfläche für eine Auslastung der nachfolgenden Seite verbessert werden kann. Der Schlüssel ist, die Sie benötigen, um Verlust in Funktionalität für die Verstärkung erreicht auszugleichen. Wenn Gewinn nur beim ersten realisiert ist ein Benutzer eine Website besucht, die Optimierung möglicherweise nicht lohnt des Verlust an Funktionalität.
  
    
    

## Komprimieren Sie (crunchen) JavaScript und CSS-Dateien
<a name="OptimizingPagePerformance_Crunch"> </a>

Dateien, die JavaScript und Formatvorlagen enthalten möglicherweise stark verkleinert werden durch Entfernen von Leerzeichen, Formatvorlage Vererbung und Wiederverwendung von Code. Einige Bibliotheken sind in regulären (Debug) und komprimierten Versionen (crunched). Sie können eine Vielzahl von Tools zum Automatisieren der Datei crunching über eine Suche im Internet suchen.
  
    
    
Stellen Sie sicher, dass die komprimierten Versionen in Produktionsservern bereitgestellt werden. Dieses Beispiel zeigt, wie eine CSS-Datei in Größe über einige relativ einfach Umschreiben von Adressen reduziert werden kann.
  
    
    



```
.article-ByLine  {
  font-family: Tahoma,sans-serif; 
  font-size: 9.5pt; 
  font-style: normal; 
  line-height: normal; 
  font-weight: normal; 
  color: #000000
}
.article-Caption { 
  font-family: Tahoma,sans-serif; 
  font-size: 8pt; 
  font-style: normal; 
  line-height: normal; 
  font-weight: normal; 
  color: #000000
}
.article-Headline { 
  font-family: Tahoma,sans-serif; 
  font-size: 14pt; 
  font-style: normal; 
  line-height: normal; 
  font-weight: bold; 
  color: #000000
}
.article-SubHead  { 
  font-family: Tahoma, sans-serif; 
  font-size: 11pt; 
  font-style: normal; 
  line-height: normal; 
  font-weight: normal; 
  color: #000000
}
.article-Text {
  font-family: Tahoma, sans-serif; 
  font-size: 10pt; 
  font-style: normal; 
  line-height: normal; 
  font-weight: normal; 
  color: #000000
}

```

In der Regel finden Sie Verfahren zum Erreichen der gleichen Stil und verringern Sie die Größe der Dateien von effizient umschreiben CSS-Dateien. Im folgenden Beispiel wird veranschaulicht, wie auf die vorherige Größe CSS optimieren, indem Sie die Formatvorlagen von einem übergeordneten Element erben.
  
    
    



```

BODY {font:100% Tahoma,sans-serif}
.art-By {font: 79%}
.art-Cap {font: 67%}
.art-Head {font: bold 117%}
.art-Sub {font: 92%}
.art-Txt {font: 83%}
```

Die erste Version des CSS ist 783 Zeichen lang sein darf, und die zweite beträgt 140 Zeichen lang sein darf.
  
    
    

## Entity-tags
<a name="OptimizingPagePerformance_Crunch"> </a>

Entitätstags (ETags) kann den Client Dateien unnötig erneut zu laden. ETags sollen Dateien anhand einer Zahl, die der Server generiert eindeutig zu identifizieren. In der Server-Clustern wird jeder Server eine unterschiedliche Anzahl erstellt. Beispielsweise ist ein Browser **Get** -Methode mit einem **If-Modified** -Header-Feld für eine Datei senden, die es in seinem Cache gefunden. Ist nur einen Server, die Datei wird wahrscheinlich übereinstimmen, und ein **304 Not-Modified** HTTP-Status wird gesendet. Aber, wenn der Benutzer auf eine große Serverfarm zugreift, wird es wahrscheinlich einen anderen Server mit einem anderen Entitätstag, verursacht diese Server zum Senden der Datei an den Browser erreicht.
  
    
    

## Cacheeinstellungen
<a name="OptimizingPagePerformance_Crunch"> </a>

Verwenden Sie Fiddler oder einem ähnlichen Tool wird überprüft, ob der Cache Anforderungen verarbeitet wird. Häufige Ursachen für Zwischenspeichern nicht verarbeiten Anforderungen umfassen:
  
    
    

- Einen Datumswert Ablauf keine Ressourcen.
    
  
- Die Abfragezeichenfolge ändert ständig.
    
  
- Die Summe der **Max-Age** cachesteuerelement Richtlinie plus **last-modified** Header erzeugt ein Datum vor heute.
    
  
- Die **Max-Age** -Eigenschaft unterstützt die Proxy-Server nicht.
    
  
- Der **Cache-Control-** Header hat den Wert **Nein-Cache**. Ein Wert von **privaten** speichert die Ressource nur für den Benutzer, der die Anforderung ausgibt.
    
  

## Anzahl und Größe von Bildern
<a name="OptimizingPagePerformance_Crunch"> </a>

Sie sollten die Anzahl der Bilder in Ihrer Website minimieren. Damit mit dieser Aufwand gewährleistet ist, können Sie mehrere Bilder in einer einzigen Datei einbetten und dann einzelne Bilder auf der Seite verweisen. Nicht nur Download Dateigröße verschlechtert, sondern weniger Dateien weniger Netzwerkverkehr führen. Es ist zum Erstellen von Seiten mithilfe dieses Verfahrens komplizierter, aber in Situationen, wo jedem Roundtrip und Dateigröße ankommt, kann es nachweisen, wertvolle Lösung zur Verbesserung von Leistung. Abbildung 3 zeigt ein Beispiel für eine einzelne Bilddatei, die mehrere Bilder enthält.
  
    
    

**Abbildung 3. Einzelne Bilddatei an, die mehrere Bilder enthält**

  
    
    

  
    
    
![Datei mit nur einem Bild, die mehrere Bilder enthält](images/OptimizePage_Fig03_SingleImage.png)
  
    
    
Abbildung 4 zeigt, wie die Bilddatei später geändert wird, um als einzelne Bilder in einer Tabelle anzuzeigen.
  
    
    

**Abbildung 4. Mehrere Bilder in einer Tabelle dargestellte**

  
    
    

  
    
    
![Mehrere in einer Tabelle dargestellte Bilder](images/OptimizePage_Fig04_ImageTable.png)
  
    
    
Die Änderung dieser Bilder erfolgt komplett über Stylesheetklassen. Es wurden zwei primäre Klassen in **div**- und **img**-Elementen in jeder Tabellenzelle verwendet. Diese Klassen lauten wie folgt:
  
    
    



```

BODY {font:100% Tahoma,sans-serif}
.art-By  {font: 79%}
.art-Cap {font: 67%}
.art-Head {font: bold 117%}
.art-Sub  {font: 92%}
.art-Txt {font: 83%}


.cluster { 
   height:50px; 
   position:relative; 
   width:50px; 
} 
.cluster img { 
   position:absolute; 
}
```

Jedem Bild ist basierend auf dem Bezeichner (ID) für das Bild eine Klasse zugeordnet. Dieser Stil beschneidet das Bild und definiert ein Offset aus dem anfänglichen Bild im Cluster. Diese Klassen lauten wie folgt:
  
    
    



```

#person {
   border:none; 
   clip:rect(0, 49, 49, 0); 
} 
#keys { 
   clip:rect(0, 99, 49, 50); 
   left:-50px; 
} 
#people { 
   clip: rect(0, 149, 49, 100); 
   left:-100px; 
} 
#lock { 
   clip:rect(0, 199, 49, 150); 
   left:-150px; 
} 
#phone { 
   clip:rect(0,249, 49, 200); 
   left:-200px; 
}
#question {
    clip: rect(0, 299, 49, 250);
    left: -250px;
}
```


## Listenansichtsseiten
<a name="OptimizingPagePerformance_Crunch"> </a>

Microsoft hat funktioniert, um quantifizieren und Verbessern der Leistung der Liste Ansicht Seite Rendering Zeiten. Eine Seite mit der Listenansicht ist die AllItems.aspx-Seite, die von jedem Listen- und Bibliotheksdaten zum Durchsuchen von Inhalten aktivieren verwendet wird. Die Renderzeit dieser Seite kann weit basierend auf der Anzahl der Spalten, die in der Ansicht sichtbar sind und das Format der Spalten variieren. Beispielsweise können Optionen anzeigen und Aktivieren von Anwesenheitssymbole Renderzeit erheblich beeinträchtigen. Die Gruppierungsoption reduzierten wesentlich länger als die Gruppierungsoption erweiterte gerendert wurde, und beide wurden in allen langsamer als die Gruppierungsoption keine.
  
    
    
Diese Art von Nuancen sind, warum es ist wichtig, überlegen Sie sich wie Ansichten in der Liste anzeigen, Seiten anzeigen, insbesondere über langsame Verbindungen erstellt werden. Bei der Verwendung von Listen, die eine große Datenmenge enthalten ist es wichtig, alle Ansichten, insbesondere die Standardansicht sorgfältig entwerfen. Im Allgemeinen können Sie die Zeit für das Rendering von Listenansichtsseiten beschleunigen, mithilfe der folgenden Empfehlungen:
  
    
    

- Nur die unbedingt erforderlichen Spalten anzeigen.
    
  
- Wenn möglich, schließen Sie Spalten aus, die Anwesenheitsinformationen enthalten.
    
  
- Verwenden Sie einen Link zum Anzeigen der Elementdetails anstelle einer im Bearbeitungsmenü.
    
  
In der folgenden Tabelle sind die Anpassungen beschrieben, mit denen die zum Rendern einer Ansicht erforderliche Zeit reduziert wird.
  
    
    

**Tabelle 8. Anpassungen, die den Zeitaufwand zum Rendern der Ansicht zu reduzieren.**


|**Element**|**Beschreibung**|
|:-----|:-----|
|Ansichtstyp <br/> |Erstellen Sie eine Ansicht als Datenblattansicht und nicht als Standardansicht. <br/> |
|Ansicht: Elementgrenzwert <br/> |Alles über 1.000 wird wahrscheinlich langsam gerendert. Über eine langsame Verbindung ist es wichtig, zu der ein Gleichgewicht zwischen der Menge der Daten aufgeführt und die Anzahl von Roundtrips erforderlich sind, um alle Daten anzuzeigen. Je mehr Zeilen, die an eine Zeit, die weniger Roundtrips aber größere Seiten angezeigt werden. <br/> |
|Ansicht: Filter <br/> |Verwenden Sie **[Today]** und **[Me]** -Schlüsselwörter, um Elemente nach Aktualität oder eine Zuordnung zu filtern. Verwenden Sie Statusfelder, um nur aktive Elemente in standardmäßigen Ansichten anzeigen. <br/> |
|Ansicht: Spalten <br/> |Fügen Sie die kleinste Anzahl an Spalten ein. Erstellen Sie eine Standardansicht mit wenigen Spalten, die ein Browsen auf höchster Stufe ermöglicht, nachdem Benutzer ein Drilldown ausführen können. <br/> |
   
In der folgenden Tabelle sind die Anpassungen beschrieben, mit denen die zum Rendern einer Ansicht erforderliche Zeit erhöht wird. Mit jeder zusätzlichen Spalte wird die Renderingzeit um eine geringe Menge erhöht: bis zu einer halben Sekunde pro Spalte bei einer schnellen Netzwerkverbindung für eine Liste mit 1.000 Elementen. Wie in der Tabelle angemerkt ist, wird die Renderingzeit bei einigen Spalten mehr erhöht als bei anderen.
  
    
    

**Tabelle 9. Anpassungen, die den Zeitaufwand zum Rendern der Ansicht erhöhen**


|**Element**|**Description**|
|:-----|:-----|
|Gruppieren nach <br/> |Beim Gruppieren werden HTML und JScript hinzugefügt, wodurch das Rendering für große Listen verlangsamt wird. Durch das standardmäßige Erweitern aller Gruppen wird die Renderingzeit aufgrund zusätzlicher Vorgänge im Browserobjektmodell weiter erhöht. <br/> |
|Spalte - Titel über das Menü **Bearbeiten** mit Element verknüpft <br/> |Die Option **Hyperlink zu Element mit Menü 'Bearbeiten'** dauert am längsten. Bei der ähnlichen Option **Hyperlink zum Eintrag** wird die Renderingzeit nicht bedeutend erhöht. <br/> |
   

## Entwicklerdashboard
<a name="DeveloperDashboard"> </a>

Das Entwicklerdashboard ist für SharePoint 2013 zum Bereitstellen weiterer Informationen, einschließlich MDS ein neu erstellt. Er ausgeführt wird, in einem separaten Fenster festzulegen, damit Rendering der aktuellen Seite und liefert ausführliche Anforderungsinformationen pro Seite mit einer Diagrammansicht. Darüber hinaus eine dedizierte Registerkarte für Protokolleinträge für eine bestimmte Anforderung Unified Logging System (ULS). Weitere detaillierte Informationen ist für die Analyse der Anforderung enthalten. Ablaufverfolgungsinformationen bieten einen dedizierten Windows Communication Foundation (WCF) Dienst (diagnosticsdata.svc) verwendet.
  
    
    
Erfahren Sie mehr über das Entwicklerdashboard:
  
    
    

-  [Übersicht über die SharePoint 2013 erneuert Entwicklerdashboard](http://www.microsoft.com/resources/technet/en-us/office/media/video/video.mdl?cid=stc&amp;amp;from=mscomstc&amp;amp;VideoID=505bdd61-1fcc-4125-97fc-b5f0dda72cbc) (Video).
    
  
-  [Entwicklerdashboard erneuert](http://download.microsoft.com/download/7/7/3/773CA2C2-579B-408C-808E-A6F561194E20/Ig15_SP_IT_M10V3_devdash.pptx) (PowerPoint-Bildschirmpräsentation).
    
  
Verwenden Sie zum Aktivieren des Entwicklerdashboards der folgende Codeausschnitt Windows PowerShell.
  
    
    



```

$content = ([Microsoft.SharePoint.Administration.SPWebService]::ContentService)
$appsetting =$content.DeveloperDashboardSettings
$appsetting.DisplayLevel = [Microsoft.SharePoint.Administration.SPDeveloperDashboardLevel]::On
$appsetting.Update() 

```

Abbildung 5 zeigt das Entwicklerdashboard.
  
    
    

**Abbildung 5. Entwicklerdashboard**

  
    
    

  
    
    
![Entwickler-Dashboard](images/OptimizePage_Fig05_DevDashboard.png)
  
    
    
Es ist wichtig zu verstehen, wie diese Anforderungen und die Anzahl der Bilder und Abfragen die Leistung beeinträchtigen. Ähnlichkeiten sind vorhanden, wenn es um serverseitige gerenderten Listenansichten (XSL- oder CAML) geht, wie sie die gleiche Größe Empfehlungen als clientseitige gerenderten Listenansichten folgen. Server die Listenansicht Anweisungen sind nur Listenansichten erforderlich sind, um Ihre Anforderungen zu erreichen, wenn Ihr Ziel ist es eine optimale Leistung als Tausende von Ansichten erstellen, jedoch wird größer Beeinträchtigung Leistung aufgrund der Kompilierung Cache-verursacht. Die physischen Merkmale des Computers, beispielsweise Arbeitsspeicher und Geschwindigkeit werden in der gesamten Geschwindigkeit Faktor. Es gibt auch berücksichtigt, in dem die Anforderungen weiterleiten oder wie sie verteilt werden. Um besser zu verstehen, wie SharePoint leitet und verteilt die Anforderungen, können Sie das Tool Anforderungs-Manager verwenden. Besprechen jedoch anfordern, Verteilung Gegenstand dieses Artikels ist. Weitere Informationen finden Sie unter  [Konfigurieren von Anforderungs-Manager in SharePoint Server 2013](http://technet.microsoft.com/library/jj712708.aspx).
  
    
    

## Schlussbemerkung
<a name="bk_conclusion"> </a>

Ein Großteil der Richtlinien für die SharePoint 2010 Seite zur Optimierung der Leistung auf SharePoint 2013 angewendet wird. Dieser Artikel enthält einige der Elemente von Richtlinien für die SharePoint 2010 beim Einstieg in die neuen Bereiche, die speziell Leistung profitieren würden. Wir haben behandelt einige Verbesserungen, beispielsweise MDS und die erweiterte Entwicklerdashboard oder offensichtlich geändert wird. Wir mit der klassischen Anleitung eingeschlossen: Crunch nach unten JavaScript und cascading Stylesheets, ein CDN für allgemeine JavaScript Bibliotheken nach Möglichkeit zum Zwischenspeichern, kombinieren und Bilder so weit wie möglich komprimieren, beschränken oder Entfernen nicht benötigte Daten aus der Ansicht und erstellen Listenansichten überlegt, verwenden. Die Techniken und Funktionen, die in diesem Artikel erläuterten mitwirken für die Unterstützung der Leistungsziele.
  
    
    

## Zusätzliche Ressourcen
<a name="bk_addresources"> </a>


-  [Erstellen von Websites für SharePoint](build-sites-for-sharepoint.md)
    
  
-  [SharePoint 2013 Design Manager - Bilddarstellungen](sharepoint-2013-design-manager-image-renditions.md)
    
  
-  [Übersicht über den Entwurfs-Manager in SharePoint 2013](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [Übersicht über das SharePoint 2013-Seitenmodell](overview-of-the-sharepoint-2013-page-model.md)
    
  
