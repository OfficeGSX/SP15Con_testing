---
title: Vorgehensweise Arbeiten mit Benutzerprofilen und Organisationsprofilen mithilfe des Serverobjektmodells in SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 13f16dc3-f652-4fb3-996b-5f2166236d2b
---


# Vorgehensweise: Arbeiten mit Benutzerprofilen und Organisationsprofilen mithilfe des Serverobjektmodells in SharePoint 2013
In diesem Artikel erfahren Sie, wie Sie Benutzerprofile und Benutzerprofileigenschaften in SharePoint 2013 programmgesteuert mithilfe des SharePoint 2013-Serverobjektmodells erstellen, abrufen und ändern können.
## Was sind Benutzerprofile in SharePoint 2013?

Benutzerprofile stellen in SharePoint 2013 SharePoint-Benutzer dar. Benutzerprofileigenschaften stellen Informationen über die Benutzer und über die Eigenschaften selbst dar. Eigenschaften umfassen beispielsweise den Kontonamen oder die E-Mail-Adresse eines Benutzers und den Datentyp einer Eigenschaft. Sie können das Serverobjektmodell verwenden, um Benutzerprofile, Profiluntertypen und Profileigenschaften zu erstellen, abzurufen und zu ändern.
  
    
    

> **HINWEIS**
> Weitere Informationen zu allgemeinen Programmierungsaufgaben für das Arbeiten mit Benutzerprofilen und der API, die Sie zum Ausführen der Aufgaben verwenden, finden Sie unter  [Arbeiten mit Benutzerprofilen in SharePoint 2013](work-with-user-profiles-in-sharepoint-2013.md). 
  
    
    


## Voraussetzungen für das Einrichten der Entwicklungsumgebung für das Arbeiten mit Benutzerprofilen mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_Prereqs"> </a>

Um eine Konsolenanwendung zu erstellen, die das Serverobjektmodell für das Arbeiten mit Benutzerprofilen und Benutzerprofileigenschaften verwendet, benötigen Sie Folgendes:
  
    
    

- SharePoint Server 2013 mit dem für den aktuellen Benutzer erstellen Profil
    
  
- Visual Studio 2012.
    
  
- Berechtigungen zum Erstellen, Abrufen und Ändern der Benutzerprofilobjekte. (Für das Erstellen und Ändern der Profile ist die Berechtigung **Benutzerprofile ändern** erforderlich.)
    
  

## Erstellen einer mit Benutzerprofilen funktionierenden Konsolenanwendung mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_CreateConsoleApp"> </a>


1. Öffnen Sie Visual Studio, und wählen Sie **Datei**, **Neu**, **Projekt**.
    
  
2. Wählen Sie im Dialogfeld **Neues Projekt** aus der Dropdownliste oben im Dialogfeld die Option **.NET Framework 4.5**.
    
  
3. Wählen Sie in der Liste **Vorlagen** **Windows** und dann **Konsolenanwendung**.
    
  
4. Geben Sie für das Projekt den Namen UserProfilesSSOM an, und klicken Sie dann auf **OK**.
    
    > **HINWEIS**
      > Vergewissern Sie sich, dass die Einstellung **32-Bit bevorzugen** in den **Build**-Eigenschaften des Projekts nicht ausgewählt ist. 
5. Fügen Sie Verweise auf die folgenden Assemblys hinzu:
    
  - Microsoft.Office.Server
    
  
  - Microsoft.Office.Server.UserProfiles
    
  
  - Microsoft.SharePoint
    
  
  - System.Web
    
  
6. Ersetzen Sie die Inhalte der **Program**-Klasse durch das Codebeispiel aus einem der folgenden Szenarien:
    
  -  [Erstellen eines Benutzerprofils](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md#bkmk_CreateUP)
    
  
  -  [Erstellen einer Benutzerprofileigenschaft](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md#bkmk_CreateUPProp)
    
  
  -  [Abrufen und Ändern von Benutzerprofilen](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md#bkmk_GetChangeUP)
    
  
  -  [Abrufen und Ändern der Attribute für Benutzerprofile](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md#bkmk_GetChangePropAttributes)
    
  
  -  [Abrufen und Ändern der Werte für Benutzereigenschaften](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md#bkmk_GetChangePropValues)
    
  
7. Wählen Sie zum Testen der Konsolenanwendung auf der Menüleiste **Debuggen**, **Debuggen starten** aus. Sie können die Änderungen auf der Seite **Profildienst verwalten** für die Benutzerprofildienst-Anwendung in der SharePoint-Zentraladministration überprüfen.
    
  

## Codebeispiel: Erstellen von Benutzerprofilen mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_CreateUP"> </a>

Benutzerprofile stellen in SharePoint 2013 SharePoint-Benutzer dar. Profiltypen und Untertypen helfen bei der Kategorisierung von Profilen in Gruppen wie Mitarbeiter oder Kunden. Profiltypen und Untertypen werden zum Festlegen von allgemeinen Profileigenschaften und Attributen auf Untertypebene verwendet. SharePoint Server enthält einen standardmäßigen Benutzerprofil-Untertyp.
  
    
    
Das folgende Codebeispiel erstellt ein  [UserProfile](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.UserProfile.aspx) -Objekt, das dem standardmäßigen Benutzerprofil-Untertyp zugeordnet ist. Einige Benutzerprofileigenschaften werden automatisch mit Informationen gefüllt, die aus dem Verzeichnis mit den Benutzerkonten importiert werden, z. B. Active Directory-Domänendienste. Ein Codebeispiel, mit dem ein benutzerdefinierter Untertyp erstellt wird, finden Sie unter [ProfileSubtype](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileSubtype.aspx) .
  
    
    

> **HINWEIS**
> Ändern Sie die Platzhalterwerte Domäne\\\\Benutzername undServername, bevor Sie den Code ausführen. 
  
    
    




```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.Office.Server;
using Microsoft.Office.Server.UserProfiles;
using Microsoft.SharePoint;
using System.Web;
namespace UserProfilesSSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace "domain\\\\username" and "servername" with actual values.
            string newAccountName = "domain\\\\username";
            using (SPSite site = new SPSite("http://servername"))
            {
                SPServiceContext serviceContext = SPServiceContext.GetContext(site);
                try
                {

                    // Create a user profile that uses the default user profile
                    // subtype.
                    UserProfileManager userProfileMgr = new UserProfileManager(serviceContext);
                    UserProfile userProfile = userProfileMgr.CreateUserProfile(newAccountName);

                    Console.WriteLine("A profile was created for " + userProfile.DisplayName);
                    Console.Read();
                }

                catch (System.Exception e)
                {
                    Console.WriteLine(e.GetType().ToString() + ": " + e.Message);
                    Console.Read();
                }
            }
        }
    }
}
```


## Codebeispiel: Erstellen von Benutzerprofileigenschaften mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_CreateUPProp"> </a>

Benutzerprofileigenschaften beschreiben persönliche und Organisationsinformationen über Benutzer. Sie können eine benutzerdefinierte Profileigenschaft erstellen oder sie zu dem Standardsatz der SharePoint-Profileigenschaften hinzufügen.
  
    
    
Eine Profileigenschaft und die dazugehörigen Attribute werden von einem Satz verknüpfter Objekte dargestellt: einem  [CoreProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.CoreProperty.aspx) -Objekt, einem [ProfileTypeProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileTypeProperty.aspx) -Objekt und einem [ProfileSubtypeProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileSubtypeProperty.aspx) -Objekt.
  
    
    
Das folgende Codebeispiel erstellt einen  [CoreProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.CoreProperty.aspx) mit einem URL-Datentyp (oder optional einen [CoreProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.CoreProperty.aspx) mit einem Zeichenfolgentyp mit mehreren Werten). Es erstellt auch einen [ProfileTypeProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileTypeProperty.aspx) und einen [ProfileTypeProperty](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileTypeProperty.aspx) , die Verfügbarkeit, Datenschutz und weitere Einstellungen für die Eigenschaft definieren. Die [ProfileSubtypeProperty.DefaultPrivacy](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileSubtypeProperty.DefaultPrivacy.aspx) -Eigenschaft steuert die Sichtbarkeit der Eigenschaften und weiteren „Meine Website"-Inhalten. Eine vollständige Liste möglicher Datentypen für Profileigenschaftenwerte finden Sie unter [PropertyDataType](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.PropertyDataType.aspx) .
  
    
    

> **HINWEIS**
> Ändern Sie den Platzhalterwert Servername, bevor Sie den Code ausführen. 
  
    
    




```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.Office.Server;
using Microsoft.Office.Server.Administration;
using Microsoft.Office.Server.UserProfiles;
using Microsoft.SharePoint;
using System.Web;
namespace UserProfilesSSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace "servername" with an actual value.
            using (SPSite site = new SPSite("http://servername"))
            {
                SPServiceContext serviceContext = SPServiceContext.GetContext(site);
                try
                {
                    ProfilePropertyManager profilePropMgr = new UserProfileConfigManager(serviceContext).ProfilePropertyManager;
                    CorePropertyManager corePropMgr = profilePropMgr.GetCoreProperties();

                    // Create a URL property.
                    CoreProperty coreProp = corePropMgr.Create(false);
                    coreProp.Name = "AppsWebsite";
                    coreProp.DisplayName = "Apps site";
                    coreProp.Type = PropertyDataType.URL;
                    coreProp.Length = 100;
                    corePropMgr.Add(coreProp);

                    //// Create a multivalue property.
                    //// To create this property, comment out the previous
                    //// block of code and uncomment this block of code.
                    //CoreProperty coreProp = corePropMgr.Create(false);
                    //coreProp.Name = "PublishedAppsList";
                    //coreProp.DisplayName = "Published apps";
                    //coreProp.Type = PropertyDataType.StringMultiValue;
                    //coreProp.IsMultivalued = true;
                    //coreProp.Length = 100;
                    //corePropMgr.Add(coreProp);

                    // Create a profile type property and make the core property 
                    // visible in the Details section page.
                    ProfileTypePropertyManager typePropMgr = profilePropMgr.GetProfileTypeProperties(ProfileType.User);
                    ProfileTypeProperty typeProp = typePropMgr.Create(coreProp);
                    typeProp.IsVisibleOnViewer = true;
                    typePropMgr.Add(typeProp);

                    // Create a profile subtype property.
                    ProfileSubtypeManager subtypeMgr = ProfileSubtypeManager.Get(serviceContext);
                    ProfileSubtype subtype = subtypeMgr.GetProfileSubtype(ProfileSubtypeManager.GetDefaultProfileName(ProfileType.User));
                    ProfileSubtypePropertyManager subtypePropMgr = profilePropMgr.GetProfileSubtypeProperties(subtype.Name);
                    ProfileSubtypeProperty subtypeProp = subtypePropMgr.Create(typeProp);
                    subtypeProp.IsUserEditable = true;
                    subtypeProp.DefaultPrivacy = Privacy.Public;
                    subtypeProp.UserOverridePrivacy = true;
                    subtypePropMgr.Add(subtypeProp);

                    Console.WriteLine("The properties were created.");
                    Console.Read();
                }

                catch (System.Exception e)
                {
                    Console.WriteLine(e.GetType().ToString() + ": " + e.Message);
                    Console.Read();
                }
            }
        }
    }
}
```


## Codebeispiel: Abrufen und Ändern von Benutzerprofilen mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_GetChangeUP"> </a>

Das folgende Codebeispiel ruft alle Benutzerprofile innerhalb des Kontexts ab und ändert den Wert einer  [DisplayName](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.UserProfile.DisplayName.aspx) -Benutzereigenschaft. Auf die meisten Profileigenschaften wird mit dem [UserProfile.Item](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.UserProfile.Item.aspx) -Accessor zugegriffen.
  
    
    

> **HINWEIS**
> Ändern Sie die Platzhalterwerte Domäne\\\\Benutzername undServername, bevor Sie den Code ausführen. 
  
    
    


```cs

using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.Office.Server;
using Microsoft.Office.Server.UserProfiles;
using Microsoft.SharePoint;
using System.Web;
namespace UserProfilesSSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace "domain\\\\username" and "servername" with actual values.
            string targetAccountName = "domain\\\\username";
            using (SPSite site = new SPSite("http://servername"))
            {
                SPServiceContext serviceContext = SPServiceContext.GetContext(site);
                try
                {

                    // Retrieve and iterate through all of the user profiles in this context.
                    Console.WriteLine("Retrieving user profiles:");
                    UserProfileManager userProfileMgr = new UserProfileManager(serviceContext);
                    IEnumerator userProfiles = userProfileMgr.GetEnumerator();
                    while (userProfiles.MoveNext())
                    {
                        UserProfile userProfile = (UserProfile)userProfiles.Current;
                        Console.WriteLine(userProfile.AccountName);
                    }

                    // Retrieve a specific user profile. Change the value of a user profile property
                    // and save (commit) the change on the server.
                    UserProfile user = userProfileMgr.GetUserProfile(targetAccountName);
                    Console.WriteLine("\\nRetrieving user profile for " + user.DisplayName + ".");
                    user.DisplayName = "Pat";
                    user.Commit();
                    Console.WriteLine("\\nThe user\\'s display name has been changed.");
                    Console.Read();
                }

                catch (System.Exception e)
                {
                    Console.WriteLine(e.GetType().ToString() + ": " + e.Message);
                    Console.Read();
                }
            }
        }
    }
}
```


## Codebeispiel: Abrufen und Ändern der Attribute für Benutzerprofileigenschaften mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_GetChangePropAttributes"> </a>

Das folgende Codebeispiel ruft den Eigenschaftensatz ab, der eine bestimmte Benutzereigenschaft und die dazugehörigen Attribute darstellt, und ändert dann die  [CoreProperty.DisplayName](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.CoreProperty.DisplayName.aspx) -, [ProfileTypeProperty.IsVisibleOnViewer](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileTypeProperty.IsVisibleOnViewer.aspx) - und [ProfileSubtypeProperty.PrivacyPolicy](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileSubtypeProperty.PrivacyPolicy.aspx) -Attribute. Diese Änderungen gelten global für den Eigenschaftensatz. [ProfileSubtypeProperty.PrivacyPolicy](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileSubtypeProperty.PrivacyPolicy.aspx) gibt an, ob Benutzer einen Wert für die Eigenschaft angeben müssen. [PrivacyPolicy](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.ProfileSubtypeProperty.PrivacyPolicy.aspx) gilt nur für Benutzerprofileigenschaften.
  
    
    

> **HINWEIS**
> Ändern Sie den Platzhalterwert Servername, bevor Sie den Code ausführen. 
  
    
    


```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.Office.Server;
using Microsoft.Office.Server.UserProfiles;
using Microsoft.SharePoint;
using System.Web;
namespace UserProfilesSSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace "servername" with an actual value.
            using (SPSite site = new SPSite("http://servername"))
            {
                SPServiceContext serviceContext = SPServiceContext.GetContext(site);
                try
                {
                    ProfilePropertyManager profilePropMgr = new UserProfileConfigManager(serviceContext).ProfilePropertyManager;
                    ProfileSubtypePropertyManager subtypePropMgr = profilePropMgr.GetProfileSubtypeProperties("UserProfile");

                    // Retrieve a specific property set (a profile subtype property and
                    // its associated core and profile type properties).
                    // Changing these properties affects all instances of this property set.
                    ProfileSubtypeProperty subtypeProp = subtypePropMgr.GetPropertyByName(PropertyConstants.Title);
                    CoreProperty coreProp = subtypeProp.CoreProperty;
                    ProfileTypeProperty typeProp = subtypeProp.TypeProperty;

                    Console.WriteLine("Property name: " + coreProp.DisplayName);
                    Console.WriteLine("IsVisibleOnViewer = " + typeProp.IsVisibleOnViewer);
                    Console.WriteLine("PrivacyPolicy = " + subtypeProp.PrivacyPolicy);
                    Console.WriteLine("Press Enter to change the values.");
                    Console.Read();

                    // Change attributes on the properties and save (commit) the changes
                    // on the server.
                    coreProp.DisplayName = "Position";
                    coreProp.Commit();
                    typeProp.IsVisibleOnViewer = true;
                    typeProp.Commit();
                    subtypeProp.PrivacyPolicy = PrivacyPolicy.OptOut;
                    subtypeProp.Commit();
                    Console.WriteLine("The property attributes have been changed.");
                    Console.Read();
                }

                catch (System.Exception e)
                {
                    Console.WriteLine(e.GetType().ToString() + ": " + e.Message);
                    Console.Read();
                }
            }
        }
    }
}
```


## Codebeispiel: Abrufen und Ändern der Werte für Benutzereigenschaften mithilfe des SharePoint 2013-Serverobjektmodells
<a name="bkmk_GetChangePropValues"> </a>

Das folgende Beispiel ruft alle **UserProfile**-Typeigenschaften und Eigenschaftswerte für einen bestimmten Benutzer ab. Anschließend ändert es die  [PictureUrl](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.PropertyConstants.PictureUrl.aspx) -Eigenschaft mit einem Wert und die [PastProjects](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.PropertyConstants.PastProjects.aspx) -Eigenschaft mit mehreren Werten. Eine vollständige Liste der Profileigenschaften-Namenkonstanten finden Sie unter [PropertyConstants](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.PropertyConstants.aspx) .
  
    
    

> **HINWEIS**
> Ändern Sie die Platzhalterwerte Domäne\\\\Benutzername, http://Servername/docLib/pic.jpg undServername, bevor der Code ausgeführt wird. 
  
    
    


```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.Office.Server;
using Microsoft.Office.Server.UserProfiles;
using Microsoft.SharePoint;
using System.Web;
namespace UserProfilesSSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace "domain\\\\username," "http://servername/docLib/pic.jpg," and "servername" with actual values.
            string accountName = "domain\\\\username";
            string newPictureUrl = "http://servername/docLib/pic.jpg";
            using (SPSite site = new SPSite("http://servername"))
            {
                SPServiceContext serviceContext = SPServiceContext.GetContext(site);

                try
                {
                    UserProfileManager userProfileMgr = new UserProfileManager(serviceContext);
                    ProfilePropertyManager profilePropMgr = new UserProfileConfigManager(serviceContext).ProfilePropertyManager;
                   
                    // Retrieve all properties for the "UserProfile" profile subtype,
                    // and retrieve the property values for a specific user.
                    ProfileSubtypePropertyManager subtypePropMgr = profilePropMgr.GetProfileSubtypeProperties("UserProfile");
                    UserProfile userProfile = userProfileMgr.GetUserProfile(accountName);
                    IEnumerator<ProfileSubtypeProperty> userProfileSubtypeProperties = subtypePropMgr.GetEnumerator();
                    while (userProfileSubtypeProperties.MoveNext())
                    {
                        string propName = userProfileSubtypeProperties.Current.Name;
                        ProfileValueCollectionBase values = userProfile.GetProfileValueCollection(propName);
                        if (values.Count > 0)
                        {

                            // Handle multivalue properties.
                            foreach (var value in values)
                            {
                                Console.WriteLine(propName + ": " + value.ToString());
                            }
                        }
                        else
                        {
                            Console.WriteLine(propName + ": ");
                        }
                    }
                    Console.WriteLine("Press Enter to change the values.");
                    Console.Read();

                    // Change the value of a single-value user property.
                    userProfile[PropertyConstants.PictureUrl].Value = newPictureUrl;

                    // Add a value to a multivalue user property.
                    userProfile[PropertyConstants.PastProjects].Add((object)"Team Feed App");
                    userProfile[PropertyConstants.PastProjects].Add((object)"Social Ratings View Web Part");

                    // Save the changes to the server.
                    userProfile.Commit();
                    Console.WriteLine("The property values for the user have been changed.");
                    Console.Read();
                }

                catch (System.Exception e)
                {
                    Console.WriteLine(e.GetType().ToString() + ": " + e.Message);
                    Console.Read();
                }
            }
        }
    }
}
```


## Weitere Ressourcen
<a name="bk_addresources"> </a>


-  [Arbeiten mit Benutzerprofilen in SharePoint 2013](work-with-user-profiles-in-sharepoint-2013.md)
    
  
-  [Vorgehensweise: Abrufen von Benutzerprofileigenschaften mithilfe des .NET-Clientobjektmodells in SharePoint 2013](how-to-retrieve-user-profile-properties-by-using-the-net-client-object-model-in.md)
    
  
-  [Vorgehensweise: Abrufen von Benutzerprofileigenschaften mithilfe des JavaScript-Objektmodells in SharePoint 2013](how-to-retrieve-user-profile-properties-by-using-the-javascript-object-model-in.md)
    
  
-  [Microsoft.Office.Server.UserProfiles](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.aspx)
    
  

