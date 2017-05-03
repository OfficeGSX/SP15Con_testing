---
title: SharePoint Server 2013 での検索のカスタム セキュリティ トリミング
ms.prod: SHAREPOINT
ms.assetid: fbbf0cc4-e135-426a-9996-34eb954dbd5a
---



# SharePoint Server 2013 での検索のカスタム セキュリティ トリミング
 **ISecurityTrimmerPre** と **ISecurityTrimmerPost** という 2 種類のカスタム セキュリティ トリマー インターフェイスと、カスタム セキュリティ トリマーの作成に必要な手順について説明します。
SharePoint 2013 の検索では、クエリ処理時に、クエリを送信したユーザーの ID に基づいて検索結果のセキュリティ トリミングを実行します。その際、クロール コンポーネントから取得されたセキュリティ情報を使用します。
  
    
    

ただし、シナリオによっては、組み込みのセキュリティ トリミングによる結果では要件を十分に満たせないことがあります。そうしたシナリオでは、カスタム セキュリティ トリミングを実装する必要があります。SharePoint 2013 の検索では、 [Microsoft.Office.Server.Search.Query](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.aspx) 名前空間の [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx) インターフェイス、 [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx) インターフェイス、および [ISecurityTrimmer2](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmer2.aspx) インターフェイス (非推奨) によってカスタム セキュリティ トリミングをサポートしています。
 **注:** カスタム プリトリマーはクレームのみサポートしており、ACL の Windows SID はサポートしていません。SID クレームの種類のいずれかがカスタム プリトリマーから返された場合、インデックス内の結果の ACE は SID としてではなく、クレームとしてエンコードされます。したがって SID に基づく Windows ユーザー ID と一致しません。
  
    
    


## カスタム セキュリティ トリミング用のインターフェイスの実装
<a name="Implementing_the_interfaces"> </a>

 **ISecurityTrimmerPre** インターフェイスは、プリトリミング (クエリ前評価) を実行します。プリトリミングでは、セキュリティ情報を追加するようにクエリを書き換えてから検索インデックスと照合します。 **ISecurityTrimmerPost** インターフェイスは、ポストトリミング (クエリ後評価) を実行します。ポストトリミングでは、ユーザーに結果を渡す前に検索結果の余分な部分が取り除かれます。
  
    
    
パフォーマンスと全体的な正確性を重視する場合は、プリトリミングの使用をお勧めします。プリトリミングは、データの絞り込みとヒット カウント インスタンスによる情報の漏れを防ぐことができます。ポストトリマーは、クエリ フィルターではセキュリティ トリミングを正確に表現できない場合 (たとえば、正式な業務時間中のみなど、クエリを発行するユーザーの現地時間によってドキュメントをフィルターで除去する必要がある場合) に使用できます。
  
    
    

### ISecurityTrimmerPre インターフェイスの実装

検索結果のカスタム セキュリティ プリトリマーを作成するには、 **ISecurityTrimmerPre** インターフェイスを実装したコンポーネントを作成する必要があります。
  
    
    
 **ISecurityTrimmerPre** インターフェイスには、実装が必要な 2 つのメソッドとして、 [Initialize(NameValueCollection, SearchServiceApplication)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.Initialize.aspx) と [AddAccess(Boolean, Claims)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.AddAccess.aspx) が含まれています。
  
    
    

#### メソッドを初期化する

 **Initialize** メソッドは、セキュリティ プリトリマーがワーカー プロセスに読み込まれる際に実行され、そのワーカー プロセスがリサイクルされるまで再実行されることはありません。このメソッドには、次の 2 つのパラメーターを渡します。
  
    
    

-  _staticProperties_: Search Service アプリケーションへの登録時にセキュリティ トリマーに対して指定された構成プロパティが含まれている  [NameValueCollection](https://msdn.microsoft.com/library/System.Collections.Specialized.NameValueCollection.aspx) オブジェクト。
    
  
-  _searchApplication_: Search Service アプリケーションを表す  [SearchServiceApplication](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.aspx) オブジェクト。
    
  

#### AddAccess メソッド

 **AddAccess** メソッドは、クエリの評価前に、受け取ったクエリのそれぞれについて、プリトリマーごとに 1 回実行されます。
  
    
    
このメソッドには、次の 2 つのパラメーターを渡します。
  
    
    

-  _sessionProperties_: クエリのプロパティが含まれる **[T:System.Collections.Generic.IDictionary<String,Object>]** オブジェクト。
    
  
-  _userIdentity_: ユーザー ID が含まれる  [IIdentity](https://msdn.microsoft.com/library/System.Security.Principal.IIdentity.aspx) オブジェクト。
    
  

### ISecurityTrimmerPost インターフェイスの実装

検索結果のカスタム セキュリティ ポストトリマーを作成するには、 **ISecurityTrimmerPost** インターフェイスを実装したコンポーネントを作成する必要があります。
  
    
    
 **ISecurityTrimmerPost** インターフェイスには、実装が必要な 2 つのメソッドとして、 [Initialize(NameValueCollection, SearchServiceApplication)](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.Initialize.aspx) と **CheckAccess(IList<String>, IList<String>, IDictionary<String, Object>, IIdentity)** が含まれています。
  
    
    

#### メソッドを初期化する

 **Initialize** メソッドは、セキュリティ ポストトリマーがワーカー プロセスに読み込まれる際に実行され、そのワーカー プロセスがリサイクルされるまで再実行されることはありません。このメソッドには、次の 2 つのパラメーターを渡します。
  
    
    

-  _staticProperties_: Search Service アプリケーションへの登録時にセキュリティ トリマーに対して指定された構成プロパティが含まれている  [NameValueCollection](https://msdn.microsoft.com/library/System.Collections.Specialized.NameValueCollection.aspx) オブジェクト。
    
  
-  _searchApplication_: Search Service アプリケーションを表す  [SearchServiceApplication](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.aspx) オブジェクト。
    
  

#### CheckAccess メソッド

 **CheckAccess** メソッドは、クエリの評価後に、クエリ結果セットのそれぞれについて、ポストトリマーごとに 1 回実行されます。
  
    
    
このメソッドには、次の 4 つのパラメーターを渡します。
  
    
    

-  _documentUrls_: クロール ルールに一致する検索結果の各コンテンツ項目の URL が格納された  [IList<T>](http://msdn2.microsoft.com/JA-JP/library/5y536ey6) オブジェクト。
    
  
-  _documentAcls_: セキュリティ トリマーの実装によってアクセス権が決定される各コンテンツ項目のアイテム ACL が格納された  [IList<T>](http://msdn2.microsoft.com/JA-JP/library/5y536ey6) オブジェクト。
    
  
-  _sessionProperties_: 遷移プロパティ バッグが格納された  [IDictionary<TKey, TValue>](http://msdn2.microsoft.com/JA-JP/library/s4ys34ea) オブジェクト。
    
  
-  _userIdentity_: 実装者がユーザーの ID を取得できる  [IIdentity](https://msdn.microsoft.com/library/System.Security.Principal.IIdentity.aspx) オブジェクト。
    
  
 **CheckAccess** メソッドは、 **true** または **false** 値の配列を表す [BitArray](https://msdn.microsoft.com/library/System.Collections.BitArray.aspx) オブジェクトを返します。それぞれの値は、このメソッドの最初のパラメーターとして渡された **IList** オブジェクト内の各コンテンツ項目 URL に対するものです。クエリ処理コンポーネントは、これらの値を使用して、結果のセキュリティ ポストトリミングを実行します。該当する配列値が **true** のコンテンツ項目は、返される結果に含められます。配列値が **false** の項目は、削除されます。
  
    
    
 **CheckAccess** メソッドの実装時には、各項目に対して **true** または **false** のどちらを返すかを決定するための 2 つの情報として、クエリを送信したユーザーの ID とコンテンツ項目の URL を使用できます。また、コネクタから **CheckAccess** メソッドにカスタム ドキュメントの ACL 情報を渡すこともできます。
  
    
    

#### セキュリティ トリマーのユーザー ID の取得

ユーザーの ID は、次のコード例に示すように、スレッドの現在のプリンシパルにアクセスすることで取得できます。
  
    
    

```cs

IIdentity userIdentity = System.Threading.Thread.CurrentPrincipal.Identity;
```

次の名前空間ディレクティブを含める必要もあります。
  
    
    



```cs
using System.Security.Principal;
```

また、ユーザーの ID は、 **CheckAccess** メソッドの **passedUserIdentity** パラメーターから取得することもできます。
  
    
    

#### コネクタからセキュリティ トリマーへのドキュメント ACL の引き渡し

コネクタとは、その名のとおり、SharePoint 2013 と外部データをホストする外部システムとを接続するコミュニケーション ブリッジです。カスタム コネクタを操作する場合は、 **docaclmeta** ドキュメント プロパティを設定することで、ドキュメントの ACL 情報をポストトリマーに直接渡すことができます。構成されたコネクタとポストトリマーでフィールドの書式と解釈が同じである限り、自由にコネクタを使用してカスタム データを渡すことができます。
  
    
    
コネクタによって **docaclmeta** に格納された文字列は、カスタム セキュリティ トリマーの **CheckAccess** メソッドが呼び出されると、 _documentAcls_ パラメーターに表示されます。 **docacl** プロパティの通常のドキュメント ACL は、基本的なセキュリティによるトリミングによって処理され、カスタム セキュリティ トリマーには表示されません。同様に、 **docaclmeta** プロパティは、基本的なセキュリティによるトリミングに影響しません。
  
    
    

#### ポストトリマーとこのトリマーがセキュリティ トリマーの絞り込み条件カウントに与える影響

ポストトリマーに関する作業を行う際には、 **RelevantResults** と **RefinementResults** という 2 種類の結果テーブルが存在することに注意が必要です。ポストトリマーは、 **RelevantResults** にヒットした結果のみに適用されます。その結果、ポストトリミングされたヒットに関連する絞り込み条件が存在し、 **RefinementResults** のカウント数は **RelevantResults** に等しいかそれより大きくなる可能性があります。こうした挙動には、次の 2 つの方法で対処できます。
  
    
    

- 絞り込み条件によって情報の漏洩が起こらないように、既定の Web パーツの絞り込みパネルから機密性の高い絞り込み条件を除外する。
    
  
- **RefinementResults** カウントが **RelevantResults** カウントを上回った場合に **RefinementResults** の表示を適切に隠せるように、カスタム Web パーツを使用して、ポストトリマー使用時に結果または絞り込み条件を表示する。
    
  

### セキュリティ トリマーに関する個別の構成プロパティの取得

個々の構成プロパティにアクセスするには、セキュリティ ポストトリマーの登録時に指定されたプロパティ名を使用します。たとえば、次のコードは **CheckLimit** という構成プロパティの値を取得します。
  
    
    

```cs
public void Initialize(NameValueCollection staticProperties, SearchServiceApplication searchApplication)
{
    if (staticProperties["CheckLimitProperty"] != null)
    {
         intCheckLimit = Convert.ToInt32(staticProperties["CheckLimitProperty"]);
    }
}
```


## カスタム セキュリティ トリマー コンポーネントの展開
<a name="Deploying_the_trimmer"> </a>

作成したカスタム セキュリティ トリマーは、クエリの役割にあるいずれかのサーバーのグローバル アセンブリ キャッシュに展開する必要があります。カスタム セキュリティ トリマーをグローバル アセンブリ キャッシュに展開する処理については、「 [方法: SharePoint Server の検索結果でカスタム セキュリティ トリマーを使用する](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)」の手順 2 を参照してください。
  
    
    

## カスタム セキュリティ トリマーの登録
<a name="Registering_the_trimmer"> </a>

ポストトリマーの場合は、カスタム セキュリティ トリマー登録を特定の Search Service アプリケーションやクロール ルールと関連付ける必要があります。プリトリマーの場合、この操作は省略できます。
  
    
    
カスタム セキュリティ トリマーを登録するには、SharePoint 管理シェルの **SPEnterpriseSearchSecurityTrimmer** コマンドレットを使用します。
  
    
    
次の表は、このコマンドレットで使用するパラメーターを説明したものです。
  
    
    

**表 1. SPEnterpriseSearchSecurityTrimmer コマンドレットで使用するパラメーター**


|**パラメーター**|**説明**|
|:-----|:-----|
| _SearchApplication_ <br/> |必須。Search Service アプリケーションの名前です (例: "Search Service Application")。  <br/> |
| _typeName_ <br/> |必須。カスタム セキュリティ トリマー アセンブリの厳密な名前です。  <br/> |
| _RulePath_ <br/> |ポストトリマーの場合は必須、プリトリマーの場合は省略可能。セキュリティ トリマーのクロール ルールです。  <br/> <br/> **メモ**<br/> コンテンツ ソースにつき 1 つのクロール ルールを使用することをお勧めします。           |
| _id_ <br/> |必須。セキュリティ トリマーの識別子 (ID) です。この値は一意です。別のセキュリティ トリマーで既に登録されている ID によってセキュリティ トリマーを登録した場合、最初のトリマーの登録は、2 番目のトリマーの登録によって上書きされます。  <br/> |
| _properties_ <br/> |省略可能。構成プロパティを指定する名前と値のペアです。 `Name1~Value1~Name2~Value~…` という形式になっている必要があります。 <br/> |
   
カスタム セキュリティ トリマーを登録するための基本的なコマンドの例と、構成プロパティを指定する例については、「 [方法: SharePoint Server の検索結果でカスタム セキュリティ トリマーを使用する](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)」を参照してください。
  
    
    

## その他の技術情報
<a name="bk_sectrimmer_addlresources"> </a>


-  [方法: SharePoint Server の検索結果でカスタム セキュリティ トリマーを使用する](how-to-use-a-custom-security-trimmer-for-sharepoint-server-search-results.md)
    
  
-  [ISecurityTrimmerPre](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPre.aspx)
    
  
-  [ISecurityTrimmerPost](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ISecurityTrimmerPost.aspx)
    
  
-  [SharePoint 2013 の Business Connectivity Services の概要](http://technet.microsoft.com/ja-jp/library/ee661740.aspx)
    
  
