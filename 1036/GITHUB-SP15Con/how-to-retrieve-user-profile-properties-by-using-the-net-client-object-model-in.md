---
title: Procédure  Récupérer les propriétés de profil utilisateur à l'aide du modèle objet client .NET dans SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 236ebaf8-f92e-4192-9b51-0a9de0210885
---


# Procédure : Récupérer les propriétés de profil utilisateur à l'aide du modèle objet client .NET dans SharePoint 2013
Découvrez comment récupérer des propriétés de profil utilisateur par programme à l'aide du modèle objet client .NET SharePoint 2013.
## À quoi correspondent les propriétés de profil utilisateur dans SharePoint 2013 ?
<a name="bkmk_WhatIs"> </a>

Les propriétés utilisateur et les propriétés de profil utilisateur fournissent des informations sur les utilisateurs SharePoint, telles que le nom d'affichage, l'adresse de messagerie, la fonction et d'autres informations professionnelles et personnelles. Dans les API côté client, vous pouvez accéder à ces propriétés à partir de l'objet  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) et de sa propriété [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) . La propriété [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) contient toutes les propriétés de profil utilisateur, mais l'objet [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) contient les propriétés fréquemment utilisées (telles que [AccountName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.AccountName.aspx) , [DisplayName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.DisplayName.aspx) et [Email](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.Email.aspx) ) plus faciles d'accès.
  
    
    
L'objet  [PeopleManager](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.aspx) comprend les méthodes suivantes, que vous pouvez utiliser pour récupérer les propriétés utilisateur et les propriétés de profil utilisateur à l'aide du modèle objet client .NET :
  
    
    

- Les méthodes  [GetMyProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetMyProperties.aspx) et [GetPropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetPropertiesFor.aspx) renvoient un objet [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) .
    
  
- Les méthodes  [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) et [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) renvoient les valeurs des propriétés de profil utilisateur que vous spécifiez.
    
  
Les propriétés de profil utilisateur provenant d'API client sont en lecture seule (à l'exception de l'image de profil, que vous pouvez modifier à l'aide de la méthode  [PeopleManager.SetMyProfilePicture](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.SetMyProfilePicture.aspx) ). Si vous souhaitez modifier d'autres propriétés de profil utilisateur, vous devez utiliser le modèle objet serveur.
  
    
    

> **REMARQUE**
> La version client de l'objet  [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) ne contient pas toutes les propriétés utilisateur de la version côté serveur. Cependant, la version côté client fournit les méthodes permettant de créer un site personnel pour l'utilisateur actuel. Pour récupérer l'objet [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) côté client pour l'utilisateur actuel, utilisez la méthode [ProfileLoader.GetUserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.ProfileLoader.GetUserProfile.aspx) .
  
    
    

Pour plus d'informations sur l'utilisation de profils, voir  [Utiliser des profils utilisateur dans SharePoint 2013](work-with-user-profiles-in-sharepoint-2013.md).
  
    
    

## Conditions préalables à la configuration de votre environnement de développement pour récupérer les propriétés de profil utilisateur à l'aide du modèle objet client .NET SharePoint 2013
<a name="bk_Prereqs"> </a>

Pour créer une application console qui utilise le modèle objet client .NET afin de récupérer les propriétés de profil utilisateur, vous aurez besoin des éléments suivants :
  
    
    

- SharePoint Server 2013 avec des profils créés pour l'utilisateur actuel et un utilisateur cible
    
  
- Visual Studio 2012
    
  
- Autorisations de connexion en mode **Contrôle total** pour accéder à l'application de service de profil utilisateur pour l'utilisateur actuel
    
  

> **REMARQUE**
> Si vous ne procédez pas au développement sur l'ordinateur qui exécute SharePoint Server 2013, obtenez le téléchargement  [Composants clients SharePoint](http://www.microsoft.com/en-us/download/details.aspx?id=35585) qui contient les assemblies clients SharePoint 2013.
  
    
    


## Créez l'application console qui récupère les propriétés de profil utilisateur à l'aide du modèle objet client .NET SharePoint 2013.
<a name="bk_RetrieveProps"> </a>


1. Sur l'ordinateur de développement, ouvrez Visual Studio et sélectionnez **Fichier**, **Nouveau**, puis **Projet**.
    
  
2. Dans la boîte de dialogue **Nouveau projet**, sélectionnez **.NET Framework 4.5** dans la liste déroulante située en haut.
    
  
3. Dans les modèles de projet, sélectionnez **Windows**, puis **Application console**.
    
  
4. Nommez le projet UserProfilesCSOM, puis cliquez sur le bouton **OK**.
    
  
5. Ajoutez des références aux assemblies suivants :
    
  - **Microsoft.SharePoint.Client**
    
  
  - **Microsoft.SharePoint.ClientRuntime**
    
  
  - **Microsoft.SharePoint.Client.UserProfiles**
    
  
6. Dans la méthode **Main**, définissez les variables pour l'URL du serveur et le nom d'utilisateur cible, comme indiqué dans le code suivant.
    
  ```cs
  
const string serverUrl = "http://serverName/";
const string targetUser = "domainName\\\\userName";
  ```


> **REMARQUE**
> N'oubliez pas de remplacer les valeurs des espaces réservés  `http://serverName/` et `domainName\\\\userName` avant d'exécuter le code.
7. Initialisez le contexte client SharePoint, comme indiqué dans le code suivant.
    
  ```cs
  
ClientContext clientContext = new ClientContext(serverUrl);

  ```

8. Obtenez les propriétés de l'utilisateur cible à partir de l'objet **PeopleManager**, comme indiqué dans le code suivant.
    
  ```cs
  
PeopleManager peopleManager = new PeopleManager(clientContext);
PersonProperties personProperties = peopleManager.GetPropertiesFor(targetUser);
  ```


    L'objet **personProperties** est un objet client. Certains objets clients ne contiennent aucune donnée avant leur initialisation. Par exemple, vous ne pouvez pas accéder aux valeurs de propriété de l'objet **personProperties** avant de l'initialiser. Si vous tentez d'accéder à une propriété avant qu'elle ne soit initialisée, vous recevez une exception **PropertyOrFieldNotInitializedException**.
    
  
9. Pour initialiser l'objet **personProperties**, enregistrez la requête que vous souhaitez exécuter, puis exécutez-la sur le serveur, comme indiqué dans le code suivant.
    
  ```cs
  
clientContext.Load(personProperties, p => p.AccountName, p => p.UserProfileProperties);
clientContext.ExecuteQuery();
  ```


Lorsque vous appelez la méthode **Load** (ou la méthode **LoadQuery**), vous transmettez l'objet que vous voulez récupérer ou modifier. Dans cet exemple, l'appel de la méthode **Load** transmet des paramètres facultatifs pour filtrer la demande. Les paramètres sont des expressions lambda qui demandent uniquement les propriétés **AccountName** et **UserProfileProperties** de l'objet **personProperties**.
    
> **CONSEIL**
> Pour réduire le trafic réseau, demandez uniquement les propriétés que vous souhaitez utiliser lorsque vous appelez la méthode **Load**. En outre, si vous utilisez plusieurs objets, si possible, regroupez plusieurs appels de la méthode **Load** avant d'appeler la méthode **ExecuteQuery**. 
10. Itérez les propriétés de profil utilisateur et lisez le nom et la valeur de chaque propriété, comme indiqué dans le code suivant.
    
  ```cs
  
foreach (var property in personProperties.UserProfileProperties)
{
    Console.WriteLine(string.Format("{0}: {1}", 
        property.Key.ToString(), property.Value.ToString()));
}

  ```


## Exemple de code : récupération de toutes les propriétés de profil utilisateur à l'aide du modèle objet client .NET SharePoint 2013
<a name="SP15UserProf_codeexample1"> </a>

L'exemple de code suivant montre comment récupérer et itérer toutes les propriétés de profil utilisateur d'un utilisateur cible, comme décrit dans la  [procédure précédente](how-to-retrieve-user-profile-properties-by-using-the-net-client-object-model-in.md#bk_RetrieveProps).
  
    
    

> **REMARQUE**
> Remplacez les valeurs des espaces réservés  `http://serverName/` et `domainName\\\\userName` avant d'exécuter le code.
  
    
    


```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.UserProfiles;

namespace UserProfilesCSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace the following placeholder values with the target SharePoint site and
            // target user.
            const string serverUrl = "http://serverName/";  
            const string targetUser = "domainName\\\\userName";  

            // Connect to the client context.
            ClientContext clientContext = new ClientContext(serverUrl);

            // Get the PeopleManager object and then get the target user's properties.
            PeopleManager peopleManager = new PeopleManager(clientContext);
            PersonProperties personProperties = peopleManager.GetPropertiesFor(targetUser);

            // Load the request and run it on the server.
            // This example requests only the AccountName and UserProfileProperties
            // properties of the personProperties object.
            clientContext.Load(personProperties, p => p.AccountName, p => p.UserProfileProperties);
            clientContext.ExecuteQuery();

            foreach (var property in personProperties.UserProfileProperties)
            {
                Console.WriteLine(string.Format("{0}: {1}", 
                    property.Key.ToString(), property.Value.ToString()));
            }
            Console.ReadKey(false);

            // TODO: Add error handling and input validation.
        }
    }
}
```


## Exemple de code : récupération des propriétés de profil utilisateur des personnes qui me suivent à l'aide du modèle objet client .NET SharePoint 2013
<a name="SP15UserProfilescodeInApp"> </a>

L'exemple de code suivant montre comment obtenir, dans une Complément SharePoint, les propriétés de profil utilisateur des personnes qui vous suivent.
  
    
    

```cs

string contextTokenString = TokenHelper.GetContextTokenFromRequest(Request);

if (contextTokenString != null)
{
    Uri sharepointUrl = new Uri(Request.QueryString["SP.Url"]);

    ClientContext clientContext = TokenHelper.GetClientContextWithContextToken(sharepointUrl.ToString(), contextTokenString, Request.Url.Authority);

    PeopleManager peopleManager = new PeopleManager(clientContext);
    ClientObjectList<PersonProperties> peopleFollowedBy = peopleManager.GetMyFollowers();
    clientContext.Load(peopleFollowedBy, people => people.Include(person => person.PictureUrl, person => person.DisplayName));
    clientContext.ExecuteQuery();

    foreach (PersonProperties personFollowedBy in peopleFollowedBy)
    {
        if (!string.IsNullOrEmpty(personFollowedBy.PictureUrl))
        {
        Response.Write("<img src=\\"" + personFollowedBy.PictureUrl + "\\" alt=\\"" + personFollowedBy.DisplayName + "\\"/>");
        }
    }
    clientContext.Dispose();
}
```


## Exemple de code : récupération d'un jeu de propriétés de profil utilisateur à l'aide du modèle objet client .NET SharePoint 2013
<a name="SP15UserProfilescode2"> </a>

L'exemple de code suivant montre comment récupérer un jeu spécifique de propriétés de profil utilisateur pour un utilisateur cible.
  
    
    

> **REMARQUE**
> Pour récupérer la valeur d'une seule propriété de profil utilisateur, utilisez la méthode  [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) .
  
    
    

Contrairement à l'exemple de code précédent, qui récupère un objet  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) pour l'utilisateur cible, cet exemple appelle la méthode [PeopleManager.GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) et transmet un objet [UserProfilePropertiesForUser](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfilePropertiesForUser.aspx) qui spécifie l'utilisateur cible et les propriétés de profil utilisateur à récupérer. [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) renvoie une collection **IEnumerable<string>** qui contient les valeurs des propriétés que vous spécifiez.
  
    
    

> **REMARQUE**
> Remplacez les valeurs des espaces réservés  `http://serverName/` et `domainName\\\\userName` avant d'exécuter le code.
  
    
    




```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.UserProfiles;

namespace UserProfilesCSOM
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace the following placeholder values with the target SharePoint site and the
            // target user.
            const string serverUrl = "http://serverName/";  
            const string targetUser = "domainName\\\\userName";  

            // Connect to the client context.
            ClientContext clientContext = new ClientContext(serverUrl);

            // Get the PeopleManager object.
            PeopleManager peopleManager = new PeopleManager(clientContext);

            // Retrieve specific properties by using the GetUserProfilePropertiesFor method. 
            // The returned collection contains only property values.
            string[] profilePropertyNames = new string[] { "PreferredName", "Department", "Title" };
            UserProfilePropertiesForUser profilePropertiesForUser = new UserProfilePropertiesForUser(
                clientContext, targetUser, profilePropertyNames);
            IEnumerable<string> profilePropertyValues = peopleManager.GetUserProfilePropertiesFor(profilePropertiesForUser);

            // Load the request and run it on the server.
            clientContext.Load(profilePropertiesForUser);
            clientContext.ExecuteQuery();

            // Iterate through the property values.
            foreach (var value in profilePropertyValues)
            {
                Console.Write(value + "\\n");
            }
            Console.ReadKey(false);

            // TO DO: Add error handling and input validation.
        }
    }
}

```


## Ressources supplémentaires
<a name="bk_addresources"> </a>


-  [Utiliser des profils utilisateur dans SharePoint 2013](work-with-user-profiles-in-sharepoint-2013.md)
    
  
-  [Procédure : Récupérer les propriétés de profil utilisateur à l'aide du modèle objet JavaScript dans SharePoint 2013](how-to-retrieve-user-profile-properties-by-using-the-javascript-object-model-in.md)
    
  
-  [Comment travailler avec les profils utilisateur et les profils d'organisation en utilisant le modèle objet serveur dans SharePoint 2013](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md)
    
  
-  [Microsoft.SharePoint.Client.UserProfiles](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.aspx)
    
  

