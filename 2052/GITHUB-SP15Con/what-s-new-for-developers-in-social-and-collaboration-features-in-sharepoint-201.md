---
title: SharePoint 2013 中的社交和协作功能的面向开发人员的新增内容
ms.prod: SHAREPOINT
ms.assetid: 65365b1d-cde5-47cd-8b04-1b76be0e3490
---


# SharePoint 2013 中的社交和协作功能的面向开发人员的新增内容
了解我的网站的新的和已更改的社会和协作功能和 SharePoint 2013 中的社区网站开发方案。
SharePoint 2013 中的社交和协作功能使用户能够轻松地进行沟通并始终了解最新信息。个人网站和工作组网站中改进的好友动态订阅源可帮助用户了解他们所关心的人员和内容的最新动态。新社区网站功能提供丰富的社区体验，可让用户轻松查找和共享信息以及查找与其具有共同爱好的人员。
  
    
    

有关 SharePoint 2013 中的新社会和协作功能的深入概述，请参阅 TechNet 上的  [SharePoint 2013 中的社会计算的新增功能](http://technet.microsoft.com/zh-cn/library/jj219766%28office.15%29.aspx)。有关使用社会和协作功能编程的详细信息，请参阅  [SharePoint 2013 中的社交和协作功能](social-and-collaboration-features-in-sharepoint-2013.md)。
## SharePoint Server 2013 中的新的和已更改的我的网站功能
<a name="bkmk_Social"> </a>

我的网站社交 API 包括用户配置文件和社会数据，还包含许多新功能和已更改的功能。"我的网站"和团队网站的新功能提供源内的交互式对话体验，使用户能够更轻松地与好友保持联系，并了解与其有关的内容。
  
    
    
SharePoint Server 2013 上的"新闻源"页面显示多个这些改进，包括具有以下功能的文本框：使用户能够快速发布微博公告和公告的交互式对话源，以及从用户正密切注意的人员和内容更新。
  
    
    

### 新社交命名空间提供了密切关注人员和内容的好友动态订阅源 API

 **Social** 命名空间包含用于源和微博帖子以及密切关注人员和内容的主 API。有关详细信息，请参阅 [Microsoft.SharePoint.Client.Social](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.aspx) （针对 .NET 客户端对象模型）、 [SP.Social](http://msdn.microsoft.com/library/43d47f01-c085-0e77-bd01-48bcb7d5bb35%28Office.15%29.aspx)（针对 JavaScript 对象模型）和  [Microsoft.Office.Server.Social](https://msdn.microsoft.com/library/Microsoft.Office.Server.Social.aspx) （针对服务器对象模型）。
  
    
    

> **注释**
>  [Microsoft.Office.Server.ActivityFeed](https://msdn.microsoft.com/library/Microsoft.Office.Server.ActivityFeed.aspx) 命名空间中的 API 已被弃用。请参阅 [已弃用和已删除的我的网站社交 API 和功能](#bkmk_DeprecatedAPI)。 
  
    
    


### 密切关注人员和内容的好友动态订阅源的新客户端 API SharePoint 2013 中的用户属性

SharePoint 2013 包括新客户端 API，您可以将这些 API 与好友动态订阅源结合使用，通过它们密切关注人员和内容，以及在联机、本地和移动开发中检索用户属性。如有可能，应在 SharePoint 2013 开发中使用客户端 API，而不要使用服务器对象模型或 Web 服务。客户端 API 包括托管客户端对象模型、JavaScript 对象模型和代表性状态传输 (REST) 服务。如果您正在开发 SharePoint 外接程序，则必须使用客户端 API。
  
    
    
并不是客户端 API 中 Microsoft.Office.Server.UserProfiles 程序集中的所有服务器端功能均可供使用。例如，在  [Microsoft.Office.Server.Audience](https://msdn.microsoft.com/library/Microsoft.Office.Server.Audience.aspx) 命名空间、 [Microsoft.Office.Server.ReputationModel](https://msdn.microsoft.com/library/Microsoft.Office.Server.ReputationModel.aspx) 命名空间或 [Microsoft.Office.Server.SocialData](https://msdn.microsoft.com/library/Microsoft.Office.Server.SocialData.aspx) 命名空间中没有 API 的客户端访问权限。若要查看哪些 API 可供使用，请参阅 [Microsoft.SharePoint.Client.Social](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.aspx) 命名空间和 [Microsoft.SharePoint.Client.UserProfiles](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.aspx) 命名空间。
  
    
    
有关如何访问我的网站社交 客户端 API 的详细信息，请参阅 [使用 SharePoint 2013 中的社交功能开始进行开发](get-started-developing-with-social-features-in-sharepoint-2013.md)。有关SharePoint 2013中的 API 集以及何时使用它们的详细信息，请参阅  [在 SharePoint 2013 中选择正确的 API 集](choose-the-right-api-set-in-sharepoint-2013.md)。
  
    
    

### 使用 ProfileLoader.CreatePersonalSiteEnqueueBulk 方法为多个用户提供个人网站和 OneDrive for Business（仅 SharePoint Online 上的 我的网站 主机管理员）
<a name="bk_CreatePersonalSiteEnqueueBulk"> </a>

我的网站 主机管理员可以使用 **ProfileLoader.CreatePersonalSiteEnqueueBulk** 方法，以编程方式为 SharePoint Online 上的多个用户配置个人网站，其中包括 OneDrive for Business 等功能和网站页面。
  
    
    
以下代码示例在控制台应用程序中使用 .NET 客户端对象模型。在运行示例之前，添加对 Microsoft.SharePoint.Client.dll、Microsoft.SharePoint.Client.Runtime.dll 和 Microsoft.SharePoint.Client.UserProfiles.dll 的引用，并更改 **userName**、 **passwordStr** 和 **serverUrl** 变量的占位符值。 **serverUrl** 变量必须为 SharePoint Online 管理中心的 URL。
  
    
    

> **注释**
> 要获取所需的客户端 DLL，请下载  [SharePoint Online 客户端组件 SDK](http://www.microsoft.com/en-us/download/details.aspx?id=42038)。 
  
    
    




```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Security;
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.UserProfiles;

namespace CreatePersonalSiteBulkConsole
{
    class Program
    {
        static void Main(string[] args)
        {
            string userName = "administrator@contoso.onmicrosoft.com";
            string passwordStr = "password";
            string serverUrl = "https://contoso-admin.sharepoint.com/";

            using (var clientContext = new ClientContext(serverUrl))
            {
                SecureString password = new SecureString();
                Array.ForEach(passwordStr.ToCharArray(), c => password.AppendChar(c));

                var credentials = new SharePointOnlineCredentials(userName, password);

                clientContext.Credentials = credentials;

                var web = clientContext.Web;
                clientContext.Load(web);
                clientContext.ExecuteQuery();
                ProfileLoader loader = ProfileLoader.GetProfileLoader(clientContext);

                if (loader == null)
                {
                    throw new InvalidOperationException("Failed to get ProfileLoader");
                }

                string[] userEmails = { "usera@contoso.onmicrosoft.com", "userb@contoso.onmicrosoft.com" };
                loader.CreatePersonalSiteEnqueueBulk(userEmails);
                loader.Context.ExecuteQuery();
            }
        }
    }
}
```

要将 **CreatePersonalSiteEnqueueBulk** 方法与 Windows PowerShell 一起使用，请首先更改以下命令中的占位符值（SharePoint Online 管理中心的 URL 和用户名），然后在 SharePoint Management Shell 中运行命令。
  
    
    



```

$webUrl = "https://yoursharepointadmin.sharepoint.com"
$ctx = New-Object Microsoft.SharePoint.Client.ClientContext($webUrl)

$web = $ctx.Web
$username = "admin@myadmin.sharepoint.com"
$password = read-host -AsSecureString

$ctx.Credentials = New-Object Microsoft.SharePoint.Client.SharePointOnlineCredentials($username,$password)

$ctx.Load($web)
$ctx.ExecuteQuery()

[System.Reflection.Assembly]::LoadWithPartialName("Microsoft.SharePoint.Client.UserProfiles")

$loader =[Microsoft.SharePoint.Client.UserProfiles.ProfileLoader]::GetProfileLoader($ctx)

#To get profile
$profile = $loader.GetUserProfile()
$ctx.Load($profile)
$ctx.ExecuteQuery()
$profile 
#To enqueue profile
$loader.CreatePersonalSiteEnqueueBulk(@("user1@domain.com")) 
$loader.Context.ExecuteQuery()
```

有关详细信息，请参阅 [您想以编程方式配置 Office 365 中的个人网站 (One Drive for Business)？](http://blogs.msdn.com/b/frank_marasco/archive/2014/03/25/so-you-want-to-programmatically-provision-personal-sites-one-drive-for-business-in-office-365.aspx)和 [使用 Windows PowerShell 管理 SharePoint 2013](https://technet.microsoft.com/zh-cn/library/ee806878%28v=office.15%29.aspx)。
  
    
    

### 针对 SharePoint 2013 中的用户和用户属性的新对象
<a name="bkmk_NewUserObjects"> </a>

SharePoint Server 2013 包括表示用户和用户属性的新对象：
  
    
    

-  [SocialActor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialActor.aspx) 对象表示源和以下活动的用户（和其他实体）。
    
  
-  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) 对象包含一般用户属性和用户配置文件属性。
    
  

> **注释**
> 服务器对象模型版本是  [SPSocialActor](https://msdn.microsoft.com/library/Microsoft.Office.Server.Social.SPSocialActor.aspx) 对象和 [PersonProperties](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.PersonProperties.aspx) 对象。
  
    
    

SharePoint Server 2013 还包括新客户端  [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) 对象，该对象提供可用于为当前用户创建个人网站的方法。但是，它不包含服务器端 [UserProfile](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.UserProfile.aspx) 对象包含的所有用户属性。若要通过客户端代码访问所有用户属性，请使用 [PeopleManager.GetMyProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetMyProperties.aspx) 方法或 [PeopleManager.GetPropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetPropertiesFor.aspx) 方法（用户配置文件属性存储在 [PersonProperties.UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) 属性中），或者使用 [PeopleManager.GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) 方法或 [PeopleManager.GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) 方法。
  
    
    

### 新增客户端人员选取器控件
<a name="bkmk_NewUserObjects"> </a>

客户端人员选取器控件是一种 HTML 和 JavaScript 控件，可支持跨浏览器选取人员、组和声明。可以使用与服务器端版本的控件相同的设置来配置该选取器，这些设置包括控件特定属性（如允许多个用户或用户和组）以及 Web 应用程序级配置设置（如 Active Directory 域服务参数或锁定特定林）。有关详细信息，请参阅 [在 SharePoint 托管的 SharePoint 外接程序中使用客户端人员选取器控件](http://msdn.microsoft.com/library/383f265f-ed44-4d09-b2f6-366f13d52347%28Office.15%29.aspx)。
  
    
    

### 已弃用和已删除的我的网站社交 API 和功能
<a name="bkmk_DeprecatedAPI"> </a>

以下我的网站社交 API 和功能在 SharePoint Server 2013 中已被弃用：
  
    
    

-  [Microsoft.Office.Server.ActivityFeed](https://msdn.microsoft.com/library/Microsoft.Office.Server.ActivityFeed.aspx) 命名空间中的 API 已被弃用。 **Social** 命名空间提供在 SharePoint 2013 中以编程方式用于好友动态订阅源的 API。对于向后兼容性，SharePoint 2010 中的 **ActivityEvent** 项作为不可答复的事件在 SharePoint 2013 源中显示。（管理中心中必须启用遗留事件迁移）。
    
  
- 我的网站 RSS 源 (ActivityFeed.aspx) 替换为 REST 服务中的新 API、客户端对象模型和 JavaScript 对象模型。若要迁移使用此 API（最好为客户端 API）的自定义 SharePoint 2010 代码，请用对新 API 的调用替换对 ActivityFeed.aspx 的所有请求，并处理以 JavaScript 对象表示法 (JSON) 格式返回的源数据。
    
  
- "最近的活动"Web 部件替换为新的"新闻源"Web 部件，后者支持多线索检索和动态源检索。
    
    > **注释**
    > 我们不支持新闻源 Web 部件或其他源 Web 部件（例如工作组网站上的网站源 Web 部件）的自定义。如果您确认要自定义这些 Web 部件，例如使用 JavaScript 覆盖，请注意您的自定义可能在 SharePoint 更新时中断。 
- "社交评论"Web 部件已被弃用。
    
  
- 以下活动事件不再自动通知源：配置文件更新、即将到来的生日、即将到来的工作场所周年日、新成员资格以及更改经理。但是，可以为这些活动创建自定义事件接收器。尚未添加任何新的社会事件。
    
  
-  [Privacy](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.Privacy.aspx) 枚举中的以下字段已被弃用： **Contacts**、 **Organization** 和 **Manager**。SharePoint 2013 仅提供 **Private**（"只有我"）和 **Public**（"任何人"）隐私设置。现有隐私设置将在用户进行更改前一直保留。若要迁移使用此 API 的自定义 SharePoint 2010 代码，请替换对已弃用的隐私字段的所有引用。
    
  
- 从 **SocialFollowingManager** 访问的"关注的人"API 取代 SharePoint Server 2010 的"同事"功能。"同事"页面替换为"我关注的人"页面。使用户能够将同事组织到各组中的"组"功能不再可用。
    
  
- SharePoint 2013 中的组织配置文件已过时，弃用了以下类型： **OrganizationProfile**、 **OrganizationProfileManager**、 **OrganizationMembershipType**、 **OrganizationNotFoundException**、 **OrganizationProfileChange**、 **OrganizationProfileChangeQuery**、 **OrganizationProfileMembershipChange** 和 **OrganizationProfileValueCollection**。
    
  
- "我的链接"功能在 SharePoint 2013 中已被弃用。
    
  

## SharePoint 2013 中的新社区网站功能
<a name="bkmk_Collab"> </a>

新社区网站功能包括新网站模板和改进的讨论体验。声望、类别、特色讨论、问题发布类型和最佳答复使社区成员可轻松查找流行的讨论、相关信息和具有相似兴趣的人。成员在参与社区时建立声望。
  
    
    
社区网站功能不公布针对开发的特定。若要扩展社区网站功能，请使用 SharePoint 网站并直接列出 API。例如，可使用 SharePoint API 来自定义网站和列表模板、从社区为好友动态订阅源创建自定义活动、将声望信息集成到搜索结果中、自定义信誉模型或创建工作流来主持讨论。
  
    
    
下面的列表包含有关使用社区网站功能进行开发的信息：
  
    
    

- 社区网站使用 **Community** 网站模板 ( [Id](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.WebTemplate.Id.aspx) = **62**)。该网站模板不适用于公共模板。讨论板列表的模板类型是 [DiscussionBoard](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.ListTemplateType.DiscussionBoard.aspx) （值 = **108**）。
    
  
- 激活"社区网站"功能将激活 **CommunityEventReceiver** 事件接收器。
    
  
- 若要自定义客户端呈现的列表视图，您必须使用 JavaScript 重写来替换该视图。无法通过 SharePoint 2013 API 扩展列表视图。有关详细信息，请参阅 [使用客户端呈现在 SharePoint 外接程序中自定义列表视图](http://msdn.microsoft.com/library/8d5cabb2-70d0-46a0-bfe0-9e21f8d67d86%28Office.15%29.aspx)。
    
  
- 社区网站使用异步事件来恒心对象。如果异步事件在后台运行，则在尝试更新列表或列表项时，可能遇到 *保存*  冲突，并且对象句柄可能过期。
    
一种解决方法是处理"更新"调用返回的异常，在重新尝试调用前刷新示例，并循环多次重试，如下面的代码示例所示。
    


  ```cs
  
int retries = 1;
while (retries <= 10)
{
    try
    {
        spListItem.IconOverlay = urlString;
        spListItem.Update();
        break;
    }
    catch (SPException saveConflict)
    {
        spListItem = web.Lists.GetItemById(spListItem.ID);
        retries++;
        if (retries > 10) throw;
        System.Threading.Thread.Sleep(1000);
    }
}

  ```


## 其他资源
<a name="SP15NewSocial_addlresources"> </a>


-  [SharePoint 2013 中的社交和协作功能](social-and-collaboration-features-in-sharepoint-2013.md)
    
  
-  [使用 SharePoint 2013 中的社交功能开始进行开发](get-started-developing-with-social-features-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中面向开发人员的新增功能](what’s-new-for-developers-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 中的社会计算的新增功能](http://technet.microsoft.com/zh-cn/library/jj219766%28office.15%29.aspx)
    
  

