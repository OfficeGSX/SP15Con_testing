---
title: Anspruchsbasierte Identität in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 32b6af2a-72f3-4302-a6af-5f00143cbf67
---


# Anspruchsbasierte Identität in SharePoint 2013
Informationen Sie zu den Grundlagen der anspruchsbasierten identitätsarchitektur in SharePoint 2013.
## Anspruchsbasierte Authentifizierung

Die anspruchsbasierte Authentifizierung ermöglicht Systemen und Anwendungen die Authentifizierung eines Benutzers, ohne mehr persönliche Informationen (wie Sozialversicherungsnummer und Geburtsdatum) als nötig von ihm zu erfragen. Beispiele für die anspruchsbasierte Authentifizierung sind etwa, wenn jemand behauptet, über 18 Jahre alt zu sein oder der Marketinggruppe eines Unternehmens anzugehören. Ein externes System (vertrauende Seite) muss nur der Authentifizierungsstelle vertrauen, die diese Ansprüche überprüfen kann, um die Authentifizierung des Benutzers für bestimmte Funktionen zuzulassen.
  
    
    

### Ansprüche: Eine Reihe von Informationen zu einem Thema

Am einfachsten lassen sich Ansprüche als ein Satz von Informationen über ein bestimmtes Subjekt betrachten. Bei diesem Subjekt handelt es sich meist um eine Person, es könnte jedoch auch eine Anwendung, ein Computer oder etwas anderes sein. Wenn eine Identität im Netzwerk übertragen wird, wird sie durch eine Art Token (auch als Sicherheitstoken bezeichnet) dargestellt.
  
    
    
Ein Anspruch ist eine Information zu einem Thema, die bestätigt ein Anspruchsanbieters zu diesem Thema. Es ist eine Anweisung zu einem Thema (beispielsweise Name), die erfolgt nach einem Betreff über sich selbst oder einer anderen Thema. Sie können eine Forderung als Identitätsinformationen, wie e-Mail-Adresse, Name, Alter oder die Mitgliedschaft in der sales-Rolle auszudrücken vorstellen. Es ist ein eindeutiger Bezeichner, der einen bestimmten Benutzer, eine Anwendung, Computer oder sonstige Entität darstellt. Es ermöglicht den Zugriff auf mehrere Ressourcen, wie Anwendungen und Netzwerkressourcen, ohne Anmeldeinformationen mehrfach eingeben dieser Entität. Darüber hinaus können Ressourcen, um Anfragen aus einer Entität zu überprüfen. Mehrere Ansprüche, dass eine Anwendung empfängt, wissen Sie mehr über Ihre Benutzer.
  
    
    
Ein Anspruch wird mit einem oder mehreren Werte versehen und anschließend in Sicherheitstoken gepackt, die von einem Sicherheitstokendienst (Security Token Service, STS) ausgegeben werden.
  
    
    
Das Wort Anspruch wird aufgrund der Übermittlungsmethode anstelle des BegriffsAttribute verwendet, der mehr im Kontext der Unternehmensverzeichnisse gängig ist. In diesem Modell werden Benutzerattribute nicht von einer Anwendung in einem Verzeichnis nachgeschlagen. Vielmehr übermittelt der Benutzer Ansprüche an die Anwendung. Jeder Anspruch wird von einem Aussteller erhoben, und Sie vertrauen dem Anspruch nur so weit, wie Sie dem Aussteller vertrauen. Beispielsweise vertrauen Sie einem Anspruch, der vom Domänencontroller des Unternehmens gestellt wird, mehr als einem Anspruch des Benutzers. Die Anspruchs-API verfügt über eine Ausstellereigenschaft, über die Sie herausfinden können, wer den Anspruch gestellt hat.
  
    
    

### Token: Informationen über eine Identität

Ein Token ist ein Satz von Bytes, der Informationen über eine Identität darstellt. Diese Informationen bestehen aus einem oder mehreren Ansprüchen, die jeweils Informationen über das Subjekt enthalten, für das dieses Token gilt. Die Ansprüche in einem Token enthalten üblicherweise Informationen wie den Namen des Benutzers, der es vorweist. Darüber hinaus können unterschiedliche andere Informationen enthalten sein - Ansprüche sind nicht beschränkt auf den Namen eines Subjekts und müssen diesen auch nicht enthalten. Wie das Wort Ansprüche nahe legt, akzeptiert eine Anwendung, die ein Token empfängt, nicht automatisch die darin enthaltenen Informationen. Stattdessen wird ein empfangenes Token in der Regel auf irgendeine Art überprüft, bevor eine Anwendung von darin enthaltenen Ansprüchen Gebrauch macht.
  
    
    
Das Schlüsselkonzept liegt darin, dass ein Anspruch nicht nur ein eindeutiger Bezeichner ist, der die Ressource, die Anwendung oder den Benutzer identifiziert. Vielmehr wird die Ressource, die Anwendung oder der Benutzer durch einen Satz von Ansprüchen (d. h. Werten) beschrieben. Die Ansprüche werden auch zur Autorisierung des Zugriffs verwendet.
  
    
    

## Zusätzliche Ressourcen
<a name="SP15_RoleInheritance_AdditionalResources"> </a>


-  [Authentifizierung, Autorisierung und Sicherheit in SharePoint 2013](authentication-authorization-and-security-in-sharepoint-2013.md)
    
  
-  [Eingehende Ansprüche: Anmelden bei SharePoint 2013](incoming-claims-signing-into-sharepoint-2013.md)
    
  
-  [Anspruchsanbieter in SharePoint 2013](claims-provider-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Erstellen ein anspruchsanbieters in SharePoint 2013](how-to-create-a-claims-provider-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: POST eines anspruchsanbieters in SharePoint 2013](how-to-deploy-a-claims-provider-in-sharepoint-2013.md)
    
  

