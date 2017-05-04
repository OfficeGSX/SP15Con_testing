---
title: Vorgehensweise Exportieren Sie das Namensfeld in einer Dokumentbibliothek-Liste in einer mobilen Anwendung
ms.prod: SHAREPOINT
ms.assetid: 901c2012-18c6-4dbd-a787-f8650a0cc7a8
---


# Vorgehensweise: Exportieren Sie das Namensfeld in einer Dokumentbibliothek-Liste in einer mobilen Anwendung
Exportieren Sie das Feld "Name" einer Dokumentbibliotheksliste mit dem Visual Studio SharePoint-Listen-Assistenten in eine mobile App. Das Feld "Name" wird nicht automatisch angezeigt, wenn Benutzer eine mobile App für eine Dokumentbibliothek in SharePoint erstellen.
In einer Liste Dokumentbibliothek kann Benutzer verschiedene Dokumente hochladen. Ein Listenelement für ein Dokument besteht typischerweise als Eigenschaften **Title**, **Name**und eine Verknüpfung mit dem Dokument. Wenn ein Entwickler eine Windows Phone 7-app für die Dokumentbibliothek erstellt, wird das Feld **Name** nicht in der IDE-Assistent exportiert. Daher hat der Entwickler nur schwer feststellen, den Namen des Dokuments. (Dies ist, da SharePoint 2013 Felder vom Typ "Datei" standardmäßig nicht unterstützt.)
  
    
    


> **HINWEIS**
> Die **Title** -Eigenschaft umfasst nicht die Dateinamenerweiterung.
  
    
    


Wenn Sie einem Dokument abrufen, die vollständige URL des Dokuments erstellen und öffnen Sie das Dokument in der standardanwendung (beispielsweise Word) möchten, müssen Sie Code zum Abrufen des Namens des tatsächlichen Datei so schreiben.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie anstelle von Visual Studio 2010 Express Visual Studio Express 2012 verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung Erstellen von apps für Windows Phone 8 und Windows Phone 7.> Weitere Informationen finden Sie unter  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## Erforderliche Komponenten für das Exportieren des Namensfelds einer Dokumentbibliothek


- SharePoint 2013
    
  
- Visual Studio Express 2010 mit den neuen SharePoint-Vorlagen
    
  

## Anpassen der generierten Klassen
<a name="HowToExportTheNameFieldInADocumentLibraryListToAMobileApp_CustomizeTheGeneratedClases"> </a>

Um die **Name** dar und Access-Anlagen aus einer Dokumentbibliothek zu exportieren, müssen Sie einige Änderungen vornehmen, in der **ListDataProvider** -Klasse, DisplayItemViewModel.cs und in der **DisplayForm** -Klasse. Wenn ein Entwickler den Assistenten für neuen SPList aus Visual Studio verwendet, erstellt der Assistent mehrere Klassen, die das Model-View-ViewModel-Entwurfsmuster folgen. (Weitere Informationen finden Sie [unter Verwendung des Model-View-ViewModel-Musters](http://msdn.microsoft.com/en-us/library/hh821028.aspx).) Es werden zwei Ordner erstellt: eine heißt Ansichten und enthält alle Dateien, die zum Ändern der verschiedenen Listenansichten (beispielsweise DisplayForm, EditForm, Listen- und NewForm) erforderlich. Der andere heißt ViewModels und DisplayItemViewModel, EditItemViewModel, ListViewModel und NewItemViewModel enthält. Sie können einige dieser Klassen, die vom Assistenten generierte ändern.
  
    
    

### Schritt 1: Ändern von ListDataProvider zum Abrufen von SPListItem.File-Eigenschaft


1. Fügen Sie in der **ListDataProvider** -Klasse die folgende Codezeile in der **LoadItem** -Methode hinzu.
    
     `Context.Load(spListItem, Item => Item.File);`
    
  
2. Fügen Sie auch in der **ListDataProvider** -Klasse den folgenden Code in der **LoadData** -Methode.
    
     `Context.Load(items, listItems => listItems.Include(item => item.File));`
    
  

### Schritt 2: Hinzufügen der Eigenschaften FileUrl und Dateiname


- Fügen Sie den folgenden Code in DisplayItemViewModel.cs um **DisplayVM**.
    
  ```cs
  
  public string m_fileUrl;
        public string FileUrl
        {
            get
            {
                if (string.IsNullOrEmpty(m_fileUrl))
                {
                    IListDataProvider p = this.DataProvider;
                    p.LoadItem(this.ID, (LoadItemCompletedEventArgs args) =>
                         {
                             FileUrl = this.DataProvider.SiteUrl + 
                                       args.Item.File.ServerRelativeUrl;
                         });
                }
                return m_fileUrl;
            }
            set
            {
                m_fileUrl = value;
                RaisePropertyChanged("FileUrl");
            }
        }
        public string m_fileName;
        public string FileName
        {
            get
            {
                if (string.IsNullOrEmpty(m_fileName))
                {
                    IListDataProvider p = this.DataProvider;
                    p.LoadItem(this.ID, (LoadItemCompletedEventArgs args) =>
                    {
                        FileName = args.Item.File.Name;
                    });
                }

                return m_fileName;
            }
            set
            {
                m_fileName = value;
                RaisePropertyChanged("FileName");
            }
        }
  ```


### Schritt 3: Hinzufügen eines Hyperlinks zu den DisplayForm, die FileUrl und der Dateiname bindet


- Fügen Sie dem Formular anzeigen.
    
  ```XML
  
  <StackPanel HorizontalAlignment="Left" Orientation="Horizontal" Margin="0,5,0,5">
  <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
   Style="{StaticResource PhoneTextNormalStyle}">
    FileUrl :
  </TextBlock>
  <HyperlinkButton Content="{Binding FileName}" NavigateUri="{Binding FileUrl}" 
   x:Name="hypFile" TargetName="_blank" />
  </StackPanel>

  ```


## Zusätzliche Ressourcen
<a name="SP15StoreSPlist_addlresources"> </a>


-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  

