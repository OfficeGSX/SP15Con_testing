---
title: Vorgehensweise folgen Sie Menschen mit der JavaScript-Objektmodell in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 2643c286-47c9-4a7a-9273-7474394477d6
---


# Vorgehensweise: folgen Sie Menschen mit der JavaScript-Objektmodell in SharePoint 2013
Erfahren Sie, wie in der folgenden personenfeatures entwickelt, mithilfe der SharePoint Server 2013 JavaScript-Objektmodells.
## Gründe für die Verwendung in der folgenden personenfeatures in SharePoint Server 2013 ?
<a name="bk_FollowingPeopleFeatures"> </a>

In SharePoint Server 2013 helfen folgende personenfeatures, Benutzer miteinander verbundenen bleiben. Beispielsweise wenn ein Benutzer eine Person folgt, angezeigt dieser Person Beiträge und Aktivitäten im Newsfeed eines Benutzers. Mithilfe der folgenden Personen Features konzentrieren die Personen, die Benutzer kümmern können Sie die Relevanz Ihrer app oder Lösung verbessern. Personen, die Sie folgen werden im Objektmodell JavaScript [SocialActor](http://msdn.microsoft.com/library/4e369fd5-b9b0-9804-957e-b3e39c559cd4%28Office.15%29.aspx) Objekte dargestellt. Informationen zum folgenden Personen Kernaufgaben im Objektmodell von JavaScript ausführen, verwenden Sie das [SocialFollowingManager](http://msdn.microsoft.com/library/9ee1c0c0-b864-f0c3-f0cb-4dd4f1870dfa%28Office.15%29.aspx) -Objekt. In diesem Artikel veranschaulicht, wie das Objektmodell JavaScript folgenden personenfeatures entwickelt.
  
    
    

> **HINWEIS**
>  [SocialFollowingManager](http://msdn.microsoft.com/library/9ee1c0c0-b864-f0c3-f0cb-4dd4f1870dfa%28Office.15%29.aspx) ist die empfohlene API für folgende Personen und Inhalte verwenden. Das [PeopleManager](http://msdn.microsoft.com/library/985fd2df-0e31-6ece-b846-ba2ccb156d00%28Office.15%29.aspx) -Objekt enthält jedoch zusätzliche Funktionen für die folgenden Personen, wie die [AmIFollowedBy](http://msdn.microsoft.com/library/3641c469-0063-054d-355d-e56697cb08ae%28Office.15%29.aspx) -Methode und Methoden, die den folgenden Status anderer Benutzer zu erhalten.
  
    
    


## Voraussetzungen für das Einrichten Ihrer Entwicklungsumgebung in der folgenden personenfeatures entwickelt, mithilfe der SharePoint 2013 JavaScript-Objektmodells
<a name="bk_Prereqs"> </a>

Um die farmlösung erstellen, die das JavaScript-Objektmodell verwendet, um mit den folgenden Personen Features arbeiten, benötigen Sie Folgendes:
  
    
    

- SharePoint Server 2013 mit Meine Website konfiguriert und Benutzerprofile und persönliche Websites, die für den aktuellen Benutzer und einen Zielbenutzer erstellt
    
  
- Visual Studio 2012
    
  
- Office Developer Tools für Visual Studio 2012
    
  
- **Vollzugriff auf die Benutzerprofildienst-Anwendung für den angemeldeten Benutzer**
    
  
- Lokale Administratorberechtigungen für den angemeldeten Benutzer
    
  

## Erstellen einer Farm Solution und Anwendung Seite in Visual Studio 2012
<a name="bk_CreateSolution"> </a>


1. Führen Sie Visual Studio als Administrator aus, und wählen Sie **Datei**, **neu**, **Projekt**.
    
  
2. Wählen Sie im Dialogfeld **Neues Projekt** aus der Dropdownliste oben im Dialogfeld die Option **.NET Framework 4.5**.
    
  
3. In **der Vorlagenliste** erweitern Sie **Office/SharePoint**, wählen Sie **SharePoint-Lösungen**, und wählen Sie dann die Vorlage **SharePoint 2013 - leeres Projekt**.
    
  
4. Nennen Sie das Projekt FollowPeopleJSOM, und wählen Sie dann auf die Schaltfläche **OK**.
    
  
5. Wählen Sie in das Dialogfeld des **Assistenten zum Anpassen von SharePoint** **als farmlösung bereitstellen**, und wählen Sie dann auf die Schaltfläche **Fertig stellen**.
    
  
6. Im **Projektmappen-Explorer** das Kontextmenü für das Projekt **FollowPeopleJSOM** öffnen, und fügen Sie eine SharePoint "Layouts" Ordner zugeordnet.
    
  
7. Öffnen Sie im Ordner **Layouts** das Kontextmenü für den Ordner **FollowPeopleJSOM**, und fügen Sie eine neue SharePoint-Anwendungsseite namens FollowPeople.aspx.
    
    > **HINWEIS**
      > Die Codebeispiele in diesem Artikel legen benutzerdefinierten Code im Seitenmarkup verwenden jedoch kein Code-Behind-Klasse, Visual Studio für die Seite erstellt.
8. Öffnen Sie das Kontextmenü für die Seite FollowPeople.aspx, und wählen Sie dann **Set as Startup Item**.
    
  
9. Fügen Sie folgenden Code zwischen den "Main" **asp:Content** Tags im Markup der FollowPeople.aspx-Datei. Dieser Code definiert die Steuerelemente und Skriptverweise.
    
  ```HTML
  
<span id="followResults"></span><br/><br />
<button id="sendRequest" type="button"></button><br/>
<span id="message" style="color: #FF0000;"></span>

<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js" type="text/javascript"></script>

<SharePoint:ScriptLink name="SP.js" runat="server" ondemand="false" localizable="false" loadafterui="true" />
<SharePoint:ScriptLink name="SP.UserProfiles.js" runat="server" ondemand="false" localizable="false" loadafterui="true" />
<SharePoint:FormDigest id="FormDigest" runat="server"/>
<script type="text/javascript">

    // Replace this comment with the code for your scenario.

</script>
  ```


> **HINWEIS**
> Das Beispiel "Erhalten Sie nachfolgende Aktivitäten und beobachteter Personen" nicht verwenden, das Button-Steuerelement oder das Formular Digest-Steuerelement, das nur ist erforderlich für Vorgänge, die Serverinhalte zu aktualisieren. Eine Formulardigest generiert einen Nachrichtenhash für die sicherheitsüberprüfung verwendet.
10. Ersetzen Sie den Kommentar zwischen den Tags **script** durch das Codebeispiel aus einem der folgenden Szenarien:
    
  -  [Starten oder Beenden von Personen folgen](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_FollowPeople)
    
  
  -  [Nachfolgende Aktivitäten und beobachteter Personen heranziehen.](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_GetFollowers)
    
  
11. Wählen Sie zum Testen der Lösung auf der Menüleiste **Debuggen**, **Debuggen starten** aus.
    
  

## Codebeispiel: Starten oder beenden nach Personen, für die mithilfe der SharePoint 2013 JavaScript-Objektmodells
<a name="bk_FollowPeople"> </a>

Im folgenden Codebeispiel wird der aktuelle Benutzer Start nach oder einen Zielbenutzer nach Beenden. Außerdem wird gezeigt, wie Sie:
  
    
    

- Überprüfen Sie, ob der aktuelle Benutzer einen Zielbenutzer folgt, mit der  [IsFollowed](http://msdn.microsoft.com/library/2c1f62e6-fb75-ad4d-c081-36408b418c21%28Office.15%29.aspx) -Methode.
    
  
- Rufen Sie die Anzahl der Personen, die der aktuelle Benutzer mithilfe der Methode  [GetFollowedCount](http://msdn.microsoft.com/library/97b53b4f-481a-cf41-1854-8f3ff860b2bb%28Office.15%29.aspx) folgt.
    
  
- Starten Sie den Zielbenutzer mithilfe der Methode  [Führen Sie die](http://msdn.microsoft.com/library/40d14320-27ba-2941-b0e2-be3b5a407c89%28Office.15%29.aspx) folgenden.
    
  
- Folgen den Zielbenutzer mithilfe der  [StopFollowing](http://msdn.microsoft.com/library/65b0e9be-dc5e-09fb-c57f-7a933de09a4c%28Office.15%29.aspx) -Methode nicht mehr.
    
  

> **HINWEIS**
> Fügen Sie den folgenden Code zwischen den **script** -Tags, die Sie in der Prozedur [Erstellen einer farmlösung und Anwendungsseite](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_CreateSolution) hinzugefügt. Ändern Sie den Platzhalterwert für die Variable **targetUser** vor dem Ausführen des Codes.
  
    
    


```

// Replace the placeholder value with the account name of the target user.
var targetUser = 'domain\\\\userName';

var clientContext;
var followingManager;
var actorInfo;
var isFollowed;
var followedCount;

// Ensure that the SP.UserProfiles.js file is loaded before running your code.
$(document).ready(function () {
    SP.SOD.executeOrDelayUntilScriptLoaded(getFollowingStatus, 'SP.UserProfiles.js');
});

// Get the Following status of the current user.
function getFollowingStatus() {

    // Get the current client context.
    clientContext = SP.ClientContext.get_current();

    // Get the SocialFeedManager instance.
    followingManager = new SP.Social.SocialFollowingManager(clientContext);

    // Create a SocialActorInfo object to represent the target user.
    actorInfo = new SP.Social.SocialActorInfo();
    actorInfo.set_accountName(targetUser);

    // Find out whether the current user is following the target user.
    isFollowed = followingManager.isFollowed(actorInfo);
    followedCount = followingManager.getFollowedCount(1);

    // Get the information from the server.
    clientContext.executeQueryAsync(showFollowingStatus, requestFailed)
}

// Show the Following status of the current user.
function showFollowingStatus() {
    var results = '';
    results += 'Is the current user following the target user? ' + isFollowed.get_value();
    results += '<br/>Total count of followed people: ' + followedCount.get_value();
    $('#followResults').html(results);

    // Initialize the button for this example.
    $('#sendRequest').click(
        function () {
            $('#message').empty();
            toggleFollowingStatus();
        });
    $('#sendRequest').text('Toggle following status');
}

// Follow or stop following the target user.
function toggleFollowingStatus() {
    if (isFollowed.get_value() === false) {
        followingManager.follow(actorInfo);
    }
    else if (isFollowed.get_value() === true) {
        followingManager.stopFollowing(actorInfo);
    }
    clientContext.executeQueryAsync(getFollowingStatus, requestFailed);
}

// Failure callback.
function requestFailed(sender, args) {
    $('#message').html('Error: ' + args.get_message());
}
```


## Codebeispiel: Abrufen nachfolgende Aktivitäten und gefolgt von Personen mithilfe der SharePoint 2013 JavaScript-Objektmodells
<a name="bk_GetFollowers"> </a>

Das folgende Codebeispiel ruft die Personen, die der aktuelle Benutzer folgt und ruft die Personen, die vom aktuellen Benutzer eingehalten werden. Außerdem wird gezeigt, wie Sie:
  
    
    

- Rufen Sie die Personen, die der aktuelle Benutzer mithilfe der Methode  [GetFollowed](http://msdn.microsoft.com/library/432a7cec-6add-fdb1-a79f-a93414ee8cd3%28Office.15%29.aspx) folgt.
    
  
- Rufen Sie die Personen, die den aktuellen Benutzer folgen, indem Sie mithilfe der Methode  [GetFollowers](http://msdn.microsoft.com/library/ae4a944b-c043-05fb-c74b-101d2ce4a813%28Office.15%29.aspx) und übergeben **1** **User** Actor Typen dargestellt.
    
  
- Durchlaufen Sie die Gruppen von Benutzern und Get Person Name, persönliche Website URI anzeigen sowie Bild URI.
    
  

> **HINWEIS**
> Fügen Sie den folgenden Code zwischen den **script** -Tags, die Sie in der Prozedur [Erstellen einer farmlösung und Anwendungsseite](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_CreateSolution) hinzugefügt.
  
    
    


```

var followed;
var followers;

// Ensure that the SP.UserProfiles.js file is loaded before running your code.
$(document).ready(function () {
    SP.SOD.executeOrDelayUntilScriptLoaded(getFollowedAndFollowers, 'SP.UserProfiles.js');

    // Hide the button for this example.
    $('#sendRequest').hide();
});

// Get the Following status of the current user.
function getFollowedAndFollowers() {

    // Get the current client context.
    var clientContext = SP.ClientContext.get_current();

    // Get the SocialFeedManager instance.
    var followingManager = new SP.Social.SocialFollowingManager(clientContext);

    // Get followed people and followers.
    followers = followingManager.getFollowers();
    followed = followingManager.getFollowed(1);

    // Send the request to the server.
    clientContext.executeQueryAsync(showFollowedAndFollowers, requestFailed)
}

// Show the Following status of the current user.
function showFollowedAndFollowers() {
    var results = 'The current user is following ' + followed.length + ' people: <br/>';

    for (var i = 0; i < followed.length; i++) {
        var user = followed[i];
        var name = user.get_name();
        var personalSiteUri = user.get_personalSiteUri();
        var pictureUri = user.get_imageUri();

        results += '<br/>' + name + '<br/>' + personalSiteUri + '<br/>' + pictureUri + '<br/>';
    }

    results += '<br/>The current user is followed by ' + followers.length + ' people: <br/>';

    for (var i = 0; i < followers.length; i++) {
        var user = followers[i];
        var name = user.get_name();
        var personalSiteUri = user.get_personalSiteUri();
        var pictureUri = user.get_imageUri();

        results += '<br/>' + name + '<br/>' + personalSiteUri + '<br/>' + pictureUri + '<br/>';
    }
    $('#followResults').html(results);
}

// Failure callback.
function requestFailed(sender, args) {
    $('#message').html('Error: ' + args.get_message());
}
```


## Zusätzliche Ressourcen
<a name="bk_AdditionalResources"> </a>


-  [Folgen von Personen in SharePoint 2013](follow-people-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: führen Sie die Personen mithilfe des clientobjektmodells .NET SharePoint 2013](how-to-follow-people-by-using-the-net-client-object-model-in-sharepoint-2013.md)
    
  

