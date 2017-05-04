---
title: 如何：了解通过使用 SharePoint 2013 中的 .NET 客户端对象模型读取和写入好友动态订阅源
ms.prod: SHAREPOINT
ms.assetid: 3c15ede5-8a59-47e6-a0b2-c17ec6bf4ae1
---


# 如何：了解通过使用 SharePoint 2013 中的 .NET 客户端对象模型读取和写入好友动态订阅源
创建一个使用 SharePoint 2013 .NET 客户端对象模型读取和写入好友动态订阅源的控制台应用程序。
## 创建使用 SharePoint 2013 .NET 客户端对象模型读取和写入好友动态订阅源的控制台应用程序的先决条件
<a name="bkmk_Prereqs"> </a>

您将创建的控制台应用程序可检索目标用户的新闻源并输出来自编号列表的每个线索中的根帖。然后，对所选线索发布简单的文本回复。同样的方法 ( [CreatePost](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.CreatePost.aspx) ) 可用于发布帖子和回复新闻源。
  
    
    
若要创建控制台应用程序，您将需要具备以下条件：
  
    
    

- 配置有"我的网站"、为当前用户和目标用户创建有个人网站以及带有由目标用户撰写的几个帖子的 SharePoint Server 2013
    
  
- Visual Studio 2008
    
  
- 对所登录用户的 User Profile Service 应用程序具有"完全控制"访问权限
    
  

> **注释**
> 如果没有在运行 SharePoint Server 2013 的计算机上进行开发，请下载包含 SharePoint 2013 客户端程序集的  [SharePoint 客户端组件](http://www.microsoft.com/en-us/download/details.aspx?id=35585)。 
  
    
    


### 了解有关使用 SharePoint 2013 好友动态订阅源的核心概念
<a name="bkmk_CoreConcepts"> </a>

表 1 包含指向介绍您在开始前应当了解的核心概念的文章链接。
  
    
    

**表 1. 关于使用 SharePoint 2013 好友动态订阅源的核心概念**


|**文章标题**|**说明**|
|:-----|:-----|
| [使用 SharePoint 2013 中的社交功能开始进行开发](get-started-developing-with-social-features-in-sharepoint-2013.md) <br/> |了解如何开始使用 好友动态订阅源和微博帖子进行编程、关注好友和内容（文档、网站和标签），以及使用用户配置文件。  <br/> |
| [使用 SharePoint 2013 中的社交源](work-with-social-feeds-in-sharepoint-2013.md) <br/> |了解使用好友动态订阅源的常见编程任务和用于执行任务的 API。  <br/> |
   

## 在 Visual Studio 2008 中创建控制台应用程序和添加对客户端程序集的引用
<a name="bkmk_CreateApp"> </a>


1. 在您的开发计算机上，打开 Visual Studio 2008。
    
  
2. 在菜单栏上，依次选择"文件"、"新建"、"项目"。
    
  
3. 在"新建项目"对话框中，从该对话框顶部的下拉列表中选择".NET Framework 4.5"。
    
  
4. 从"模板"列表中，选择"Windows"，然后选择"控制台应用程序"模板。
    
  
5. 将项目命名为 ReadWriteMySite，然后单击"确定"按钮。
    
  
6. 添加对客户端程序集的引用，如下所示：
    
1. 在"解决方案资源管理器"中，打开 ReadWriteMySite 项目的快捷菜单，然后选择"添加引用"。
    
  
2. 在"引用管理器"对话框中，选择以下程序集：
    
  - **Microsoft.SharePoint.Client**
    
  
  - **Microsoft.SharePoint.Client.Runtime**
    
  
  - **Microsoft.SharePoint.Client.UserProfiles**
    
  

    如果您在运行 SharePoint Server 2013 的计算机上进行开发，则程序集将位于"扩展"类别中。否则，请浏览到存放您下载的客户端程序集的位置（请参阅  [SharePoint 客户端组件](http://www.microsoft.com/en-us/download/details.aspx?id=30355)）。
    
  
7. 在 Program.cs 文件中，添加以下 **using** 语句。
    
  ```cs
  
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.Social;
  ```


## 使用 SharePoint 2013 .NET 客户端对象模型检索目标用户的好友动态订阅源
<a name="bkmk_RetrieveFeed"> </a>


1. 为服务器 URL 和目标用户的帐户凭据声明变量。
    
  ```cs
  
const string serverUrl = "http://serverName/";
const string targetUser = "domainName\\\\userName";
  ```


> **注释**
> 请记住在运行代码之前，替换  `http://serverName/` 和 `domainName\\\\userName` 占位符值。
2. 在 **Main** 方法中，初始化 SharePoint 客户端上下文。
    
  ```cs
  
ClientContext clientContext = new ClientContext(serverUrl);

  ```

3. 创建  [SocialFeedManager](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.aspx) 实例。
    
  ```cs
  
SocialFeedManager feedManager = new SocialFeedManager(clientContext);
  ```

4. 为您要检索的订阅源内容指定参数。
    
  ```cs
  SocialFeedOptions feedOptions = new SocialFeedOptions();
feedOptions.MaxThreadCount = 10;
  ```


    默认选项将返回订阅源中前 20 个线索，按最后修改日期排序。
    
  
5. 获取目标用户的订阅源。
    
  ```cs
  
ClientResult<SocialFeed> feed = feedManager.GetFeedFor(targetUser, feedOptions);
clientContext.ExecuteQuery();
  ```


[GetFeedFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.GetFeedFor.aspx) 会返回将线索集合存储在其 [Value](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.ClientResult`1.Value.aspx) 属性中的 **ClientResult<T>** 对象。
    
  

## 使用 SharePoint 2013 .NET 客户端对象模型循环访问和读取好友动态订阅源
<a name="bkmk_ReadFeed"> </a>

以下代码可循环访问订阅源中的线索。它可以检查每个线索是否具有  [CanReply](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialThreadAttributes.CanReply.aspx) 属性，并获取线索标识符和根帖的文本。该代码还可以创建词典以存储线索标识符（用于回复线索）以及将根帖的文本写入控制台。
  
    
    

```cs

Dictionary<int, string> idDictionary = new Dictionary<int, string>();
for (int i = 0; i < feed.Value.Threads.Length; i++)
{
    SocialThread thread = feed.Value.Threads[i];
    string postText = thread.RootPost.Text;
    if (thread.Attributes.HasFlag(SocialThreadAttributes.CanReply))
    {
        idDictionary.Add(i, thread.Id);
        Console.WriteLine("\\t" + (i + 1) + ". " + postText);
    }
}
```


## 使用 SharePoint 2013 .NET 客户端对象模型对好友动态订阅源发表回复
<a name="bkmk_WriteFeed"> </a>


1. （仅与 UI 相关）获取要回复的线索并提示用户的回复。
    
  ```cs
  
Console.Write("Which post number do you want to reply to?  ");
string threadToReplyTo = "";
int threadNumber = int.Parse(Console.ReadLine()) - 1;
idDictionary.TryGetValue(threadNumber, out threadToReplyTo);
Console.Write("Type your reply:  ");
  ```

2. 定义回复。以下代码可从控制台应用程序获取回复的文本内容。
    
  ```cs
  
SocialPostCreationData postCreationData = new SocialPostCreationData();
postCreationData.ContentText = Console.ReadLine();
  ```

3. 发布回复。 _threadToReplyTo_ 参数表示线索的 [Id](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialThread.Id.aspx) 属性。
    
  ```cs
  
feedManager.CreatePost(threadToReplyTo, postCreationData);
clientContext.ExecuteQuery();
  ```


> **注释**
>  [CreatePost](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.CreatePost.aspx) 方法还可以用于通过为第一个参数传递 **null** 将根帖发布到当前用户的订阅源中。
4. （仅与 UI 相关）退出程序。
    
  ```cs
  
Console.WriteLine("Your reply was published.");
Console.ReadKey(false);
  ```

5. 若要测试控制台应用程序，请在菜单栏上，依次选择"调试"和"开始调试"。
    
  

## 代码示例：使用 SharePoint 2013 .NET 客户端对象模型检索订阅源和回帖
<a name="bkmk_CodeExample"> </a>

以下示例是来自 Program.cs 文件的完整代码。
  
    
    

```cs

using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.Social;

namespace ReadWriteMySite
{
    class Program
    {
        static void Main(string[] args)
        {

            // Replace the following placeholder values with the target server running SharePoint and the
            // target thread owner.
            const string serverUrl = "http://serverName/";
            const string targetUser = "domainName\\\\userName";

            // Connect to the client context.
            ClientContext clientContext = new ClientContext(serverUrl);

            // Get the SocialFeedManager instance.
            SocialFeedManager feedManager = new SocialFeedManager(clientContext);

            // Specify the parameters for the feed content that you want to retrieve.
            SocialFeedOptions feedOptions = new SocialFeedOptions();
            feedOptions.MaxThreadCount = 10;

            // Get the target owner's feed (posts and activities) and then run the request on the server.
            ClientResult<SocialFeed> feed = feedManager.GetFeedFor(targetUser, feedOptions);
            clientContext.ExecuteQuery();

            // Create a dictionary to store the Id property of each thread. This code example stores
            // the ID so a user can select a thread to reply to from the console application.
            Dictionary<int, string> idDictionary = new Dictionary<int, string>();
            for (int i = 0; i < feed.Value.Threads.Length; i++)
            {
                SocialThread thread = feed.Value.Threads[i];

                // Keep only the threads that can be replied to.
                if (thread.Attributes.HasFlag(SocialThreadAttributes.CanReply))
                {
                    idDictionary.Add(i, thread.Id);

                    // Write out the text of the post.
                    Console.WriteLine("\\t" + (i + 1) + ". " + thread.RootPost.Text);
                }
            }
            Console.Write("Which post number do you want to reply to?  ");

            string threadToReplyTo = "";
            int threadNumber = int.Parse(Console.ReadLine()) - 1;
            idDictionary.TryGetValue(threadNumber, out threadToReplyTo);

            Console.Write("Type your reply:  ");

            // Define properties for the reply.
            SocialPostCreationData postCreationData = new SocialPostCreationData();
            postCreationData.ContentText = Console.ReadLine();
            
            // Post the reply and make the changes on the server.
            feedManager.CreatePost(threadToReplyTo, postCreationData);
            clientContext.ExecuteQuery();

            Console.WriteLine("Your reply was published.");
            Console.ReadKey(false);

            // TODO: Add error handling and input validation.
        }
    }
}
```


## 后续步骤
<a name="SP15ReadWriteSocial_nextsteps"> </a>

若要了解如何通过 .NET 客户端对象模型使用好友动态订阅源来执行更多读取任务和写入任务，请参阅以下内容：
  
    
    

-  [如何：使用 SharePoint 2013 中的 .NET 客户端对象模型创建和删除帖子以及检索好友动态订阅源](how-to-create-and-delete-posts-and-retrieve-the-social-feed-by-using-the-net-cli.md)
    
  

## 其他资源
<a name="SP15ReadWriteSocial_addlresources"> </a>


-  [使用 SharePoint 2013 中的社交功能开始进行开发](get-started-developing-with-social-features-in-sharepoint-2013.md)
    
  
-  [使用 SharePoint 2013 中的社交源](work-with-social-feeds-in-sharepoint-2013.md)
    
  

