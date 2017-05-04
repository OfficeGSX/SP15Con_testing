---
title: Syntaxreferenz für FQL (FAST Query Language)
ms.prod: SHAREPOINT
ms.assetid: bd98a41b-623c-41d4-a15d-26c0d4ba4311
---



# Syntaxreferenz für FQL (FAST Query Language)
In diesem Artikel erfahren Sie mehr über den Aufbau komplexer Suchabfragen für Suche in SharePoint 2013 mit FQL (FAST Query Language). In dieser Referenz sind die Elemente einer FQL-Abfrage beschrieben, und es wird erläutert, wie Eigenschaftsspezifikationen, Tokenausdrücke und Operatoren in FQL-Abfragen verwendet werden.
## Einführung in FQL und Unterausdrücke und Ausdrücke der Abfragesprache in SharePoint Server 2013
<a name="SP15FQL_about"> </a>

FQL (FAST Query Language) ist eine leistungsstarke Abfragesprache, die es Entwicklern ermöglicht, genaue Suchvorgänge durchzuführen und den Suchbereich auf Werte zu begrenzen, die zu einer bestimmten verwalteten Eigenschaft oder einem Volltextindex gehören.
  
    
    
Ein Ausdruck der Abfragesprache kann geschachtelte Unterausdrücke enthalten, die Abfragebegriffe, Eigenschaftsspezifikationen und Operatoren enthalten, wie in Tabelle 1 beschrieben.
  
    
    

**Tabelle 1: Unterausdrücke in Ausdrücken der Abfragesprache**


|**Element**|**Beschreibung**|
|:-----|:-----|
|Tokenausdrücke  <br/> |Ein oder mehrere Abfragebegriffe, Phrasen oder numerische Werte, nach denen in einer Abfrage gesucht werden soll  <br/> |
|Eigenschaftsspezifikation  <br/> |Eine Eigenschaft oder ein Volltextindex, die bzw. der dem betroffenen Ausdruck zugeordnet werden soll  <br/> |
|Operatoren  <br/> |Schlüsselwörter, die boolesche Operationen (zum Beispiel **AND**, **OR**) oder andere Einschränkungen für Operanden angeben (zum Beispiel **FILTER**.)  <br/> |
   

### FQL-Abfrage - Beispiel

Im folgenden FQL-Abfragebeispiel wird nach den Begriffen "hello" und "world" in der verwalteten Eigenschaft **body** eines indizierten Elements gesucht:
  
    
    
 `body:string("hello world", mode="and")`
  
    
    
In diesem Beispiel:
  
    
    

-  `body:` schränkt den Bereich der Abfrage auf die verwaltete Eigenschaft **body** im Element ein.
    
  
-  `"hello world"` ist der Operand für den **STRING** -Operator, der die zu suchenden Begriffe angibt.
    
  
-  `mode="and"` gibt an, dass der logische Abfrageoperator **AND** auf `"hello world"` angewendet wird.
    
  
Die Länge der FAST Query Language-Abfragen ist auf 2.048 Zeichen beschränkt.
  
    
    

## Eigenschaftsspezifikation in FQL
<a name="property_specification"> </a>

Eine Eigenschaftsspezifikation begrenzt den Bereich des betroffenen Ausdrucks auf bestimmte Bereiche des indizierten Inhalts. Ein solcher Bereich kann durch einen Volltextindex oder eine verwaltete Eigenschaft angegeben werden. 
  
    
    
Verwaltete Eigenschaften des Typs **Text** und **YesNo** werden als Nächstes ausgewertet. Alle übrigen verwalteten Eigenschaftstypen, einschließlich des Typs **Datetime** werden als numerische Werte ausgewertet.
  
    
    
Wenn Sie keine Eigenschaftsspezifikation für einen Ausdruck einfügen, versucht das Suchmodul den im Indexschema definierten standardmäßigen Volltextindex zuzuordnen.
  
    
    
Dem Namen der Eigenschaft muss stets ein Doppelpunkt vorangestellt werden (Operator **In**), und numerische Operatoren müssen immer eine Eigenschaftsspezifikation enthalten.
  
    
    
Eine Eigenschaftsspezifikation (der Operator **In**) kann auf die folgenden Abfrageeinheiten angewendet werden:
  
    
    

- Einen einzelnen Begriff oder eine Phrase wie folgt:
    
     `author:shakespeare`
  
    
    
 `title:"to be or not to be"`
    
  
- Einen Operator, zum Beispiel den Operator **STRING**, wie folgt:
    
  ```
  
  title:string("to be or not to be")
  ```


    In diesem Fall gilt die Eigenschaftsspezifikation für den vollständigen Operator.
    
  

### Beispiele

Jeder der folgenden Ausdrücke findet Elemente, die in der verwalteten **title**-Eigenschaft sowohl den Begriff "viel" als auch den Begriff "nichts" enthalten.
  
    
    
 `title:and(much, nothing)`
  
    
    
 `and(title:much, title:nothing)`
  
    
    
 `title:string("much nothing", mode="and")`
  
    
    

## Tokenausdrücke in FQL
<a name="token_expressions"> </a>

Tokenausdrücke sind Wörter, Phrasen und numerische Werte, die mit dem Index abgeglichen werden.
  
    
    
Bei einem Tokenausdruck vom Typ Text kann es sich um ein einzelnes Wort oder eine in doppelte Anführungszeichen eingeschlossene Phrase handeln.
  
    
    
Ein numerischer Tokenausdruck kann ein einzelner Wert oder Wertebereichausdruck sein.
  
    
    

### Platzhalterausdrücke

Mit einem Platzhalterausdruck wird ein einzelner Begriff oder eine Phrase angegeben, die ein Sternchen (" *****") enthält. Durch die Angabe des Sternchens können auch 0 oder mehrere Zeichen mit Ausnahme von Leerzeichen gefunden werden. FQL unterstützt die Präfixsuche für individuelle verwaltete Texteigenschaften und Volltextindizes.
  
    
    

#### Platzhalterausdrücke - Beispiele

Nachfolgend finden Sie eine Liste gültiger Verwendungszwecke von Platzhalterausdrücken in FQL:
  
    
    

-  `text*`
    
  
-  `string("this examp*")`
    
  

### Numerische Ausdrücke
<a name="fql_token_numeric"> </a>

Jeder numerische Ausdruck muss eine Eigenschaftsspezifikation eines kompatiblen Indexschema-Datentyps enthalten. Tabelle 2 enthält die numerischen Datentypen, die in FQL verwendet werden können. 
  
    
    

**Tabelle 2: Numerische Datentypen, die in FQL verwendet werden können**


|**FQL-Typ**|**Kompatible Indexschematypen**|**Beschreibung**|
|:-----|:-----|:-----|
|**Int** <br/> |**Integer** <br/> |64-Bit-Ganzzahl  <br/> |
|**Float** <br/> |**Double** <br/> |64-Bit-Gleitkommazahl mit doppelter Genauigkeit  <br/> |
|**Decimal** <br/> |**Decimal** <br/> |128-Bit-Dezimal  <br/> |
|**Datetime** <br/> |**Datetime** <br/> |Ein Wert für Datum und Uhrzeit  <br/> Aufgrund der Unterstützung von Datum/Uhrzeit in FQL können die gleichen numerischen Operationen an Datum-/Uhrzeitwerten ausgeführt werden wie an anderen numerischen Werten.  <br/> |
   

#### Abfrageausdrücke für Datum und Uhrzeit
<a name="fql_token_datetime"> </a>

FQL stellt den **datetime**-Datentyp für Datum und Uhrzeit zur Verfügung.
  
    
    
Die folgenden ISO 8601-kompatiblen **datetime**-Formate werden in Abfragen unterstützt:
  
    
    

- JJJJ-MM-TT 
    
  
- JJJJ-MM-TTThh:mm:ss 
    
  
- JJJJ-MM-TTThh:mm:ssZ 
    
  
- JJJJ-MM-TTThh:mm:ssfrZ
    
  
In diesen **datetime**-Formaten:
  
    
    

-  _JJJJ_ gibt eine vierziffrige Jahresangabe an.
    
    > **HINWEIS**
      > Es werden nur vierziffrige Jahresangaben unterstützt. 
-  _MM_ gibt einen zweiziffrigen Monat an. Z. B. 01 = Januar.
    
  
-  _TT_ gibt einen zweiziffrigen Tag des Monats an (01-31).
    
  
-  _T_ gibt den Buchstaben "T" an.
    
  
-  _hh_ gibt eine zweiziffrige Stundenangabe an (00-23).
    
  
-  _mm_ gibt eine zweiziffrige Minutenangabe an (00-59).
    
  
-  _ss_ gibt eine zweiziffrige Sekundenangabe an (00-59).
    
  
-  _fr_ gibt optional eine Zehntelsekundenangabe an, _ss_; zwischen 1 und 7, die nach dem **.** nach den Sekunden folgt. Beispiel: 2012-09-27T11:57:34.1234567.
    
  
Alle Datums-/Zeitwerte müssen gemäß der UTC (Coordinated Universal Time) angegeben werden, auch als GMT (Greenwich Mean Time) Zeitzone bekannt. Der UTC-Zeitzonenbezeichner (nachfolgendes "Z") ist optional.
  
    
    

### Reservierte Wörter, Sonderzeichen und Escapezeichen
<a name="fql_token_numeric"> </a>

Die folgenden Wörter sind in FQL reserviert.
  
    
    
 `and, or, any, andnot, count, decimal, rank, near, onear, int, in32, int64, float, double, datetime, max, min, range, phrase, scope, filter, not, string, starts-with, ends-with, equals, words, xrank.`
  
    
    
Wenn Sie diese Wörter als Begriffe in einem Abfrageausdruck ausdrücken möchten, müssen Sie sie in doppelte Anführungszeichen einschließen, wie in den folgenden Beispielen gezeigt: 
  
    
    

-  `or("any", "and", "xrank")`
    
  
-  `string("any and xrank", mode="OR")`
    
  
-  `phrase(this, is, a, "phrase")`
    
  

> **TIPP**
> Reservierte Wörter und Zeichen unterliegen nicht der Groß-/Kleinschreibung. Es wird jedoch die Verwendung von Kleinbuchstaben empfohlen, um zukünftige Kompatibilität zu gewährleisten. 
  
    
    

FQL erfordert nicht immer, dass Zeichenfolgen in doppelte Anführungszeichen eingeschlossen werden. Beispielsweise ist  `and(cat, dog)` gültige FQL-Syntax, obwohl `cat` und `dog` nicht in doppelte Anführungszeichen eingeschlossen sind. Wir empfehlen jedoch, doppelte Anführungszeichen zu verwenden, um Konflikte mit reservierten Wörtern zu vermeiden.
  
    
    
Die Abfragebegriffe sind gemäß Ihren Gebietsschemaeinstellungen mit Token versehen. Beim Vorgang der Tokenisierung werden bestimmte Sonderzeichen entfernt. Da Sonderzeichen entfernt werden, stimmen die folgenden FQL-Ausdrücke überein.
  
    
    
 `and("[king]", "<queen>")`
  
    
    
 `and("king", "queen")`
  
    
    
Wenn eine Abfrage Begriffe aus der Benutzereingabe oder einer anderen Anwendung enthält, verwenden Sie den Operator  `string("<query terms>", mode="AND|OR|PHRASE")`, um Konflikte mit reservierten Wörtern in der Abfragesprache zu vermeiden. Sie müssen auch mögliche doppelte Anführungszeichen aus der vom Benutzer angegebenen Abfrage entfernen.
  
    
    

## FQL-Operatoren
<a name="fql_operators"> </a>

FQL-Operatoren (FAST Query Language) sind Schlüsselwörter, die boolesche Operationen oder andere Einschränkungen für Operanden angeben. Die Syntax der FQL-Operatoren lautet wie folgt:
  
    
    
 `[property-spec:]operator(operand [,operand]* [, parameter="value"]*)`
  
    
    
Inhalt der Syntax:
  
    
    

-  _property-spec_ ist eine optionale Eigenschaftsspezifikation gefolgt vom Operator "in".
    
  
-  _operator_ ist ein Schlüsselwort, das eine auszuführende Operation angibt.
    
  
-  _operand_ ist ein Begriffsausdruck oder ein anderer Operator.
    
  
-  _parameter_ ist der Name des Werts, der das Verhalten des Operators ändert.
    
  
-  _value_ ist der für den Parameternamen zu verwendende Wert.
    
  
Operatornamen, Parameternamen und Textwerte für Parameter unterliegen nicht der Groß-/Kleinschreibung. Leerzeichen sind im Textkörper des Operators zulässig, werden jedoch ignoriert, sofern sie nicht in doppelte Anführungszeichen eingeschlossen sind. Die Länge der FAST Query Language-Abfragen ist auf 2.048 Zeichen beschränkt.
  
    
    
Tabelle 3 enthält die Typen der von FQL unterstützten Operatoren. 
  
    
    

**Tabelle 3: Von FQL unterstützte Operatortypen**


|**Typ**|**Beschreibung**|**Operatoren**|
|:-----|:-----|:-----|
|String  <br/> |Ermöglicht die Angabe von Abfrageoperationen in einer Begriffszeichenfolge. Dies ist der gängigste Operator für Textzeichenfolgen.  <br/> | [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator) <br/> |
|Boolesch  <br/> |Ermöglicht die Kombination von Begriffen und Unterausdrücken in einer Abfrage.  <br/> | [AND](fast-query-language-fql-syntax-reference.md#fql_and_operator),  [OR](fast-query-language-fql-syntax-reference.md#fql_or_operator),  [ANY](fast-query-language-fql-syntax-reference.md#fql_any_operator),  [ANDNOT](fast-query-language-fql-syntax-reference.md#fql_andnot_operator),  [NOT](fast-query-language-fql-syntax-reference.md#fql_not_operator),  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator),  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator) <br/> |
|Näherung  <br/> |Hiermit können Sie angeben, wie nah die Abfragebegriffe in einer übereinstimmenden Textsequenz beieinander liegen.  <br/> | [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator),  [ONEAR](fast-query-language-fql-syntax-reference.md#fql_onear_operator),  [PHRASE](fast-query-language-fql-syntax-reference.md#fql_phrase_operator),  [STARTS-WITH](fast-query-language-fql-syntax-reference.md#fql_startswith_operator),  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator),  [EQUALS](fast-query-language-fql-syntax-reference.md#fql_equals_operator) <br/> |
|Numerisch  <br/> |Hiermit können Sie numerische Bedingungen in der Abfrage angeben.  <br/> | [RANGE](fast-query-language-fql-syntax-reference.md#fql_range_operator) , [INT](fast-query-language-fql-syntax-reference.md#fql_int_operator),  [FLOAT](fast-query-language-fql-syntax-reference.md#fql_float_operator),  [DATETIME](fast-query-language-fql-syntax-reference.md#fql_datetime_operator),  [DECIMAL](#fql_decimal_operator) <br/> |
|Relevanz  <br/> |Ermöglicht Ihnen, die Relevanzbewertung einer Abfrage zu beeinflussen.  <br/> | [XRANK](fast-query-language-fql-syntax-reference.md#fql_xrank_operator) und [FILTER](fast-query-language-fql-syntax-reference.md#fql_filter_operator) <br/> |
   
Tabelle 4 enthält eine Liste der unterstützten Operatoren.
  
    
    

**Tabelle 4: Von FQL unterstützte Operatoren**


|**Operator**|**Beschreibung**|**Typ**|
|:-----|:-----|:-----|
| [AND](fast-query-language-fql-syntax-reference.md#fql_and_operator) <br/> |Gibt nur Elemente zurück, die mit allen **AND** -Operanden übereinstimmen. <br/> |Boolesch  <br/> |
| [ANDNOT](fast-query-language-fql-syntax-reference.md#fql_andnot_operator) <br/> |Gibt nur Elemente zurück, die mit dem ersten Operanden und nicht mit den nachfolgenden Operanden überstimmen.  <br/> |Boolesch  <br/> |
| [ANY](fast-query-language-fql-syntax-reference.md#fql_any_operator) <br/> |Ähnelt dem Operator **OR**, abgesehen davon, dass die dynamische Rangfolge (die Relevanzbewertung in der Ergebnismenge) weder von der Anzahl der übereinstimmenden Operanden noch vom Abstand zwischen den Begriffen im Element betroffen ist. <br/> |Boolesch  <br/> |
| [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator) <br/> |Ermöglicht Ihnen, anzugeben, wie oft ein Abfragebegriff in einem Element enthalten sein muss, damit er als Ergebnis zurückgegeben wird. Der Operand kann ein einzelner Abfragebegriff, eine Phrase oder ein Abfragebegriff mit Platzhalter sein.  <br/> |Boolesch  <br/> |
| [DATETIME](fast-query-language-fql-syntax-reference.md#fql_datetime_operator) <br/> |Stellt die explizite Eingabe numerischer Werte zur Verfügung.  <br/> Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.  <br/> |Numerisch  <br/> |
| [DECIMAL](fast-query-language-fql-syntax-reference.md#fql_decimal_operator) <br/> |Stellt die explizite Eingabe numerischer Werte zur Verfügung.  <br/> Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.  <br/> |Numerisch  <br/> |
| [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator) <br/> |Gibt an, dass ein Wort oder eine Phrase am Ende einer verwalteten Eigenschaft angezeigt werden muss.  <br/> |Näherung  <br/> |
| [EQUALS](fast-query-language-fql-syntax-reference.md#fql_equals_operator) <br/> |Gibt an, dass ein Wort, ein Phrasenbegriff oder eine Phrase eine genaue Tokenübereinstimmung mit der verwalteten Eigenschaft aufweisen muss.  <br/> |Näherung  <br/> |
| [FILTER](fast-query-language-fql-syntax-reference.md#fql_filter_operator) <br/> |Wird verwendet, um Metadaten oder andere strukturierte Daten abzufragen.  <br/> |Relevanz  <br/> |
| [FLOAT](fast-query-language-fql-syntax-reference.md#fql_float_operator) <br/> |Stellt die explizite Eingabe numerischer Werte zur Verfügung.  <br/> Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.  <br/> |Numerisch  <br/> |
| [INT](fast-query-language-fql-syntax-reference.md#fql_int_operator) <br/> |Stellt die explizite Eingabe numerischer Werte zur Verfügung.  <br/> Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.  <br/> |Numerisch  <br/> |
| [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator) <br/> |Beschränkt die Ergebnismenge auf Elemente, die  `N` Begriffe innerhalb eines bestimmten Abstands voneinander aufweisen. <br/> |Näherung  <br/> |
| [NOT](fast-query-language-fql-syntax-reference.md#fql_not_operator) <br/> |Gibt nur Elemente zurück, die den Operanden ausschließen.  <br/> |Boolesch  <br/> |
| [ONEAR](fast-query-language-fql-syntax-reference.md#fql_onear_operator) <br/> |Die sortierte Variante von **NEAR**, die eine sortierte Zuordnung der Begriffe erfordert. Der Operator **ONEAR** kann verwendet werden, um die Ergebnismenge auf Elemente mit `N` Begriffen zu beschränken, die sich in einem bestimmten Abstand voneinander befinden. Gibt nur Elemente zurück, die nicht mit dem Operanden übereinstimmen. Der Operand kann jeder gültige FQL-Ausdruck sein. <br/> |Näherung  <br/> |
| [OR](fast-query-language-fql-syntax-reference.md#fql_or_operator) <br/> |Gibt nur Elemente zurück, die mindestens einem der **OR** -Operanden entsprechen. Übereinstimmende Elemente werden in der dynamischen Rangfolge (Relevanzbewertung in der Ergebnismenge) weiter oben angegeben, wenn mehrere der **OR** -Operanden übereinstimmen. <br/> |Boolesch  <br/> |
| [PHRASE](fast-query-language-fql-syntax-reference.md#fql_phrase_operator) <br/> | Gibt nur Elemente zurück, die einer genauen Tokenzeichenfolge entsprechen. <br/> |Näherung  <br/> |
| [RANGE](fast-query-language-fql-syntax-reference.md#fql_range_operator) <br/> | Ermöglicht mit Bereichen übereinstimmende Ausdrücke. Der Operator **RANGE** wird für numerische verwaltete Eigenschaften und verwaltete Eigenschaften für Datum/Uhrzeit verwendet. <br/> |Numerisch  <br/> |
| [STARTS-WITH](fast-query-language-fql-syntax-reference.md#fql_startswith_operator) <br/> |Gibt an, dass ein Wort oder eine Phrase am Anfang einer verwalteten Eigenschaft angezeigt werden muss.  <br/> |Näherung  <br/> |
| [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator) <br/> |Definiert eine übereinstimmende boolesche Bedingung für eine Textzeichenfolge.  <br/> |String  <br/> |
| [XRANK](fast-query-language-fql-syntax-reference.md#fql_xrank_operator) <br/> |Ermöglicht es Ihnen, die dynamische Rangfolge von Elementen basierend auf bestimmten Begriffsvorkommen zu erhöhen, ohne zu ändern, welche Elemente mit der Abfrage übereinstimmen. Ein **XRANK** -Ausdruck enthält eine Komponente, die abgeglichen werden muss, und eine oder mehrere Komponenten, die nur für die dynamische Rangfolge relevant sind. <br/> |Relevanz  <br/> |
   

> **HINWEIS**
> In SharePoint 2013 gilt der Operator **RANK** als überholt und hat keine Wirkung mehr. Verwenden Sie stattdessen den Operator **XRANK**. 
  
    
    


### AND
<a name="fql_and_operator"> </a>

Gibt nur Elemente zurück, die mit allen **AND** -Operanden übereinstimmen. Bei den Operanden kann es sich um einen einzelnen Begriff oder einen beliebigen gültigen FQL-Unterausdruck handeln.
  
    
    

#### Syntax

 `and(operand, operand [, operand]*)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze", „Hund" und „Fuchs" enthält.
  
    
    
 `and(cat, dog, fox)`
  
    
    

### ANDNOT
<a name="fql_andnot_operator"> </a>

Gibt nur Elemente zurück, die mit dem ersten Operanden und nicht mit den nachfolgenden Operanden überstimmen. Bei den Operanden kann es sich um einen einzelnen Begriff oder einen beliebigen gültigen FQL-Unterausdruck handeln.
  
    
    

#### Syntax

 `andnot(operand, operand [,operand]*)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

 **Beispiel 1**: Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze", aber nicht den Begriff „Hund" enthält.
  
    
    
 `andnot(cat, dog)`
  
    
    
 **Beispiel 2**: Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Hund", aber weder „Beagle" noch „Chihuahua" enthält.
  
    
    
 `andnot(dog, beagle, chihuahua)`
  
    
    

### ANY
<a name="fql_any_operator"> </a>


> **HINWEIS**
> In SharePoint Server 2013 gilt der Operator **ANY** als verwaltet. Verwenden Sie stattdessen den Operator **OR**. 
  
    
    

Ähnelt dem Operanden  [OR](fast-query-language-fql-syntax-reference.md#fql_or_operator), abgesehen davon, dass die dynamische Rangfolge (die Relevanzbewertung in der Ergebnismenge) weder von der Anzahl der übereinstimmenden Operanden noch vom Abstand zwischen den Begriffen im Element betroffen ist. Bei den Operanden kann es sich um einen einzelnen Begriff oder einen beliebigen gültigen FQL-Unterausdruck handeln.
  
    
    
Die Komponente der dynamischen Rangfolge für diesen Teil der Abfrage basiert auf dem Begriff mit der bestmöglichen Übereinstimmung innerhalb des Ausdrucks **ANY**.
  
    
    

> **HINWEIS**
> Der Unterschied zu **OR** bezieht sich nur auf die Rangfolge innerhalb der Ergebnismenge. Die Abfrage gleicht dieselbe Gesamtmenge von Elementen ab.
  
    
    


#### Syntax

 `any(operand, operand [,operand]*)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

 Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält.
  
    
    
Wenn der Index sowohl „Katze" als auch „Hund" enthält, „Katze" aber als der bessere Treffer betrachtet wird, basiert die dynamische Rangfolge des Elements auf dem Begriff „Katze", und der Begriff „Hund" wird nicht berücksichtigt.
  
    
    
 `any(cat, dog)`
  
    
    

### COUNT
<a name="fql_count_operator"> </a>

Gibt an, wie oft ein Abfragebegriff in einem Element enthalten sein muss, damit das Element als Ergebnis zurückgegeben wird. Der Operand kann ein einzelner Abfragebegriff, eine Phrase oder ein Abfragebegriff mit Platzhalter sein.
  
    
    

#### Syntax

 `property-spec:count(operand [,from=<numeric value>, to=<numeric value>])`
  
    
    

#### Parameter



|**Parameter**|**Value**|**Beschreibung**|
|:-----|:-----|:-----|
| _From_ <br/> | _<numeric_value>_ <br/> |Der Wert des Parameters  _from_ muss eine positive ganze Zahl sein, die angibt, wie oft eine Übereinstimmung des Operanden mindestens erzielt werden muss. <br/> Wenn der Parameter  _from_ nicht angegeben ist, ist kein unterer Grenzwert vorhanden. <br/> |
| _to_ <br/> | _<numeric_value>_ <br/> |Der Wert des Parameters  _to_ muss eine positive ganze Zahl sein, die angibt, wie oft eine Übereinstimmung des Operanden höchstens (nicht inklusive) erzielt werden muss. Beispielsweise gibt ein _to_-Wert von **11** eine Übereinstimmung von 10 Mal oder weniger an. <br/> Wenn der Parameter  _to_ nicht angegeben ist, ist kein oberer Grenzwert vorhanden. <br/> |
   

#### Beispiele

 **Beispiel 1**: Der folgende Ausdruck gleicht mindestens 5 Vorkommen des Begriffs "Katze" ab.
  
    
    
 `count(cat, from=5)`
  
    
    
 **Beispiel 2**: Der folgende Ausdruck gleicht mindestens 5, aber nicht 10 oder mehr Vorkommen des Begriffs "Katze" ab.
  
    
    
 `count(cat, from=5, to=10)`
  
    
    
 **Beispiel 3**: Jeder der folgenden Ausdrücke gleicht mindestens 3 Vorkommen eines bestimmten Worts ab. Das Wort kann entweder „Katze" oder „Hund" sein.
  
    
    
 `count(or(cat, dog), from=3)count(string("cat dog", mode="or"), from=3)`
  
    
    
Die folgende Tabelle enthält Beispiele der Zeichenfolgenwerte der verwalteten Eigenschaft und gibt an, ob diese mit den beiden Ausdrücken in Beispiel 3 übereinstimmen.
  
    
    


|**Treffer?**|**Text**|
|:-----|:-----|
|Ja  <br/> |Meine Katze mag meinen Hund, aber mein Hund hasst meine Katze.  <br/> |
|Nein  <br/> |Mein Vogel mag meinen Molch, aber mein Hund hasst meine Katze.  <br/> |
   

### DATETIME
<a name="fql_datetime_operator"> </a>

Stellt die explizite Eingabe numerischer Werte für Datum/Uhrzeit zur Verfügung. Der Operand ist eine Datum-/Uhrzeitzeichenfolge, die gemäß der unter  [Tokenausdrücke in FQL](fast-query-language-fql-syntax-reference.md#token_expressions) angegebenen Syntax formatiert ist.
  
    
    
Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.
  
    
    

#### Syntax

 `datetime(<date/time string>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

### DECIMAL
<a name="fql_decimal_operator"> </a>

Stellt die explizite Eingabe von Dezimalwerten zur Verfügung. Der Operand ist ein Dezimalwert gemäß der unter  [Tokenausdrücke in FQL](fast-query-language-fql-syntax-reference.md#token_expressions) angegebenen Syntax.
  
    
    
Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.
  
    
    

#### Syntax

 `decimal(<decimal point value>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

### ENDS-WITH
<a name="fql_endswith_operator"> </a>

Gibt an, dass ein Wort oder eine Phrase am Ende einer verwalteten Eigenschaft angezeigt werden muss (Begrenzungsabgleich).
  
    
    
Der Begrenzungsabgleich wird bei numerischen verwalteten Eigenschaften nicht unterstützt. Numerische verwaltete Eigenschaften unterliegen stets dem Abgleich von exakten Werten oder Wertebereichen. 
  
    
    
Einige Anwendungen erfordern möglicherweise, dass Sie einen exakten Abgleich einer verwalteten Eigenschaft durchführen können. Dies kann beispielsweise eine verwaltete **product name**-Eigenschaft sein, bei der der vollständige Name eines Produkts eine Teilzeichenfolge eines anderen Produktnamens ist.
  
    
    

#### Syntax

 `ends-with(<term or phrase>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Der folgende Ausdruck gleicht Elemente mit den Werten „Herr Adam Jones" und „Adam Jones" in der verwalteten Eigenschaft „author" ab. Elemente mit dem Wert „Mr. Adam Jones" werden nicht gefunden.
  
    
    
 `author:ends-with("adam jones")`
  
    
    

#### Hinweise

Der Begrenzungsabgleich kann auf den gesamten Text der verwalteten Eigenschaft angewendet werden, oder auf einzelne Zeichenfolgen innerhalb einer verwalteten Eigenschaft, die eine Liste von Werten enthält (beispielsweise eine Liste mit Namen). In diesem Fall empfiehlt es sich, den genauen Inhalt jeder Zeichenfolge abzugleichen, und Abgleichsabfragen über Zeichenfolgengrenzen hinweg zu vermeiden. 
  
    
    
Um Begrenzungsabgleichsabfragen durchzuführen, müssen Sie die relevante verwaltete Eigenschaft im Indexschema konfigurieren. 
  
    
    
Wenn Sie das Begrenzungsabgleichsfeature für die verwaltete Eigenschaft aktivieren, können Sie Folgendes durchführen: 
  
    
    

- Verwenden expliziter Begrenzungsabgleichsabfragen 
    
  
- Verhindern, dass Phrasen über Zeichenfolgengrenzen hinweg abgeglichen werden. Für verwaltete Eigenschaften, die mehrere Zeichenfolgen enthalten, stellt dieses Feature sicher, dass eine Zeichenfolge vor oder nach einer Begrenzungsangabe keine Wörter abgleicht.
    
  

### EQUALS
<a name="fql_equals_operator"> </a>

Gibt an, dass ein Wort oder eine Phrase eine genaue Tokenübereinstimmung mit der verwalteten Eigenschaft aufweisen muss.
  
    
    

#### Syntax

 `equals(<term or phrase>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Das folgende Beispiel gleicht Elemente mit den Werten „Adam Jones"" in der verwalteten Eigenschaft „author" ab. Elemente mit den Werten „Mr. Adam Jones" oder „Herr Adam Jones" werden nicht gefunden.
  
    
    
 `author:equals("adam jones")`
  
    
    

#### Hinweise

Siehe auch  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator).
  
    
    

### FILTER
<a name="fql_filter_operator"> </a>

Wird verwendet, um Metadaten oder andere strukturierte Daten abzufragen. 
  
    
    
Die Verwendung des Operators **FILTER** impliziert Folgendes für die angegebene Abfrage:
  
    
    

- Die Linguistik wird auf linguistics="OFF" festgelegt.
    
  
- Die Rangfolge wird deaktiviert.
    
  
- In der Zusammenfassung mit hervorgehobenen Treffern für den Abfrageergebnistreffer wird keine Abfragehervorhebung verwendet.
    
  

> **TIPP**
> Wenn Sie den Operator **STRING** innerhalb eines **FILTER** -Ausdrucks verwenden, ist die Linguistik standardmäßig deaktiviert. Sie können die linguistische Verarbeitung innerhalb jedes **STRING** -Ausdrucks innerhalb eines **FILTER** -Ausdrucks mit dem Operanden `linguistics="ON"` aktivieren.
  
    
    


#### Syntax

 `filter(<any valid FQL operator expression>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Der folgende Ausdruck gleicht Elemente mit einer verwalteten **Title**-Eigenschaft ab, die „Sonate" enthält, und einer verwalteten **Doctype**-Eigenschaft, die nur das Token „Audio" enthält. Für „Audio" wird kein linguistischer Abgleich durchgeführt. Da das Token **FILTER** zum Abgleich von „Audio" verwendet wird, wird dieser Text in der Zusammenfassung mit hervorgehobenen Treffern nicht hervorgehoben.
  
    
    
 `and(title:sonata, filter(doctype:equals("audio")))`
  
    
    

#### Hinweise

Wenn Sie die Abfrage so einschränken müssen, dass wenigstens ein umfangreicher Satz von Ganzzahlen in einer numerischen Eigenschaft abgeglichen werden muss, können Sie dies auf zwei funktional äquivalente Arten ausdrücken: 
  
    
    

-  `and(string("hello world"), filter(property-spec:or(1, 20, 453, ... , 3473)))`
    
  
-  `and(string("hello world"), filter(property-spec:int("1 20 453 ... 3473", mode="or")))`
    
  
Im zweiten Beispiel wird der Operator **INT** mit einer Zeichenfolge verwendet, deren Gruppe numerischer Werte in doppelte Anführungszeichen eingeschlossen ist. Dadurch wird eine erheblich bessere Abfrageleistung erzielt, wenn eine umfangreiche Gruppe numerischer Werte gefiltert wird.
  
    
    
Wenn eine große Gruppe von Werten gefiltert werden muss, sollten Sie erwägen, anstelle von Zeichenfolgewerten numerische Werte zu verwenden, und Ihre Abfrage mit der optimierten Syntax ausdrücken.
  
    
    

### FLOAT
<a name="fql_float_operator"> </a>

Stellt die explizite Eingabe numerischer Gleitkommawerte zur Verfügung. Der Operand ist ein Gleitkommawert gemäß der unter  [Tokenausdrücke in FQL](fast-query-language-fql-syntax-reference.md#token_expressions) angegebenen Syntax.
  
    
    
Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.
  
    
    

#### Syntax

 `float(<floating point value>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

### INT
<a name="fql_int_operator"> </a>

Stellt die explizite Eingabe von Ganzzahlwerten zur Verfügung. Der Operand ist ein Ganzzahlwert gemäß der unter  [Tokenausdrücke in FQL](fast-query-language-fql-syntax-reference.md#token_expressions) angegebenen Syntax.
  
    
    
Die explizite Typumwandlung ist optional und normalerweise nicht notwendig. Der Typ des Abfragebegriffs wird gemäß dem Typ der numerischen verwalteten Zieleigenschaft erkannt.
  
    
    
Der Operator **INT** kann auch verwendet werden, um eine Gruppe von Ganzzahlwerten als Argumente für boolesche FQL-Operatoren auszudrücken. Dadurch wird ein leistungseffizientes Verfahren zur Verfügung gestellt, um eine Gruppe von Ganzzahlwerten in einer Abfrage anzugeben, da die Werte, die mit dem Operator **INT** übergeben werden, nicht mit der FQL-Abfrageanalyse analysiert werden, sondern direkt an die Komponente für den Abfrageabgleich übergeben werden.
  
    
    

#### Syntax

 `int(<integer value>)`
  
    
    
 `int("value, value, … , value")`
  
    
    
In der ersten Syntax wird eine einzelne ganze Zahl angegeben. Die zweite Syntax gibt eine kommagetrennte Liste von Ganzzahlwerten an, die in doppelte Anführungszeichen eingeschlossen sind.
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Wenn Sie die Abfrage so einschränken müssen, dass mindestens ein Wert aus einer großen Gruppe von Ganzzahlwerten in einer numerischen Eigenschaft gefunden wird, können Sie dies mit dem Operator **INT** ausdrücken:
  
    
    
 `and(string("hello world"), filter(id:int("1 20 49 124 453 985 3473", mode="or")))`
  
    
    

### NEAR
<a name="fql_near_operator"> </a>

Beschränkt die Ergebnismenge auf Elemente, die  _N_ Begriffe innerhalb eines bestimmten Abstands voneinander aufweisen.
  
    
    
Die Reihenfolge der Abfragebegriffe ist für die Übereinstimmung nicht von Bedeutung, sondern nur der Abstand. 
  
    
    
Mit den **NEAR** -Operatoren kann eine beliebige Anzahl von Begriffen kombiniert werden.
  
    
    
 **NEAR** -Operanden können einzelne Begriffe, Phrasen oder boolesche Ausdrücke des Operators **OR** oder **ANY** sein. Platzhalter werden akzeptiert.
  
    
    
Wenn mehrere Operanden des Operators **NEAR** das gleiche indizierte Token abgleichen, werden sie als nah beieinander liegend betrachtet.
  
    
    

#### Syntax

 `near(arg, arg [, arg]* [, N=<numeric value>])`
  
    
    

#### Parameter



|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _N_ <br/> | _<numeric_value>_ <br/> |Gibt die maximale Anzahl der Wörter an, die zwischen den Begriffen angezeigt werden darf (explizite Nähe).  <br/> Wenn **NEAR** mehr als zwei Operanden enthält, wird die maximale Anzahl der zwischen den Begriffen zulässigen Wörter ( _N_) innerhalb des ganzen Ausdrucks gezählt.  <br/> Standardwert: **4** <br/> |
   

#### Beispiele

 **Beispiel 1**: Der folgende Ausdruck gleicht Zeichenfolgen ab, die die Begriffe „Katze" und „Hund" enthalten, wenn diese durch höchstens vier indizierte Token (Standardwert) getrennt sind.
  
    
    
 `near(cat, dog)`
  
    
    
 **Beispiel 2**: Der folgende Ausdruck gleicht Zeichenfolgen ab, die „Katze", „Hund", „Fuchs" und „Wolf" enthalten, wenn diese durch höchstens vier indizierte Token getrennt sind.
  
    
    
 `near(cat, dog, fox, wolf)`
  
    
    
Die folgende Tabelle enthält Beispiele der Zeichenfolgenwerte der verwalteten Eigenschaft und gibt an, ob diese mit dem vorherigen Ausdruck in Beispiel 2 übereinstimmen.
  
    
    


|**Treffer?**|**Text**|
|:-----|:-----|
|Ja  <br/> |Die Abbildung zeigt eine Katze, einen Hund, einen Fuchs und einen Wolf.  <br/> |
|Ja (mit Wortstammerkennung)  <br/> |Hunde, Füchse und Wölfe sind hundeartig, Katzen hingegen sind katzenartig.  <br/> |
|Nein  <br/> |Die Abbildung zeigt eine Katze mit einem Hund, einem Fuchs und einem Wolf.  <br/> |
   
Der folgende Ausdruck gleicht alle Zeichenfolgen in der vorherigen Tabelle ab.
  
    
    
 `near(cat, dog, fox, wolf, N=5)`
  
    
    

#### Hinweise

 **Überlegungen zum Abstand zwischen NEAR/ONEAR-Begriffen**
  
    
    
 _N_ gibt die maximale Anzahl der Wörter an, die zwischen den Abfragebegriffen innerhalb des übereinstimmenden Segments des Elements angezeigt werden dürfen. Wenn **NEAR** oder **ONEAR** mehr als zwei Operanden enthält, wird die maximal zulässige Anzahl der Wörter zwischen den Abfragebegriffen ( _N_) innerhalb des Segments des Elements gezählt, das mit allen **NEAR** - oder **ONEAR** -Begriffen übereinstimmt.
  
    
    
 **NEAR** oder **ONEAR** fungiert als Text mit Token. Dies bedeutet, dass Sonderzeichen wie Komma („ **,** "), Punkt („ **.** "), Doppelpunkt („ **:** ") oder Semikolon („ **;** ") als Leerzeichen behandelt werden. Der Begriff „Abstand" bezieht sich auf Token innerhalb des indizierten Texts.
  
    
    
Wenn Sie **ONEAR** oder **NEAR** mit gleichen Operanden verwenden, funktioniert der Operator wie folgt:
  
    
    
 `near(a, a, n=x)`
  
    
    
Diese Abfrage gibt immer **true** zurück, wenn mindestens eine Instanz des Buchstabens „ `a`"' im Kontext angezeigt wird. Dies bedeutet auch, dass **NEAR** nicht als **COUNT**-Operator verwendet werden kann. Weitere Informationen zum Zählen von Begriffsvorkommen finden Sie unter Operator  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator). 
  
    
    
Wenn **NEAR** auf Phrasen angewendet wird, werden ebenfalls sich überlappende Phrase im Text gefunden.
  
    
    
Wenn ein Token im übereinstimmenden Segment mehr als einen Operanden dem **NEAR** - oder dem **ONEAR** -Ausdruck zuordnet, kann die Abfrage die Elemente möglicherweise auch dann abgleichen, wenn die Anzahl der nicht übereinstimmenden Token innerhalb des übereinstimmenden Segments den Wert von _N_ im Ausdruck des **NEAR**- oder des **ONEAR**-Operators überschreitet. Bei einer Überlappung kann es sich beispielsweise um sich überlappende Phrasen handeln. Wenn die Anzahl der Treffer bei Tokenüberlappungen  `O` ist, gleicht die Abfrage Elemente ab, wenn nicht mehr als `N+O` nicht übereinstimmende Token innerhalb des übereinstimmenden Segments des Elements angezeigt werden.
  
    
    
 ** **NEAR** oder **ONEAR** mit **NOT****
  
    
    
Der Operator **NOT** kann nicht innerhalb des Operators **NEAR** oder **ONEAR** verwendet werden. Die folgende FQL-Syntax ist nicht korrekt:
  
    
    
 `near(audi,not(bmw),n=2)`
  
    
    

### NOT
<a name="fql_not_operator"> </a>

Gibt nur Elemente zurück, die dem Operanden nicht entsprechen. Der Operand kann ein beliebiger gültiger FQL-Ausdruck sein.
  
    
    

#### Syntax

 `not(operand)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

### ONEAR
<a name="fql_onear_operator"> </a>

Die sortierte Variante von **NEAR**, die eine sortierte Zuordnung der Begriffe erfordert. The Operator **ONEAR** kann verwendet werden, um die Ergebnismenge auf Elemente mit _N_-Begriffen zu beschränken, die sich in einem bestimmten Abstand voneinander befinden.
  
    
    

#### Syntax

 `onear(arg, arg [, arg]* [, N=<numeric value>])`
  
    
    

#### Parameter



|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _N_ <br/> | _<numeric_value>_ <br/> |Gibt die maximale Anzahl der Wörter an, die zwischen den Begriffen angezeigt werden darf (explizite Nähe).  <br/> Wenn **ONEAR** mehr als zwei Operanden enthält, wird die maximale Anzahl der zwischen den Begriffen zulässigen Wörter ( _N_) innerhalb des ganzen Ausdrucks gezählt.  <br/> Standardwert: **4** <br/> |
   

#### Beispiele

 **Beispiel 1**: Der folgende Ausdruck gleicht alle Vorkommen der Wörter „Katze", „Hund", „Fuchs" und „Wolf" ab, die nacheinander angezeigt werden, wenn diese durch maximal vier indizierte Token getrennt sind.
  
    
    
 `onear(cat, dog, fox, wolf)`
  
    
    
Die folgende Tabelle enthält Beispiele der Zeichenfolgenwerte der verwalteten Eigenschaft und gibt an, ob diese mit dem vorherigen Ausdruck in Beispiel 2 übereinstimmen.
  
    
    


|**Treffer?**|**Text**|
|:-----|:-----|
|Ja  <br/> |Die Abbildung zeigt eine Katze, einen Hund, einen Fuchs und einen Wolf.  <br/> |
|Nein  <br/> |Hunde, Füchse und Wölfe sind hundeartig, Katzen hingegen sind katzenartig.  <br/> |
|Nein  <br/> |Die Abbildung zeigt eine Katze mit einem Hund, einem Fuchs und einem Wolf.  <br/> |
   
 **Beispiel 2**: Der folgende Ausdruck (mit Wortstammerkennung) gleicht den Text in der zweiten Zeile der vorherigen Tabelle ab.
  
    
    
 `onear(dog, fox, wolf, cat, N=5)`
  
    
    
 **Beispiel 3**: Der folgende Ausdruck gleicht den Text in der ersten und dritten Zeile der vorherigen Tabelle ab.
  
    
    
 `onear(cat, dog, fox, wolf, N=5)`
  
    
    

#### Hinweise

Siehe auch  [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator).
  
    
    

### OR
<a name="fql_or_operator"> </a>

Gibt nur Elemente zurück, die mindestens einem der **OR** -Operanden entsprechen. Übereinstimmende Elemente werden in der dynamischen Rangfolge (Relevanzbewertung in der Ergebnismenge) weiter oben angegeben, wenn mehrere der **OR** -Operanden übereinstimmen. Bei den Operanden kann es sich um einen einzelnen Begriff oder einen beliebigen gültigen FQL-Unterausdruck handeln.
  
    
    

#### Syntax

 `or(operand, operand [,operand]*)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Der folgende Ausdruck gleicht alle Elemente ab, bei denen der Standard-Volltextindex entweder „Katze" oder „Hund" enthält. Wenn der Standard-Volltextindex eines Elements sowohl den Begriff „Katze" als auch den Begriff „Hund" enthält, wird das Element abgeglichen und erhält eine höhere dynamische Bewertung als der Fall wäre, wenn das Element nur eines der Token enthielte.
  
    
    
 `or(cat, dog)`
  
    
    

### PHRASE
<a name="fql_phrase_operator"> </a>

Sucht nach einer exakten Tokenzeichenfolge. 
  
    
    
Die **PHRASE** -Operanden können einzelne Begriffe sein. Platzhalter werden akzeptiert.
  
    
    

#### Syntax

 `phrase(term [, term]*)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Hinweise

Siehe auch  [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator).
  
    
    

### RANGE
<a name="fql_range_operator"> </a>

Verwenden Sie den Operator **RANGE** für numerische verwaltete Eigenschaften und verwaltete Eigenschaften für Datum/Uhrzeit.
  
    
    

#### Syntax

 `range(start, stop [,from="GE"|"GT"] [,to="LE"|"LT"])`
  
    
    

#### Parameter



|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _start_ <br/> | _<numeric_value>|<date/time_value>_ <br/> |Startwert für den Bereich  <br/> Verwenden Sie das reservierte Wort **min**, um anzugeben, dass der Bereich keine untere Grenze hat. <br/> |
| _stop_ <br/> | _<numeric_value>|<date/time_value>_ <br/> |Endwert für den Bereich  <br/> Verwenden Sie das reservierte Wort **max**, um anzugeben, dass der Bereich keine obere Grenze hat. <br/> |
| _from_ <br/> |**GE|GT** <br/> | Optionaler Parameter, der das öffnende oder schließende Startintervall angibt. <br/>  Gültige Werte: <br/> **GE** Größer als oder gleich dem Startwert (>= Start des Intervalls) <br/> **GT** Größer als der Startwert (> Start des Intervalls) <br/>  Standardwert: **GE** <br/> |
| _to_ <br/> |**LE|LT** <br/> | Optionaler Parameter, der das öffnende oder schließende Endintervall angibt <br/>  Gültige Werte: <br/> **LE** Kleiner oder gleich dem Endwert (<= Ende des Intervalls) <br/> **LT** Kleiner als der Endwert (< Ende des Intervalls) <br/>  Standardwert: **LT** <br/> |
   

#### Beispiele

Der folgende Ausdruck gleicht eine Beschreibungseigenschaft ab, die mit der Phrase „Olympische Spiele" beginnt, die in Elementen mit einer Größe von mindestens 10.000 Byte angezeigt wird.
  
    
    
 `and(size:range(10000, max), description:starts-with("olympic games"))`
  
    
    

### STARTS-WITH
<a name="fql_startswith_operator"> </a>

Gibt ein Wort oder eine Phrase an, die am Anfang einer verwalteten Eigenschaft angezeigt werden muss.
  
    
    

#### Syntax

 `starts-with(<term or phrase>)`
  
    
    

#### Parameter

Nicht zutreffend
  
    
    

#### Beispiele

Der folgende Ausdruck gleicht Elemente mit den Werten „Mr. Adam Jones" und „Adam Jones" in der verwalteten **author**-Eigenschaft ab. Er gleicht keine Elemente mit dem Wert „Herr Adam Jones" ab.
  
    
    
 `author:starts-with("adam jones")`
  
    
    

#### Hinweise

Weitere Informationen zum Begrenzungsabgleich finden Sie unter  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator).
  
    
    

### STRING
<a name="fql_string_operator"> </a>

Definiert eine boolesche Abgleichbedingung für eine Textzeichenfolge.
  
    
    
Der Operand ist eine Textzeichenfolge (ein oder mehrere Begriffe), die abgeglichen werden soll. Auf die Zeichenfolge folgt der Wert null, oder es folgen mehrere Parameter. 
  
    
    
Der Parameter **STRING** kann auch als Typumwandlung verwendet werden. Die Abfrage `string("24.5")` beispielsweise behandelt den numerischen Wert „24.5" als Textzeichenfolge.
  
    
    

#### Syntax

 `string("<text string>"`
  
    
    
 ` [, mode=<mode>]`
  
    
    
 ` [, n=<near>]`
  
    
    
 ` [, weight=<n>]`
  
    
    
 ` [, linguistics=<on|off>]`
  
    
    
 ` [, wildcard=<on|off>])`
  
    
    

#### Parameter



|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _mode_ <br/> | _<mode>_ <br/> | Der Parameter _mode_ gibt an, wie der Wert <text string> ausgewertet werden soll. Die folgende Liste enthält gültige Werte. <br/> **"PHRASE"** - `phrase(term [,term]*)` <br/> |**Modus**|**Äquivalenter Operatorausdruck**|
|:-----|:-----|
|**"PHRASE"** <br/> | `phrase(term [,term]*)` <br/> |
|**"AND"** <br/> | `and(term, term [,term]*)` <br/> |
|**"OR"** <br/> | `or(term, term [,term]*)` <br/> |
|**"ANY"** <br/> | `any(term, term [,term]*)` <br/> |
|**"NEAR"** <br/> | `near(term, term [,term]*, N)` <br/> |
|**"ONEAR"** <br/> | `onear(term, term [,term]*, N)` <br/> |
   
 Standardwert: **"PHRASE"** <br/> |
| _n_ <br/> | _<numeric_value>_ <br/> |Dieser Parameter gibt den maximalen Abstand eines Begriffs für  _mode_= **"NEAR"** oder _mode_= **"ONEAR"** an. <br/> Die folgenden Ausdrücke sind gleichwertig:  <br/>  `string("hello world", mode="NEAR", n=5)` <br/>  `near(hello, world, n=5)` <br/> Standardwert: **4** <br/> |
| _weight_ <br/> | _<numeric_value>_ <br/> |Dieser Parameter ist eine positive ganze Zahl, die die Begriffsgewichtung für eine dynamische Rangfolge angibt.  <br/> Ein niedrigerer Wert gibt an, dass ein Begriff in der dynamischen Rangfolge weniger relevant ist. Ein höherer Wert gibt an, dass ein Begriff in der Rangfolge relevanter ist. Der Wert null für den gewichteten Parameter gibt an, dass ein Begriff für die dynamische Rangfolge nicht relevant ist.  <br/> Der Parameter  _weight_ gilt für alle Begriffe im **STRING** -Ausdruck. <br/> > **TIPP**> Der weight-Parameter ist nur für Volltextindex-Abfragen relevant.           Standardwert: **100**. <br/> |
| _linguistics_ <br/> |**on|off** <br/> |Deaktiviert/aktiviert alle linguistischen Features für die Zeichenfolge (Lemmatisierung, Synonyme, Rechtschreibprüfung), sofern sie für die Abfrage aktiviert sind.  <br/> Mit diesem Parameter können Sie die linguistische Verarbeitung für einen bestimmten Begriff oder eine bestimmte Zeichenfolge ändern, während der Begriff oder die Zeichenfolge weiterhin für die Rangfolge relevant sein soll.  <br/> Standardwert: **"ON"** <br/> |
| _wildcard_ <br/> |**on|off** <br/> | Dieser Parameter steuert die Platzhaltererweiterung von Begriffen innerhalb von _<text string>_. Diese Einstellung setzt alle Platzhaltereinstellungen in Abfrageparametern außer Kraft und ermöglicht die Aktivierung oder Deaktivierung von erweiterten Platzhalterzeichen für bestimmte Teile der Abfrage.  <br/>  Die folgenden Werte sind gültig: <br/> **"ON"** Gibt an, dass das Zeichen „ *****" als Platzhalter ausgewertet wird. Das Zeichen „ *****" entspricht null oder mehr Zeichen.  <br/> **"OFF"** Gibt an, dass das Zeichen „ *****" nicht als Platzhalter ausgewertet wird.  <br/>  Standardwert: **"ON"** <br/> |
   

> **HINWEIS**
> In SharePoint 2013 gelten die Parameter  _minexpansion_,  _maxexpansion_ und _annotation_class_ für den Operator **STRING** als veraltet.
  
    
    


#### Beispiele

 **Beispiel 1**: Da der standardmäßige Zeichenfolgemodus **PHRASE** ist, gibt jeder der folgenden Ausdrücke die gleichen Ergebnisse zurück.
  
    
    
 `"what light through yonder window breaks"string("what light through yonder window breaks")string("what light through yonder window breaks", mode="phrase")phrase(what, light, through, yonder, window, breaks)`
  
    
    
 **Beispiel 2**: Der folgende Zeichenfolge-Tokenausdruck und der **AND** -Operatorausdruck geben die gleichen Ergebnisse zurück.
  
    
    
 `string("cat dog fox", mode="and")and(cat, dog, fox)`
  
    
    
 **Beispiel 3**: Der folgende Zeichenfolge-Tokenausdruck und der **OR** -Operatorausdruck geben die gleichen Ergebnisse zurück.
  
    
    
 `string("coyote saguaro", mode="or")or(coyote, saguaro)`
  
    
    
 **Beispiel 4**: Der folgende Zeichenfolge-Tokenausdruck und der **ANY** -Operatorausdruck geben die gleichen Ergebnisse zurück.
  
    
    
 `string("coyote saguaro", mode="any")any(coyote, saguaro)`
  
    
    
 **Beispiel 5**: Der folgende Zeichenfolge-Tokenausdruck und der **NEAR** -Operatorausdruck geben die gleichen Ergebnisse zurück.
  
    
    
 `string("coyote saguaro", mode="near")near(coyote, saguaro)`
  
    
    
 **Beispiel 6**: Der folgende Zeichenfolge-Tokenausdruck und der **NEAR** -Operatorausdruck geben die gleichen Ergebnisse zurück.
  
    
    
 `string("cat dog fox wolf", mode="near", N=4)near(cat, dog, fox, wolf, N=4)`
  
    
    
 **Beispiel 7**: Der folgende Zeichenfolge-Tokenausdruck und der **ONEAR** -Operatorausdruck geben die gleichen Ergebnisse zurück.
  
    
    
 `string("cat dog fox wolf", mode="onear")onear(cat, dog, fox, wolf)`
  
    
    
 **Beispiel 8**: Der folgende Zeichenfolge-Tokenausdruck gleicht das Wort „Adel" mit deaktivierten linguistischen Features ab, sodass andere Wortformen (zum Beispiel „adelnd") nicht über die Wortstammerkennung abgeglichen werden.
  
    
    
 `string("nobler", linguistics="off")`
  
    
    
 **Beispiel 9**: Der folgende Ausdruck gleicht Elemente ab, die entweder „Katze" oder „Hund" enthalten. Der Ausdruck erhöht jedoch die Position der Elemente, die den Begriff „Hund" enthalten, in der dynamischen Rangfolge stärker, als die von Elementen, die „Katze" enthalten.
  
    
    
 `or(string("cat", weight="200"), string("dog", weight="500"))`
  
    
    

#### Hinweise

 **Relevanzgewichtung für die dynamische Rangfolge**
  
    
    
Die Hauptauswirkung des Parameters **weight** betrifft **OR** -Abfragen. Er kann zudem eine gewisse Auswirkung auf **AND** -Abfragen haben. Der Algorithmus der dynamischen Rangfolge kann implizieren, dass verschiedene Begriffe eine unterschiedliche Relevanz in der Rangfolge haben, je nachdem, wo in dem Element die Begriffsübereinstimmung auftritt.
  
    
    
Dem Unterschied bei der Relevanz in der Rangfolge kann auch die Häufigkeit des Begriffs und die inverse Häufigkeit des Begriffs zugrunde liegen. Bei den folgenden Angaben handelt es sich um ein Beispiel:
  
    
    

- Abfrage:  `and(string("a"), string("b", weight=200))`
    
  
- Indexschema: Die verwaltete **title** -Eigenschaft hat eine höhere Gewichtung als die verwaltete **body** -Eigenschaft.
    
  
- Das Indexelement 1 enthält den Begriff „a" im Titel (title) und den Begriff „b" im Textkörper (body). 
    
  
- Das Indexelement 2 enthält den Begriff „a" im Textkörper (body) und den Begriff „b" im Titel (title). 
    
  
In diesem Beispiel erhält Element 2 die höchste Position in der Rangfolge, da die Elemente mit höherer Relevanz in der Rangfolge eine größere Verstärkung erhalten.
  
    
    

> **TIPP**
> Die relative Begriffsverstärkung (positiv oder negativ) wird auf die Komponente der dynamischen Rangfolge in der Gesamtrangfolge angewendet. Rangfolgeberechnungen mittels Näheverstärkung (Abstand zwischen Wörtern) sind nicht von der Begriffsgewichtung betroffen. Die relative Gewichtung impliziert nicht immer, dass die Gesamtrangfolge für das Element gemäß dem angegebenen Prozentsatz geändert wird. > Mit der folgenden Abfrage wird nach den Begriffen „Peter", „Paul" oder „Mary" gesucht, wobei „Peter" zweimal so viel Relevanz in der Rangfolge hat wie die beiden anderen Begriffe. >  `or(peter, string("paul mary", mode="OR", weight=50))`
  
    
    

 **Umgang mit Zeichenfolgen mit Sonderzeichen**
  
    
    
Sonderzeichen wie Komma („,"), Semikolon („;"), Doppelpunkt („:"), Punkt („."), Minuszeichen („-"), Unterstrich („_"), oder Schrägstrich („/") werden innerhalb eines Zeichenfolgeausdrucks, der in doppelte Anführungszeichen eingeschlossen ist, als Leerzeichen behandelt. Dieses Verhalten steht im Zusammenhang mit dem Vorgang der Tokenisierung. Diese Zeichen schließen zudem eine implizite Phrasierung der Token ein, die durch diese Zeichen voneinander getrennt sind. 
  
    
    
Die folgenden Abfrageausdrücke sind gleichwertig:
  
    
    
 `title:string("animals birds", mode="phrase")title:"animals/birds"title:string("animals/birds", mode="and")title:string("animals/birds", mode="or")`
  
    
    
Die folgenden Abfrageausdrücke sind gleichwertig:
  
    
    
 `title:or(string("animals birds", mode="phrase"), string("animals insects", mode="phrase"))title:string("animals/birds animals/insects", mode="or")`
  
    
    
Die folgenden Abfrageausdrücke sind gleichwertig:
  
    
    
 `body:string("help contoso com", mode="phrase")body:string("help@contoso.com")`
  
    
    
 **Phrasenabgleich mit Token**
  
    
    
Sie können nach einer exakten Tokenzeichenfolge mit dem Operator **STRING** mit _mode_="phrase" oder dem Operator **PHRASE** suchen.
  
    
    
Alle Suchphrasenvorgänge implizieren eine Phrasenzuordnung mit Token. Dies bedeutet, dass Sonderzeichen wie Komma („ **,** "), Semikolon („ **;** "), Doppelpunkt („ **:** "), Unterstrich („ **_** "), Minuszeichen („ **-** ") oder Schrägstrich („ **/** ") als Leerzeichen behandelt werden. Dieses Verhalten steht im Zusammenhang mit dem Vorgang der Tokenisierung.
  
    
    

### XRANK
<a name="fql_xrank_operator"> </a>

Verstärkt den dynamischen Rang von Elementen basierend auf bestimmten Vorkommnissen von Begriffen im  _match expression_, ohne die Abfrage dahingehend zu verändern, welche Elemente mit der Abfrage übereinstimmen. Ein **XRANK** -Ausdruck besteht aus einer Komponente, die abgeglichen werden muss, dem _match expression_ und einer oder mehreren Komponenten, die nur in der dynamischen Rangfolge relevant sind, dem _rank expression_. Es muss mindestens **einer** der Parameter angegeben werden, abgesehen von _n_, damit ein XRANK-Ausdruck gültig ist.
  
    
    
 _Match expressions_ kann ein beliebiger gültiger FQL-Ausdruck sein, einschließlich geschachtelter **XRANK** -Ausdrücke. _Rank expressions_ kann ein beliebiger gültiger FQL-Ausdruck ohne **XRANK**-Ausdrücke sein. Wenn Ihre FQL-Abfragen über mehrere **XRANK**-Operatoren verfügen, wird der endgültige dynamische Rangfolgewert als Summe der Verstärkungen aller **XRANK**-Operatoren berechnet.
  
    
    

> **HINWEIS**
> In SharePoint Server 2010 wies der **XRANK** -Operator zwei Parameter auf: _boost_ und _boostall_, sowie die folgende Syntax:  `xrank(operand, rank-operand [, rank-operand]* [,boost=n] [,boostall=yes])`. Diese Syntax zusammen mit ihren Parametern gilt in SharePoint Server 2013 als veraltet. Verwenden Sie stattdessen die neue Syntax und die neuen Parameter. 
  
    
    


#### Syntax

 `xrank(<match expression> [, <rank-expression>]*, rank-parameter[, rank-parameter]*)`
  
    
    

#### Formel


  
    
    
![Formel für XRANK-Operator](images/XRANKFormula.gif)
  
    
    

  
    
    

  
    
    

#### Parameter



|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _N_ <br/> | _<integer_value>_ <br/> |Gibt die Anzahl der Ergebnisse an, aus denen die Statistik berechnet werden soll.  <br/> Dieser Parameter hat keine Auswirkungen auf die Anzahl der Ergebnisse, die durch die dynamische Rangfolge hinzu kommen; es handelt sich dabei lediglich um eine Methode zum Ausschließen irrelevanter Elemente aus den Berechnungen der Statistik.  <br/> Standardwert: **0**. Ein Nullwert trägt die Semantik *aller Dokumente*  . <br/> |
| _Nb_ <br/> | _<float_value>_ <br/> |Der  _nb_-Parameter verweist auf die normale Verstärkung. Dieser Parameter gibt den Faktor an, der mit dem Produkt der Abweichung und der Durchschnittsbewertung der Rangwerte der Ergebnisgruppe multipliziert wird.  <br/>  _f_ in der XRANK-Formel. <br/> |
   
In der Regel ist die normale Verstärkung ( _nb_) der einzige Parameter, der geändert wird. Dieser Parameter stellt das benötigte Steuerelement bereit, um ein bestimmtes Element höher oder niedriger zu stufen, ohne dabei die Standardabweichung zu berücksichtigen. 
  
    
    

#### Erweiterte Parameter

Außerdem sind folgende erweiterte Parameter verfügbar. In der Regel werden sie jedoch nicht verwendet.
  
    
    


|**Parameter**|**Wert**|**Beschreibung**|
|:-----|:-----|:-----|
| _cb_ <br/> | _<float_value>_ <br/> |Der  _cb_-Parameter verweist auf die konstante Verstärkung.  <br/> Standardwert: **0**. <br/>  _a_ in der XRANK-Formel. <br/> |
| _stdb_ <br/> | _<float_value>_ <br/> |Der  _stdb_-Parameter verweist auf die Verstärkung der Standardabweichung.  <br/> Standardwert: **0**. <br/>  _e_ in der XRANK-Formel. <br/> |
| _avgb_ <br/> | _<float_value>_ <br/> |Der  _avgb_-Parameter verweist auf die durchschnittliche Verstärkung. Dieser Faktor wird mit dem Durchschnitt der Rangfolgewerte in der Ergebnismenge multipliziert.  <br/> Standardwert: **0**. <br/>  _d_ in der XRANK-Formel. <br/> |
| _rb_ <br/> | _<float_value>_ <br/> |Der  _rb_-Parameter verweist auf die Bereichsverstärkung. Dieser Faktor wird mit dem Bereich der Rangfolgewerte in der Ergebnisgruppe multipliziert.  <br/> Standardwert: **0**. <br/>  _b_ in der XRANK-Formel. <br/> |
| _pb_ <br/> | _<float_value>_ <br/> |Der  _pb_-Parameter verweist auf die Prozentwertverstärkung. Dieser Faktor wird im Vergleich zum Mindestwert des Korpus mit der Rangfolge des Elements multipliziert.  <br/> Standardwert: **0**. <br/>  _c_ in der XRANK-Formel. <br/> |
   

#### Beispiele

 **Beispiel 1.** Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält. Der Ausdruck erhöht die dynamische Rangfolge der Elemente mit einer konstanten Verstärkung von 100 für Elemente, die zusätzlich den Begriff „Vollblutpferd" enthalten.
  
    
    
 `xrank(or(cat, dog), thoroughbred, cb=100)`
  
    
    
 **Beispiel 2.** Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält. Der Ausdruck erhöht die dynamische Rangfolge der Elemente mit einer normalen Verstärkung von 1,5 für Elemente, die zusätzlich den Begriff „Vollblutpferd" enthalten.
  
    
    
 `xrank(or(cat, dog), thoroughbred, nb=1.5)`
  
    
    
 **Beispiel 3.** Der folgende Ausdruck gleicht Elemente ab, bei denen der Standard-Volltextindex den Begriff „Katze" oder „Hund" enthält. Der Ausdruck erhöht die dynamische Rangfolge der Elemente mit einer konstanten Verstärkung von 100 und einer normalen Verstärkung von 1,5, für Elemente, die zusätzlich den Begriff „Vollblutpferd" enthalten.
  
    
    
 `xrank(or(cat, dog), thoroughbred, cb=100, nb=1.5)`
  
    
    
 **Beispiel 4.** Der folgende Ausdruck gleicht alle Elemente ab, die den Begriff „Tiere" enthalten und die dynamische Rangfolge wie folgt verstärken:
  
    
    

- Die dynamische Rangfolge von Elementen, die den Begriff „Hunde" enthalten, werden um 100 Punkte verstärkt.
    
  
- Die dynamische Rangfolge der Elemente, die den Begriff „Katzen" enthalten, werden um 200 Punkte verstärkt.
    
  
- Die dynamische Rangfolge der Elemente, die die Begriffe „Hunde" und „Katzen" enthalten, werden um 300 Punkte verstärkt.
    
  
 `xrank(xrank(animals, dogs, cb=100), cats, cb=200)`
  
    
    

## Zusätzliche Ressourcen
<a name="SP15FQL_addlresources"> </a>


-  [Erstellen von Suchabfragen in SharePoint 2013](building-search-queries-in-sharepoint-2013.md)
    
  
