---
title: Vorgehensweise Verwenden von SAP-Workflows mit Duet Enterprise 2.0
ms.prod: SHAREPOINT
ms.assetid: 816e28ed-8cea-4e33-98e5-d3d27136e2e6
---


# Vorgehensweise: Verwenden von SAP-Workflows mit Duet Enterprise 2.0

## Einführung
<a name="bkmk_Introduction"> </a>

SAP Business Workflow bietet eine Möglichkeit zum Automatisieren von Geschäftsprozessen und mit Duet Enterprise 2.0, die Workflows innerhalb einer SharePoint-Add-In integriert werden können.
  
    
    
Die folgenden Schritte zeigen, wie eine app erstellen, konfigurieren, und klicken Sie dann wie die Informationen anzeigen aus der SAP-Workflows abgerufen.
  
    
    

## Erstellen einer app für SharePoint und Duet Enterprise 2.0
<a name="bkmk_CreateApp"> </a>

Der erste Schritt besteht darin ein SharePoint-Add-In erstellen, das die Verbindungsinformationen mithilfe eines externen Inhaltstyps für Business Connectivity Services (BCS), externe Listen und Anpassungen möglicherweise verwenden, um die Daten zu präsentieren möchten.
  
    
    

### So erstellen eine app für SharePoint:


1. Starten Sie Visual Studio 2012 als Administrator.
    
  
2. Wählen Sie **Datei**, **neu**, **Neues Projekt**.
    
  
3. Klicken Sie im Dialogfeld **Neues Projekt** erweitern Sie den Knoten **Visual c#**, erweitern Sie den Knoten **Office/SharePoint** und wählen Sie dann den Knoten **Apps**.
    
  
4. Wählen Sie **die App für SharePoint 2013**.
    
  
5. Nennen Sie das Projekt, und klicken Sie auf **OK**.
    
  
6. Klicken Sie im ersten App für SharePoint-Einstellungen im Dialogfeld **angeben** Namens für Ihre app, und wählen Sie unter **Wie möchten Sie hosten Ihre app für SharePoint** **in SharePoint gehostet**. Geben Sie auch die URL der **Duet-Workflow-Website** soll gegen unter **welche SharePoint-Website soll für das debugging Ihrer app** Debuggen, und wählen **Fertig stellen**.
    
  
7. Wählen Sie im Menü erstellen **Deploy** *< Ihr app-Name >*  .
    
  
Nachdem die app bereitgestellt wird, wird in Visual Studio die Standardseite für die app gestartet.
  
    
    

## Fügen Sie den externen Inhaltstyp und externe Listen der App
<a name="bkmk_AddECT"> </a>

BCS müssen der externen Datenquelle darauf aufmerksam gemacht werden. Dies erfolgt mithilfe eines externen Inhaltstyps.
  
    
    

### So fügen Sie einen externen Inhaltstyp hinzu:


1. Klicken Sie im **Projektmappen-Explorer** öffnen Sie das Kontextmenü für das Projekt zu, und wählen Sie **Hinzufügen** **von Inhaltstypen für die externe Datenquelle**.
    
  
2. Geben Sie auf der Seite **OData-Quelle anzugeben** die URL der Duet Enterprise-Workflow-Dienst. Es folgt ein Beispiel für eine Duet-Dienst-URL:
    
     *https://<<DUETGATEWAY>>:<<Port>>/SAP/OPU/OData/IWWRK/DUET_WORKFLOW_CORE;Mo;c=SHAREPOINT_DE* 
    
  
3. Wählen Sie einen Namen für die ODATA-Quelle beispielsweise SAPWorkflows, und wählen Sie dann auf **Weiter**.
    
  
4. Geben Sie den SAP-Benutzername und das Kennwort zum Herstellen der Webdienst-Metadaten.
    
  
5. Es wird eine Liste von Datenentitäten, die vom OData Service verfügbar gemacht werden angezeigt. Wählen Sie die Entitäten, die Sie in den externen Inhaltstyp aufnehmen möchten. Wählen Sie in diesem Beispiel wird mit der SAP-Workflow-Dienste aus den folgenden:
    
  - **AttachmentCollection**
    
  
  - **CommentCollection**
    
  
  - **DecisionOptionCollection**
    
  
  - **ExtensibleElementCollection**
    
  
  - **ParticipantCollection**
    
  
  - **ServiceOperations**
    
  
  - **TaskDescriptionCollection**
    
  
  - **WorkflowTaskCollection**
    
  
6. Aktivieren Sie das Kontrollkästchen **Listeninstanzen für die ausgewählten Datenentitäten (mit Ausnahme von Dienstvorgänge) erstellen**.
    
  
7. Wählen Sie **Fertig stellen** aus.
    
  

> **HINWEIS**
> Stellen Sie sicher, dass der SAP-Workflow-Dienst Automatische Generierung BDC-Tools in Visual Studio für die Standardauthentifizierung ermöglicht.
  
    
    


## Hinzufügen einer benutzerdefinierten Aktion Feature zu einer Duet Enterprise 2.0 Workflow-Liste
<a name="bkmk_AddCustomAction"> </a>

Um eine Möglichkeit für den Benutzer zum Arbeiten mit der neuen Funktionalität einer SharePoint-Liste hinzugefügt zu gewährleisten, werden die folgenden Schritte im Menü Websiteaktionen ein Element hinzuzufügen.
  
    
    

### So fügen Sie eine benutzerdefinierte Aktion hinzu:


1. Klicken Sie mit der rechten Maustaste auf das SharePoint-Add-In-Projekt und fügen Sie ein neues Element **Benutzerdefinierte UI-Aktion** hinzu.
    
  
2. Öffnen Sie die Datei **Elements.xml** der benutzerdefinierten Aktion Host-Web-Feature. Durch den Code in der Datei durch den folgenden XML-Code ersetzen, wird die benutzerdefinierte Aktion:
    
  - Deklarieren einer benutzerdefinierten ECB-Aktion und ihrer Attribute.
    
  
  - Deklarieren Sie eine benutzerdefinierte Aktion und ihrer Attribute.
    
  
  - Deklarieren Sie das app-Webseite Ziel und die Werte, die durch die Abfragezeichenfolge übergeben werden.
    
    Das Element **UrlAction** verwendet mehrere Token. Weitere Informationen finden Sie unter [URL-Zeichenfolgen und Tokens in Add-Ins für SharePoint](http://msdn.microsoft.com/library/800ec8cd-a448-46bc-b41e-d4030eeb4048%28Office.15%29.aspx).
    
  

  ```XML
  
<?xml version="1.0" encoding="utf-8"?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
  <CustomAction Id="ViewDetailsECBCustomAction"
          RegistrationType="List"
          RegistrationId="107"
          Location="EditControlBlock"
          Sequence="1"
          ImageUrl="_layouts/15/images/placeholder16x16.png"
          Title="View Details">
     <UrlAction Url="~appWebUrl/Pages/ViewDetails.aspx?
            {StandardTokens}&amp;amp;ListId={ListId}&amp;amp;ItemId={ItemId}"/>
  </CustomAction>

  <CustomAction
          Id="ViewDetailsRibbonAction"
          RegistrationId="107"
          RegistrationType="List"
          Location="CommandUI.Ribbon"
          Title="View Details"
          HostWebDialog="false"
          HostWebDialogHeight="500"
          HostWebDialogWidth="500">
    <CommandUIExtension>
      <CommandUIDefinitions>
        <CommandUIDefinition 
              Location="Ribbon.ListItem.Workflow.Controls._children">
          <Button
              Id="Ribbon.ListItem.Workflow.ViewDetails"
              Alt="View Details"
              Sequence="25"
              Command="Invoke_ViewDetailsSAPWorkflow"
              LabelText="View Details"
              TemplateAlias="o1"
              Image32by32="_layouts/15/images/placeholder32x32.png"
              Image16by16="_layouts/15/images/placeholder16x16.png"/>
        </CommandUIDefinition>
      </CommandUIDefinitions>
      <CommandUIHandlers>
        <CommandUIHandler
          Command="Invoke_ViewDetailsSAPWorkflow"
          CommandAction="~appWebUrl/Pages/ViewDetails.aspx?
           {StandardTokens}&amp;amp;ListId={ListId}&amp;amp;ItemId={SelectedItemId}"/>
      </CommandUIHandlers>
    </CommandUIExtension>
  </CustomAction>
</Elements>
  ```

3. Fügen Sie dem Projekt ViewDetails.aspx Seite und eine neue Seite hinzu.
    
  
4. Drücken Sie **F5** zum Erstellen und Bereitstellen der SharePoint-app.
    
  
5. Wechseln Sie zu der **WorkItem** -Liste im hostweb, und wählen Sie im Kontextmenü **Anzeigen**. Sie werden auf **ViewDetails.aspx**geleitet werden.
    
  

> **HINWEIS**
> Stellen Sie sicher, dass der SAP-Workflow-Dienst die Standardauthentifizierung ermöglicht. BDC-automatische Generierung Tools in Visual Studio derzeit unterstützen nur anonyme und die Standardauthentifizierung.
  
    
    

Wenn Sie die app bereitstellen und navigieren Sie zu **Listen/WorkflowTaskCollection** innerhalb Ihrer app, sehen Sie die folgende Meldung angezeigt:
  
    
    
"Nachricht vom externen System:"LobSystem (externes System) hat Authentifizierungsfehler zurückgegeben."".
  
    
    
Um dieses Problem zu beheben, müssen Sie die Bereitstellung von Authentifizierungsanmeldeinformationen BCS und dem SAP-Back-End-app-auf Hinzufügen.
  
    
    

## Sicherheit für einmaliges Anmelden an die app hinzufügen
<a name="bkmk_AddSSO"> </a>

Duet Enterprise Single Sign-On können Benutzer authentifiziert werden, damit sie auf SharePoint und SAP-Seiten mit einem anmelden Ressourcen zugreifen können.
  
    
    

### So fügen Sie einmaliges Anmelden hinzu:


1. Erstellen einer OData-Verbindungs den folgenden Befehl auf der SharePoint-Verwaltungsshell ausführen.
    
  ```XML
  
New-SPODataConnectionSetting -Name SAPWorkflow
-ServiceContext "http://localhost" -ExtensionProvider 
"OBA.Server.Canary.ObaOdataServerExtensionProvider, OBA.Server.SSOProvider, 
Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c" 
-AuthenticationMode passthrough -ServiceAddressURL 
"https://<<DUETGATEWAY>>:<<PORT>>/sap/opu/odata/IWWRK/
DUET_WORKFLOW_CORE;mo;c=SHAREPOINT_DE"

  ```

2. Doppelklick auf **WorkflowTaskCollection.ect**.
    
  
3. Aktualisieren Sie im Fenster Eigenschaften den Wert der  `ODataConnectionSettingId` auf *SAPWorkflow*  .
    
  
4. Zulassen der App, die Verbindung zu verwenden.
    
  
5. Öffnen Sie die **Datei AppManifest.xml**.
    
  
6. In **Berechtigungsanfragen**, select **Bereich**, **BCS** dann **Berechtigung** = lesen.
    
  
7. Drücken Sie **F5**, um die app bereitstellen.
    
  
8. Wählen Sie auf der Seite **Berechtigungen** für die app die OData-Verbindung, die diese app verwendet werden soll.
    
  
9. Klicken Sie auf die Schaltfläche **Vertrauen**.
    
  
10. Navigieren Sie zu **. / Listen/WorkflowTaskCollection**. Die SAP-Aufgaben zugewiesen, die direkt aus dem SAP-System stammen sollte angezeigt werden.
    
  

## Access-Workflowaufgaben und zugehörigen Elemente mit JSOM
<a name="bkmk_UseJSOM"> </a>

Da SharePoint-Add-Ins Clientcode Kommunikation mit SharePoint verwenden müssen, wird die folgenden Interaktion mit der SAP-Workflowaufgaben mithilfe des JavaScript-Objektmodells veranschaulichen.
  
    
    

### So erstellen Sie den Code:


1. Mit der rechten Maustaste in des Ordners **Seiten** im **Projektmappen-Explorer**, fügen Sie eine Seite .NET und nennen Sie es **ViewDetails.aspx**
    
  
2. Fügen Sie den folgenden HTML-Code im Abschnitt  `PlaceHolderAdditionalPageHead` . Wird den Stylesheet-Verweis festgelegt, laden Sie das Skript, und klicken Sie dann die zugehörige Workflowaufgaben aus SAP zurückzugeben.
    
  ```XML
  
<link
    rel="Stylesheet" 
    type="text/css"
    href="../Content/App.css"/>

<script 
    src="//ajax.aspnetcdn.com/ajax/4.0/1/MicrosoftAjax.js" 
    type="text/javascript">
</script>
<script 
    src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js" 
    type="text/javascript">
</script>      
<script 
    src="../Scripts/ViewDetails.js" 
    type="text/javascript">
</script>

  ```

3. Mit der rechten Maustaste in des Ordners **Scripts**, und fügen Sie eine JavaScript-Datei. Nennen Sie es **ViewDetails.js**.
    
  
4. Mit diesem Markup wird Folgendes:
    
  - Ruft die  `ClientContext` für die übergeordnete Website.
    
  
  - Ruft die  `TaskInstanceParentId` für die ausgewählte Workflowaufgabe mithilfe der Duet Workflow Listen-GUID und das ausgewählte Element-ID.
    
  
  - Analysiert die  `TaskInstanceParentId` zum Abrufen von SAP-Ursprung und die ID für den Workflow in SAP.
    
  
  - Lädt die Entitäten für externen Inhaltstypen.
    
  
  - Liest die bestimmten Workflow-Aufgabe von SAP mit bestimmten Suchmethode, indem Sie SAP Ursprung und der Workflow Vorgangsnummer für SAP als Parameter übergeben.
    
  
  - Liest die zugeordneten Elemente für die Workflowaufgabe von SAP an.
    
  
5. Es folgt der vollständige HTML und JavaScript für die Seite.
    
  ```
  
// This code runs when the DOM is ready. It ensures the SharePoint
// script files are loaded and then executes execOperation().
$(document).ready(function () {
  SP.SOD.executeFunc("sp.js", "SP.ClientContext", execOperation);
});

function execOperation() {
  retrieveTaskFromList();
}

function retrieveTaskFromList() {
  // Retrieves the ClientContext of the parent web.
  var clientContext = 
  new SP.ClientContext.get_current();
  var appContextSite = 
  new SP.AppContextSite(clientContext, 
      decodeURIComponent(getQueryStringParameter("SPHostUrl")));
  var web = appContextSite.get_web();
  // Loads the selected workflow task from the duet workflow list.
  var listGuid = decodeURIComponent(getQueryStringParameter("ListId"));
  var itemId = decodeURIComponent(getQueryStringParameter("ItemId"))
  this.workflowItem = web.get_lists().getById(
      listGuid.substring(1, listGuid.length - 1)).getItemById(itemId);
  clientContext.load(workflowItem);

  clientContext.executeQueryAsync(
    Function.createDelegate(this, this.onLoadingTaskSucceeded),
    Function.createDelegate(this, this.onError)
  );
}

function onLoadingTaskSucceeded() {
  var sapParameters = parseParentTaskId(workflowItem.
      get_item("TaskInstanceParentId"));
  getDataFromSAP(sapParameters);
}

// Parses the TaskInstanceParentId 
// to get SAP Origin and the id for the workflow in SAP.
function parseParentTaskId(parentTaskId) {
  var idlength = parseInt(parentTaskId.substring(0, 2));
  var sapParameters = new Object();
  if (parentTaskId.length > (17 + idlength)) {
    sapParameters.workitemId = parentTaskId.substring(5, 5 + idlength);
    sapParameters.sapOrigin = parentTaskId.substring(17 + idlength);
  }
  return sapParameters;
}
    
// Retrieves the workflow task and associated elements from SAP.
function getDataFromSAP(sapParameters) {
  context = new SP.ClientContext.get_current();
  //Loads the entities for the external content types.
  wfEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "WorkflowTaskCollection");
  context.load(wfEntity);
  wfExtensibleElementEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "ExtensibleElementCollection");
  context.load(wfExtensibleElementEntity);
  wfCommentsEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "CommentCollection");
  context.load(wfCommentsEntity);
  wfDecisionsEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "DecisionOptionCollection");
  context.load(wfDecisionsEntity);
  wfParticipantEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "ParticipantCollection");
  context.load(wfParticipantEntity);
  wfDescriptionEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "TaskDescriptionCollection");
  context.load(wfDescriptionEntity);
  wfAttachmentEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "AttachmentCollection");
  context.load(wfAttachmentEntity);

  // Loads a LOB system instances.
  lobSystem = wfEntity.getLobSystem();
  lobSystemInstanceCollection = lobSystem.getLobSystemInstances();
  context.load(lobSystemInstanceCollection);

  context.executeQueryAsync(onLoadingLOBInstanceSucceeded, onError);

  function onLoadingLOBInstanceSucceeded() {
    lobSystemInstance = lobSystemInstanceCollection.get_item(0);
    // Reads the workflow task from SAP using specific finder.
    var identifierValues = [sapParameters.sapOrigin, sapParameters.workitemId];
    var entityIdentity = SP.BusinessData.Runtime.EntityIdentity.
          newObject(context, identifierValues);
    wfEntityInstance = wfEntity.findSpecific(entityIdentity, 
          "ReadSpecificWorkflowTask", lobSystemInstance);
    context.load(wfEntityInstance);

    context.executeQueryAsync(onLoadingWorkflowSucceeded, onError);
  }

  function onLoadingWorkflowSucceeded() {
    // Reads the associated elements for the workflow task.
    var exFilterCollection = wfExtensibleElementEntity.getFilters(
        "GetExtensibleElementsFromWorkflowTaskCollection");
    context.load(exFilterCollection);
    exElementsCollection = wfExtensibleElementEntity.findAssociated(
        wfEntityInstance, "GetExtensibleElementsFromWorkflowTaskCollection", 
        exFilterCollection, lobSystemInstance);
    context.load(exElementsCollection);

    var comFilterCollection = wfCommentsEntity.getFilters(
        "GetCommentsFromWorkflowTaskCollection");
    context.load(comFilterCollection);
    commentsCollection = wfCommentsEntity.findAssociated(
        wfEntityInstance, "GetCommentsFromWorkflowTaskCollection", 
        comFilterCollection, lobSystemInstance);
    context.load(commentsCollection);

    var decFilterCollection = wfDecisionsEntity.getFilters(
        "GetDecisionOptionsFromWorkflowTaskCollection");
    context.load(decFilterCollection);
    decisionsCollection = wfDecisionsEntity.findAssociated(
        wfEntityInstance, "GetDecisionOptionsFromWorkflowTaskCollection", 
        decFilterCollection, lobSystemInstance);
    context.load(decisionsCollection);

    var partFilterCollection = wfParticipantEntity.getFilters(
        "GetParticipantsFromWorkflowTaskCollection");
    context.load(partFilterCollection);
    participantsCollection = wfParticipantEntity.findAssociated(
        wfEntityInstance, "GetParticipantsFromWorkflowTaskCollection", 
        partFilterCollection, lobSystemInstance);
    context.load(participantsCollection);

    var descFilterCollection = wfDescriptionEntity.getFilters(
        "GetDescriptionFromWorkflowTaskCollection");
    context.load(descFilterCollection);
    descriptionCollection = wfDescriptionEntity.findAssociated(
        wfEntityInstance, "GetDescriptionFromWorkflowTaskCollection", 
        descFilterCollection, lobSystemInstance);
    context.load(descriptionCollection);

    var attaFilterCollection = wfAttachmentEntity.getFilters(
        "GetAttachmentsFromWorkflowTaskCollection");
    context.load(attaFilterCollection);
    attachmentCollection = wfAttachmentEntity.findAssociated(
        wfEntityInstance, "GetAttachmentsFromWorkflowTaskCollection", 
        attaFilterCollection, lobSystemInstance);
    context.load(attachmentCollection);

    context.executeQueryAsync(getUpdatedDataSucceeded, onError);
  }

  function getUpdatedDataSucceeded() {
    alert("# of extensible elements: " + exElementsCollection.get_count() + 
      "\\n# of comments: " + commentsCollection.get_count() + 
      "\\n# of decision options: " + decisionsCollection.get_count() + 
      "\\n# of attachments: " + attachmentCollection.get_count() + 
      "\\n# of participants: " + participantsCollection.get_count());
  }

  function onError(sender, e) {
    alert("Request failed. " + e.get_message() +
          "\\n" + e.get_stackTrace());
  }
}

// Retrieves a query string value.
function getQueryStringParameter(paramToRetrieve) {
  var params = document.URL.split("")[1].split("&amp;");
  var strParams = "";
  for (var i = 0; i < params.length; i = i + 1) {
    var singleParam = params[i].split("=");
    if (singleParam[0] == paramToRetrieve)
      return singleParam[1];
  }
}

  ```

6. Öffnen Sie die **Datei AppManifest.xml** aus.
    
  
7. **Berechtigungsanfragen** wählen Sie **Bereich**, **Web- und Berechtigung** aus und legen Sie den Wert zum *Lesen von*  .
    
  
8. Drücken Sie **F5**, um die Lösung in der Duet-Workflow-Website bereitzustellen.
    
  
9. Wechseln Sie auf **Meine Workflowaufgaben**. Wählen Sie in der ECB-Menü **Anzeigen** oder wählen Sie eine Aufgabe aus, und drücken Sie **Details anzeigen** im Menüband. Sie werden zur **ViewDetails.aspx** umgeleitet werden, in dem Sie eine Benachrichtigung mit der Anzahl für einige der Elemente, die den Workflow zugeordnet sind angezeigt wird.
    
  

## Mithilfe von HTML und JavaScript zum Rendern von Custom UI
<a name="bkmk_UseJSOM"> </a>

Ersetzen Sie den folgenden Code in ViewDetails.aspx mit eigenen HTML und JavaScript zum Rendern von einer eigene benutzerdefinierten Benutzeroberfläche.
  
    
    

```

alert("# of extensible elements: " + exElementsCollection.get_count() +
"\\n# of comments: " + commentsCollection.get_count() + 
"\\n# of decision options: " + decisionsCollection.get_count() + 
"\\n# of attachments: " + attachmentCollection.get_count() + 
"\\n# of participants: " + participantsCollection.get_count());

```


## Hinzufügen von Lync-Steuerelement auf der Detailseite an
<a name="bkmk_UseJSOM"> </a>

Hier ist eine Option für die benutzerdefinierte Benutzeroberfläche. Hinzufügen eines Steuerelements Lync erhalten Sie die Möglichkeit, die Lync-Kontakte aus der benutzerdefinierten Seite kommunizieren.
  
    
    

### So fügen Sie ein Lync-Steuerelement hinzu:


1. Mit der rechten Maustaste im Projektmappen-Explorer Ordner **Scripts**, fügen Sie eine JavaScript-Datei, und nennen Sie sie **People.js**.
    
  
2. Das folgende Markup wird:
    
  - Fügen Sie Anwesenheitsinformationen-Steuerelement für die Teilnehmer des Vorgangs.
    
  
  - Lync-Integration in Legende für Teilnehmer für die Zusammenarbeit mit einem Klick für den Vorgang.
    
  

    Fügen Sie den folgenden Code in die Seite **People.js**.
    


  ```
  
var presenceControlCount = 0;
var appWebURL = '';

function AddPeopleControl(userName) {
  var id = "pc_participants_" + presenceControlCount++;
  var prevId = null;

  return "<div id='" + id + "' ></div>" + 
    "<script>" + 
      "LoadPeopleControl('" +id+ "', '" +prevId+ "', '" +userName+ "');" + 
    "</script>";
}

function LoadPeopleControl(elemId, prevId, user) {
  var prevElement = null;
  var element = $("#" + elemId);
  user = "fareast\\\\" + user.toLowerCase();

  GetUsersInfo(user, element, prevElement);
}

function GetUsersInfo(accountName, htmlElement, waitforElement) {
  var clientContext = SP.ClientContext.get_current();
  var website = clientContext.get_web();

  clientContext.load(website);
  clientContext.executeQueryAsync(onRequestSucceeded, onRequestFailed);

  function onRequestSucceeded() {
    appWebURL = website.get_url();
    var queryURL = appWebURL + 
    "/_api/SP.UserProfiles.PeopleManager/GetPropertiesFor(accountName=@v)?@v='"
     + accountName + "'";

    jQuery.ajax({
      url: queryURL,
      type: "GET",
      headers: {
        "ACCEPT": "application/json;odata=verbose"
      },
      success: function (data) {
        var html = [];
        var i = 0;

        var id = htmlElement[0].id + "_";

        var about_info;

        if (data.d.UserProfileProperties != null) {
          for (i = 0; i < data.d.UserProfileProperties.results.length; i++) {
            if (data.d.UserProfileProperties.results[i].Key == "AboutMe") {
              about_info = data.d.UserProfileProperties.results[i].Value;
            }
          }
        }

        var email = data.d.Email;
        var profileUrl = data.d.UserUrl;
        var pictureUrl = data.d.PictureUrl;
        var name = data.d.DisplayName;

        if (name == null) {
          name = accountName;
        }

        var isFollowed = data.d.IsFollowed;

        var html = [
          "<table>",
          "<tr>",
          "<td>",
            "<span class='ms-spimn-presenceWrapper ms-spimn-imgSize-5x48'> ",
              "<img id='imn_" + id + "_" + i++ + ",type=smtp'" ,
                "onload=\\"IMNRC('", email, "')\\" ",
                "class='ms-spimn-img ms-spimn-presence-offline-5x48x32 '", 
                "title='' name='imnmark' alt='Offline' ", 
                "src='/_layouts/15/images/spimn.png' ", 
                "sip='" + email + "' showofflinepawn='1'>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-imnSpan'>",
              "<a class='ms-imnlink' tabindex='-1'", 
                "onclick='IMNImageOnClick(event);return false;' href='#'>",
                "<img id='imn_" + id + "_" + i++ + ",type=smtp'",
                  "onload=\\"IMNRC('", email, "')\\"",
                  "class=' ms-hide' title='' name='imnmark' alt='Away' ",
                  "src='/_layouts/15/images/spimn.png' ",
                  "sip='" + email + "' showofflinepawn='1'>",
              "</a>",
              "<a class='ms-subtleLink ms-peopleux-imgUserLink' ",
                "onclick='GoToLinkOrDialogNewWindow(this);return false;' ",
                "href='" + profileUrl + "'>",
                "<span style='width: 48px; height: 48px;'" ,
                  "class='ms-peopleux-userImgWrapper'>",
                "<img style='cliptop: 0px; clipright: 48px;" ,
                  "clipbottom: 48px; clipleft: 0px; ",
                  "min-height: 48px; min-width: 48px; max-width: 48px;' ",
                  "class='ms-peopleux-userImg' alt='' src='" +pictureUrl+ "'>",
                "</span>",
              "</a>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-spimn-presenceWrapper ms-spimn-imgSize-5x48'>",
            "<img id='imn_" + id + "_" + i++ + ",type=smtp' ",
              "onload=\\"IMNRC('", email, "')\\" ",
              "class='ms-spimn-img ms-spimn-presence-offline-5x48x32' ",
              "title='' name='imnmark' alt='Offline' ",
              "src='/_layouts/15/images/spimn.png' ",
              "sip='" + email + "' showofflinepawn='1'>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-microfeed-userName ms-textLarge' ",
              "style='font-size: medium;'>" + name + "<br/>",
              "<div id='people_callout_" + id + "_" + i + "' ",
                "class='ms-microfeed-button ms-textSmall" + 
                "ms-secondaryCommandLink ms-microfeed-footerButton' ",
                "align='center' ",
                "style='cursor:pointer; font-size:small;' > more... </div>",
                  "<script>",
                    "RegisterCallOut(\\"people_callout_" + id + "_" + i + 
                      "\\",\\"" + name + "\\",\\"" + encodeURI(about_info) + 
                      "\\",\\"" + profileUrl + "\\",\\"" + isFollowed + "\\");",
                  "</script>",
            "</span>",
          "</td>",
          "</tr>",
          "</table>",
        ];

        htmlElement.html(html.join(''));
      }

    });
  }
  function onRequestFailed(sender, args) {
    alert('Error: ' + args.get_message());
  }

}

function RegisterCallOut(divId, displayName, aboutme, userUrl, isFollowed) {
  if (typeof CalloutManager !== "object" || typeof Callout !== "function" || typeof CalloutAction !== "function")
    return;

  var launchdiv = document.getElementById(divId);
  var calloutId = divId + "_callout";
  var html = [];

  html.push("<br/>");

  if (aboutme == "") {
    html.push("No Information Found about this person.");
  }
  else {
    html.push(decodeURI(aboutme));
  }

  html.push("<hr/>");

  if (isFollowed == true) {
    html.push("<div>You are <b>following</b> this person</div>");
  }
  else {
    html.push("<div >You are <b>not following</b> this person</div>");
  }

  var callout = CalloutManager.createNew({
    launchPoint: launchdiv,
    openOptions: {
      closeCalloutOnBlur: true,
      event: "click",
      showCloseButton: true
    },
    ID: calloutId,
    title: displayName,
    content: html.join("")
  });

  callout.addAction(new CalloutAction({
    text: "View Profile", onClickCallback: 
    function (calloutActionClickEvent, calloutAction) {
      window.open(userUrl);
    }
  }));
}

  ```


> **HINWEIS**
> Der Benutzername des Teilnehmers im Netzwerk des Unternehmens entspricht, die in SAP.
3. Öffnen Sie die **Datei AppManifest.xml** -Seite.
    
  
4. **Berechtigungsanfragen** wählen Sie **Bereich**, **Benutzerprofile und Berechtigung** und legen Sie dessen Wert zum *Lesen*  .
    
  
5. Kopieren Sie das folgende Markup, und fügen Sie ihn im Abschnitt  `PlaceHolderAdditionalPageHead` **ViewDetails.aspx**.
    
  ```HTML
  
<script src="../Scripts/People.js" type="text/javascript"></script>
  ```

6. Rufen Sie auf, um das Steuerelement Anwesenheitsinformationen für einen Teilnehmer hinzuzufügen, Folgendes:
    
  ```
  AddPeopleControl(userName);
  ```


## Weitere Ressourcen
<a name="bk_addresources"> </a>


-  [SharePoint-Add-Ins](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)
    
  
-  [Ausführen grundlegender Vorgänge unter Verwendung von SharePoint 2013-Clientbibliothekscode](http://msdn.microsoft.com/library/5a69c9e3-73bf-4ed5-bc19-182056bdb394%28Office.15%29.aspx)
    
  
-  [Eine Einführung in SAP Business Workflow](http://scn.sap.com/docs/DOC-31056)
    
  

