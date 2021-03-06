---
title: Syntax für Trigger Ausdrücken in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: d50536f6-d02e-4060-9286-634dfd06206c
---


# Syntax für Trigger Ausdrücken in SharePoint 2013
Informationen Sie zu Trigger-Ausdrücke, die Sie zum Erstellen von Trigger Conditions so konfigurieren Sie das Webdienst-Callout in SharePoint Server 2013 verwenden können.
## Elemente, die in der Syntax von Ausdrücken Trigger verwendet
<a name="SP15triggerex_elements"> </a>

Elemente, die in einem Ausdruck für Trigger verwendet werden können, sind:
  
    
    

- Operatoren
    
  
- Verwaltete Eigenschaft Wert Zugriff
    
  
- Literale
    
  
- Funktionen
    
  
- Constants
    
  

> **HINWEIS**
> Die Zeichenfolge " **Null**" ist für den Wert **Null**reserviert.
  
    
    


## Operatoren in der Syntax trigger
<a name="SP15triggerex_operators"> </a>

Tabelle 1 beschreibt die Operatoren, die von der Sprache des Trigger-Ausdruck, mit der Reihenfolge der Priorität von der höchsten zur niedrigsten wird unterstützt. Operatoren in der gleichen Kategorie haben dieselbe Rangfolge. Mehrere Operatoren haben zwei Versionen von deren Syntax.
  
    
    

**In Tabelle 1. Operatoren für Trigger Ausdruck-Syntax unterstützt**


|**Kategorie**|**Ausdruck**|**Beschreibung**|
|:-----|:-----|:-----|
|Unärer Operator <br/> |- <br/> !, NICHT <br/> |Arithmetische negation <br/> Logische negation <br/> |
|Mit <br/> |* <br/> / <br/> %, mod <br/> |Multiplikation <br/> Division <br/> Rest <br/> |
|Additive <br/> |+ <br/> - <br/> &amp; <br/> |Addition <br/> Subtraktion <br/> Zeichenfolgenverknüpfung <br/> |
|Relationale <br/> |=, == <br/> ! = <> <br/> < <br/> > <br/> <= <br/> >= <br/> |Ist gleich <br/> Ungleichheit <br/> Kleiner als <br/> Größer als <br/> Kleiner oder gleich <br/> Größer als oder gleich <br/> |
|Logisches UND <br/> |&amp; &amp;, AND <br/> |Logisches UND <br/> |
|Logisches ODER <br/> |||, ODER <br/> |Logisches ODER <br/> |
   

## Verwaltete Eigenschaftenzugriff in Trigger Ausdrücken
<a name="SP15triggerex_managed"> </a>

Verwaltete Eigenschaften in der durchforsteten Elemente werden nach ihren Namen verwiesen; der Name ist nicht in Anführungszeichen ("") und die Groß-/Kleinschreibung beachtet wird.
  
    
    

## Literale in Ausdrücken trigger
<a name="SP15triggerex_literals"> </a>

Die folgenden Datentypen können als Literale ausgedrückt werden: **String**, **Int32**, **Double**und **Boolean**.
  
    
    

## Funktionen in Ausdrücken trigger
<a name="SP15triggerex_functions"> </a>

Breites Zusammenstellung von Funktionen von mathematische Funktionen wie  `Floor` bis hin zu den Funktionen für die Verwendung mit bestimmten Datentypen, wie etwa `Lists`. Sie können diese Funktionen einzeln oder schachteln Sie sie.
  
    
    

-  `bool? ListContains<T>(IList<T> list, T obj)`
    
  
-  `int? Count<TElement>(IList<TElement> list)`
    
  
-  `TElement Item<TElement>(IList<TElement> list, int? index)`
    
  
-  `bool IsInsideRange(DateTime? date, long? fromTicks, long? toTicks)`
    
  
-  `DateTime Now()`
    
  
-  `DateTime? ToDate(string date, string format)`
    
  
-  `int? Day(DateTime? date)`
    
  
-  `int? DayOfWeek(DateTime? date)`
    
  
-  `int? DayOfYear(DateTime? date)`
    
  
-  `int? GetDatePart(DateTime? date, DatePartConstant datePartConstant)`
    
  
-  `int? Hour(DateTime? date)`
    
  
-  `int? Minute(DateTime? date)`
    
  
-  `int? Month(DateTime? date)`
    
  
-  `int? Quarter(DateTime? date)`
    
  
-  `int? Second(DateTime? date)`
    
  
-  `int? Year(DateTime? date)`
    
  
-  `long? GetDateDiff(DateTime? occursFirst, DateTime? occursLast, DatePartConstant datePartConstant)`
    
  
-  `string Extension(string arg)`
    
  
-  `string FileName(string arg)`
    
  
-  `string FileName(string arg, bool? excludeExtension)`
    
  
-  `bool IsNull(object value)`
    
  
-  `bool? IsDate(string input, string format)`
    
  
-  `object IfThenElse(bool? condition, object thenBranch, object elseBranch)`
    
  
-  `decimal? Ceiling(decimal? number)`
    
  
-  `decimal? Floor(decimal? number)`
    
  
-  `double? Ceiling(double? number)`
    
  
-  `double? Floor(double? number)`
    
  
-  `double? Sqrt(double? number)`
    
  
-  `bool? Contains(string arg, string contained)`
    
  
-  `bool? EndsWith(string arg, string suffix)`
    
  
-  `bool? IsMatch(string input, string pattern)`
    
  
-  `bool? IsMatch(string input, string pattern, int? start, RegexOptionConstant options)`
    
  
-  `bool? IsMatch(string input, string pattern, RegexOptionConstant options)`
    
  
-  `bool? IsNullOrEmpty(string input)`
    
  
-  `bool? StartsWith(string arg, string prefix)`
    
  
-  `int? IndexOf(string arg, string toFind)`
    
  
-  `int? IndexOfRegex(string input, string regex)`
    
  
-  `int? LastIndexOf(string arg, string toFind)`
    
  
-  `int? Length(string arg)`
    
  
-  `string Match(string input, string pattern)`
    
  
-  `string Match(string input, string pattern, int? start, int? length, RegexOptionConstant options)`
    
  
-  `string Match(string input, string pattern, int? start, RegexOptionConstant options)`
    
  
-  `string Match(string input, string pattern, RegexOptionConstant options)`
    
  
-  `string Substring(string arg, int? start)`
    
  
-  `string Substring(string arg, int? start, int? length)`
    
  
-  `string ToLower(string arg)`
    
  
-  `string Trim(string value)`
    
  

## Konstanten in Ausdrücken trigger
<a name="SP15triggerex_constants"> </a>

Es gibt zwei Sätze von Konstanten, die mit bestimmten Funktionen verwendet werden können: **DatePartConstant** und **RegexOptionConstant**. Tabelle 2 sind die beiden Beispiele der folgenden Konstanten und, in dem Sie sie verwenden können.
  
    
    

**In Tabelle 2. Trigger Ausdruck Konstanten und Verwendung von Diensten in SharePoint 2013**


|**Gruppe von Konstanten**|**Beispiele**|**Verwendung**|
|:-----|:-----|:-----|
|**DatePartConstant** <br/> |**Day**, **Month**, **Year**, **Hour**, **Minute**, **Second**. <br/> |Mit der **GetDatePart** -Funktion <br/> |
|**RegexOptionConstant** <br/> |**IgnoreCase** <br/> |Mit den Funktionen **IsMatch**, **Match**, **ReplaceRegex**und **IndexOfRegex**. <br/> |
   

## Zusätzliche Ressourcen
<a name="SP15triggerex_addresources"> </a>


-  [Benutzerdefinierte Inhaltsverarbeitung mit dem Webdienstpopup zur Inhaltsanreicherung](custom-content-processing-with-the-content-enrichment-web-service-callout.md)
    
  
-  [Vorgehensweise: verwenden die Anreicherung Web Service als Legende für SharePoint Server](how-to-use-the-content-enrichment-web-service-callout-for-sharepoint-server.md)
    
  

  
    
    

