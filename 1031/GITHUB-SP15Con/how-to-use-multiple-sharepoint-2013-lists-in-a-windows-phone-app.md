---
title: Vorgehensweise Verwenden mehrerer 2013 für SharePoint-in ein Windows Phone-app Listen
ms.prod: SHAREPOINT
ms.assetid: 5251d35a-d659-49b3-8e0d-dfd4a7faee6b
---


# Vorgehensweise: Verwenden mehrerer 2013 für SharePoint-in ein Windows Phone-app Listen
Erstellen Sie Windows Phone-Apps, die Daten aus mehreren SharePoint-Listen verwenden.
Sie können mehrere SharePoint-Listen in Ihrer app auf verschiedene Weise verwenden. Beim Erstellen einer Windows Phone-app basierend auf der Vorlage Windows Phone SharePoint List Application Angabe eines einzelnen Ziels SharePoint-Liste, aber die Architektur des resultierenden Projekts ist erweiterbar sein müssen, um die Integration von mehreren Listen zu berücksichtigen.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie anstelle von Visual Studio 2010 Express Visual Studio Express 2012 verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung Erstellen von apps für Windows Phone 8 und Windows Phone 7.<br/> Weitere Informationen finden Sie unter  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## Erstellen einer Lösung im Zusammenhang mit SharePoint-Listen basierend auf dem gleichen schema
<a name="BKMK_SameSchemaProject"> </a>

Wenn Sie zwei SharePoint-Listen, die basierend auf dem gleichen Schema verfügen, können Sie nutzen Sie die Klassen, die durch die Vorlage Windows Phone SharePoint List Application implementiert und Objekte dieser Klassen speziell für jede Liste erstellen.
  
    
    
Angenommen Sie, Sie haben zwei SharePoint-Listen basierend auf die Kontakte Listenvorlage. Eine Liste mit dem Namen, beispielsweise Marketingteam, enthält Mitglieder ein Marketingteam in Ihrem Unternehmen, der anderen Liste Entwicklungsteams, Mitglieder der engineering-Team. Wenn Sie Erstellen eines Projekts mithilfe der Vorlage Windows Phone SharePoint List Application, und geben Sie die Liste Marketingteam als der Zielliste, auf dem das Projekt basieren soll, wird eine Instanz der Klasse **ListDataProvider** (benannte **DataProvider** standardmäßig) in der Implementierung der **App** -Klasse in der Datei App.xaml.cs im Projekt erstellt. Dieses Objekt stellt die Liste (d. h., das Marketingteam) Liste als Datenquelle für die app und Bereitstellen von Operationen Zugriff und zum Manipulieren von Elementen in der Liste dar. Eine Instanz der **ListViewModel** -Klasse wird auch für die Liste erstellt, auf dem die app basiert. Dieses Objekt verfügt über einen Eigenschaftenmember (in diesem Fall auch **DataProvider**heißen), der auf einer bestimmten Instanz der Klasse **ListDataProvider** festgelegt werden kann die Datenquelle für die Instanz der **ListViewModel** herstellen.
  
    
    
Sie können eine zusätzliche Instanz der **ListDataProvider** -Klasse im Projekt dienen als Datenquelle für die zweite Liste (Entwicklungsteam) in der Datei App.xaml.cs erstellen. Das Objekt wird in den folgenden Code **SecondaryDataProvider** bezeichnet.
  
    
    



```cs

private static ListDataProvider m_SecondaryDataProvider;

public static ListDataProvider SecondaryDataProvider
{
    get
    {
        if (m_SecondaryDataProvider != null)
            return m_SecondaryDataProvider;

        m_SecondaryDataProvider = new ListDataProvider();
        m_SecondaryDataProvider.ListTitle = "Engineering Team";
        m_SecondaryDataProvider.SiteUrl = new Uri("http://contoso:2012/sites/samplesite/");

        return m_SecondaryDataProvider;
    }
}
```

Sie können dann Instanziieren Sie ein anderes Objekt der **ListViewModel** -Klasse (mit dem Namen, beispielsweise **SecondaryViewModel**) und dessen **DataProvider** -Eigenschaft, wie im folgenden Code **SecondaryDataProvider** -Objekt zuweisen.
  
    
    



```cs

private static ListViewModel m_SecondaryViewModel;

public static ListViewModel SecondaryViewModel
{
    get
    {
        if (m_SecondaryViewModel == null)
            m_SecondaryViewModel = new ListViewModel { DataProvider = App.SecondaryDataProvider };

        return m_SecondaryViewModel;
    }
    set
    {
        m_SecondaryViewModel = value;
    }
}
```

Wenn die gleichen Felder und Ansichten für die beiden Listen für Ihre Zwecke geeignet sind (und erneut, wenn die beiden Listen die gleichen Spalten und Felder haben), müssen Sie keine Änderungen vorgenommen haben, in der Implementierung der **ListDataProvider** -Klasse (in der Datei ListDataProvider.cs).
  
    
    
Zum Anzeigen oder ändern Sie die Daten aus der zweiten Liste in Ihrem Projekt, müssen Sie jedoch hinzufügen, Formulare anzeigen des Projekts, die an gebunden und für diese **SecondaryViewModel**konfiguriert sind. Beispielsweise konnten Sie einen Ordner hinzufügen, um das Projekt mit dem Namen "SecondaryViews" und Hinzufügen einer SecondaryList.xaml-Datei in den Ordner mit Markup ähnelt der von der Vorlage für die primäre Liste im Projekt generierte List.xaml-Standarddatei. Beachten Sie, dass Sie Ihre sekundären Listenformular aus dem primären Listenformular in der app unterscheiden sollte durch eine charakteristische Wert für das **x:Class** -Attribut des **PhoneApplicationPage** -Elements in der Datei SecondaryList.xaml angeben.
  
    
    



```

<phone:PhoneApplicationPage
    x:Class="MultipleSPListApp.SecondaryViews.ListForm"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:phone="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone"
    xmlns:shell="clr-namespace:Microsoft.Phone.Shell;assembly=Microsoft.Phone"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d" d:DesignWidth="480" d:DesignHeight="696"
    FontFamily="{StaticResource PhoneFontFamilyNormal}"
    FontSize="{StaticResource PhoneFontSizeNormal}"
    Foreground="{StaticResource PhoneForegroundBrush}"
    SupportedOrientations="PortraitOrLandscape" Orientation="Portrait"
    shell:SystemTray.IsVisible="True" x:Name = "ListViewPage">
...
</phone:PhoneApplicationPage>
```

Ersetzen Sie in der zugeordneten Code-Behind-Datei, SecondaryList.xaml.cs, alle Verweise auf "App.MainViewModel" durch Verweise auf "App.SecondaryViewModel". Beispielsweise sollte der Konstruktor in der Datei wie folgt aussehen.
  
    
    



```cs

public ListForm()
    {
        InitializeComponent();
        this.DataContext = App.SecondaryViewModel;
    }
```

Auch ersetzen Sie alle Verweise in der CodeBehind-Datei auf "App.DataProvider" durch Verweise auf "App.SecondaryDataProvider", und aktualisieren Sie alle Navigation Netzwerkpfade so zeigen Sie auf den entsprechenden sekundären XAML-Seiten. Wenn Sie auch ein sekundäres neues Formular auf Ihr Projekt (mit dem Namen, beispielsweise im Ordner SecondaryViews Ihres Projekts SecondaryNewForm.xaml) hinzufügen würde der Handler in der Datei SecondaryList.xaml.cs für das **OnNewButtonClick** -Ereignis dem folgenden Code ähneln.
  
    
    



```cs

private void OnNewButtonClick(object sender, EventArgs e)
    {
        // Instantiate a new instance of NewItemViewModel and go to NewForm.
        App.SecondaryViewModel.CreateItemViewModelInstance = new NewItemViewModel { DataProvider = App.SecondaryDataProvider };
        NavigationService.Navigate(new Uri("/SecondaryViews/SecondaryNewForm.xaml", UriKind.Relative));
    }
```

Schließlich können Sie die **ApplicationBar** in der Datei List.xaml zum Anzeigen der Seite SecondaryList.xaml eine Schaltfläche hinzufügen.
  
    
    



```

...
    <phone:PhoneApplicationPage.ApplicationBar>
        <shell:ApplicationBar IsVisible="True" IsMenuEnabled="True">
            <shell:ApplicationBarIconButton x:Name="btnNew" IconUri="/Images/appbar.new.rest.png" Text="New" Click="OnNewButtonClick"/>
            <shell:ApplicationBarIconButton x:Name="btnRefresh" IconUri="/Images/appbar.refresh.rest.png" Text="Refresh" IsEnabled="True" Click="OnRefreshButtonClick"/>
            <!--Add the following button to navigate to the secondary list (Engineering Team).-->
            <shell:ApplicationBarIconButton x:Name="btnSecondaryList" IconUri="/Images/appbar.upload.rest.png" Text="Engineering" IsEnabled="True" Click="OnSecondaryListButtonClick"/>
        </shell:ApplicationBar>
    </phone:PhoneApplicationPage.ApplicationBar>
...
```

Fügen Sie in der zugeordneten Code-Behind-Datei, List.xaml.cs, einen Handler für das **OnSecondaryListButtonClick** -Ereignis in der Datei List.xaml deklariert.
  
    
    



```cs

private void OnSecondaryListButtonClick(object sender, EventArgs e)
{
    NavigationService.Navigate(new Uri("/SecondaryViews/SecondaryList.xaml", UriKind.Relative));
}
```

Benutzer Ihrer App können dann zwischen der Marketingteam und das Entwicklungsteam Liste navigieren. Da die zugrunde liegenden Listenschemas die gleichen, die Standard- **DataProvider** sind und **MainViewModel** Objekte mithilfe der Vorlage und den hinzugefügten **SecondaryDataProvider** und **SecondaryViewModel** Objekte Handle alle Datentransaktionen generiert ohne alle Änderungen an der Datei ListDataProvider.cs.
  
    
    

## Erstellen einer Lösung im Zusammenhang mit SharePoint-Listen basierend auf anderen schemas
<a name="BKMK_DifferentSchemasProject"> </a>

Die Vorgehensweise im vorherigen Abschnitt Works so ein, dass wechselt (, die für SharePoint-Listen auf das gleiche Schema basiert,), aber der **ListDataProvider** -Klasse in der Windows Phone SharePoint List Application Vorlage für Entwickler zur Anpassung mehreren SharePoint-Listen bearbeitet, die nicht basieren auf dem gleichen Schema oder kann nicht enthalten dieselben Spalten und Felder, verfügbar ist.
  
    
    
Nehmen Sie an, wie im vorherigen Abschnitt, dass eine SharePoint-Liste Marketingteam (basierend auf der Listenvorlage Kontakte), mit der ein Marketingteam-Mitgliedern vorhanden ist. Nehmen Sie an auch, dass Sie eine zweite Liste, mit dem Namen Orders (basierend auf der Vorlage für benutzerdefinierte Listen), mit der Spalten und Feldtypen in Tabelle 1 aufgeführten verfügen.
  
    
    

**In Tabelle 1. Spalten und Felder für die Orders-Liste**


|**Spalte**|**Feldtyp**|**Erforderlich**|
|:-----|:-----|:-----|
|Produkt (d. h., Titel) <br/> |Einzelne Textzeile (Text) <br/> |Ja <br/> |
|Einzelpreis <br/> |**Währung** <br/> |Ja <br/> |
|Anzahl <br/> |**Number** <br/> |Keine (Standardwert: 0 (null)) <br/> |
|ORDER-Wert <br/> |Berechnet (Einzelpreis * Menge) <br/> |Nein <br/> |
|Bestelldatum <br/> |Datum und Uhrzeit (Datetime) <br/> |Nein <br/> |
|Auftragsstatus <br/> |Auswahl <br/> |Nein <br/> |
|Kunde <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
   
Wie im Beispiel im vorherigen Abschnitt können Sie ein separates **ListDataProvider** und eine andere **ListViewModel** -Objekt zum Verwalten der Orders-Liste instanziieren. Wird davon ausgegangen Sie, dass das instanziierte **ListDataProvider** -Objekt mit der Bezeichnung **OrdersListDataProvider**, wie im folgenden Code wird.
  
    
    



```cs

private static ListDataProvider m_OrdersListDataProvider;

public static ListDataProvider OrdersListDataProvider
{
    get
    {
        if (m_OrdersListDataProvider != null)
            return m_OrdersListDataProvider;

        m_OrdersListDataProvider = new ListDataProvider();
        m_OrdersListDataProvider.ListTitle = "Orders";
        m_OrdersListDataProvider.SiteUrl = new Uri("http://contoso:2012/sites/samplesite/"); // Specify a URL here for your server.

        return m_OrdersListDataProvider;
    }
}
```

Und wird davon ausgegangen, dass das instanziierte **ListViewModel** -Objekt für die Orders-Liste mit der Bezeichnung **OrdersListViewModel**, wie im folgenden Code wird.
  
    
    



```cs

private static ListViewModel m_OrdersListViewModel;

public static ListViewModel OrdersListViewModel
{
    get
    {
        if (m_OrdersListViewModel == null)
            m_OrdersListViewModel = new ListViewModel { DataProvider = App.OrdersListDataProvider };

        return m_OrdersListViewModel;
    }
    set
    {
        m_OrdersListViewModel = value;
    }
}
```

Das Schema für die Orders-Liste unterscheidet sich von der Liste Marketingteam. Sie können die Unterschiede durch Hinzufügen von Code in der Datei ListDataProvider.cs speziell auf die Klasse **CamlQueryBuilder** angeordnet werden.
  
    
    



```cs

public static class CamlQueryBuilder
{
    static Dictionary<string, string> ViewXmls = new Dictionary<string, string>()
    {   
        {"View1",   @"<View><Query><OrderBy><FieldRef Name='Title' />
                    <FieldRef Name='FirstName'  /></OrderBy></Query><RowLimit>30</RowLimit><ViewFields>{0}</ViewFields></View>"},
      {"View2",   @"<View><Query><OrderBy><FieldRef Name='ID' /></OrderBy></Query><RowLimit>30</RowLimit>
     <ViewFields>{0}</ViewFields></View>"}
    };

    static string View1Fields = @"<FieldRef Name='Title'/><FieldRef Name='FirstName'/>
   <FieldRef Name='JobTitle'/><FieldRef Name='Email'/><FieldRef Name='Comments'/>";
    static string View2Fields = @"<FieldRef Name='Title'/><FieldRef Name='Unit_x0020_Price'/><FieldRef Name='Quantity'/>
            <FieldRef Name='Order_x0020_Value'/><FieldRef Name='Order_x0020_Date'/>
            <FieldRef Name='Status'/><FieldRef Name='Customer'/>";

    public static CamlQuery GetCamlQuery(string viewName)
    {
        string viewXml = ViewXmls[viewName];
        // Add ViewFields to the ViewXml depending on the view.
        switch (viewName)
        {
            case "View2":
                viewXml = string.Format(viewXml, View2Fields);
                break;
            case "View1":
            default:
                viewXml = string.Format(viewXml, View1Fields);
                break;
        }
        return new CamlQuery { ViewXml = viewXml };
    }
}
```

Hier wird der zweite Eintrag mit einem Schlüssel-Wert von "View2" das **ViewXmls** **Dictionary** -Objekt für die Orders-Liste hinzugefügt. (Denken Sie daran, die die Schlüsselwerte für Einträge, die **ViewXmls** **Dictionary** in der Klasse **CamlQueryBuilder** hinzugefügt (in der Projektmappe) eindeutig sein für die zwischenspeicherlogik in der Vorlage ordnungsgemäß funktioniert.) String-Variablen ( **View1Fields** und **View2Fields**) dienen zum Speichern der Liste der Felder für die einzelnen Ansichten. Abhängig vom Wert des Parameters **viewName** an die **GetCamlQuery** -Methode übergeben, wird Sie dann die entsprechende CAML-Abfrage-XML-Zeichenfolge erstellt.
  
    
    
Klicken Sie, wie im vorherigen Abschnitt, Ansicht Formulare für die Liste, diesmal gebunden, auf die Objekte **OrdersListViewModel** und **OrdersListDataProvider** erstellen können. Als Beispiel, das XAML für ein Listenformular speziell für die Orders-Liste mit dem Namen OrdersList.xaml, ähnlich wie das Markup in der Datei List.xaml von der Vorlage für die primäre Liste in der app generiert würde, außer dass würde, nennen Sie das **PivotItem** -Steuerelement, das die Liste "View2" rendert (anstelle der Standardeinstellung "View1") und legen Sie die Deklaration **Binding** für das **ItemsSource** -Attribut des **ListBox** -Steuerelements in der Listenelemente auf "View2" wie in der folgenden gerendert werden Markup (der nur das Markup für das Stammraster der Seite angezeigt wird).
  
    
    



```

...
    <Grid x:Name="LayoutRoot" Background="Transparent" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" xmlns:controls="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone.Controls">
        <!--Pivot Control-->
        <ProgressBar x:Name="progressBar" Opacity="1" HorizontalAlignment="Center" VerticalAlignment="Top" 
               Height="30" Width="470" IsIndeterminate="{Binding IsBusy}" Visibility="{Binding ShowIfBusy}" />
        <Grid x:Name="ContentPanel" Grid.Row="0" Width="470">
            <controls:Pivot Name="Views" Title="Orders" LoadedPivotItem="OnPivotItemLoaded">
                <!--Pivot item-->
                <controls:PivotItem Name="View2" Header="All Orders">
                    <!--Double line list with text wrapping-->
                    <ListBox x:Name="lstBox1" Margin="0,0,-12,0" SelectionChanged="OnSelectionChanged" ItemsSource="{Binding [View2]}">
                        <ListBox.ItemTemplate>
                            <DataTemplate>
                                <StackPanel Orientation="Vertical" Margin="10">
                                    <TextBlock Name="txtTitle" Text="{Binding [Title]}" TextWrapping="NoWrap" 
                                          Style="{StaticResource PhoneTextTitle2Style}" />
                                    <TextBlock Name="txtUnitPrice" Text="{Binding [Unit_x0020_Price]}" 
                                         TextWrapping="NoWrap" Style="{StaticResource PhoneTextNormalStyle}" />
                                    <TextBlock Name="txtQuantity" Text="{Binding [Quantity]}"
                                         TextWrapping="NoWrap" Style="{StaticResource PhoneTextNormalStyle}" />
                                </StackPanel>
                            </DataTemplate>
                        </ListBox.ItemTemplate>
                    </ListBox>
                </controls:PivotItem>
            </controls:Pivot>
        </Grid>
    </Grid>
...
```

Eine bequeme Möglichkeit zum Erstellen der geeigneten XAML-Markup wird die Vorlage Windows Phone SharePoint List Application verwendet, so generieren ein separates Projekt basierend auf der Orders-Liste und kopieren Sie den generierten XAML-Code aus diesem Projekt zum Projekt mit mehreren Listen, sorgen, dass Sie den Namen des Steuerelements **PivotItem** ändern (die standardmäßig auf "View1") auf "View2" und die **Binding** Deklaration des **ListBox** -Steuerelements zu ändern, wie hier gezeigt. Sie müssen auch alle Verweise in der zugeordneten Code-Behind-Datei für das Formular an die entsprechenden Objekte **ListViewModel** und **DataProvider** (als, z. B. **OrdersListViewModel** und **OrdersListDataProvider**) zu ändern.
  
    
    
Diese Vorgehensweise funktioniert, weil in der zugeordneten Code-Behind-Datei (mit dem Namen, in diesem Fall OrdersList.xaml.cs), die verschiedene Ereignishandler, die Methoden des **ListDataProvider** -Objekts (in diesem Fall **OrdersListDataProvider**) Liste Daten mit den Namen der **PivotItem** Zugriff auf Aufrufen als anzugeben, verwenden Sie die gewünschte Ansicht steuern. Beispielsweise der **OnPivotItemLoaded** -Ereignishandler ruft die **LoadData** -Methode des **OrdersListViewModel** -Objekts, das von der **ListViewModel** -Klasse instanziiert (und diese Methode ruft wiederum die **LoadData** -Methode des **OrdersListDataProvider** -Objekts), übergeben Sie den Namen des Steuerelements **PivotItem** (hier "View2") als Wert des Parameters **ViewName** an die-Methode. Diese denselben Wert wird letztendlich (als Wert des Parameters **viewName** ) an die in der geänderten Implementierung der **CamlQueryBuilder** -Klasse oben gezeigten **GetCamlQuery** -Methode übergeben.
  
    
    

## Ein anderer Ansatz für eine Lösung unter Verwendung von SharePoint-Listen basierend auf anderen schemas
<a name="BKMK_DifferentSchemasAlternative"> </a>

Als Alternative zum Ansatz im vorherigen Abschnitt können Sie die Vorlage Windows Phone SharePoint List Application verwenden, erstellen Sie eine Windows Phone-app-Projekt in einer Projektmappe Microsoft Visual Studio 2010 basierend auf einer bestimmten SharePoint-Liste, und fügen Sie die Projekte basierend auf anderen Listen, um die gleiche Lösung erstellt. Dieser Ansatz ermöglicht Ihnen die Vorlage zum Generieren von Formularen, die speziell für jede Liste SharePoint nutzen. Anschließend können Sie die Lösung anpassen, je nach Bedarf steuern, wie Benutzer mit den Listen interagieren. Führen Sie die Verfahren in diesem Abschnitt vor, die diesen Ansatz.
  
    
    
Wird für die folgenden Verfahren vorausgesetzt, steht Ihnen eine SharePoint-Liste mit dem Namen Orders (basierend auf der Vorlage für benutzerdefinierte Listen), und die Spalten und Feldtypen wie in Tabelle 1 im vorherigen Abschnitt dargestellt. Darüber hinaus wird vorausgesetzt, Sie haben eine andere SharePoint-Liste (erneut, basierend auf der Vorlage für benutzerdefinierte Listen), mit dem Namen Kunden, und die Spalten und Feldtypen in Tabelle 2 gezeigt.
  
    
    

**In Tabelle 2. Spalten und Felder für Kundenliste**


|**Spalte**|**Feldtyp**|**Erforderlich**|
|:-----|:-----|:-----|
|Kundenname (d. h., Titel) <br/> |Einzelne Textzeile (Text) <br/> |Ja <br/> |
|Rufnummer <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
|E-Mail-Adresse <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
|Firma <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
   
In den folgenden Verfahren erstellen Sie eine Windows Phone-app, die beide dieser Listen verwendet. Die primäre Liste in der app ist die Kunden-Liste. Wenn Sie die Details für einen bestimmten Kunden in dem Formular anzeigen anzeigen, ist eine Schaltfläche auf dem Formular enthalten, die Benutzer alle Aufträge (aus der Orders-Liste) zugeordnet dieses Kunden anzeigen können.
  
    
    

### Erstellen der Komponentenprojekte für die Lösung


1. Erstellen einer Windows Phone-app mithilfe der Vorlage Windows Phone SharePoint List Application, Angeben von einer SharePoint-Liste definierten basierend auf der Spalten und Feldtypen in Tabelle 2 gezeigt. In den Verfahren in diesem Abschnitt wird davon ausgegangen, dass der Name der Liste in das Projekt ist "Kunden" und der Name des Projekts ist "CustomersSPListApp". (Siehe  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md) Informationen zum Erstellen einer app basierend auf der Vorlage Windows Phone SharePoint List Application).
    
  
2. Wählen Sie im Visual Studio, **Datei** **Hinzufügen**, **Neues Projekt**.
    
    Das Dialogfeld **Neues Projekt hinzufügen** wird angezeigt.
    
  
3. Wählen Sie im Dialogfeld **Neues Projekt hinzufügen** unter dem Knoten **Visual c#** den **Silverlight für Windows Phone**-Knoten.
    
  
4. Wählen Sie im Bereich **Vorlagen** die Vorlage Windows Phone SharePoint List Application.
    
  
5. Benennen der app, beispielsweise OrdersSPListApp, und klicken Sie dann auf **OK**.
    
  
6. Gehen Sie wie beschrieben in  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md) zu einem anderen Windows Phone-app-Projekt erstellen, Angeben von einer SharePoint-Liste definierten basierend auf der Spalten und Feld Typen anzeigen in Tabelle 1 als der Zielliste für das Projekt. Sie haben zwei Projekte jetzt in Ihrer Lösung, mit der Bezeichnung "CustomersSPListApp" und "OrdersSPListApp" (Wenn Sie die Namenskonventionen in diesem Verfahren folgen).
    
  
7. Wählen Sie im **Projektmappen-Explorer** den Projektknoten CustomerSPListApp ein.
    
  
8. Wählen Sie im Menü **Projekt** **Verweis hinzufügen** aus.
    
    Das Dialogfeld **Verweis hinzufügen** wird angezeigt.
    
  
9. Klicken Sie auf der Registerkarte **Projekte** wählen Sie das Projekt OrdersSPListApp in der Lösung, und wählen Sie dann auf die Schaltfläche **OK**. Das Projekt wird unter dem Knoten **Verweise** des Projekts CustomersSPListApp hinzugefügt.
    
  
Konfigurieren Sie anschließend die beiden Projekte in der Lösung an. Sie konfigurieren im Wesentlichen das OrdersSPListApp-Projekt (basierend auf der Orders-Liste) als "Suche" Projekt für das CustomerSPListApp-Projekt (basierend auf der Liste für Kunden) ausgeführt werden.
  
    
    

### So konfigurieren Sie das Projekt OrdersSPListApp


1. Ändern Sie die Navigationspfade in den Formularen anzeigen des Projekts OrdersSPListApp, um den primären Namespace des Projekts ("OrdersSPListApp") und die Bezeichnung "Komponente" enthalten. Beispielsweise im Ereignishandler für das OnNewButtonClick-Ereignis in der Datei List.xaml.cs des Projekts OrdersSPListApp, ändern Sie den Anruf an die Navigate-Methode des NavigationService-Objekts aus diesem:
    
     `NavigationService.Navigate(new Uri("/Views/NewForm.xaml", UriKind.Relative));`
    
    in:
    
     `NavigationService.Navigate(new Uri("/OrdersSPListApp;component/Views/NewForm.xaml", UriKind.Relative));`
    
    Die einfachste Möglichkeit, diese Änderungen vornehmen ist den Befehl **Quick Replace** im Projekt OrdersSPListApp verwendet.
    
1. Wählen Sie im **Projektmappen-Explorer** den Projektknoten OrdersSPListApp ein.
    
  
2. Drücken Sie STRG + H, um das Dialogfeld **Quick Replace** anzuzeigen.
    
  
3. Geben Sie in das Feld **Suchen nach** Text an, der folgende Text genau wie er hier angezeigt wird:
    
    Der URI ("/Views/
    
  
4. Geben Sie den folgenden Text in das Textfeld **Ersetzen durch** genau wie hier angezeigt:
    
    Uri("/OrdersSPListApp;component/Views/
    
  
5. Stellen Sie sicher, dass das **Aktuelle Projekt** in der Dropdownliste **Suchen in** ausgewählt ist.
    
  
6. Wählen Sie **Alle ersetzen**.
    
  
7. Speichern Sie alle geänderte Dateien im Projekt.
    
  
2. Fügen Sie eine Elementeigenschaft in die Datei App.xaml.cs des Projekts OrdersSPListApp. Wählen Sie im **Projektmappen-Explorer** unter den Projektknoten OrdersSPListApp Objektebene.
    
  
3. Taste(n)F7um seine zugeordnete Code-Behind-Datei App.xaml.cs, zur Bearbeitung zu öffnen.
    
  
4. Fügen Sie innerhalb der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die partiellen Klasse **App** implementiert wird, den folgenden Code ein.
    
  ```cs
  
public static string CustomerName { get; set; }
  ```

5. Wählen Sie im **Projektmappen-Explorer** unter den Projektknoten OrdersSPListApp die List.xaml-Datei ein.
    
  
6. Taste(n)F7um seine zugeordnete Code-Behind-Datei List.xaml.cs, zur Bearbeitung zu öffnen.
    
  
7. Ändern des **OnNavigatedTo** -ereignishandlers in der Datei analysiert die **QueryString** -Eigenschaft des **NavigationContext** -Objekts, um den Wert der in Schritt 4 deklarierten Variablen **CustomerName** festzulegen. Sie können auch die **Header** -Eigenschaft des Steuerelements **PivotItem** des Listenformulars entsprechend den Namen des Kunden für die Nutzung durch die Benutzer festlegen. Der geänderte Handler sollte wie folgt aussehen.
    
  ```cs
  protected override void OnNavigatedTo(System.Windows.Navigation.NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    if (this.NavigationContext.QueryString.ContainsKey("CustomerName"))
    {
        App.CustomerName = NavigationContext.QueryString["CustomerName"];
    }

    // Also set the value of the Header property for the PivotItem to match the customer name.
    if (!string.IsNullOrWhiteSpace(App.CustomerName))
    {
        this.View1.Header = App.CustomerName;
    }

    App.MainViewModel.ViewDataLoaded += new EventHandler<ViewDataLoadedEventArgs>(OnViewDataLoaded);
    App.MainViewModel.InitializationCompleted += new EventHandler<InitializationCompletedEventArgs>(OnViewModelInitialization);
}
  ```

8. Fügen Sie die **CustomerName** -Variable als Argument im Aufruf der **LoadData** -Methode in der **OnPivotItemLoaded** -Ereignishandler in der Datei List.xaml.cs. Die Implementierung der **OnPivotItemLoaded** -Ereignishandler sollte wie folgt aussehen.
    
  ```cs
  
private void OnPivotItemLoaded(object sender, PivotItemEventArgs e)
{
    if (!App.MainViewModel.IsInitialized)
    {
        //Initialize ViewModel and Load Data for PivotItem upon initialization.
        App.MainViewModel.Initialize();
    }
    else
    {
        //Load Data for the currently loaded PivotItem.
        App.MainViewModel.LoadData(e.Item.Name, App.CustomerName);
    }
}
  ```


    Die **LoadData** -Methode der **ListViewModel** -Klasse in der Vorlage ist wie definiert, optionale Parameter akzeptieren kann.
    
  
9. Fügen Sie auch die **CustomerName** -Variable als Argument im Aufruf der **LoadData** -Methode in der **OnViewModelInitialization** -Ereignishandler hinzu. Die Implementierung der **OnViewModelInitialization** -Ereignishandler sollte wie folgt aussehen.
    
  ```cs
  
private void OnViewModelInitialization(object sender, InitializationCompletedEventArgs e)
{
    this.Dispatcher.BeginInvoke(() =>
    {
        //If initialization has failed, show error message and return.
        if (e.Error != null)
        {
            MessageBox.Show(e.Error.Message, e.Error.GetType().Name, MessageBoxButton.OK);
            return;
        }
        App.MainViewModel.LoadData(((PivotItem)Views.SelectedItem).Name, App.CustomerName);
        this.DataContext = (sender as ListViewModel);
    });
}
  ```

10. Fügen Sie die **CustomerName** -Variable als Argument im Aufruf der **RefreshData** -Methode in der **OnRefreshButtonClick** -Ereignishandler in der Datei List.xaml.cs. Die Implementierung der **OnRefreshButtonClick** -Ereignishandler sollte wie folgt aussehen.
    
  ```cs
  
private void OnRefreshButtonClick(object sender, EventArgs e)
{
    if (Views.SelectedItem == null)
        return;

    if (!App.MainViewModel.IsInitialized)
    {
        //Initialize ViewModel and Load Data for PivotItem upon completion.
        App.MainViewModel.Initialize();
    }
    else
    {   //Refresh Data for the currently loaded PivotItem.
        App.MainViewModel.RefreshData(((PivotItem)Views.SelectedItem).Name, App.CustomerName);
    }
}
  ```


Wie bei der **LoadData** -Methode wird die **RefreshData** -Methode auch definiert, um optionale Parameter angenommen werden. Beachten Sie, dass die vorherigen drei Schritte, die einzige Änderung an den Ereignishandler generiert mit der Vorlage ist das Hinzufügen der Variablen **CustomerName** als Argument für den Anruf an die Methoden **LoadData** oder **RefreshData**.
    
  
11. Wenn Benutzer auf das Listenformular für die Orders-Liste auf die Schaltfläche **neu** in Ihrer app auswählen können, sollte das Feld Kunde in das neue Formular bereits den Namen des Kunden, enthalten, da die Liste der Aufträge angezeigt, die dem Benutzer basierend auf den Namen des Kunden gefiltert wurde. Neue Aufträge aus der gefilterten Liste hinzugefügt sollte der Name des Kunden zugeordnet sein, auf denen die Liste gefiltert wird. Um den Wert der Variablen **CustomerName** an das neue Formular zu übergeben, ändern Sie das **OnNewButtonClick** -Ereignis, um den Wert als Zeichenfolge in der Pfad zum neuen Formular enthalten wie im folgenden Code dargestellt.
    
  ```cs
  
private void OnNewButtonClick(object sender, EventArgs e)
{
    //Instantiate a new instance of NewItemViewModel and go to NewForm.
    App.MainViewModel.CreateItemViewModelInstance = new NewItemViewModel { DataProvider = App.DataProvider };
    
    if (!string.IsNullOrWhiteSpace(App.CustomerName))
    {
        NavigationService.Navigate(new Uri("/OrdersSPListApp;component/Views/NewForm.xaml?CustomerName=" + 
                                                                            App.CustomerName, UriKind.Relative));
    }
    else
    {
        NavigationService.Navigate(new Uri("/OrdersSPListApp;component/Views/NewForm.xaml", UriKind.Relative));
    }
}
  ```

12. Überprüfen Sie die Abfragezeichenfolge für einen Kundennamen im **OnNavigatedTo** -Ereignishandler für das neue Formular und falls es zur Verfügung steht, weisen Sie das Feld Kunde, der das Ansichtsmodell für das Formular. Klicken Sie im **Projektmappen-Explorer** unter dem Projekt OrdersSPListApp wählen Sie die Datei NewForm.xaml und drücken SieF7um seine zugeordnete Code-Behind-Datei NewForm.xaml.cs, zur Bearbeitung zu öffnen.
    
  
13. Ändern des **OnNavigatedTo** -ereignishandlers in der Datei mit den folgenden Code übereinstimmt.
    
  ```cs
  
protected override void OnNavigatedTo(System.Windows.Navigation.NavigationEventArgs e)
{
    base.OnNavigatedTo(e);

    if (this.NavigationContext.QueryString.ContainsKey("CustomerName"))
    {
        this.viewModel["Customer"] = NavigationContext.QueryString["CustomerName"];
    }

    viewModel.ItemCreated += new EventHandler<ItemCreatedEventArgs>(OnItemCreated);
}
  ```

14. Fügen Sie in der **CamlQueryBuilder** -Klasse in der Datei ListDataProvider.cs im Projekt OrdersSPListApp **WHERE** -Klausel auf das Feld Kunde die CAML-Abfrage zum Abrufen von Elementen aus der Orders-Liste zum Filtern der Liste basierend auf einen bestimmten Kundennamen (aus der Variablen **CustomerName** ) verwendet. Fügen Sie einen Parameter an die **GetCamlQuery** -Methode in der Klasse für die Übergabe von des Kundennamens ein. Die geänderte **CamlQueryBuilder** Klasse sollte wie folgt aussehen.
    
  ```cs
  
public static class CamlQueryBuilder
{
    static Dictionary<string, string> ViewXmls = new Dictionary<string, string>()
    {   
        {"View1", @"<View><Query>{0}</Query><RowLimit>30</RowLimit><ViewFields>{1}</ViewFields></View>"}
    };

    static string ViewFields = @"<FieldRef Name='Title'/><FieldRef Name='Unit_x0020_Price'/><FieldRef Name='Quantity'/><FieldRef Name='Order_x0020_Value'/><FieldRef Name='Order_x0020_Date'/><FieldRef Name='Status'/><FieldRef Name='Customer'/>";

    public static CamlQuery GetCamlQuery(string viewName, string customerName)
    {
        string queryClause = string.Empty;

        // Create appropriate Query Clause, depending on customerName parameter.
        if (string.IsNullOrWhiteSpace(customerName))
        {
            queryClause = "<OrderBy><FieldRef Name='ID' /></OrderBy>";
        }
        else
        {
            queryClause = string.Format("<Where><Eq><FieldRef Name='Customer' /><Value Type='Text'>{0}</Value></Eq></Where>", customerName);
        }

        // Add Query Clause and ViewFields to ViewXml.
        string viewXml = ViewXmls[viewName];
        viewXml = string.Format(viewXml, queryClause, ViewFields);

        return new CamlQuery { ViewXml = viewXml };
    }
}
  ```

15. Ändern Sie die **LoadDataFromServer** -Methode in der Datei ListDataProvider.cs für das Argument **CustomerName** prüfen und das Argument an die **GetCamlQuery** -Methode übergeben. Die geänderte-Methode sollte wie folgt aussehen.
    
  ```cs
  
private void LoadDataFromServer(string ViewName, Action<LoadViewCompletedEventArgs>
                                              loadItemCompletedCallback, params object[] filterParameters)
{
    string customerName = string.Empty;
    string cacheKey = ViewName;

    // Parse the optional parameters:
    if (filterParameters.Length > 0)
    {
        customerName = filterParameters[0].ToString();
        cacheKey += "-" + customerName;
    }

    CamlQuery query = CamlQueryBuilder.GetCamlQuery(ViewName, customerName);
    ListItemCollection items = Context.Web.Lists.GetByTitle(ListTitle).GetItems(query);
    Context.Load(items);
    Context.Load(items, listItems => listItems.Include(item => item.FieldValuesAsText));

    Context.ExecuteQueryAsync(
        delegate(object sender, ClientRequestSucceededEventArgs args)
        {
            base.CacheView(cacheKey, items);
            loadItemCompletedCallback(new LoadViewCompletedEventArgs { ViewName = ViewName, Items = base.GetCachedView(cacheKey) });
        },
        delegate(object sender, ClientRequestFailedEventArgs args)
        {
            loadItemCompletedCallback(new LoadViewCompletedEventArgs { Error = args.Exception });
        });
}
  ```

16. Ändern Sie entsprechend die **LoadData** -Methode in der Datei ListDataProvider.cs zum Verarbeiten des **CustomerName** -Parameters.
    
  ```cs
  
public override void LoadData(string ViewName, Action<LoadViewCompletedEventArgs>
                                                           loadViewCompletedCallback, params object[] filterParameters)
{
    string customerName = string.Empty;
    string cacheKey = ViewName;

    // Parse the optional parameters:
    if (filterParameters.Length > 0)
    {
        customerName = filterParameters[0].ToString();
        cacheKey += "-" + customerName;
    }

    List<ListItem> CachedItems = GetCachedView(cacheKey);
    if (CachedItems != null)
    {
        loadViewCompletedCallback(new LoadViewCompletedEventArgs { ViewName = ViewName, Items = CachedItems });
        return;
    }

    LoadDataFromServer(ViewName, loadViewCompletedCallback, filterParameters);
}
  ```

17. Hinzufügen einer Schaltfläche **Abbrechen** auf das **ApplicationBar** -Element in der Datei List.xaml im Projekt OrdersSPListApp. Klicken Sie im **Projektmappen-Explorer** unter dem Knoten OrdersSPListApp wählen Sie die Datei List.xaml, und drücken SieUMSCHALT + F7um die Datei zur Bearbeitung im Designer zu öffnen.
    
  
18. Fügen Sie XAML, um die Schaltfläche **Abbrechen** innerhalb des Tags `<phone:PhoneApplicationPage.ApplicationBar>` deklarieren, wie im folgenden Markup gezeigt.
    
  ```
  
<phone:PhoneApplicationPage.ApplicationBar>
    <shell:ApplicationBar IsVisible="True" IsMenuEnabled="True">
        <shell:ApplicationBarIconButton x:Name="btnNew" 
                 IconUri="/Images/appbar.new.rest.png" Text="New" Click="OnNewButtonClick"/>
        <shell:ApplicationBarIconButton x:Name="btnRefresh" IconUri="/Images/appbar.refresh.rest.png" 
                 Text="Refresh" IsEnabled="True" Click="OnRefreshButtonClick"/>
        <shell:ApplicationBarIconButton x:Name="btnCancel" IconUri="/Images/appbar.cancel.rest.png" Text="Cancel" IsEnabled="True" Click="OnCancelButtonClick" />
    </shell:ApplicationBar>
</phone:PhoneApplicationPage.ApplicationBar>
  ```

19. Die List.xaml-Datei im **Projektmappen-Explorer** ausgewählt und drücken SieF7um die zugeordneten Code-Behind-Datei List.xaml.cs, zur Bearbeitung zu öffnen.
    
  
20. Innerhalb der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die partiellen Klasse **ListForm** implementiert wird, fügen Sie den folgenden Ereignishandler für das **OnCancelButtonClick** -Ereignis hinzu.
    
  ```cs
  
private void OnCancelButtonClick(object sender, EventArgs e)
{
    NavigationService.Navigate(new Uri("/CustomersSPListApp;component/Views/DisplayForm.xaml", UriKind.Relative));
}
  ```

21. Speichern Sie die Dateien im Projekt.
    
  
Nun, bleibt sie zum Hinzufügen einer Schaltfläche zum Anzeigeformular in CustomersSPListApp Projekts, um die Aufträge mit einem bestimmten Kunden anzuzeigen.
  
    
    

### So konfigurieren Sie das Projekt CustomersSPListApp


1. Wählen Sie im **Projektmappen-Explorer** unter dem Knoten für das Projekt CustomersSPListApp die DisplayForm.xaml-Datei aus.
    
  
2. Taste(n)UMSCHALT + F7(oder doppelklicken Sie auf die Datei) auf die Datei zur Bearbeitung im Designer zu öffnen.
    
  
3. Fügen Sie XAML-Deklarationen für ein Steuerelement **Button** innerhalb einer **StackPanel** Containersteuerelements nach der letzten **StackPanel** Steuerelement-Container für das letzte Feld des Listenelements, wie im folgenden Markup hinzu.
    
  ```
  
...
    <Grid x:Name="LayoutRoot" Background="Transparent" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" xmlns:controls="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone.Controls">
        <StackPanel>
            <ProgressBar Background="Red" x:Name="progressBar" Opacity="1" HorizontalAlignment="Center" 
              VerticalAlignment="Top" Height="15" Width="470" IsIndeterminate="{Binding IsBusy}" 
               Visibility="{Binding ShowIfBusy}" />
            <ScrollViewer HorizontalScrollBarVisibility="Auto" Height="700">
                <Grid x:Name="ContentPanel" Width="470">
                    <StackPanel Margin="0,5,0,5">
                        <StackPanel HorizontalAlignment="Left" Orientation="Horizontal" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
                                           Style="{StaticResource PhoneTextNormalStyle}">Title :</TextBlock>
                            <TextBlock Width="310" HorizontalAlignment="Left" Name="txtTitle"
                                    Text="{Binding [Title]}" TextWrapping="Wrap" Style="{StaticResource PhoneTextSubtleStyle}" />
                                   </StackPanel>
                        <StackPanel HorizontalAlignment="Left" Orientation="Horizontal" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
                                       Style="{StaticResource PhoneTextNormalStyle}">Contact Number :</TextBlock>
                            <TextBlock Width="310" HorizontalAlignment="Left" Name="txtContact_x0020_Number"
                                       Text="{Binding [Contact_x0020_Number]}" TextWrapping="Wrap" 
                                       Style="{StaticResource PhoneTextSubtleStyle}" />
                        </StackPanel>
                        <StackPanel HorizontalAlignment="Left" Orientation="Horizontal" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
                                     Style="{StaticResource PhoneTextNormalStyle}">E-mail Address :</TextBlock>
                            <TextBlock Width="310" HorizontalAlignment="Left" Name="txtE_x002d_mail_x0020_Address" 
                                 Text="{Binding [E_x002d_mail_x0020_Address]}" TextWrapping="Wrap" 
                                             Style="{StaticResource PhoneTextSubtleStyle}" />
                        </StackPanel>
                        <StackPanel HorizontalAlignment="Left" Orientation="Horizontal" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
                                     Style="{StaticResource PhoneTextNormalStyle}">Company :</TextBlock>
                            <TextBlock Width="310" HorizontalAlignment="Left" Name="txtCompany" 
                                     Text="{Binding [Company]}" TextWrapping="Wrap" Style="{StaticResource PhoneTextSubtleStyle}" />
                        </StackPanel>
                        <StackPanel Margin="0,60,0,5"><Button Content="Get Orders" Height="70" Name="OrdersButton" Width="400" Click="OnButtonOrdersClick" /></StackPanel>
                    </StackPanel>
                </Grid>
            </ScrollViewer>
        </StackPanel>
    </Grid>
...
  ```

4. Die DisplayForm.xaml-Datei im **Projektmappen-Explorer** ausgewählt und drücken SieF7um die zugeordneten Code-Behind-Datei DisplayForm.xaml.cs, zur Bearbeitung zu öffnen.
    
  
5. Innerhalb der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die partiellen Klasse **DisplayForm** implementiert wird, fügen Sie den folgenden Ereignishandler für das **OnButtonOrdersClick** -Ereignis hinzu.
    
  ```cs
  
private void OnOrdersButtonClick(object sender, RoutedEventArgs e)
{
    this.NavigationService.Navigate(new Uri("/OrdersSPListApp;component/Views/List.xaml?CustomerName=" + 
                                                                 viewModel["Title"], UriKind.Relative));
}
  ```

6. Speichern Sie die Datei.
    
  
Wenn Sie die Projektmappe erstellen und an den Windows Phone-Emulator bereitstellen, wird das Listenformular für die Liste der Kunden angezeigt. Wenn Sie ein Element in der Liste, um das Anzeigeformular für einen bestimmten Kunden anzeigen auswählen, sehen Sie eine Schaltfläche, um die Bestellungen dieses Kunden anzeigen (Abbildung 1) zugeordneten abrufen.
  
    
    

**Abbildung 1. Anzeigeformular für Kunden**

  
    
    

  
    
    
![Anzeigeformular für Kunden](images/70d5a602-b349-4791-bf05-d61635d0fc91.gif)
  
    
    
Wenn Sie die Schaltfläche **Get Orders** in diesem Formular anzeigen auswählen, werden die Aufträge für den Kunden in der Liste Form aus dem OrdersSPListApp-Projekt in der Projektmappe (Abbildung 2) angezeigt.
  
    
    

**Abbildung 2. Formular ' Bestellliste '**

  
    
    

  
    
    
![Formular der Auftragsliste](images/884d6dcb-c690-4b43-9bb2-d1bcaf4b7a31.gif)
  
    
    
Aus diesem Formular (das Listenformular für die Orders-Liste) können Sie hinzufügen, bearbeiten oder Löschen von Bestellungen für einen Kunden. Wenn Sie die Schaltfläche **Abbrechen** geklickt haben, navigieren Sie zum Listenformular für die Liste der Kunden zurück. In einer einzelnen Phone-app können Sie die Listenelemente aus zwei SharePoint-Listen verwalten.
  
    
    

## Zusätzliche Ressourcen
<a name="SP15Usemultlists_addlresources"> </a>


-  [Vorgehensweise: Konfigurieren und Verwenden von Pushbenachrichtigungen in SharePoint 2013-apps für Windows Phone](how-to-configure-and-use-push-notifications-in-sharepoint-2013-apps-for-windows.md)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 8.0](http://www.microsoft.com/en-us/download/details.aspx?id=35471)
    
  
-  [Microsoft SharePoint SDK für Windows Phone 8](http://www.microsoft.com/en-us/download/details.aspx?id=36818)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  

