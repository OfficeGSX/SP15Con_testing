---
title: Vorgehensweise anmelden und Fortsetzen von Anrufen SharePoint Listenelementen auf einem Telefon mit Windows
ms.prod: SHAREPOINT
ms.assetid: 14ca37a2-5b45-430d-9004-ff3016f89834
---


# Vorgehensweise: anmelden und Fortsetzen von Anrufen SharePoint Listenelementen auf einem Telefon mit Windows
In diesem Artikel erhalten Sie Informationen zum Windows Phone-Anwendungslebenszyklus und zum lokalen Speichern von Netzwerkdaten.
Einer der wichtigsten Aspekte bei der Entwicklung von apps für Windows Phone ist die Verwaltung von Zustandsinformationen, sowohl für die gesamte Anwendung als auch für einzelne Seiten oder Daten Elemente in der Anwendung. Wenn Sie Windows Phone-apps entwickeln, müssen Sie berücksichtigen, dass Benutzer Ihre Apps Konnektivität zu Netzwerkressourcen verlieren möglicherweise (z. B. SharePoint-Listen). Die Entwicklungsinfrastruktur für Windows Phone-apps bietet Mechanismen für die Verarbeitung von Statusinformationen in verschiedenen Phasen im Lebenszyklus einer app.
  
    
    


> **WICHTIG**
> Wenn Sie eine app für Windows Phone 8 entwickeln, müssen Sie anstelle von Visual Studio 2010 Express Visual Studio Express 2012 verwenden. Alle Informationen in diesem Artikel betrifft mit Ausnahme der Entwicklungsumgebung Erstellen von apps für Windows Phone 8 und Windows Phone 7.<br/> Weitere Informationen finden Sie unter  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md).
  
    
    


## Speichern Sie SharePoint-Listendaten in einer Windows Phone lokal
<a name="BKMK_StoringDataLocally"> </a>

Auf einem Windows Phone nur eine app ausgeführt wird, zu einem Zeitpunkt und, wenn ein Benutzer auf einer anderen Anwendung auf dem Telefon umschaltet (durch Drücken der Schaltfläche **Start** auf dem Telefon, beispielsweise), die zurzeit ausgeführte app deaktiviert ist, oder die Begriffe Windows Phone-Entwicklung, dieals veraltet markiert. Wechselt von der Benutzer wieder auf die deaktivierte app (durch Drücken der Schaltfläche **zurück** ), kann die app reaktiviert werden, aber es sei denn, Sie Logik zur Verarbeitung von Anwendungsinformationen Status im Verlauf des Lebenszyklus der app bereitstellen, wird diese Statusinformationen nicht standardmäßig in der Übergang vom Aktivierung, Deaktivierung und wieder zu beibehalten. (Weitere Informationen zum Lebenszyklus Anwendung für Windows Phone-apps finden Sie unter [Ausführung Model Overview for Windows Phone](http://msdn.microsoft.com/en-us/library/ff817008%28v=VS.92%29.aspx).)
  
    
    
Für Windows Phone-apps macht die **PhoneApplicationService** -Klasse standard Lebenszyklusereignisse, die zum Verwalten von Anwendungszustand verwendet werden können. In Projekten, die aus der Vorlage für Windows Phone SharePoint List Application erstellt haben (wie bei der Projekte aus allen **Silverlight für Windows Phone**-Vorlagen erstellt wurden), diese standard Windows Phone-Anwendung Lebenszyklusereignisse in Objektebene deklariert und Ereignishandler in der Code-Behind-Datei App.xaml.cs zugeordnet sind. Das folgende Markup sollte die Deklarationen in Objektebene für Ihre SharePoint-Liste apps entsprechen.
  
    
    



```

<Application.ApplicationLifetimeObjects>
    <!--Required object that handles lifetime events for the application-->
    <shell:PhoneApplicationService 
        Launching="Application_Launching" Closing="Application_Closing"Activated="Application_Activated" Deactivated="Application_Deactivated"/>
</Application.ApplicationLifetimeObjects>
```

Die **Application_Activated** und **Application_Deactivated** -Ereignishandler in Objektebene deklariert werden in der Datei App.xaml.cs CodeBehind-Datei mit Standardlogik implementiert, die Anwendung Statusinformationen für die Verwendung in der Phone-app zwischengespeichert, solange die app nicht beendet wird. Die Implementierung der Handler für diese Ereignisse werden die **State** -Eigenschaft (ermöglicht den Zugriff auf ein Objekt **Dictionary** ) der **PhoneApplicationService** -Klasse zum Speichern von Daten verwendet. In dieser Eigenschaft **State** gespeicherten Daten sind vorübergehende. D. h., wird es beibehalten, wenn die app deaktiviert oder als veraltet markiert ist, jedoch nicht, wenn die app beendet wird. Es ist wichtig, im Hinterkopf behalten, wie die Anwendung Lebenszyklusereignisse in Ihre Projekte behandelt werden, wenn eine Windows-app deaktiviert wird, wenn ein Benutzer auf einer anderen Anwendung umschaltet, das deaktiviert app Beendigung vom Betriebssystem Windows Phone, je nach Umständen fällt. Alle Daten auf dem Telefon, das dauerhaft gespeichert ist nicht geht verloren, auch wenn diese Daten mithilfe der **State** -Eigenschaft von der **PhoneApplicationService**temporärer Speicher gespeichert wurde.
  
    
    
In einer Windows Phone-app, die Daten aus einer SharePoint-Liste zu versehen, können die Daten auf dem Telefon von Sitzung zu Sitzung verwendet natürlich vom Server mit SharePoint Server, abgerufen werden, wenn der Server verfügbar ist. Aber dauerhafte Verbindung zu einem SharePoint Server möglicherweise nicht verfügbar für Windows Phone-Gerät aufgrund von Variationen in Service Abdeckung durch Position und anderen Faktoren. Um Benutzern Ihrer App Zugriff auf Daten im Fall von verlorenen Konnektivität mit dem Server mit SharePoint Server bereitzustellen oder einfach zum Speichern von Daten in einen permanenten Speicher zwischen Sitzungen der app unabhängig von der Verfügbarkeit des Servers, können Sie die Ereignisse **Closing** und **Launching** der Klasse **PhoneApplicationService** nutzen.
  
    
    
Die **Application_Launching** und **Application_Closing** Handler für diese Ereignisse in App.xaml deklariert und in der Datei App.xaml.cs definiert sind, jedoch nicht implementiert werden. So behandeln Sie speichern und Abrufen von Anwendung Zustandsinformationen im Kontext der app-Beendigung, können Sie eine Implementierung für den Ereignishandler **Application_Closing** zum Speichern von Daten in den isolierten Speicher für die app definiert werden, sodass die Daten zwischen Sitzungen der app bleibt, und Sie können eine Implementierung für den Ereignishandler **Application_Launching** zum Abrufen von Daten aus dem isolierten Speicher, wenn eine neue Sitzung der app gestartet wird (wenn die app gestartet wird) bereitstellen bereitstellen , auch wenn die Verbindung mit dem Server mit SharePoint Server, die die ursprüngliche Quelle der Daten ist nicht verfügbar ist.
  
    
    

> **TIPP**
> Daten sollten verschlüsselt werden, bevor Sie es in einem lokalen Gerät speichern. Weitere Informationen zum Verschlüsseln von Daten finden Sie unter  [Vorgehensweise: Verschlüsseln von Daten in einer Windows Phone-Anwendung](http://msdn.microsoft.com/en-us/library/hh487164%28v=vs.92%29.aspx)
  
    
    


### Implementieren der Ereignishandler für das Speichern und Abrufen von Anwendungszustand


1. Erstellen einer Windows Phone-app mithilfe von Windows Phone SharePoint List Application-Vorlage in Visual Studio durch die Schritte in  [Vorgehensweise: Erstellen eine Windows Phone SharePoint 2013 Liste app](how-to-create-a-windows-phone-sharepoint-2013-list-app.md).
    
  
2. Wählen Sie im **Projektmappen-Explorer** Objektebene.
    
  
3. Taste(n)F7um die CodeBehind-Datei App.xaml.cs, zur Bearbeitung zu öffnen.
    
  
4. Suchen Sie die Implementierung der **Application_Launching** -Ereignishandler (leere), und Ersetzen Sie den Ereignishandler durch den folgenden Code.
    
  ```cs
  
private void Application_Launching(object sender, LaunchingEventArgs e)
{
    if (IsolatedStorageSettings.ApplicationSettings.Contains(DataProvider.ListTitle))
    {
        App.MainViewModel = (ListViewModel)IsolatedStorageSettings.ApplicationSettings
                                              [DataProvider.ListTitle];                
        App.MainViewModel.Initialize();
    }
}
  ```

5. Suchen Sie die Implementierung der **Application_Closing** -Ereignishandler (leere), und Ersetzen Sie diesen Ereignishandler durch den folgenden Code.
    
  ```cs
  
private void Application_Closing(object sender, ClosingEventArgs e)
{
    if (IsolatedStorageSettings.ApplicationSettings.Contains(DataProvider.ListTitle))
    {
        IsolatedStorageSettings.ApplicationSettings[DataProvider.ListTitle] = App.MainViewModel;
    }
    else
    {
        IsolatedStorageSettings.ApplicationSettings.Add(DataProvider.ListTitle, App.MainViewModel);
    }
    IsolatedStorageSettings.ApplicationSettings.Save();
}
  ```

6. Speichern Sie die Datei.
    
  
Mit dieser Implementierung direkten Ausführen Ihrer app, um die Haupt-ViewModel in der app mit Daten vom Server mit SharePoint Server zu initialisieren. Beenden Sie die app auf dem Telefon (durch Drücken der Schaltfläche **Sichern**, um nach der ersten Seite der app zu navigieren) auf das **Application_Closing** -Ereignis ausgelöst. Wenn Sie Ihre app ohne Verbindung mit dem Server ausführen, ist das Ansichtsmodell, das auf das **IsolatedStorageSettings** **Dictionary** -Objekt (im Ereignis **Application_Closing** ) gespeichert wurde abgerufen und initialisiert. Die SharePoint-Listenelemente, die in einer vorherigen Sitzung der app auf isolierten Speicher gespeichert wurden, werden in das Listenformular (List.xaml) der app angezeigt.
  
    
    

## Implementieren Sie einen Mechanismus zum Bearbeiten von Listenelementen offline
<a name="BKMK_ImplementingOfflineEditing"> </a>

Wenn Sie die Schritte im vorherigen Abschnitt, um Handler für die Ereignisse **Closing** und **Launching** in Ihrer app implementieren können SharePoint-Listendaten, die vom Server abgerufen wurde, wenn Connectivity verfügbar war in Ihrer app angezeigt werden, auch wenn die Verbindung mit dem Server in einer späteren Sitzung der app, verloren gehen, da die Listenelemente aus dem lokalen beständiger Speicher auf dem Telefon abgerufen werden. Basierend auf der Implementierung im vorherigen Abschnitt, jedoch die Listenelemente auf diese Weise für die Anzeige während offline bearbeitet und, wenn die Verbindung wiederhergestellt wird wieder auf dem Server gespeichert werden kann nicht zur Verfügung gestellt. Im folgenden Verfahren fügen Sie einen Mechanismus für Ihre Anwendung zum Speichern von bearbeitete Versionen von Listenelementen lokal, wenn Verbindung nicht verfügbar ist. Wenn die Verbindung mit dem Server wieder verfügbar ist, können diese bearbeiteten Listenelemente abrufen und speichern Sie die Änderungen an den Server zurückgesendet.
  
    
    
Für die Verfahren in diesem Abschnitt wird davon ausgegangen, dass Sie arbeiten im Zusammenhang mit einer Windows Phone-app-Projekt aus der Vorlage für Windows Phone SharePoint List Application erstellt und, dass Ihre app auf einer Bestellungen Liste aus der Vorlage benutzerdefinierte Liste auf dem Server erstellt basiert und enthält die Spalten und Feldtypen in Tabelle 1 dargestellt sind.
  
    
    

**In Tabelle 1. Beispielliste Bestellungen**


|**Spalte**|**Typ**|**Erforderlich**|
|:-----|:-----|:-----|
|Produkt (beispielsweise "Titel") <br/> |Einzelne Textzeile (Text) <br/> |Ja <br/> |
|Beschreibung <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
|Anzahl <br/> |Zahl <br/> |Ja <br/> |
|Bestelldatum <br/> |Datum und Uhrzeit (DateTime) <br/> |Nein <br/> |
|Auftragserfüllung Datum <br/> |Datum und Uhrzeit (DateTime) <br/> |Nein <br/> |
|Rufnummer <br/> |Einzelne Textzeile (Text) <br/> |Nein <br/> |
   

### Implementieren eine Klasse zur Unterstützung der Bearbeitung Elemente beim offline


1. Beginnend mit einem Visual Studio Projekt, das erstellt wurde, basierend auf der Bestellungen Liste dargestellt durch die in Tabelle 1, klicken Sie im **Projektmappen-Explorer**, wählen Sie den Knoten, der das Projekt (beispielsweise SPListAppLocalStorage) darstellt.
    
  
2. Wählen Sie im Menü **Projekt** **Klasse hinzufügen** aus.
    
    Das Dialogfeld **Neues Element hinzufügen** wird angezeigt, mit der ausgewählten Vorlage C#- **Klasse**.
    
  
3. Nennen Sie die Klassendatei DraftItemStore.cs, und wählen Sie dann auf **Hinzufügen**.
    
    Die Klassendatei ist dem Projekt hinzugefügt und zur Bearbeitung geöffnet.
    
  
4. Ersetzen Sie den Inhalt der Klassendatei durch den folgenden Code ein.
    
  ```cs
  
using System;
using System.Net;
using System.Windows;
using System.Collections.Generic;
using System.IO.IsolatedStorage;

namespace SPListAppLocalStorage // Based on project name by default.
{
    public class DraftItemStore
    {
        const string DraftsKey = "Drafts";

        public static void AddDraftItem(string id, EditItemViewModel model)
        {
            Dictionary<string, EditItemViewModel> draftCollection = GetDraftItemCollection();
            draftCollection[id] = model;
            SaveDrafts(draftCollection);
        }

        public static void RemoveDraftItem(string id)
        {
            Dictionary<string, EditItemViewModel> draftCollection = GetDraftItemCollection();
            draftCollection.Remove(id);
            SaveDrafts(draftCollection);
        }

        public static void SaveDrafts(Dictionary<string, EditItemViewModel> draft)
        {
            if (IsolatedStorageSettings.ApplicationSettings.Contains(DraftsKey))
            {
                IsolatedStorageSettings.ApplicationSettings[DraftsKey] = draft;
            }
            else
            {
                IsolatedStorageSettings.ApplicationSettings.Add(DraftsKey, draft);
            }
        }

        public static List<EditItemViewModel> Drafts
        {
            get
            {
                Dictionary<string, EditItemViewModel> draftCollection = GetDraftItemCollection();

                List<EditItemViewModel> modelCollection = new List<EditItemViewModel>();
                foreach (KeyValuePair<string, EditItemViewModel> entry in draftCollection)
                {
                    modelCollection.Add(entry.Value);
                }

                return modelCollection;
            }
        }

        public static Dictionary<string, EditItemViewModel> GetDraftItemCollection()
        {
            Dictionary<string, EditItemViewModel> draftCollection = null;
            if (IsolatedStorageSettings.ApplicationSettings.Contains(DraftsKey))
                draftCollection = (Dictionary<string,
                EditItemViewModel>)IsolatedStorageSettings.ApplicationSettings[DraftsKey];

            if (draftCollection == null)
                draftCollection = new Dictionary<string, EditItemViewModel>();

            return draftCollection;
        }

        public static EditItemViewModel GetDraftItemById(string id)
        {
            Dictionary<string, EditItemViewModel> draftCollection = GetDraftItemCollection();
            return !draftCollection.ContainsKey(id) ? null : draftCollection[id];
        }
    }
}
  ```


In diesem Code angegebene Namespace basiert auf den Namen des Projekts (in diesem FallSPListAppLocalStorage ). Möglicherweise möchten einen anderen Namespace, basierend auf den Namen Ihres Projekts angeben.
    
  
5. Speichern Sie die Datei.
    
  
Eine bestimmte Instanz der **EditItemViewModel** -Klasse stellt einen SharePoint-Listenelement, das auf dem Telefon bearbeitet wird. Sie können ein Listenelement in Betracht ziehen, das geändert wurde, wie ein "Entwurf Element" vor Änderung für das Element auf dem Server gespeichert sind. Im Code in dieser Klasse fügt die **AddDraftItem** -Methode eine bestimmte Instanz der Klasse **EditItemViewModel** (d. h., ein Element Entwurf) als Wert auf ein **Dictionary** -Objekt, das **EditItemViewModel** in der **Dictionary** mit einem Schlüssel auf Grundlage des Bezeichners für das angegebene Listenelement zuordnen. (Ein Bezeichner wird vom SharePoint Server an jedes Element in einer Liste zugewiesen. In einem Projekt basierend auf der Vorlage Windows Phone SharePoint List Application wird dieser Bezeichner gespeichert in der **ID** -Eigenschaft der angegebenen **ViewModel** -Klasse, wie **EditItemViewModel** oder **DisplayItemViewModel**, die das Listenelement darstellt.) Die **RemoveDraftItem** -Methode entfernt ein **EditItemViewModel** aus dem **Dictionary** -Objekt basierend auf einem angegebenen Bezeichner. Sowohl der folgenden Methoden verwenden Sie die **GetDraftItemCollection** -Methode, um das **Dictionary** -Objekt mit den Objekten **EditItemViewModel** aus dem isolierten Speicher abzurufen und beide Methoden die **SaveDrafts** -Methode verwenden, um das geänderte **Dictionary** -Objekt (mit einem Entwurf Element entweder hinzugefügt oder daraus entfernt) auf den isolierten Speicher zu speichern. Die **GetDraftItemCollection** -Methode bestimmt zunächst, ob ein "Entwürfe" **Dictionary** -Objekt in isolierten Speicher gespeichert wurde. Ist dies der Fall ist, gibt die Methode dieses **Dictionary** -Objekt zurück. Andernfalls wird die Methode initialisiert und gibt ein neues **Dictionary** -Objekt. Die **Drafts** -Eigenschaft der Klasse ermöglicht den Zugriff auf die **Dictionary** der Entwurfselemente durch Zurückgeben einer Liste (d. h., ein Objekt basierend auf der **List<T>** generischen) der Entwurfselemente als **EditItemViewModel** -Objekte. Die **GetDraftItemById** -Methode gibt einen bestimmten Entwurf-Element aus dem **Dictionary** -Objekt basierend auf dem angegebenen Bezeichner-Wert zurück.
  
    
    
Nun können Sie die Benutzeroberfläche der app Telefon Elemente hinzugefügt und so werden konfiguriert, um die **DraftItemStore** -Klasse für die Bearbeitung von Listenelementen offline zu verwenden. In den folgenden Verfahren können Sie folgende Aktionen ausführen:
  
    
    

- Hinzufügen und Konfigurieren einer Windows Phone-Seite, um alle Elemente anzuzeigen, die als Entwurfselemente isolierten Speicher auf dem Telefon gespeichert wurden.
    
  
- Hinzufügen und konfigurieren Sie eine andere Seite, die an ein **EditItemViewModel**, zum Bearbeiten eines Elements einzelne Entwurf, vergleichbar mit dem Formular bearbeiten (EditForm.xaml) für Listenelemente gebunden.
    
  
- Fügen Sie eine Methode, **SaveAsDraft**, mit der **EditItemViewModel** -Klasse, die die **AddDraftItem** -Methode der **DraftItemStore** -Klasse implementiert, die im vorherigen Verfahren ausgeführt wird.
    
  
- Hinzufügen einer **ApplicationBar** -Schaltfläche in der Datei EditForm.xaml die **SaveAsDraft** -Methode aufrufen.
    
  
- Hinzufügen einer Schaltfläche **ApplicationBar** zur Datei List.xaml, navigieren zur Seite, die alle als Entwürfe gespeicherte Listenelementen anzeigt.
    
  

### So fügen Sie einer Seite zum Anzeigen aller auf dem Telefon gespeichert Entwurfselemente hinzu


1. Wählen Sie im **Projektmappen-Explorer** den Ordner " **Ansichten** " aus.
    
  
2. Wählen Sie im Menü **PROJEKT** die Option **Neues Element hinzufügen** aus.
    
    Das Dialogfeld **Neues Element hinzufügen** wird geöffnet.
    
  
3. Wählen Sie im Dialogfeld **Neues Element hinzufügen** unter den Knoten **Visual c#** den **Silverlight für Windows Phone**-Knoten.
    
  
4. Wählen Sie im Bereich **Vorlagen** die Vorlage **Windows Phone-Hochformat**.
    
  
5. Nennen Sie die Datei Drafts.xaml, und wählen Sie dann auf **Hinzufügen**.
    
    Die Datei wird unter dem Knoten **Ansichten** dem Projekt hinzugefügt und zur Bearbeitung geöffnet.
    
  
6. Ersetzen Sie in der XAML-Bereich des Designers den Inhalt der Datei mit den folgenden XAML-Code.
    
  ```
  
<phone:PhoneApplicationPage
    x:Class="SPListAppLocalStorage.Views.Drafts"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:phone="clr-namespace:Microsoft.Phone.Controls;assembly=Microsoft.Phone"
    xmlns:shell="clr-namespace:Microsoft.Phone.Shell;assembly=Microsoft.Phone"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    FontFamily="{StaticResource PhoneFontFamilyNormal}"
    FontSize="{StaticResource PhoneFontSizeNormal}"
    Foreground="{StaticResource PhoneForegroundBrush}"
    SupportedOrientations="Portrait" Orientation="Portrait"
    mc:Ignorable="d" d:DesignHeight="696" d:DesignWidth="480"
    shell:SystemTray.IsVisible="True">

    <!--LayoutRoot is the root grid where all page content is placed-->
    <Grid x:Name="LayoutRoot" Background="Transparent">
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <!--TitlePanel contains the name of the application and page title-->
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock x:Name="ApplicationTitle" Text="Product Orders" 
                                    Style="{StaticResource PhoneTextNormalStyle}"/>
            <TextBlock x:Name="PageTitle" Text="Draft Items" Margin="9,-7,0,0" 
                                      Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>

        <!--ContentPanel - place additional content here-->
        <Grid x:Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <ListBox x:Name="lstBoxDraftItems" ItemsSource="{Binding}"
                                  SelectionChanged="lstBoxDraftItems_SelectionChanged">
                <ListBox.ItemTemplate>
                    <DataTemplate>
                        <StackPanel>
                            <TextBlock Text="{Binding [Title]}" Style="
                                           {StaticResource PhoneTextTitle2Style}"></TextBlock>
                            <TextBlock Text="{Binding [Description]}" Style="
                                            {StaticResource PhoneTextNormalStyle}"></TextBlock>
                            <TextBlock Text="{Binding [Contact_x0020_Number]}" Style="
                                           {StaticResource PhoneTextNormalStyle}"></TextBlock>
                        </StackPanel>
                    </DataTemplate>
                </ListBox.ItemTemplate>
            </ListBox>
        </Grid>
    </Grid>
 
    <phone:PhoneApplicationPage.ApplicationBar>
        <shell:ApplicationBar IsVisible="True" IsMenuEnabled="True">
            <shell:ApplicationBarIconButton x:Name="btnCancel" 
             IconUri="/Images/appbar.cancel.rest.png" Text="Cancel" Click="OnCancelButtonClick" />
        </shell:ApplicationBar>
    </phone:PhoneApplicationPage.ApplicationBar>

</phone:PhoneApplicationPage>
  ```


Der Wert der Namespace Bezeichnung  `<x:Class>` in diesem Code ("SPListAppLocalStorage.Views.Drafts") variiert je nach den Namen Ihres Projekts.
    
  
7. Die Drafts.xaml-Datei im **Projektmappen-Explorer** ausgewählt und drücken SieF7um die zugeordneten Code-Behind-Datei Drafts.xaml.cs, zur Bearbeitung zu öffnen.
    
  
8. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:
    
  ```cs
  
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Animation;
using System.Windows.Shapes;
using Microsoft.Phone.Controls;

namespace SPListAppLocalStorage.Views
{
    public partial class Drafts : PhoneApplicationPage
    {
        public Drafts()
        {
            InitializeComponent();
            this.Loaded += new RoutedEventHandler(Drafts_Loaded);
        }

        private void lstBoxDraftItems_SelectionChanged(object sender, SelectionChangedEventArgs e)
        {
            ListBox lstBox = sender as ListBox;
            if (lstBox.SelectedIndex == -1)
                return;

            EditItemViewModel selectedDraftItem = lstBox.SelectedItem as EditItemViewModel;
            NavigationService.Navigate(new Uri(string.Format("/Views/DraftItemEditForm.xaml?ID={0}",
                                                   selectedDraftItem.ID), UriKind.Relative));

            lstBox.SelectedIndex = -1;
        }

        void Drafts_Loaded(object sender, RoutedEventArgs e)
        {
            this.DataContext = DraftItemStore.Drafts;
        }

        private void OnCancelButtonClick(object sender, EventArgs e)
        {
            // Navigate back to initial List View form.
            NavigationService.Navigate(new Uri("/Views/List.xaml", UriKind.Relative));
        }
    }
}
  ```

9. Speichern Sie die Dateien.
    
  

### So fügen Sie eine Seite zum Bearbeiten der einzelnen Entwurfselemente hinzu


1. Wählen Sie im **Projektmappen-Explorer** den Ordner " **Ansichten** " aus.
    
  
2. Wählen Sie im Menü **PROJEKT** die Option **Neues Element hinzufügen** aus.
    
    Das Dialogfeld **Neues Element hinzufügen** wird geöffnet.
    
  
3. Wählen Sie im Dialogfeld **Neues Element hinzufügen** unter den Knoten **Visual c#** den **Silverlight für Windows Phone**-Knoten.
    
  
4. Wählen Sie im Bereich **Vorlagen** die Vorlage **Windows Phone-Hochformat**.
    
  
5. Nennen Sie die Datei DraftItemEditForm.xaml, und wählen Sie dann auf **Hinzufügen**.
    
    Die Datei wird unter dem Knoten **Ansichten** dem Projekt hinzugefügt und zur Bearbeitung geöffnet.
    
  
6. Ersetzen Sie in der XAML-Bereich des Designers den Inhalt der Datei mit den folgenden XAML-Code.
    
  ```
  
<phone:PhoneApplicationPage
    x:Class="SPListAppLocalStorage.DraftItemEditForm"
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
    SupportedOrientations="Portrait" Orientation="Portrait"
    shell:SystemTray.IsVisible="True" x:Name="DraftItemEditPage">

    <!--LayoutRoot is the root grid where all page content is placed-->
    <Grid x:Name="LayoutRoot" Background="Transparent"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml" 
             xmlns:controls="clr-namespace:Microsoft.Phone.Controls;assembly=
                Microsoft.Phone.Controls">
        <StackPanel>
            <ProgressBar Background="Red" x:Name="progressBar" Opacity="1" 
                                    HorizontalAlignment="Center" VerticalAlignment="Top" 
                                    Height="15" Width="470" IsIndeterminate="{Binding IsBusy}" 
                                    Visibility="{Binding ShowIfBusy}" />
            <ScrollViewer HorizontalScrollBarVisibility="Auto" Height="700">
                <Grid x:Name="ContentPanel" Width="470">
                    <StackPanel Margin="0,5,0,5">
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                                Style="{StaticResource PhoneTextNormalStyle}">Product*</TextBlock>
                            <TextBox Style="{StaticResource TextValidationTemplate}" 
                         FontSize="{StaticResource   PhoneFontSizeNormal}" Width="470" 
                         HorizontalAlignment="Left" Name="txtTitle" Text="{Binding [Title], 
                                   Mode=TwoWay,ValidatesOnNotifyDataErrors=True,NotifyOnValidationError=True}" 
                                                                         TextWrapping="Wrap" />
                        </StackPanel>
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                               Style="{StaticResource PhoneTextNormalStyle}">Description</TextBlock>
                            <TextBox Style="{StaticResource TextValidationTemplate}" 
                               FontSize="{StaticResource PhoneFontSizeNormal}" Width="470" 
                                HorizontalAlignment="Left" Name="txtDescription" 
                                                           Text="{Binding [Description],
                                                           Mode=TwoWay, ValidatesOnNotifyDataErrors=True, 
                                                           NotifyOnValidationError=True}" 
                                       TextWrapping="Wrap" />
                        </StackPanel>
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                               Style="{StaticResource PhoneTextNormalStyle}">
                                                           Product Category</TextBlock>
                            <ListBox MaxHeight="400" Width="Auto" x:Name="lstBoxProduct_x0020_Category"
                                              ItemsSource="{Binding [Product_x0020_Category]}">
                                <ListBox.ItemTemplate>
                                    <DataTemplate>
                                        <RadioButton FontSize="{StaticResource PhoneFontSizeNormal}" 
                                          HorizontalAlignment="Left" GroupName="Product_x0020_Category" 
                                                                 Content="{Binding Name}" 
                               IsChecked="{Binding IsChecked, Mode=TwoWay}" />
                                    </DataTemplate>
                                </ListBox.ItemTemplate>
                            </ListBox>
                        </StackPanel>
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                                       Style="{StaticResource PhoneTextNormalStyle}">Quantity*</TextBlock>
                            <TextBox Style="{StaticResource TextValidationTemplate}" 
                                   FontSize="{StaticResource PhoneFontSizeNormal}" Width="470" 
                                      HorizontalAlignment="Left" Name="txtQuantity" Text="{Binding [Quantity], 
                                        Mode=TwoWay, ValidatesOnNotifyDataErrors=True, 
                                           NotifyOnValidationError=True}"
                                             TextWrapping="Wrap" />
                        </StackPanel>
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                                Style="{StaticResource PhoneTextNormalStyle}">Order Date</TextBlock>
                            <TextBox Height="Auto" Style="{StaticResource TextValidationTemplate}"
                                               FontSize="{StaticResource PhoneFontSizeNormal}" Width="470"  
                                                         HorizontalAlignment="Left" Name="txtOrder_x0020_Date" 
                         Text="{Binding [Order_x0020_Date], Mode=TwoWay, ValidatesOnNotifyDataErrors=True, 
                                           NotifyOnValidationError=True}" TextWrapping="Wrap" />
                            <TextBlock FontSize="16" TextWrapping="Wrap" HorizontalAlignment="Left" 
                                                 Style="{StaticResource PhoneTextSubtleStyle}" 
                                                             Text="{Binding DateTimeFormat}" />
                        </StackPanel>
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                         Style="{StaticResource PhoneTextNormalStyle}">Fulfillment Date</TextBlock>
                            <TextBox Height="Auto" Style="{StaticResource TextValidationTemplate}"
                                          FontSize="{StaticResource PhoneFontSizeNormal}" Width="470" 
                                          HorizontalAlignment="Left" Name="txtFulfillment_x0020_Date" 
                        Text="{Binding [Fulfillment_x0020_Date], Mode=TwoWay, 
                     ValidatesOnNotifyDataErrors=True, NotifyOnValidationError=True}" 
                         TextWrapping="Wrap" />
                            <TextBlock FontSize="16" TextWrapping="Wrap" HorizontalAlignment="Left"
                                                    Style="{StaticResource PhoneTextSubtleStyle}" Text="{Binding
                                                           DateTimeFormat}" />
                        </StackPanel>
                        <StackPanel Orientation="Horizontal">
                            <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left"
                                                  Style="{StaticResource PhoneTextNormalStyle}">Rush 
                                                                   :</TextBlock>
                            <CheckBox Name="txtRush" FontSize="{StaticResource PhoneFontSizeNormal}" 
                                          HorizontalAlignment="Left" IsChecked="{Binding [Rush], Mode=TwoWay, 
                               ValidatesOnNotifyDataErrors=True, NotifyOnValidationError=True}" />
                        </StackPanel>
                        <StackPanel Orientation="Vertical" Margin="0,5,0,5">
                            <TextBlock TextWrapping="Wrap" HorizontalAlignment="Left" 
                        Style="{StaticResource PhoneTextNormalStyle}">Contact Number</TextBlock>
                            <TextBox Style="{StaticResource TextValidationTemplate}" 
                                   FontSize="{StaticResource PhoneFontSizeNormal}" Width="470"
                                              HorizontalAlignment="Left" Name="txtContact_x0020_Number"
                                                         Text="{Binding [Contact_x0020_Number], 
                                                         Mode=TwoWay, ValidatesOnNotifyDataErrors=True, 
                                                         NotifyOnValidationError=True}" 
                                                                           TextWrapping="Wrap" />
                        </StackPanel>
                    </StackPanel>
                </Grid>
            </ScrollViewer>
        </StackPanel>
    </Grid>

    <phone:PhoneApplicationPage.ApplicationBar>
        <shell:ApplicationBar IsVisible="True" IsMenuEnabled="True">
            <shell:ApplicationBarIconButton x:Name="btnSubmit" 
                                IconUri="/Images/appbar.save.rest.png" 
                                Text="Submit" Click="OnSubmitButtonClick"/>
            <shell:ApplicationBarIconButton x:Name="btnBack" 
                                IconUri="/Images/appbar.back.rest.png" 
                                 Text="Back to List" Click="OnBackButtonClick"/>
        </shell:ApplicationBar>
    </phone:PhoneApplicationPage.ApplicationBar>

</phone:PhoneApplicationPage>
  ```


Der XAML-Code zum Definieren von dieser Seite ähnelt der EditForm.xaml-Datei. Kopieren Sie die Datei EditForm.xaml als Grundlage für DraftItemEditForm.xaml, die Änderungen an der Datei vornehmen, wie in diesem Markup angegeben verwenden.
    
  
7. Die DraftItemEditForm.xaml-Datei im **Projektmappen-Explorer** ausgewählt und drücken SieF7um die zugeordneten Code-Behind-Datei DraftItemEditForm.xaml.cs, zur Bearbeitung zu öffnen.
    
  
8. Ersetzen Sie den Inhalt der Datei durch den folgenden Code:
    
  ```cs
  
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net;
using System.Windows;
using System.Windows.Controls;
using System.Windows.Documents;
using System.Windows.Input;
using System.Windows.Media;
using System.Windows.Media.Animation;
using System.Windows.Shapes;
using Microsoft.Phone.Controls;
using Microsoft.SharePoint.Client;
using Microsoft.Phone.Tasks;
using System.Device.Location;
using Microsoft.Phone.Shell;
using Microsoft.SharePoint.Phone.Application;

namespace SPListAppLocalStorage
{
    public partial class DraftItemEditForm : PhoneApplicationPage
    {
        EditItemViewModel viewModel;

        /// <summary>
        /// Constructor for Draft Item Edit Form.
        /// </summary>
        public DraftItemEditForm()
        {
            InitializeComponent();
        }

        protected override void OnNavigatedTo(System.Windows.Navigation.NavigationEventArgs e)
        {
 // Include initialization of ViewModel here rather than in constructor to be able to use QueryString value.
            if (viewModel == null)
            {
                viewModel = DraftItemStore.GetDraftItemById(NavigationContext.QueryString["ID"].ToString());
            }

            viewModel.Initialize();
            this.DataContext = viewModel;

            base.OnNavigatedTo(e);
            viewModel.ItemUpdated += new EventHandler<ItemUpdatedEventArgs>(OnItemUpdated);
        }

        protected override void OnNavigatedFrom(System.Windows.Navigation.NavigationEventArgs e)
        {
            base.OnNavigatedFrom(e);
            viewModel.ItemUpdated -= new EventHandler<ItemUpdatedEventArgs>(OnItemUpdated);
        }

        private void OnViewModelInitialization(object sender, InitializationCompletedEventArgs e)
        {
            this.Dispatcher.BeginInvoke(() =>
            {
                // If initialization has failed show error message and return.
                if (e.Error != null)
                {
                    MessageBox.Show(e.Error.Message, e.Error.GetType().Name, MessageBoxButton.OK);
                    return;
                }

                // Set Page's DataContext to current ViewModel instance.
                this.DataContext = (sender as EditItemViewModel);
            });
        }

        private void OnCancelButtonClick(object sender, EventArgs e)
        {
            NavigationService.Navigate(new Uri("/Views/List.xaml", UriKind.Relative));
        }

        private void OnSubmitButtonClick(object sender, EventArgs e)
        {
            viewModel.UpdateItem();
        }

        private void OnItemUpdated(object sender, ItemUpdatedEventArgs e)
        {
            this.Dispatcher.BeginInvoke(() =>
            {
                if (e.Error != null)
                {
                    MessageBox.Show(e.Error.Message, e.Error.GetType().Name, MessageBoxButton.OK);
                    return;
                }

                // Remove Draft Item from local storage if update to server succeeds.
                DraftItemStore.RemoveDraftItem(viewModel.ID.ToString());
                this.NavigationService.Navigate(new Uri("/Views/List.xaml", UriKind.Relative));
            });
        }

        private void OnBackButtonClick(object sender, EventArgs e)
        {
            NavigationService.Navigate(new Uri("/Views/List.xaml", UriKind.Relative));
        }
    }
}
  ```


Wie Sie sehen können, basieren auf den Namen des Projekts (SPListAppLocalStorage) der Namespace in dieser Datei verwendet.
    
  
9. Fügen Sie das Bild appbar.back.rest.png Sie dem Projekt für die **ApplicationBar** -Schaltfläche (BtnBack) in der Datei DraftItemEditForm.xaml deklariert. Wählen Sie im **Projektmappen-Explorer** den Knoten **Bilder** Ordner im Projekt.
    
  
10. Wählen Sie im Menü **Projekt** die Option **Vorhandenes Element hinzufügen**.
    
  
11. Navigieren Sie im Browser, der geöffnet wird zu dem Ordner, in dem die standardmäßigen Windows Phone Symbolbilder durch die Windows Phone SDK 7.1 installiert wurden.
    
    > **HINWEIS**
      > Die Bilder mit einem hellen Vorder- und ein dunkler Hintergrund sind in  `%PROGRAMFILES%(x86)\\Microsoft SDKs\\Windows Phone\\v7.1\\Icons\\dark` in einer Standardinstallation des SDK.
12. Wählen Sie die Bilddatei mit dem Namen appbar.back.rest.png, und wählen Sie **Hinzufügen**. Das Bild wird dem Projekt unter dem Knoten **Bilder** hinzugefügt.
    
  
13. Wählen Sie im **Projektmappen-Explorer** die Bilddatei, die Sie gerade hinzugefügt haben, und klicken Sie im **Fenster Eigenschaften** für die Datei legen die Eigenschaft **Buildvorgang** für die Bilddatei auf **Inhalte**, und legen Sie die Eigenschaft **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer**.
    
  
14. Speichern Sie die Dateien.
    
  

### Hinzufügen eine ApplicationBar-Schaltfläche zum Bearbeitungsformular für ein Element als Entwurf speichern


1. Wählen Sie im **Projektmappen-Explorer** die EditItemViewModel.cs-Datei unter dem Knoten **ViewModels** im Projekt. Drücken SieF7um die Datei zur Bearbeitung zu öffnen.
    
  
2. Fügen Sie in der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die **EditItemViewModel** -Klasse implementiert die folgende öffentliche Methode in der Datei.
    
  ```cs
  
public void SaveAsDraft()
{
    DraftItemStore.AddDraftItem(this.ID.ToString(), this);
}
  ```

3. Klicken Sie im **Projektmappen-Explorer** unter dem Knoten **Ansichten** des Projekts, doppelklicken Sie auf die EditForm.xaml.
    
    Die Datei wird zur Bearbeitung im Designer geöffnet.
    
  
4. Fügen Sie im Bereich XAML-Designer eine weitere Schaltfläche in den  `<shell:ApplicationBar>` Tag (zusätzlich zu den vorhandenen Schaltflächen **Absenden** und **Abbrechen** ), wie in den folgenden XAML-Code dargestellt.
    
  ```
  
<phone:PhoneApplicationPage.ApplicationBar>
    <shell:ApplicationBar IsVisible="True" IsMenuEnabled="True">
        <shell:ApplicationBarIconButton x:Name="btnSubmit" 
              IconUri="/Images/appbar.save.rest.png" 
              Text="Submit" Click="OnBtnSubmitClick"/>
        <shell:ApplicationBarIconButton x:Name="btnSaveDraft"            IconUri="/Images/appbar.save.rest.png" Text="Save Draft"            Click="OnSaveDraftButtonClick"/>
        <shell:ApplicationBarIconButton x:Name="btnCancel" 
                      IconUri="/Images/appbar.cancel.rest.png" 
                      Text="Cancel" Click="OnCancelButtonClick"/>
    </shell:ApplicationBar>
</phone:PhoneApplicationPage.ApplicationBar>
  ```

5. Die EditForm.xaml-Datei im **Projektmappen-Explorer** ausgewählt und drücken SieF7um die zugeordneten Code-Behind-Datei EditForm.xaml.cs, zur Bearbeitung zu öffnen.
    
  
6. Innerhalb der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die partiellen Klasse **EditForm** implementiert wird, fügen Sie in der Datei den folgenden Ereignishandler hinzu.
    
  ```cs
  
private void OnSaveDraftButtonClick(object sender, EventArgs e)
{
    viewModel.SaveAsDraft();
}
  ```

7. Speichern Sie die Dateien.
    
  

### Hinzufügen eine Schaltfläche ApplicationBar zum Ansichtsformular Liste zum Anzeigen aller Entwurfselemente


1. Doppelklicken Sie im **Projektmappen-Explorer** unter dem Knoten **Ansichten** auf die Datei List.xaml.
    
    Die Datei wird zur Bearbeitung im Designer geöffnet.
    
  
2. Im Bereich XAML-Designer Hinzufügen einer anderen Schaltfläche in den **<shell:ApplicationBar>** Tag (zusätzlich zu den vorhandenen Schaltflächen **neu** und **Aktualisieren** ), wie im folgenden XAML-Markup dargestellt.
    
  ```
  
<phone:PhoneApplicationPage.ApplicationBar>
    <shell:ApplicationBar IsVisible="True" IsMenuEnabled="True">
        <shell:ApplicationBarIconButton x:Name="btnNew" 
        IconUri="/Images/appbar.new.rest.png" Text="New" 
                    Click="OnNewButtonClick"/>
        <shell:ApplicationBarIconButton x:Name="btnRefresh" 
                   IconUri="/Images/appbar.refresh.rest.png" 
        Text="Refresh" IsEnabled="True" Click="OnRefreshButtonClick"/>
        <shell:ApplicationBarIconButton x:Name="btnDrafts"            IconUri="/Images/appbar.folder.rest.png"            Text="View Drafts" IsEnabled="True"            Click="OnDraftsButtonClick"/>
    </shell:ApplicationBar>
</phone:PhoneApplicationPage.ApplicationBar>
  ```

3. Fügen Sie ein Symbolbild Sie dem Projekt für die Schaltfläche " **Entwürfe** ". Wählen Sie im **Projektmappen-Explorer** den Knoten **Bilder** Ordner im Projekt.
    
  
4. Wählen Sie im Menü **Projekt** die Option **Vorhandenes Element hinzufügen**.
    
  
5. Navigieren Sie im Browser, der geöffnet wird zu dem Ordner, in dem die standardmäßigen Windows Phone Symbolbilder durch die Windows Phone SDK 7.1 installiert wurden.
    
    > **HINWEIS**
      > Die Bilder mit einem hellen Vorder- und ein dunkler Hintergrund sind in  `%PROGRAMFILES%(x86)\\Microsoft SDKs\\Windows Phone\\v7.1\\Icons\\dark` in einer Standardinstallation des SDK.
6. Wählen Sie die Bilddatei mit dem Namen appbar.folder.rest.png, und wählen Sie dann auf **Hinzufügen**.
    
    Das Bild wird dem Projekt unter dem Knoten **Bilder** hinzugefügt wird hinzugefügt.
    
  
7. Wählen Sie im **Projektmappen-Explorer** die Bilddatei, die Sie soeben hinzugefügt, und klicken Sie im **Fenster Eigenschaften** die Eigenschaft **Buildvorgang** für die Bilddatei auf **Inhalt** festgelegt und die Eigenschaft **In Ausgabeverzeichnis kopieren** auf **Kopieren, wenn neuer** festgelegt.
    
  
8. Wählen Sie im **Projektmappen-Explorer** die Datei List.xaml aus, unter dem Knoten **Ansichten**, und drücken SieF7. Die zugeordneten Code-Behind-Datei, List.xaml.cs, wird zur Bearbeitung geöffnet.
    
  
9. Fügen Sie den folgenden Ereignishandler an der Datei, in der Codeblock (abgegrenzt durch öffnende und schließende geschweifte Klammern), der die partiellen Klasse **ListForm** implementiert.
    
  ```cs
  
private void OnDraftsButtonClick(object sender, EventArgs e)
{
    NavigationService.Navigate(new Uri("/Views/Drafts.xaml", UriKind.Relative));
}
  ```

10. Speichern Sie alle Dateien in der Lösung, und drücken SieF6So kompilieren Sie die Lösung
    
  
Wenn Sie das Projekt starten und mit einem Windows Phone-Emulator bereitstellen, sehen Sie eine Schaltfläche **Zum Anzeigen von Entwürfen** auf die **ApplicationBar** des Listenformulars (Abbildung 1), die Sie alle Listenelemente als Entwürfe gespeichert werden.
  
    
    

**Abbildung 1. Modifiziertes Listenformular mit Schaltfläche zum Anzeigen von Entwürfen**

  
    
    

  
    
    
![Modifiziertes Listenformular mit Schaltfläche zum Anzeigen von Entwürfen](images/921bd9ab-d9a4-4cb6-b168-9ae53401d6d6.gif)
  
    
    
Zuerst, da keine Entwürfe gespeichert sind, wird die Seite zum Anzeigen von Entwürfen leer sein. Auswählen eines Elements aus dem Formular Liste (zum das Formular anzeigen (DisplayForm.xaml) für ein Element anzeigen), und wählen Sie dann auf die Schaltfläche **Bearbeiten**, um das Formular bearbeiten anzuzeigen. Wenn Sie die Konnektivität mit dem SharePoint Server verlieren sollten, können Sie dann die Schaltfläche **Entwurf speichern**, auf dem Formular bearbeiten (Abbildung 2), um alle Änderungen zu speichern, die Sie auf das Listenelement isolierten Speicher vorgenommen haben auswählen.
  
    
    

**Abbildung 2. Modifiziertes Bearbeitungsformular mit Schaltfläche ' Entwurf speichern '**

  
    
    

  
    
    
![Modifiziertes Bearbeitungsformular mit Schaltfläche "Entwurf speichern"](images/ce88b78d-5d41-406f-bf51-bee45ad2fe8f.gif)
  
    
    
Wenn der Server wieder verfügbar ist, können Sie die Schaltfläche **Zum Anzeigen von Entwürfen** Liste Formular zum Anzeigen der Seite Entwürfe (Abbildung 3).
  
    
    

**Abbildung 3. Entwurfsseite mit als Entwürfe isolierten Speicher gespeicherte Elemente**

  
    
    

  
    
    
![Entwurfsseite mit als Entwürfe gespeicherten Listenelementen](images/99ff6118-ed5d-4a21-9c11-3aabec8df1e5.gif)
  
    
    
Wenn Sie ein Element auf der Seite Entwürfe auswählen, wird das Formular Entwurf Element bearbeiten (DraftItemEditForm.xaml) befindet, angezeigten (Abbildung 4), und Sie können nehmen Sie keine zusätzlichen Änderungen, und klicken Sie dann auf die Schaltfläche **Absenden**, um das bearbeitete Element auf dem Server zu speichern. An dieser Stelle wird das Element aus dem isolierten Speicher entfernt, da es nicht mehr als Entwurf Element behandelt wird, nachdem es mit den Änderungen an den Server gespeichert ist.
  
    
    

**Abbildung 4. Bearbeitungsformular für Entwurfselemente**

  
    
    

  
    
    
![Das Entwurfselemente-Bearbeitungsformular](images/e3718687-682a-41a8-92b5-4eff9070348d.gif)
  
    
    
Beachten Sie die Ähnlichkeit zwischen dem Formular Entwurf Element bearbeiten (Abbildung 4) und dem standard Bearbeitungsformular (Abbildung 2) in dieser Anwendung. Der bearbeitungsmöglichkeiten für Elemente als Entwurfselemente sollte wie der bearbeitungsmöglichkeiten für Elemente im Kontext des dem Formular bearbeiten.
  
    
    

## Zusätzliche Ressourcen
<a name="SP15StoreSPlist_addlresources"> </a>


-  [Erstellen von Windows Phone-Apps, die auf SharePoint 2013 zugreifen](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [Lokalen Datenspeicher für Windows Phone](http://msdn.microsoft.com/library/fdf7e973-5de5-4cfa-bf63-1e65c90744cc%28Office.15%29.aspx)
    
  
-  [Vorgehensweise: beibehalten und Wiederherstellung Anwendungszustand für Windows Phone](http://msdn.microsoft.com/library/342e97c1-ff92-4cb2-81fa-e46f87c3cfc2%28Office.15%29.aspx)
    
  
-  [Windows Phone SDK 8.0](http://www.microsoft.com/en-us/download/details.aspx?id=35471)
    
  
-  [Microsoft SharePoint SDK für Windows Phone 8](http://www.microsoft.com/en-us/download/details.aspx?id=36818)
    
  
-  [Vorgehensweise: Einrichten einer Umgebung für die Entwicklung von mobilen Anwendungen für SharePoint](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/de-de/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)
    
  

