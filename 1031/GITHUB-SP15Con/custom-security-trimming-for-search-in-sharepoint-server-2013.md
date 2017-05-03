---
title: Benutzerdefinierte sicherheitskürzung für die Suche in SharePoint Server 2013
ms.prod: SHAREPOINT
ms.assetid: fbbf0cc4-e135-426a-9996-34eb954dbd5a
---



# Benutzerdefinierte sicherheitskürzung für die Suche in SharePoint Server 2013
Erfahren Sie mehr über die zwei Arten von benutzerdefinierten Security Trimmer Schnittstellen, **ISecurityTrimmerPre** und **ISecurityTrimmerPost**, und die Schritte, die Sie ausführen müssen, um ein benutzerdefinierter Security Trimmer zu erstellen.
Zum Zeitpunkt der Abfrage führt Suche in SharePoint 2013 sicherheitskürzung von Suchergebnissen, die auf die Identität des Benutzers, senden Sie die Abfrage, mithilfe der Sicherheitsinformationen aus der Durchforstungskomponente basieren.
  
    
    

Sie müssen bestimmte Szenarien, jedoch in denen die integrierte Sicherheit verkürzen Ergebnisse für Ihren Anforderungen nicht ausreichend. In solchen Szenarien müssen Sie benutzerdefinierte sicherheitskürzung implementieren. Suche in SharePoint 2013 bietet Unterstützung für benutzerdefinierte sicherheitskürzung durch die  [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) Benutzeroberflächen, [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) Benutzeroberflächen und [ISecurityTrimmer2](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmer2.aspx) Benutzeroberflächen (veraltet) im Namespace [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) .
 **Hinweis:** Benutzerdefinierte Pre Rasentrimmer unterstützt nicht Windows SIDs in Zugriffssteuerungslisten, nur Ansprüche. Wenn der SID Ansprüche, die Typen von einer benutzerdefinierten Pre Trimmer zurückgegeben werden, werden die resultierende ACE-Einträge in den Index als Anspruch, nicht als SID codiert werden. Sie daher nicht Windows-Benutzeridentitäten übereinstimmen, die auf SIDs basieren.
  
    
    


## Implementieren der Schnittstellen für benutzerdefinierte sicherheitskürzung
<a name="Implementing_the_interfaces"> </a>

Die Benutzeroberfläche **ISecurityTrimmerPre** führt vor dem Zuschneiden oder vor dem Abfrage Auswertung, wo die Suchabfrage portiert ist Sicherheitsinformationen hinzufügen, bevor die Suchabfrage an den Suchindex verglichen wird. Die Benutzeroberfläche **ISecurityTrimmerPost** führt nach der verkürzen, oder nach der Abfrage Auswertung, wo die Suchergebnisse gelöscht werden, bevor sie an den Benutzer zurückgegeben werden.
  
    
    
Es empfiehlt sich die Verwendung von vor dem Zuschneiden für Leistung und allgemeine Richtigkeit; vor dem Zuschneiden verhindert Datenverlust für Einschränkung Daten und die Anzahl der Zugriffe Instanzen. Nach der Rasentrimmer können in Fällen verwendet werden, in dem die sicherheitskürzung am genauesten mit Abfrage Filter dargestellt werden kann. Es ist Dokumente einer müssen abwesend filtern beispielsweise je nach der lokalen Zeit des Benutzers, der die Abfrage, wie z. B. offizielle üblichen Geschäftszeiten nur ausgegeben.
  
    
    

### Implementieren der ISecurityTrimmerPre-Schnittstelle

Um ein benutzerdefinierter Security Pre Trimmer für Suchergebnisse zu erstellen, müssen Sie eine Komponente erstellen, die die **ISecurityTrimmerPre** Schnittstelle implementiert.
  
    
    
Die Benutzeroberfläche **ISecurityTrimmerPre** enthält zwei Methoden, mit denen Sie implementieren müssen: [Initialize(NameValueCollection, SearchServiceApplication)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.Initialize.aspx) und [AddAccess(Boolean, Claims)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.AddAccess.aspx) .
  
    
    

#### "Initialize"-Methode

Die **Initialize** -Methode wird ausgeführt, wenn die Vorabversion Trimmer Sicherheit in Worker-Prozess geladen wird, und nicht erneut ausgeführt wird, bis der Worker-Prozess wieder verwendet wird. Zwei Parameter werden an die Methode übergeben:
  
    
    

-  _staticProperties_: ein  [NameValueCollection](https://msdn.microsoft.com/library/System.Collections.Specialized.NameValueCollection.aspx) Objekt, enthält die Konfigurationseigenschaften, die für die Sicherheit Trimmer angegeben werden, wenn sie mit der Suchdienstanwendung registriert wird.
    
  
-  _searchApplication_: ein  [SearchServiceApplication](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.aspx) -Objekt, das die Suchdienstanwendung darstellt.
    
  

#### AddAccess-Methode

Die Methode **AddAccess** wird einmal ausgeführt, pro Pre Trimmer, für jede eingehende Abfrage, bevor die Abfrage ausgewertet wird.
  
    
    
In dieser Methode werden zwei Parameter übergeben:
  
    
    

-  _sessionProperties_: ein **[T:System.Collections.Generic.IDictionary<String,Object>]** -Objekt, das die Eigenschaften der Abfrage enthält.
    
  
-  _userIdentity_: ein  [IIdentity](https://msdn.microsoft.com/library/System.Security.Principal.IIdentity.aspx) -Objekt, das die Identität des Benutzers enthält.
    
  

### Implementieren der ISecurityTrimmerPost-Schnittstelle

Um ein benutzerdefinierter Security nach der Trimmer für Suchergebnisse zu erstellen, müssen Sie eine Komponente erstellen, die die **ISecurityTrimmerPost** Schnittstelle implementiert.
  
    
    
Die Benutzeroberfläche **ISecurityTrimmerPost** enthält zwei Methoden, mit denen Sie implementieren müssen: [Initialize(NameValueCollection, SearchServiceApplication)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.Initialize.aspx) und **CheckAccess(IList<String>, IList<String>, IDictionary<String, Object>, IIdentity)**.
  
    
    

#### "Initialize"-Methode

Die **Initialize** Methode wird ausgeführt, wenn Security Trimmer in der Worker-Prozess geladen wird, und nicht erneut ausgeführt wird, bis der Worker-Prozess wieder verwendet wird. Zwei Parameter werden an die Methode übergeben: und
  
    
    

-  _staticProperties_: ein  [NameValueCollection](https://msdn.microsoft.com/library/System.Collections.Specialized.NameValueCollection.aspx) Objekt, enthält die Konfigurationseigenschaften für die Sicherheit Trimmer angegeben werden, wenn sie mit der Suchdienstanwendung registriert wird.
    
  
-  _searchApplication_: ein  [SearchServiceApplication](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.aspx) -Objekt, das die Suchdienstanwendung darstellt.
    
  

#### "CheckAccess"-Methode

Die **CheckAccess** -Methode wird einmal pro nach der Trimmer, für jede Abfrage Ergebnis festlegen, ausgeführt, nachdem die Abfrage ausgewertet wird.
  
    
    
In dieser Methode werden vier Parameter übergeben:
  
    
    

-  _documentUrls_: ein  [IList<T>](http://msdn2.microsoft.com/DE-DE/library/5y536ey6) -Objekt, das die URLs für jedes Inhaltselement in den Suchergebnissen enthalten, die die Durchforstungsregel entsprechen.
    
  
-  _documentAcls_: ein  [IList<T>](http://msdn2.microsoft.com/DE-DE/library/5y536ey6) Objekt, das Element ACLs für jedes Inhaltselement, deren Zugriff ist von der Security Trimmer Implementierung Verbindungsstatus, enthält.
    
  
-  _sessionProperties_: ein  [IDictionary<TKey, TValue>](http://msdn2.microsoft.com/DE-DE/library/s4ys34ea) Objekt, enthält die Eigenschaftensammlung vorübergehende.
    
  
-  _userIdentity_: ein  [IIdentity](https://msdn.microsoft.com/library/System.Security.Principal.IIdentity.aspx) Objekt aus der Implementierung die Identität des Benutzers abrufen können.
    
  
Die Methode **CheckAccess** gibt ein [BitArray](https://msdn.microsoft.com/library/System.Collections.BitArray.aspx) -Objekt, das ein Array von Werten **true** oder **false**, eine für jede Inhaltselement URL in das Objekt **IList** darstellt, der als erster Parameter der Methode übergeben wird. Verarbeitung Komponente die Abfrage verwendet diese Werte, um die nach der sicherheitskürzung der Ergebnisse durchzuführen. Wenn der Matrix-Wert für ein besonderes Inhaltselement **true**ist, ist das Element in die zurückgegebenen Ergebnisse enthalten; Wenn der Arraywert **false**ist, wird das Element entfernt.
  
    
    
Beim Implementieren der **CheckAccess** -Methode können Sie zweierlei Informationen für jedes Element verwenden, um festzustellen, ob **true** oder **false** für das Element zurückgeben: die Identität des Benutzers, der die Abfrage und die URL für das Inhaltselement übermittelt. Alternativ können Sie auch benutzerdefinierte ACL Dokumentinformationen aus der Verbinder an die Methode **CheckAccess** übergeben.
  
    
    

#### Abrufen von der Identität des Benutzers für Ihre Trimmer Sicherheit

Sie können die Identität des Benutzers abrufen, indem Sie den Zugriff auf den Thread des aktuellen Tilgungsanteile, wie im folgenden Beispiel gezeigt.
  
    
    

```cs

IIdentity userIdentity = System.Threading.Thread.CurrentPrincipal.Identity;
```

Außerdem müssen Sie die folgende Namespacedirektive einfügen.
  
    
    



```cs
using System.Security.Principal;
```

Sie können die Identität des Benutzers auch aus dem **passedUserIdentity**-Parameter der **CheckAccess**-Methode abrufen.
  
    
    

#### Dokument ACLs aus den Verbinder zu Ihrer Sicherheit Rasentrimmer übergeben

Ein Verbinder, ist wie der Name sagt, zwischen SharePoint 2013 und externen Systems, die externen Daten befindet. Wenn Sie mit den benutzerdefinierten Verbinder arbeiten, können Sie direkt auf den nach der Trimmer ACL-Informationen des Dokuments übergeben, mithilfe der Eigenschaft **docaclmeta** Dokument. Der Verbinder konfiguriert und nach der Rasentrimmer das Format und die Interpretation des Felds besitzen, können Sie kostenlose zur gemeinsamen Nutzung von benutzerdefinierten Daten zu übergeben.
  
    
    
Die Zeichenfolgen in **docaclmeta** gespeichert, von der Verbinder werden in den Parameter _documentAcls_ bereitstellen, wenn die **CheckAccess** -Methode der benutzerdefinierten Security Trimmer aufgerufen wird. Das normale Dokument ACLs in der Eigenschaft **docacl** von grundlegende sicherheitskürzung verarbeitet werden und sind für die benutzerdefinierter Security Trimmer nicht sichtbar. Die Eigenschaft **docaclmeta** verfügt ebenso nicht Auswirkung auf grundlegende sicherheitskürzung.
  
    
    

#### Nach der Rasentrimmer und deren Einfluss auf die Anzahl der Einschränkung für Sicherheit Rasentrimmer

Bei der Arbeit mit nach der Rasentrimmer ist zu beachten, dass es zwei Arten von Ergebnistabellen gibt: **RelevantResults** und der **RefinementResults**. Nach der Rasentrimmer, die nur auf dem Ergebnis Zugriffe in den **RelevantResults** angewendet werden. Daher können vorhanden sein, dass die Einschränkungen auf dem neuen gekürzte Zugriffe beziehen, und die Anzahl der **RefinementResults** möglicherweise größer als oder gleich der **RelevantResults**. Sie können dieses Verhalten auf zwei Arten beheben:
  
    
    

- Ausschließen von der vertraulichen Einschränkungen aus der Einschränkungsbereich in Standard-Webpart, damit es werden keine Informationen über die Einschränkungen offengelegt werden.
    
  
- Verwenden Sie ein benutzerdefiniertes Webpart, um die Ergebnisse oder Einschränkungen angezeigt, wenn Sie nach der Rasentrimmer verwenden, damit die **RefinementResults** elegante in Fällen ausgeblendet werden kann, wenn die Anzahl der **RefinementResults** die Anzahl der **RelevantResults** übersteigt.
    
  

### Abrufen von Eigenschaften für Ihre Sicherheit Trimmer einzelne Konfiguration

Sie können eine einzelne Konfigurationseigenschaft zugreifen, mit dem Namen der Eigenschaft, der beim Erstellen der Sicherheit nach der Trimmer registriert wurde angegeben wurde. Mit dem folgende Code wird beispielsweise der Wert für eine Konfigurationseigenschaft mit dem Namen **CheckLimit**abgerufen.
  
    
    

```cs
public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{
    if (staticProperties["CheckLimitProperty"] != null)
    {
         intCheckLimit = Convert.ToInt32(staticProperties["CheckLimitProperty"]);
    }
}
```


## Bereitstellen der benutzerdefinierten Komponente mit dem Security Trimmer
<a name="Deploying_the_trimmer"> </a>

Nachdem Sie die benutzerdefinierter Security Trimmer erstellt haben, müssen Sie es zum globalen Assemblycache auf einem beliebigen Server in der Abfrage Rolle bereitstellen. Schritt 2 im  [Vorgehensweise: verwenden ein benutzerdefinierten Security Trimmer für SharePoint Server-Suchergebnisse](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md) beschreibt die für die Bereitstellung von benutzerdefinierten Security Trimmer zum globalen Assemblycache.
  
    
    

## Registrieren des benutzerdefinierten Security Trimmers
<a name="Registering_the_trimmer"> </a>

Für nach der Rasentrimmer müssen Sie eine benutzerdefinierte Security Trimmer Registrierung mit einem bestimmten Suchdienstanwendung und Durchforstungsregel zuordnen; für die Vorabversion Rasentrimmer ist dies optional.
  
    
    
Verwenden Sie das Cmdlet **SPEnterpriseSearchSecurityTrimmer**, der die SharePoint-Verwaltungsshell, um ein benutzerdefinierter Security Trimmer zu registrieren.
  
    
    
Die folgende Tabelle beschreibt die Parameter, die das Cmdlet verwendet.
  
    
    

**Tabelle 1. Parameter, die vom SPEnterpriseSearchSecurityTrimmer-Cmdlet verwendet werden**


|**Parameter**|**Beschreibung**|
|:-----|:-----|
| _SearchApplication_ <br/> |Erforderlich ist. Der Name der Suchdienstanwendung, beispielsweise "Suchdienstanwendung". <br/> |
| _typeName_ <br/> |Erforderlich. Der starke Name der Assembly für den benutzerdefinierten Security Trimmer. <br/> |
| _RulePath_ <br/> |Benötigt nach der Rasentrimmer; für die Vorabversion Rasentrimmer optional. Die Durchforstungsregel für die Sicherheit Trimmer. <br/> <br/> **HINWEIS**<br/> Es empfiehlt sich, eine Durchforstungsregel pro Inhaltsquelle verwenden.          |
| _id_ <br/> |Erforderlich. Der Bezeichner (ID) des Security Trimmers. Dieser Wert ist eindeutig. Wird ein Security Trimmer mit einem Bezeichner registriert, der bereits für einen anderen Security Trimmer registriert ist, wird die Registrierung des ersten Trimmers mit der Registrierung für den zweiten Trimmer überschrieben. <br/> |
| _properties_ <br/> |Optional. Die Name/Wert-Paare, die die Konfigurationseigenschaften angeben. Erforderliches Format:  `Name1~Value1~Name2~Value~…` <br/> |
   
Ein Beispiel für einen einfachen Befehl für die Registrierung ein benutzerdefinierter Security Trimmer und einer Stichprobe, die angibt, die Konfigurationseigenschaften, finden Sie unter  [Vorgehensweise: verwenden ein benutzerdefinierten Security Trimmer für SharePoint Server-Suchergebnisse](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md).
  
    
    

## Weitere Ressourcen
<a name="bk_sectrimmer_addlresources"> </a>


-  [Vorgehensweise: verwenden ein benutzerdefinierten Security Trimmer für SharePoint Server-Suchergebnisse](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)
    
  
-  [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx)
    
  
-  [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx)
    
  
-  [Übersicht über Business Connectivity Services in SharePoint 2013](http://technet.microsoft.com/en-us/library/ee661740.aspx)
    
  
