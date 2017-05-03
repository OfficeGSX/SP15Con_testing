---
title: Vorgehensweise Erstellen Filter-Editoren für PerformancePoint Services in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 51354c88-4330-40f0-bffa-1ce73f9ff298
---


# Vorgehensweise: Erstellen Filter-Editoren für PerformancePoint Services in SharePoint 2013
In diesem Artikel erfahren Sie, wie die Editor-Komponente einer benutzerdefinierten Filtererweiterung für PerformancePoint-Dienste erstellt wird.
## Was sind benutzerdefinierte Filter-Editoren für PerformancePoint-Dienste ?
<a name="bk_intro"> </a>

In PerformancePoint-Dienste aktivieren Sie benutzerdefinierte Filter-Editoren Benutzer Eigenschaften für benutzerdefinierte Filter fest. Filter-Editoren müssen auch  [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) -Eigenschaft des Filters, initialisieren, die den filteranfangspunkt definiert, der Parameterwerte für Scorecard- und berichtsconsumer enthält. Weitere Informationen zu Editor Anforderungen und Funktionen finden Sie unter [Editoren für benutzerdefinierte PerformancePoint Services-Objekte](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx).
  
    
    
Die folgenden Prozeduren und Beispielen basieren auf der **SampleFilterEditor** -Klasse aus der [benutzerdefinierten Objekte (Beispiel)](http://msdn.microsoft.com/library/af021d52-7562-4e7a-9de4-e1fc5784a59d%28Office.15%29.aspx). Der Editor ist eine dünne Anwendung, die Benutzern ermöglicht, ändern Name und Beschreibung des Filters und die zugrunde liegenden Datenquelle auswählen. Den vollständigen Code für die Klasse, finden Sie unter  [Codebeispiel: erstellen, abrufen und Aktualisieren von benutzerdefinierten PerformancePoint-Dienste Filter in SharePoint Server 2013](#bk_example).
  
    
    
Es wird empfohlen, dass Sie den Beispiel-Editor als Vorlage verwenden. Das Beispiel zeigt wie Objekte aufrufen PerformancePoint-Dienste-API bietet Hilfsobjekte, die vereinfachen von Anrufen für Repository-Vorgänge (wie erstellen und Aktualisieren der Objekte), und bewährte Methoden für die Entwicklung von PerformancePoint-Dienste veranschaulicht.
  
    
    

## Erstellen Sie und konfigurieren Sie die Editorklasse für benutzerdefinierte PerformancePoint-Dienste Filter
<a name="bk_intro"> </a>


  
    
    

1. Installieren Sie PerformancePoint-Dienste, oder kopieren Sie die DLLs, die die Erweiterung verwendet (siehe Schritt 3) auf Ihrem Computer. Weitere Informationen finden Sie unter  [PerformancePoint-Dienste-DLLs in Entwicklungsszenarios](http://msdn.microsoft.com/library/41e92619-8253-481d-82f9-35b6a6abc477%28Office.15%29.aspx).
    
  
2. Erstellen Sie in Visual Studio eine C#-Klassenbibliothek. Wenn Sie eine Klassenbibliothek für die Erweiterung erstellt haben, fügen Sie eine neue C#-Klasse.
    
    Sie müssen die DLL mit einem starken Namen signieren. Stellen Sie außerdem sicher, dass alle Assemblys, auf die von der DLL verwiesen wird, ebenfalls starke Namen haben. Informationen dazu, wie Sie eine Assembly mit einem starken Namen signieren und ein öffentliches/privates Schlüsselpaar erstellen, finden Sie unter  [How to: Create a public/private key pair](http://msdn.microsoft.com/library/05026813-f3bd-4d7c-9e0b-fc588eb3d114.aspx).
    
  
3. Fügen Sie dem Projekt die folgenden DLLs als Assemblyverweise hinzu:
    
  - Microsoft.PerformancePoint.Scorecards.Client.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerCommon.dll
    
  
  - Microsoft.PerformancePoint.Scorecards.ServerRendering.dll
    
  
  - **Microsoft.PerformancePoint.Scorecards.Store.dll** (von Hilfsklassen verwendet)
    
  
  - **Microsoft.SharePoint.dll** (von Hilfsklassen verwendet)
    
  

    Der Beispiel-Editor enthält ebenfalls Assemblyverweise auf **System.Web.dll** und **System.Web.Services.dll**. Je nach Funktionalität Ihrer Erweiterung sind u. U. weitere Projektverweise erforderlich.
    
  
4. Fügen Sie dem Projekt die folgenden Klassen aus dem Beispiel. Im Editor wird für die Interaktion mit dem Repository PerformancePoint-Dienste Hilfsklassen verwendet:
    
  - DataSourceConsumerHelper.cs
    
  
  - ExtensionRepositoryHelper.cs
    
  
  - FilterRepositoryHelper.cs
    
  
  - IDataSourceConsumer.cs
    
  
5. Fügen Sie in der Editorklasse **using** Direktiven für die folgenden PerformancePoint-Dienste-Namespaces hinzu:
    
  - **Microsoft.PerformancePoint.Scorecards**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerCommon**
    
  
  - **Microsoft.PerformancePoint.Scorecards.ServerRendering**
    
  

    Je nach Funktionalität der Erweiterung sind u. U. andere **using**-Direktiven erforderlich.
    
  
6. Erben Sie von der Basisklasse, die Ihre Editorimplementierung unterstützt. Da es sich beim Beispiel-Filter-Editor um eine Webanwendung handelt, wird von der  [Page](https://msdn.microsoft.com/library/System.Web.UI.Page.aspx) -Klasse geerbt. Andere Implementierungen können von Basisklassen wie z. B. der [UserControl](https://msdn.microsoft.com/library/System.Windows.Forms.UserControl.aspx) - oder [WebPart](https://msdn.microsoft.com/library/System.Web.UI.WebControls.WebParts.WebPart.aspx) -Klasse abgeleitet werden.
    
  
7. Definieren Sie Steuerelemente, mit denen die Eigenschaften verfügbar gemacht werden, die die Benutzer anzeigen oder ändern sollen. Mit dem Beispiel-Filter-Editor werden zunächst Variablen für die Webserversteuerelemente deklariert, die in der Benutzeroberflächenkomponente definiert sind, wobei es sich um eine ASPX-Seite handelt. Außerdem wird mit dem Beispieleditor ein Schaltflächensteuerelement definiert, mit dem Benutzer Änderungen senden können. Anschließend ruft der Editor die  [CreateChildControls()](https://msdn.microsoft.com/library/System.Web.UI.Control.CreateChildControls.aspx) -Methode auf, um die Steuerelemente auf der Seite verfügbar zu machen.
    
    > **HINWEIS**
      > Im Editor wird die Programmierlogik separat von der Benutzeroberfläche definiert. Anweisungen zum Erstellen der Benutzeroberflächenkomponenten würden den Rahmen dieser Dokumentation sprengen.

    Im Beispiel Filter-Editor führt die Schritte 8 bis 12 in der **Page_Load** -Methode. **Page_Load** wird auch initialisieren und Überprüfen von Variablen und Steuerelemente, füllen Sie Steuerelemente und Statusinformationen für die benutzerdefinierte Filter und Hilfsprogramm-Objekte zu speichern.
    
  
8. Legen Sie die  [AllowUnsafeUpdates](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ServerCommon.ServerUtils.AllowUnsafeUpdates.aspx) -Eigenschaft auf **true** fest. Dadurch können mit dem Filter-Editor Daten in das Repository geschrieben werden, ohne POST-Vorgänge des Formulars zu verwenden.
    
  
9. Rufen Sie die Parameter aus der Abfragezeichenfolge ab, und legen Sie sie als Werte für lokale Variablen fest, wie im folgenden Codebeispiel dargestellt.
    
  ```cs
  
// The URL of the site collection that contains the PerformancePoint Services repository.
string server = Request.QueryString[ClickOnceLaunchKeys.SiteCollectionUrl];

// The location of the filter in the repository.
string itemLocation = Request.QueryString[ClickOnceLaunchKeys.ItemLocation];

// The operation to perform: OpenItem or CreateItem.
string action = Request.QueryString[ClickOnceLaunchKeys.LaunchOperation];
  ```


  ```VB.net
  
' The URL of the site collection that contains the PerformancePoint Services repository.
Dim server As String = Request.QueryString(ClickOnceLaunchKeys.SiteCollectionUrl)

' The location of the filter in the repository.
Dim itemLocation As String = Request.QueryString(ClickOnceLaunchKeys.ItemLocation)

' The operation to perform: OpenItem or CreateItem.
Dim action As String = Request.QueryString(ClickOnceLaunchKeys.LaunchOperation)
  ```


    Informationen zu den Abfragezeichenfolgenparametern finden Sie unter  [Editoren für benutzerdefinierte PerformancePoint Services-Objekte](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx).
    
  
10. Rufen Sie das **FilterRepositoryHelper**-Objekt ab, das für Aufrufe an das Repository verwendet wird, wie im folgenden Codebeispiel illustriert.
    
  ```cs
  
filterRepositoryHelper = new FilterRepositoryHelper();
  ```


  ```VB.net
  filterRepositoryHelper = New FilterRepositoryHelper()
  ```

11. Legen Sie wie im folgenden Codebeispiel dargestellt den Filter für die Datenquelle basierend auf dem Abfragezeichenfolgen-Parameter fest.
    
  ```cs
  RepositoryLocation repositoryFilterLocation = RepositoryLocation.CreateFromUriString(itemLocation);
  ```


  ```VB.net
  Dim repositoryFilterLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(itemLocation)
  ```

12. Vorgang ( _OpenItem_ oder _CreateItem_) aus der Abfragezeichenfolge abzurufen und abzurufen oder den benutzerdefinierten Filter zu erstellen.
    
  - Verwenden Sie die **FilterRepositoryHelper.Get**-Methode zum Abrufen des benutzerdefinierten Filters.
    
  
  - Um den benutzerdefinierten Filter zu erstellen, verwenden Sie den Konstruktor **Filter()**, und definieren Sie Eigenschaften für den Filter [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) , [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.RendererClassName.aspx) und [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.SubTypeId.aspx) . [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.SubTypeId.aspx) ist der eindeutige Bezeichner für den Filter, und es muss übereinstimmen, das **subType** -Attribut, das Sie für den benutzerdefinierten Filter in der PerformancePoint-Dienste web.config-Datei angeben. [RendererClassName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.RendererClassName.aspx) ist den vollqualifizierten Namen der Klasse, die das Webserversteuerelement Renderer definiert. Wenn es nicht im Editor definiert ist, ist die Standardeinstellung der Rendererklasse, die in der Datei web.config angegeben ist.
    
  

  ```cs
  if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
{

                // Use the repository-helper object to retrieve the filter.
                filter = filterRepositoryHelper.Get(repositoryFilterLocation);
                if (filter == null)
                {
                    displayError("Could not retrieve the filter for editing.");
                    return;
                }
 }
else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
{
                filter = new Filter
                {
                    RendererClassName = typeof(MultiSelectTreeViewControl).AssemblyQualifiedName,
                    SubTypeId = "SampleFilter"
                };
}
  ```


  ```VB.net
  
If ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then

' Use the repository-helper object to retrieve the filter.
      filter = filterRepositoryHelper.Get(repositoryFilterLocation)
      If filter Is Nothing Then
           displayError("Could not retrieve the filter for editing.")
           Return
      End If

ElseIf ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then
      filter = New Filter With {.RendererClassName = GetType(MultiSelectTreeViewControl).AssemblyQualifiedName, .SubTypeId = "SampleFilter"}
End If
  ```


    > **HINWEIS**
      <br/> Standardmäßig können Benutzer von PerformancePoint Dashboard-Designer nur benutzerdefinierte Objekte erstellen. Um Benutzern das Erstellen eines benutzerdefinierten Objekts außerhalb Dashboard-Designer zu aktivieren, müssen Sie ein Menüelement hinzufügen, die vom Inhaltstyp im Repository eine  _CreateItem_ -Anforderung an den Editor sendet. Weitere Informationen finden Sie unter [Editoren für benutzerdefinierte PerformancePoint Services-Objekte](http://msdn.microsoft.com/library/7c5924f1-91f3-436a-9d94-2e0dc454c8cc%28Office.15%29.aspx).
13. Rufen Sie die zugrunde liegende Datenquelle des Filters aus dem Repository ab. Der Beispiel-Filter-Editor verwendet die **FilterRepositoryHelper.DataSourceHelper**-Eigenschaft zum Abrufen der **DataSourceConsumerHelper.GetDataSource**-Methode, mit der die Datenquelle anhand ihres Speicherorts im Repository abgerufen wird. Dies wird im folgenden Codebeispiel demonstriert.
    
  ```cs
  
if (!string.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl))
    {
        RepositoryLocation repositoryDatasourceLocation =
        RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl);
        datasource =
        filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation);
    }
  ```


  ```VB.net
  
If Not String.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl) Then
      Dim repositoryDatasourceLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl)
      datasource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation)
End If
  ```

14. Damit Benutzer auf eine Datenquelle für den Filter auswählen können, füllen Sie das Steuerelement zur Auswahl mit PerformancePoint-Dienste-Datenquellen. Die **PopulateDataSourceDropDown** -Methode im Filter-Editor Beispiel ruft die **DataSourceConsumerHelper.GetDataSourcesBySourceNames** -Methode zum Abrufen der Datenquellen. Dies ist im folgenden Codebeispiel dargestellt.
    
  ```cs
  
// The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
// Edit this value if you are not using the default path. A leading forward slash may not be needed.
ICollection dataSourceCollection =
    
filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames
    ("/BICenter/Data%20Connections%20for%20PerformancePoint/",
         new[] { "WSTabularDataSource", DataSourceNames.ExcelWorkbook });
  ```


  ```VB.net
  
' The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
' Edit this value if you are not using the default path. A leading forward slash may not be needed.
Dim dataSourceCollection As ICollection = filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames ("("/BICenter/Data%20Connections%20for%20PerformancePoint/", { "WSTabularDataSource", DataSourceNames.ExcelWorkbook })
  ```


    Im Beispiel Filter-Editor werden nur zwei Arten von Datenquelle abgerufen, aber Sie können diese Methode zur Unterstützung von anderen Arten von Datenquellen oder den Benutzer für den Typ der Datenquelle abrufen abgefragt ändern. Eine systemeigene Datenquelle eines bestimmten Typs verweisen möchten, verwenden Sie die  [SourceName](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSource.SourceName.aspx) -Eigenschaft ein Felds aus der [DataSourceNames](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSourceNames.aspx) -Klasse zurückgegeben. Um eine benutzerdefinierte Datenquelle verweisen möchten, verwenden Sie die [SubTypeId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.DataSource.SubTypeId.aspx) -Eigenschaft der Datenquelle, die den gleichen Wert wie das Attribut **subType** ist, die in der PerformancePoint-Dienste web.config-Datei für die datenquellenerweiterung registriert ist.
    
    Wenn Sie diese Methode ändern, müssen Sie die entsprechende Änderung für die  [GetDisplayDataInternal](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetDisplayDataInternal.aspx) -Methode im Datenanbieter des Beispielfilters vornehmen.
    
  
15. Definieren Sie den Filteranfangspunkt, der durch die  [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) -Eigenschaft dargestellt wird. Hiermit wird die Quelle der Filterwerte definiert. Dies ist erforderlich, damit der Filter Daten an Scorecards und Berichte senden kann.
    
1. Erstellen Sie ein  [ParameterDefinition](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.aspx) -Objekt. [BeginPoints](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.BeginPoints.aspx) gibt ein [ParameterDefinitionCollection](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinitionCollection.aspx) -Objekt zurück, das nur ein [ParameterDefinition](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.aspx) -Objekt enthält.
    
  
2. Legen Sie zum Angeben des Datenanbieters des Filters für die  [ParameterProviderId](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.ParameterProviderId.aspx) -Eigenschaft den eindeutigen Bezeichner des Datenanbieters fest. Dieser Wert muss mit dem Wert übereinstimmen, der von der [GetId()](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetId.aspx) -Methode des Datenanbieters zurückgegeben wird.
    
  
3. Zur Angabe der Quelle der Schlüsselbezeichner für die Filterwerte legen Sie für die  [KeyColumn](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.KeyColumn.aspx) -Eigenschaft die Spalte in der Anzeigedatentabelle fest, die die Schlüsselbezeichner enthält. Für den Beispiel-Filter-Editor wird diese Eigenschaft als **Symbol**-Spalte definiert.
    
  
4. Zur Angabe der Quelle der Anzeigewerte für das Filtersteuerelement legen Sie für die  [DisplayColumn](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.DisplayColumn.aspx) -Eigenschaft die Spalte in der Anzeigedatentabelle fest, die die Anzeigewerte enthält. Für den Beispiel-Filter-Editor wird diese Eigenschaft als **Symbol**-Spalte definiert.
    
  

    > **HINWEIS**
      <br/> Die Anzeigedatentabelle wird von der  [DisplayValues](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.ParameterDefinition.DisplayValues.aspx) -Eigenschaft zurückgegeben, und sie wird initialisiert, wenn der Filterdatenanbieter die [GetDisplayDataInternal](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Server.Extensions.CustomParameterDataProvider.GetDisplayDataInternal.aspx) -Methode aufruft. Falls die Datentabelle andere Spalten enthält, können Sie andere Spaltenzuordnungen definieren, um zusätzliche Funktionalität anzubieten.

  ```cs
  
if (0 == filter.BeginPoints.Count)
{
    ParameterDefinition paramDef = new ParameterDefinition();

    // Reference the data provider.
    paramDef.ParameterProviderId = "SampleFilterDataProvider";
    paramDef.DefaultPostFormula = string.Empty;

    // Specify the column that contains the key identifiers and the column
    // that contains the display values. The sample uses the same column
    // for both purposes.
    // These values must match the structure of the data table that is
    // returned by the ParameterDefinition.DisplayValues property.

    paramDef.KeyColumn = "Symbol";
    paramDef.DisplayColumn = "Symbol";

    // You can use this property to store custom information for this filter.
    paramDef.CustomDefinition = string.Empty;
    
    filter.BeginPoints.Add(paramDef);
}
  ```


  ```VB.net
  
If 0 = filter.BeginPoints.Count Then
        Dim paramDef As New ParameterDefinition()

        ' Reference the data provider.
      paramDef.ParameterProviderId = "SampleFilterDataProvider"
      paramDef.DefaultPostFormula = String.Empty

        ' Specify the column that contains the key identifiers and the column
        ' that contains the display values. The sample uses the same column
        ' for both purposes.
        ' These values must match the structure of the data table that is
        ' returned by the ParameterDefinition.DisplayValues property.

      paramDef.KeyColumn = "Symbol"
      paramDef.DisplayColumn = "Symbol"

        ' You can use this property to store custom information for this filter.
      paramDef.CustomDefinition = String.Empty

      filter.BeginPoints.Add(paramDef)
End If
  ```


    Der Anfangspunkt für den Beispieleditor wird in der **VerifyFilter**-Methode definiert. Mithilfe von **VerifyFilter** wird außerdem überprüft, ob erforderliche Eigenschaften festgelegt sind, und der Auswahlmodus wird definiert, bei dem es sich um eine optionale Eigenschaft handelt.
    
  
16. Initialisieren Sie den Filter, indem Sie die Abfrage des Filters ausführen und Daten aus der Datenquelle abrufen. Mit der **buttonOK_Click**-Methode im Beispiel-Filter-Editor wird die **FilterRepositoryHelper.GetParameterDisplayData**-Methode zum Initialisieren des Filters aufgerufen.
    
    > **HINWEIS**
      > Der Editor muss **FilterRepositoryHelper.GetParameterDisplayData** mindestens einmal aufrufen, bevor das Filterobjekt aktualisiert wird.
17. Aktualisieren Sie den Filter mit benutzerdefinierten Änderungen. Mit der **buttonOK_Click**-Methode im Beispiel-Filter-Editor wird die **FilterRepositoryHelper.Update**-Methode zum Aktualisieren der Eigenschaften  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) , [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) und [DataSourceLocation](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Filter.DataSourceLocation.aspx) des Filters im Repository aufgerufen. Darüber hinaus werden mit **buttonOK_Click** die Inhalte der Steuerelemente überprüft und Statusinformationen für den benutzerdefinierten Filter und das Hilfsobjekt abgerufen.
    
    > **HINWEIS**
      > Benutzer können eines benutzerdefinierten Objekts  [Name](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Name.aspx) , [Description](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Description.aspx) und [Owner](https://msdn.microsoft.com/library/Microsoft.PerformancePoint.Scorecards.Element.Owner.aspx) ( **Verantwortliche Person** ) Eigenschaften festgelegt und benutzerdefinierte Objekte direkt in Dashboard-Designer und die PerformancePoint-Dienste-Repository löschen.

## Codebeispiel: erstellen, abrufen und Aktualisieren von benutzerdefinierten PerformancePoint-Dienste Filter in SharePoint Server 2013
<a name="bk_example"> </a>

Im folgenden Codebeispiel wird erstellt, abgerufen und benutzerdefinierte Filter aktualisiert. Dieser Code hat ihren Ursprung im Editor CodeBehind-Klasse, die die Programmierlogik für Steuerelemente bereitstellt, die in einer ASPX-Seite definiert sind.
  
    
    
Bevor Sie dieses Codebeispiel kompilieren können, müssen Sie Ihre Entwicklungsumgebung konfigurieren, wie beschrieben in  [Erstellen und konfigurieren Sie die Editorklasse für einen Filtereditor in PerformancePoint Services](#BKMK_ConfigFEd).
  
    
    



```cs

using System;
using System.Collections;
using System.Collections.Generic;
using System.Data;
using System.Web.UI;
using System.Web.UI.WebControls;
using Microsoft.PerformancePoint.Scorecards; 
using Microsoft.PerformancePoint.Scorecards.ServerCommon;
using Microsoft.PerformancePoint.Scorecards.ServerRendering;

namespace Microsoft.PerformancePoint.SDK.Samples.SampleFilter
{

    // Represents the class that defines the sample filter editor.
    public class SampleFilterEditor : Page
    {

        // Declare private variables for the ASP.NET controls defined in the user interface.
        // The sample's user interface is an ASPX page that defines the controls in HTML.
        private TextBox textboxName;
        private TextBox textboxDescription;
        private Label labelErrorMessage;
        private DropDownList dropdownlistDataSource;
        private Button buttonOK;
        private ListBox listboxStocks;

        // Make the controls available to this class.
        protected override void CreateChildControls()
        {
            base.CreateChildControls();

            if (null == textboxName) 
                textboxName = FindControl("textboxName") as TextBox;
            if (null == textboxDescription) 
                textboxDescription = FindControl("textboxDescription") as TextBox;
            if (null == dropdownlistDataSource) 
                dropdownlistDataSource = FindControl("dropdownlistDataSource") as DropDownList;
            if (null == labelErrorMessage)
                labelErrorMessage = FindControl("labelErrorMessage") as Label;
            if (null==buttonOK)
                buttonOK = FindControl("buttonOK") as Button;
            if (null==listboxStocks)
                listboxStocks = FindControl("listboxStocks") as ListBox;
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
                FilterRepositoryHelper filterRepositoryHelper = null;
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
                    filterRepositoryHelper =
                        new FilterRepositoryHelper();

                    // Set the filter location.
                    RepositoryLocation repositoryFilterLocation = RepositoryLocation.CreateFromUriString(itemLocation);

                    Filter filter;
                    DataSource datasource = null;

                    // Retrieve or create the filter object, depending on the operation
                    // passed in the query string (OpenItem or CreateItem).
                    if (ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Retrieve the filter object by using the repository-helper object.
                        filter = filterRepositoryHelper.Get(repositoryFilterLocation);
                        if (filter == null)
                        {
                            displayError("Could not retrieve the filter for editing.");
                            return;
                        }

                    }
                    else if (ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase))
                    {

                        // Create a filter.
                        // CreateItem requests can be sent from a SharePoint list, but
                        // you must create a custom menu item to send the request.
                        // Dashboard Designer can send edit requests only.
                        filter = new Filter
                            {

                                // Specify the class that defines the renderer
                                // web server control. The sample filter uses a native
                                // PerformancePoint Services renderer.
                                // Defaults to the value specified in the web.config file
                                RendererClassName = typeof(MultiSelectTreeViewControl).AssemblyQualifiedName,

                                // Specify the unique identifier for the filter.
                                // The SubTypeId property must match the
                                // subType attribute in the web.config file.
                                SubTypeId = "SampleFilter"
                            };
                    }
                    else
                    {
                        displayError("Invalid Action.");
                        return;
                    }

                    VerifyFilter(filter);

                    // Retrieve filter's underlying data source.
                    if (!string.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl))
                    {
                        RepositoryLocation repositoryDatasourceLocation =
                            RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl);
                        datasource =

                            // Gets a PerformancePoint Services data source by using the
                            // DataSourceHelper property to call the
                            // DataSourceConsumerHelper.GetDataSource method. 
                            filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation);
                    }

                    // Save the original filter and helper objects across page postbacks.
                    ViewState["action"] = action;
                    ViewState["filter"] = filter;
                    ViewState["filterrepositoryhelper"] = filterRepositoryHelper;
                    ViewState["itemlocation"] = itemLocation;

                    // Populate the child controls.
                    textboxName.Text = filter.Name.ToString();
                    textboxDescription.Text = filter.Description.ToString();

                    // Populate the dropdownlistDataSource control with data sources of specific
                    // types from the PerformancePoint Services repository.
                    // This method looks up the passed data source in the data sources
                    // that are registered in the web.config file. 
                    // Although the sample retrieves data sources of two specific types,
                    // you can modify it to prompt the user for a data source type.
                    PopulateDataSourceDropDown(datasource);

                    // Call the SelectedIndexChanged event directly to populate the 
                    // listbox control with preview data.
                    dropdownlistDataSource_SelectedIndexChanged(null, null);
                }
                catch (Exception ex)
                {
                    displayError("An error has occurred. Please contact your administrator for more information.");
                    if (filterRepositoryHelper != null)
                    {
                        // Add the exception detail to the server event log.
                        filterRepositoryHelper.HandleException(ex);
                    }
                }
            }
        }

        // Handles the SelectedIndexChanged event of the dropdownlistDataSource control.
        protected void dropdownlistDataSource_SelectedIndexChanged(object sender, EventArgs e)
        {
            EnsureChildControls();

            // Check if a valid data source is selected.
            if (null != dropdownlistDataSource.SelectedItem &amp;&amp;
                !string.IsNullOrEmpty(dropdownlistDataSource.SelectedItem.Text))
            {
                // Retrieve the data source object.
                FilterRepositoryHelper filterRepositoryHelper =
                    (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];
                string selectedDataSourceItemUrl = dropdownlistDataSource.SelectedItem.Value;

                RepositoryLocation repositoryDatasourceLocation =
                    RepositoryLocation.CreateFromUriString(selectedDataSourceItemUrl);
                DataSource datasource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation);
                ViewState["datasource"] = datasource;

                // Populate the listboxStocks control with the preview data for the selected
                // data source.
                PopulateListBoxData(datasource);
            }
            else
            {
                ClearStocksListBox();
            }
        }

        // Clears the listboxStocks control.
        // The sample filter works with a web service that provides stock information.
        private void ClearStocksListBox()
        {
            listboxStocks.DataSource = null;
            listboxStocks.DataBind();
            listboxStocks.Items.Clear();
        }

        // Handles the Click event of the buttonOK control.
        protected void buttonOK_Click(object sender, EventArgs e)
        {
            EnsureChildControls();

            // Verify that the controls contain values.
            if (string.IsNullOrEmpty(textboxName.Text))
            {
                labelErrorMessage.Text = "A filter name is required.";
                return;
            }
            if (dropdownlistDataSource.SelectedIndex == 0)
            {
                labelErrorMessage.Text = "A data source is required.";
                return;
            }

            // Clear any pre-existing error message.
            labelErrorMessage.Text = string.Empty;

            // Retrieve the filter, data source, and helper objects from view state.
            string action = (string)ViewState["action"];
            string itemLocation = (string) ViewState["itemlocation"];
            Filter filter = (Filter)ViewState["filter"];
            DataSource datasource = (DataSource)ViewState["datasource"];
            FilterRepositoryHelper filterRepositoryHelper = (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];

            // Update the filter object with form changes.
            filter.Name.Text = textboxName.Text;
            filter.Description.Text = textboxDescription.Text;
            filter.DataSourceLocation = datasource.Location;
            foreach (ParameterDefinition parameterDefinition in filter.BeginPoints)
            {
                parameterDefinition.DisplayName = filter.Name.Text;
            }

            // Initialize the filter. This method runs the filter's query and retrieves preview data.
            filterRepositoryHelper.GetParameterDisplayData(ref filter);

            // Save the filter object to the PerformancePoint Services repository.
            try
            {
                filter.Validate();

                if (ClickOnceLaunchValues.CreateItem.Equals(action,StringComparison.OrdinalIgnoreCase))
                {
                    Filter newFilter = filterRepositoryHelper.Create(
                        string.IsNullOrEmpty(filter.Location.ItemUrl) ? itemLocation : filter.Location.ItemUrl, filter);
                    ViewState["filter"] = newFilter;
                    ViewState["action"] = ClickOnceLaunchValues.OpenItem;
                }
                else
                {
                    filterRepositoryHelper.Update(filter);
                }
            }
            catch (Exception ex)
            {
                displayError("An error has occurred. Please contact your administrator for more information.");
                if (filterRepositoryHelper != null)
                {

                    // Add the exception detail to the server event log.
                    filterRepositoryHelper.HandleException(ex);
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

        // Verifies that the properties for the filter object are set.
        static void VerifyFilter(Filter filter)
        {

            if (null != filter)
            {

                // Verify that all required properties are set.
                if (string.IsNullOrEmpty(filter.SubTypeId)) 
                {

                    // This value must match the subType attribute specified
                    // in the web.config file.
                    filter.SubTypeId = "SampleFilter";
                }

                if (string.IsNullOrEmpty(filter.RendererClassName)) 
                {
                    filter.RendererClassName = typeof (MultiSelectTreeViewControl).AssemblyQualifiedName; 
                }

                // Define the BeginPoints property so the filter can send a parameter value to
                // scorecards and reports.
                // The value must be from the KeyColumn of the display
                // DataTable object, which is defined in the data provider. The data table is
                // returned by the FilterRepositoryHelper.GetParameterDisplayData method.
                // A filter has one beginpoint only, and it is represented by a
                // ParameterDefinition object. The ParameterDefinition object defines how
                // the filter accesses the data.
                if (0 == filter.BeginPoints.Count)
                {
                    ParameterDefinition paramDef = new ParameterDefinition
                                                       {
                                                           // This value must match the value returned 
                                                           // by the data provider's GetId method.
                                                           ParameterProviderId = "SampleFilterDataProvider",

                                                           // Reference the data provider.
                                                           DefaultPostFormula = string.Empty,

                                                           // Specify the column that contains
                                                           // the key identifiers and the column
                                                           // that contains the display values.
                                                           // The sample uses the same column
                                                           // for both purposes.
                                                           // These values must match the structure
                                                           // of the data table that is returned
                                                           // by the ParameterDefinition.DisplayValues property.
                                                           KeyColumn = "Symbol",
                                                           DisplayColumn = "Symbol",

                                                           // You can use this property to store
                                                           // extra information for this filter.
                                                           CustomDefinition = string.Empty
                                                       };
                    filter.BeginPoints.Add(paramDef);
                }

                // Set optional properties. The renderer can return multiple values. 
                filter.SelectionMode = FilterSelectionMode.MultiSelect;
            }
        }

        // Populates the dropdownlistDataSource control.
        void PopulateDataSourceDropDown(DataSource filterDataSource)
        {
            EnsureChildControls();
            
            FilterRepositoryHelper filterRepositoryHelper =
                (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];

            // Retrieve data sources from the repository by using the DataSourceHelper
            // property to call the DataSourceConsumerHelper object.
            // If you modify the types of data source to retrieve, you must make the corresponding
            // change in the filter's data provider.
            // The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
            // Edit this value if you are not using the default path. A leading forward slash may not be needed.
            ICollection dataSourceCollection = filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames("/BICenter/Data%20Connections%20for%20PerformancePoint/",
                new[] { "WSTabularDataSource", DataSourceNames.ExcelWorkbook });
            if (null == dataSourceCollection)
            {
                displayError("No available data sources were found.");
                return;
            }

            // Create a list of name/value pairs for the dropdownlistDataSource control.
            var dataSources = new List<KeyValuePair<string, string>>();
            int selectedIndex = 0;
            int i = 1;
            dataSources.Add(new KeyValuePair<string, string>(string.Empty, string.Empty));

            foreach (DataSource ds in dataSourceCollection)
            {
                dataSources.Add(new KeyValuePair<string, string>(ds.Name.Text, ds.Location.ItemUrl));

                // Check if the entry is the originally selected data source.
                if ((filterDataSource != null) &amp;&amp;
                    (string.Compare(ds.Name.Text, filterDataSource.Name.Text) == 0))
                {
                    selectedIndex = i;
                }
                ++i;
            }

            dropdownlistDataSource.DataSource = dataSources;
            dropdownlistDataSource.DataTextField = "Key";
            dropdownlistDataSource.DataValueField = "Value";
            dropdownlistDataSource.DataBind();
            dropdownlistDataSource.SelectedIndex = selectedIndex;
        }


        // Populate the list box data.
        void PopulateListBoxData(DataSource datasource)
        {
            EnsureChildControls();
            
            ClearStocksListBox();
            
            FilterRepositoryHelper filterRepositoryHelper =
                (FilterRepositoryHelper)ViewState["filterrepositoryhelper"];

            // Retrieve the first 100 rows of the preview data from the data source
            DataSet dataSet = filterRepositoryHelper.DataSourceHelper.GetDataSet(100, datasource);

            if (null != dataSet &amp;&amp; null != dataSet.Tables[0])
            {
                listboxStocks.DataTextField = "Symbol";
                listboxStocks.DataValueField = "Value";
                listboxStocks.DataSource = dataSet.Tables[0];
                listboxStocks.DataBind();
            }
        }
    }
}
```




```VB.net

'INSTANT VB NOTE: This code snippet uses implicit typing. You will need to set 'Option Infer On' in the VB file or set 'Option Infer' at the project level:

Imports System
Imports System.Collections
Imports System.Collections.Generic
Imports System.Data
Imports System.Web.UI
Imports System.Web.UI.WebControls
Imports Microsoft.PerformancePoint.Scorecards
Imports Microsoft.PerformancePoint.Scorecards.ServerCommon
Imports Microsoft.PerformancePoint.Scorecards.ServerRendering

Namespace Microsoft.PerformancePoint.SDK.Samples.SampleFilter

    ' Represents the class that defines the sample filter editor.
    Public Class SampleFilterEditor
        Inherits Page

        ' Declare private variables for the ASP.NET controls defined in the user interface.
        ' The sample's user interface is an ASPX page that defines the controls in HTML.
        Private textboxName As TextBox
        Private textboxDescription As TextBox
        Private labelErrorMessage As Label
        Private dropdownlistDataSource As DropDownList
        Private buttonOK As Button
        Private listboxStocks As ListBox

        ' Make the controls available to this class.
        Protected Overrides Sub CreateChildControls()
            MyBase.CreateChildControls()

            If Nothing Is textboxName Then
                textboxName = TryCast(FindControl("textboxName"), TextBox)
            End If
            If Nothing Is textboxDescription Then
                textboxDescription = TryCast(FindControl("textboxDescription"), TextBox)
            End If
            If Nothing Is dropdownlistDataSource Then
                dropdownlistDataSource = TryCast(FindControl("dropdownlistDataSource"), DropDownList)
            End If
            If Nothing Is labelErrorMessage Then
                labelErrorMessage = TryCast(FindControl("labelErrorMessage"), Label)
            End If
            If Nothing Is buttonOK Then
                buttonOK = TryCast(FindControl("buttonOK"), Button)
            End If
            If Nothing Is listboxStocks Then
                listboxStocks = TryCast(FindControl("listboxStocks"), ListBox)
            End If
        End Sub

        ' Handles the Load event of the Page control.
        ' Methods that use a control variable should call the Control.EnsureChildControls
        ' method before accessing the variable for the first time.
        Protected Sub Page_Load(ByVal sender As Object, ByVal e As EventArgs)

            ' Required to enable custom report and filter editors to
            ' write data to the repository.
            ServerUtils.AllowUnsafeUpdates = True

            ' Initialize controls the first time the page loads only.
            If Not IsPostBack Then
                EnsureChildControls()
                Dim filterRepositoryHelper As FilterRepositoryHelper = Nothing
                Try

                    ' Get information from the query string parameters.
                    Dim server As String = Request.QueryString(ClickOnceLaunchKeys.SiteCollectionUrl)
                    Dim itemLocation As String = Request.QueryString(ClickOnceLaunchKeys.ItemLocation)
                    Dim action As String = Request.QueryString(ClickOnceLaunchKeys.LaunchOperation)

                    ' Validate the query string parameters.
                    If String.IsNullOrEmpty(server) OrElse String.IsNullOrEmpty(itemLocation) OrElse String.IsNullOrEmpty(action) Then
                        displayError("Invalid URL.")
                        Return
                    End If

                    ' Retrieve the repository-helper object.
                    filterRepositoryHelper = New FilterRepositoryHelper()

                    ' Set the filter location.
                    Dim repositoryFilterLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(itemLocation)

                    Dim filter As Filter
                    Dim datasource As DataSource = Nothing

                    ' Retrieve or create the filter object, depending on the operation
                    ' passed in the query string (OpenItem or CreateItem).
                    If ClickOnceLaunchValues.OpenItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then

                        ' Retrieve the filter object by using the repository-helper object.
                        filter = filterRepositoryHelper.Get(repositoryFilterLocation)
                        If filter Is Nothing Then
                            displayError("Could not retrieve the filter for editing.")
                            Return
                        End If

                    ElseIf ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then

                        ' Create a filter.
                        ' CreateItem requests can be sent from a SharePoint list, but
                        ' you must create a custom menu item to send the request.
                        ' Dashboard Designer can send edit requests only.
                        filter = New Filter With {.RendererClassName = GetType(MultiSelectTreeViewControl).AssemblyQualifiedName, .SubTypeId = "SampleFilter"}
                        ' Specify the class that defines the renderer
                        ' web server control. The sample filter uses a native
                        ' PerformancePoint Services renderer.
                        ' Defaults to the value specified in the web.config file
                        ' Specify the unique identifier for the filter.
                        ' The SubTypeId property must match the
                        ' subType attribute in the web.config file.
                    Else
                        displayError("Invalid Action.")
                        Return
                    End If

                    VerifyFilter(filter)

                    ' Retrieve filter's underlying data source.
                    If Not String.IsNullOrEmpty(filter.DataSourceLocation.ItemUrl) Then
                        Dim repositoryDatasourceLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(filter.DataSourceLocation.ItemUrl)
                        ' Gets a PerformancePoint Services data source by using the
                        ' DataSourceHelper property to call the
                        ' DataSourceConsumerHelper.GetDataSource method. 
                        datasource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation)
                    End If

                    ' Save the original filter and helper objects across page postbacks.
                    ViewState("action") = action
                    ViewState("filter") = filter
                    ViewState("filterrepositoryhelper") = filterRepositoryHelper
                    ViewState("itemlocation") = itemLocation

                    ' Populate the child controls.
                    textboxName.Text = filter.Name.ToString()
                    textboxDescription.Text = filter.Description.ToString()

                    ' Populate the dropdownlistDataSource control with data sources of specific
                    ' types from the PerformancePoint Services repository.
                    ' This method looks up the passed data source in the data sources
                    ' that are registered in the web.config file. 
                    ' Although the sample retrieves data sources of two specific types,
                    ' you can modify it to prompt the user for a data source type.
                    PopulateDataSourceDropDown(datasource)

                    ' Call the SelectedIndexChanged event directly to populate the 
                    ' listbox control with preview data.
                    dropdownlistDataSource_SelectedIndexChanged(Nothing, Nothing)
                Catch ex As Exception
                    displayError("An error has occurred. Please contact your administrator for more information.")
                    If filterRepositoryHelper IsNot Nothing Then
                        ' Add the exception detail to the server event log.
                        filterRepositoryHelper.HandleException(ex)
                    End If
                End Try
            End If
        End Sub

        ' Handles the SelectedIndexChanged event of the dropdownlistDataSource control.
        Protected Sub dropdownlistDataSource_SelectedIndexChanged(ByVal sender As Object, ByVal e As EventArgs)
            EnsureChildControls()

            ' Check if a valid data source is selected.
            If Nothing IsNot dropdownlistDataSource.SelectedItem AndAlso (Not String.IsNullOrEmpty(dropdownlistDataSource.SelectedItem.Text)) Then
                ' Retrieve the data source object.
                Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)
                Dim selectedDataSourceItemUrl As String = dropdownlistDataSource.SelectedItem.Value

                Dim repositoryDatasourceLocation As RepositoryLocation = RepositoryLocation.CreateFromUriString(selectedDataSourceItemUrl)
                Dim datasource As DataSource = filterRepositoryHelper.DataSourceHelper.GetDataSource(repositoryDatasourceLocation)
                ViewState("datasource") = datasource

                ' Populate the listboxStocks control with the preview data for the selected
                ' data source.
                PopulateListBoxData(datasource)
            Else
                ClearStocksListBox()
            End If
        End Sub

        ' Clears the listboxStocks control.
        ' The sample filter works with a web service that provides stock information.
        Private Sub ClearStocksListBox()
            listboxStocks.DataSource = Nothing
            listboxStocks.DataBind()
            listboxStocks.Items.Clear()
        End Sub

        ' Handles the Click event of the buttonOK control.
        Protected Sub buttonOK_Click(ByVal sender As Object, ByVal e As EventArgs)
            EnsureChildControls()

            ' Verify that the controls contain values.
            If String.IsNullOrEmpty(textboxName.Text) Then
                labelErrorMessage.Text = "A filter name is required."
                Return
            End If
            If dropdownlistDataSource.SelectedIndex = 0 Then
                labelErrorMessage.Text = "A data source is required."
                Return
            End If

            ' Clear any pre-existing error message.
            labelErrorMessage.Text = String.Empty

            ' Retrieve the filter, data source, and helper objects from view state.
            Dim action As String = CStr(ViewState("action"))
            Dim itemLocation As String = CStr(ViewState("itemlocation"))
            Dim filter As Filter = CType(ViewState("filter"), Filter)
            Dim datasource As DataSource = CType(ViewState("datasource"), DataSource)
            Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)

            ' Update the filter object with form changes.
            filter.Name.Text = textboxName.Text
            filter.Description.Text = textboxDescription.Text
            filter.DataSourceLocation = datasource.Location
            For Each parameterDefinition As ParameterDefinition In filter.BeginPoints
                parameterDefinition.DisplayName = filter.Name.Text
            Next parameterDefinition

            ' Initialize the filter. This method runs the filter's query and retrieves preview data.
            filterRepositoryHelper.GetParameterDisplayData(filter)

            ' Save the filter object to the PerformancePoint Services repository.
            Try
                filter.Validate()

                If ClickOnceLaunchValues.CreateItem.Equals(action, StringComparison.OrdinalIgnoreCase) Then
                    Dim newFilter As Filter = filterRepositoryHelper.Create(If(String.IsNullOrEmpty(filter.Location.ItemUrl), itemLocation, filter.Location.ItemUrl), filter)
                    ViewState("filter") = newFilter
                    ViewState("action") = ClickOnceLaunchValues.OpenItem
                Else
                    filterRepositoryHelper.Update(filter)
                End If
            Catch ex As Exception
                displayError("An error has occurred. Please contact your administrator for more information.")
                If filterRepositoryHelper IsNot Nothing Then

                    ' Add the exception detail to the server event log.
                    filterRepositoryHelper.HandleException(ex)
                End If
            End Try
        End Sub

        ' Displays the error string in the labelErrorMessage label.
        Private Sub displayError(ByVal msg As String)
            EnsureChildControls()

            labelErrorMessage.Text = msg

            ' Disable the OK button because the page is in an error state.
            buttonOK.Enabled = False
            Return
        End Sub

        ' Verifies that the properties for the filter object are set.
        Private Shared Sub VerifyFilter(ByVal filter As Filter)

            If Nothing IsNot filter Then

                ' Verify that all required properties are set.
                If String.IsNullOrEmpty(filter.SubTypeId) Then

                    ' This value must match the subType attribute specified
                    ' in the web.config file.
                    filter.SubTypeId = "SampleFilter"
                End If

                If String.IsNullOrEmpty(filter.RendererClassName) Then
                    filter.RendererClassName = GetType(MultiSelectTreeViewControl).AssemblyQualifiedName
                End If

                ' Define the BeginPoints property so the filter can send a parameter value to
                ' scorecards and reports.
                ' The value must be from the KeyColumn of the display
                ' DataTable object, which is defined in the data provider. The data table is
                ' returned by the FilterRepositoryHelper.GetParameterDisplayData method.
                ' A filter has one beginpoint only, and it is represented by a
                ' ParameterDefinition object. The ParameterDefinition object defines how
                ' the filter accesses the data.
                If 0 = filter.BeginPoints.Count Then
                    Dim paramDef As ParameterDefinition = New ParameterDefinition With {.ParameterProviderId = "SampleFilterDataProvider", .DefaultPostFormula = String.Empty, .KeyColumn = "Symbol", .DisplayColumn = "Symbol", .CustomDefinition = String.Empty}
                    ' This value must match the value returned 
                    ' by the data provider's GetId method.
                    ' Reference the data provider.
                    ' Specify the column that contains
                    ' the key identifiers and the column
                    ' that contains the display values.
                    ' The sample uses the same column
                    ' for both purposes.
                    ' These values must match the structure
                    ' of the data table that is returned
                    ' by the ParameterDefinition.DisplayValues property.
                    ' You can use this property to store
                    ' extra information for this filter.
                    filter.BeginPoints.Add(paramDef)
                End If

                ' Set optional properties. The renderer can return multiple values. 
                filter.SelectionMode = FilterSelectionMode.MultiSelect
            End If
        End Sub

        ' Populates the dropdownlistDataSource control.
        Private Sub PopulateDataSourceDropDown(ByVal filterDataSource As DataSource)
            EnsureChildControls()

            Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)

            ' Retrieve data sources from the repository by using the DataSourceHelper
            ' property to call the DataSourceConsumerHelper object.
            ' If you modify the types of data source to retrieve, you must make the corresponding
            ' change in the filter's data provider.
            ' The parameter contains the default server-relative URL to the PerformancePoint Data Connections Library.
            ' Edit this value if you are not using the default path. A leading forward slash may not be needed.
            Dim dataSourceCollection As ICollection = filterRepositoryHelper.DataSourceHelper.GetDataSourcesBySourceNames("/BICenter/Data%20Connections%20for%20PerformancePoint/", {"WSTabularDataSource", DataSourceNames.ExcelWorkbook})
            If Nothing Is dataSourceCollection Then
                displayError("No available data sources were found.")
                Return
            End If

            ' Create a list of name/value pairs for the dropdownlistDataSource control.
            Dim dataSources = New List(Of KeyValuePair(Of String, String))()
            Dim selectedIndex As Integer = 0
            Dim i As Integer = 1
            dataSources.Add(New KeyValuePair(Of String, String)(String.Empty, String.Empty))

            For Each ds As DataSource In dataSourceCollection
                dataSources.Add(New KeyValuePair(Of String, String)(ds.Name.Text, ds.Location.ItemUrl))

                ' Check if the entry is the originally selected data source.
                If (filterDataSource IsNot Nothing) AndAlso (String.Compare(ds.Name.Text, filterDataSource.Name.Text) = 0) Then
                    selectedIndex = i
                End If
                i += 1
            Next ds

            dropdownlistDataSource.DataSource = dataSources
            dropdownlistDataSource.DataTextField = "Key"
            dropdownlistDataSource.DataValueField = "Value"
            dropdownlistDataSource.DataBind()
            dropdownlistDataSource.SelectedIndex = selectedIndex
        End Sub


        ' Populate the list box data.
        Private Sub PopulateListBoxData(ByVal datasource As DataSource)
            EnsureChildControls()

            ClearStocksListBox()

            Dim filterRepositoryHelper As FilterRepositoryHelper = CType(ViewState("filterrepositoryhelper"), FilterRepositoryHelper)

            ' Retrieve the first 100 rows of the preview data from the data source
            Dim dataSet As DataSet = filterRepositoryHelper.DataSourceHelper.GetDataSet(100, datasource)

            If Nothing IsNot dataSet AndAlso Nothing IsNot dataSet.Tables(0) Then
                listboxStocks.DataTextField = "Symbol"
                listboxStocks.DataValueField = "Value"
                listboxStocks.DataSource = dataSet.Tables(0)
                listboxStocks.DataBind()
            End If
        End Sub
    End Class
End Namespace
```


## Nächste Schritte
<a name="bk_example"> </a>

Nach dem Erstellen Sie eines Filter-Editors (einschließlich der Benutzeroberfläche, falls erforderlich) und einen Datenanbieter, Bereitstellen Sie die Erweiterung, wie in  [Gewusst wie: Manuelles Registrieren von PerformancePoint-Dienste-Erweiterungen](http://msdn.microsoft.com/library/3aa6d340-4b05-46b3-9648-2b6e18e04e09%28Office.15%29.aspx)beschrieben.
  
    
    

## Zusätzliche Ressourcen
<a name="bk_addResources"> </a>


-  [Vorgehensweise: Erstellen Filter von Datenanbietern für PerformancePoint Services in SharePoint 2013](how-to-create-filter-data-providers-for-performancepoint-services-in-sharepoint.md)
    
  
-  [PerformancePoint-Dienste in SharePoint 2013](performancepoint-services-in-sharepoint-2013.md)
    
  

