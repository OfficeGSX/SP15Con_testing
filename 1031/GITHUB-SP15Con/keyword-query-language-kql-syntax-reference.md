---
title: Syntaxreferenz für die Keyword Query Language (KQL)
ms.prod: SHAREPOINT
ms.assetid: d8489f59-522f-433c-b9c1-69e597be51c7
---



# Syntaxreferenz für die Keyword Query Language (KQL)
Erfahren Sie, wie Sie KQL-Abfragen für Suche in SharePoint 2013 erstellen können. In dieser Syntaxreferenz werden die KQL-Abfrageelemente und die Vorgehensweise zum Verwenden der Eigenschaftseinschränkungen und -operatoren in KQL-Abfragen erläutert.
## Elemente einer KQL-Abfrage
<a name="SP15KQL_elements"> </a>

Eine KQL-Abfrage besteht aus einem oder mehreren der folgenden Elemente: 
  
    
    

- Freitext-Schlüsselwörter, -Wörter oder -Ausdrücke 
    
  
- Eigenschaftseinschränkungen 
    
  
Sie können KQL-Abfrageelemente mit einem oder mehreren der verfügbaren Operatoren kombinieren.
  
    
    
Wenn die KQL-Abfrage nur Operatoren enthält oder leer ist, ist sie ungültig. Bei KQL-Abfragen muss die Groß- und Kleinschreibung nicht berücksichtigt werden, bei Operatoren jedoch schon (Großbuchstaben).
  
    
    

> **HINWEIS**
> Die Längenbeschränkung einer KQL-Abfrage variiert je nachdem, wie Sie sie erstellt haben. Wenn Sie die KQL-Abfrage erstellen, indem Sie das SharePoint-Standardsuch-Front-End verwenden, beträgt die Längenbeschränkung 2.048 Zeichen. Mit dem Abfrageobjektmodell programmgesteuert erstellte KQL-Abfragen verfügen standardmäßig über eine Längenbeschränkung von 4.096 Zeichen. Sie können diese Beschränkung mit der  [MaxKeywordQueryTextLength](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.MaxKeywordQueryTextLength.aspx) -Eigenschaft oder der [DiscoveryMaxKeywordQueryTextLength](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.DiscoveryMaxKeywordQueryTextLength.aspx) -Eigenschaft (für eDiscovery) auf bis zu 20.480 Zeichen erhöhen.
  
    
    


## Erstellen von Freitext-Abfragen mit KQL
<a name="SP15KQL_constructing_freetext_queries"> </a>

Wenn Sie die KQL-Abfrage mit Freitext-Ausdrücken erstellen, gleicht Suche in SharePoint 2013 die Ergebnisse mit den von Ihnen für die Abfrage ausgewählten Begriffen basierend auf den im Volltextindex gespeicherten Begriffen ab. Dazu gehören auch verwaltete Eigenschaftswerte, bei denen  [FullTextQueriable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.FullTextQueriable.aspx) auf **true** gesetzt wurde.
  
    
    
Bei Freitext-KQL-Abfragen muss die Groß- und Kleinschreibung nicht berücksichtigt werden, Operatoren müssen jedoch groß geschrieben werden. Sie können KQL-Abfragen erstellen, indem Sie eine oder mehrere der folgenden Freitext-Abfragen verwenden:
  
    
    

- Ein **word** (enthält ein oder mehrere Zeichen ohne Leerzeichen oder Interpunktion)
    
  
- Ein **phrase** (enthält zwei oder mehrere Wörter, die durch Leerzeichen getrennt sind; die Wörter müssen jedoch in doppelte Anführungszeichen gesetzt werden)
    
  
Um komplexe Abfragen zu erstellen, können Sie mehrere Freitext-Ausdrücke mit KQL-Abfrageoperatoren kombinieren. Wenn mehrere Freitext-Ausdrücke mit darin enthaltenen Operatoren vorhanden sind, verhält sich die Abfrage wie beim Verwenden des **AND**-Operators.
  
    
    

### Verwenden von Wörtern in der Freitext-KQL-Abfrage

Wenn Sie Wörter in einer Freitext-KQL-Abfrage verwenden, gibt Suche in SharePoint 2013 die Ergebnisse basierend auf den exakten Übereinstimmungen Ihrer Wörter mit den im Volltextindex gespeicherten Begriffen zurück. Sie können auch nur den ersten Teil eines Worts verwenden, indem Sie den Platzhalter-Operator (*) verwenden, um den Präfix-Abgleich zu aktivieren. Im Präfix-Abgleich gleicht Suche in SharePoint 2013 die Ergebnisse mit den Begriffen ab, die das Wort gefolgt von Null oder mehr Zeichen enthalten.
  
    
    
Folgende KQL-Abfragen geben beispielsweise Inhaltselemente zurück, die die Begriffe „Verbund" und „Suche" enthalten: 
  
    
    
 `federated search`
  
    
    
 `federat* search`
  
    
    
 `search fed*`
  
    
    
KQL-Abfragen unterstützen keinen Suffix-Abgleich.
  
    
    

### Verwenden von Ausdrücken in der Freitext-KQL-Abfrage

Wenn Sie Ausdrücke in einer Freitext-KQL-Abfrage verwenden, gibt Suche in SharePoint 2013 nur die Elemente zurück, in denen sich die Wörter aus Ihrem Ausdruck direkt nebeneinander befinden. Um einen Ausdruck in einer KQL-Abfrage anzugeben, müssen Sie doppelte Anführungszeichen verwenden. 
  
    
    
KQL-Abfragen unterstützen keinen Suffix-Abgleich, Sie können also in Freitext-Abfragen keinen Platzhalter-Operator vor einem Ausdruck verwenden. Sie können den Platzhalter-Operator jedoch nach einem Ausdruck verwenden.
  
    
    

## Eigenschaftseinschränkung-Abfragen in KQL
<a name="kql_property_restriction_queries"> </a>

Mit KQL können Sie Abfragen erstellen, die Eigenschaftseinschränkungen verwenden, um den Fokus der Abfrage einzugrenzen, damit die Ergebnisse nur auf einer bestimmten Bedingung basierend abgeglichen werden.
  
    
    

### Angeben von Eigenschaftseinschränkungen

Eine einfache Eigenschaftseinschränkung besteht aus folgenden Teilen:
  
    
    
 `<Property Name><Property Operator><Property Value>`
  
    
    
In Tabelle 1 werden einige Beispiele gültiger Eigenschaftseinschränkungssyntaxen in KQL-Abfragen aufgeführt.
  
    
    

**Tabelle 1: Gültige Eigenschaftseinschränkungssyntax**


|**Syntax**|**gibt zurück**|
|:-----|:-----|
| `author:"John Smith"` <br/> |Gibt von John Smith verfasste Inhaltselemente zurück.  <br/> |
| `filetype:docx` <br/> |Gibt Microsoft Word-Dokumente zurück.  <br/> |
| `filename:budget.xlsx` <br/> |Gibt Inhaltselemente mit dem Dateinamen  `budget.xlsx` zurück. <br/> |
   
Die Eigenschaftseinschränkung darf kein Leerzeichen zwischen Eigenschaftsname, Eigenschaftsoperator und Eigenschaftswert enthalten, andernfalls wird die Eigenschaftseinschränkung als Freitext-Abfrage behandelt. Die Länge einer Eigenschaftseinschränkung ist auf 2.048 Zeichen begrenzt. 
  
    
    
In den folgenden Beispielen bewirkt das Leerzeichen, dass die Abfrage Inhaltselemente zurückgibt, die die Begriffe „Autor" und „John Smith" enthalten, statt von John Smith verfasste Inhaltselemente zurückzugeben:
  
    
    
 `author: "John Smith"`
  
    
    
 `author :"John Smith"`
  
    
    
 `author : "John Smith"`
  
    
    
Anders ausgedrückt, die vorherigen Eigenschaftseinschränkungen entsprechen folgendem Sachverhalt:
  
    
    
 `author "John Smith"`
  
    
    

### Angeben von Eigenschaftsnamen für Eigenschaftseinschränkungen

Sie müssen einen gültigen verwalteten Eigenschaftsnamen für die Eigenschaftseinschränkung angeben. Standardmäßig enthält Suche in SharePoint 2013 verschiedene verwaltete Eigenschaften für Dokumente.
  
    
    
Um eine Eigenschaftseinschränkung für einen durchforsteten Eigenschaftswert anzugeben, müssen Sie die durchforstete Eigenschaft zunächst der verwalteten Eigenschaft zuordnen. Weitere Informationen dazu finden Sie unter **Verwaltete und durchforstete Eigenschaften** unter [Planen der Endbenutzer-Sucherfahrung](http://technet.microsoft.com/de-de/library/cc263089.aspx). 
  
    
    
Die verwaltete Eigenschaft muss  [Queryable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Queryable.aspx) sein, damit Sie in einem Dokument nach der verwalteten Eigenschaft suchen können. Außerdem kann die verwaltete Eigenschaft [Retrievable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Retrievable.aspx) sein, damit sie abgerufen werden kann. Die verwaltete Eigenschaft muss jedoch nicht [Retrievable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Retrievable.aspx) sein, damit Eigenschaftssuchen ausgeführt werden können.
  
    
    

### In Eigenschaftseinschränkungen unterstützte Eigenschaftsoperatoren

Suche in SharePoint 2013 unterstützt verschiedene Eigenschaftsoperatoren für Eigenschaftseinschränkungen, siehe Tabelle 2. 
  
    
    

**Tabelle 2: Gültige Eigenschaftsoperatoren für Eigenschaftseinschränkungen**


|**Operator**|**Beschreibung**|**Unterstützter verwalteter Eigenschaftstyp**|
|:-----|:-----|:-----|
|:  <br/> |Gibt Ergebnisse zurück, in denen der in der Eigenschaftseinschränkung angegebene Wert dem in der Eigenschaftsspeicher-Datenbank gespeicherten Eigenschaftswert entspricht, oder mit einzelnen Begriffen des im Volltextindex gespeicherten Eigenschaftswerts übereinstimmt.  <br/> | [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|=  <br/> |Gibt Suchergebnisse zurück, in denen der Eigenschaftswert dem in der Eigenschaftseinschränkung angegebenen Wert entspricht.  <br/> <br/> **HINWEIS**<br/> Es wird nicht empfohlen, den **=**-Operator bei genauen Übereinstimmungen mit einem Sternchen ( *****) zu kombinieren.           | [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|<  <br/> |Gibt Ergebnisse zurück, in denen der Eigenschaftswert geringer als der in der Eigenschaftseinschränkung angegebene Wert ist.  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|>  <br/> |Gibt Suchergebnisse zurück, in denen der Eigenschaftswert größer als der in der Eigenschaftseinschränkung angegeben Wert ist.  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|<=  <br/> |Gibt Suchergebnisse zurück, in denen der Eigenschaftswert kleiner gleich dem in der Eigenschaftseinschränkung angegebenen Wert ist.  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|>=  <br/> |Gibt Suchergebnisse zurück, in denen der Eigenschaftswert größer gleich dem in der Eigenschaftseinschränkung angegebenen Wert ist.  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|<>  <br/> |Gibt Suchergebnisse zurück, in denen der Eigenschaftswert nicht dem in der Eigenschaftseinschränkung angegebenen Wert entspricht.  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|..  <br/> |Gibt Suchergebnisse zurück, in denen der Eigenschaftswert innerhalb des in der Eigenschaftseinschränkung angegebenen Bereichs liegt.  <br/> Der Bereich A..B stellt beispielsweise eine Gruppe von Werten zwischen A und B, in der A und B enthalten sind. Bei Datumsbereichen liegt der Bereich dann zwischen Anfang Tag A und Ende Tag B.  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
   

### Angeben von Eigenschaftswerten

Sie müssen einen Eigenschaftswert angeben, der ein gültiger Datentyp für den verwalteten Eigenschaftstyp ist. In Tabelle 3 sind diese Typenzuordnungen aufgelistet.
  
    
    

**Tabelle 3: Gültige Datentypenzuordnungen für verwaltete Eigenschaftstypen**


|**Verwalteter Typ**|**Datentyp**|
|:-----|:-----|
| [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/> | [String](https://msdn.microsoft.com/library/System.String.aspx) <br/> |
| [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/> | [Int64](https://msdn.microsoft.com/library/System.Int64.aspx) <br/> |
| [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> | [System.Double](https://msdn.microsoft.com/library/System.Double.aspx) <br/> |
| [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/> | [Decimal](https://msdn.microsoft.com/library/System.Decimal.aspx) <br/> |
| [DateTime()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/> | [DateTime](https://msdn.microsoft.com/library/System.DateTime.aspx) <br/> |
| [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> | [Boolean](https://msdn.microsoft.com/library/System.Boolean.aspx) <br/> |
   

#### Texteigenschaftstypen

Bei Texteigenschaftswerten hängt das Übereinstimmungsverhalten davon ab, ob die Eigenschaft im Volltextindex oder im Suchindex gespeichert ist. 
  
    
    

#### Eigenschaftswerte im Volltextindex

Eigenschaftswerte werden im Volltextindex gespeichert, wenn die **FullTextQueriable**-Eigenschaft für eine verwaltete Eigenschaft auf **true**wurde. Sie können diese Einstellung nur für Zeichenfolgeneigenschaften konfigurieren. In der Abfrage angegebene Eigenschaftswerte werden mit den einzelnen Begriffen abgeglichen, die im Volltextindex gespeichert sind. Verwenden Sie die  [NoWordBreaker](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.NoWordBreaker.aspx) -Eigenschaft, um anzugeben, ob sie mit dem gesamten Eigenschaftswert abgeglichen werden sollen.
  
    
    
Wenn Sie beispielsweise nach einem von Paul Shakespear verfassten Inhaltselement suchen, gibt die folgende KQL-Abfrage übereinstimmende Ergebnisse zurück:
  
    
    
 `author:Shakespear`
  
    
    
 `author:Paul`
  
    
    
Der Präfix-Abgleich wird auch unterstützt. Sie können den Platzhalter-Operator (*) verwenden, das ist jedoch nicht erforderlich, wenn Sie einzelne Wörter angeben. Um erneut auf das vorherige Beispiel zurückzukommen: folgende KQL-Abfrage gibt von Paul Shakespear verfasste Inhaltselemente als Übereinstimmungen zurück: 
  
    
    
 `author:Shakesp*`
  
    
    
Wenn Sie für den Eigenschaftswert einen Ausdruck angeben, müssen übereinstimmende Ergebnisse den angegebenen Ausdruck im Eigenschaftswert enthalten, der im Volltextindex gespeichert ist. Das folgende Abfrage-Beispiel gibt Inhaltselemente zurück, die den Text „Erweiterte Suche" im Titel enthalten, wie z. B. „Erweiterte Suche XML", „Weitere Informationen zur erweiterten Suche Webpart" usw.:
  
    
    
 `title:"Advanced Search"`
  
    
    
Der Präfix-Abgleich wird auch für in den Eigenschaftswerten angegebene Ausdrücke unterstützt, dafür müssen Sie jedoch den Platzhalter-Operator (*) in der Abfrage verwenden. Außerdem wird er nur am Ende des Ausdrucks unterstützt, siehe folgendes Beispiel:
  
    
    
 `title:"Advanced Sear*"`
  
    
    
Folgende Abfragen geben nicht das erwartete Ergebnis zurück:
  
    
    
 `title:"Advan* Search"`
  
    
    
 `title:"Advanced Sear"`
  
    
    

#### Numerische Werte für Eigenschaften

Numerische Eigenschaftswerte, die die verwalteten Typen **Integer**, **Double** und **Decimal** enthalten, wird die Eigenschaftseinschränkung mit dem gesamten Eigenschaftswert abgeglichen.
  
    
    

### Datums- oder Zeitwerte für Eigenschaften

KQL stellt den Datentyp **datetime** für das Datum und die Zeit bereit. Folgende mit der ISO 8601 kompatible datetime-Formate werden in Abfragen unterstützt:
  
    
    

- JJJJ-MM-TT
    
  
- JJJJ-MM-TTThh:mm:ss
    
  
- JJJJ-MM-TTThh:mm:ssZ
    
  
- JJJJ-MM-TTThh:mm:ssfrZ
    
  
In diesen **datetime**-Formaten:
  
    
    

- gibt  _YYYY_ eine vierziffrige Jahresangabe an.
    
    > **HINWEIS**
      > Es werden nur vierziffrige Jahresangaben unterstützt. 
-  _MM_ gibt einen zweiziffrigen Monat an. Z. B. 01 = Januar.
    
  
-  _DD_ gibt einen zweiziffrigen Tag des Monats an (01-31).
    
  
-  _T_ gibt den Buchstaben „T" an.
    
  
-  _hh_ gibt eine zweiziffrige Stundenangabe an (00-23).
    
  
-  _mm_ gibt eine zweiziffrige Minutenangabe an (00-59).
    
  
-  _ss_ gibt eine zweiziffrige Sekundenangabe an (00-59).
    
  
-  _fr_ gibt optional eine Zehntelsekundenangabe an, ss; zwischen 1 und 7, die als Stelle **.** nach dem Komma folgt. Z. B. 27.09.2012T11:57:34.1234567.
    
  
Alle Datums-/Zeitwerte müssen gemäß der UTC (Coordinated Universal Time) angegeben werden, auch als GMT (Greenwich Mean Time) Zeitzone bekannt. Der UTC-Zeitzonenbezeichner (nachfolgendes „Z") ist optional.
  
    
    

#### Von KQL unterstützte relevante Datumsintervalle

Mit KQL können Sie Suchabfragen erstellen, die die relative „Tag"-Bereichsabfrage mit reservierten Schlüsselwörtern unterstützt, siehe Tabelle 4. Verwenden Sie doppelte Anführungszeichen ("") für Datumsintervalle, zwischen denen sich Leerzeichen befinden.
  
    
    


|**Name des Datumsintervalls**|**Beschreibung**|
|:-----|:-----|
|today  <br/> |Stellt den Zeitraum vom Anfang des aktuellen Tags bis zum Ende des aktuellen Tags dar.  <br/> |
|yesterday  <br/> |Stellt den Zeitraum vom Anfang des Tags bis zum Ende des Tags, der vor dem aktuellen Tag liegt, dar.  <br/> |
|this week  <br/> |Stellt den Zeitraum zwischen dem Anfang der aktuellen Woche und dem Ende der aktuellen Woche dar. Die Kultur, in der der Abfragetext formuliert wurde, wird dabei berücksichtigt, um den ersten Tag der Woche zu ermitteln.  <br/> |
|this month  <br/> |Stellt den Zeitraum vom Anfang des aktuellen Monats bis zum Ende des aktuellen Monats dar.  <br/> |
|last month  <br/> |Stellt den gesamten Monat dar, der vor dem aktuellen Monat liegt.  <br/> |
|this year  <br/> |Stellt den Zeitraum vom Anfang des aktuellen Jahres bis zum Ende des aktuellen Jahres dar.  <br/> |
|last year  <br/> |Stellt das gesamte Jahr dar, das vor dem aktuellen Jahr liegt.  <br/> |
   

### Verwenden von mehreren Eigenschaftseinschränkungen innerhalb einer KQL-Abfrage

Suche in SharePoint 2013 unterstützt die Verwendung von mehreren Eigenschaftseinschränkungen innerhalb der gleichen KQL-Abfrage. Sie können entweder die gleiche Eigenschaft für mehrere Eigenschaftseinschränkungen verwenden oder für jede Eigenschaftseinschränkung eine andere Eigenschaft nutzen. 
  
    
    
Wenn Sie mehrere Instanzen der gleichen Eigenschaftseinschränkung verwenden, basieren die Übereinstimmung auf einer Verbindung der Eigenschaftseinschränkungen in der KQL-Abfrage. Die Übereinstimmungen enthalten von John Smith oder Jane Smith verfasste Inhaltselemente, siehe folgendes Beispiel:
  
    
    
 `author:"John Smith" author:"Jane Smith"`
  
    
    
Diese Funktion entspricht der Verwendung des booleschen Operators **OR**, siehe folgendes Beispiel:
  
    
    
 `author:"John Smith" OR author:"Jane Smith"`
  
    
    
Wenn Sie verschiedene Eigenschaftseinschränkungen verwenden, basieren die Übereinstimmungen auf einer Schnittmenge der Eigenschaftseinschränkungen in der KQL-Abfrage, siehe folgendes Beispiel:
  
    
    
 `author:"John Smith" filetype:docx`
  
    
    
Übereinstimmungen würden von John Smith verfasste Microsoft Word-Dokumente enthalten. Diese Funktion entspricht der Verwendung des booleschen Operators **AND**, siehe folgendes Beispiel:
  
    
    
 `author:"John Smith" AND filetype:docx`
  
    
    

## KQL-Operatoren für komplexe Abfragen
<a name="kql_operators"> </a>

Die KQL-Syntax enthält mehrere Operatoren, die Sie zum Erstellen komplexer Abfragen verwenden können. 
  
    
    

### Boolesche Operatoren

Sie können boolesche Operatoren verwenden, um die Suche zu erweitern oder einzugrenzen. Sie können boolesche Operatoren in KQL-Abfragen mit Freitext-Ausdrücken und Eigenschaftseinschränkungen verwenden. In Tabelle 5 sind die unterstützten booleschen Operatoren aufgelistet.
  
    
    

**Tabelle 5: In KQL unterstützte boolesche Operatoren**


|**Operator**|**Beschreibung**|
|:-----|:-----|
|**AND** <br/> |Gibt Suchergebnisse zurück, die alle Freitext-Ausdrücke oder Eigenschaftseinschränkungen enthält, die mit dem Operator **AND** angegeben wurden. Sie müssen vor und nach dem **AND**-Operator einen gültigen Freitext-Ausdruck und/oder eine gültige Eigenschaftseinschränkung angeben. Diese Funktion entspricht der Verwendung des „+"-Zeichens.  <br/> |
|**NOT** <br/> |Gibt Suchergebnisse zurück, in denen die angegebenen Freitext-Ausdrücke oder Eigenschaftseinschränkungen nicht enthalten sind. Sie müssen nach dem Operator **NOT** einen gültigen Freitext-Ausdruck und/oder eine gültige Eigenschaftseinschränkung angeben. Diese Funktion entspricht der Verwendung des „-"-Zeichens. <br/> |
|**OR** <br/> |Gibt Suchergebnisse zurück, die eine oder mehrere der angegebenen Freitext-Ausdrücke oder Eigenschaftseinschränkungen enthält. Sie müssen vor und nach dem **OR**-Operator einen gültigen Freitext-Ausdruck und/oder eine gültige Eigenschaftseinschränkung angeben.  <br/> |
   

  
    
    

### Näherungsoperatoren

Sie können Näherungsoperatoren verwenden, um die Ergebnisse abzugleichen, bei denen die Suchbegriffe nah beieinander auftauchen. Näherungsoperatoren können nur mit Freitext-Ausdrücken verwendet werden; mit Eigenschaftseinschränkungen in KQL-Abfragen werden sie nicht unterstützt. Es gibt zwei Näherungsoperatoren: **NEAR** und **ONEAR**.
  
    
    

#### NEAR-Operator

Der **NEAR**-Operator gleicht die Ergebnisse ab, bei denen die Suchbegriffe nah beieinander auftauchen, ohne dabei die Reihenfolge der Begriffe einhalten zu müssen. Die Syntax für **NEAR** lautet wie folgt:
  
    
    
 `<expression> NEAR(n=4) <expression>`
  
    
    
Dabei stellt  _n_ einen optionalen Parameter dar, der den maximalen Abstand zwischen den Begriffen angibt. Der Wert _n_ ist eine ganze Zahl >= 0 mit einem Standardwert von **8**.
  
    
    
Der Parameter  _n_ kann als `n=v` angegeben werden, dabei stellt _v_ den Wert dar, oder gekürzt _v_; z. B.  `NEAR(4)`, dabei beträgt  _v_ 4.
  
    
    
Beispiel:
  
    
    
 `"acquisition" NEAR "debt"`
  
    
    
Diese Abfrage gleicht Elemente ab, bei denen die Begriffe „Akquirierung" und „Forderung" im gleichen Element auftreten. Dabei wird eine Instanz „Akquirierung" gefolgt von bis zu acht weiteren Begriffen und anschließend folgt der Begriff „Forderung"; oder umgekehrt. Die Reihenfolge der Begriffe ist für den Abgleich nicht relevant.
  
    
    
Wenn ein geringerer Abstand zwischen den Begriffen liegen soll, können Sie ihn angeben. Mit der folgenden Abfrage werden Elemente abgeglichen, bei denen die Begriffe „Akquirierung" und „Forderung" innerhalb des gleichen Elements auftreten, dabei beträgt der Abstand zwischen den Begriffen maximal 3. Die Reihenfolge der Begriffe hat keine Auswirkungen auf den Abgleich.
  
    
    
 `"acquisition" NEAR(n=3) "debt"`
  
    
    

> **HINWEIS**
> In SharePoint 2013 behält der **NEAR**-Operator die Sortierung der Token nicht mehr bei. Außerdem empfängt der **NEAR**-Operator einen optionalen Parameter, der den maximalen Abstand zwischen den Token angibt. Der Standardwert beträgt **8**. Wenn Sie die vorherige Verhaltensweise verwenden müssen, verwenden Sie stattdessen **ONEAR**. 
  
    
    


#### ONEAR-Operator

Der **ONEAR**-Operator gleicht die Ergebnisse ab, bei denen die angegebenen Suchbegriffe nah beieinander auftreten und behält die Reihenfolge der Begriffe bei. Die Syntax für **ONEAR** lautet wie folgt, dabei ist _n_ ein optionaler Parameter, der den maximalen Abstand zwischen den Begriffen angibt. Der Wert _n_ ist eine ganze Zahl >= 0 mit einem Standardwert **8**.
  
    
    
 `<expression> ONEAR(n=4) <expression>`
  
    
    
Der Parameter  _n_ kann als `n=v` angegeben werden, dabei stellt _v_ den Wert dar, oder gekürzt _v_; z. B.  `ONEAR(4)`, dabei beträgt  _v_ 4.
  
    
    
Die folgende Abfrage gleicht beispielsweise Elemente ab, in denen die Begriffe „Akquirierung" und „Forderung" innerhalb des gleichen Elements auftauchen, dabei folgen auf eine Instanz „Akquirierung" bis zu acht weitere Begriffe und anschließend eine Instanz des Begriffs „Forderung". Die Reihenfolge der Begriffe **muss** übereinstimmen, damit ein Element zurückgegeben werden kann:
  
    
    
 `"acquisition" ONEAR "debt"`
  
    
    
Wenn ein kleinerer Abstand zwischen den Begriffen liegen soll, können Sie ihn wie folgt angeben. Diese Abfrage gleicht Elemente ab, in denen die Begriffe „Akquirierung" und „Forderung" innerhalb des gleichen Elements auftauchen, dabei beträgt der maximale Abstand zwischen den Begriffen 3. Die Reihenfolge der Begriffe **muss** übereinstimmen, damit ein Element zurückgegeben werden kann:
  
    
    
 `"acquisition" ONEAR(n=3) "debt"`
  
    
    

### Synonym-Operatoren

Sie können den Operator **WORDS** verwenden, um anzugeben, dass die Begriffe in der Abfrage Synonyme sind und zurückgegebene Ergebnisse mit einem der angegebenen Begriffe übereinstimmen sollten. Sie können den Operator **WORDS** nur mit Freitext-Ausdrücken verwenden; mit Eigenschaftseinschränkungen in KQL-Abfragen wird er nicht unterstützt.
  
    
    
Die folgende Beispielabfrage gleicht Ergebnisse ab, die den Begriff „TV" oder den Begriff „Fernsehen" enthalten. Dieses Abgleichverhalten entspricht der Verwendung folgender Abfrage:
  
    
    
 `WORDS(TV, Television)`
  
    
    
 `TV OR Television`
  
    
    
Diese Abfragen unterscheiden sich in der Ergebnisrangfolge. Wenn Sie den Operator **WORDS** verwenden, werden die Begriffe „TV" und „Fernsehen" als Synonyme behandelt, statt als einzelne Begriffe. Deshalb ist die Rangfolge der Instanzen der Begriffe so, als wäre es derselbe Begriff. Ein Inhaltselement, das eine Instanz des Begriffs „Fernsehen" und fünf Instanzen des Begriffs „TV" enthält, erhält die gleiche Rangfolge, als wären sechs Instanzen des Begriffs „TV" im Inhaltselement enthalten.
  
    
    

### Platzhalter-Operator

Sie können den Platzhalter-Operator  das Sternchen („ ***** ") - verwenden, um den Präfix-Abgleich zu aktivieren. Sie können einen Teil des Anfangs eines Worts in einer Abfrage gefolgt vom Platzhalter-Operator wie folgt angeben. Bei dieser Abfrage würden Ergebnisse abgeglichen, die Begriffe enthalten, die mit „Serv" gefolgt von Null oder weiteren Zeichen wie z. B. Serve, Server, Service usw.beginnen:
  
    
    
 `serv*`
  
    
    

### Inklusions- und Ausschluss-Operatoren

Sie können angeben, ob die zurückgegebenen Ergebnisse Inhalte enthalten oder ausschließen sollen, die mit dem im Freitext-Ausdruck oder in der Eigenschaftseinschränkung angegebenen Wert übereinstimmen, indem Sie Inklusions- oder Ausschluss-Operatoren verwenden, siehe Tabelle 6.
  
    
    

**Tabelle 6: Operatoren zum Einschließen und Ausschließen von Inhalten in Ergebnissen**


|**Name**|**Operator**|**Verhalten**|
|:-----|:-----|:-----|
|Inklusion  <br/> |„ **+** " <br/> |Schließt Inhalte mit Werten ein, die mit der Inklusion übereinstimmen.  <br/> Wenn kein Zeichen angegeben wurde, verhält sich die Abfrage standardmäßig so. Diese Funktion entspricht der Verwendung des **AND**-Operators.  <br/> |
|Ausschluss  <br/> |„ **-** " <br/> |Schließt Inhalte mit Werten ein, die mit der Exklusion übereinstimmen. Diese Funktion entspricht der Verwendung des Operators **NOT**.  <br/> |
   

### Dynamischer Rangfolge-Operator

Sie können den Operator **XRANK** verwenden, um den dynamischen Rang der Elemente basierend auf bestimmten Vorkommnissen von Begriffen im _match expression_ verstärken, ohne die Abfrage dahingehend zu verändern, welche Elemente mit der Abfrage übereinstimmen. Ein **XRANK**-Ausdruck enthält eine Komponente, die abgeglichen werden muss, der  _match expression_ und eine oder mehrere Komponenten, die nur in der dynamischen Ranfolge relevant sind und den _rank expression_. Es muss mindestens **einer** der Parameter angegeben werden, abgesehen von _n_, damit ein **XRANK**-Ausdruck gültig ist.
  
    
    
 _Match expressions_ kann ein beliebiger gültiger KQL-Ausdruck sein, einschließlich verschachtelter **XRANK**-Ausdrücke.  _Rank expressions_ kann ein beliebiger gültiger KQL-Ausdruck ohne **XRANK**-Ausdrücke sein. Wenn Ihre KQL-Abfragen über mehrere **XRANK**-Operatoren verfügt, wird der endgültige dynamische Rangfolgewert als Summe der Verstärkungen aller **XRANK**-Operatoren berechnet.
  
    
    

> **HINWEIS**
> Verwenden Sie Klammern, um die Reihenfolge der Berechnungen für die KQL-Abfragen explizit anzugeben, die über mehr als einen **XRANK**-Operator auf der gleichen Ebene verfügen. 
  
    
    

Sie können den **XRANK**-Operator in folgender Syntax verwenden:
  
    
    
 `<match expression> XRANK(cb=100, rb=0.4, pb=0.4, avgb=0.4, stdb=0.4, nb=0.4, n=200) <rank expression>`
  
    
    
Die Berechnung der dynamischen Rangfolge des **XRANK**-Operators basiert auf dieser Formel:
  
    
    

  
    
    
![Formel für XRANK-Operator](images/XRANKFormula.gif)
  
    
    
In Tabelle 7 sind die verfügbaren Grundparameter für den **XRANK**-Operator aufgelistet.
  
    
    

**Tabelle 7: XRANK-Operatorparameter**


|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _n_ <br/> | _<integer_value>_ <br/> |Gibt die Anzahl der Ergebnisse an, aus denen die Statistik berechnet werden soll.  <br/> Dieser Parameter hat keine Auswirkungen auf die Anzahl der Ergebnisse, die durch die dynamische Rangfolge hinzu kommen; es handelt sich dabei lediglich um eine Methode zum Ausschließen irrelevanter Elemente aus den Berechnungen der Statistik.  <br/> Standardwert: **0**. Ein Nullwert trägt die Semantik *aller Dokumente*  . <br/> |
| _nb_ <br/> | _<float_value>_ <br/> |Der  _nb_-Parameter verweist auf die normale Verstärkung. Dieser Parameter gibt den Faktor an, der mit dem Produkt der Abweichung und der Durchschnittsbewertung der Rangwerte der Ergebnisgruppe multipliziert wird.  <br/>  _f_ in der XRANK-Formel. <br/> |
   
In der Regel ist die normale Verstärkung ( _nb_) der einzige Parameter, der geändert wird. Dieser Parameter stellt das benötigte Steuerelement bereit, um ein bestimmtes Element höher oder niedriger zu stufen, ohne dabei die Standardabweichung zu berücksichtigen. 
  
    
    
Außerdem sind folgende erweiterte Parameter verfügbar. In der Regel werden sie jedoch nicht verwendet.
  
    
    

**Tabelle 8: Erweiterte Parameter für XRANK**


|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _cb_ <br/> | _<float_value>_ <br/> |Der  _cb_-Parameter verweist auf die konstante Verstärkung.  <br/> Standardwert: **0**. <br/>  _a_ in der XRANK-Formel. <br/> |
| _stdb_ <br/> | _<float_value>_ <br/> |Der  _stdb_-Parameter verweist auf die Verstärkung der Standardabweichung.  <br/> Standardwert: **0**. <br/>  _e_ in der XRANK-Formel. <br/> |
| _avgb_ <br/> | _<float_value>_ <br/> |Der  _avgb_-Parameter verweist auf die durchschnittliche Verstärkung.  <br/> Standardwert: **0**. <br/>  _d_ in der XRANK-Formel. <br/> |
| _rb_ <br/> | _<float_value>_ <br/> |Der  _rb_-Parameter verweist auf die Bereichsverstärkung. Dieser Faktor wird mit dem Bereich der Rangfolgewerte in der Ergebnisgruppe multipliziert.  <br/> Standardwert: **0**. <br/>  _b_ in der XRANK-Formel. <br/> |
| _pb_ <br/> | _<float_value>_ <br/> |Der  _pb_-Parameter verweist auf die Prozentwertverstärkung. Dieser Faktor wird im Vergleich zum Mindestwert des Korpus mit der Rangfolge des Elements multipliziert.  <br/> Standardwert: **0**. <br/>  _c_ in der XRANK-Formel. <br/> |
   

#### Beispiele

 **Beispiel 1.** Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält. Der Ausdruck erhöht die dynamische Rangfolge der Elemente mit einer konstanten Verstärkung von 100 für Elemente, die zusätzlich den Begriff „Vollblutpferd" enthalten.
  
    
    
 `(cat OR dog) XRANK(cb=100) thoroughbred`
  
    
    
 **Beispiel 2.** Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält. Der Ausdruck erhöht die dynamische Rangfolge der Elemente mit einer normalen Verstärkung von 1,5 für Elemente, die zusätzlich den Begriff „Vollblutpferd" enthalten.
  
    
    
 `(cat OR dog) XRANK(nb=1.5) thoroughbred`
  
    
    
 **Beispiel 3.** Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält. Der Ausdruck erhöht die dynamische Rangfolge der Elemente mit einer konstanten Verstärkung von 100 und einer normalen Verstärkung von 1,5, für Elemente, die zusätzlich den Begriff „Vollblutpferd" enthalten.
  
    
    
 `(cat OR dog) XRANK(cb=100, nb=1.5) thoroughbred`
  
    
    
 **Beispiel 4.** Der folgende Ausdruck gleicht alle Elemente ab, die den Begriff „Tiere" enthalten und die dynamische Rangfolge wie folgt verstärken:
  
    
    

- Die dynamische Rangfolge von Elementen, die den Begriff „Hunde" enthalten, werden um 100 Punkte verstärkt.
    
  
- Die dynamische Rangfolge der Elemente, die den Begriff „Katzen" enthalten, werden um 200 Punkte verstärkt.
    
  
- Die dynamische Rangfolge der Elemente, die die Begriffe „Hunde" und „Katzen" enthalten, werden um 300 Punkte verstärkt.
    
  
 `(animals XRANK(cb=100) dogs) XRANK(cb=200) cats`
  
    
    

### Klammer

Sie können verschiedene Teile einer Stichwortabfrage kombinieren, indem Sie eine öffnende „ **(** " und schließende Klammer „ **)** " verwenden. Zu jeder öffnenden Klammer „ **(** " muss eine schließende Klammer „ **)** " vorhanden sein. Ein Leerzeichen vor und nach einer Klammer hat keine Auswirkungen auf die Abfrage.
  
    
    

## Zusätzliche Ressourcen
<a name="SP15KQL_addlresources"> </a>


-  [Erstellen von Suchabfragen in SharePoint 2013](building-search-queries-in-sharepoint-2013.md)
    
  
