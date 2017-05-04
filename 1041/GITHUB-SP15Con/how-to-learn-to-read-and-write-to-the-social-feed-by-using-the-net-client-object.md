---
title: [方法] SharePoint 2013 の .NET クライアント オブジェクト モデルを使用してソーシャル フィードに対する読み書きを行う
ms.prod: SHAREPOINT
ms.assetid: 3c15ede5-8a59-47e6-a0b2-c17ec6bf4ae1
---


# [方法] SharePoint 2013 の .NET クライアント オブジェクト モデルを使用してソーシャル フィードに対する読み書きを行う
SharePoint 2013 .NET クライアント オブジェクト モデルを使用して、ソーシャル フィードに対する読み書きを行うコンソール アプリケーションを作成します。
## SharePoint 2013の .NET クライアント オブジェクト モデルを使用してソーシャル フィードに対する読み書きを行うコンソール アプリケーションを作成するための前提条件
<a name="bkmk_Prereqs"> </a>

ここで作成するコンソール アプリケーションは、対象ユーザーのフィードを取得し、番号付きリストにある各スレッドの最初の投稿を出力します。さらに、選択されているスレッドに対する簡単なテキスト返信を発行します。フィードへの投稿と返信は、どちらを発行する場合にも同じメソッド ( [CreatePost](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.CreatePost.aspx) ) を使用します。
  
    
    
このコンソール アプリケーションを作成するには、次のものが必要です。
  
    
    

- 個人用サイトが構成済みで、現在のユーザーと対象ユーザー向けに個人用サイトが作成され、対象ユーザーによっていくつかの投稿が作成されている SharePoint Server 2013
    
  
- Visual Studio 2012
    
  
- ログオン ユーザーの User Profile Service アプリケーションに対する **フル コントロール**のアクセス許可
    
  

> **メモ**
> 開発作業をしているコンピューターで SharePoint Server 2013 が実行されていない場合は、SharePoint 2013 のクライアント アセンブリが含まれている  [SharePoint クライアント コンポーネント](http://www.microsoft.com/en-us/download/details.aspx?id=35585)のダウンロードを入手してください。 
  
    
    


### SharePoint 2013のソーシャル フィードの操作に関する重要な概念
<a name="bkmk_CoreConcepts"> </a>

表 1 は、作業を開始する前に知っておく必要がある重要な概念を説明する記事へのリンクをまとめたものです。
  
    
    

**表 1. SharePoint 2013のソーシャル フィードの操作に関する重要な概念**


|**記事のタイトル**|**説明**|
|:-----|:-----|
| [SharePoint 2013 でのソーシャル機能の開発の概要](get-started-developing-with-social-features-in-sharepoint-2013.md) <br/> |のソーシャル フィードとマイクロブログの投稿を使用したプログラミングを開始し、ユーザーやコンテンツ (ドキュメント、サイト、タグ) のフォローや、ユーザー プロファイルの操作を行う方法を説明しています。  <br/> |
| [SharePoint 2013 でのソーシャル フィードの操作](work-with-social-feeds-in-sharepoint-2013.md) <br/> |ソーシャル フィードを操作するための一般的なプログラミング作業と、そうした作業の実行に使用する API について説明しています。  <br/> |
   

## Visual Studio 2012 でコンソール アプリケーションを作成してクライアント アセンブリへの参照を追加する
<a name="bkmk_CreateApp"> </a>


1. 開発用のコンピューターで、Visual Studio 2012 を開きます。
    
  
2. メニュー バーで、[ **ファイル**]、[ **新規作成**]、[ **プロジェクト**] の順に選択します。
    
  
3. [ **新しいプロジェクト**] ダイアログ ボックスで、上部のドロップダウン リストから [ **.NET Framework 4.5**] を選択します。
    
  
4. [ **テンプレート**] ボックスの一覧で、[ **Windows**] を選択し、[ **コンソール アプリケーション**] テンプレートを選択します。
    
  
5. プロジェクトの名前を ReadWriteMySite とし、[ **OK**] をクリックします。
    
  
6. 次のようにして、クライアント アセンブリへの参照を追加します。
    
1. **ソリューション エクスプローラー**で、 **ReadWriteMySite** プロジェクトのショートカット メニューを開き、[ **参照の追加**] を選択します。
    
  
2. [ **参照マネージャー**] ダイアログ ボックスで、次のアセンブリを選択します。
    
  - **Microsoft.SharePoint.Client**
    
  
  - **Microsoft.SharePoint.Client.Runtime**
    
  
  - **Microsoft.SharePoint.Client.UserProfiles**
    
  

    SharePoint Server 2013を実行しているコンピューター上で開発を行っている場合、上記のアセンブリは [ **拡張機能**] カテゴリ内にあります。それ以外の場合は、ダウンロードしたクライアント アセンブリの場所を参照します (「 [SharePoint クライアント コンポーネント](https://www.microsoft.com/en-us/download/details.aspx?id=35585)」を参照)。
    
  
7. Program.cs ファイルに、次の **using** ステートメントを追加します。
    
  ```cs
  
using Microsoft.SharePoint.Client;
using Microsoft.SharePoint.Client.Social;
  ```


## SharePoint 2013の .NET クライアント オブジェクト モデルを使用して対象ユーザーのソーシャル フィードを取得する
<a name="bkmk_RetrieveFeed"> </a>


1. サーバー URL と対象ユーザーのアカウント資格情報に関する変数を宣言します。
    
  ```cs
  
const string serverUrl = "http://serverName/";
const string targetUser = "domainName\\\\userName";
  ```


> **メモ**
> コードを実行する前に、 `http://serverName/` および `domainName\\\\userName` プレースホルダーの値を必ず置き換えてください。
2. **Main** メソッドで、SharePoint クライアント コンテキストを初期化します。
    
  ```cs
  
ClientContext clientContext = new ClientContext(serverUrl);

  ```

3.  [SocialFeedManager](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.aspx) インスタンスを作成します。
    
  ```cs
  
SocialFeedManager feedManager = new SocialFeedManager(clientContext);
  ```

4. 取得するフィード コンテンツのパラメーターを指定します。
    
  ```cs
  SocialFeedOptions feedOptions = new SocialFeedOptions();
feedOptions.MaxThreadCount = 10;
  ```


    既定のオプションでは、フィード内の最初の 20 スレッドが最終更新日で並び替えられた状態で返されます。
    
  
5. 対象ユーザーのフィードを取得します。
    
  ```cs
  
ClientResult<SocialFeed> feed = feedManager.GetFeedFor(targetUser, feedOptions);
clientContext.ExecuteQuery();
  ```


[GetFeedFor](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.GetFeedFor.aspx) は、 **ClientResult<T>** オブジェクトを返します。スレッドのコレクションは、このオブジェクトの [Value](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.ClientResult`1.Value.aspx) プロパティに格納されています。
    
  

## SharePoint 2013の .NET クライアント オブジェクト モデルを使用して反復処理によってソーシャル フィードからの読み取りを行う
<a name="bkmk_ReadFeed"> </a>

次のコードは、フィード内のスレッドに対して反復処理を行います。各スレッドが  [CanReply](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialThreadAttributes.CanReply.aspx) 属性を持つかどうかを確認したうえで、スレッド識別子と最初の投稿のテキストを取得します。また、(スレッドへの返信に使用される) スレッド識別子を格納するための辞書を作成し、最初の投稿のテキストをコンソールに書き込みます。
  
    
    

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


## SharePoint 2013の .NET クライアント オブジェクト モデルを使用してソーシャル フィードへの返信を投稿する
<a name="bkmk_WriteFeed"> </a>


1. (UI 関連のみ) 返信先のスレッドを取得し、ユーザーに返信内容の入力を求めます。
    
  ```cs
  
Console.Write("Which post number do you want to reply to?  ");
string threadToReplyTo = "";
int threadNumber = int.Parse(Console.ReadLine()) - 1;
idDictionary.TryGetValue(threadNumber, out threadToReplyTo);
Console.Write("Type your reply:  ");
  ```

2. 返信の定義を行います。次のコードは、コンソール アプリケーションから返信テキストを取得します。
    
  ```cs
  
SocialPostCreationData postCreationData = new SocialPostCreationData();
postCreationData.ContentText = Console.ReadLine();
  ```

3. 返信を発行します。 _threadToReplyTo_ パラメーターは、スレッドの [Id](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialThread.Id.aspx) プロパティを表しています。
    
  ```cs
  
feedManager.CreatePost(threadToReplyTo, postCreationData);
clientContext.ExecuteQuery();
  ```


> **メモ**
>  [CreatePost](https://msdn.microsoft.com/library/Microsoft.SharePoint.Client.Social.SocialFeedManager.CreatePost.aspx) メソッドは、最初のパラメーターに **null** を渡すことで、現在のユーザーのフィードに対する最初の投稿の発行にも使用されます。
4. (UI 関連のみ) プログラムを終了します。
    
  ```cs
  
Console.WriteLine("Your reply was published.");
Console.ReadKey(false);
  ```

5. コンソール アプリケーションをテストするには、メニュー バーの [ **デバッグ**] を選択し、[ **デバッグ開始**] をクリックします。
    
  

## コード例: SharePoint 2013の .NET クライアント オブジェクト モデルを使用してフィードの取得と投稿への返信を行う
<a name="bkmk_CodeExample"> </a>

以下に、Program.cs ファイルの完全なコード例を示します。
  
    
    

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


## 次の手順
<a name="SP15ReadWriteSocial_nextsteps"> </a>

.NET クライアント オブジェクト モデルを使用したソーシャル フィードに関する読み書きのタスクの実行方法については、以下を参照してください。
  
    
    

-  [[方法] SharePoint 2013 で .NET クライアント オブジェクト モデルを使用して投稿の作成と削除、およびソーシャル フィードの取得を行う](how-to-create-and-delete-posts-and-retrieve-the-social-feed-by-using-the-net-cli.md)
    
  

## その他の技術情報
<a name="SP15ReadWriteSocial_addlresources"> </a>


-  [SharePoint 2013 でのソーシャル機能の開発の概要](get-started-developing-with-social-features-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 でのソーシャル フィードの操作](work-with-social-feeds-in-sharepoint-2013.md)
    
  

