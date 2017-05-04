---
title: 方法 SharePoint Server の検索結果でカスタム セキュリティ トリマーを使用する
ms.prod: SHAREPOINT
ms.assetid: e1a8664e-fb43-45c2-83aa-9635fe1efc99
---


# 方法: SharePoint Server の検索結果でカスタム セキュリティ トリマーを使用する
このハウツーでは、Microsoft Visual Studio 2010 を使用して、SharePoint 2013 の検索のカスタム セキュリティ トリマーを実装 (作成、展開、および登録) する手順について説明します。
SharePoint 2013 の検索 は、検索結果のクエリ時セキュリティ トリミングを実行します。ただし、カスタム セキュリティ トリミングを実行するシナリオがある場合もあります。SharePoint 2013 の検索 は、 [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) 名前空間の [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) 、 [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) 、および [ISecurityTrimmer2](https://msdn.microsoft.com/library/Microsft.Office.Server.Search.Query.ISecurityTrimmer2.aspx) (非推奨) の各インターフェイスによってこれらのシナリオについてもサポートしています。
  
    
    

カスタム セキュリティ トリミングには、プリトリミングとポストトリミングの 2 種類があります。プリトリミングとは、セキュリティ情報を追加するようにクエリを書き換えてから検索インデックスに突き合わせる、クエリ前評価を示します。ポストトリミングとは、検索結果の余分な部分を取り除いてからユーザーに返す、クエリ後評価を示します。
パフォーマンスと全体的な正確性を重視する場合は、プリトリミングの使用をお勧めします。ポストトリミングはデータの絞り込みとヒット カウント インスタンスによる情報の漏れを防ぐことができます。
  
    
    

セキュリティ トリマー登録処理によって、カスタム セキュリティ トリマーの構成プロパティを指定することができます。
## 概要

SharePoint 2013 の検索のカスタム セキュリティ トリミングは 2 つのインターフェイスで構成され、検索結果のプリトリミングまたはポストトリミングを実行できます。このハウツーでは、両方のインターフェイスについて、独自のセキュリティ トリマーを作成して登録するために必要な手順を説明します。
  
    
    

## 前提条件
<a name="PreReq"> </a>

このハウツーを完了するには、開発環境に次のツールがインストールされている必要があります。
  
    
    

- Microsoft SharePoint 2013 の検索
    
  
- Microsoft Visual Studio 2010 または同等の Microsoft .NET Framework 互換の開発ツール
    
  

## カスタム セキュリティ トリマー プロジェクトを設定する
<a name="SetUp"> </a>

この手順では、カスタム セキュリティ トリマー プロジェクトを作成し、必要な参照をプロジェクトに追加します。
  
    
    

### カスタム セキュリティ トリマーのプロジェクトを作成するには


1. Visual Studio を開き、[ **ファイル**]、[ **新規作成**]、[ **プロジェクト**] の順にクリックします。
    
  
2. [ **プロジェクトの種類**] で、[ **C#**] の [ **SharePoint**] を選択します。
    
  
3. [ **テンプレート**] の [ **空の SharePoint プロジェクト**] を選択します。[ **名前**] フィールドに「 **CustomSecurityTrimmerSample**」と入力し、[ **OK**] をクリックします。
    
  
4. **SharePoint カスタマイズ ウィザード**で、[ **ファーム ソリューションとして配置する**] を選択します。[ **完了**] をクリックします。
    
  

### カスタム セキュリティ トリマー プロジェクトに参照を追加するには


1. [ **プロジェクト**] メニューの [ **参照の追加**] をクリックします。
    
  
2. [ **.NET**] タブで、以下のコンポーネント名を持つ参照を選択した後、[ **OK**] をクリックします。
    
  - **Microsoft Search component**
    
    [ **.NET**] タブには、コンポーネント名が [ **Microsoft Search component**] のエントリが 2 つ表示されます。パス列が \\ISAPI\\Microsoft.Office.Server.Search.dll であるエントリを選択します。このエントリが [ **参照の追加**] ダイアログ ボックスの [ **.NET**] タブにない場合は、Microsoft.Office.Server.Search.dl のパスを使用して [ **参照**] タブから参照を追加する必要があります。
    
  
  - **Microsoft.IdentityModel**
    
    [ **Microsoft.IdentityModel**] が [ **.NET**] タブにない場合は、以下のパスを使用して [ **参照**] タブから Microsoft.IdentityModel.dll への参照を追加する必要があります。
    
     `%ProgramFiles%\\Reference Assemblies\\Microsoft\\Windows Identity Foundation\\v4.0.`
    
  

## カスタム セキュリティ プリトリマーを作成する
<a name="CreatePreTrimmer"> </a>


### セキュリティ プリトリマー用のクラス ファイルを作成するには


1. [ **プロジェクト**] メニューの [ **新しい項目の追加**] をクリックします。
    
  
2. [ **インストールされているテンプレート**] の [ **Visual C# アイテム**] で、[ **コード**] をクリックし、[ **クラス**] をクリックします。
    
  
3. 「 **CustomSecurityPreTrimmer.cs**」と入力し、[ **追加**] をクリックします。
    
  

### カスタム セキュリティ プリトリマーのコードを記述する

カスタム セキュリティ トリマーは、 **ISecurityTrimmerPre** インターフェイスを実装する必要があります。以下のコード例は、このインターフェイスの基本的な実装を示しています。
  
    
    

### CustomSecurityPreTrimmer の既定のコードを変更するには


1. 次の **using** ディレクティブをクラスの先頭に追加します。
    
  ```cs
  
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Security.Principal;
using Microsoft.IdentityModel.Claims;
using Microsoft.Office.Server.Search.Query;
using Microsoft.Office.Server.Search.Administration;

  ```

2. 次のコードで示すように、 **CustomSecurityPreTrimmer** クラスがクラス宣言で [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) インターフェイスを実装することを指定します。
    
  ```cs
  
public class CustomSecurityPreTrimmer : ISecurityTrimmerPre
  ```


### ISecurityTrimmerPre インターフェイス メソッドを実装するには


1.  [Initialize()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.Initialize.aspx) メソッド宣言用の以下のコードを追加します。
    
  ```cs
  public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{

}
  ```


このサンプルの基本バージョンでは、 **Initialize** メソッドにコードが含まれていません。
    
  
2.  [AddAccess()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.AddAccess.aspx) メソッド宣言用の以下のコードを追加します。
    
  ```cs
  
public IEnumerable<Tuple<Claim, bool>> AddAccess(
                IDictionary<string, object> sessionProperties, IIdentity passedUserIdentity)
{
//AddAccess method implementation, see steps 3-5.
}
  ```

3. **AddAccess** メソッド実装の最初の部分では、 _passedUserIdentity_ を調べてユーザーを確認します。
    
  ```cs
  
if (passedUserIdentity == null)
{
   throw new ArgumentException("AddAccess method is called with invalid user identity
   parameter", "passedUserIdentity");
}
            
   String strUser = null;
   var claimsIdentity = (IClaimsIdentity)passedUserIdentity;
   if (claimsIdentity != null)
   {
      foreach (var claim in claimsIdentity.Claims)
      {
         if (claim == null) 
         {
            continue;
         }
         
         // strUser is "domain\\\\user" format when web app is in Claims Windows Mode
         if (SPClaimTypes.Equals(claim.ClaimType, SPClaimTypes.UserLogonName))
         {
            strUser = claim.Value;
            break;
         }

         // strUser2 is "S-1-5-21-2127521184-1604012920-1887927527-66602" when web app is   
            in Legacy Windows Mode
         // In this case we need to convert it into NT user format.
         if (SPClaimTypes.Equals(claim.ClaimType, ClaimTypes.PrimarySid))
         {
            strUser = claim.Value;
            SecurityIdentifier sid = new SecurityIdentifier(strUser);
            strUser = sid.Translate(typeof(NTAccount)).Value;
            break;
         }
      }
}            

  ```

4. 次のコードで示すように、リストを作成し、リストにクレームのデータを取り込み、リストを返します。
    
  ```cs
  
var claims = new LinkedList<Tuple<Claim, bool>>();
if (!string.IsNullOrEmpty(strUser))
   {
      var groupMembership = GetMembership(strUser);
      if (!string.IsNullOrEmpty(groupMembership))
      {
         var groups = groupMembership.Split(new[] {';'},
         StringSplitOptions.RemoveEmptyEntries);
         foreach (var group in groups)
         {
            claims.AddLast(new Tuple<Claim, bool>(
            new Claim("http://schemas.happy.bdc.microsoft.com/claims/acl", group), false));
         }
      }
   }
   return claims;
}

  ```


**GetMembership** メソッドには、トリマーのカスタム ロジックが含まれています。
    
  

## カスタム セキュリティ ポストトリマーを作成する
<a name="CreatePostTrimmer"> </a>


### セキュリティ ポストトリマー用のクラス ファイルを作成するには


1. [ **プロジェクト**] メニューの [ **新しい項目の追加**] をクリックします。
    
  
2. [ **インストールされているテンプレート**] の [ **Visual C# アイテム**] で、[ **コード**] をクリックし、[ **クラス**] をクリックします。
    
  
3. 「CustomSecurityPostTrimmer.cs」と入力し、[ **追加**] をクリックします。
    
  

### カスタム セキュリティ ポストトリマー コードを記述する

カスタム セキュリティ トリマーは、 **ISecurityTrimmerPost** インターフェイスを実装する必要があります。このセクションのコード例は、このインターフェイスの基本的な実装を示しています。
  
    
    

### CustomSecurityPostTrimmer の既定のコードを変更するには


1. 次の **using** ディレクティブをクラスの先頭に追加します。
    
  ```cs
  
using System;
using System.Collections;
using System.Collections.Generic;
using System.Collections.Specialized;
using System.Security.Principal;
using Microsoft.IdentityModel.Claims;
using Microsoft.Office.Server.Search.Query;
using Microsoft.Office.Server.Search.Administration;

  ```

2. **CustomSecurityPostTrimmer** クラスが [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) インターフェイスを実装するように、クラス宣言を以下のように指定します。
    
  ```cs
  
public class CustomSecurityPostTrimmer : ISecurityTrimmerPost
  ```


### ISecurityTrimmerPost インターフェイス メソッドを実装するには


1.  [Initialize()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.Initialize.aspx) メソッドの宣言に次のコードを追加します。
    
  ```cs
  public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{

}
  ```


このサンプルの基本バージョンでは、Initialize メソッドにコードは含まれていません。
    
  
2.  [CheckAccess()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.CheckAccess.aspx) メソッドの宣言に次のコードを追加します。
    
  ```cs
  
public BitArray CheckAccess(IList<string> documentCrawlUrls, IList<string> documentAcls, IDictionary<string, object> sessionProperties, IIdentity passedUserIdentity)
{
//CheckAccess method implementation, see steps 3-5.
}
  ```

3. **CheckAccess** メソッド実装の最初の部分では、次のコードで示すように、 **BitArray** 変数を宣言して初期化し、 **documentCrawlUrls** コレクション内の各 URL のアクセス チェックの結果を格納して、クエリを送信したユーザーを取得します。
    
  ```cs
  
if (documentCrawlUrls == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid URL list",
   "documentCrawlUrls");
}
if (documentAcls == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid documentAcls   
   list", "documentAcls");
}
if (passedUserIdentity == null)
{
   throw new ArgumentException("CheckAccess method is called with invalid user identity
   parameter", "passedUserIdentity");
}

  ```

4. 次のコードで示すように、コレクション内の各クロール URL をループし、アクセス チェックを実行して、クエリを送信したユーザーがクロール URL に関連付けられたコンテンツ アイテムにアクセスできるかどうかを判断します。 
    
  ```cs
  
// Initialize the bit array with TRUE value which means all results are visible by default.
var urlStatusArray = new BitArray(documentCrawlUrls.Count, true);
var claimsIdentity = (IClaimsIdentity)passedUserIdentity;
if (claimsIdentity != null)
{
   var userGroups = GetGroupList(claimsIdentity.Claims);
   var numberDocs = documentCrawlUrls.Count;
   for (var i = 0; i < numberDocs; ++i)
   {
      if (!string.IsNullOrEmpty(documentAcls[i]))
      {
         urlStatusArray[i] = VerifyAccess(documentAcls[i], userGroups);
      }
   }
}

  ```


次のように、ユーザーがコンテンツ アイテムにアクセスできる場合は、そのインデックス **urlStatusArray[i]** にある **BitArray** アイテムの値を **true** に設定します。それ以外の場合は、 **false** に設定します。
    
  
5. 次のコードで示すように、 **CheckAccess** メソッドの戻り値を **urlStatusArray** に設定します。
    
  ```cs
  
return urlStatusArray;
  ```


## カスタム セキュリティ トリマーを登録する
<a name="RegisterTrimmer"> </a>

この手順ではカスタム セキュリティ トリマーを構成する方法を示し、以下の作業について説明します。
  
    
    

- Windows PowerShell コマンドレットを使用して、検索サービス アプリケーションのカスタム セキュリティ トリマーを登録する。
    
  
- SharePoint 検索ホスト コントローラー (SPSearchHostController) を再起動する。
    
  

### カスタム セキュリティ トリマーを登録する

カスタム セキュリティ トリマーを  _ClassName_ で登録するには、SharePoint 管理シェル を使用します。このケースでは、 _ClassName_ は **CustomSecurityPreTrimmer** または **CustomSecurityPostTrimmer** のどちらかにできます。次の手順は、検索サービス アプリケーションの ID を 1 に設定してカスタム セキュリティ トリマーを登録する方法を示しています。
  
    
    

### カスタム セキュリティ トリマーを登録するには


1. エクスプローラーで、パス  _<Local_Drive>_:\\WINDOWS\\assemblyの CustomSecurityTrimmerSample.dll を見つけます。
    
  
2. ファイルのショートカット メニューを開き、[プロパティ] を選択します。
    
  
3. [ **プロパティ**] ダイアログ ボックスの [ **全般**] タブで、トークンを選択してコピーします。
    
  
4. SharePoint 管理シェル を開きます。このツールの使用に関する詳細については、「 [SharePoint 2010 管理シェルを使用してサービス アプリケーションを管理する](http://msdn.microsoft.com/library/aff64855-7377-4e4a-b3a9-b620c9047076%28Office.15%29.aspx)」を参照してください。
    
  
5. コマンド プロンプトに、以下のコマンドを入力します。
    
  ```
  New-SPEnterpriseSearchSecurityTrimmer -SearchApplication "Search Service Application"
-typeName "CustomSecurityTrimmerSample.ClassName, CustomSecurityTrimmerSample, 
Version=1.0.0.0, Culture=neutral, PublicKeyToken=token" -RulePath "xmldoc://*"
  ```


コマンドで、 _ClassName_ を **CustomSecurityPreTrimmer** または **CustomSecurityPostTrimmer** のいずれかで置換し、 _token_ を CustomSecurityTrimmerSample.dll ファイル用の公開キー トークンで置換します。クロール ルール ( _"xmldoc://*"_) とすべてのポストトリマーを関連付ける必要がありますが、プリトリマーについては任意です。
    
> **メモ**
> 複数のフロントエンド Web サーバーがある場合は、ファーム内のすべてのフロントエンド Web サーバー上のグローバル アセンブリ キャッシュにセキュリティ トリマーを展開する必要があります。 
6. セキュリティ トリマーが登録されていることを次の PowerShell コマンドレットで確認してください。
    
  ```
  
$searchApp = Get-SPEnterpriseSearchServiceApplication
$searchApp | Get-SPEnterpriseSearchSecurityTrimmer
  ```


セキュリティ トリマーは結果に表示されます。
    
  

### SharePoint 検索ホスト コントローラーを再起動するには


- 次の PowerShell コマンドレットを入力して、検索サービスを再起動できます。
    
  ```
  
  net restart sphostcontrollerservice

  ```


## その他の技術情報
<a name="bk_sectrimmer_addlresources"> </a>


-  [SharePoint Server 2013 での検索のカスタム セキュリティ トリミング](custom-security-trimming-for-search-in-sharepoint-server-2013.md)
    
  
-  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx)
    
  
-  [Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx)
    
  
-  [Microsoft.Office.Server.Search.Query.PluggableAccessCheckException](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.PluggableAccessCheckException.aspx)
    
  

