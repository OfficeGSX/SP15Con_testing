---
title: Anspruchsanbieter in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 5918d5b6-5fd6-4f41-9473-a15b1491d056
---


# Anspruchsanbieter in SharePoint 2013

## Anspruchsanbieter

Ein Anspruchsanbieter in SharePoint Server Probleme Ansprüche und Pakete Ansprüche in Sicherheitstokens, d. h., in dem Token des Benutzers. Wenn ein Benutzer bei SharePoint Server anmeldet, wird das Zugriffstoken des Benutzers überprüft und zur Anmeldung bei SharePoint verwendet.
  
    
    
Ein Anspruchsanbieter in SharePoint hat zwei Funktionen: Erweiterung und Auswahl.
  
    
    

> **HINWEIS**
> Weitere Informationen zum Erstellen eines Anspruchsanbieters finden Sie unter  [Vorgehensweise: Erstellen ein anspruchsanbieters in SharePoint 2013](how-to-create-a-claims-provider-in-sharepoint-2013.md).
  
    
    


### Anspruchserweiterung

In der Erweiterungsfunktion erweitert ein Anspruchsanbieter ein Benutzertoken während der Anmeldung mit Ansprüchen. Durch die Anspruchserweiterung kann eine Anwendung zusätzliche Ansprüche in das Token des Benutzers aufnehmen. Beispielsweise kann bei der Windows-basierten Anmeldung der Active Directory-Verzeichnisdienst alle Sicherheitsgruppen eines Benutzers in das Windows-Token des Benutzers aufnehmen. Bei der anspruchsbasierten Anmeldung kann eine CRM-Anwendung (Customer Relationship Management, Kundenbeziehungsmanagement) Rollen aus einer CRM-Datenbank erweitern. Ressourcen können für diese Ansprüche autorisiert werden, indem diese Ansprüche in das Token des Benutzers eingeschlossen werden. Das heißt, mit diesen Ansprüchen wird bestimmt, ob ein bestimmter Benutzer Zugriff auf bestimmte Ressourcen hat.
  
    
    

### Anspruchsauswahl

In der Auswahlfunktion bietet ein Anspruchsanbieter die Funktionalitäten Auflistung, Auflösung, Suche und benutzerfreundliche Anzeige von Ansprüchen bei der Personenauswahl. Durch die Anspruchsauswahl kann eine Anwendung Ansprüche in der Personenauswahl anzeigen, z. B. beim Konfigurieren der Sicherheit einer SharePoint-Website oder eines SharePoint-Diensts.
  
    
    

## Szenarien für die Verwendung von Anspruchsanbietern

Anspruchsanbieter werden verwendet, um verschiedene Szenarien zu lösen. Es folgen einige Beispiele für Szenarien, zum Lösen Anspruchsanbieter verwenden können.
  
    
    

### Auflisten, Auflösen und Suchen

In SharePoint Server sind integrierte Ansprüche zu Liste aktivieren Anbieter auflösen und suchen für integrierte Authentifizierungsanbieter. Beispiele für integrierte Authentifizierungsanbieter sind Windows Active Directory, formularbasierte Authentifizierung und vertrauenswürdigen Security Assertion Markup Language (SAML) tokenherausgebern - d. h., Sicherheitstokendienst (Security Token Service, STS).
  
    
    
Für einen vertrauenswürdigen SAML-tokenherausgeber bietet SharePoint Server oder in den keine. Wenn ein Benutzer einen Wert eingibt, löst SharePoint Server immer sie. Dies bedeutet, dass bei Eingabe von adam@contoso.comdie Personenauswahl den Wert annimmt. Dies ist, da es keine Industriestandard ist, die gibt an, wie ein STS aufgelöst wird, Suche implementiert oder listet Forderung Werte.
  
    
    
Benutzer können den integrierten Anspruchsanbieter zum Implementieren benutzerdefinierter Features für Suche, Namensauflösung und Auflistung überschreiben. Dies ist sehr nützlich in Szenarien wie der Verwendung eines vertrauenswürdigen Herausgebers von SAML-Token.
  
    
    

### Authentifizierte Benutzer oder "Alle Benutzer"-Ansprüche

In SharePoint Server sind einige bestimmte integrierte Anspruchsanbietern, die Implementierung von Unterstützung für Konzepte wie authentifizierten Benutzernzu ermöglichen. Dies ist auch bekannt als ein Anspruch Alle Benutzer . In diesem Szenario können Sie alle Benutzer aus einer bestimmten Authentifizierungsanbieter Zugriffsrechte.
  
    
    

> **HINWEIS**
> Bei einem Authentifizierungsanbieter kann es sich um Windows Active Directory, formularbasierte Authentifizierung oder einen vertrauenswürdigen Herausgeber von SAML-Token (d. h. einen STS) handeln.
  
    
    

In SharePoint Server ist auch ein Anspruchsanbieters für Systeme, das einige interner Ansprüche, die von einem taxonomiedienst verwendete hinzufügt. Es wird beispielsweise Identität und Application Pool Farmkonto.
  
    
    

### Hinzufügen von Ansprüchen zu einem ursprünglichen Token

Einige der integrierten Anspruchsanbieter werden auch verwendet, um die Ansprüche aus dem ursprünglichen "Token" hinzuzufügen. Das Wort "Token" wird in Anführungszeichen gesetzt, da einige Authentifizierungsanbieter, z. B. die formularbasierte Authentifizierung (d. h. ASP.NET-Mitgliedschafts- und Rollenanbieter), kein echtes Token bereitstellen. In diesem Fall können Sie dieses Token aus einer konzeptionellen Perspektive betrachten.
  
    
    
Möglicherweise müssen dem ursprünglichen Anspruchstoken des Benutzers zusätzliche Ansprüche hinzugefügt werden. In diesem Szenario kann ein Anspruchsanbieter erforderlich sein. Beispielsweise kann ein Anspruchsanbieter erforderlich sein, um dem ursprünglichen Anspruchstoken des Benutzers SAP-Rollen hinzuzufügen.
  
    
    

### Identität nicht vom ursprünglichen Token

Nehmen Sie ein Szenario an, in dem das System einige spezifische Anforderungen für die Personenauswahl und Anspruchstoken aufweist. In diesem Szenario kennen Sie die Identität des Benutzers basierend auf der Microsoft .NET Passport Unique ID (PUID) und dem ursprünglichen Benutzer. Die Informationen über den Benutzer stammen jedoch nicht aus dem ursprünglichen Benutzertoken, sondern aus Ihrem benutzerdefinierten Active Directory. Sie verwenden auch einige zusätzliche Active Directory-Gruppen, denen der Benutzer angehören kann, die nicht im ursprünglichen Benutzertoken (von Windows Live ID ausgegeben) enthalten sind. In diesem Szenario können Sie einen Anspruchsanbieter für Ihre systemspezifischen Anforderungen erstellen.
  
    
    

## Zusätzliche Ressourcen
<a name="bk_addresources"> </a>


-  [Anspruchsbasierte Identität in SharePoint 2013](claims-based-identity-in-sharepoint-2013.md)
    
  
-  [Eingehende Ansprüche: Anmelden bei SharePoint 2013](incoming-claims-signing-into-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Erstellen ein anspruchsanbieters in SharePoint 2013](how-to-create-a-claims-provider-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: POST eines anspruchsanbieters in SharePoint 2013](how-to-deploy-a-claims-provider-in-sharepoint-2013.md)
    
  

