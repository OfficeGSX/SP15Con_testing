---
title: Vorgehensweise Anpassen der Liste Element Abfragen und Filtern von Daten für Windows Phone-apps
ms.prod: SHAREPOINT
ms.assetid: 32f89b97-8274-4cb0-9164-7898735a18aa
---


# Vorgehensweise: Anpassen der Liste Element Abfragen und Filtern von Daten für Windows Phone-apps
Passen Sie die Datenabfragen an, auf denen die Ansichten in einer Windows Phone-App basieren.
Mit Projekten, die aus der Vorlage für Windows Phone SharePoint List Application erstellt haben können Entwickler nutzen ein Entwurfsmuster in die Vorlage, die sie zum Anpassen von Komponenten der Datenebene für eine Windows Phone-app kann implementiert. Eine Ansicht einer SharePoint-Liste in einer Windows Phone-app enthalten, wie in der app auf dem Telefon wird und in Microsoft SharePoint Server konfiguriert werden kann, oder eine benutzerdefinierte Ansicht für die app erstellt werden.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie anstelle von Visual Studio 2010 Express Visual Studio Express 2012 verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung Erstellen von apps für Windows Phone 8 und Windows Phone 7.> Weitere Informationen finden Sie unter  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## Konfigurieren von Listenansichten auf dem Server für die Verwendung in Windows Phone-apps
<a name="BKMK_ConfiguringLists"> </a>

Wenn Sie eine SharePoint-Listen-app für eine Windows Phone mithilfe der Windows Phone SharePoint List Application Vorlage erstellen, können Sie auswählen in Ihrer app enthalten alle vorhandenen Ansichten, die Ziel-SharePoint-Liste zugeordnet sind. Einer der Methoden zum Filtern von Elementen in einer SharePoint-Liste wie die Liste auf dem Telefon angezeigt wird dann ist so konfigurieren Sie eine gefilterte Ansicht für die Liste auf dem Server und dann wählen Sie diese Ansicht für die in Ihrer Windows Phone-app enthalten sein. Die Windows Phone SharePoint List Application Vorlagen-Assistent generiert eine Abfrage Collaborative Application Markup Language (CAML) für die ausgewählte Ansicht, die die filterbedingungen für die Ansicht auf dem Server konfiguriert. Sie möglicherweise, beispielsweise eine Liste auf dem Server verfügen, die auf die Listenvorlage Aufgaben basiert. Sie können eine Ansicht erstellen für die Liste mit dem Namen "Feiertag Partei", die nur Elemente im Zusammenhang mit, beispielsweise umfasst Planung einer Unternehmens Feiertag Partei durch das Hinzufügen einer filterbedingung zum Anzeigen der Listenelemente, nur, wenn das Feld Beschreibung die Wörter "Feiertag" oder "Partei" enthält. In der Windows Phone-app würde das CAML-Markup für die Ansicht generiert (je nach den Feldern, die sich entschieden, die in Ihrer app enthalten sein) wie folgt.
  
    
    

```XML

<View>
    <Query>
        <Where>
            <Or>
                <Contains>
                    <FieldRef Name='Body' />
                    <Value Type='Note'>holiday</Value>
                </Contains>
                <Contains>
                    <FieldRef Name='Body' />
                    <Value Type='Note'>party</Value>
                </Contains>
            </Or>
        </Where>
    </Query>
    <RowLimit>30</RowLimit>
    <ViewFields>
        <FieldRef Name='Title'/>
        <FieldRef Name='Body'/>
        <FieldRef Name='AssignedTo'/>
        <FieldRef Name='Status'/>
        <FieldRef Name='PercentComplete'/>
        <FieldRef Name='StartDate'/>
        <FieldRef Name='DueDate'/>
        <FieldRef Name='Checkmark'/>
    </ViewFields>
</View>
```

Wie bei anderen vorhandenen Ansichten für die Aufgabenliste, die Sie angeben, dass in Ihrer Windows Phone-app einschließen aus, wenn Sie das Projekt erstellen, wird ein **PivotItem** -Steuerelement, das der ausgewählten Ansicht entspricht dem Steuerelement **Pivot** hinzugefügt, aus denen das wichtigsten Element der Benutzeroberfläche (UI) in der app besteht.
  
    
    

## Anpassen der Liste Ansicht Abfragen in der Windows Phone-app
<a name="BKMK_CustomizingLists"> </a>

Für einen oder anderen Grund möglicherweise nicht möglich oder sinnvoll sein, die Ansichten zu konfigurieren, die alle von Ihren Erfordernissen für eine bestimmte Liste auf dem Server erfüllen. In einem Microsoft Visual Studio-Projekt aus der Vorlage für Windows Phone SharePoint List Application erstellt haben sind Aspekte der was der Datenebene aufgerufen werden kann für Entwickler, hauptsächlich durch die ListDataProvider.cs-Datei des Projekts verfügbar. Sie können das CAML für eine vorhandene Ansicht definierten ändern, oder Sie können CAML-Abfragen für neue Ansichten in der Datei ListDataProvider.cs hinzufügen.
  
    
    

### Die Datei ListDataProvider.cs

In einem Projekt basierend auf der Vorlage Windows Phone SharePoint List Application definiert die Datei ListDataProvider.cs Objekte, die für den Zugriff und Konfigurieren einer SharePoint-Liste als Datenquelle für die Ansichten in der Windows Phone-app bereitstellen. In der Datei List.xaml, die die wichtigsten Anwendungsseite für die app definiert wird, wird ein **Pivot** -Steuerelement (selbst mit den untergeordneten **PivotItem** Steuerelementen) mit einem Ereignishandler zugewiesen an das Ereignis **LoadedPivotItem** deklariert. Die **LoadDataFromServer** -Methode in der Datei ListDataProvider.cs ist letztlich aufgerufen, wenn ein **PivotItem** -Steuerelement (die als Rendering Container für Listenelemente in der Windows Phone-app verwendet wird) auf der Seite Anwendung der app geladen wird.
  
    
    

1. Die **PivotItem** einer angegebenen Listenansicht zugeordnet ist in der Benutzeroberfläche geladen.
    
  
2. In der Datei List.xaml.cs ruft der Ereignishandler für das Ereignis **LoadedPivotItem** die **LoadData** -Methode in der Datei ListViewModel.cs implementiert übergeben Sie den Namen des Steuerelements **PivotItem**, die vollständig geladen wurde. (Zum Entwerfen von Projekten basierend auf der Vorlage Windows Phone SharePoint List Application ist der Name eines bestimmten **PivotItem** -Steuerelements auf festgelegt den Schlüsselwert für die CAML-Abfragezeichenfolge für die Ansicht in der **ViewXmls** **Dictionary** -Typ in der **CamlQueryBuilder** -Klasse in ListViewModel.cs definierten diesem Steuerelement zugeordneten identisch sein.)
    
  
3. Die **LoadData** -Methode in ListViewModel.cs Ruft die **LoadData** -Methode in der Datei ListDataProvider.cs implementiert.
    
  
4. Die **LoadData** -Methode in ListDataProvider.cs Ruft die **LoadDataFromServer** -Methode, die auch in der gleichen Datei implementiert. Die **LoadDataFromServer** -Methode führt dann Folgendes aus:
    
1. Ruft die CAML-Abfrage-Zeichenfolge einer bestimmten Ansicht zugeordnet.
    
  ```cs
  
CamlQuery query = CamlQueryBuilder.GetCamlQuery(ViewName);
  ```

2. Journale mit das Clientobjekt modellieren die Liste abgerufen werden sollen.
    
  ```cs
  ListItemCollection items = Context.Web.Lists.GetByTitle(ListTitle).GetItems(query);
  ```

3. Zeigt das Clientobjektmodell, dass die Listenelemente und den Feldern des diese Listenelemente (als Textwerte) zurückgegeben werden soll.
    
  ```cs
  Context.Load(items);
Context.Load(items, listItems => listItems.Include(item => item.FieldValuesAsText));
  ```

4. Ruft die **ExecuteQueryAsync** zum Senden von Anfragen an SharePoint Server und die Daten (asynchron) abzurufen.
    
  

  
    
    

## Hinzufügen einer benutzerdefinierten listenansichtsabfrage und der entsprechenden Benutzeroberfläche Elemente
<a name="BKMK_AddingCustomizations"> </a>

In eigene Projekte können Sie die Art und Weise nutzen, die die Datenebene entwickelt wurde, um Ihre eigenen benutzerdefinierten CAML-Abfragezeichenfolgen und Listenansichten hinzuzufügen.
  
    
    
Im folgenden Codebeispiel wird angenommen Sie erneut, dass die Zielinstallation von SharePoint Server eine Bestellungen Liste erstellt aus der Vorlage benutzerdefinierte Liste mit den Feldern konfiguriert und Typen in dem Thema  [Vorgehensweise: Implementieren von Validierung von Business Logik und die Daten in einem Windows Phone-app für SharePoint 2013](how-to-implement-business-logic-and-data-validation-in-a-windows-phone-app-for-s.md)in Tabelle 1 angegeben ist. Erstellen eines Projekts basierend auf der Windows Phone SharePoint List Application-Vorlage, die eine Liste wie die Liste Bestellungen als Quelle (gemäß  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md)) verwendet. Spielte in diesem Beispiel wird hinzugefügt eine benutzerdefinierte Ansicht zu Windows Phone-app (nicht für die Liste auf dem Server), die gefiltert wird, um nur die Bestellungen anzuzeigen, in dem die bestellte Menge 100 oder mehr beträgt.
  
    
    

### Zum Hinzufügen einer benutzerdefinierten Abfrage und anzeigen


1. Klicken Sie im **Projektmappen-Explorer** Doppelklicken Sie auf die Datei ListDataProvider.cs (oder wählen Sie die Datei und drücken SieF7) die Datei zur Bearbeitung geöffnet.
    
  
2. Aktualisieren Sie die Definition der **ViewXmls** **Dictionary** -Typ in der statischen **CamlQueryBuilder** -Klasse, die eine zusätzliche CAML-Abfrage mit einer WHERE-Klausel legt die entsprechende Filtern Bedingung enthalten.
    
  ```cs
  
static Dictionary<string, string> ViewXmls = new Dictionary<string, string>()
{   
    {"View1",   @"<View><Query><OrderBy><FieldRef Name='ID'/>
        </OrderBy></Query><RowLimit>30</RowLimit><ViewFields>{0}</ViewFields></View>"},
    {"View2",   @"<View><Query><OrderBy><FieldRef Name='ID' /></OrderBy>
     <Where><Geq><FieldRef Name='Quantity' />
          <ValueType='Number'>100</Value>
                </Geq></Where>
             </Query><RowLimit>30</RowLimit>
               <ViewFields>{0}</ViewFields></View>"}
};
  ```

3. Doppelklicken Sie auf die Datei List.xaml, um die Datei zur Bearbeitung zu öffnen.
    
  
4. Hinzufügen von Markup, um eine zusätzliche untergeordnete **PivotItem** Steuerelement innerhalb des Steuerelements Hauptfenster **Pivot** definieren. Das **Grid** -Element, in dem die Elemente der Benutzeroberfläche, die die wichtigsten Anwendungsseite definieren deklariert werden, sollte dem folgenden Code ähneln.
    
  ```XML
  
<Grid x:Name="LayoutRoot" Background="Transparent"
 xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
 xmlns:controls="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone.Controls">
    <!--Pivot Control-->
    <ProgressBar x:Name="progressBar" Opacity="1" HorizontalAlignment="Center" 
     VerticalAlignment="Top" Height="30" Width="470" IsIndeterminate="{Binding IsBusy}" 
     Visibility="{Binding ShowIfBusy}" />
    <Grid x:Name="ContentPanel" Grid.Row="0" Width="470">
        <controls:Pivot Name="Views" Title="Product Orders" LoadedPivotItem="OnPivotItemLoaded">
            <!--Pivot item-->
            <controls:PivotItem Name="View1" Header="All Items">
                <!--Double line list with text wrapping-->
                <ListBox x:Name="lstBox1" Margin="0,0,-12,0" SelectionChanged="OnSelectionChanged" 
                 ItemsSource="{Binding [View1]}">
                    <ListBox.ItemTemplate>
                        <DataTemplate>
                            <StackPanel Orientation="Vertical" Margin="10">
                                <TextBlock Name="txtTitle" Text="{Binding [Title]}" 
                                 TextWrapping="NoWrap" Style="{StaticResource PhoneTextTitle2Style}" />
                                <TextBlock Name="txtDescription" Text="{Binding [Description]}" 
                                 TextWrapping="NoWrap" Style="{StaticResource PhoneTextNormalStyle}" />
                                <TextBlock Name="txtQuantity" Text="{Binding [Quantity]}" 
                                 TextWrapping="NoWrap" Style="{StaticResource PhoneTextNormalStyle}" />
                            </StackPanel>
                        </DataTemplate>
                    </ListBox.ItemTemplate>
                </ListBox>                    
            </controls:PivotItem>
            
            <!--Added PivotItem control for customized view--><controls:PivotItem Name="View2" Header="Big Orders"><!--Double line list with text wrapping--><ListBox x:Name="lstBox2" Margin="0,0,-12,0" 
                 SelectionChanged="OnSelectionChanged" ItemsSource="{Binding [View2]}"><ListBox.ItemTemplate><DataTemplate><StackPanel Orientation="Vertical" Margin="10"><TextBlock Name="txtTitle" Text="{Binding [Title]}" 
                                 TextWrapping="NoWrap" Style="{StaticResource PhoneTextTitle2Style}" /><TextBlock Name="txtDescription" Text="{Binding [Description]}" 
                                 TextWrapping="NoWrap" Style="{StaticResource PhoneTextNormalStyle}" /><TextBlock Name="txtQuantity" Text="{Binding [Quantity]}" 
                                 TextWrapping="NoWrap" Style="{StaticResource PhoneTextNormalStyle}" /></StackPanel></DataTemplate></ListBox.ItemTemplate></ListBox></controls:PivotItem>

        </controls:Pivot>
    </Grid>
</Grid>
  ```


> **HINWEIS**
> Insbesondere ist, dass der Wert des Attributs **Name** ("View2") von der **PivotItem** Steuerelement identisch mit den Schlüsselwert des Eintrags, der in Schritt 2 definierten **Dictionary** -Typ hinzugefügt wurde. Dieser Wert wird zum Identifizieren der entsprechenden CAML-Abfrage zum Abrufen der Daten in der **PivotItem**angezeigt werden. Beachten Sie, dass die **ListBox** hier deklariert (namens "lstBox2" einfach zur Unterscheidung von der **ListBox** für die Standardansicht) ist auch auch zur Ansicht gebunden.

    
    
  
Wenn Sie Ihr Projekt (durch Drücken von F5) starten, enthält das **Pivot** -Steuerelement für die app die beiden **PivotItem** Steuerelemente und die Daten abgerufen, indem die CAML-Abfragen, die ihren jeweiligen Ansichten zugeordnet. Die Standardansicht für alle Elemente zeigt alle Aufträge, wie in Abbildung 1 (mit Beispieldaten) dargestellt.
  
    
    

**Abbildung 1. Alle Bestellungen (Listenelemente) in einer Beispielliste**

  
    
    

  
    
    
![Alle Bestellungen (Listenelemente) in einer Beispielliste](images/e9cf225a-7040-4545-8db2-9e6f04e7a6eb.gif)
  
    
    
Und die benutzerdefinierte Ansicht, wie im vorherigen Verfahren definiert eine gefilterte Liste von Elementen, die nur die Aufträge für die eine Menge von mindestens 100 angegeben ist, enthält, wie in Abbildung 2 dargestellt angezeigt.
  
    
    

**Abbildung 2. Nur die großen Bestellungen**

  
    
    

  
    
    
![Nur die großen Aufträge](images/0d28b4f9-7f81-47c0-976e-247c2b9544d4.gif)
  
    
    
Sie können viele weitere Anpassungen vor tätigen, um die CAML-Abfragen, auf denen Ansichten basieren, und die Benutzeroberflächenelemente Ansichten zugeordnet.
  
    
    

## Zusätzliche Ressourcen
<a name="SP15Custlistitem_addlresources"> </a>


-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Einführung in Collaborative Application Markup Language (CAML)](http://msdn.microsoft.com/en-us/library/ms426449.aspx)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 8.0](http://www.microsoft.com/en-us/download/details.aspx?id=35471)
    
  
-  [Microsoft SharePoint SDK für Windows Phone 8](http://www.microsoft.com/en-us/download/details.aspx?id=36818)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  

