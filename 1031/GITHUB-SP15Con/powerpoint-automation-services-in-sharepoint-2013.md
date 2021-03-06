---
title: PowerPoint Automation Services in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 168c7dc0-fbdc-41a2-84db-65d211d3d673
---


# PowerPoint Automation Services in SharePoint 2013
Informationen zur Verwendung von Microsoft PowerPoint Automation Services für eine serverseitige Konvertierung einer Präsentation in und von zahlreichen Dateiformaten. 

  
    
    

 * **Applies to:*** *Microsoft SharePoint Server 2013*  | *Microsoft PowerPoint Automation Services* 
## Einführung
<a name="PAS_Intro"> </a>

Viele Klein- und mittelständische Unternehmen verwenden ihre Microsoft SharePoint Server-Bibliotheken als Repository für Microsoft PowerPoint-Präsentationen. Diese Unternehmen stellen spezielle Anforderungen an das Speichern, Verteilen und Aktualisieren ihrer Präsentationen. Microsoft PowerPoint Automation Services ist ein neues Feature von Microsoft SharePoint Server 2013, das Unternehmen bei der Verwaltung ihrer Präsentationen unterstützt. Es ist ein freigegebener Dienst, der eine unbeaufsichtigte, serverseitige Konvertierung von Präsentationen in andere Formate bietet. Der Dienst wurde für das Arbeiten auf Servern entwickelt und kann zahlreiche Präsentationsdateien auf verlässliche und vorhersehbare Weise verarbeiten.
  
    
    
Mit PowerPoint Automation Services können Sie das PowerPoint-Binärdateiformat (.ppt) und das PowerPoint-Open XML-Dateiformat (.pptx) in andere Formate konvertieren. Sie können beispielsweise mehrere PowerPoint 97-2003-Dateien in Open XML-Präsentationsdateien konvertieren. Sie können auch eine benutzerdefinierte Aktion im Menü **Bearbeiten** erstellen, damit Benutzer bei Bedarf eine PDF-Version ihrer Präsentationen erstellen können.
  
    
    

> **HINWEIS**
> PowerPoint Automation Services nutzt die Funktionen von SharePoint 2013 und ist ein Feature davon. SharePoint Server 2013 muss installiert sein, damit Sie PowerPoint Automation Services verwenden können. Wenn Sie SharePoint Server 2013 in einer Serverfarm verwenden, müssen Sie PowerPoint Automation Services explizit aktivieren. 
  
    
    


## PowerPoint Automation Services-Szenarien
<a name="PAS_Scenarios"> </a>

Die folgenden Szenarien beschreiben einige Möglichkeiten der Verwendung von PowerPoint Automation Services zum Automatisieren der Präsentationsverarbeitung auf einem Server:
  
    
    

- Ein Großunternehmen speichert alle Präsentationen zum Gewinn in einer einzelnen Dokumentbibliothek auf einer Unternehmens-Intranetwebsite. Die Bibliothek enthält zahlreiche Präsentationen, die sich über die Jahre angesammelt haben. Die IT-Abteilung möchte alle Präsentationsdateien im PowerPoint 97-2003-Binärdateiformat (.ppt) in das Open XML-Präsentationsdateiformat (.pptx) umwandeln. Die diese Konvertierung durchführenden Entwickler möchten eine Lösung auf dem Server bereitstellen, die alle Dateien in der Bibliothek durchläuft, prüft, ob die Datei das PPT-Format aufweist, und anschließend eine solche Datei in das PPTX-Dateiformat umwandelt.
    
  
- Eine regionale Vertriebsabteilung bietet maßgeschneiderte Kundendienstangebote für jeden einzelnen Kunden. Jeder Verkäufer überprüft zusammen mit dem Kunden seine Angebote in einem Meeting, entweder persönlich oder online. Nach dem Meeting stellt der Verkäufer eine Kopie des Angebots für den Kunden in Form einer PDF zur Verfügung. Die Abteilung beauftragt einen Anbieter damit, ein benutzerdefiniertes Verb im Menü **Bearbeiten** für die in einer Dokumentbibliothek in ihrem Extranet gespeicherten PowerPoint-Dateien zu erstellen. Durch Klicken auf das Verb führt der Server ein Programm aus, das die PowerPoint-Datei in eine in der gleichen Bibliothek gespeicherte PDF umwandelt.
    
  

### Unterstützte Formate der Quellpräsentation

Die folgenden Formate der Quellpräsentation werden bei der Konvertierung unterstützt:
  
    
    

- Open XML-Präsentationsformat (.pptx)
    
  
- PowerPoint 97-2003-Präsentation (.ppt)
    
  

### Unterstützte Formate des Zieldokuments

Die unterstützten Formate der Zieldokumente umfassen alle unterstützten Formate der Quelldokumente und die folgenden Formate:
  
    
    

- .pptx (Open XML-Präsentationsformat)
    
  
- .pdf
    
  
- .xps (Open XML Paper Specification)
    
  
- .jpg
    
  
- .png (Portable Network Graphics Format)
    
  

### Einschränkungen von PowerPoint Automation Services

PowerPoint Automation Services umfasst keine Funktionen zum Ausdrucken von Dokumenten. Es ist jedoch einfach, PowerPoint-Präsentationsdateien (.ppt und .pptx) in PDF- oder XPS-Dateien zu konvertieren, und sie an einen Drucker zu senden.
  
    
    

## PowerPoint Automation Services-API
<a name="PAS_APIs"> </a>

Sie verwenden für PowerPoint Automation Services die Programmierschnittstelle, um eine Konvertierungsanforderung an den SharePoint Server 2013-Server zu senden. Sie geben für jede Konvertierungsanforderung die zu konvertierenden Dateien und das Ausgabeformat an. Bei bestimmten Konvertierungsanforderungen können Sie auch den zu konvertierenden Inhaltstyp angeben, einschließlich Kommentare, ausgeblendete Folien oder Dokumenteigenschaften.
  
    
    
PowerPoint Automation Services verwendet die asynchrone Mustermethode zum Senden und Empfangen von Konvertierungsanforderungen. Sie können somit einen Code schreiben, mit dem mit der Ausführung fortgefahren wird, nachdem eine Konvertierungsanforderung gesendet wurde. Wenn Sie die Benutzer über den Abschluss einer Konvertierungsanforderung benachrichtigen möchten, können Sie eine Stellvertretung angeben, die auf eine auszuführende Rückrufmethode verweist, sobald der Vorgang abgeschlossen ist. 
  
    
    

> **HINWEIS**
> Weitere Informationen zum Arbeiten mit dem asynchronen Entwurfsmuster finden Sie unter  [Asynchrones Programmiermodell (APM)](http://msdn.microsoft.com/de-de/library/ms228963.aspx). 
  
    
    

Die folgenden Abschnitte beinhalten eine eingeschränkte Liste von Klassen, die zum Senden und Empfangen von Konvertierungsanforderungen erforderlich sind. All diese Klassen sind im **Microsoft.Office.Server.PowerPoint.Conversion**-Namespace enthalten. 
  
    
    

### Request-Basisklasse

Die **Request**-Klasse ist die grundlegendste Klasse im **Microsoft.Office.Server.PowerPoint.Conversion**-Namespace. Alle anderen request-Typen - **PresentationRequest**, **PictureRequest**, **PdfRequest** und **XpsRequest** - erben von dieser.
  
    
    

**Tabelle 1. Request-Basisklassenelemente**


|**Elementname**|**Beschreibung**|
|:-----|:-----|
|**BeginConvert(Microsoft.SharePoint.SPServiceContext, System.AsyncCallback, System.Object)**-Methode  <br/> |Startet den Konvertierungsvorgang. Der erste Parameter,  _serviceContext_, gibt den Kontext der SharePoint-Website an, auf der sich die zu konvertierende Datei befindet. Mit dem  _callback_-Parameter können Sie einen Delegaten festlegen, der auf eine Methode verweist, die nach Abschluss des Vorgangs auszuführen ist. Verwenden Sie den  _state_-Parameter, wenn weitere Informationen aus dem aufrufenden Code an die Rückrufmethode übergeben werden müssen.  <br/> Gibt ein  [IAsyncResult](https://msdn.microsoft.com/library/System.IAsyncResult.aspx) -Objekt zurück. <br/> |
|**EndConvert(IAsyncResult)**-Methode  <br/> |Beendet den Konvertierungsvorgang. Der  _result_-Parameter erwartet das resultierende  [IAsyncResult](https://msdn.microsoft.com/library/System.IAsyncResult.aspx) -Objekt, das von der entsprechenden **BeginConvert**-Konvertierungsanforderung zurückgegeben wird. Wenn diese Anforderung zum Zeitpunkt des Aufrufs von **EndConvert**noch nicht abgeschlossen wurde, wird der aufrufende Thread blockiert, bis der Konvertierungsvorgang abgeschlossen ist.  <br/> Gibt keinen Wert zurück.  <br/> |
   

### PresentationRequest-Klasse

Die **PresentationRequest**-Klasse, die von der **Request**-Klasse erbt, wandelt eine PowerPoint 97-2003-Datei (.ppt) oder Open XML-Präsentationsdatei (.pptx) in ein anderes Dateiformat um. Im ersten oben erwähnten Szenario wird diese Klasse zum Konvertieren älterer Präsentationsdateien in einer Dokumentbibliothek in das Open XML-Präsentationsdateiformat verwendet.
  
    
    
Die Konstruktormethode für die **PresentationRequest**-klasse verfügt über drei erforderliche Parameter: 
  
    
    

-  _input_ Wählt die Datei, die Sie in ein [Stream](https://msdn.microsoft.com/library/System.IO.Stream.aspx) -Objekt konvertiert möchten.
    
  
-  _extension_ Eine Zeichenfolge, die die Dateierweiterung der zu konvertierenden Datei angibt.
    
  
-  _output_ Ein [SPFileStream](http://msdn.microsoft.com/de-de/library/microsoft.sharepoint.spfilestream.aspx)-Objekt, das den Speicherort für die Ausgabe festlegt.
    
  
Die **PresentationRequest**-Klasse verfügt über eine Überladung für die Konstruktormethode, die einen  _settings_-Parameter hinzufügt. Der  _settings_-Parameter lässt ein **PresentationSettings**-Objekt als Argument zu.
  
    
    

> **TIPP**
> Überprüfen Sie beim Konvertieren des Ausgabe- [Stream](https://msdn.microsoft.com/library/System.IO.Stream.aspx) -Objekts zurück in ein [SPFile](http://msdn.microsoft.com/de-de/library/microsoft.sharepoint.spfile.aspx)-Objekt, ob die für die Ausgabedatei angegebene Dateierweiterung korrekt ist (.ppt oder .pptx). 
  
    
    


### PdfRequest-Klasse

Die **PdfRequest**-Klasse, die auch von der **Request**-Klasse erbt, konvertiert eine PowerPoint 97-2003-Datei (.ppt) oder Open XML-Präsentationsdatei (.pptx.) in eine PDF-Datei. Im zweiten oben erwähnten Szenario wird diese Klasse zum Konvertieren von Präsentationen in PDF-Dateien verwendet.
  
    
    
Die Konstruktormethode für die **PdfRequest**-Klasse verfügt, ähnlich wie die **PresentationRequest**-Klasse, ebenfalls über drei erforderliche Parameter:  _input_,  _extension_ und _output_.
  
    
    
Die **PdfRequest**-Klasse verfügt auch über eine Überladung für die Konstruktormethode, die einen  _settings_-Parameter hinzufügt. Der  _settings_-Parameter lässt ein **FixedFormatSettings**-Objekt als Argument zu.
  
    
    

> **TIPP**
> Überprüfen Sie beim Konvertieren des Ausgabe- [Stream](https://msdn.microsoft.com/library/System.IO.Stream.aspx) -Objekts zurück in ein [SPFile](http://msdn.microsoft.com/de-de/library/microsoft.sharepoint.spfile.aspx)-Objekt, ob die für die Ausgabedatei angegebene Dateierweiterung korrekt ist (.pdf). 
  
    
    


### PictureRequest-Klasse

Die **PictureRequest**-Klasse, die auch von der **Request**-Klasse erbt, wandelt eine PowerPoint 97-2003-Datei (.ppt) oder Open XML-Präsentationsdatei File (.pptx) in eine Auflistung von Bilddateien im JPG- oder PNG-Format um.
  
    
    
Die Konstruktormethode für die **PictureRequest**-Klasse verfügt ebenfalls über vier erforderliche Parameter. Die  _input_-,  _extension_- und _output_-Parameter ähneln den Parametern für den **PresentationRequest**-Klassenkonstruktor. Die Konstruktormethode für die **PictureRequest**-Klasse verfügt auch über einen erforderlichen  _format_-Parameter, der eine Konstante aus der **PictureFormat**-Enumeration sein muss.
  
    
    
Die **PictureRequest**-Klasse verfügt über keine Überladungen für ihre Konstruktormethode.
  
    
    

> **TIPP**
> Die **PictureRequest**-Klasse gibt ein Stream mit einem Paket von Bilddateien zurück. Prüfen Sie beim Konvertieren des Ausgabe- [Stream](https://msdn.microsoft.com/library/System.IO.Stream.aspx) -Objekts zurück in ein [SPFile](http://msdn.microsoft.com/de-de/library/microsoft.sharepoint.spfile.aspx)-Objekt, ob die angegebene Dateierweiterung .zip ist. 
  
    
    


## Erstellen einer PowerPoint Automation Services-Anwendung
<a name="PAS_Build"> </a>

Am einfachsten kann das Schreiben von Code, der PowerPoint Automation Services verwendet, anhand der Erstellung einer Konsolenanwendung veranschaulicht werden. Sie müssen die Konsolenanwendung auf dem SharePoint Server, und nicht auf einem Clientcomputer erstellen und ausführen. Der Code zum Starten von Konvertierungsanforderung weicht, abhängig davon, ob der Konvertierungsanforderungscode in einem Webpart, einem Workflow oder einem Ereignishandler integriert ist, nur gering ab. In der folgenden Vorgehensweise wird dargestellt, wie eine API bei der Verwendung von PowerPoint Automation Services aus einer Konsolenanwendung ohne zusätzliche Komplexität eines Webparts, eines Ereignishandlers oder eines Workflows verwendet werden kann.
  
    
    

> **HINWEIS**
> Da PowerPoint Automation Services ein Dienst von SharePoint Server 2013 ist, können Sie diesen nur in einer Anwendung verwenden, die direkt auf einem SharePoint Server ausgeführt wird. Sie müssen die Anwendung als Farmlösung erstellen. PowerPoint Automation Services aus einer Sandkastenlösung kann nicht verwendet werden. 
  
    
    


### So erstellen Sie die Anwendung


1. Starten Sie Microsoft Visual Studio 2012.
    
  
2. Zeigen Sie im Menü **Datei** auf **Neu**, und wählen Sie dann **Projekt**. 
    
  
3. Erweitern Sie im Dialogfenster **Neues Projekt** unter **Installiert** **Vorlagen**, erweitern Sie **Visual C#**, und wählen Sie dann **Windows**.
    
  
4. Wählen Sie in der Liste der Projektvorlagen **Konsolenanwendung**
    
  
5. Stellen Sie sicher, dass das Projekt in Visual Studio auf .NET Framework 4 abzielt.
    
    > **HINWEIS**
      > In den vorherigen Versionen von SharePoint Server war es erforderlich, auf .NET Framework 3.5 abzuzielen. Die Microsoft.SharePoint-Bibliotheken verweisen nun auf Assemblys in .NET Framework 4. Stellen Sie des Weiteren sicher, dass Ihr Projekt auf .NET Framework 4 verweist, und nicht auf .NET Framework 4 Client Profile. 
6. Geben Sie im Feld **Name** den Namen für das Projekt ein, z. B. PAS_Sample.
    
  
7. Geben Sie im Feld **Speicherort** den Speicherort ein, an dem das Projekt abgelegt werden soll.
    
  
8. Klicken Sie zum Erstellen der Projektmappe auf **OK**.
    
  
9. Standardmäßig erstellt Visual Studio 2012 Projekte, die auf x86-CPUs abzielen, zum Erstellen von SharePoint Server-Anwendungen muss jedoch auf alle CPUs abgezielt werden.
    
    Klicken Sie beim Erstellen einer Microsoft Visual C#-Anwendung im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Eigenschaften**.
    
  - Klicken Sie im Projektfenster **Eigenschaften** auf **Build**
    
  
  - Zeigen Sie auf die Liste **Konfiguration**, und wählen Sie **Alle Konfigurationen**.
    
  
  - Zeigen Sie auf die Liste **Zielplattform**, und wählen Sie **Alle CPUs**.
    
  

    
    
    Klicken Sie beim Erstellen einer Microsoft Visual Basic .NET Framework-Anwendung im Fenster **Eigenschaften** auf **Kompilieren**.
    
  - Klicken Sie auf **Erweiterte Kompilierungsoptionen**.
    
  
  - Zeigen Sie auf die Liste **Konfiguration**, und wählen Sie **Alle Konfigurationen**.
    
  
  - Zeigen Sie auf die Liste **Zielplattform**, und klicken Sie dann auf **Alle CPUs**.
    
  

    
    
  
10. Klicken Sie im Menü **Projekt** auf **Verweis hinzufügen**, um das Dialogfeld **Verweis hinzufügen** zu öffnen.
    
  
11. Erweitern Sie **Assemblys**, und führen Sie die folgenden Aktionen durch: 
    
  - Erweitern Sie **Framework**, und fügen Sie dann einen Verweis auf System.Web hinzu.
    
  
  - Erweitern Sie **Erweiterungen**, und fügen Sie dann einen Verweis auf Microsoft.SharePoint hinzu.
    
  
12. Wählen Sie im Dialogfenster **Verweis hinzufügen** **Durchsuchen**, navigieren Sie zum Speicherort für die Datei „Microsoft.Office.Server.PowerPoint.dl" (standardmäßig ist dies C:\\Windows\\Microsoft.NET\\assembly\\GAC_MSIL\\Microsoft.Office.Server.PowerPoint\\v4.0_15.0.0.0__71e9bce111e9429c), wählen Sie die Assembly aus, und wählen Sie dann **Hinzufügen**. 
    
  
Die folgenden C#- und Visual Basic-Beispiele zeigen eine einfache PowerPoint Automation Services-Anwendung, die eine PowerPoint 97-2003-Datei (.ppt) im Ordner „Freigegebene Dokumente" einer SharePoint-Website in eine PowerPoint Open XML-Datei (.pptx) im gleichen Ordner konvertiert.
  
    
    



```cs

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;
using System.Web;
using Microsoft.SharePoint;
using Microsoft.Office.Server.PowerPoint.Conversion;

namespace PAS_Sample
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                string siteURL = "http://localhost";
                using (SPSite site = new SPSite(siteURL))
                {
                    using (SPWeb web = site.OpenWeb())
                    {
                        Console.WriteLine("Begin conversion");

                        // Get a reference to the "Shared Documents" library 
                        // and the presentation file to be converted.
                        SPFolder docs = web.Folders[siteURL + 
                            "/Shared Documents"];
                        SPFile file = docs.Files[siteURL + 
                            "/Shared Documents/Pres1.ppt"];

                        // Convert the file to a stream and create an
                        // SPFileStream object for the conversion output.
                        Stream fStream = file.OpenBinaryStream();
                        SPFileStream stream = new SPFileStream(web, 0x1000);

                        // Create the presentation conversion request.
                        PresentationRequest request = new PresentationRequest(
                            fStream,
                            ".ppt",
                            stream);

                        // Send the request synchronously, passing
                        // in a 'null' value for the callback parameter, 
                        // and capturing the response in the result object.
                        IAsyncResult result = request.BeginConvert(
                            SPServiceContext.GetContext(site),
                            null,
                            null);

                        // Use the EndConvert method to get the result. 
                        request.EndConvert(result);

                        // Add the converted file to the document library.
                        SPFile newFile = docs.Files.Add(
                            "newPres1.pptx", 
                            stream, 
                            true);
                        Console.WriteLine("Output: {0}", newFile.Url);

                    }
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Error: " + ex.Message);
            }
            finally
            {
                Console.WriteLine("Complete");
                Console.ReadKey();
            }
        }
    }
}
```




```VB.net

Imports System
Imports System.Collections.Generic
Imports System.IO
Imports System.Linq
Imports System.Text
Imports System.Web
Imports Microsoft.SharePoint
Imports Microsoft.Office.Server.PowerPoint.Conversion

Namespace PAS_Sample
    Class Program
        Private Shared Sub Main(args As String())
            Try
                Dim siteURL As String = "http://localhost"
                Using site As New SPSite(siteURL)
                    Using web As SPWeb = site.OpenWeb()
                        Console.WriteLine("Begin conversion")

                        ' Get a reference to the "Shared Documents" library 
                        ' and the presentation file to be converted.
                        Dim docs As SPFolder = web.Folders(siteURL + _
                            "/Shared Documents")
                        Dim file As SPFile = docs.Files(siteURL + _
                            "/Shared Documents/Pres1.ppt")

                        ' Convert the file to a stream and create an
                        ' SPFileStream object for the conversion output.
                        Dim fStream As Stream = file.OpenBinaryStream()
                        Dim stream As New SPFileStream(web, &amp;H1000)

                        ' Create the presentation conversion request.
                        Dim request As New PresentationRequest(fStream, _
                            ".ppt", 
                            stream)

                        ' Send the request synchronously, passing
                        ' in a Nothing value for the callback parameter, 
                        ' and capturing the response in the result object.
                        Dim result As IAsyncResult = request.BeginConvert(_
                            SPServiceContext.GetContext(site), _
                            Nothing, _
                            Nothing)

                        ' Use the EndConvert method to get the result. 
                        request.EndConvert(result)

                        ' Add the converted file to the document library.
                        Dim newFile As SPFile = docs.Files.Add(_
                            "newPres1.pptx", _
                            stream, _
                            True)

                        Console.WriteLine("Output: {0}", newFile.Url)
                    End Using
                End Using
            Catch ex As Exception
                Console.WriteLine("Error: " + ex.Message)
            Finally
                Console.WriteLine("Complete")
                Console.ReadKey()
            End Try
        End Sub
    End Class
End Namespace

```


### So erstellen Sie das Beispiel und führen es aus


1. Fügen Sie ein PowerPoint-Dokument mit dem Namen Pres1.ppt zum Ordner „Freigegebene Dokumente" auf der SharePoint-Website hinzu.
    
  
2. Erstellen Sie das Beispiel, und führen Sie es aus.
    
  
3. Warten Sie eine Minute ab, bis der Konvertierungsprozess ausgeführt wird, navigieren Sie zum Ordner „Freigegebene Dokumente" auf einer SharePoint-Website, und aktualisieren Sie die Seite. Die Dokumentbibliothek enthält nun ein neues PowerPoint-Dokument, Pres1.pptx.
    
  
PowerPoint Automation Services auf SharePoint Server 2013 bietet Unternehmen erweiterte Funktionen für die Verwaltung der Präsentationsdateien. Diese leistungsfähige Lösung ermöglicht eine skalierbare, serverseitige Präsentationsbearbeitung und -generierung, sowohl als Batch als auch bedarfsgesteuert. 
  
    
    

> **HINWEIS**
>  Stellen Sie vor der Ausführung des Beispiels sicher, dass PowerPoint Automation Services in der SharePoint-Zentraladministrationskonsole aktiviert ist.>  Führen Sie die folgenden Aktionen durch, um sicherzustellen, dass PowerPoint Automation Services aktiviert ist:>  Wählen Sie in der Zentraladministrationskonsole unter **Systemeinstellungen** **Dienste auf dem Server verwalten**, und vergewissern Sie sich dann, dass der **PowerPoint-Konvertierungsdienst** auf **Gestartet** festgelegt ist.>  Wählen Sie in der Zentraladministrationskonsole unter **Anwendungsverwaltung** **Dienstanwendungen verwalten**, und vergewissern Sie sich dann, dass die **PowerPoint-Konvertierungsdienstanwendung** und der **Proxy der PowerPoint-Konvertierungsdienstanwendung** auf „Gestartet" festgelegt sind.
  
    
    


## Schlussbemerkung
<a name="PAS_Conclusion"> </a>

PowerPoint Automation Services auf SharePoint Server 2013 bietet Unternehmen erweiterte Funktionen für die Verwaltung der Präsentationsdateien. Diese leistungsfähige Lösung ermöglicht eine skalierbare, serverseitige Präsentationsbearbeitung und -generierung, sowohl als Batch als auch bedarfsgesteuert. 
  
    
    

## Weitere Ressourcen
<a name="PAS_Additional"> </a>


-  [Entwickeln mit SharePoint 2010 Word Automation Services](http://msdn.microsoft.com/de-de/library/ff742315.aspx)
    
  
-  [PowerPoint Developer Center](http://msdn.microsoft.com/de-de/office/aa905465)
    
  
-  [SharePoint Developer Center](http://msdn.microsoft.com/de-de/sharepoint/default.aspx)
    
  

