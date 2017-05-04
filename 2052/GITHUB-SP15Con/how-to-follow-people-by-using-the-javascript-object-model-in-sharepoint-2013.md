---
title: 如何：在 SharePoint 2013 中使用 JavaScript 对象模型关注好友
ms.prod: SHAREPOINT
ms.assetid: 2643c286-47c9-4a7a-9273-7474394477d6
---


# 如何：在 SharePoint 2013 中使用 JavaScript 对象模型关注好友
了解如何通过 SharePoint Server 2013 JavaScript 对象模型使用关注好友功能。
## 为什么在 SharePoint Server 2013 中使用关注好友功能？
<a name="bk_FollowingPeopleFeatures"> </a>

在 SharePoint Server 2013 中，关注好友功能可帮助用户保持彼此之间的联系。例如当一个用户关注了某人，则那个人的文章和活动就会显示在该用户的新闻源中。通过使用关注好友功能来关注用户所关心的人，可以提高应用程序或解决方案的相关性。在 JavaScript 对象模型中，您所关注的好友用  [SocialActor](http://msdn.microsoft.com/library/4e369fd5-b9b0-9804-957e-b3e39c559cd4%28Office.15%29.aspx) 对象表示。若要在 JavaScript 对象模型中执行核心关注好友任务，请使用 [SocialFollowingManager](http://msdn.microsoft.com/library/9ee1c0c0-b864-f0c3-f0cb-4dd4f1870dfa%28Office.15%29.aspx) 对象。本文展示如何通过 JavaScript 对象模型来使用关注好友功能。
  
    
    

> **注释**
>  [SocialFollowingManager](http://msdn.microsoft.com/library/9ee1c0c0-b864-f0c3-f0cb-4dd4f1870dfa%28Office.15%29.aspx) 是建议用于关注好友和内容的 API。但是， [PeopleManager](http://msdn.microsoft.com/library/985fd2df-0e31-6ece-b846-ba2ccb156d00%28Office.15%29.aspx) 对象还包含关注好友的其他功能，如可获取其他用户关注状态的 [amIFollowedBy](http://msdn.microsoft.com/library/3641c469-0063-054d-355d-e56697cb08ae%28Office.15%29.aspx) 方法。
  
    
    


## 设置通过 SharePoint 2013 JavaScript 对象模型使用关注好友功能的开发环境的先决条件
<a name="bk_Prereqs"> </a>

若要创建使用 JavaScript 对象模型以使用关注好友功能的场解决方案，您将需要：
  
    
    

- 配置有"我的网站"以及为当前用户和目标用户创建有用户配置文件和个人网站的 SharePoint Server 2013
    
  
- Visual Studio 2008
    
  
- Visual Studio 2012 Office 开发人员工具
    
  
- 登录用户 User Profile service 应用程序的"完全控制"访问权限
    
  
- 登录用户的本地管理员权限
    
  

## 在 Visual Studio 2008 中创建一个场解决方案和应用程序页面
<a name="bk_CreateSolution"> </a>


1. 以管理员身份运行 Visual Studio，依次选择"文件"、"新建"、"项目"。
    
  
2. 在"新建项目" 对话框中，对话框中，从对话框的顶部下拉列表中选择".NET Framework 4.5"。
    
  
3. 在"模版"列表中，展开"Office/SharePoint"，选择"SharePoint 解决方案"，然后选择"SharePoint 2013 - 空项目"模板。
    
  
4. 将项目命名为 FollowPeopleJSOM，然后选择"确定"按钮。
    
  
5. 在"SharePoint 自定义向导"对话框中，选择"部署场解决方案"，然后选择"完成"按钮。
    
  
6. 在"解决方案资源管理器"中，打开"FollowPeopleJSOM"项目的快捷菜单，然后添加"SharePoint 的'Layouts'映射文件夹"。
    
  
7. 在"Layouts"文件夹中，打开"FollowPeopleJSOM"文件夹的快捷菜单，然后添加新的名为 FollowPeople.aspx 的 SharePoint 应用程序页。
    
    > **注释**
      > 本文的代码示例定义了页面标记中的自定义代码，但是没有使用 Visual Studio 为该页面创建的代码隐藏类。 
8. 打开 FollowPeople.aspx 页面的快捷菜单，然后选择 **Set as Startup Item**。
    
  
9. 在 FollowPeople.aspx 文件的标记中，在"主" **asp:Content** 标签之间粘贴以下代码。此代码定义控件和脚本引用。
    
  ```HTML
  
<span id="followResults"></span><br/><br />
<button id="sendRequest" type="button"></button><br/>
<span id="message" style="color: #FF0000;"></span>

<script src="https://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js" type="text/javascript"></script>

<SharePoint:ScriptLink name="SP.js" runat="server" ondemand="false" localizable="false" loadafterui="true" />
<SharePoint:ScriptLink name="SP.UserProfiles.js" runat="server" ondemand="false" localizable="false" loadafterui="true" />
<SharePoint:FormDigest id="FormDigest" runat="server"/>
<script type="text/javascript">

    // Replace this comment with the code for your scenario.

</script>
  ```


> **注释**
> "获取粉丝和被关注人"的示例中没有使用按钮控件或窗体摘要控件，它们仅在更新服务器内容的操作时才需要使用。窗体摘要生成用于安全验证的消息摘要。 
10. 将 **script** 标签间的注释替换为以下其中一个方案的代码示例：
    
  -  [开始或停止关注好友](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_FollowPeople)
    
  
  -  [获取粉丝和被关注人](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_GetFollowers)
    
  
11. 要测试解决方案，请在菜单栏上，依次选择"调试"、"开始调试"。
    
  

## 代码示例：使用 SharePoint 2013 JavaScript 对象模型开始或停止关注好友
<a name="bk_FollowPeople"> </a>

以下代码示例使当前用户开始关注或停止关注某目标用户。该代码示例演示如何：
  
    
    

- 使用  [isFollowed](http://msdn.microsoft.com/library/2c1f62e6-fb75-ad4d-c081-36408b418c21%28Office.15%29.aspx) 方法检查当前用户是否正在关注某目标用户。
    
  
- 使用  [getFollowedCount](http://msdn.microsoft.com/library/97b53b4f-481a-cf41-1854-8f3ff860b2bb%28Office.15%29.aspx) 方法获得当前用户所关注好友的个数。
    
  
- 使用  [follow](http://msdn.microsoft.com/library/40d14320-27ba-2941-b0e2-be3b5a407c89%28Office.15%29.aspx) 方法开始关注目标用户。
    
  
- 使用  [stopFollowing](http://msdn.microsoft.com/library/65b0e9be-dc5e-09fb-c57f-7a933de09a4c%28Office.15%29.aspx) 方法停止关注目标用户。
    
  

> **注释**
> 粘贴您在 [创建场解决方案和应用程序页](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_CreateSolution)过程中添加的 **script** 标记间的以下代码。然后在运行代码之前，请先更改 **targetUser** 变量的占位符值。
  
    
    


```

// Replace the placeholder value with the account name of the target user.
var targetUser = 'domain\\\\userName';

var clientContext;
var followingManager;
var actorInfo;
var isFollowed;
var followedCount;

// Ensure that the SP.UserProfiles.js file is loaded before running your code.
$(document).ready(function () {
    SP.SOD.executeOrDelayUntilScriptLoaded(getFollowingStatus, 'SP.UserProfiles.js');
});

// Get the Following status of the current user.
function getFollowingStatus() {

    // Get the current client context.
    clientContext = SP.ClientContext.get_current();

    // Get the SocialFeedManager instance.
    followingManager = new SP.Social.SocialFollowingManager(clientContext);

    // Create a SocialActorInfo object to represent the target user.
    actorInfo = new SP.Social.SocialActorInfo();
    actorInfo.set_accountName(targetUser);

    // Find out whether the current user is following the target user.
    isFollowed = followingManager.isFollowed(actorInfo);
    followedCount = followingManager.getFollowedCount(1);

    // Get the information from the server.
    clientContext.executeQueryAsync(showFollowingStatus, requestFailed)
}

// Show the Following status of the current user.
function showFollowingStatus() {
    var results = '';
    results += 'Is the current user following the target user? ' + isFollowed.get_value();
    results += '<br/>Total count of followed people: ' + followedCount.get_value();
    $('#followResults').html(results);

    // Initialize the button for this example.
    $('#sendRequest').click(
        function () {
            $('#message').empty();
            toggleFollowingStatus();
        });
    $('#sendRequest').text('Toggle following status');
}

// Follow or stop following the target user.
function toggleFollowingStatus() {
    if (isFollowed.get_value() === false) {
        followingManager.follow(actorInfo);
    }
    else if (isFollowed.get_value() === true) {
        followingManager.stopFollowing(actorInfo);
    }
    clientContext.executeQueryAsync(getFollowingStatus, requestFailed);
}

// Failure callback.
function requestFailed(sender, args) {
    $('#message').html('Error: ' + args.get_message());
}
```


## 代码示例：使用 SharePoint 2013 JavaScript 对象模型获取粉丝和被关注人
<a name="bk_GetFollowers"> </a>

以下代码示例获取当前用户关注的好友，并获取关注当前用户的好友。它演示了如何执行以下操作：
  
    
    

- 使用  [getFollowed](http://msdn.microsoft.com/library/432a7cec-6add-fdb1-a79f-a93414ee8cd3%28Office.15%29.aspx) 方法获取当前用户正在关注的好友。
    
  
- 使用  [getFollowers](http://msdn.microsoft.com/library/ae4a944b-c043-05fb-c74b-101d2ce4a813%28Office.15%29.aspx) 方法并传递用来表示 **User** 角色类型的 **1**，从而获取正在关注当前用户的人员。
    
  
- 循环访问一组好友并获得每个人的显示名称、个人网站 URI 和照片 URI。
    
  

> **注释**
> 粘贴您在 [创建场解决方案和应用程序页](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_CreateSolution)过程中添加的 **script** 标记间的以下代码。
  
    
    


```

var followed;
var followers;

// Ensure that the SP.UserProfiles.js file is loaded before running your code.
$(document).ready(function () {
    SP.SOD.executeOrDelayUntilScriptLoaded(getFollowedAndFollowers, 'SP.UserProfiles.js');

    // Hide the button for this example.
    $('#sendRequest').hide();
});

// Get the Following status of the current user.
function getFollowedAndFollowers() {

    // Get the current client context.
    var clientContext = SP.ClientContext.get_current();

    // Get the SocialFeedManager instance.
    var followingManager = new SP.Social.SocialFollowingManager(clientContext);

    // Get followed people and followers.
    followers = followingManager.getFollowers();
    followed = followingManager.getFollowed(1);

    // Send the request to the server.
    clientContext.executeQueryAsync(showFollowedAndFollowers, requestFailed)
}

// Show the Following status of the current user.
function showFollowedAndFollowers() {
    var results = 'The current user is following ' + followed.length + ' people: <br/>';

    for (var i = 0; i < followed.length; i++) {
        var user = followed[i];
        var name = user.get_name();
        var personalSiteUri = user.get_personalSiteUri();
        var pictureUri = user.get_imageUri();

        results += '<br/>' + name + '<br/>' + personalSiteUri + '<br/>' + pictureUri + '<br/>';
    }

    results += '<br/>The current user is followed by ' + followers.length + ' people: <br/>';

    for (var i = 0; i < followers.length; i++) {
        var user = followers[i];
        var name = user.get_name();
        var personalSiteUri = user.get_personalSiteUri();
        var pictureUri = user.get_imageUri();

        results += '<br/>' + name + '<br/>' + personalSiteUri + '<br/>' + pictureUri + '<br/>';
    }
    $('#followResults').html(results);
}

// Failure callback.
function requestFailed(sender, args) {
    $('#message').html('Error: ' + args.get_message());
}
```


## 其他资源
<a name="bk_AdditionalResources"> </a>


-  [通过 SharePoint 2013 跟踪人员](follow-people-in-sharepoint-2013.md)
    
  
-  [如何：使用 SharePoint 2013 中的 .NET 客户端对象模型关注好友](how-to-follow-people-by-using-the-net-client-object-model-in-sharepoint-2013.md)
    
  

