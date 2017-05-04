---
title: Vorgehensweise Bereitstellen eines benutzerdefinierten Designs in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# Vorgehensweise: Bereitstellen eines benutzerdefinierten Designs in SharePoint 2013
Hier finden Sie Informationen dazu, wie Sie mithilfe der Benutzeroberfläche oder durch Implementierung eines Featureempfängers ein benutzerdefiniertes Design auf einer SharePoint 2013-Website bereitstellen.
SharePoint 2013 enthält vorinstallierte Designs. Sie können zusätzliche Farbpaletten, Schriftartenschemas und Gestaltungsvorlagen erstellen und so benutzerdefinierte Designs erstellen. Sobald die Dateien in die Design Gallery und den Gestaltungsvorlagenkatalog hochgeladen wurden, können Sie ein Design mithilfe der Benutzeroberfläche oder mithilfe von Code bereitstellen. Weitere Informationen zu Farbpaletten und Schriftartenschemas finden Sie unter  [Farbpaletten und Schriftarten in SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md).
  
    
    


## Wichtige Kernkonzepte für die Bereitstellung eines Designs
<a name="core"> </a>

In Tabelle 1 sind Artikel aufgeführt, die grundlegende Informationen zu den Kernkonzepten der Bereitstellung von Designs enthalten.
  
    
    

**Tabelle 1. Kernkonzepte zur Bereitstellung eines Designs**


|**Titel des Artikels**|**Beschreibung**|
|:-----|:-----|
| [Übersicht über Designs für SharePoint 2013](themes-overview-for-sharepoint-2013.md) <br/> |Informationen zur Designoberfläche in SharePoint 2013.  <br/> |
| [Featureereignisse](http://msdn.microsoft.com/de-de/library/ms469501.aspx) <br/> |Informationen zu Featureereignissen, die es Ihnen ermöglichen, ein Ereignis abzufangen und darauf zu reagieren, wenn ein Feature in der Serverfarm installiert ist.  <br/> |
   

## Hochladen von Dateien in die Design Gallery und den Gestaltungsvorlagenkatalog
<a name="section1"> </a>

Sie können benutzerdefinierte Designs erstellen, indem Sie zusätzliche Farbpaletten und Schriftartenschemas erstellen und diese in die Design Gallery hochladen. Die neuen Farbpaletten und Schriftartenschemas stehen Ihnen dann zur Verfügung, wenn Sie ein Design in der Designoberfläche ändern, oder wenn Sie ein Design programmgesteuert anwenden. Genauso können Sie zusätzliche Gestaltungsvorlagen und entsprechende Vorschaudateien in den Gestaltungsvorlagenkatalog hochladen, wenn Sie zusätzliche Websitelayouts zur Auswahl haben möchten. In der folgenden Liste wird beschrieben, wo Sie die Dateien ablegen.
  
    
    

- **Gestaltungsvorlagenkatalog** Führt die Gestaltungsvorlagendateien und die entsprechenden Vorschaudateien (.preview-Dateien) auf. Sie benötigen eine Vorschaudatei für Gestaltungsvorlagen, wenn die Gestaltungsvorlage im Assistent **Aussehen ändern** verfügbar sein soll. JavaScript-Dateien und andere Designressourcen können auch in den Gestaltungsvorlagenkatalog hochgeladen werden.
    
    Um den Gestaltungsvorlagenkatalog über die Benutzeroberfläche von SharePoint aufzurufen, wählen Sie auf der Seite **Websiteeinstellungen** unter **Web-Designer-Kataloge** **Gestaltungsvorlagen** aus. Sie können auch direkt zu der Website navigieren (http:// _SiteName_/_catalogs/masterpage/).
    
  
- **Design Gallery** Führt die Farbpaletten und Schriftartenschemas auf, die in der Designoberfläche verfügbar sind. SharePoint sucht im Ordner **15**, um die verfügbaren Farbpaletten und Schriftartenschemas zu ermitteln.
    
    Um die Design Gallery über die Benutzeroberfläche von SharePoint aufzurufen, wählen Sie auf der Seite **Websiteeinstellungen** unter **Web-Designer-Kataloge** **Designs** aus. Sie können auch direkt zu der Website navigieren (http:// _SiteCollectionName_/_catalogs/theme/15).
    
  
- **Formatbibliothek** Führt die benutzerdefinierten CSS-Dateien auf, die Sie in der Designoberfläche verwenden möchten. Sie können direkt zur Formatbibliothek navigieren (ersetzen Sie _SiteCollectionName_ und _language_ in dieser URL: http:// _SiteCollectionName_/Style Library/ _language_/Themable/).
    
> **HINWEIS**
> Legen Sie die benutzerdefinierten CSS-Dateien im Ordner "Themable" in der Formatbibliothek ab, nicht im Ordner "Themable" im Gestaltungsvorlagenkatalog. Vom Designmodul werden nur CSS-Dateien erkannt, die im Ordner "Themable" in der Formatbibliothek gespeichert sind. 

> **HINWEIS**
> Wenn Sie im Gestaltungsvorlagenkatalog und der Design Gallery Versionsverwaltung aktiviert haben, müssen Sie auch die Designdateien veröffentlichen, bevor sie vom Designmodul verwendet werden können. 
  
    
    


## Bereitstellen eines Designs mithilfe der Benutzeroberfläche
<a name="section2"> </a>

Unter einem durchkomponierten Look oder einem Design versteht man die Farbpalette, das Schriftartenschema, das Hintergrundbild und die Gestaltungsvorlage, welche das Erscheinungsbild einer Website bestimmen. Die Liste "Durchkomponierte Looks" enthält die durchkomponierten Looks, die in der Design Gallery verfügbar sind. Sie können ein Design erstellen, indem Sie in der Liste "Durchkomponierte Looks" ein Listenelement hinzufügen und angeben, welche Gestaltungsvorlage, welche Farbpalette, welches Schriftartenschema und welches Hintergrundbild verwendet werden soll.
  
    
    

> **HINWEIS**
> Sie benötigen eine Vorschaudatei für Gestaltungsvorlagen, wenn die Gestaltungsvorlage in der Design Gallery verfügbar sein soll. 
  
    
    


### So fügen Sie einen durchkomponierten Look hinzu


1. Wählen Sie das Symbol **Einstellungen** und dann **Websiteeinstellungen** aus.
    
  
2. Wählen Sie unter **Web-Designer-Kataloge** **Durchkomponierte Looks** aus.
    
  
3. Wählen Sie in der Liste **Durchkomponierte Looks** **Neues Element** aus.
    
  
4. Geben Sie im Textfeld **Titel** einen Titel für das Design ein.
    
  
5. Geben Sie im Textfeld **Name** einen Namen für das Design ein. Der Name wird in der Liste "Durchkomponierte Looks" und in der Design Gallery angezeigt.
    
  
6. Geben Sie im Textfeld ** Gestaltungsvorlagen-URL** die URL der Gestaltungsvorlage ein. Die URL kann eine relative URL sein.
    
  
7. Geben Sie im Textfeld **Design-URL** die URL der Farbpalette (die URL zur .spcolor-Datei) ein. Die URL kann eine relative URL sein.
    
  
8. Geben Sie im Textfeld **Bild-URL** die URL zum Hintergrundbild ein. Dies ist optional. Die URL kann eine relative URL sein.
    
  
9. Geben Sie im Textfeld **Schriftartenschema-URL** die URL des Schriftartenschemas (die URL zur .spfont-Datei) ein. Dies ist optional. Die URL kann eine relative URL sein.
    
  
10. Geben Sie im Textfeld **Anzeigereihenfolge** die Nummer der Anzeigereihenfolge ein. Hiervon hängt ab, wo das Design in der Design Gallery angezeigt wird.
    
  
11. Wählen Sie **Speichern** aus.
    
    > **HINWEIS**
      > Liegt ein Problem mit den Werten des durchkomponierten Looks vor, wird der durchkomponierte Look der Design Gallery nicht hinzugefügt, und in den Protokolldateien wird keine Meldung aufgezeichnet. Mögliche Gründe dafür, warum ein durchkomponierter Look nicht hinzugefügt werden kann, sind: Eine Datei kann nicht gefunden werden, bei einer der Dateien liegt ein Formatierungsproblem vor, oder SharePoint kann auf die Dateien nicht zugreifen. 
Sie können jetzt die Design Gallery verwenden, um das neue Design auf Ihre Website anzuwenden. Weitere Informationen finden Sie auf Office.com unter  [Auswählen eines Designs für Ihre Veröffentlichungswebsite ](http://office.microsoft.com/de-de/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx).
  
    
    

## Bereitstellen eines Designs mithilfe von Code
<a name="section3"> </a>

Sie können ein Design durch die Implementierung eines Featureempfängers bereitstellen.
  
    
    

### So stellen Sie ein Design mithilfe eines Featureempfängers bereit


1. Erstellen Sie eine Featureempfängerklasse, die von der  [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) -Klasse erbt.
    
  
2. Erstellen Sie in der  [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) -Methode ein [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) -Objekt, das die Farbpalette und das Schriftartenschema verwendet, und wenden Sie das Design dann auf Ihre Website an.
    
    Das folgende Codebeispiel zeigt, wie Sie eine benutzerdefinierte Farbpalette und ein Schriftartenschema auf einer Website bereitstellen.
    


  ```cs
  
// Get the SPColor file. Replace with the path to your SPColor file.
SPFile colorPaletteFile = Web.GetFile("path to .spcolor file");
if (null == colorPaletteFile || !colorPaletteFile.Exists)
{
    // TODO: handle the error.
    return;
}

// Get the SPFont file. Replace with the path to your SPFont file.
SPFile fontSchemeFile = Web.GetFile("path to .spfont file");
if (null == fontSchemeFile || !fontSchemeFile.Exists)
{
    // TODO: handle the error.
    return;
}

// Open an SPTheme with the two files. Replace NewTheme with the name for your theme.
// Note: If you have a background image, you can specify the following:
// SPTheme.Open("NewTheme", colorPaletteFile, fontSchemeFile, backgroundURI)
SPTheme theme = SPTheme.Open("NewTheme", colorPaletteFile, fontSchemeFile);


// Now apply your theme to the site.
// The themed CSS output files are stored in the Themed folder of the Theme Gallery of the root web
// of the site collection. To specify that the files should be stored in the _themes folder within the root 
// web, pass false to the ApplyTo method.
theme.ApplyTo(Web, true);
  ```


> **HINWEIS**
> Der  _shareGenerated_-Parameter in der **ApplyTo**-Methode gibt an, ob die Designdateien von mehreren Websites in einer Websitesammlung verwendet werden können. Normalerweise ist er bei SharePoint Server- und SharePoint Online-Websites auf **true** und bei SharePoint Foundation-Websites auf **false** festgelegt. Der _shareGenerated_-Parameter muss auf **true** festgelegt sein, wenn die Designdateien freigegeben werden sollen. Weitere Informationen finden Sie unter [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) .

Wenn ein Benutzer im Assistenten **Aussehen ändern** ein Design anwendet, aktualisiert der Assistent auch das Design mit dem Titel "Aktuell" in der Liste "Durchkomponierte Looks" und in der Design Gallery. Wenn Sie ein Design programmgesteuert anwenden, müssen Sie das Design mit dem Titel "Aktuell" manuell aktualisieren. Mit dem folgenden Beispiel wird gezeigt, wie Sie das Design mit dem Titel "Aktuell" aktualisieren.
    


  ```cs
  
SPList designGallery = Web.GetCatalog(SPListTemplateType.DesignCatalog);
if (null == designGallery)
{
    // TODO: Handle the error.
    return;
}

SPQuery q = new SPQuery();
q.RowLimit = 1;
q.Query = "<Where><Eq><FieldRef Name='DisplayOrder'/><Value Type='Number'>0</Value></Eq></Where>";
q.ViewFields = "<FieldRef Name='DisplayOrder'/>";
q.ViewFieldsOnly = true;

SPListItemCollection currentItems = designGallery.GetItems(q);

If (currentItems.Count == 1)
{
    // Remove the old Current item.
    currentItems[0].Delete();
}

SPListItem currentItem = designGallery.AddItem();

currentItem["Name"] = SPResource.GetString(CultureInfo.CurrentUICulture, Strings.DesignGalleryCurrentItemName);
currentItem["Title"] = SPResource.GetString(CultureInfo.CurrentUICulture, Strings.DesignGalleryCurrentItemName);

// Change this line if you want to specify a different master page.
currentItem["MasterPageUrl"] = Web.MasterUrl;

// Replace with the path to your SPColor file.
currentItem["ThemeUrl"] = "path to .spcolor file";

// Delete the following line if you do not have a background image. Otherwise, replace with the path to
// the background image.
currentItem["ImageUrl"] = "path to background image"; 

// Replace with the path to your SPFont file. Or, you can delete this line if you want to use
// the default font scheme of the selected master page.
currentItem["FontSchemeUrl"] = "path to .spfont file"; 

currentItem["DisplayOrder"] = 0;
currentItem.Update();

  ```


## Zusätzliche Ressourcen
<a name="bk_addresources"> </a>


-  [Entwickeln des Website-Designs in SharePoint 2013](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [Farbpaletten und Schriftarten in SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [Gewusst wie: Erstellen einer Gestaltungsvorlagen-Vorschaudatei in SharePoint 2013](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [SharePoint-Teamblog: Beweisen Sie Stil mit SharePoint-Designs](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

