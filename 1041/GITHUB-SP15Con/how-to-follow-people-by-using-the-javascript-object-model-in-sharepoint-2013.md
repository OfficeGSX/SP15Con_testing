---
title: SharePoint 2013 で JavaScript オブジェクト モデルを使用してユーザーをフォローする方法
ms.prod: SHAREPOINT
ms.assetid: 2643c286-47c9-4a7a-9273-7474394477d6
---


# SharePoint 2013 で JavaScript オブジェクト モデルを使用してユーザーをフォローする方法
SharePoint Server 2013 JavaScript オブジェクト モデルを使用してユーザー フォロー機能を操作する方法について説明します。
## SharePoint Server 2013でユーザー フォロー機能を使用する理由
<a name="bk_FollowingPeopleFeatures"> </a>

SharePoint Server 2013 では、ユーザー フォロー機能により、ユーザー同士のつながりを容易に維持することができます。たとえば、ユーザーが他のユーザーをフォローすると、そのユーザーのニュースフィードにフォロー対象ユーザーの投稿とアクティビティが表示されます。ユーザー フォロー機能を使用して関心のある人たちに焦点を当てることで、アプリまたはソリューションの適合度 (関連性) を高めることができます。JavaScript オブジェクト モデルでは、フォロー対象ユーザーが  [SocialActor](http://msdn.microsoft.com/library/4e369fd5-b9b0-9804-957e-b3e39c559cd4%28Office.15%29.aspx) オブジェクトによって表されます。JavaScript オブジェクト モデルで、中心的なユーザー フォロー タスクを実行するには、 [SocialFollowingManager](http://msdn.microsoft.com/library/9ee1c0c0-b864-f0c3-f0cb-4dd4f1870dfa%28Office.15%29.aspx) オブジェクトを使用します。この記事では、JavaScript オブジェクト モデルを使用して、ユーザー フォロー機能を操作する方法を示します。
  
    
    

> **メモ**
>  [SocialFollowingManager](http://msdn.microsoft.com/library/9ee1c0c0-b864-f0c3-f0cb-4dd4f1870dfa%28Office.15%29.aspx) は、ユーザーとコンテンツのフォローに使用が推奨される API です。ただし、 [PeopleManager](http://msdn.microsoft.com/library/985fd2df-0e31-6ece-b846-ba2ccb156d00%28Office.15%29.aspx) オブジェクトには、 [amIFollowedBy](http://msdn.microsoft.com/library/3641c469-0063-054d-355d-e56697cb08ae%28Office.15%29.aspx) メソッドや他のユーザーのフォロー状態を取得するメソッドなど、ユーザーをフォローするための追加的な機能も含まれます。
  
    
    


## SharePoint 2013 JavaScript オブジェクト モデルを使用してユーザー フォロー機能を操作するための開発環境をセットアップするための前提条件
<a name="bk_Prereqs"> </a>

JavaScript オブジェクト モデルを使用してユーザー フォロー機能を操作するファーム ソリューションを作成するには、次のものが必要になります。
  
    
    

- 個人用サイトが構成され、現在のユーザーおよび対象ユーザーについてユーザー プロファイルおよび個人用サイトが作成されている SharePoint Server 2013
    
  
- Visual Studio 2012
    
  
- Office Developer Tools for Visual Studio 2012
    
  
- ログオン ユーザー用に、User Profile Service アプリケーションの [ **フル コントロール**] アクセス許可
    
  
- ログオン ユーザー用のローカル管理アクセス許可
    
  

## Visual Studio 2012 でファーム ソリューションおよびアプリケーション ページを作成する
<a name="bk_CreateSolution"> </a>


1. Visual Studio を管理者として実行し、[ **ファイル**]、[ **新規作成**]、[ **プロジェクト**] の順に選択します。
    
  
2. [ **新しいプロジェクト**] ダイアログ ボックスの上部にあるドロップダウン リストから [ **.NET Framework 4.5**] を選択します。
    
  
3. [ **テンプレート**] リストの [ **Office/SharePoint**] を展開し、[ **SharePoint ソリューション**] を選択して、[ **SharePoint 2013 - 空のプロジェクト**] テンプレートを選択します。
    
  
4. プロジェクトの名前を FollowPeopleJSOM にして、[ **OK**] ボタンをクリックします。
    
  
5. [ **SharePoint カスタマイズ ウィザード**] ダイアログ ボックスで、[ **ファーム ソリューションとして配置する**] を選択して、[ **完了**] をクリックします。
    
  
6. [ **ソリューション エクスプローラー**] で、[ **FollowPeopleJSOM**] プロジェクトのショートカット メニューを開き、SharePoint のマップされた "Layouts" フォルダーを追加します。
    
  
7. [ **Layouts**] フォルダーで、[ **FollowPeopleJSOM**] フォルダーのショートカット メニューを開き、FollowPeople.aspx という名前の新しい SharePoint アプリケーション ページを追加します。
    
    > **メモ**
      > この記事のコード例では、ページ マークアップにカスタム コードを定義していますが、Visual Studio によって作成されるページの分離コードは使用しません。 
8. FollowPeople.aspx ページのショートカット メニューを開き、 **Set as Startup Item** を選択します。
    
  
9. FollowPeople.aspx ファイルのマークアップ内で、"Main" **asp:Content** タグ間に次のコードを貼り付けます。 このコードによって、コントロールおよびスクリプト参照が定義されます。
    
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


> **メモ**
> 「フォロワーとフォロー対象ユーザーを取得する」の例では、ボタン コントロールもフォーム ダイジェスト コントロールも使用していません。これらはサーバー コンテンツを更新する操作に必要なだけです。フォーム ダイジェストは、セキュリティ検証に使用されるメッセージ ダイジェストを生成します。 
10. **script** タグに挟まれたコメントを、以下のいずれかのシナリオのコード例で置き換えます。
    
  -  [ユーザーのフォローを開始または停止する](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_FollowPeople)
    
  
  -  [フォロワーとフォロー対象ユーザーを取得する](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_GetFollowers)
    
  
11. ソリューションをテストするには、[ **デバッグ**] メニューの [ **デバッグ開始**] をクリックします。
    
  

## コード例: SharePoint 2013 JavaScript オブジェクト モデルを使用してユーザーのフォローを開始または停止する
<a name="bk_FollowPeople"> </a>

次のコード例では、現在のユーザーに対象ユーザーのフォローを開始または停止させます。このコード例では、以下の方法を示しています。
  
    
    

-  [isFollowed](http://msdn.microsoft.com/library/2c1f62e6-fb75-ad4d-c081-36408b418c21%28Office.15%29.aspx) メソッドを使用して、現在のユーザーが対象ユーザーをフォローしているかどうかを確認します。
    
  
-  [getFollowedCount](http://msdn.microsoft.com/library/97b53b4f-481a-cf41-1854-8f3ff860b2bb%28Office.15%29.aspx) メソッドを使用して、現在のユーザーがフォローしている人数を取得します。
    
  
-  [follow](http://msdn.microsoft.com/library/40d14320-27ba-2941-b0e2-be3b5a407c89%28Office.15%29.aspx) メソッドを使用して、対象ユーザーのフォローを開始します。
    
  
-  [stopFollowing](http://msdn.microsoft.com/library/65b0e9be-dc5e-09fb-c57f-7a933de09a4c%28Office.15%29.aspx) メソッドを使用して、対象ユーザーのフォローを停止します。
    
  

> **メモ**
>  [ファーム ソリューションおよびアプリケーション ページを作成する](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_CreateSolution)」の手順で追加した **script** タグの間に以下のコードを貼り付けます。次に、コードを実行する前に、 **targetUser** 変数のプレースホルダーの値を変更します。
  
    
    


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


## コード例: SharePoint 2013 JavaScript オブジェクト モデルを使用してフォロワーとフォロー対象ユーザーを取得する
<a name="bk_GetFollowers"> </a>

次のコード例では、現在のユーザーがフォローしているユーザーを取得し、現在のユーザーによるフォロー対象のユーザーを取得します。 以下にその方法を説明します。
  
    
    

-  [getFollowed](http://msdn.microsoft.com/library/432a7cec-6add-fdb1-a79f-a93414ee8cd3%28Office.15%29.aspx) メソッドを使用して、現在のユーザーがフォローしているユーザーを取得します。
    
  
-  [getFollowers](http://msdn.microsoft.com/library/ae4a944b-c043-05fb-c74b-101d2ce4a813%28Office.15%29.aspx) メソッドを使用し、 **User** アクター タイプを表す **1** を渡して、現在のユーザーをフォローしているユーザーを取得します。
    
  
- ユーザーのグループを反復処理して、各ユーザーの表示名、個人サイト URI、および画像 URI を取得します。
    
  

> **メモ**
>  [ファーム ソリューションおよびアプリケーション ページを作成する](how-to-follow-people-by-using-the-javascript-object-model-in-sharepoint-2013.md#bk_CreateSolution)」の手順で追加した **script** タグの間に以下のコードを貼り付けます。
  
    
    


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


## その他の技術情報
<a name="bk_AdditionalResources"> </a>


-  [SharePoint 2013 で他のユーザーをフォローする](follow-people-in-sharepoint-2013.md)
    
  
-  [方法: SharePoint 2013 で .NET クライアント オブジェクト モデルを使用してユーザーをフォローする](how-to-follow-people-by-using-the-net-client-object-model-in-sharepoint-2013.md)
    
  

