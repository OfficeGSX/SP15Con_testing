---
title: Einschränkung von Abfragen in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: ec31782e-1bc5-4dc3-8df7-c29cd5f7f05c
---



# Einschränkung von Abfragen in SharePoint 2013
Hier erfahren Sie, wie Sie Features zum Einschränken von Abfragen von SharePoint Server 2013 in Abfragen und Ergebnissen einsetzen können.
Features zum Einschränken von Abfragen geben dem Benutzer relevante Einschränkungsoptionen für seine Abfragen an die Hand. Der Benutzer kann damit einen Drilldown in die Suchergebnisse ausführen, indem er für die Ergebnisse berechnete Einschränkungsdaten verwendet. Einschränkungsdaten werden durch die Indexkomponente basierend auf der Aggregation von Statistiken für verwaltete Eigenschaften für alle Ergebnisse einer Suchabfrage berechnet.
  
    
    

Die Abfrageeinschränkung wird typischerweise für Metadaten verwendet, die den indizierten Elementen zugeordnet sind, z. B. das Erstellungsdatum, der Autor und Personennamen, die in dem Element vorhanden sind. Mit den entsprechenden Einschränkungsoptionen können Sie eine Abfrage so präzisieren, dass nur Elemente dargestellt werden, die innerhalb einer bestimmten Zeitspanne erstellt wurden oder einen bestimmten Dateityp aufweisen.
## Verwenden von Einschränkungen im Abfrageobjektmodell
<a name="SP15_Using_refiners"> </a>

An der Abfrageeinschränkung sind zwei Abfragen beteiligt:
  
    
    

1. Sie können anfordern, dass eine Reihe von Einschränkungen in den Suchergebnissen zurückgegeben wird, indem Sie  [eine Einschränkungsspezifikation](#SP15_Adding_refiners) zu der Abfrage des Benutzers hinzufügen. Eine Einschränkungsspezifikation ist die Eingabe für die [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) -Eigenschaft. Diese Abfrage wird gegen den Suchindex ausgeführt. Die Suchergebnisse bestehen aus relevanten Ergebnissen und Einschränkungsdaten.
    
  
2. Sie können die Einschränkungsdaten verwenden, um einen Drilldown in die Suchergebnisse auszuführen, indem Sie  [eine eingeschränkte Abfrage erstellen](#SP15_Creating_refined_query). Fügen Sie die  [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) -Eigenschaft zu der Abfrage hinzu, sodass die endgültigen Suchergebnisse sowohl den Anforderungen des ursprünglichen Abfragetexts des Benutzers als auch der ausgewählten Einschränkungsoption aus den Einschränkungsdaten entspricht.
    
  
In den folgenden Abschnitten werden diese Schritte ausführlich beschrieben un Codebeispiele bereitgestellt.
  
    
    

## Hinzufügen von Einschränkungen zu der Abfrage des Benutzers mithilfe von Einschränkungsspezifikationen
<a name="SP15_Adding_refiners"> </a>

Sie können die angeforderten Abfrageeinschränkungen mithilfe der  [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) -Eigenschaft der [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.KeywordQuery.aspx) -Klasse angeben. Verwenden Sie die folgende Syntax, um die angeforderten Abfrageeinschränkungen anzugeben:
  
    
    
 `<refiner>[,<refiner>]*`
  
    
    

  
    
    
Jede  `refiner` weist das folgende Format auf:
  
    
    
 `<refiner-name>[(parameter=value[,parameter=value]*)]?`
  
    
    

  
    
    
Dabei gilt:
  
    
    

-  `<refiner-name>` ist der Name der verwalteten Eigenschaft, die der Einschränkung zugeordnet ist. Diese verwaltete Eigenschaft muss im Suchschema auf [Refinable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Refinable.aspx) oder [Sortable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Sortable.aspx) festgelegt sein.
    
  
- Die optionale Liste von  `parameter=value`-Paaren gibt die nicht standardmäßigen Konfigurationswerte für die benannte Einschränkung an. Wenn ein Parameter für eine Einschränkung nicht innerhalb der Klammern aufgeführt ist, erhalten Sie von der Suchschemakonfiguration die Standardeinstellung. In Tabelle 1 sind mögliche Werte für die  `parameter=value`-Paare aufgeführt.
    
  

> **HINWEIS**
> Wenn Sie Einschränkungen angeben, müssen Sie mindestens einen  `refiner-name` angeben, der eine verwaltete Eigenschaft ist.<br/> **Example**<br/> `Refiners = "FileType"`<br/> <br/> Sie können auch die erweiterte Syntax verwenden, um die Einschränkungseinstellungen anzupassen. <br/> **Example**<br/> `Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22),companies"`
  
    
    


**Tabelle 1: Liste von Parametern für Einschränkungen**


|**Parameter**|**Beschreibung**|
|:-----|:-----|
| _deephits_ <br/> |Überschreibt die standardmäßige Anzahl von Treffern, die als Grundlage für die Berechnung der Einschränkung verwendet wird. Wenn Einschränkungen erstellt werden, werden alle Ergebnisse für die Abfrage ausgewertet. Bei Verwendung dieses Parameters wird in der Regel die Suchleistung verbessert.  <br/> **Syntax**          `deephits=<integer value>` <br/> **Example**          `price(deephits=1000)` <br/> <br/> **HINWEIS**<br/> Diese Grenze gilt innerhalb jeder Indexpartition. Die tatsächliche Anzahl von Treffern, die ausgewertet werden, ist aufgrund der Aggregation über Suchpartitionen hinweg größer als dieser Wert.           |
| _discretize_ <br/> | Gibt benutzerdefinierte Intervalle (Einschränkungsbins) für numerische Einschränkungen an. <br/> **Syntax**          `discretize=manual/<threshold>/<threshold>[/<threshold>]*` <br/> **Example**          `write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22)` <br/>  Das `<threshold>`-Attribut gibt den Schwellenwert für jeden Einschränkungsbin an.  <br/>  Es gibt ein Intervall für alle Elemente unterhalb des ersten angegebenen Schwellenwerts, ein Intervall zwischen jedem aufeinanderfolgenden Schwellenwert, und ein Intervall für alle Elemente überhalb dem letzten Schwellenwert. <br/>  Für eine Einschränkung des Typs **DateTime** geben Sie den Schwellenwert gemäß einem der folgenden ISO 8601-kompatiblen Formate an: <br/>  _YYYY-MM-DD_ <br/>  _YYYY-MM-DDThh:mm:ss_ <br/>  _YYYY-MM-DDThh:mm:ss:Z_ <br/>  Die Formatwerte geben Folgendes an: <br/>  _YYYY_ - Vierziffrige Jahresangabe. Es werden nur vierziffrige Jahresangaben unterstützt. <br/>  _MM_ - Zweiziffriger Monat. 01 = Januar. <br/>  _DD_ - Zweiziffriger Tag des Monats (01 bis 31). <br/>  _T_ - Den Buchstaben „T". <br/>  _hh_ - Zweiziffrige Stundenangabe (00 bis 23). Die Angabe von A.M. oder P.M. ist nicht zulässig. <br/>  _mm_ - Zweiziffrige Minutenangabe (00 bis 59). <br/>  _ss_ - Zweiziffrige Sekundenangabe (00 bis 59). <br/> <br/> **WICHTIG**<br/>  Alle Datums-/Zeitwerte müssen gemäß der UTC (Coordinated Universal Time) angegeben werden, auch als GMT (Greenwich Mean Time) Zeitzone bekannt. Der UTC-Zeitzonenbezeichner (nachfolgendes „Z") ist optional.          |
| _sort_ <br/> | Definiert, wie die Bins innerhalb einer Zeichenfolgeneinschränkung sortiert werden sollen. <br/> **Syntax**          `sort=<property>/<direction>` <br/>  Die Attribute führen Folgendes aus: <br/>  _<property>_ - Gibt den Sortieralgorithmus an. Die folgenden Werte in Kleinbuchstaben sind gültig: <br/> **frequency** - Anordnung nach Vorkommen innerhalb der Bins. <br/> **name** - Anordnung nach Bezeichnungsname. <br/> **number** - Behandelt die Zeichenfolgen als numerisch und verwendet die numerische Sortierung. Dieser Wert kann hilfreich sein, um diskrete Werte anzugeben, beispielsweise, um eine numerische Sortierung für einen Wert auszuführen, der in einer verwalteten Eigenschaft des Typs **String** enthalten ist. <br/>  _<direction>_ - Gibt die Sortierrichtung an. Die folgenden Werte in Kleinbuchstaben sind gültig: <br/> **descending** <br/> **ascending** <br/> **Example**          `sort=name/ascending` <br/> **Default**: Häufigkeit/absteigend  <br/> |
| _filter_ <br/> | Definiert wie Bins innerhalb einer Einschränkung des Typs **String** gefiltert werden, bevor sie an den Client zurückgeben werden. <br/> **Syntax**          `filter=<bins>/<freq>/<prefix>[<levels>]` <br/>  Die Attribute führen Folgendes aus: <br/>  _<bins>_ - Gibt die maximale Anzahl zurückgegebener Bins an. <br/>  Verwenden Sie dieses Attribut nach der Sortierung der Bins innerhalb der Einschränkung, um nachfolgende Bins abzuschneiden. Bei bins=10 werden gemäß des angegebenen Sortieralgorithmus beispielsweise nur die ersten 10 Bins zurückgegeben. <br/>  _<freq>_ - Beschränkt die Anzahl zurückgegebener Bins. <br/>  Verwenden Sie dieses Attribut, um Bins zu entfernen, die nur selten vorkommen. _freq_=2 bedeutet beispielsweise, dass nur die Bins mit 2 oder mehr Mitgliedern zurückgegeben werden.  <br/>  _<prefix>_ - Gibt an, dass nur Bins mit einem Namen zurückgegeben werden, der mit dieser Präfix beginnt. <br/>  Das Platzhalterzeichen „*" gibt alle Namen zurück. <br/> **Example** <br/>  `filter=30/2/*` <br/>  _<levels>_ - Gibt die Taxonomieebenen an. <br/>  Verwenden Sie dieses Attribut, um eine hierarchische Einschränkungsausgabe basierend auf Taxonmieebenen zu filtern. Die Attribute müssen eine positive Ganzzahl _n_ sein. Der **default**-Wert ist  _n_=0. Bei  _n_> werden nur Einschränkungseingaben zurückgegeben, die weniger als  _n_ Trennsymbole für den Taxonomiepfad („/") enthalten. Bei _n_=0 wird keine Ebenenfilterung durchgeführt. Das Ebenentrennzeichen ist ein Schrägstrich („/").  <br/>  Beachten Sie die Leistungskosten bei der Verwendung eines großen Taxonomienavigators. Die Filterung wird als Teil der Ergebnisverarbeitung durchgeführt. <br/> <br/> **HINWEIS**<br/>  Dieser Parameter wendet eine anwendungsspezifische Filterung auf Ergebnisseite an. Dies unterscheidet sich von dem Cutoff-Parameter, der aus Leistungsgründen Einschränkungen in jeder Indexpartition anwendet.          |
| _cutoff_ <br/> | Schränkt die Daten ein, die für tiefe Zeichenfolgeneinschränkungen übertragen und verarbeitet werden müssen. Sie können die Einschränkungen so konfigurieren, dass nur die relevantesten Werte (Bins) zurückgegeben werden. <br/> <br/> **HINWEIS**<br/>  Diese Cutoff-Filterung wird innerhalb jeder Indexpartition durchgeführt. Dies unterscheidet sich von dem _filter_-Parameter, der nur eine Filterung auf Ergebnisseite durchführt. Sie können die beiden Parameter kombinieren.           **Syntax**          `cutoff=<frequency>/<minbins>/<maxbins>` <br/>  Die Attribute führen Folgendes aus: <br/>  _<maxbins>_ - Schränkt die Anzahl von Bins ein. <br/>  Dieser Parameter schränkt die Anzahl eindeutiger Werte (Bins) ein, die für eine Einschränkung zurückgegeben werden. Dies ist die bevorzugte Methode, um die Suchleistung zu steigern, wenn Zeichenfolgeneinschränkungen mit einer großen Anzahl von Bins zurückgegeben werden. „0" bedeutet, dass der im Suchschema angegebene Standardwert verwendet wird. <br/>  _<frequency>_ - Schränkt die Anzahl von Bins nach Häufigkeit ein. <br/>  Wenn die Anzahl der Vorkommnisse eines Einschränkungswerts in einem Resultset kleiner oder gleich dem Häufigkeitswert ist, wird der Einschränkungswert nicht zurückgegeben. „0" bedeutet, dass der Standardwert gemäß des Suchschemas verwendet wird. <br/>  _<minbins>_ - Gibt den maximalen Wert für den Häufigkeitsgrenzwert an. <br/>  Wenn die Anzahl eindeutiger Einschränkungswerte für die Abfrage niedriger als dieser Wert ist, wird keine Häufigkeitsabtrennung durchgeführt, und alle Einschränkungswerte werden von dieser Suchpartition zurückgegeben. Wenn eine Abtrennung nach Häufigkeit verwendet wird, kann dieser Parameter verwendet werden, um eine minimale Anzahl von eindeutigen Einschränkungswerten anzugeben, die zurückgegeben werden, unabhängig von der Anzahl von Vorkommnissen. Der Standarwert dieses Attributs ist „0"; dadurch wird angegeben, dass die Häufigkeitsabtrennung unabhängig von der Anzahl eindeutiger Navigatorwerte durchgeführt wird. „0" bedeutet, dass der im Indexschema angegebene Standardwert verwendet wird. <br/> <br/> **HINWEIS**<br/>  Seien Sie bei der Verwendung von _<frequency>_ und _<minbins>_ vorsichtig. Es wird empfohlen, dass Sie nur _<maxbins>_ verwenden.          |
   

### Beispiel: Hinzufügen von Einschränkungen
<a name="SP15_Example_adding_refiners"> </a>

Im folgenden CSOM-Beispiel wird gezeigt, wie drei Einschränkungen programmgesteuert angefordert werden: **FileType**, **Write** und **Companies**. **Write** stellt das Datum der letzten Änderung für das Element dar und verwendet die erweiterte Syntax, um Datums-/Uhrzeitbins mit fester Größe zurückzugeben.
  
    
    

```cs

using (var context = new ClientContext("http://<serverName>/<siteCollectionPath>"))
{
    var query = new KeywordQuery(context)
    {
        QueryText = "home",
        Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/2013-09-
            15/2013-09-21/2013-09-22),companies"
    };
    
    var executor = new SearchExecutor(context);
    var results = executor.ExecuteQuery(query);

    context.ExecuteQuery();

    ResultTable relevantResultsTable = results.Value[0];
    ResultTable refinerResultsTable = results.Value[1];          
    Console.WriteLine(refinerResultsTable.RowCount + " refinement options:");
    foreach (var refinementOption in refinerResultsTable.ResultRows)
    {
        Console.WriteLine("RefinerName: '{0}' RefinementName: '{1}'
            RefinementValue: '{2}' RefinementToken: '{3}' RefinementCount: '{4}'", 
            refinementOption["RefinerName"],
            refinementOption["RefinementName"],
            refinementOption["RefinementValue"],
            refinementOption["RefinementToken"],
            refinementOption["RefinementCount"]
        );
    }
}
```


## Verstehen der Einschränkungsdaten im Suchergebnis
<a name="SP15_Understanding_refinement_data"> </a>

Wenn Sie die Abfrageeinschränkung für eine verwaltete Eigenschaft in Ihrer Abfrage aktiviert haben, enthält das Abfrageergebnis Einschränkungsdaten, die in Einschränkungsbins unterteilt sind. Diese Daten befinden sich in der **RefinementResults**-Tabelle ( [RefinementResults](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KnownTableTypes.RefinementResults.aspx) ) innerhalb der [ResultTableCollection](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ResultTableCollection.aspx) . Ein Einschränkungsbin stellt einen bestimmten Wert oder Wertebereich für die verwaltete Eigenschaft dar. Die **RefinementResults**-Tabelle enthält eine Zeile pro Einschränkungsbin und enthält die Spalten, wie in Tabelle 2 angegeben.
  
    
    

**Tabelle 2: Für Einschränkungsbins zurückgegebene Daten**


|**Parameter**|**Beschreibung**|
|:-----|:-----|
|**RefinerName** <br/> |Der Name der Abfrageeinschränkung.  <br/> |
|**RefinementName** <br/> |Die Zeichenfolge, die das Einschränkungsbin darstellt. Diese Zeichenfolge wird in der Regel verwendet, wenn Einschränkungsoptionen für die Benutzer auf einer Suchergebnisseite präsentiert werden.  <br/> |
|**RefinementValue** <br/> |Eine implementierungsspezifische formatierte Zeichenfolge, die die Einschränkung darstellt. Diese Daten werden zum Debuggen zurückgegeben und sind normalerweise für den Client nicht erforderlich.  <br/> |
|**RefinementToken** <br/> |Eine Zeichenfolge, die das für **RefinerName** zu verwendende Einschränkungsbin darstellt, wenn Sie eine eingeschränkte Abfrage ausführen. <br/> |
|**RefinementCount** <br/> |Die Anzahl der Ergebnisse für dieses Einschränkungsbin. Diese Daten stellen die Anzahl von Elementen (einschließlich Duplikaten) im Suchergebnis mit einem Wert für die angegebene verwaltete Eigenschaft dar, der in dieses Einschränkungsbin fällt.  <br/> |
   

### Beispiel: Einschränkungsdaten
<a name="SP15_Example_refinement_data"> </a>

Tabelle 3 unten enthält zwei Zeilen mit Einschränkungsdaten. Die erste Zeile enthält Einschränkungsdaten für indizierte Elemente, wobei der Dateityp HTML ist. Die zweite Zeile enthält Einschränkungsdaten für indizierte Elemente, wobei der Zeitpunkt der letzten Bearbeitung zwischen 21.09.2013 und 22.09.2013 liegt.
  
    
    

**Tabelle 3: Format und Inhalt der Einschränkungsdaten**


|**RefinerName**|**RefinementName**|**RefinementValue**|**RefinementToken**|**RefinementCount**|
|:-----|:-----|:-----|:-----|:-----|
|FileType  <br/> |Html  <br/> |Html  <br/> |"ǂǂ68746d6c"  <br/> |50553  <br/> |
|Write  <br/> |Von 2013-09-21T00:00:00Z bis 2013-09-22T00:00:00Z  <br/> |Von 2013-09-21T00:00:00Z bis 2013-09-22T00:00:00Z  <br/> |range(2013-09-21T00:00:00Z, 2013-09-22T00:00:00Z)  <br/> |37  <br/> |
   

## Erstellen einer Einschränkungsabfrage
<a name="SP15_Creating_refined_query"> </a>

Ein Suchergebnis stellt Einschränkungsoptionen in der Form von Zeichenfolgenwerten oder Wertebereichen dar. Jeder Zeichenfolgenwert oder numerische Wertebereich wird als Einschränkungsbin bezeichnet, und jeder Einschränkungsbin weist einen zugewiesenen **RefinementToken**-Wert auf. Eine Einschränkungsoption ist einer verwalteten Eigenschaft zugeordnet, die vom **RefinerName**-Wert bereitgestellt wird.
  
    
    
Sowohl der Wert **RefinementToken** als auch der Wert **RefinerName** ist verkettet, sodass eine _refinement filter_-Zeichenfolge erstellt wird. Diese Zeichenfolge stellt einen Filter dar, der verwendet werden kann, um Suchergebnisse so einzuschränken, dass nur Elemente enthalten sind, bei denen eine verwaltete Eigenschaft einen Wert innerhalb eines Einschränkungsbin aufweist. Kurz ausgedrückt:
  
    
    
 `refinement filter = <RefinerName>:<RefinementToken>`
  
    
    
Sie können einen oder mehrere Einschränkungsfilter für eine eingeschränkte Abfrage angeben, indem Sie der  [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) -Eigenschaft der [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.aspx) -Klasse Einschränkungsfilter hinzufügen. Mit mehreren Einschränkungsfiltern können Sie einen Drilldown über mehrere Stufen in Suchergebnisse bereitstellen und eine Einschränkung auf Eigenschaften mit mehreren Werten anwenden. Sie können beispielsweise die Abfrage auf Elemente mit zwei Autoren einschränken, von denen jeder von einem Einschränkungsbin dargestellt wird, aber Elemente ausschließen, die nur einen der Autoren enthalten.
  
    
    

### Beispiel 1: Erstellen einer eingeschränkten Abfrage für HTML-Dateitypen

Im folgenden CSOM-Beispiel wird veranschaulicht, wie eine Einschränkung programmgesteuert durchgeführt werden kann, um die Suchergebnisse auf Elemente zu begrenzen, die nur den HTML-Dateityp aufweisen. Wie in  [Beispiel: Einschränkungsdaten](#SP15_Example_refinement_data) erwähnt, ist bei den Einschränkungsdaten im Zusammenhang mit dieser Einschränkungsoption **RefinerName** auf _Filetype_ und **RefinementToken** auf _"ǂǂ68746d6c"_ festgelegt.
  
    
    

```cs

using (var context = new ClientContext("http://<serverName>/<siteCollectionPath>"))
{
    var query = new KeywordQuery(context)
    {        
        QueryText = "home"
    };
    
    query.RefinementFilters.Add("FileType:\\"ǂǂ68746d6c\\"");
    var executor = new SearchExecutor(context);
    var results = executor.ExecuteQuery(query);

    context.ExecuteQuery();

    ResultTable relevantResultsTable = results.Value[0];
    var resultCount = 1;
    foreach (var relevantResult in relevantResultsTable.ResultRows)
    {
        Console.WriteLine("Relevant result number {0} has file type {1}.", 
            resultCount, relevantResult["FileType"]);
            resultCount++;
    }
}
```


### Beispiel 2: Erstellen einer Einschränkungsabfrage unter Verwendung zuvor abgerufener Einschränkungsdaten

Im folgenden CSOM-Beispiel wird veranschaulicht, wie eine Abfrage mit einer Einschränkungsspezifikation ausgeführt wird, um Einschränkungsdaten zu erstellen, die anschließend zum Durchführen einer Einschränkung verwendet werden. In diesem Beispiel wird der Prozess simuliert, bei dem ein Benutzer die erste Einschränkungsoption auswählt.
  
    
    

```cs

using (var context = new ClientContext("http://<serverName>/<siteCollectionPath>"))
{
    // Step 1: Run the query with refiner spec to provide refinement data in search result
    var query = new KeywordQuery(context)
    {
        QueryText = "home",
        Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/
2013-09-15/2013-09-21/2013-09-22),companies"
    };
    
    Console.WriteLine("Run query '{0}' with refiner spec '{1}'.", query.QueryText,
query.Refiners);
    var executor = new SearchExecutor(context);
    var results = executor.ExecuteQuery(query);
    context.ExecuteQuery();

    // The query has been run and we can now look at the refinement data, to view the
    // refinement options
    ResultTable relevantResultsTable = results.Value[0];
    ResultTable refinerResultsTable = results.Value[1];
    Console.WriteLine("Got back {0} refinement options in the result:",
        refinerResultsTable.RowCount);
    
    foreach (var refinementOption in refinerResultsTable.ResultRows)
    {
        Console.WriteLine("RefinerName: '{0}' RefinementName: '{1}'
            RefinementValue: '{2}' RefinementToken: '{3}' RefinementCount: '{4}'",
            refinementOption["RefinerName"],
            refinementOption["RefinementName"],
            refinementOption["RefinementValue"],
            refinementOption["RefinementToken"],
            refinementOption["RefinementCount"]
        );
    }

    // Step 2: Run the refined query with refinement filter to drill down into 
    // the search results. This example uses the first refinement option in the refinement
    // data, if available. This simulates an end user selecting this refinement option.
    var refinementOptionArray = refinerResultsTable.ResultRows.ToArray();

    if (refinementOptionArray.Length > 0)
    {                         
        var firstRefinementOption = refinementOptionArray[6];x
        // Construct the refinement filter by concatenation
        var refinementFilter = firstRefinementOption["RefinerName"] + ":" +
            firstRefinementOption["RefinementToken"];
        var refinedQuery = new KeywordQuery(context)
        {
            QueryText = query.QueryText
        };
        refinedQuery.RefinementFilters.Add(refinementFilter);
        refinedQuery.SelectProperties.Add("FileType");
        refinedQuery.SelectProperties.Add("Write");
        refinedQuery.SelectProperties.Add("Companies");
        Console.WriteLine("Run query '{0}' with refinement filter '{1}'",            
            refinedQuery.QueryText, refinementFilter);
        var refinedResults = executor.ExecuteQuery(refinedQuery);
        context.ExecuteQuery();
        ResultTable refinedRelevantResultsTable = refinedResults.Value[0];
        var resultCount = 1;
        foreach (var relevantResult in refinedRelevantResultsTable.ResultRows)
        {
            Console.WriteLine("Relevant result number {0} has FileType='{1}',
                Write='{2}', Companies='{3}'", 
                resultCount, 
                relevantResult["FileType"],
                relevantResult["Write"],
                relevantResult["Companies"]
            );
            resultCount++;
        }
    }
}
```


## Zusätzliche Ressourcen
<a name="SP15_Query_refinement_addresources"> </a>


-  [Konfigurieren des Einschränkungs-Webparts in SharePoint Server 2013](http://technet.microsoft.com/de-de/library/gg549985.aspx)
    
  
-  [Übersicht über das Suchschema in SharePoint Server 2013](http://technet.microsoft.com/de-de/library/jj219669%28office.15%29.aspx)
    
  

## Siehe auch
<a name="SP15_Query_refinement_addresources"> </a>


#### Weitere Ressourcen


  
    
    
 [Indexschema (FAST Search Server für SharePoint)](http://msdn.microsoft.com/library/3e1eed3f-8a6f-4911-9607-f1616e6a44f3%28Office.15%29.aspx)
  
    
    
 [Refiner-Element im Microsoft.Search.Query-Schema](http://msdn.microsoft.com/library/0512c17a-18e9-45e3-9061-9dba9cd45ef4%28Office.15%29.aspx)
