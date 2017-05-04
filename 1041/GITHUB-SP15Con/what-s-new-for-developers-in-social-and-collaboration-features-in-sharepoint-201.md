---
title: 開発者向けの SharePoint 2013 のソーシャルおよびコラボレーション機能の新機能
ms.prod: SHAREPOINT
ms.assetid: 65365b1d-cde5-47cd-8b04-1b76be0e3490
---


# 開発者向けの SharePoint 2013 のソーシャルおよびコラボレーション機能の新機能
SharePoint 2013の個人用サイトおよびコミュニティ サイト開発シナリオ向けのソーシャル機能とコラボレーション機能に関して、新機能と更新された機能を確認します。
SharePoint 2013 のソーシャル機能とコラボレーション機能を使用すると、コミュニケーションや最新情報の把握を簡単に行えます。個人用サイトおよびチーム サイトの向上したソーシャル フィードでは、気になるユーザーやコンテンツに関して最新情報を常に把握できます。新しいコミュニティ サイト機能では、充実したコミュニティ体験を実感でき、情報を簡単に見つけ出して共有したり、同じ関心を持つユーザーを見つけることができます。
  
    
    

SharePoint 2013の新しいソーシャル機能とコラボレーション機能の詳細については、TechNet の「 [SharePoint Server 2013 でのソーシャル コンピューティングの新機能](http://technet.microsoft.com/ja-jp/library/jj219766%28office.15%29.aspx)」を参照してください。ソーシャル機能とコラボレーション機能のプログラミングの詳細については、「 [SharePoint 2013 のソーシャルおよびコラボレーション機能](social-and-collaboration-features-in-sharepoint-2013.md)」を参照してください。
## SharePoint Server 2013の個人用サイトの新機能と更新された機能
<a name="bkmk_Social"> </a>

ユーザー プロファイルとソーシャル データを含む個人用サイト ソーシャル API には、さまざまな新機能と更新された機能があります。個人用サイトおよびチーム サイトの新機能によってフィード内での双方向の対話式の操作が実現し、ユーザーは関心のある人々やコンテンツの最新情報を簡単に把握できます。
  
    
    
SharePoint Server 2013の [ **ニュースフィード**] ページには、ミニブログの投稿を簡単に公開できるテキストボックスや、フォローしているユーザーとコンテンツからの投稿と更新を配信する双方向対話式フィードなど、強化された機能の一部が表示されます。
  
    
    

### 新しい Social 名前空間で提供されるソーシャル フィードおよび以下のユーザーとコンテンツのための API

 **Social** 名前空間には、フィードとミニブログの投稿を操作したり、ユーザーやコンテンツをフォローするためのプライマリ API が含まれています。.NET クライアント オブジェクト モデルの詳細については「 [Microsoft.SharePoint.Client.Social](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.aspx) 」を、JavaScript オブジェクト モデルの詳細については「 [SP.Social](http://msdn.microsoft.com/library/43d47f01-c085-0e77-bd01-48bcb7d5bb35%28Office.15%29.aspx)」を、サーバー オブジェクト モデルの詳細については「 [Microsoft.Office.Server.Social](https://msdn.microsoft.com/library/Microsoft.Office.Server.Social.aspx) 」を、それぞれ参照してください。
  
    
    

> **メモ**
>  [Microsoft.Office.Server.ActivityFeed](https://msdn.microsoft.com/library/Microsoft.Office.Server.ActivityFeed.aspx) 名前空間の API は使用されなくなりました。「 [使用されなくなった (削除された) 個人用サイト ソーシャル API および機能](#bkmk_DeprecatedAPI)」を参照してください。 
  
    
    


### ソーシャル フィード、ユーザーおよびコンテンツのフォロー、およびユーザー プロパティ向けの SharePoint 2013の新しいクライアント API

SharePoint 2013には、ソーシャル フィードの操作、ユーザーおよびコンテンツのフォロー、オンライン、社内、モバイル開発でのユーザー プロパティの取得に使用できる新しいクライアント API が含まれています。可能な場合は、サーバー オブジェクト モデルまたは Web サービスの代わりに SharePoint 2013開発用クライアント API を使用することをお勧めします。クライアント API には、マネージ クライアント オブジェクト モデル、JavaScript オブジェクト モデル、Representational State Transfer (REST) サービスが含まれます。SharePoint アドインを開発する場合は、クライアント API を使用する必要があります。
  
    
    
Microsoft.Office.Server.UserProfiles アセンブリのサーバー側機能の中には、クライアント API からは使用できないものがあります。たとえば、 [Microsoft.Office.Server.Audience](https://msdn.microsoft.com/library/Microsoft.Office.Server.Audience.aspx) 名前空間、 [Microsoft.Office.Server.ReputationModel](https://msdn.microsoft.com/library/Microsoft.Office.Server.ReputationModel.aspx) 名前空間、または [Microsoft.Office.Server.SocialData](https://msdn.microsoft.com/library/Microsoft.Office.Server.SocialData.aspx) 名前空間の API に対してクライアント側のアクセスはありません。使用できる API については、 [Microsoft.SharePoint.Client.Social](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.aspx) 名前空間および [Microsoft.SharePoint.Client.UserProfiles](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.aspx) 名前空間を参照してください。
  
    
    
個人用サイト ソーシャル クライアント API へのアクセス方法については、「 [SharePoint 2013 でのソーシャル機能の開発の概要](get-started-developing-with-social-features-in-sharepoint-2013.md)」を参照してください。SharePoint 2013の API セットおよびその使用方法の詳細については、「 [SharePoint 2013 での適切な API セットの選択](choose-the-right-api-set-in-sharepoint-2013.md)」を参照してください。
  
    
    

### 複数ユーザー (SharePoint Online 上の 個人用サイト ホスト管理者のみ) 向けの個人サイトおよび OneDrive for Business をプロビジョニングするには、ProfileLoader.CreatePersonalSiteEnqueueBulk メソッドを使用します。
<a name="bk_CreatePersonalSiteEnqueueBulk"> </a>

個人用サイト ホスト管理者は **ProfileLoader.CreatePersonalSiteEnqueueBulk** メソッドを使用して、プログラムによって複数ユーザー向けの個人サイトを SharePoint Online 上にプロビジョニングできます。これには OneDrive for Business や [サイトの管理] ページなどの機能が含まれています。
  
    
    
次のコード例では、コンソール アプリケーションに .NET クライアント オブジェクト モデルを使用しています。この例を実行する前に、Microsoft.SharePoint.Client.dll、Microsoft.SharePoint.Client.Runtime.dll および Microsoft.SharePoint.Client.UserProfiles.dll への参照を追加し、 **userName**、 **passwordStr**、および **serverUrl** 変数のプレース ホルダー値を変更してください。 **serverUrl** 変数は、SharePoint Online 管理センターのURL でなければなりません。
  
    
    

> **メモ**
> 必要なクライアント DLL を入手するために、 [SharePoint Online Client Components SDK](http://www.microsoft.com/en-us/download/details.aspx?id=42038) をダウンロードしてください。
  
    
    




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

 **CreatePersonalSiteEnqueueBulk** メソッドを Windows PowerShell で使用するには、次のコマンドのプレースホルダー値 (SharePoint Online 管理センター の URL とユーザー名) を変更してから、コマンドを SharePoint 管理シェル で実行します。
  
    
    



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

詳しくは、「 [So you want to programmatically provision Personal Sites (One Drive for Business) in Office 365?](http://blogs.msdn.com/b/frank_marasco/archive/2014/03/25/so-you-want-to-programmatically-provision-personal-sites-one-drive-for-business-in-office-365.aspx)」および「 [Windows Powershell を使用して SharePoint 2013 を管理する](http://technet.microsoft.com/ja-jp/library/ee806878%28v=office.15%29.aspx)」を参照してください。
  
    
    

### SharePoint 2013の新しいユーザーおよびユーザー プロパティ向けオブジェクト
<a name="bkmk_NewUserObjects"> </a>

SharePoint Server 2013には次のように、ユーザーおよびユーザー プロパティを表す新しいオブジェクトが含まれます。
  
    
    

-  [SocialActor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialActor.aspx) オブジェクトは、フィードとその後のアクティビティの対象となるユーザー (およびその他のエンティティ) を表します。
    
  
-  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) オブジェクトには一般的なユーザー プロパティおよびユーザー プロファイルのプロパティが含まれます。
    
  

> **メモ**
> サーバー オブジェクト モデルのバージョンは、 [SPSocialActor](https://msdn.microsoft.com/library/Microsoft.Office.Server.Social.SPSocialActor.aspx) オブジェクトと、 [PersonProperties](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.PersonProperties.aspx) オブジェクトです。
  
    
    

SharePoint Server 2013 には新しいクライアント側  [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) オブジェクトも含まれています。このオブジェクトは現在のユーザーの個人用サイトを作成するときに使用できるメソッドを提供します。ただし、ここにはサーバー側 [UserProfile](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.UserProfile.aspx) オブジェクトに含まれるすべてのユーザー プロパティは含まれていません。クライアント側のコードからすべてのユーザー プロパティにアクセスするには、 [PeopleManager.GetMyProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetMyProperties.aspx) メソッドまたは [PeopleManager.GetPropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetPropertiesFor.aspx) メソッドを使用するか (ユーザー プロファイルのプロパティが [PersonProperties.UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) プロパティに格納されている場合)、 [PeopleManager.GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) メソッドまたは [PeopleManager.GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) メソッドを使用します。
  
    
    

### 新しいクライアント側ユーザー選択ウィンドウ コントロール
<a name="bkmk_NewUserObjects"> </a>

クライアント側ユーザー選択ウィンドウ コントロールは、ユーザー、グループ、およびクレームを選択するためのクロス ブラウザー サポートを提供する HTML および JavaScript コントロールです。コントロール固有のプロパティ (複数のユーザーまたはユーザーとグループを許可するなど) や Web アプリケーション レベルの構成設定 (Active Directory ドメイン サービスのパラメーターや、特定のフォレストの対象化など) を含めて、この選択ウィンドウ コントロールはサーバー側バージョンのコントロールと同じ設定を使用して構成できます。詳細については、「 [SharePoint 用の SharePoint ホスト型アドインでクライアント側ユーザー選択ウィンドウ コントロールを使用する](http://msdn.microsoft.com/library/383f265f-ed44-4d09-b2f6-366f13d52347%28Office.15%29.aspx)」を参照してください。
  
    
    

### 使用されなくなった (削除された) 個人用サイト ソーシャル API および機能
<a name="bkmk_DeprecatedAPI"> </a>

次の個人用サイト ソーシャル API および機能は SharePoint Server 2013で使用できなくなりました。
  
    
    

-  [Microsoft.Office.Server.ActivityFeed](https://msdn.microsoft.com/library/Microsoft.Office.Server.ActivityFeed.aspx) 名前空間の API は使用されなくなりました。 **Social** 名前空間は、SharePoint 2013でソーシャル フィードをプログラム的に操作するための API です。以前のバージョンとの互換性に関して、SharePoint 2010 の **ActivityEvent** アイテムは、返信できないイベントとして SharePoint 2013のフィードに表示されます (サーバーの全体管理でレガシ イベントの移行を有効にする必要があります)。
    
  
- 個人用サイト RSS フィード (ActivityFeed.aspx) は、REST サービス、クライアント オブジェクト モデル、および JavaScript オブジェクト モデルの新しい API に変更されました。この API (クライアント API が望ましい) を使用するカスタムの SharePoint 2010 コードを移行するには、ActivityFeed.aspx に対するすべての要求を、新しい API に対する呼び出しに変更し、JavaScript Object Notation (JSON) 形式で返されるフィード データを処理します。
    
  
- [ **最近のアクティビティ**] Web パーツは、新しく [ **ニュースフィード**] Web パーツに変更され、マルチスレッド式の対話とダイナミックなフィード取得がサポートされるようになりました。
    
    > **メモ**
    > ニュースフィード Web パーツまたは他のフィード Web パーツ (チーム サイトのサイト フィード Web パーツなど) のカスタマイズはサポートしません。これらの Web パーツをカスタマイズする場合 (たとえば、JavaScript のオーバーライドを使用することによって)、そのカスタマイズにより SharePoint の更新が中断される可能性があるため注意してください。 
- **ソーシャル コメント** Web パーツは推奨されなくなっています。
    
  
- 次のアクティビティ イベントは、フィードを自動的に通知しなくなりました (プロファイルの更新、近づいている誕生日および職場の記念日、新しいメンバーシップ、マネージャーの変更)。ただし、これらのアクティビティに対するカスタム イベント レシーバーを作成できます。新しく追加されたソーシャル イベントはありません。
    
  
-  [Privacy](https://msdn.microsoft.com/library/Microsoft.Office.Server.UserProfiles.Privacy.aspx) 列挙の次のフィールドは使用できなくなりました。 **Contacts**、 **Organization**、 **Manager**。SharePoint 2013で使用できるプライバシー設定は、 **Private** ([ **自分のみ**]) と **Public** ([ **すべてのユーザー**]) のみです。既存のプライバシー設定はユーザーが変更するまで保持されます。この API を使用するカスタムの SharePoint 2010 コードを移行するには、使用できなくなったプライバシー フィールドへの参照をすべて変更します。
    
  
- **SocialFollowingManager** からアクセスされる **Following People** API は、SharePoint Server 2010 の **仕事仲間**機能に代わるものです。[ **仕事仲間**] ページは [ **フォロー相手**] ページに変更されました。ユーザーが仲間をグループに分類できる **グループ**機能は提供されなくなりました。
    
  
- SharePoint 2013では組織プロファイルが廃止され、次の種類が使用できなくなりました。 **OrganizationProfile**、 **OrganizationProfileManager**、 **OrganizationMembershipType**、 **OrganizationNotFoundException**、 **OrganizationProfileChange**、 **OrganizationProfileChangeQuery**、 **OrganizationProfileMembershipChange**、および **OrganizationProfileValueCollection**。
    
  
- SharePoint 2013では、 **個人用リンク**機能は使用できなくなりました。
    
  

## SharePoint 2013の新しいコミュニティ サイト機能
<a name="bkmk_Collab"> </a>

新しいコミュニティ サイト機能として、新しいサイト テンプレートと強化されたディスカッション機能があります。コミュニティ メンバーは、評価、カテゴリ、注目のディスカッション、質問投稿タイプ、最適な返信などの機能を使用して、人気のあるディスカッションや関連情報、同じ関心を持つユーザーを簡単に見つけることができます。メンバーはコミュニティに参加する際に評価を作成します。
  
    
    
コミュニティ サイト機能では開発用の特定の API を公開しません。コミュニティ サイト機能を拡張するには、SharePoint サイトおよびリスト API を直接使用します。たとえば、SharePoint API を使用してサイトおよびリスト テンプレートをカスタマイズする、ソーシャル フィード用にコミュニティからのカスタム アクティビティを作成する、評価情報を検索結果に統合する、評価モデルをカスタマイズする、ディスカッションを主宰するワークフローを作成することができます。
  
    
    
次のリストには、コミュニティ サイト機能を使用した開発に関する情報が含まれています。
  
    
    

- コミュニティ サイトは **Community** サイト テンプレート ( [Id](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.WebTemplate.Id.aspx) = **62**) を使用します。このサイト テンプレートはパブリック Web サイトに使用できません。ディスカッション掲示板リストのテンプレートの種類は、 [DiscussionBoard](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.ListTemplateType.DiscussionBoard.aspx) (値 = **108**) です。
    
  
- [ **コミュニティ サイト**] 機能をアクティブ化すると、 **CommunityEventReceiver** イベント レシーバーがアクティブにされます。
    
  
- クライアント側に表示されるリスト ビューをカスタマイズするには、JavaScript オーバーライドを使用してビューを変更します。リスト ビューは SharePoint 2013 API を使用して拡張できません。詳細については、「 [クライアント側レンダリングを使用して SharePoint アドインのリスト ビューをカスタマイズする](http://msdn.microsoft.com/library/8d5cabb2-70d0-46a0-bfe0-9e21f8d67d86%28Office.15%29.aspx)」を参照してください。
    
  
- コミュニティ サイトでは非同期イベントを使用してオブジェクトを更新します。バックグラウンドで非同期イベントが実行されているときにリストおよびリストアイテムの更新を試みると、保存の競合が発生し、オブジェクト処理が無効になる場合があります。
    
回避方法として、次のコード例に示すように、 **Update** 呼び出しによって返される例外を処理し、呼び出しを再試行する前にインスタンスを更新して、複数回の再試行用にループ処理を作成します。
    


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


## その他の技術情報
<a name="SP15NewSocial_addlresources"> </a>


-  [SharePoint 2013 のソーシャルおよびコラボレーション機能](social-and-collaboration-features-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 でのソーシャル機能の開発の概要](get-started-developing-with-social-features-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の開発者のための新機能](what’s-new-for-developers-in-sharepoint-2013.md)
    
  
-  [SharePoint Server 2013 でのソーシャル コンピューティングの新機能](http://technet.microsoft.com/ja-jp/library/jj219766%28office.15%29.aspx)
    
  

