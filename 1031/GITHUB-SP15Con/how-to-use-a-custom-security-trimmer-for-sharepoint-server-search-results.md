---
title: Vorgehensweise verwenden ein benutzerdefinierten Security Trimmer für SharePoint Server-Suchergebnisse
ms.prod: SHAREPOINT
ms.assetid: e1a8664e-fb43-45c2-83aa-9635fe1efc99
---



# Vorgehensweise: verwenden ein benutzerdefinierten Security Trimmer für SharePoint Server-Suchergebnisse
Diese Vorgehensweise führt Sie durch die Schritte zum Implementieren - erstellen, bereitstellen und registrieren - einen benutzerdefinierten Security Trimmer für Suche in SharePoint 2013 mithilfe von Microsoft Visual Studio 2010.
 * **Gilt für:*** 
  
    
    


|||
|:-----|:-----|
|**In diesem Artikel**          [Voraussetzungen](#PreReq)           [Einrichten eines benutzerdefinierten Security Trimmer-Projekts](#SetUp)           [Erstellen eines vor dem benutzerdefinierten Security trimmers](#CreatePreTrimmer)           [Erstellen eines nach der benutzerdefinierten Security trimmers](#CreatePostTrimmer)           [Registrieren des benutzerdefinierten Security Trimmers.](#RegisterTrimmer)           [Zusätzliche Ressourcen](#bk_sectrimmer_addlresources)||
   

Suche in SharePoint 2013 führt Abfragezeit die Einschränkung aus Sicherheitsgründen von Suchergebnissen. Möglicherweise gibt es jedoch Szenarien, in denen Sie benutzerdefinierte Sicherheitskürzung ausführen möchten. Suche in SharePoint 2013 bietet Unterstützung für diese Szenarien über die  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) , [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) und Schnittstellen im Namespace [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) [ISecurityTrimmer2](https://msdn.microsoft.com/library/Microsft.Office.Server.Search.Query.ISecurityTrimmer2.aspx) (veraltet).
  
    
    

Es gibt zwei Arten von benutzerdefinierten Einschränkung aus Sicherheitsgründen: Trimming vor und nach dem Zuschneiden. Vor dem Zuschneiden bezieht sich auf vor der Abfrage Evaluierungshandbuch und exemplarische Vorgehensweisen, in dem die Abfrage umgeschrieben wird Sicherheitsinformationen hinzufügen, bevor die Abfrage mit dem Suchindex abgeglichen wird. Nach der Einschränkung aus Sicherheitsgründen bezieht sich auf die nach der Abfrage Evaluierungshandbuch und exemplarische Vorgehensweisen, in denen die Suchergebnisse gelöscht werden, bevor sie an den Benutzer zurückgegeben werden.Wir empfehlen die Verwendung von vor Einschränkung aus Sicherheitsgründen für Leistung und allgemeine Korrektheit; nach der Einschränkung aus Sicherheitsgründen wird verhindert, dass Informationslecks für Einschränkung Daten und Anzahl der Zugriffe Instanzen.Der Security Trimmer-Registrierungsprozess können Sie für den benutzerdefinierten Security Trimmer Konfigurationseigenschaften angeben.
## Übersicht

Benutzerdefinierte Security Trimming in Suche in SharePoint 2013 besteht aus zwei Schnittstellen, die verwendet werden können, um vor dem Zuschneiden oder nach der Trimming der Suchergebnisse auszuführen. Diese Vorgehensweise konzentriert sich auf beide Schnittstellen, beschreibt die erforderlichen Schritte zum Erstellen und registrieren Ihre eigene Security Trimmer ab.
  
    
    

## Voraussetzungen
<a name="PreReq"> </a>

Um diese Vorgehensweise ausführen zu können, müssen Sie Folgendes in Ihrer Entwicklungsumgebung installiert sein:
  
    
    

- Suche in Microsoft SharePoint 2013
    
  
- Microsoft Visual Studio 2010 oder ähnliche Microsoft .NET Framework - kompatibles Entwicklungstool
    
  

## Einrichten eines benutzerdefinierten Security Trimmer-Projekts
<a name="SetUp"> </a>

In diesem Schritt werden Sie erstellen des Projekts für den benutzerdefinierten Security Trimmer, und fügen Sie die erforderlichen Verweise auf das Projekt.
  
    
    

### So erstellen Sie das Projekt für einen benutzerdefinierten Security trimmer


1. Öffnen Sie Visual Studio, und wählen Sie **Datei**, **neu**, **Projekt**.
    
  
2. Wählen Sie in **Projekttypen** unter **c#** **SharePoint** aus.
    
  
3. Wählen Sie unter **Vorlagen** **Leeres SharePoint-Projekt**. Klicken Sie im Feld **Name** Geben Sie **CustomSecurityTrimmerSample**, und wählen Sie dann auf die Schaltfläche **OK**.
    
  
4. Klicken Sie im **Assistenten zum Anpassen von SharePoint**-Wählen Sie **als Farmlösung bereitstellen**, und wählen Sie dann auf **Fertig stellen**.
    
  

### So fügen Sie dem Projekt für den benutzerdefinierten Security Trimmer Verweise hinzu


1. Wählen Sie im **Projekt** auf **Verweis hinzufügen**.
    
  
2. Klicken Sie auf der Registerkarte **.NET** Wählen Sie die Verweise mit den folgenden Komponentennamen aus, und wählen Sie dann auf die Schaltfläche **OK**:
    
  - **Microsoft Search-Komponente**
    
    Auf der Registerkarte **.NET** mit den Namen der Komponente **Microsoft Search-Komponente** sollte zwei Einträge angezeigt werden. Wählen Sie den Eintrag, in die Spalte Pfad \\ISAPI\\Microsoft.Office.Server.Search.dll ist. Wenn dieser Eintrag auf der Registerkarte **.NET** im Dialogfeld **Verweise hinzufügen** nicht vorhanden ist, müssen Sie den Verweis von der Registerkarte **Durchsuchen** mithilfe des Pfads zu der Microsoft.Office.Server.Search.dll hinzufügen.
    
  
  - **Microsoft.IdentityModel**
    
    Wenn **Microsoft.IdentityModel** auf der Registerkarte **.NET** nicht aufgeführt ist, müssen Sie den Verweis auf die Microsoft.IdentityModel.dll über die Registerkarte **Durchsuchen** unter Verwendung des folgenden Pfads hinzufügen:
    
     `%ProgramFiles%\\Reference Assemblies\\Microsoft\\Windows Identity Foundation\\v4.0.`
    
  

## Erstellen eines vor dem benutzerdefinierten Security trimmers
<a name="CreatePreTrimmer"> </a>


### Die Klassendatei für den Security Trimmer vor dem Erstellen


1. Wählen Sie im Menü **PROJEKT** die Option **Neues Element hinzufügen** aus.
    
  
2. Klicken Sie unter **Visual C#-Elemente** in **Installierte Vorlagen** Wählen Sie **Code** aus, und wählen Sie dann auf **Klasse**.
    
  
3. Geben Sie **CustomSecurityPreTrimmer.cs** ein, und wählen Sie dann auf **Hinzufügen**.
    
  

### Schreiben den benutzerdefinierten Security Trimmer Pre-code

Ihre benutzerdefinierten Security Trimmer muss die **ISecurityTrimmerPre** -Schnittstelle implementieren. Im folgenden Codebeispiel wird eine einfache Implementierung dieser Schnittstelle.
  
    
    

### So ändern Sie den Standardcode in CustomSecurityPreTrimmer


1. Fügen Sie am Anfang der Klasse die folgenden **using**-Direktiven hinzu:
    
  ```cs
  
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Security.Principal;
using Microsoft.IdentityModel.Claims;
using Microsoft.Office.Server.Search.Query;
using Microsoft.Office.Server.Search.Administration;

  ```

2. Geben Sie an, dass die **CustomSecurityPreTrimmer** -Klasse die [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) -Schnittstelle in der Klassendeklaration implementiert, wie im folgenden Code dargestellt.
    
  ```cs
  
public class CustomSecurityPreTrimmer : ISecurityTrimmerPre
  ```


### Implementieren die ISecurityTrimmerPre-Schnittstellenmethoden


1. Fügen Sie den folgenden Code für die  [Initialize()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.Initialize.aspx) -Methodendeklaration hinzu.
    
  ```cs
  public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{

}
  ```


Die Basisversion von in diesem Beispiel enthält keinen Code in der **Initialize** -Methode.
    
  
2. Fügen Sie den folgenden Code für die  [AddAccess()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.AddAccess.aspx) -Methodendeklaration hinzu.
    
  ```cs
  
public IEnumerable<Tuple<Claim, bool>> AddAccess(
                IDictionary<string, object> sessionProperties, IIdentity passedUserIdentity)
{
//AddAccess method implementation, see steps 3-5.
}
  ```

3. Für den ersten Teil der Implementierung **AddAccess** -Methode herausfinden, die der Benutzer anhand der _passedUserIdentity_ist.
    
  ```cs
  
if (passedUserIdentity == null)
{
   throw new ArgumentException("AddAccess method is called with invalid user identity
   parameter", "passedUserIdentity");
}
            
   String strUser = null;
   var claimsIdentity = (IClaimsIdentity)passedUserIdentity;
   if (claimsIdentity != null)
   {
      foreach (var claim in claimsIdentity.Claims)
      {
         if (claim == null) 
         {
            continue;
         }
         
         // strUser is "domain\\\\user" format when web app is in Claims Windows Mode
         if (SPClaimTypes.Equals(claim.ClaimType, SPClaimTypes.UserLogonName))
         {
            strUser = claim.Value;
            break;
         }

         // strUser2 is "S-1-5-21-2127521184-1604012920-1887927527-66602" when web app is   
            in Legacy Windows Mode
         // In this case we need to convert it into NT user format.
         if (SPClaimTypes.Equals(claim.ClaimType, ClaimTypes.PrimarySid))
         {
            strUser = claim.Value;
            SecurityIdentifier sid = new SecurityIdentifier(strUser);
            strUser = sid.Translate(typeof(NTAccount)).Value;
            break;
         }
      }
}            

  ```

4. Erstellen einer Liste, füllen Sie die Liste von Ansprüchen und die Liste zurückzugeben, wie im folgenden Code dargestellt.
    
  ```cs
  
var claims = new LinkedList<Tuple<Claim, bool>>();
if (!string.IsNullOrEmpty(strUser))
   {
      var groupMembership = GetMembership(strUser);
      if (!string.IsNullOrEmpty(groupMembership))
      {
         var groups = groupMembership.Split(new[] {';'},
         StringSplitOptions.RemoveEmptyEntries);
         foreach (var group in groups)
         {
            claims.AddLast(new Tuple<Claim, bool>(
            new Claim("http://schemas.happy.bdc.microsoft.com/claims/acl", group), false));
         }
      }
   }
   return claims;
}

  ```


Die **GetMembership** -Methode enthält die benutzerdefinierte Logik des Ihrer Trimmer.
    
  

## Erstellen eines nach der benutzerdefinierten Security trimmers
<a name="CreatePostTrimmer"> </a>


### Erstellen Sie die Klassendatei für den nach der Security trimmer


1. Wählen Sie im Menü **PROJEKT** die Option **Neues Element hinzufügen** aus.
    
  
2. Klicken Sie unter **Visual C#-Elemente** in **Installierte Vorlagen** Wählen Sie **Code** aus, und wählen Sie dann **Klasse**.
    
  
3. Geben Sie CustomSecurityPostTrimmer.csein, und wählen Sie dann auf **Hinzufügen**.
    
  

### Den benutzerdefinierten Security Trimmer nach der Code zu schreiben

Ihre benutzerdefinierten Security Trimmer muss die **ISecurityTrimmerPost** -Schnittstelle implementieren. Das Codebeispiel in diesem Abschnitt wird eine einfache Implementierung dieser Schnittstelle.
  
    
    

### So ändern Sie den Standardcode in CustomSecurityPostTrimmer


1. Fügen Sie am Anfang der Klasse die folgenden **using**-Direktiven hinzu:
    
  ```cs
  
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Security.Principal;
using Microsoft.IdentityModel.Claims;
using Microsoft.Office.Server.Search.Query;
using Microsoft.Office.Server.Search.Administration;

  ```

2. Festlegen Sie, dass die **CustomSecurityPostTrimmer** -Klasse die [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) -Schnittstelle in der Klassendeklaration wie folgt implementiert:
    
  ```cs
  
public class CustomSecurityPostTrimmer : ISecurityTrimmerPost
  ```


### Implementieren die ISecurityTrimmerPost-Schnittstellenmethoden


1. Fügen Sie den folgenden Code für die  [Initialize()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.Initialize.aspx) -Methodendeklaration hinzu.
    
  ```cs
  public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{

}
  ```


Die Basisversion von in diesem Beispiel enthält keinen Code in der Initialize-Methode.
    
  
2. Fügen Sie den folgenden Code für die  [CheckAccess()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.CheckAccess.aspx) -Methodendeklaration hinzu.
    
  ```cs
  
public BitArray CheckAccess(IList<string> documentCrawlUrls, IList<string> documentAcls, IDictionary<string, object> sessionProperties, IIdentity passedUserIdentity)
{
//CheckAccess method implementation, see steps 3-5.
}
  ```

3. Für den ersten Teil der Implementierung der **CheckAccess** -Methode deklarieren und Initialisieren einer Variablen **BitArray** zum Speichern der Ergebnisse der Zugriffsüberprüfung für jede URL in der Auflistung **documentCrawlUrls** und Abrufen des Benutzers, der die Suchabfrage übermittelt hat, wie im folgenden Code dargestellt.
    
  ```cs
  
if (documentCrawlUrls == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid URL list",
   "documentCrawlUrls");
}
if (documentAcls == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid documentAcls   
   list", "documentAcls");
}
if (passedUserIdentity == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid user identity
   parameter", "passedUserIdentity");
}

  ```

4. Durchlaufen Sie jede Durchforstungs-URL in der Auflistung, und führen Sie die Zugriffsüberprüfung, um festzustellen, ob der Benutzer, übermittelt hat, dass die Abfrage der Durchforstungs-URLs zugreifen kann, Inhaltselemente verknüpft ist, wie im folgenden Code dargestellt.
    
  ```cs
  
// Initialize the bit array with TRUE value which means all results are visible by default.
var urlStatusArray = new BitArray(documentCrawlUrls.Count, true);
var claimsIdentity = (IClaimsIdentity)passedUserIdentity;
if (claimsIdentity != null)
{
   var userGroups = GetGroupList(claimsIdentity.Claims);
   var numberDocs = documentCrawlUrls.Count;
   for (var i = 0; i < numberDocs; ++i)
   {
      if (!string.IsNullOrEmpty(documentAcls[i]))
      {
         urlStatusArray[i] = VerifyAccess(documentAcls[i], userGroups);
      }
   }
}

  ```


Wenn der Benutzer Zugriff auf das Inhaltselement verfügt, legen Sie den Wert des Elements **BitArray** an diesem Index **urlStatusArray[i]**zu **true**; Legen Sie sie andernfalls zu **false**.
    
  
5. Legen Sie den Rückgabewert der Methode **CheckAccess** auf **urlStatusArray**, wie im folgenden Code gezeigt.
    
  ```cs
  
return urlStatusArray;
  ```


## Registrieren des benutzerdefinierten Security Trimmers.
<a name="RegisterTrimmer"> </a>

Dieser Schritt beschreibt, wie Sie den benutzerdefinierten Security Trimmer konfigurieren und umfasst die folgenden Aufgaben:
  
    
    

- Registrieren des benutzerdefinierten Security trimmers für die Suchdienstanwendung mithilfe der Windows PowerShell-Cmdlets.
    
  
- Neustarten des SharePoint Search Host Controllers (SPSearchHostController).
    
  

### Registrieren des benutzerdefinierten Security Trimmers.

Sie verwenden die SharePoint-Verwaltungsshell, um einen benutzerdefinierten Security Trimmer mit  _ClassName_registrieren. In unserem Fall konnte  _ClassName_ **CustomSecurityPreTrimmer** oder **CustomSecurityPostTrimmer**sein. Das folgende Verfahren zeigt, wie zum Registrieren eines benutzerdefinierten Security trimmers mit der ID für die Suchdienstanwendung auf 1 festgelegt.
  
    
    

### So registrieren Sie den benutzerdefinierten Security Trimmer


1. Gehen Sie in Windows Explorer zu **CustomSecurityTrimmerSample.dll** im Pfad _<Local_Drive>_:\\WINDOWS\\assembly.
    
  
2. Öffnen Sie das Kontextmenü für die Datei, und wählen Sie dann auf Eigenschaften.
    
  
3. Wählen Sie im Dialogfeld **Eigenschaften** auf der Registerkarte **Allgemein** das Token aus, und kopieren Sie es.
    
  
4. Open the SharePoint-Verwaltungsshell. For information about using this tool, see  [Administering Service Applications Using the SharePoint 2010 Management Shell.](http://msdn.microsoft.com/library/ff64855-7377-4e4a-b3a9-b620c9047076.aspx)
    
  
5. Geben Sie an der Eingabeaufforderung den folgenden Befehl ein:
    
  ```
  New-SPEnterpriseSearchSecurityTrimmer -SearchApplication "Search Service Application"
-typeName "CustomSecurityTrimmerSample.ClassName, CustomSecurityTrimmerSample, 
Version=1.0.0.0, Culture=neutral, PublicKeyToken=token" -RulePath "xmldoc://*"
  ```


Ersetzen Sie in den Befehl  _ClassName_ entweder mit **CustomSecurityPreTrimmer** oder **CustomSecurityPostTrimmer** und _token_ durch das öffentliche Schlüsseltoken für die Datei CustomSecurityTrimmerSample.dll. Sie müssen alle Post-trimmern eine Durchforstungsregel, _"xmldoc://*"_zuordnen. aber dieser Schritt ist optional für Trimmer vor.
    
> **HINWEIS**
> Wenn Sie mehrere Front-End-Webserver vorhanden sind, müssen Sie den Security Trimmer auf allen Front-End-Webservern in der Farm im globalen Assemblycache bereitstellen.
6. Stellen Sie sicher, dass mit den folgenden PowerShell-Cmdlets der Security Trimmer registriert ist.
    
  ```
  
$searchApp = Get-SPEnterpriseSearchServiceApplication
$searchApp | Get-SPEnterpriseSearchSecurityTrimmer
  ```


Die Einschränkung aus Sicherheitsgründen muss in den Ergebnissen sichtbar sein.
    
  

### So starten Sie den SharePoint-suchhostcontroller neu


- Sie können den Suchdienst neu starten, durch Eingabe des folgenden PowerShell-Cmdlets.
    
  ```
  
  net restart sphostcontrollerservice

  ```


## Zusätzliche Ressourcen
<a name="bk_sectrimmer_addlresources"> </a>


-  [Benutzerdefinierte sicherheitskürzung für die Suche in SharePoint Server 2013](custom-security-trimming-for-search-in-sharepoint-server-2013.md)
    
  
-  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx)
    
  
-  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx)
    
  
-  [Microsoft.Office.Server.Search.Query.PluggableAccessCheckException](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.PluggableAccessCheckException.aspx)
    
  
