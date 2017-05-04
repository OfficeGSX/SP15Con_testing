---
title: 如何：使用 SharePoint 2013 中的 .NET 客户端对象模型检索用户配置文件属性
ms.prod: SHAREPOINT
ms.assetid: 236ebaf8-f92e-4192-9b51-0a9de0210885
---


# 如何：使用 SharePoint 2013 中的 .NET 客户端对象模型检索用户配置文件属性
了解如何使用 SharePoint 2013 .NET 客户端对象模型以编程方式检索用户配置文件属性。
## 什么是 SharePoint 2013 中的用户配置文件属性？
<a name="bkmk_WhatIs"> </a>

用户属性和用户配置文件属性提供有关 SharePoint 用户的信息，如显示名称、电子邮件、标题以及其他业务和个人信息。在客户端 API 中，您可以从  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) 对象及其 [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) 属性访问这些属性。 [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) 属性包含所有用户配置文件属性，但 [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) 对象包含更容易访问的常用属性（例如， [AccountName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.AccountName.aspx) 、 [DisplayName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.DisplayName.aspx) 和 [Email](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.Email.aspx) ）。
  
    
    
 [PeopleManager](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.aspx) 对象包含以下方法，您可以使用这些方法来通过 .NET 客户端对象模型检索用户属性和用户配置文件属性：
  
    
    

-  [GetMyProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetMyProperties.aspx) 方法和 [GetPropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetPropertiesFor.aspx) 方法返回一个 [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) 对象。
    
  
-  [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) 方法和 [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) 方法返回您指定的用户配置文件属性的值。
    
  
来自客户端 API 的用户配置文件属性是只读的（配置文件图片除外，您可以使用  [PeopleManager.SetMyProfilePicture](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.SetMyProfilePicture.aspx) 方法对该图片进行更改）。如果您希望更改其他用户配置文件属性，则必须使用服务器对象模型。
  
    
    

> **注释**
> 与服务器端版本不同，客户端版本的  [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) 对象不包含所有的用户属性。但是，客户端版本确实提供了为当前用户创建个人网站的方法。若要检索当前用户的客户端 [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) ，请使用 [ProfileLoader.GetUserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.ProfileLoader.GetUserProfile.aspx) 方法。
  
    
    

有关使用配置文件的详细信息，请参阅 [在 SharePoint 2013 中使用用户配置文件](work-with-user-profiles-in-sharepoint-2013.md)。
  
    
    

## 设置使用 SharePoint 2013 .NET 客户端对象模型检索用户配置文件属性的开发环境的先决条件
<a name="bk_Prereqs"> </a>

若要创建一个使用 .NET 客户端对象模型检索用户配置文件属性的控制台应用程序，您将需要具备以下条件：
  
    
    

- 为当前用户和目标用户创建有配置文件的 SharePoint Server 2013
    
  
- Visual Studio 2008
    
  
- 访问当前用户的 User Profile Service 应用程序的"完全控制"连接权限。
    
  

> **注释**
> 如果没有在运行 SharePoint Server 2013 的计算机上进行开发，请下载包含 SharePoint 2013 客户端程序集的  [SharePoint 客户端组件](http://www.microsoft.com/en-us/download/details.aspx?id=35585)。 
  
    
    


## 创建使用 SharePoint 2013 .NET 客户端对象模型检索用户配置文件属性的控制台应用程序
<a name="bk_RetrieveProps"> </a>


1. 在您的开发计算机上，打开 Visual Studio 并依次选择"文件"、"新建"、"项目"。
    
  
2. 在"新建项目"对话框中，从该对话框顶部的下拉列表中选择".NET Framework 4.5"。
    
  
3. 从项目模板中，选择"Windows"，然后选择"控制台应用程序"。
    
  
4. 将项目命名为 UserProfilesCSOM，然后选择"确定"按钮。
    
  
5. 添加对以下程序集的引用：
    
  - **Microsoft.SharePoint.Client**
    
  
  - **Microsoft.SharePoint.ClientRuntime**
    
  
  - **Microsoft.SharePoint.Client.UserProfiles**
    
  
6. 在 **Main** 方法中，定义服务器 URL 和目标用户名的变量，如以下代码所示。
    
  ```cs
  
const string serverUrl = "http://serverName/";
const string targetUser = "domainName\\\\userName";
  ```


> **注释**
> 请记住在运行代码之前，替换  `http://serverName/` 和 `domainName\\\\userName` 占位符值。
7. 初始化 SharePoint 客户端上下文，如以下代码所示。
    
  ```cs
  
ClientContext clientContext = new ClientContext(serverUrl);

  ```

8. 从 **PeopleManager** 对象中获取目标用户的属性，如以下代码所示。
    
  ```cs
  
PeopleManager peopleManager = new PeopleManager(clientContext);
PersonProperties personProperties = peopleManager.GetPropertiesFor(targetUser);
  ```


**personProperties** 对象是一个客户端对象。有些客户端对象在被初始化之后才会包含数据。例如，直到您初始化 **personProperties** 对象之后，才能访问该对象的属性值。如果您在其初始化之前尝试访问属性，则会收到一条 **PropertyOrFieldNotInitializedException** 异常消息。
    
  
9. 若要初始化 **personProperties** 对象，请先注册您要运行的请求，然后在服务器上运行该请求，如以下代码所示。
    
  ```cs
  
clientContext.Load(personProperties, p => p.AccountName, p => p.UserProfileProperties);
clientContext.ExecuteQuery();
  ```


当您调用 **Load** 方法（或 **LoadQuery** 方法）时，您即传入了要检索或更改的对象。在此示例中，调用 **Load** 方法可传入可选参数以用于筛选请求。这些参数是 lambda 表达式，仅请求 **personProperties** 对象的 **AccountName** 属性和 **UserProfileProperties** 属性。
    
> **提示**
> 若要降低网络流量，请在您调用 **Load** 方法时只请求您要使用的属性。此外，如果您要处理多个对象，请在调用 **ExecuteQuery** 方法之前，分组对 **Load** 方法的多个调用。
10. 循环访问用户配置文件属性以及读取每个属性的名称和值，如以下代码所示。
    
  ```cs
  
foreach (var property in personProperties.UserProfileProperties)
{
    Console.WriteLine(string.Format("{0}: {1}", 
        property.Key.ToString(), property.Value.ToString()));
}

  ```


## 代码示例：使用 SharePoint 2013 .NET 客户端对象模型检索所有用户配置文件属性
<a name="SP15UserProf_codeexample1"> </a>

以下代码示例演示如何检索和循环访问目标用户的所有用户配置文件属性，如 [上一过程](how-to-retrieve-user-profile-properties-by-using-the-net-client-object-model-in.md#bk_RetrieveProps)中所述。
  
    
    

> **注释**
> 在运行代码之前，请先替换  `http://serverName/` 和 `domainName\\\\userName` 占位符值。
  
    
    


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


## 代码示例：使用 SharePoint 2013 .NET 客户端对象模型检索关注我的人员的用户配置文件属性
<a name="SP15UserProfilescodeInApp"> </a>

以下代码示例表明如何在 SharePoint 外接程序中获取关注您的人员的用户配置文件属性。
  
    
    

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


## 代码示例：使用 SharePoint 2013 .NET 客户端对象模型检索用户配置文件属性集
<a name="SP15UserProfilescode2"> </a>

以下代码示例演示如何检索目标用户的用户配置文件属性的特定集。
  
    
    

> **注释**
> 若要仅检索一个用户配置文件属性的值，请使用  [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) 方法。
  
    
    

与上述检索目标用户的  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) 对象的代码示例不同，此示例调用 [PeopleManager.GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) 方法并传入指定目标用户和要检索的用户配置文件属性的 [UserProfilePropertiesForUser](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfilePropertiesForUser.aspx) 对象。 [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) 将返回一个包含所指定属性的值的 **IEnumerable<string>** 集合。
  
    
    

> **注释**
> 在运行代码之前，请先替换  `http://serverName/` 和 `domainName\\\\userName` 占位符值。
  
    
    




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


## 其他资源
<a name="bk_addresources"> </a>


-  [在 SharePoint 2013 中使用用户配置文件](work-with-user-profiles-in-sharepoint-2013.md)
    
  
-  [如何：在 SharePoint 2013 中使用 JavaScript 对象模型检索用户配置文件属性](how-to-retrieve-user-profile-properties-by-using-the-javascript-object-model-in.md)
    
  
-  [如何：通过使用 SharePoint 2013 中的服务器对象模型来处理用户配置文件和组织配置文件](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md)
    
  
-  [Microsoft.SharePoint.Client.UserProfiles](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.aspx)
    
  

