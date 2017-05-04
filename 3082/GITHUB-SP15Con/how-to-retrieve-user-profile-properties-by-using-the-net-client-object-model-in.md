---
title: Recuperar propiedades de perfil de usuario usando el modelo de objetos de cliente .NET en SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 236ebaf8-f92e-4192-9b51-0a9de0210885
---


# Recuperar propiedades de perfil de usuario usando el modelo de objetos de cliente .NET en SharePoint 2013
Aprenda a recuperar propiedades de perfil de usuario mediante programación usando el modelo de objetos de cliente .NET de SharePoint 2013.
## ¿Qué son las propiedades de perfil de usuario en SharePoint 2013?
<a name="bkmk_WhatIs"> </a>

Las propiedades de usuario y las propiedades de perfil de usuario ofrecen información sobre los usuarios de SharePoint, como el nombre para mostrar, el correo electrónico, el puesto y otros datos personales y de negocios. En las API del lado cliente, se obtiene acceso a estas propiedades desde el objeto  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) y su propiedad [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) . La propiedad [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) contiene todas las propiedades de perfil de usuario, pero el objeto [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) contiene propiedades usadas habitualmente (como [AccountName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.AccountName.aspx) , [DisplayName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.DisplayName.aspx) y [Email](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.Email.aspx) ) que son de más fácil acceso.
  
    
    
El objeto  [PeopleManager](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.aspx) incluye los siguientes métodos que se pueden usar para recuperar propiedades de usuario y propiedades de perfil de usuario usando el modelo de objetos .NET:
  
    
    

- El método  [GetMyProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetMyProperties.aspx) y el método [GetPropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetPropertiesFor.aspx) devuelven un objeto [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) .
    
  
- El método  [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) y el método [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) devuelven los valores de las propiedades de perfil de usuario que especifique.
    
  
Las propiedades de perfil de usuario de las API de cliente son de solo lectura (excepto la imagen de perfil, que se puede cambiar usando el método  [PeopleManager.SetMyProfilePicture](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.SetMyProfilePicture.aspx) ). Si quiere cambiar otras propiedades de perfil de usuario, tiene que usar el modelo de objetos de servidor.
  
    
    

> **NOTA**
> La versión de cliente del objeto  [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) no contiene todas las propiedades de usuario que tiene la versión de servidor. No obstante, la versión del lado cliente ofrece los métodos para crear un sitio personal para el usuario actual. Si quiere recuperar el [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) del lado cliente para el usuario actual, use el método [ProfileLoader.GetUserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.ProfileLoader.GetUserProfile.aspx) .
  
    
    

Para más información sobre cómo trabajar con los perfiles, vea  [Trabajar con perfiles de usuario en SharePoint 2013](work-with-user-profiles-in-sharepoint-2013.md).
  
    
    

## Requisitos previos de configuración del entorno de desarrollo para recuperar propiedades de perfil de usuario usando el modelo de objetos de cliente .NET de SharePoint 2013
<a name="bk_Prereqs"> </a>

Para crear una aplicación de consola que use el modelo de objetos de cliente .NET para recuperar propiedades de perfil de usuario, necesita lo siguiente:
  
    
    

- SharePoint Server 2013 con perfiles creados para el usuario actual y un usuario de destino.
    
  
- Visual Studio 2012
    
  
- Permisos de conexión de **control total** para obtener acceso a la aplicación del servicio de perfiles de usuario del usuario actual.
    
  

> **NOTA**
> Si no va a desarrollar en el PC donde tiene instalado SharePoint Server 2013, bájese los  [Componentes del cliente de SharePoint](http://www.microsoft.com/en-us/download/details.aspx?id=35585) que contienen los ensamblados de cliente de SharePoint 2013.
  
    
    


## Crear la aplicación de consola que recupera las propiedades de perfil de usuario usando el modelo de objetos de cliente .NET de SharePoint 2013
<a name="bk_RetrieveProps"> </a>


1. En el PC de desarrollo, abra Visual Studio y elija **Archivo**, **Nuevo**, **Proyecto**.
    
  
2. En el cuadro de diálogo **Nuevo proyecto**, seleccione **.NET Framework 4.5** en la lista desplegable situada en la parte superior del cuadro de diálogo.
    
  
3. En las plantillas del proyecto, elija **Windows** y luego elija **Aplicación de consola**.
    
  
4. Ponga al proyecto el nombre CSOMPerfilesDeUsuario y luego elija el botón **Aceptar**.
    
  
5. Agregue referencias a los ensamblados siguientes:
    
  - **Microsoft.SharePoint.Client**
    
  
  - **Microsoft.SharePoint.ClientRuntime**
    
  
  - **Microsoft.SharePoint.Client.UserProfiles**
    
  
6. En el método **Main**, defina variables para la dirección URL del servidor y el nombre de usuario de destino, tal como se muestra en el código siguiente.
    
  ```cs
  
const string serverUrl = "http://serverName/";
const string targetUser = "domainName\\\\userName";
  ```


> **NOTA**
> Recuerde sustituir los valores de marcador de posición  `http://serverName/` y `domainName\\\\userName` antes de ejecutar el código.
7. Inicialice el contexto de cliente de SharePoint como se ve en el código siguiente.
    
  ```cs
  
ClientContext clientContext = new ClientContext(serverUrl);

  ```

8. Obtenga las propiedades del usuario de destino a partir del objeto **PeopleManager**, tal como se ve en el código siguiente.
    
  ```cs
  
PeopleManager peopleManager = new PeopleManager(clientContext);
PersonProperties personProperties = peopleManager.GetPropertiesFor(targetUser);
  ```


El objeto **personProperties** es un objeto de cliente. Algunos objetos de cliente no contienen datos hasta que se inicializan. Por ejemplo, no se puede obtener acceso a los valores de propiedad del objeto **personProperties** hasta que se inicializa. Si intenta obtener acceso a una propiedad antes de inicializarla, recibirá la excepción **PropertyOrFieldNotInitializedException**.
    
  
9. Para inicializar el objeto **personProperties**, registre la solicitud que quiere ejecutar y luego ejecute la solicitud en el servidor, como se ve en el código siguiente.
    
  ```cs
  
clientContext.Load(personProperties, p => p.AccountName, p => p.UserProfileProperties);
clientContext.ExecuteQuery();
  ```


Al llamar al método **Load** (o al método **LoadQuery**), se pasa el objeto que se quiere recuperar o cambiar. En este ejemplo, la llamada al método **Load** pasa parámetros opcionales para filtrar la solicitud. Los parámetros son expresiones lambda que solicitan solo la propiedad **AccountName** y la propiedad **UserProfileProperties** del objeto **personProperties**.
    
> **SUGERENCIA**
> Para reducir el tráfico de red, tiene que solicitar solo las propiedades con las que quiere trabajar al llamar al método **Load**. Además, si va a trabajar con varios objetos, agrupe varias llamadas en el método **Load** cuando se pueda antes de llamar al método **ExecuteQuery**. 
10. Itere las propiedades de perfil de usuario y lea el nombre y el valor de cada propiedad, tal como se ve en el código siguiente.
    
  ```cs
  
foreach (var property in personProperties.UserProfileProperties)
{
    Console.WriteLine(string.Format("{0}: {1}", 
        property.Key.ToString(), property.Value.ToString()));
}

  ```


## Ejemplo de código: recuperar todas las propiedades de perfil de usuario usando el modelo de objetos de cliente .NET de SharePoint 2013
<a name="SP15UserProf_codeexample1"> </a>

En el siguiente ejemplo de código se ve cómo recuperar e iterar todas las propiedades de perfil de usuario de un usuario de destino, como se describe en el  [proceso anterior](how-to-retrieve-user-profile-properties-by-using-the-net-client-object-model-in.md#bk_RetrieveProps).
  
    
    

> **NOTA**
> Sustituya los valores de marcador de posición  `http://serverName/` y `domainName\\\\userName` antes de ejecutar el código.
  
    
    


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


## Ejemplo de código: recuperar las propiedades de perfil de usuario de las personas que me siguen usando el modelo de objetos de cliente .NET de SharePoint 2013
<a name="SP15UserProfilescodeInApp"> </a>

El siguiente ejemplo de código muestra cómo obtener, en una Complemento de SharePoint, las propiedades de perfiles de usuario de personas que le siguen.
  
    
    

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


## Ejemplo de código: recuperar un conjunto de propiedades de perfil de usuario usando el modelo de objetos de cliente .NET de SharePoint 2013
<a name="SP15UserProfilescode2"> </a>

En el siguiente ejemplo de código se ve cómo recuperar un conjunto determinado de propiedades de perfil de usuario correspondiente a un usuario de destino.
  
    
    

> **NOTA**
> Para recuperar el valor de una sola propiedad de perfil de usuario, use el método  [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) .
  
    
    

A diferencia del ejemplo de código anterior, que recupera un objeto  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) para el usuario de destino, en este ejemplo se llama al método [PeopleManager.GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) y se pasa un objeto [UserProfilePropertiesForUser](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfilePropertiesForUser.aspx) que especifica el usuario de destino y las propiedades de perfil de usuario que hay que recuperar. [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) devuelve una colección **IEnumerable<string>** que contiene los valores de las propiedades que especifique.
  
    
    

> **NOTA**
> Sustituya los valores de marcador de posición  `http://serverName/` y `domainName\\\\userName` antes de ejecutar el código.
  
    
    




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


## Recursos adicionales
<a name="bk_addresources"> </a>


-  [Trabajar con perfiles de usuario en SharePoint 2013](work-with-user-profiles-in-sharepoint-2013.md)
    
  
-  [Procedimiento para recuperar propiedades de perfil de usuario mediante el modelo de objetos de JavaScript en SharePoint 2013](how-to-retrieve-user-profile-properties-by-using-the-javascript-object-model-in.md)
    
  
-  [Cómo trabajar con perfiles de usuario y perfiles de organización con el modelo de objetos del servidor en SharePoint 2013](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md)
    
  
-  [Microsoft.SharePoint.Client.UserProfiles](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.aspx)
    
  

