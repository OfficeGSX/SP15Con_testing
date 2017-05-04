---
title: Vorgehensweise erstellen Bericht-Editoren für PerformancePoint Services in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: b42b4452-90f8-464c-828f-d3abac40670c
---


# Vorgehensweise: erstellen Bericht-Editoren für PerformancePoint Services in SharePoint 2013
In diesem Artikel erfahren Sie, wie die Editor-Komponente einer benutzerdefinierten Berichtserweiterung für PerformancePoint-Dienste erstellt wird.
## Was sind benutzerdefinierte Bericht-Editoren für PerformancePoint-Dienste ?
<a name="bi_intro"> </a>

Benutzerdefinierter Bericht-Editoren aktivieren PerformancePoint-Dienste Benutzer Eigenschaften für benutzerdefinierte Berichte fest. Bericht-Editoren initialisieren auch den berichtendpunkt, der Parameterwerte von Scorecard- und filteranbietern empfängt. Weitere Informationen zu Editor Anforderungen und Funktionen finden Sie unter  [Editoren für benutzerdefinierte PerformancePoint Services-Objekte](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx).
  
    
    
Die folgenden Prozeduren und Beispielen basieren auf der **SampleReportViewEditor** -Klasse aus der [benutzerdefinierten Objekte (Beispiel)](http://msdn.microsoft.com/library/af021d52-7562-4e7a-9de4-e1fc5784a59d%28Office.15%29.aspx). Der Editor ist eine dünne Anwendung, die Benutzern ermöglicht, Namen und eine Beschreibung des Berichts zu ändern. Den vollständigen Code für die Klasse, finden Sie unter  [Codebeispiel: erstellen, abrufen und Aktualisieren von benutzerdefinierten PerformancePoint-Dienste Berichte in SharePoint Server 2013](#bk_example).
  
    
    
Es wird empfohlen, dass Sie den Beispiel-Editor als Vorlage verwenden. Das Beispiel zeigt wie Objekte aufrufen PerformancePoint-Dienste-API bietet Hilfsobjekte, die vereinfachen von Anrufen für Repository-Vorgänge (wie erstellen und Aktualisieren der Objekte), und bewährte Methoden für die Entwicklung von PerformancePoint-Dienste veranschaulicht.
  
    
    

## Erstellen von Editoren für benutzerdefinierte PerformancePoint-Dienste Berichte
<a name="BKMK_ConfigREditor"> </a>


  
    
    

1. Installieren Sie PerformancePoint-Dienste, oder kopieren Sie die DLLs, die die Erweiterung verwendet (siehe Schritt 3) auf Ihrem Computer. Weitere Informationen finden Sie unter  [PerformancePoint-Dienste-DLLs in Entwicklungsszenarios](http://msdn.microsoft.com/library/41e92619-8253-481d-82f9-35b6a6abc477%28Office.15%29.aspx).
    
  
2. Erstellen Sie in Visual Studio eine C#-Klassenbibliothek. Sollten Sie bereits eine Klassenbibliothek für die Erweiterung erstellt haben, fügen Sie eine neue C#-Klasse hinzu.
    
    Sie müssen die DLL mit einem starken Namen signieren. Stellen Sie außerdem sicher, dass alle Assemblys, auf die von der DLL verwiesen wird, ebenfalls starke Namen haben. Informationen dazu, wie Sie eine Assembly mit einem starken Namen signieren und ein öffentliches/privates Schlüsselpaar erstellen, finden Sie unter  [How to: Create a public/private key pair](http://msdn.microsoft.com/library/05026813-f3bd-4d7c-9e0b-fc588eb3d114.aspx).
    
  
3. Fügen Sie dem Projekt die folgenden DLLs als Assemblyverweise hinzu:
    
  - Microsoft.PerformancePoint.Scorecards.Client.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerCommon.dll
    
  
  - **Microsoft.PerformancePoint.Scorecards.Store.dll** (von Hilfsklassen verwendet)
    
  
  - **Microsoft.SharePoint.dll** (von Hilfsklassen verwendet)
    
  

    Der Beispiel-Editor enthält ebenfalls Assemblyverweise auf **System.Web.dll** und **System.Web.Services.dll**. Je nach Funktionalität Ihrer Erweiterung sind u. U. weitere Projektverweise erforderlich.
    
  
4. Fügen Sie dem Projekt die folgenden Klassen aus dem Beispiel. Im Editor wird für die Interaktion mit dem Repository PerformancePoint-Dienste Hilfsklassen verwendet:
    
  - DataSourceConsumerHelper.cs
    
  
  - ExtensionRepositoryHelper.cs
    
  
  - ReportViewRepositoryHelper.cs
    
  
  - IDataSourceConsumer.cs
    
  

    > **HINWEIS**
      > Der Beispielbericht erhält Daten aus einen Filter, damit es nicht **DataSourceConsumerHelper** oder **IDataSourceConsumer** -Objekten verwendet wird. Wenn Ihr Bericht Daten aus einer Datenquelle PerformancePoint-Dienste erhält, können Sie die Methoden verwenden, die von der abzurufenden Datenquellen wie beschrieben in [Vorgehensweise: Erstellen Filter-Editoren für PerformancePoint Services in SharePoint 2013](how-to-create-filter-editors-for-performancepoint-services-in-sharepoint-2013.md) **DataSourceConsumerHelper** -Klasse verfügbar gemacht werden.
5. Fügen Sie in der Editorklasse **using** Direktiven für die folgenden PerformancePoint-Dienste-Namespaces hinzu:
    
  - **Microsoft.PerformancePoint.Scorecards**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerCommon**
    
  

    Je nach Funktionalität der Erweiterung sind u. U. andere **using**-Direktiven erforderlich.
    
  
6. Übernehmen Sie von der Basisklasse, die die Implementierung Ihres Editors unterstützt. Da es sich bei dem Beispielbericht-Editor um eine Webanwendung handelt, erbt diese von der  [Page](https://msdn.microsoft.com/library/System.Web.UI.Page.aspx) -Klasse. Andere Implementierungen können von Basisklassen wie der [UserControl](https://msdn.microsoft.com/library/System.Windows.Forms.UserControl.aspx) -Klasse oder der [WebPart](https://msdn.microsoft.com/library/System.Web.UI.WebControls.WebParts.WebPart.aspx) -Klasse erben.
    
  
7. Deklarieren Sie Variablen für die Steuerelemente, die die Eigenschaften offenlegen, die Benutzer anzeigen oder ändern sollen. Vom Beispielbericht-Editor werden zunächst Variablen für die Webserversteuerelemente deklariert, die in der Benutzeroberflächenkomponente - einer ASPX-Seite - definiert sind. Vom Beispiel-Editor wird auch ein Schaltflächen-Steuerelement definiert, das es Benutzern ermöglicht, Änderungen zu übermitteln. Der Editor ruft dann die  [CreateChildControls()](https://msdn.microsoft.com/library/System.Web.UI.Control.CreateChildControls.aspx) -Methode auf, um die Steuerelemente auf der Seite zur Verfügung zu stellen.
    
    > **HINWEIS**
      > Im Editor wird die Programmierlogik separat von der Benutzeroberfläche definiert. Anweisungen zum Erstellen der Benutzeroberflächenkomponenten würden den Rahmen dieser Dokumentation sprengen.

    Beispiel-Bericht-Editors führt die Schritte 8 bis 12 in der **Page_Load** -Methode. **Page_Load** wird auch initialisieren und Variablen und Steuerelemente zu überprüfen, füllen Sie Steuerelemente und Statusinformationen für den benutzerdefinierten Bericht und Helper-Objekten zu speichern.
    
  
8. Legen Sie die  [AllowUnsafeUpdates](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ServerCommon.ServerUtils.AllowUnsafeUpdates.aspx) -Eigenschaft auf **true**. Auf diese Weise können die Bericht-Editors zum Schreiben von Daten an das Repository ohne Verwendung des Formulars **POST** Vorgänge.
    
  
9. Rufen Sie die Parameter aus der Abfragezeichenfolge ab, und legen Sie sie als Werte für lokale Variablen fest, wie im folgenden Codebeispiel dargestellt.
    
  ```cs
  
// The URL of the site collection that contains the PerformancePoint Services repository.
string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];

// The location of the report in the repository.
string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];

// The operation to perform: OpenItem or CreateItem.
string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];
  ```


Informationen zu den Abfragezeichenfolgenparametern finden Sie unter  [Editoren für benutzerdefinierte PerformancePoint Services-Objekte](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx).
    
  
10. Rufen Sie das **ReportViewRepositoryHelper**-Objekt ab, das für Aufrufe an das Repository verwendet wird, wie im folgenden Codebeispiel veranschaulicht.
    
  ```cs
  
reportviewRepositoryHelper = new ReportViewRepositoryHelper();
  ```

11. Legen Sie den Speicherort für den Bericht basierend auf dem Abfragezeichenfolgenparameter fest, wie im folgenden Codebeispiel veranschaulicht.
    
  ```cs
  RepositoryLocation repositoryReportViewLocation = RepositoryLocation.CreateFromUriString(itemLocation);
  ```

12. Vorgang ( _OpenItem_ oder _CreateItem_) aus der Abfragezeichenfolge abzurufen und abzurufen oder einen benutzerdefinierten Bericht erstellen.
    
  - Zum Abrufen des benutzerdefinierten Berichts verwenden Sie die **ReportViewRepositoryHelper.Get**-Methode.
    
  
  - Zum Erstellen des benutzerdefinierten Berichts verwenden Sie den **ReportView()**-Konstruktor, und definieren Sie dann die Eigenschaften  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) , [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.RendererClassName.aspx) und [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.SubTypeId.aspx) des Berichts.
    
     [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.SubTypeId.aspx) ist der eindeutige Bezeichner für den Bericht, und es muss übereinstimmen, das **subType** -Attribut, das Sie für den benutzerdefinierten Bericht in der PerformancePoint-Dienste web.config-Datei angeben. [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.RendererClassName.aspx) ist den vollqualifizierten Namen der Klasse, die das Webserversteuerelement Renderer definiert. Wenn im Editor nicht definiert ist, ist die Standardeinstellung der Rendererklasse in der Datei web.config angegeben.
    
  

  ```cs
  if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
    {

        // Use the repository-helper object to retrieve the report.
        reportview = reportviewRepositoryHelper.Get(repositoryReportViewLocation);
        if (reportview == null)
        {
            displayError("Could not retrieve the report view for editing.");
            return;
        }
    }
    else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
    {
        reportview = new ReportView
        {
            RendererClassName = typeof(SampleReportRenderer).AssemblyQualifiedName, 
            SubTypeId = "SampleReportView"
        };
    }
    else
    {
        displayError("Invalid Action.");
        return;
    }
  ```


    > **HINWEIS**
      > Standardmäßig können Benutzer von PerformancePoint Dashboard-Designer nur benutzerdefinierte Objekte erstellen. Um Benutzern das Erstellen eines benutzerdefinierten Objekts außerhalb Dashboard-Designer zu aktivieren, müssen Sie ein Menüelement hinzufügen, die vom Inhaltstyp im Repository eine  _CreateItem_ -Anforderung an den Editor sendet. Weitere Informationen finden Sie unter [Editoren für benutzerdefinierte PerformancePoint Services-Objekte](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx).
13. Definieren Sie den Endpunkt des Berichts, damit der Bericht Daten von Filtern und Scorecards empfangen kann. Die erforderlichen Eigenschaften für den Endpunkt werden vom Beispielbericht-Editor definiert, wie im folgenden Codebeispiel veranschaulicht.
    
  ```cs
  
if (0 == reportview.EndPoints.Count)
{
    EndPoint endpoint = new EndPoint
    {
        Category = EndPointCategory.None,
        UniqueName = "SampleReportView_EndPoint",

        // The display name is shown to users in Dashboard Designer.
        // It represents the endpoint that can be connected 
        // to a filter or scorecard.
        DisplayName = "Sample Report View EndPoint"
    };

    reportview.EndPoints.Add(endpoint);
}
  ```


Der Endpunkt wird vom Beispiel-Editor in der **VerifyReportView**-Methode definiert. Darüber hinaus wird **VerifyReportView** definiert, um zu überprüfen, ob die erforderlichen Eigenschaften festgelegt wurden, und um die optionale [CustomData](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ReportView.CustomData.aspx) -Eigenschaft zu definieren, die Sie zum Speichern von Informationen für Ihren Bericht verwenden können.
    
  
14. Aktualisieren Sie den Bericht durch benutzerdefinierte Änderungen. Die **ReportViewRepositoryHelper.Update**-Methode wird von der **buttonOK_Click**-Methode im Beispielbericht-Editor aufgerufen, um die Eigenschaften  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) und [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) des Berichts im Repository zu aktualisieren. **buttonOK_Click** wird ebenfalls zum Überprüfen der Inhalte der Steuerelemente und zum Abrufen von Statusinformationen für den benutzerdefinierten Bericht und das Hilfsobjekt verwendet.
    
    > **HINWEIS**
      > Benutzer können eines benutzerdefinierten Objekts  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) , [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) und [Owner](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Owner.aspx) ( **Verantwortliche Person** ) Eigenschaften bearbeiten und benutzerdefinierte Objekte direkt in Dashboard-Designer und die PerformancePoint-Dienste-Repository löschen.

## Codebeispiel: erstellen, abrufen und Aktualisieren von benutzerdefinierten PerformancePoint-Dienste Berichte in SharePoint Server 2013
<a name="bk_example"> </a>

Im folgenden Codebeispiel wird erstellt, abgerufen und aktualisiert die benutzerdefinierte Berichte. Dieser Code hat ihren Ursprung im Editor CodeBehind-Klasse, die die Programmierlogik für Steuerelemente bereitstellt, die in einer ASPX-Seite definiert sind.
  
    
    
Bevor Sie dieses Codebeispiel kompilieren können, müssen Sie Ihre Entwicklungsumgebung konfigurieren, wie beschrieben in  [Erstellen von Editoren für benutzerdefinierte PerformancePoint-Dienste Berichte](#BKMK_ConfigREditor).
  
    
    



```cs

using System;
using System.Web.UI;
using System.Web.UI.WebControls;
using Microsoft.PerformancePoint.Scorecards;
using Microsoft.PerformancePoint.Scorecards.ServerCommon;

namespace Microsoft.PerformancePoint.SDK.Samples.SampleReport
{

    // Represents the class that defines the sample report editor. 
    public class SampleReportViewEditor : Page
    {

        // Declare private variables for the ASP.NET controls defined in the user interface.
        // The sample's user interface is an ASPX page that defines the controls in HTML.
        private TextBox textboxName;
        private TextBox textboxDescription;
        private Label labelErrorMessage;
        private Button buttonOK;

        // Make the controls available to this class.
        protected override void CreateChildControls()
        {
            base.CreateChildControls();

            if (null == textboxName)
                textboxName = FindControl("textboxName") as TextBox;
            if (null == textboxDescription)
                textboxDescription = FindControl("textboxDescription") as TextBox;
            if (null == labelErrorMessage)
                labelErrorMessage = FindControl("labelErrorMessage") as Label;
            if (null == buttonOK)
                buttonOK = FindControl("buttonOK") as Button;
        }

        // Handles the Load event of the Page control.
        // Methods that use a control variable should call the Control.EnsureChildControls
        // method before accessing the variable for the first time.
        protected void Page_Load(object sender, EventArgs e)
        {

            // Required to enable custom report and filter editors to
            // write data to the repository.
            ServerUtils.AllowUnsafeUpdates = true;

            // Initialize controls the first time the page loads only.
            if (!IsPostBack)
            {
                EnsureChildControls();
                ReportViewRepositoryHelper reportviewRepositoryHelper = null;
                try
                {

                    // Get information from the query string parameters.
                    string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];
                    string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];
                    string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];

                    // Validate the query string parameters.
                    if (string.IsNullOrEmpty(server) ||
                        string.IsNullOrEmpty(itemLocation) ||
                        string.IsNullOrEmpty(action))
                    {
                        displayError("Invalid URL.");
                        return;
                    }

                    // Retrieve the repository-helper object.
                    reportviewRepositoryHelper =
                        new ReportViewRepositoryHelper();

                    // Set the report location by using the location from the query string.
                    RepositoryLocation repositoryReportViewLocation = RepositoryLocation.CreateFromUriString(itemLocation);

                    ReportView reportview;

                    // Retrieve or create the report object, depending on the operation
                    // passed in the query string (OpenItem or CreateItem).
                    if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Retrieve the report object by using the repository-helper object.
                        reportview = reportviewRepositoryHelper.Get(repositoryReportViewLocation);
                        if (reportview == null)
                        {
                            displayError("Could not retrieve the report view for editing.");
                            return;
                        }
                    }
                    else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Create a report view.
                        // CreateItem requests can be sent from a SharePoint list, but
                        // you must create a custom menu item to send the request.
                        // Dashboard Designer can send edit requests only.
                        reportview = new ReportView
                        {
                            RendererClassName = typeof(SampleReportRenderer).AssemblyQualifiedName, 
                            SubTypeId = "SampleReportView"
                        };
                    }
                    else
                    {
                        displayError("Invalid Action.");
                        return;
                    }

                    VerifyReportView(reportview);

                    // Save the original report and helper objects across page postbacks.
                    ViewState["action"] = action;
                    ViewState["reportview"] = reportview;
                    ViewState["reportviewrepositoryhelper"] = reportviewRepositoryHelper;
                    ViewState["itemlocation"] = itemLocation;

                    // Populate the child controls.
                    textboxName.Text = reportview.Name.ToString();
                    textboxDescription.Text = reportview.Description.ToString();
                }
                catch (Exception ex)
                {
                    displayError("An error has occurred. Please contact your administrator for more information.");
                    if (reportviewRepositoryHelper != null)
                    {

                        // Add the exception detail to the server event log.
                        reportviewRepositoryHelper.HandleException(ex);
                    }
                }
            }
        }

        // Handles the Click event of the buttonOK control.
        protected void buttonOK_Click(object sender, EventArgs e)
        {
            EnsureChildControls();

            // Verify that the textboxName control contains a value.
            if (string.IsNullOrEmpty(textboxName.Text))
            {
                labelErrorMessage.Text = "A report view name is required.";
                return;
            }

            // Clear any pre-existing error message.
            labelErrorMessage.Text = string.Empty;

            // Retrieve the report and helper objects from view state.
            string action = (string)ViewState["action"];
            string itemLocation = (string) ViewState["itemlocation"];
            ReportView reportview = (ReportView)ViewState["reportview"];
            ReportViewRepositoryHelper reportviewRepositoryHelper = (ReportViewRepositoryHelper)ViewState["reportviewrepositoryhelper"];

            // Update the report object with form changes.
            reportview.Name.Text = textboxName.Text;
            reportview.Description.Text = textboxDescription.Text;

            // Save the report object to the PerformancePoint Services repository.
            try
            {
                reportview.Validate();
                if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                {
                    reportview.CreatedDate = DateTime.Now;
                    ReportView newReportView = reportviewRepositoryHelper.Create(
                        string.IsNullOrEmpty(reportview.Location.ItemUrl) ? itemLocation : reportview.Location.ItemUrl, 
                        reportview);
                    ViewState["reportview"] = newReportView;
                    ViewState["action"] = ClickOnceLaunchValues.OpenItem;
                }
                else
                {
                    reportviewRepositoryHelper.Update(reportview);
                }
            }
            catch (Exception ex)
            {
                displayError("An error has occurred. Please contact your administrator for more information.");
                if (reportviewRepositoryHelper != null)
                {
                    // Add the exception detail to the server event log.
                    reportviewRepositoryHelper.HandleException(ex);
                }
            }
        }

        // Displays the error string in the labelErrorMessage label. 
        void displayError(string msg)
        {
            EnsureChildControls();

            labelErrorMessage.Text = msg;

            // Disable the OK button because the page is in an error state.
            buttonOK.Enabled = false;
            return;
        }

        // Verifies that the properties for the report object are set.
        static void VerifyReportView(ReportView reportview)
        {

            // Verify that all required properties are set. 
            if (string.IsNullOrEmpty(reportview.SubTypeId))
            {

                // This value must match the subType attribute specified
                // in the web.config file.
                reportview.SubTypeId = "SampleReportView";
            }

            if (string.IsNullOrEmpty(reportview.RendererClassName))
            {
                reportview.RendererClassName = typeof (SampleReportRenderer).AssemblyQualifiedName;
            }

            // Reports are consumers and do not use provider endpoints.
           reportview.BeginPoints.Clear();

            // If there are no consumer endpoints, create one so we can connect a filter or a scorecard to the report.
            if (0 == reportview.EndPoints.Count)
            {
                EndPoint endpoint = new EndPoint
                {
                    Category = EndPointCategory.None,
                    UniqueName = "SampleReportView_EndPoint",

                    // The display name is shown to users in Dashboard
                    // Designer to represent the endpoint that can be 
                    // connected to a filter or scorecard.
                    DisplayName = "Sample Report View EndPoint"
                };

                reportview.EndPoints.Add(endpoint);
            }
            
            // Set optional properties for reports.
            reportview.CustomData = "You can use this property to store custom information for this filter as a string or a serialized object.";
        }
    }
}
```


## Nächste Schritte
<a name="bk_next"> </a>

Nach dem Erstellen Sie eines Bericht-Editors (einschließlich der Benutzeroberfläche, falls erforderlich) und eines berichtrenderers, Bereitstellen Sie die Erweiterung, wie in  [Gewusst wie: Manuelles Registrieren von PerformancePoint-Dienste-Erweiterungen](http://msdn.microsoft.com/library/3aa6d340-4b05-46b3-9648-2b6e18e04e09%28Office.15%29.aspx)beschrieben.
  
    
    

## Zusätzliche Ressourcen
<a name="bk_addResources"> </a>


-  [Vorgehensweise: Erstellen von berichtsrenderern für PerformancePoint Services in SharePoint 2013](how-to-create-report-renderers-for-performancepoint-services-in-sharepoint-2013.md)
    
  
-  [PerformancePoint-Dienste in SharePoint 2013](performancepoint-services-in-sharepoint-2013.md)
    
  

