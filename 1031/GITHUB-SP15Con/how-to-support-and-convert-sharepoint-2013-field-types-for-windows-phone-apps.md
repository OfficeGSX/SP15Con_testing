---
title: Vorgehensweise Support- und konvertieren SharePoint 2013 FieldTypes für Windows Phone-apps
ms.prod: SHAREPOINT
ms.assetid: 301e6e58-5153-4ca9-a419-8ae0535ebbed
---


# Vorgehensweise: Support- und konvertieren SharePoint 2013 FieldTypes für Windows Phone-apps
Implementieren Sie die Datenkonvertierungslogik, um SharePoint-Feldtypen in Windows Phone-Apps zu unterstützen.
In Projekten basierend auf die Vorlage Windows Phone SharePoint List Application die Daten der viele SharePoint-Feldtypen verarbeitet und vom Standard Konvertierungslogik, eignet sich für die Anzeige und Bearbeitung in der Silverlight-Benutzeroberfläche von einer Windows Phone koordiniert, jedoch Entwickler können auch ihre eigenen benutzerdefinierten Datenverarbeitung Routinen implementieren.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie anstelle von Visual Studio 2010 Express Visual Studio Express 2012 verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung Erstellen von apps für Windows Phone 8 und Windows Phone 7.<br/> Weitere Informationen finden Sie unter  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## SharePoint-Feldtypen in Windows Phone-apps
<a name="BKMK_SharePointFieldTypes"> </a>

SharePoint-Listen werden darstellten nach Feldern (in Spalten angeordnet sind), und ein bestimmtes Feld wird als Daten eines bestimmten Typs (d. h., Daten auf eine bestimmte Weise strukturiert) enthalten. Diese Typen werden Feldtypenbezeichnet. (Solche Typen können auch Spaltentypenbezeichnet werden, da Sie, wenn Sie eine SharePoint-Liste eine Spalte hinzufügen, mit einem bestimmten Typ von Daten verknüpften Felder in eine Spalte hinzufügen möchten.) Diese Felder sind durch ein XML-Schema definiert. Das Schema für ein Feld namens "Bestelldatum" mit einem **DateTime** Datentyp (dargestellt als Feld Datum und Uhrzeit in der Benutzeroberfläche von einer Microsoft SharePoint Server ) kann wie folgt aussehen.
  
    
    

```XML

<Field Type="DateTime" DisplayName="Order Date" Required="FALSE"
 EnforceUniqueValues="FALSE" Indexed="FALSE" Format="DateOnly"
 FriendlyDisplayFormat="Disabled" Name="Order_x0020_Date">
  <Default>[today]</Default>
  <DefaultFormulaValue>2012-01-10T00:00:00Z</DefaultFormulaValue>
</Field>
```

Beachten Sie insbesondere den Wert des **Type** -Attributs des angegebenen Schemas-Elements **Field** hier als "DateTime". Listenfelder erstellt, um Daten auf andere Weise strukturiert enthalten möglicherweise mit einem **Type** -Wert der sag, "Choice" oder "Text" oder "Boolean" festgelegt werden.
  
    
    
SharePoint-Feldtypen können nicht direkt an die Silverlight-Steuerelemente in einer Windows Phone-app gebunden werden. Die Daten in der SharePoint-Liste unverändert müssen vorbereitet oder auf eine bestimmte Weise (oder in der standard-Terminologie Silverlight Bindung, konvertiert), um auf die Steuerelemente in der app gebunden werden verarbeitet und in Vorbereitung und Koordinierung wird durch die ViewModels in Projekten erstellt aus der Vorlage für Windows Phone SharePoint List Application behandelt. Projekte, die auf dieser Vorlage basierende dienen zum standardmäßigen Konvertierungslogik zur Unterstützung der Bindung und Anzeigen von SharePoint-Daten in einer Windows Phone-app für eine Reihe von standardmäßigen SharePoint-Feldtypen (oder für benutzerdefinierte Felder, die basierend auf eine der folgenden Standardtypen erstellt) enthält. Mit der standardmäßigen Konvertierungslogik unterstützten die Feldtypen werden in Tabelle 1 aufgeführt.
  
    
    

**In Tabelle 1. Feldtypen Vorschriften für die Standard-Konvertierung**


|**SharePoint-Feldtyps**|**Silverlight-Datentyp**|
|:-----|:-----|
|Anhänge <br/> |Datei <br/> |
|Boolescher Wert <br/> |Boolescher Wert <br/> |
|Berechnet (nur zur Anzeige) <br/> |String <br/> |
|Auswahl <br/> |String <br/> |
|**Währung** <br/> |Numeric <br/> |
|DateTime <br/> |Datum (nach Gebietsschema des Telefons dargestellt) <br/> |
|URL <br/> |Link <br/> |
|Ganzzahl <br/> |Numerisch <br/> |
|Ort <br/> |GeoCoordinate <br/> |
|Verweis <br/> |Zeichenfolge <br/> |
|MultiChoice <br/> |String <br/> |
|Hinweis <br/> |Zeichenfolge <br/> |
|Zahl <br/> |Numerisch <br/> |
|OutcomeChoice <br/> |**String** <br/> |
|Bild <br/> |Link <br/> |
|Text <br/> |String <br/> |
|Benutzer <br/> |String <br/> |
   
Andere SharePoint-Feldtypen wie **Guid** -Felder können in Windows Phone-apps verwendet werden, jedoch müssen Entwickler benutzerdefinierte Konvertierungslogik zur Unterstützung der Bindung und Anzeigen von Werten für diese Feldtypen für welche keinen, die Standardwert Konvertierungslogik angegeben wurde bereitstellen. (Siehe [Benutzerdefinierte Konvertierungslogik zum Bereitstellen von nicht unterstützte Feldtypen](#BKMK_ConversionForUnsupportedFields) in diesem Artikel).
  
    
    
Zum veranschaulichen, wie die Vorlage Standard Konvertierung und Support bietet für bestimmte Feldtypen, nehmen Sie an einer SharePoint Liste eine Spalte von Feldern mit dem Namen "Produktkategorie" mit einem **Choice** festgelegt und mehrere Optionen für "Freizeit" und "Culinary" zugeordnet sind. Das Schema für ein solches Feld auf dem Server möglicherweise das folgende Markup ähneln.
  
    
    



```XML

<Field Type="Choice" DisplayName="Product Category" Required="FALSE"
 EnforceUniqueValues="FALSE" Indexed="FALSE" Format="Dropdown"
 FillInChoice="TRUE" 
 Name="Product_x0020_Category">
  <Default>Recreation</Default>
  <CHOICES>
    <CHOICE>Culinary</CHOICE>
    <CHOICE>Recreation</CHOICE>
    <CHOICE>Sartorial</CHOICE>
    <CHOICE>Travel</CHOICE>
    <CHOICE>Other</CHOICE>
  </CHOICES>
</Field>
```

Dieses Feld **Choice** muss verarbeitet werden, um für die Anzeige in der Windows Phone-Benutzeroberfläche geeignet sein. In diesem Fall werden die Daten in das Feld als Zeichenfolge (z. B. "Freizeit") in einer Auflistung von Zeichenfolgenwerten (insbesondere als einen der Werte der **FieldValuesAsText** -Eigenschaft eines Objekts **ListItem** ) dargestellt. Die Konvertierungslogik für **Choice** Felder extrahiert diese Zeichenfolge für die Anzeige auf dem Telefon-Benutzeroberfläche. Die Zeichenfolge kann an und in einem **TextBlock** -Steuerelement in einem Formular angezeigt werden. Wenn der Wert für die Bearbeitung dargestellt wird, extrahiert die standardmäßige Konvertierungslogik für **Choice** Felder die verfügbaren Optionen für das Feld ("Kulinarischen", "Freizeit", "Sartorial" usw.) aus dem XML-Schema, das das Feld definiert und stellt den verfügbaren "" eine Auflistung (insbesondere als eine Art der Auflistung, die basierend auf der **ObservableCollection(T)** -Klasse Optionen) von Objekten, die sich selbst die Option für bestimmte (z. b enthalten sind "Kulinarischen") und gibt an, ob diese Option ausgewählt wurde. Diese Vorgänge werden alle in der ViewModel-Schicht der app behandelt. In der Ansicht (oder Präsentation) Ebene (die in der XAML-Datei für das Formular Bearbeiten von der Windows Phone SharePoint List Application Vorlage generiert wird,), diese Optionen sind standardmäßig als Silverlight **RadioButton** Steuerelemente in einem **ListBox** Steuerelement gerendert.
  
    
    

## Benutzerdefinierte Konvertierung für SharePoint-Feldtypen
<a name="BKMK_CustomConversion"> </a>

In Visual Studio-Projekten, die auf die Vorlage Windows Phone SharePoint List Application basieren, wurde der Mechanismus für die Behandlung der Koordination und die Konvertierung von Daten zwischen SharePoint und die Windows Phone Silverlight-Benutzeroberfläche für die Flexibilität und Erweiterbarkeit.
  
    
    
Je nach den Absichten Design für Ihre Anwendung möglicherweise bieten Konvertierungslogik zur Unterstützung der Bindung und Anzeigen von SharePoint-Feldtypen, die nicht mit der Konvertierungslogik standardmäßig bereitgestellt werden soll, oder Sie möchten Daten für ein Feld unterstützten auf die Weise anzuzeigen, die von der Standard-Implementierung unterscheidet.
  
    
    
Projekten basierend auf die Vorlage Windows Phone SharePoint List Application implementieren Sie eine statische **Converter** -Klasse, die für das Registrieren von Methoden für die Verarbeitung der Konvertierung Vorgänge speziell für die angegebenen Daten Datentypen Routinen enthält. Das Projekt enthält und Daten Konvertierungsroutinen für bestimmte Datentypen standardmäßig registriert. Der Registrierungsmechanismus verwendet Delegaten, um für die Erweiterbarkeit des zulassen. Entwickler können eigene Funktionen zum Bereitstellen der Logik für die Konvertierung von Daten aus diesem Grund schreiben und diese benutzerdefinierten Funktionen aufgerufen werden können, wenn die Stellvertretungen anstelle der Standardfunktionen aufgerufen werden. Registrieren Sie Ihre Funktionen, anordnen für benutzerdefinierte Funktionen für die Konvertierung Datenvorgänge aufgerufen werden, um mit der **Converter** -Klasse mithilfe der Registrierungsmethoden der Klasse. Die Registrierung, die Methoden spezifisch für jede ViewModel sind, die Möglichkeit zu berücksichtigen, die möglicherweise zu implementieren und registrieren verschiedene Funktionen zum Verarbeiten Daten unterschiedlich abhängig, z. B., ob die Daten präsentiert zur Bearbeitung oder zur Anzeige nur (ohne bearbeiten).
  
    
    

> **TIPP**
> Das Währungssymbol dargestellt, in dem Anzeigeformular ist aus SharePoint-Gebietsschema, auch wenn das Windows Phone-Gebietsschema unterscheidet. Entwickler können dieses Verhalten mithilfe von **Converter** Objekte anpassen.
  
    
    

Um Daten Konvertierungsfunktionen für das Formular anzeigen (DisplayForm.xaml) zu registrieren, verwenden Sie die **RegisterDisplayFieldValueConverter** -Methode der **Converter** -Klasse. Zum Registrieren von Funktionen für das Formular bearbeiten (EditForm.xaml) verwenden Sie die **RegisterEditFieldValueConverter** -Methode, und verwenden Sie die **RegisterNewFieldValueConverter** -Methode für das neue Formular (NewForm.xaml).
  
    
    
Konvertierungsfunktionen, die Daten verarbeiten stammt aus der Liste dargestellt werden in der Benutzeroberfläche können Sie registrieren (d. h., Funktionen, mit denen bestimmt wie auf **Daten** ) und können Sie Funktionen, die Daten aus der Benutzeroberfläche als gespeicherten in der Liste auf dem Server verarbeitet registrieren (Funktionen, die bestimmen, wie Daten **festlegen** ).
  
    
    
Die Funktionen **erhalten möchten**, müssen die Signatur der folgenden Delegatdeklaration in der Klasse **Converter** übereinstimmen.
  
    
    



```cs

public delegate object GetConvertedFieldValue(string fieldName,
  ListItem item, ConversionContext context);
```

Die Funktionen **festlegen**, müssen die Signatur der der Delegatdeklaration übereinstimmen.
  
    
    



```cs

public delegate void SetConvertedFieldValue(string fieldName,
  object fieldValue, ListItem item, ConversionContext context);
```

Die **RegisterDisplayFieldValueConverter** -Methode akzeptiert eine **get** -Funktion nur, da die **DisplayItemViewModel** -Klasse, wie entwickelt, anzeigen, jedoch nicht bearbeiten von Daten vorgesehen ist. Die **RegisterEditFieldValueConverter** und die **RegisterNewFieldValueConverter** -Methoden sind **get** -Funktion, einer Funktion **festgelegt** oder beides Annahme überlastet.
  
    
    
In  [Vorgehensweise: Implementieren von Validierung von Business Logik und die Daten in einem Windows Phone-app für SharePoint 2013](how-to-implement-business-logic-and-data-validation-in-a-windows-phone-app-for-s.md)wurde eine Überprüfungsroutine entwickelt, um zu überprüfen, die Format oder Telefon Zahlen, die durch den Benutzer einer Windows Phone-App bereitgestellt. Um die Konvertierung benutzerdefinierte Daten in das folgende Codebeispiel veranschaulichen implementieren wir **Abrufen** und **Festlegen von** Funktionen zum Behandeln Telefon Datentyp Zahl in eine ganz bestimmte Weise und registrieren diese Funktionen in die bearbeiten und neue Formulare mit der **Converter** -Klasse verwendet werden.
  
    
    
Wird davon ausgegangen Sie, die aus Gründen der im folgenden Codebeispiel wird eine Windows Phone SharePoint-Listen-app basierend auf einer Bestellungen Liste erstellt mithilfe der Vorlage für benutzerdefinierte Listen auf dem Server erstellt haben. Wird davon ausgegangen Sie, dass die Liste ein Feld namens "Telefonnummer" hat und dieses Feld als **Text** Feld in der Liste vorgesehen ist. In der Standardkonfiguration der eines Listenfelds als einen Typ **Text** auf eine SharePoint Server kann ein Benutzer alle Textzeichen (bis zu 255 Zeichen) eingeben. Die Vorlage stellt Standard Konvertierungslogik zum Anzeigen und Bearbeiten von Daten aus **Text** Felder in SharePoint, aber ein **Text** -Feld (standardmäßig) ist nicht die Struktur bedingen oder bestimmte Formatierung anzeigen, wie etwa möglicherweise Regel angewendet werden an externe Telefonnummern. In Ihrer Windows Phone-app sollten Sie Telefonnummern, die Benutzern angezeigt einheitlich und, wenn in der Liste auf dem Server gespeichert formatiert werden auf eine bestimmte Weise, obwohl die zugrunde liegenden Feldtyps ( **Text**) nicht bestimmte Formatierungsregeln zugeordnet ist. Wenn Ihre bestimmten Regeln für die Formatierungen anwenden möchten, können Sie eigene benutzerdefinierte Daten Konvertierungslogik anstelle der Standardlogik für einen unterstützten Feldtyp implementieren.
  
    
    

### Implementieren der Konvertierung benutzerdefinierte Daten


1. Wenn Sie auf eine SharePoint Server, die ein **Text** Feld mit dem Namen "Telefonnummer" (wie in [Vorgehensweise: Implementieren von Validierung von Business Logik und die Daten in einem Windows Phone-app für SharePoint 2013](how-to-implement-business-logic-and-data-validation-in-a-windows-phone-app-for-s.md)verwendeten Bestellungen Beispielliste) enthält eine Liste erstellt haben, erstellen Sie eine Windows Phone-app mithilfe der Windows Phone SharePoint List Application-Vorlage in Visual Studio mithilfe der in  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md)beschriebenen Schritte.
    
  
2. Wählen Sie im **Projektmappen-Explorer** den Knoten ab, das Projekt (mit dem Namen, beispielsweiseContosoSPListApp).
    
  
3. Wählen Sie im Menü **Projekt** in Visual Studio (oder Visual Studio Express für Windows Phone) **Klasse hinzufügen** aus. Das Dialogfeld **Neues Element hinzufügen** wird geöffnet, mit der Vorlage C#- **Klasse**, die bereits ausgewählt.
    
  
4. Geben Sie einen Namen für die Klassendatei (beispielsweise ContosoConverter.cs), und wählen Sie **Hinzufügen**. Die Klassendatei ist dem Projekt hinzugefügt und zur Bearbeitung geöffnet.
    
  
5. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:
    
  ```cs
  
using System;
using Microsoft.SharePoint.Client;  // Added for ListItem.
using Microsoft.SharePoint.Phone.Application; // Added for ConversionContext.
using System.Text.RegularExpressions;

// Specify a namespace appropriate for your particular project.
namespace ContosoSPListApp
{
    public static class ContosoConverter
    {
        static Regex StandardNumberFormat = 
          new Regex(@"^\\(?([0-9]{3})\\)?[-. ]?([0-9]{3})[-. ]?([0-9]{4})$", RegexOptions.Compiled);

        public static object GetConvertedTextFieldValue(string fieldName, 
          ListItem item, ConversionContext context)
        {
            if (item == null) return null;

            if (fieldName == "Contact_x0020_Number")
            {
                string contactNumber = string.Empty;
                try
                {
                    contactNumber = item.FieldValuesAsText[fieldName];
                }
                catch (PropertyOrFieldNotInitializedException)
                {
                    object itemValue = item[fieldName];
                    if (itemValue != null)
                    {
                        contactNumber = itemValue.ToString();
                    }
                }

                // Regularize the formatting of phone number for display in UI.
                if (StandardNumberFormat.IsMatch(contactNumber))
                {
                    // Phone number is in an acceptable format, but formatting it
                    // in a specific way for visual consistency in the UI.
                    string properlyFormattedNumber = 
                      StandardNumberFormat.Replace(contactNumber, "($1) $2-$3");
                    return properlyFormattedNumber;
                }
                else
                {
                    // Return a representation of the data adorned in such a way 
                    // as to designate its invalidity.
                    if (!contactNumber.Contains("Invalid Number"))
                    {
                        return string.Format("Invalid Number: {0}", contactNumber);
                    }
                    else
                    {
                        // Assume data is already adorned with an invalidity designation.
                        return contactNumber;
                    }
                }
            }
            else
            {
                return item[fieldName];
            }
        }

        public static void SetConvertedTextFieldValue(string fieldName, 
                             object fieldValue, ListItem item, ConversionContext context)
        {
            if (fieldValue == null) return;

            if (fieldName == "Contact_x0020_Number")
            {
                // Conventional formats (e.g., 555-555-5555) are acceptable,
                // but formatting phone numbers consistently here for storage in list on Server.
                string contactNumber = (string)fieldValue;

                if (StandardNumberFormat.IsMatch(contactNumber))
                {
                    string properlyFormattedNumber = StandardNumberFormat.Replace
                                                               (contactNumber, "($1) $2-$3");
                    item[fieldName] = properlyFormattedNumber;
                }
                else
                {
                    if (!contactNumber.Contains("Invalid Number"))
                    {
                        item[fieldName] = string.Format("Invalid Number: {0}", contactNumber);
                    }
                    else
                    {
                        // Assume data is already adorned with an invalidity designation.
                        item[fieldName] = contactNumber;
                    }                    
                }
            }
            else
            {
                // Allow values for other Text fields to be passed on to list without modification.
                item[fieldName] = fieldValue;                
            }
        }
    }
}
  ```

6. Speichern Sie die Datei.
    
  
Die **GetConvertedTextFieldValue** -Funktion bestimmt, ob die Zeichenfolge, die Daten aus einem Feld für die direkte Verwendung eine Telefonnummer (namens "Telefonnummer" in diesem Beispiel) enthält entsprechend der standard-Konventionen für Telefonnummern (in Nordamerika) formatiert ist, und, wenn dies der Fall ist, die Zahl in einem bestimmten Format für die Anzeige "(XXX) XXX-XXXX konvertiert". Wenn die Daten nicht als standard Telefonnummer formatiert ist, wird es mit einem Kennzeichner vorangestellt. Diese Funktion wird nicht tatsächlich die Daten in der Liste ändern. Die Funktion **SetConvertedTextFieldValue** wird in entgegengesetzter Richtung auf der anderen Seite fortgesetzt. Den Wert der Daten für ein Feld vom Benutzer eingegeben wird bestimmt, ob die angegebenen Daten das Muster für Telefonnummern standard oder nicht entspricht überprüft. Ist dies der Fall ist, wird der angegebene Wert in der Liste auf dem Server gespeichert werden sollen ein spezifisches Format konvertiert. Der Wert mit einem Kennzeichner vorangestellt ist, ist der angegebene Wert nicht in ein Standardformat, und klicken Sie dann als Präfix Wert ist auf dem Server gespeichert.
  
    
    
Es bleibt jetzt um diese Daten Konvertierungsfunktionen mit der **Konverter** -Klasse für die Verwendung in den Formularen bearbeiten und neu zu registrieren. Sie können an mehreren Stellen die Konverter registrieren. Im folgenden Verfahren werden die Konverter in das **OnNavigatedTo** -Ereignis des Listenformulars (List.xaml) registriert. Das Listenformular erstellt und navigiert, bevor das Bearbeiten und das neue Formular in der app instanziiert werden, damit die in diesem Ereignis in der Listenform registriert Konverter für Textfelder in allen Formularen wirksam werden.
  
    
    

### Registrieren Sie die Daten Konvertierungsfunktionen


1. Klicken Sie im **Projektmappen-Explorer** für die dasselbe Projekt, in der Sie die Klasse im vorherigen Verfahren erstellt haben, wählen Sie die List.xaml-Datei unter dem Knoten **Ansichten**.
    
  
2. Taste(n)F7um die zugeordneten Code-Behind-Datei List.xaml.cs, zur Bearbeitung zu öffnen.
    
  
3. Fügen Sie die folgende Variablendeklaration private an den Anfang des Codeblocks, die nach der öffnenden Klammer in der Codeblock und vor dem  `ListForm()` Konstruktor die partiellen Klasse **ListForm** implementiert wird.
    
  ```cs
  
private bool _isConversionRegistered;
  ```

4. Fügen Sie die folgende Methode, **RegisterTextFieldValueConverters**, an der Datei, in der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die partiellen Klasse **ListForm** implementiert.
    
  ```cs
  private void RegisterTextFieldValueConverters()
{
    Converter.RegisterEditFieldValueConverter(FieldType.Text, 
                      ContosoConverter.GetConvertedTextFieldValue, 
                        ContosoConverter.SetConvertedTextFieldValue);

    Converter.RegisterNewFieldValueConverter(FieldType.Text, 
                          ContosoConverter.GetConvertedTextFieldValue, 
                          ContosoConverter.SetConvertedTextFieldValue);
}
  ```


Diese Methode ruft einfach die entsprechende Registrierungsmethoden der **Converter** -Klasse. Für diesen Code wird vorausgesetzt, dass die benutzerdefinierte Klasse, die im vorhergehenden Verfahren erstellten **Abrufen** und **Festlegen von** Funktionen mit dem Namen "ContosoConverter". Wenn Sie einen anderen Namen für die Klasse angegeben haben, ändern Sie den Code hier entsprechend anpassen.
    
  
5. Ändern Sie die Implementierung der **OnNavigatedTo** -Ereignishandler in der Datei, indem Sie eine Prüfung auf den Wert des Flags **_isConversionRegistered** und einen Anruf an die **RegisterTextFieldValueConverters** -Funktion, die im vorherigen Schritt hinzugefügt hinzufügen. Der geänderte Handler sollte wie folgt aussehen.
    
  ```cs
  
protected override void OnNavigatedTo(System.Windows.Navigation.NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    App.MainViewModel.ViewDataLoaded += 
      new EventHandler<ViewDataLoadedEventArgs>(OnViewDataLoaded);
    App.MainViewModel.InitializationCompleted += 
      new EventHandler<InitializationCompletedEventArgs>(OnViewModelInitialization);

    // The OnNavigatedTo event can fire more than once, but the converters only need
    // to be registered once, so checking the conversion flag first.
    if (_isConversionRegistered == false)
    {
        // Register converters.
        RegisterTextFieldValueConverters();
        _isConversionRegistered = true;
    }
}
  ```

6. Speichern Sie die Datei.
    
  
Beachten Sie, dass die Konvertierungsfunktionen Daten für alle Felder, die ein **Text** -Datentyp in den Formularen bearbeiten und neu zugeordnete registriert sind. Aus diesem Grund, dass die **Abrufen** und **Festlegen von** Funktionen für die im vorhergehenden Verfahren erstellten **ContosoConverter** -Klasse implementiert bedingte Prüfungen zum Verarbeiten der Daten für ein bestimmtes Feld nur enthalten ist (mit dem Namen, in diesem Fall "Contact_x0020_Number"), wobei die Daten nach "für die anderen Felder **Text** passieren".
  
    
    

## Benutzerdefinierte Konvertierungslogik zum Bereitstellen von nicht unterstützte Feldtypen
<a name="BKMK_ConversionForUnsupportedFields"> </a>

Auch wenn Sie Konvertierungslogik in Ihrer app für **Text** Felder nicht angeben, können diese Felder weiterhin angezeigt und bearbeitet werden. Bereitstellung von eigenen Konvertierungslogik für Felder, die bereits mit der standardmäßigen Konvertierungslogik bereitgestellt werden stellt eine größere Kontrolle über das Format oder die Struktur der Daten in die Felder an, wenn die Standardlogik für Ihre Absichten Design nicht geeignet ist. Für Felder mit bestimmten anderen Datentypen wie **Guid** -Feldern Konvertierungslogik nicht standardmäßig bereitgestellt, aber wenn Sie den Mechanismus (im vorherigen Abschnitt beschrieben) verstehen, um den Konvertierungslogik für Felder bereitgestellt wird, möglicherweise relativ einfach eine eigene Konvertierungslogik zur Unterstützung der Feldtypen in Ihrer app, die mit der Standardlogik nicht unterstützt werden, durch die Vorlage Windows Phone SharePoint List Application bereitzustellen.
  
    
    
Angenommen Sie, Sie erstellen eine Windows Phone-app basierend auf einer SharePoint-Liste mit dem Namen Produkt-IDs, die ein Feld mit einem Datentyp **Guid** enthält. Für die im folgenden Codebeispiel wird davon ausgegangen Sie, dass die Liste ein Feld Product (oder Titel) (der Typ **Text**) und einen Bezeichner dar (der Typ **Guid**) verfügt.
  
    
    

> **HINWEIS**
> SharePoint-Listen mit **Guid** Feldern müssen erstellt werden, programmatisch oder über eine Listenvorlage, die **Guid** Felder enthält.
  
    
    

In einer Windows Phone-app mithilfe der Vorlage erstellt und basierend auf dieser einfachen Liste werden die Daten in **Guid** Feldern nicht standardmäßig angezeigt. (Anstelle dieser Daten wird eine Meldung wie die folgende angezeigt: "Kein Konverter für Feldtyp"Guid"registriert ist.") Schließen Sie in der folgenden Prozedur Konvertierungslogik zur Unterstützung der **Guid** Felder für eine Windows Phone app ein. Sie fügen eine Klasse enthält Methoden zum Registrieren von Feld Wertkonvertern GUIDs angezeigt und zum Generieren von neuer GUID-Werten für Listenelemente hinzugefügt.
  
    
    

### Bei der Konvertierungslogik für ein nicht unterstützter Feldtyp werden


1. Wählen Sie in einem Projekt (namens, z. B. SPListAppGuidConversion) aus dem Windows Phone SharePoint List Application erstellt und basierend auf der Produkt-IDs Liste oben erwähnten den Knoten ab, das Projekt im **Projektmappen-Explorer** aus.
    
  
2. Klicken Sie im Menü **Projekt** auf **Klasse hinzufügen**. Das Dialogfeld **Neues Element hinzufügen** wird angezeigt. Die C#-Vorlage **Klasse** ist bereits ausgewählt.
    
  
3. Geben Sie GuidConversion.cs als Namen der Datei, und wählen Sie **Hinzufügen**. Die Klassendatei ist zur Lösung hinzugefügt und zur Bearbeitung geöffnet
    
  
4. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:
    
  ```cs
  
using System;
using Microsoft.SharePoint.Phone.Application;
using Microsoft.SharePoint.Client;

namespace SPListAppGuidConversion
{
    public class GuidConversion
    {
        /// <summary>
        /// Registers a GET converter to prepare GUID values for display.
        /// </summary>
        static public void RegisterDisplayFieldGuidConverter()
        {
            // Register GET converter to display GUID values.
            Converter.RegisterDisplayFieldValueConverter(FieldType.Guid,
            getConvertedFieldValue: (string fieldName, ListItem item, ConversionContext context) =>
            {
                string guidValue = string.Empty;

                try
                {
                    guidValue = item.FieldValuesAsText[fieldName];
                }
                catch (PropertyOrFieldNotInitializedException)
                {
                    object itemValue = item[fieldName];
                    if (itemValue != null)
                    {
                        guidValue = itemValue.ToString();
                    }
                }

                if (string.IsNullOrWhiteSpace(guidValue))
                {
                    return string.Format("{{{0}}}", Guid.Empty);
                }
                else
                {
                    Guid g = new Guid();

                    if (Guid.TryParse(guidValue, out g))
                    {
                        guidValue = string.Format("{{{0}}}", g.ToString().ToUpper());
                        return guidValue;
                    }
                    else
                    {
                        return "Invalid Identifier (GUID)";
                    }
                }
            });
        }

        /// <summary>
        /// Registers GET and SET converters for GUID value of new (i.e., added) list items.
        /// </summary>
        public static void RegisterNewFieldGuidConverter()
        {
            Converter.RegisterNewFieldValueConverter(FieldType.Guid,
                getConvertedFieldValue: (string fieldName, ListItem item, 
                                 ConversionContext context) => Guid.NewGuid().ToString(),
                setConvertedFieldValue: (string fieldName, object fieldValue, 
                                ListItem item, ConversionContext context) =>
                {
                    if (fieldValue == null)
                    {
                        item[fieldName] = Guid.Empty.ToString();
                    }
                    else
                    {
                        item[fieldName] = fieldValue;
                    }
                });
        }
    }
}
  ```


In dieser benutzerdefinierten Klasse die **RegisterDisplayFieldValueConverter** und die **RegisterNewFieldValueConverter** -Methoden der Klasse **Converter** heißen anonyme Funktionen (definiert durch eine Anweisung Lambda) verwenden, um das **Abrufen** und **Festlegen von** Routinen für die Delegaten von den Registrierungsmethoden erforderlich zu implementieren. Das optionale Argument hier Etiketten (z. B. "GetConvertedFieldValue:") sind in diesen Code nur, um zu verdeutlichen, welche Stellvertretungen definiert sind enthalten.)
    
Dieser Ansatz, mit Lambdaausdrücken ist eine Alternative zur Übergabe von benannten fungiert als Parameter an die Konverter Registration-Funktionen, die in einem früheren Verfahren in diesem Thema (im Abschnitt  [zum Registrieren der Daten Konvertierungsfunktionen](#BKMK_RegisterFunctions)) beschriebenen nachgewiesen wurde.
    
  
5. Speichern Sie die Datei.
    
  
6. Wählen Sie im **Projektmappen-Explorer** Objektebene.
    
  
7. Taste(n)F7um die zugeordneten Code-Behind-Datei App.xaml.cs, zur Bearbeitung zu öffnen.
    
  
8. Suchen Sie die Implementierung der **Application_Launching** -Ereignishandler (die in ein neues Projekt erstellt aus der Vorlage für Windows Phone SharePoint List Application leer ist), und Ersetzen Sie ihn durch den folgenden Code.
    
  ```cs
  
private void Application_Launching(object sender, LaunchingEventArgs e)
{
    // Register converters for GUID fields. Converters can be
    // registered just once for the app.
    GuidConversion.RegisterDisplayFieldGuidConverter();
    GuidConversion.RegisterNewFieldGuidConverter();
}
  ```

9. Für die Konverterlogik für die neue Listenelemente verwendet werden (d. h., GUID generieren Werte an, wenn der Liste Elemente hinzugefügt werden), müssen Sie sicherstellen, dass das ID-Feld von der **NewItemViewModel** an das neue Formular gebunden ist. Eine Möglichkeit, dies wird durch Hinzufügen eines ausgeblendeten **TextBlock** -Steuerelements zu NewForm.xaml **Binding** Deklaration für die **Text** -Eigenschaft auf das Feld-ID festgelegt. Sie können das Steuerelement **TextBlock** **StackPanel** -Container-Steuerelement hinzufügen, die das **TextBox** -Steuerelement für das Feld Titel wie im folgenden Abschnitt des Markups in der Datei NewForm.xaml enthält.
    
  ```
  
...
    <Grid x:Name="LayoutRoot" Background="Transparent" 
         xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
                 xmlns:controls="clr-namespace:Microsoft.Phone.Controls;
                                      assembly=Microsoft.Phone.Controls">
        <StackPanel>
            <ProgressBar Background="Red" x:Name="progressBar" 
                  Opacity="1" HorizontalAlignment="Center" VerticalAlignment="Top" 
                                    Height="15" Width="470" IsIndeterminate="{Binding IsBusy}"
                                       Visibility="{Binding ShowIfBusy}" />
            <ScrollViewer HorizontalScrollBarVisibility="Auto" Height="700">
                <Grid x:Name="ContentPanel" Width="470">
                    <StackPanel Margin="0,5,0,5">
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" 
                           HorizontalAlignment="Left" Style="{StaticResource PhoneTextNormalStyle}">
                                        Title*</TextBlock>
                            <TextBox Style="{StaticResource TextValidationTemplate}" 
                              FontSize="{StaticResource PhoneFontSizeNormal}" Width="470" 
                                   HorizontalAlignment="Left" Name="txtTitle" Text="{Binding [Title], 
                                    Mode=TwoWay, ValidatesOnNotifyDataErrors=True, 
                                      NotifyOnValidationError=True}" TextWrapping="Wrap" />
                            <TextBlock Name="txtIdentifier" Text="{Binding [Identifier]}" Visibility="Collapsed"/>
                        </StackPanel>
                    </StackPanel>
                </Grid>
            </ScrollViewer>
        </StackPanel>
    </Grid>
...
  ```


Speichern Sie die Datei.
    
  
Wenn Sie das Projekt starten und die app in der Windows Phone-Emulator bereitstellen, um Sie auszuführen, werden in der Listenform (List.xaml) Daten im Feld ID angezeigt, das entsprechende Feld in der SharePoint-Liste enthält einen GUID-Wert. (Siehe Abbildung 1).
  
    
    

**Abbildung 1. Anzeigen von Guid-Feldern**

  
    
    

  
    
    
![Anzeigen von Guid-Feldern](images/eb20da0e-793f-43af-aa5e-67f85cdfa2df.gif)
  
    
    
In Projekten auf Grundlage der Vorlage Windows Phone SharePoint List Application möglicherweise Silverlight-Steuerelemente nicht Formularen hinzugefügt und den Feldtypen, die mit der standardmäßigen Konvertierungslogik in der Vorlage nicht unterstützt werden gebunden werden. Für das Feld Bezeichner der Liste der Produkt-IDs in der vorherigen Prozedur verwendet können Sie Markup hinzufügen auf die Datei DisplayForm.xaml so definieren Sie zusätzliche Steuerelemente zum Anzeigen von GUID-Werten, wie ein **TextBlock** -Steuerelement und ein **TextBox** -Steuerelement in einem **StackPanel** -Steuerelement, das selbst innerhalb der Benutzeroberfläche des **Grid** -Steuerelements enthalten ist. Das Markup für das Steuerelement hinzugefügt **StackPanel** kann wie folgt aussehen.
  
    
    



```

<StackPanel HorizontalAlignment="Left" Orientation="Horizontal"
                                             Margin="0,5,0,5">
    <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
                      Style="{StaticResource PhoneTextNormalStyle}">Identifier:</TextBlock>
    <TextBlock Width="310" HorizontalAlignment="Left" Name="txtIdentifier" 
                 Text="{Binding [Identifier]}" TextWrapping="Wrap" Style="
                                           {StaticResource PhoneTextSubtleStyle}" />
</StackPanel>
```

Das Feld-ID wird jetzt im Anzeigeformular ebenso wie die Liste im Formular angezeigt.
  
    
    

## Zusätzliche Ressourcen
<a name="SP15Supportwinphone_addlresources"> </a>


-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 8.0](http://www.microsoft.com/en-us/download/details.aspx?id=35471)
    
  
-  [Microsoft SharePoint SDK für Windows Phone 8](http://www.microsoft.com/en-us/download/details.aspx?id=36818)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  

