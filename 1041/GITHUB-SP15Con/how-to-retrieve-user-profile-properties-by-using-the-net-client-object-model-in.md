---
title: [方法] SharePoint 2013 の .NET クライアント オブジェクト モデルを使用してユーザー プロファイル プロパティを取得する
ms.prod: SHAREPOINT
ms.assetid: 236ebaf8-f92e-4192-9b51-0a9de0210885
---


# [方法] SharePoint 2013 の .NET クライアント オブジェクト モデルを使用してユーザー プロファイル プロパティを取得する
SharePoint 2013の .NET クライアント オブジェクト モデルを使用して、ユーザー プロファイル プロパティをプログラムによって取得する方法を説明します。
## SharePoint 2013のユーザー プロファイル プロパティとは
<a name="bkmk_WhatIs"> </a>

ユーザー プロパティとユーザー プロファイル プロパティは、表示名、電子メール、肩書き、その他のビジネス情報や個人情報など、SharePoint ユーザーに関する情報を提供します。クライアント側の API では、 [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) オブジェクトとその [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) プロパティから、これらのプロパティにアクセスします。 [UserProfileProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.UserProfileProperties.aspx) プロパティにはすべてのユーザー プロファイル プロパティが含まれていますが、 [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) オブジェクトには一般的に使われるプロパティ ( [AccountName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.AccountName.aspx) 、 [DisplayName](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.DisplayName.aspx) 、 [Email](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.Email.aspx) など) が含まれていて、より簡単にアクセスできます。)
  
    
    
 [PeopleManager](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.aspx) オブジェクトには, .NET クライアント オブジェクト モデルを使用してユーザー プロパティとユーザー プロファイル プロパティの取得に使用できる、次のメソッドが含まれています。
  
    
    

-  [GetMyProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetMyProperties.aspx) メソッドと [GetPropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetPropertiesFor.aspx) メソッドは [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) オブジェクトを返します。
    
  
-  [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) メソッドと [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) メソッドは、指定したユーザー プロファイル プロパティの値を返します。
    
  
各種クライアント API のユーザー プロファイル プロパティは、読み取り専用です (ただし、 [PeopleManager.SetMyProfilePicture](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.SetMyProfilePicture.aspx) メソッドを使用して変更できるプロファイル画像は除きます)。その他のユーザー プロファイル プロパティを変更する場合は、サーバー オブジェクト モデルを使用する必要があります。
  
    
    

> **メモ**
> クライアント バージョンの  [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) オブジェクトには、サーバー側のバージョンにあるすべてのユーザー プロパティが含まれてはいません。ただし、クライアント バージョンには、現在のユーザーの個人用サイトの作成に使用できるメソッドが用意されています。現在のユーザーのクライアント側の [UserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfile.aspx) を取得するには、 [ProfileLoader.GetUserProfile](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.ProfileLoader.GetUserProfile.aspx) メソッドを使用します。
  
    
    

プロファイルの操作の詳細については、「 [SharePoint 2013 のユーザー プロファイルの操作](work-with-user-profiles-in-sharepoint-2013.md)」を参照してください。
  
    
    

## SharePoint 2013の .NET クライアント オブジェクト モデルを使用してユーザー プロファイル プロパティを取得できる開発環境をセットアップするための前提条件
<a name="bk_Prereqs"> </a>

.NET クライアント オブジェクト モデルを使用してユーザー プロファイル プロパティを取得するコンソール アプリケーションを作成するには、以下のものが必要です。
  
    
    

- 現在のユーザーと対象ユーザー向けにプロファイルが作成されている SharePoint Server 2013
    
  
- Visual Studio 2012
    
  
- 現在のユーザーの User Profile Service アプリケーションにアクセスできる [ **フル コントロール**] の接続権限
    
  

> **メモ**
> 開発作業をしているコンピューターで SharePoint Server 2013 が実行されていない場合は、SharePoint 2013 のクライアント アセンブリが含まれている  [SharePoint クライアント コンポーネント](http://www.microsoft.com/en-us/download/details.aspx?id=35585)のダウンロードを入手してください。 
  
    
    


## SharePoint 2013の .NET クライアント オブジェクト モデルを使用してユーザー プロファイル プロパティを取得するコンソール アプリケーションを作成する
<a name="bk_RetrieveProps"> </a>


1. 開発用のコンピューターで、Visual Studio を開き、[ **ファイル**]、[ **新規作成**]、[ **プロジェクト**] の順に選択します。
    
  
2. [ **新しいプロジェクト**] ダイアログ ボックスで、上部のドロップダウン リストから [ **.NET Framework 4.5**] を選択します。
    
  
3. プロジェクト テンプレートから [ **Windows**]、[ **コンソール アプリケーション**] の順に選択します。
    
  
4. プロジェクトの名前を UserProfilesCSOM とし、[ **OK**] をクリックします。
    
  
5. 次のアセンブリへの参照を追加します。
    
  - **Microsoft.SharePoint.Client**
    
  
  - **Microsoft.SharePoint.ClientRuntime**
    
  
  - **Microsoft.SharePoint.Client.UserProfiles**
    
  
6. **Main** メソッドでは、サーバー URL と対象ユーザー名の各変数を定義しています (次のコードを参照)。
    
  ```cs
  
const string serverUrl = "http://serverName/";
const string targetUser = "domainName\\\\userName";
  ```


> **メモ**
> コードを実行する前に、 `http://serverName/` および `domainName\\\\userName` プレースホルダーの値を必ず置き換えてください。
7. SharePoint クライアントのコンテキストを初期化します (次のコードを参照)。
    
  ```cs
  
ClientContext clientContext = new ClientContext(serverUrl);

  ```

8. 対象ユーザーのプロパティを **PeopleManager** オブジェクトから取得します。
    
  ```cs
  
PeopleManager peopleManager = new PeopleManager(clientContext);
PersonProperties personProperties = peopleManager.GetPropertiesFor(targetUser);
  ```


**personProperties** オブジェクトは、クライアント オブジェクトです。一部のクライアント オブジェクトには、初期化されるまでデータが一切含まれていません。たとえば、 **personProperties** オブジェクトのプロパティ値は、初期化されるまでアクセスできません。初期化される前にプロパティへのアクセスを試みると、 **PropertyOrFieldNotInitializedException** という例外が発生します。
    
  
9. **personProperties** オブジェクトを初期化するには、実行するリクエストを登録し、そのリクエストをサーバー上で実行します (次のコードを参照)。
    
  ```cs
  
clientContext.Load(personProperties, p => p.AccountName, p => p.UserProfileProperties);
clientContext.ExecuteQuery();
  ```


**Load** メソッド (または **LoadQuery** メソッド) の呼び出し時には、取得または変更するオブジェクトを渡します。この例では、 **Load** メソッドの呼び出しにより、リクエストをフィルターするためのオプション パラメーターが渡されています。このパラメーター群は、 **personProperties** オブジェクトの **AccountName** プロパティと **UserProfileProperties** プロパティのみを要求するラムダ式です。
    
> **ヒント**
> ネットワーク トラフィックを削減するには、 **Load** メソッドの呼び出し時に、操作するプロパティのみを要求します。また、複数のオブジェクトを操作する場合は、可能であれば **Load** メソッドの複数回の呼び出しをまとめたうえで **ExecuteQuery** メソッドを呼び出します。
10. ユーザー プロファイル プロパティに対して反復処理を行い、各プロパティの名前と値を読み取ります (次のコードを参照)。
    
  ```cs
  
foreach (var property in personProperties.UserProfileProperties)
{
    Console.WriteLine(string.Format("{0}: {1}", 
        property.Key.ToString(), property.Value.ToString()));
}

  ```


## コード例: SharePoint 2013の .NET クライアント オブジェクト モデルを使用したすべてのユーザー プロファイル プロパティの取得
<a name="SP15UserProf_codeexample1"> </a>

次のコード例では、 [前の手順](how-to-retrieve-user-profile-properties-by-using-the-net-client-object-model-in.md#bk_RetrieveProps)で説明した、対象ユーザーのすべてのユーザー プロファイル プロパティを取得して反復処理を行う方法を示します。
  
    
    

> **メモ**
> コードを実行する前に、 `http://serverName/` および `domainName\\\\userName` プレースホルダーの値を置き換えてください。
  
    
    


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


## コード例: SharePoint 2013 .NET クライアント オブジェクト モデルを使用した、自分をフォローしている人のユーザー プロファイルのプロパティの取得
<a name="SP15UserProfilescodeInApp"> </a>

次のコード例は、SharePoint アドイン で、自分をフォローしている人のユーザー プロファイルのプロパティを取得する方法を示しています。
  
    
    

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


## コード例: SharePoint 2013の .NET クライアント オブジェクト モデルを使用した一連のユーザー プロファイル プロパティの取得
<a name="SP15UserProfilescode2"> </a>

次のコード例は、対象ユーザーの特定のユーザー プロファイル プロパティ群を取得する方法を示しています。
  
    
    

> **メモ**
> 1 つのユーザー プロファイル プロパティのみの値を取得するには、 [GetUserProfilePropertyFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertyFor.aspx) メソッドを使用します。
  
    
    

対象ユーザーの  [PersonProperties](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PersonProperties.aspx) オブジェクトを取得していた先ほどのコード例とは異なり、この例では [PeopleManager.GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) メソッドを呼び出し、取得する対象ユーザーとユーザー プロファイル プロパティが指定された [UserProfilePropertiesForUser](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.UserProfilePropertiesForUser.aspx) オブジェクトを渡しています。 [GetUserProfilePropertiesFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.PeopleManager.GetUserProfilePropertiesFor.aspx) は、指定されたプロパティの値が含まれている **IEnumerable<string>** コレクションを返します。
  
    
    

> **メモ**
> コードを実行する前に、 `http://serverName/` および `domainName\\\\userName` プレースホルダーの値を置き換えてください。
  
    
    




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


## その他の技術情報
<a name="bk_addresources"> </a>


-  [SharePoint 2013 のユーザー プロファイルの操作](work-with-user-profiles-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 で JavaScript オブジェクトモデルを使用して、ユーザー プロファイル プロパティを取得する方法](how-to-retrieve-user-profile-properties-by-using-the-javascript-object-model-in.md)
    
  
-  [[方法] SharePoint 2013 でサーバー オブジェクト モデルを使用して、ユーザー プロファイルと組織プロファイルを操作する](how-to-work-with-user-profiles-and-organization-profiles-by-using-the-server-obj.md)
    
  
-  [Microsoft.SharePoint.Client.UserProfiles](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.UserProfiles.aspx)
    
  

