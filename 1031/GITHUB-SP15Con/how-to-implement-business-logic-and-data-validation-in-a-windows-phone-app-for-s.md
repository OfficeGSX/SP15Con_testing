---
title: Vorgehensweise Implementieren von Validierung von Business Logik und die Daten in einem Windows Phone-app für SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: fbbedc38-9651-4cd6-b523-d93cbf1cd39d
---



# Vorgehensweise: Implementieren von Validierung von Business Logik und die Daten in einem Windows Phone-app für SharePoint 2013
Implementieren Sie Datenvalidierung in einer Windows Phone-App, die mithilfe der Windows Phone - SharePoint-Listenanwendungsvorlage erstellt wurde.
 * **Gilt für:*** 
  
    
    


|||
|:-----|:-----|
|**In diesem Artikel**          [Regeln für die Gültigkeitsprüfung Standard](#BKMK_DefaultValidation)           [Implementieren der benutzerdefinierten Datenüberprüfung Regeln](#BKMK_CustomValidation)           [Zusätzliche Ressourcen](#SP15Implementbuslogic_addlresources) <br/> |
  
    
    
![Verwandte Codeausschnitte und Beispiel-Apps](images/mod_icon_links_samples.png)
  
    
    

  
    
    
 [SharePoint 2013: Implementing business logic and data validation](http://code.msdn.microsoft.com/SharePoint-2013-Implementin-ffdabd8e) <br/> |
   

Verwenden Sie in einer Windows Phone-app für die Produktionsumgebung vorgesehen ist, müssen Sie wahrscheinlich von Benutzern auf, Geschäftslogik für den bestimmten Umständen relevant erzwungen, um sicherzustellen, dass geeignete Formatierung der eingegebenen Werte oder einfach zum Abfangen von Fehlern vor dem Speichern von Werten in einer SharePoint-Liste eingegebene Daten zu überprüfen. Projekte, die basierend auf der Vorlage Windows Phone SharePoint List Application standardmäßig Daten Überprüfungslogik unter anderem solche Projekte auch einen Mechanismus für Entwickler zum Implementieren der benutzerdefinierten Datenüberprüfung bereitstellen.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie Visual Studio Express 2012 anstelle von Visual Studio 2010 Express verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung erstellen von apps für Windows Phone 8 und Windows Phone 7.> For more information, see  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## Regeln für die Gültigkeitsprüfung Standard
<a name="BKMK_DefaultValidation"> </a>

Einige Datentypen für Felder in SharePoint-Listen sind standardmäßig einfache Formatierung oder Gültigkeitsprüfung zugeordnet. Wenn Sie eingeben eine ungültige URL für ein Feld basierend auf dem Feld Hyperlink oder Bild, geben Sie in einer SharePoint-Liste, und versuchen, die Änderungen zu speichern, eine Meldung angezeigt, die angibt, dass die eingegebene Adresse ungültig ist. Wenn Sie einen Kundennamen als einen Wert eingeben Feldtyp für ein Feld basierend auf Datum und Uhrzeit, eine Meldung leiten Sie ein Datum in einem gültigen Bereich für das Feld eingeben.
  
    
    

> **HINWEIS**
> Gültigkeitsprüfung der Eingabe Datum ist in Bezug auf SharePoint-Datumsformat. Wenn das Datumsformat des Gebietsschemas Telefon erforderlich ist, passen Sie das Feld, und fügen Sie Validierungen entsprechend anpassen.
  
    
    

Einige dieser grundlegende Validierungsregeln gelten auch standardmäßig in einer Windows Phone-app aus der Vorlage für Windows Phone SharePoint List Application erstellt. Wenn Sie etwas anderes als ein Date-Wert in einem Feld, die an ein SharePoint-Feld vom Typ von Datum und Uhrzeit in dem Formular bearbeiten einer Windows Phone-App basierend auf einer SharePoint-Liste gebunden ist eingeben, sehen Sie eine Fehlermeldung der Datenüberprüfung Wenn der Fokus aus dem Feld zugeordnete **TextBox** Steuerelement wechselt. (Siehe Abbildung 1).
  
    
    

**Abbildung 1. Validierungsfehler in einer Windows Phone-app**

  
    
    

  
    
    
![Hinweis auf Validierungsfehler in einer Windows Phone-App](images/49a93d59-6755-4afc-a703-ca8469b6fa74.gif)
  
    
    
Das Textfeld mit der Bezeichnung "Startzeit" im Formular bearbeiten ist an ein Feld Datum und Uhrzeit in der SharePoint-Liste gebunden, auf der dieser Beispiel-app basiert. Die in Abbildung 1 gezeigte Gültigkeitsprüfungsfehlers (als roter Text) angezeigt wird, wenn ein ungültiges Datum wird in das Textfeld eingegeben (und anschließend das Textfeld den Fokus verliert), da die **ValidatesOnNotifyDataErrors** -Eigenschaft des zugeordneten der **Text** -Eigenschaft des Steuerelements **TextBox** **Binding** -Objekts auf **True** in der XAML-Deklaration festgelegt ist, die die **TextBox** in der Datei EditForm.xaml definiert.
  
    
    



```XML

<StackPanel Orientation="Vertical" Margin="0,5,0,5">
   <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                    Style="{StaticResource PhoneTextNormalStyle}">Start Time*
   </TextBlock>
   <TextBox Height="Auto" Style="{StaticResource TextValidationTemplate}"
    FontSize="{StaticResource PhoneFontSizeNormal}" Width="470" 
        HorizontalAlignment="Left" Name="txtEventDate"
    Text="{Binding [EventDate], Mode=TwoWay, ValidatesOnNotifyDataErrors=True,
                       NotifyOnValidationError=True}"
    TextWrapping="Wrap" />
   <TextBlock FontSize="16" TextWrapping="Wrap" HorizontalAlignment="Left"
    Style="{StaticResource PhoneTextSubtleStyle}" Text="{Binding DateTimeFormat}" />
</StackPanel>
```

(Wenn die **ValidatesOnNotifyDataErrors** -Eigenschaft auf **False**festgelegt ist, hat der Benutzer keinen Hinweis, dass die eingegebenen Daten ungültig ist, bis die Schaltfläche **Speichern** ausgewählt ist. An diesem Punkt wird dem Benutzer eine Fehlermeldung über Validierungsfehler, da Format Überprüfung auf eingegebenen Datumswerten weiterhin von der Basisklasse durchgeführt wird von der **EditItemViewModel** -Klasse abgeleitet wird.)
  
    
    
Jedoch einige Felder möglicherweise keine Benachrichtigungen für ungültige Daten in der Windows Phone-app bereit. Und ausgereiften Visual Studio-Projektvorlagen sind unbedingt allgemeiner (engl.), um als Ausgangspunkt für viele verschiedene Anwendungen verwendet werden. Die Vorlage Windows Phone SharePoint List Application kann nicht gehören das Validierungsregeln für bestimmte Kontexte und seinen Wert als allgemeine Vorlage noch beibehalten. Je nach Ihren Anforderungen und den Fällen, in denen Ihre bestimmten Windows Phone-app verwendet wird, werden Sie wahrscheinlich Ihre eigenen benutzerdefinierten Datenüberprüfung Regeln implementieren möchten.
  
    
    

## Implementieren der benutzerdefinierten Datenüberprüfung Regeln
<a name="BKMK_CustomValidation"> </a>

You can validate data entered by users of your Windows Phone app in several ways. A project created by using the Windows Phone SharePoint List Application template includes classes that serve as intermediaries between the forms (that is, the views) of the data in the Windows Phone app (for example, the EditForm.xaml file) and the data itself in the SharePoint list on which the app is based. These classes can be considered implementations of the ViewModel component of the  [Model-View-ViewModel design pattern](http://blogs.msdn.com/b/johngossman/archive/2005/10/08/478683.aspx) (Figure 2). (For more information about how the Windows Phone SharePoint List Application template conforms to the MVVM software design pattern, see [Architektur der Vorlage Windows Phone SharePoint-Liste-Anwendung](architecture-of-the-windows-phone-sharepoint-list-application-template.md).)
  
    
    

> **HINWEIS**
> Die SharePoint-Listenvorlagen enthalten nicht standardmäßigen Überprüfungen (wie in einer SharePoint-Aufgabenliste Post Kontrollkästchen für Team Diskussionsliste und Überprüfung des SP decimal Feld abgeschlossenen Prozentsatz), aber Sie können solche Validierungen implementieren.
  
    
    


**Abbildung 2. Vorlagendateien in ViewModel-Komponente**

  
    
    

  
    
    
![Vorlagendateien in der ViewModel-Komponente](images/2df9591d-a837-4130-98e4-5863d0c717e8.gif)
  
    
    
In Anwendungen basierend auf dem MVVM-Muster, ist häufig die Datenüberprüfung in der Datenebene behandelt (d. h., in dem Modell Komponente). In Projekten, die aus der Vorlage für Windows Phone SharePoint List Application erstellt, ein erweiterbaren Mechanismus für die Datenüberprüfung wurde "abgelegt" eine Ebene und in der ViewModel-Komponente zu vereinfachen für Entwickler zum Verwalten von Datenüberprüfung implementiert. In Projekten auf Grundlage der Vorlage ist daher die am besten geeigneten Position für benutzerdefinierten Code, der von Benutzereingaben überprüft oder anderweitig verwaltet werden Daten in diesen ViewModel-Klassen. Im Hinblick auf die Datenüberprüfung, die **EditItemViewModel** -Klasse und die **NewItemViewModel** -Klasse (die Klassen im Zusammenhang mit den Formularen, die am ehesten umfassen, bearbeiten und Aktualisieren von Listendaten) enthalten sowohl eine offene Implementierung einer Validierung-Methode (mit dem Namen **Validate**), die die Basis Validation-Methode in der Klasse überschreibt, von der diese beiden Klassen abgeleitet werden.
  
    
    



```cs

public override void Validate(string fieldName, object value)
   {
      base.Validate(fieldName, value);
   }
```

Diese Methode bieten eine komfortable Möglichkeit zum Hinzufügen von benutzerdefinierten Überprüfungslogik dieser Ziele einzelne Felder-Entwickler. Der allgemeine Ansatz ist, überprüfen Sie den Wert des **fieldName** -Arguments übergeben an die **Validate** -Methode, um das Feld zu identifizieren, dass Sie mit der benutzerdefinierten Überprüfungscode zuordnen möchten. Beispielsweise eine **switch** -Anweisung in der Implementierung dieser Methode können Überprüfungslogik speziell für die verschiedenen Felder im Formular bearbeiten (EditForm.xaml) Ihrer App für Windows bereitstellen.
  
    
    
Im folgenden Codebeispiel wird davon ausgegangen Sie, dass eine Installation von SharePoint Server eine Produktbestellungen Liste aus der Vorlage benutzerdefinierte Liste erstellt wurde. Die Liste mit den Spalten und Feldtypen erstellt wurde in Tabelle 1 dargestellt.
  
    
    

**In Tabelle 1. Orders Produktliste**


|**Spalte**|**Typ**|**Erforderlich**|
|:-----|:-----|:-----|
|Product (d. h., Titel) <br/> |Einzelne Textzeile (Text) <br/> |Ja <br/> |
|Description <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
|Anzahl <br/> |Zahl <br/> |Ja <br/> |
|Bestelldatum <br/> |Datum und Uhrzeit (DateTime) <br/> |Nein <br/> |
|Auftragserfüllung Datum <br/> |Datum und Uhrzeit (DateTime) <br/> |Nein <br/> |
|Rufnummer <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
   
Erneut aus, für die in diesem Beispiel wird, wird davon ausgegangen Sie, dass die folgenden einfachen Validierungsregeln erzwungen werden soll, basierend auf der Geschäftslogik, die an das fiktive Unternehmen Contoso, Ltd. für ein bestimmtes Produkt Sortierung System eingesetzt werden:
  
    
    

- Auftragserfüllung Datumsangaben für Bestellungen müssen nach dem Datum sein, auf denen die Bestellung aufgegeben wurde.
    
  
- Wenn ein Kunde ein Produkt mit dem Namen Fuzzy Würfel bestellen möchte, müssen die Würfel paarweise bestellt werden. Entsprechend den besonderen Regeln bei Contoso, Ltd. ist einfach keine so gut wie eine Fuzzy inaktiv.
    
  
- In der Liste Bestellungen der Feldtyp für Telefonnummern "Eine Textzeile" ist (d. h., Text), Text (bis zu 255 Zeichen in der Standardeinstellung) erfolgen kann. In diesem Beispiel wird eine Überprüfung Formatierungsregel erzwungen werden, die eingegebene Daten in einem gemeinsamen rufnummernformate werden benötigt; beispielsweise "(555) 555-5555".
    
  

### Zur Implementierung benutzerdefinierter Überprüfungsregeln


1. Assuming you have created a SharePoint list based on the Custom List template that includes the columns and types specified in Table 1, create a Windows Phone app by using the Windows Phone SharePoint List Application template in Visual Studio by following the steps detailed in  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md).
    
  
2. Klicken Sie im **Projektmappen-Explorer** im Ordner ViewModels für das Projekt doppelklicken Sie auf die Datei EditItemViewModel.cs (oder wählen Sie die Datei, und drücken SieF7) die Datei zur Bearbeitung geöffnet.
    
  
3. Fügen Sie die folgenden **using** Direktiven zur Liste der Direktiven am Anfang der Datei.
    
  ```cs
  
using System.Globalization;
using System.Text.RegularExpressions;
  ```

4. Ersetzen Sie die standardmäßige Implementierung der **Validate** -Methode in der Datei durch den folgenden Code ein.
    
  ```cs
  
public override void Validate(string fieldName, object value)
{
    string fieldValue = value.ToString();
    if (!string.IsNullOrEmpty(fieldValue)) //Allowing for blank fields.
    {
        bool isProperValue = false;

        switch (fieldName)
        {
            case "Quantity":
                // Enforce ordering Fuzzy Dice in pairs only.
                int quantityOrdered;
                isProperValue = Int32.TryParse(fieldValue, out quantityOrdered);
                if (isProperValue)
                {
                    if ((quantityOrdered % 2) != 0) // Odd number of product items ordered.
                    {
                        if ((string)this["Title"] == "Fuzzy Dice")
                        {
                            AddError("Item[Quantity]", "Fuzzy Dice must be ordered in pairs. 
                                                                   No such thing as a Fuzzy Die!");
                        }
                        else
                        {
                            // Restriction on ordering in pairs doesn't apply to other products.
                            RemoveAllErrors("Item[Quantity]");
                        }
                    }
                    else
                    {
                        RemoveAllErrors("Item[Quantity]");
                    }
                }
                break;
            case "Fulfillment_x0020_Date":
                // Determine whether fulfillment date is later than order date.
                DateTime fulfillmentDate;
                isProperValue = DateTime.TryParse(fieldValue, CultureInfo.CurrentCulture, 
                              DateTimeStyles.AssumeLocal, out fulfillmentDate);
                if (isProperValue)
                {
                    DateTime orderDate;
                    isProperValue = DateTime.TryParse((string)this["Order_x0020_Date"], 
                               CultureInfo.CurrentCulture, DateTimeStyles.AssumeLocal, out orderDate);

                    if (fulfillmentDate.CompareTo(orderDate) > 0)
                    {
                        RemoveAllErrors("Item[Fulfillment_x0020_Date]");
                    }
                    else
                    {
                        AddError("Item[Fulfillment_x0020_Date]", 
                                "Fulfillment Date must be later than Order Date.");
                    }
                }
                break;
            case "Contact_x0020_Number":
                // Check that contact number is in an appropriate format.
                Regex rx = new Regex(@"^\\(?([0-9]{3})\\)?[-. ]?([0-9]{3})[-. ]?([0-9]{4})$");
                if (rx.IsMatch(fieldValue))
                {
                    RemoveAllErrors("Item[Contact_x0020_Number]");
                }
                else
                {
                    //Specified Contact Number is not a valid phone number.
                    AddError("Item[Contact_x0020_Number]", "Specified Contact Number is invalid.");
                }
                break;
            default:
                // Not adding custom validation for other fields.
                break;
        }
    }

    //And then proceed with default validation from base class.
    base.Validate(fieldName, value);
}
  ```


Denken Sie daran, die in diesem Codebeispiel wird die Feldnamen angegeben basieren auf Eigenschaften der in Tabelle 1 angegebenen Beispiel Produktbestellungen Liste. (Beachten Sie, dass im XML-Schema für Listenfelder in SharePoint Server, mit der Zeichenfolge "_x0020_" für das **Name** -Attribut des **Field** -Elements, das ein bestimmtes Feld definiert werden Leerzeichen in den Namen der Felder ersetzt. Die Vorlage verwendet das **Name** -Attribut für ein Element **Field** wie es in das XML-Schema auf dem Server nicht das **DisplayName** -Attribut definiert ist.) Sie können die Namen der Felder identifizieren, für die Sie Überprüfungslogik verfolgen die **Binding** Deklarationen der **Text** Eigenschaften für die in EditForm.xaml definierten **TextBox** Objekte oder durch die Zeichenfolge **ViewFields** der **CamlQueryBuilder** -Klasse in der Datei ListProvider.cs untersuchen implementieren möchten.
    
  
5. Speichern Sie die Datei.
    
  
Code für die benutzerdefinierte Gültigkeitsprüfung in diesem Beispiel wird nur ausgeführt, wenn das **value** -Argument an die **Validate** -Methode übergeben, nicht null oder eine leere Zeichenfolge ist. Wie in Tabelle 1 dargestellt, die Felder Auftragserfüllung Datum und die Telefonnummer ist nicht erforderlich, Daten enthalten (wie die Liste für die Zwecke dieses Beispiels im SharePoint Server definiert ist), sodass wir diese Felder leer zulassen möchten. Eine einfache Überprüfung, ob das Argument **value** null ist ist nicht ausreichend, da der übergebene Wert kann eine Zeichenfolge der Länge Null (die auf einen Nullwert gleichsetzen nicht) sein, und für dieses Beispiel möchten wir nicht leere Zeichenfolgen für Felder ungültig, die leer sein können. Der Überprüfungslogik für die Auftragserfüllung Datum und Menge Felder enthält zusätzliche Überprüfungen der Werte übergeben, um sicherzustellen, dass sie den entsprechenden Typ aufweisen. Wenn die anfängliche Überprüfung (vor der Anweisung **switch** ) nur mit den übergebenen Wert bestätigt wurden nicht null (anstatt Überprüfung anhand der schmaleren Bedingung wird eine leere Zeichenfolge), die Überprüfungen würde noch nicht ausgeführt, wenn der Wert eine leere Zeichenfolge wurden, aber die Logik zum Validieren von Daten für die *würde* **Telefonnummer** dar weiterhin ausgeführt, wenn der übergebene Wert eine Zeichenfolge der Länge Null wurden. Und in diesem Beispiel wir für das Feld Telefonnummer werden leer (eine leere Zeichenfolge) zulassen möchten, insbesondere dann, wenn ein Benutzer startet Bearbeitung eines Listenelements durch Öffnen des Formulars bearbeiten.
  
    
    
Wenn Sie das Projekt erstellen und zu Windows Phone-Emulator bereitstellen, um Sie auszuführen, können Sie Testen Ihrer Überprüfungslogik durch Eingeben von Daten, die verletzt Geschäftsregeln in die Felder der Liste in dem Formular bearbeiten, der die App (siehe Abbildung 3).
  
    
    

**Abbildung 3. Benutzerdefinierte Validierungsfehler**

  
    
    

  
    
    
![Benutzerdefinierte Hinweise auf Validierungsfehler](images/fada902b-fa38-4ac8-8566-3693b736ac35.gif)
  
    
    
Der Code in diesem Beispiel erzwingt wenn er in der EditItemViewModel.cs-Datei nur enthalten ist diese Überprüfungsregeln für ausschließlich auf dem Formular Bearbeiten von Benutzern eingegebenen Daten. Wenn Sie möchten die beiden Validierungsregeln erzwingen, wenn Benutzer  *Hinzufügen*  neuer Elemente sowie beim Bearbeiten, die dieselbe Überprüfungslogik in der **Validate** -Methode in der NewItemViewModel.cs umfassen muss Datei (oder erstellen eine separate Klassendatei sollte vorzugsweise mit einer Funktion, die diese Überprüfungslogik und Anruf, dass dieselbe aus **Validate** Methoden in der Datei EditItemViewModel.cs und die Datei NewItemViewModel.cs Funktion enthält).
  
    
    
The validation logic in this sample enforces given business rules by indicating to the user that entered data is not in a format permitted by the rules, but the entered data is not intercepted and changed by this code. To intercept and, for example, format phone numbers in a consistent way before saving the data to the SharePoint list, you can implement custom data conversion for entered phone numbers. For an explanation of custom data conversion for list item fields, see  [Vorgehensweise: Support- und konvertieren SharePoint 2013 FieldTypes für Windows Phone-apps](how-to-support-and-convert-sharepoint-2013-field-types-for-windows-phone-apps.md).
  
    
    

## Zusätzliche Ressourcen
<a name="SP15Implementbuslogic_addlresources"> </a>


-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Binden von Silverlight-Daten](http://msdn.microsoft.com/en-us/library/cc278072.aspx)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 8.0](http://www.microsoft.com/en-us/download/details.aspx?id=35471)
    
  
-  [Microsoft SharePoint SDK für Windows Phone 8](http://www.microsoft.com/en-us/download/details.aspx?id=36818)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  
