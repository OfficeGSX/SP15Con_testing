---
title: SharePoint 2013 のクエリ絞り込み
ms.prod: SHAREPOINT
ms.assetid: ec31782e-1bc5-4dc3-8df7-c29cd5f7f05c
---



# SharePoint 2013 のクエリ絞り込み
検索クエリと検索結果を使用する際にプログラムで SharePoint Server 2013 クエリ絞り込み機能を使用する方法について説明します。
クエリ絞り込み機能を使用して、クエリに関連する絞り込みオプションをエンド ユーザーに提供することができます。この機能によりエンド ユーザーは、結果に対して算出された絞り込みデータを使用して検索結果を掘り下げることができます。絞り込みデータは、検索クエリの全結果の管理プロパティ統計の集計に基づき、インデックス コンポーネントによって計算されます。
  
    
    

一般に、クエリ絞り込みは、アイテムに表示される作成日、作成者、ファイルの種類など、インデックス付きアイテムに関連付けられたメタデータに使用されます。絞り込みオプションを使用すれば、クエリを絞り込んで、特定の期間に作成されたアイテムのみ表示したり、特定のファイルの種類のアイテムのみ表示したりすることができます。
## クエリ オブジェクト モデルで絞り込み条件を使用する
<a name="SP15_Using_refiners"> </a>

クエリ絞り込みに関係のあるクエリは次の 2 つです。
  
    
    

1. エンド ユーザーのクエリに [絞り込み条件仕様を追加する](#SP15_Adding_refiners)ことによって、検索結果で絞り込み条件セットが返されるように要求することができます。絞り込み条件仕様は  [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) プロパティへの入力です。このクエリは検索インデックスに対して実行されます。検索結果は、該当する結果と絞り込みデータから構成されます。
    
  
2.  [絞り込みクエリを作成する](#SP15_Creating_refined_query)ことによって、絞り込みデータを使用して検索結果を掘り下げることができます。最終的な検索結果が、エンド ユーザーの元のクエリ テキストと絞り込みデータの選択された絞り込みオプションの両方の要件を満たすように、クエリに  [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) プロパティを追加します。
    
  
以下のセクションでは、これらの手順について詳細に説明し、コード例を示します。
  
    
    

## 絞り込み条件仕様でエンド ユーザーのクエリに絞り込み条件を追加する
<a name="SP15_Adding_refiners"> </a>

要求されるクエリ絞り込み条件を指定するには、 [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.KeywordQuery.aspx) クラスの [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) プロパティを使用します。次の構文を使用して、要求されるクエリ絞り込み条件を指定します。
  
    
    
 `<refiner>[,<refiner>]*`
  
    
    

  
    
    
各  `refiner` の形式は次のとおりです。
  
    
    
 `<refiner-name>[(parameter=value[,parameter=value]*)]?`
  
    
    

  
    
    
詳細は次のとおりです。
  
    
    

-  `<refiner-name>` は、絞り込み条件に関連付けられている管理プロパティの名前です。この管理プロパティは、検索スキーマで [Refinable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Refinable.aspx) または [Sortable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Sortable.aspx) に設定する必要があります。
    
  
- オプションの  `parameter=value` のペアのリストは、名前付き絞り込み条件の既定ではない構成値を指定します。かっこ内に絞り込み条件のパラメーターがリストされていない場合は、検索スキーマ構成によって既定の設定が指定されます。表 1 に、 `parameter=value` のペアの設定可能な値を示します。
    
  

> **メモ**
> 絞り込み条件を指定する場合の最小要件は、管理プロパティの  `refiner-name` を指定することです。<br/> **Example**<br/> `Refiners = "FileType"`<br/> <br/> また、高度な構文を使用して絞り込み条件の設定を調整することもできます。 <br/> **Example**<br/> `Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22),companies"`
  
    
    


**表 1: 絞り込み条件のパラメーター一覧**


|**パラメーター**|**説明**|
|:-----|:-----|
| _deephits_ <br/> |絞り込みの計算で根拠として使用されるヒットの既定の数をオーバーライドします。絞り込み条件が生成されると、クエリのすべての結果が検証されます。通常、このパラメーターを使用すると検索パフォーマンスが向上します。  <br/> **Syntax**          `deephits=<integer value>` <br/> **Example**          `price(deephits=1000)` <br/><br/>**メモ**<br/> この制限は、各インデックス パーティション内で適用されます。検証されるヒットの実際の数は、複数の検索パーティションでの収集により、この値より大きくなります。           |
| _discretize_ <br/> | 数字の絞り込み条件で、ユーザー設定の間隔 (絞り込み bin) を指定します。 <br/> **Syntax**          `discretize=manual/<threshold>/<threshold>[/<threshold>]*` <br/> **Example**          `write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22)` <br/>  `<threshold>` 属性は、各絞り込み bin でしきい値を指定します。 <br/>  指定された最初のしきい値以下のすべてに対して 1 つの間隔、連続する各しきい値の間に 1 つの間隔、最後のしきい値以上のすべてに対して 1 つの間隔があります。 <br/> **DateTime** 型の絞り込み条件については、以下の ISO 8601 互換の形式に従ってしきい値を指定します。 <br/>  _YYYY-MM-DD_ <br/>  _YYYY-MM-DDThh:mm:ss_ <br/>  _YYYY-MM-DDThh:mm:ss:Z_ <br/>  この形式の値は、以下の内容を指定します。 <br/>  _YYYY_ - 4 桁の年。サポートされるのは、4 桁の年だけです。 <br/>  _MM_ - 2 桁の月。01 は 1 月です。 <br/>  _DD_ - 2 桁の日付 (01 ～ 31)。 <br/>  _T_ - 文字 "T"。 <br/>  _hh_ - 2 桁の時間 (00 ～ 23)。A.M. や P.M. の表示はできません。 <br/>  _mm_ - 2 桁の分 (00 ～ 59)。 <br/>  _ss_ - 2 桁の秒 (00 ～ 59)。 <br/> <br/> **重要**<br/>  すべての日付と時刻の値は、UTC (協定世界時) (GMT (グリニッジ標準時) とも呼ばれます) に従って指定する必要があります。UTC ゾーンの識別子 (末尾の "Z" 文字) はオプションです。          |
| _sort_ <br/> | 文字列絞り込み条件で、どのように bin を並べ替えるか定義します。 <br/> **Syntax**          `sort=<property>/<direction>` <br/>  この属性は以下の処理をします。 <br/>  _<property>_ - 並べ替えアルゴリズムを指定します。以下の小文字の値が有効です。 <br/> **frequency** - bin 内の出現回数で並べ替えます。 <br/> **name** - ラベル名で並べ替えます。 <br/> **number** - 文字列を数字として扱い、数字で並べ替えます。この値は、 **String** 型の管理プロパティに含まれる値を数字で並べ替えるときのように、不連続の値を指定するときに役立ちます。 <br/>  _<direction>_ - 並べ替えの方向を指定します。以下の小文字の値が有効です。 <br/> **descending** <br/> **ascending** <br/> **Example**          `sort=name/ascending` <br/> **Default**: frequency/descending  <br/> |
| _filter_ <br/> | クライアントに返される前に、 **String** 型の絞り込み条件内の bin が抽出される方法を定義します。 <br/> **Syntax**          `filter=<bins>/<freq>/<prefix>[<levels>]` <br/>  この属性は以下の処理をします。 <br/>  _<bins>_ - 返す bin の最大数を指定します。 <br/>  絞り込み条件で bin を並べ替えた後で、残りの bin を切り捨てるときにこの属性を使用します。たとえば、bins=10 の場合は、指定された並べ替えアルゴリズムに従って、最初の 10 の bin だけが返されます。 <br/>  _<freq>_ - 返す bin の数を制限します。 <br/>  少ない頻度数の bin を削除するには、この属性を使用します。たとえば、 _freq_=2 は、2 以上のメンバーを持つ bin だけが返されることを示します。  <br/>  _<prefix>_ - この接頭辞で始まる名前を持つ bin だけが返されることを指定します。 <br/>  ワイルドカードのアスタリスク文字 "*" は、すべての名前と一致します。 <br/> **Example** <br/>  `filter=30/2/*` <br/>  _<levels>_ - 分類のレベルを指定します。 <br/>  分類レベルに基づいて、階層的な絞り込み条件で抽出して出力するには、この属性を使用します。属性は、正の整数 _n_ である必要があります。 **default**値は  _n_=0 です。 _n_>0 の場合は、 _n_ 個未満の分類パス区切り記号 ("/") を含む絞り込み条件エントリのみが返されます。 _n_=0 の場合、レベル フィルター処理は行われません。レベル区切り文字はスラッシュ文字 ("/") です。  <br/>  大きな分類ナビゲーターを使用する場合は、パフォーマンスが下がることに注意してください。フィルター処理は、結果処理の一部として行われます。 <br/> <br/> **メモ**<br/>  このパラメーターは、アプリケーションに固有の、結果側フィルター処理に適用されます。これは、パフォーマンス最適化のために個別のインデックス パーティションで制限をする cutoff パラメーターとは異なります。          |
| _cutoff_ <br/> | 深い文字列絞り込み条件のために移動し処理される必要があるデータを制限します。この絞り込み条件は、もっとも関連性が高い値 (bin) のみを返すために設定することができます。 <br/> <br/> **メモ**<br/>  この切り捨てフィルター処理は、各インデックス パーティションで行われます。これは、結果側フィルター処理のみを実行する _filter_ パラメーターとは異なります。これら 2 つのパラメーターは組み合わせて使うことができます。          **Syntax**          `cutoff=<frequency>/<minbins>/<maxbins>` <br/>  この属性は以下の処理をします。 <br/>  _<maxbins>_ - bin の数を制限します。 <br/>  このパラメーターは、絞り込み条件のために返される一意的な値 (bin) の数を制限します。多数の bin を持つ文字列絞り込み条件が返される場合、これは検索パフォーマンスを向上する推奨の方法です。"0" は、検索スキーマで指定された既定値が使用されることを意味します。 <br/>  _<frequency>_ - 頻度で bin の数を制限します。 <br/>  結果セットの絞り込み条件値の出現回数が頻度値以下の場合は、絞り込み条件値は返されません。"0" は、検索スキーマに従った既定値が使用されることを意味します。 <br/>  _<minbins>_ - 頻度切り捨ての最小値を指定します。 <br/>  クエリの一意的な絞り込み条件値の数がこの値未満の場合は、頻度切り捨ては行われず、すべての絞り込み条件値がその検索パーティションから返されます。頻度での切り捨てが使用された場合、このパラメーターは出現回数にかかわらず返される、一意的な絞り込み条件値の最小数を指定するために使用することができます。この属性の既定値は "0" で、これは一意的なナビゲーター値の数にかかわらず、頻度による切り捨てが行われることを意味します。"0" は、インデックス スキーマで指定された既定値が使用されることを意味します。 <br/> <br/> **メモ**<br/>  _<frequency>_ と _<minbins>_ は注意して使用する必要があります。 _<maxbins>_ のみを使用することをお勧めします。          |
   

### 例: 絞り込み条件の追加
<a name="SP15_Example_adding_refiners"> </a>

次の CSOM の例は、3 つの絞り込み条件 ( **FileType**、 **Write**、 **Companies**) をプログラムで要求する方法を示しています。 **Write** は、アイテムの最終変更日を示し、高度な構文を使用して固定サイズの日付/時刻 bin を返します。
  
    
    

```cs

using (var context = new ClientContext("http://<serverName>/<siteCollectionPath>"))
{
    var query = new KeywordQuery(context)
    {
        QueryText = "home",
        Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/2013-09-
            15/2013-09-21/2013-09-22),companies"
    };
    
    var executor = new SearchExecutor(context);
    var results = executor.ExecuteQuery(query);

    context.ExecuteQuery();

    ResultTable relevantResultsTable = results.Value[0];
    ResultTable refinerResultsTable = results.Value[1];          
    Console.WriteLine(refinerResultsTable.RowCount + " refinement options:");
    foreach (var refinementOption in refinerResultsTable.ResultRows)
    {
        Console.WriteLine("RefinerName: '{0}' RefinementName: '{1}'
            RefinementValue: '{2}' RefinementToken: '{3}' RefinementCount: '{4}'", 
            refinementOption["RefinerName"],
            refinementOption["RefinementName"],
            refinementOption["RefinementValue"],
            refinementOption["RefinementToken"],
            refinementOption["RefinementCount"]
        );
    }
}
```


## 検索結果の絞り込みデータの概要
<a name="SP15_Understanding_refinement_data"> </a>

クエリ内の管理プロパティに対してクエリ絞り込みを有効にしている場合は、クエリ結果に、絞り込み bin に分割された絞り込みデータが含まれます。このデータは、 [ResultTableCollection](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ResultTableCollection.aspx) 内の **RefinementResults** テーブル ( [RefinementResults](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KnownTableTypes.RefinementResults.aspx) ) にあります。1 つの絞り込み bin は、管理プロパティに固有の値または値範囲を示します。 **RefinementResults** テーブルには、絞り込み bin ごとに 1 行と、表 2 に指定された列が含まれます。
  
    
    

**表 2: 絞り込み bin で返されるデータ**


|**パラメーター**|**説明**|
|:-----|:-----|
|**RefinerName** <br/> |クエリ絞り込み条件の名前。  <br/> |
|**RefinementName** <br/> |この絞り込み bin を表す文字列。この文字列は通常、検索結果ページで、ユーザーに絞り込みオプションを示すときに使用されます。  <br/> |
|**RefinementValue** <br/> |絞り込みを表す、実装に固有の書式設定がされた文字列。このデータはデバッグのために返されるもので、通常、クライアントは必要としません。  <br/> |
|**RefinementToken** <br/> |絞り込みクエリを実行するときに **RefinerName** と共に使用する絞り込み bin を表す文字列。 <br/> |
|**RefinementCount** <br/> |この絞り込み bin の結果カウント。このデータは、この絞り込み bin に含まれる特定の管理プロパティのための値を持つ検索結果での、アイテムの数 (複製を含む) を表します。  <br/> |
   

### 例: 絞り込みデータ
<a name="SP15_Example_refinement_data"> </a>

次の表 3 には、絞り込みデータの行が 2 行示されています。最初の行には、ファイルの種類が HTML である、インデックス付きアイテムの絞り込みデータが含まれています。2 番目の行には、最終変更時刻が 2013/09/21 から 2013/09/22 の間である、インデックス付きアイテムの絞り込みデータが含まれています。
  
    
    

**表 3: 絞り込みデータの形式と内容**


|**RefinerName**|**RefinementName**|**RefinementValue**|**RefinementToken**|**RefinementCount**|
|:-----|:-----|:-----|:-----|:-----|
|FileType  <br/> |Html  <br/> |Html  <br/> |"ǂǂ68746d6c"  <br/> |50553  <br/> |
|Write  <br/> |From 2013-09-21T00:00:00Z up to 2013-09-22T00:00:00Z  <br/> |From 2013-09-21T00:00:00Z up to 2013-09-22T00:00:00Z  <br/> |range(2013-09-21T00:00:00Z, 2013-09-22T00:00:00Z)  <br/> |37  <br/> |
   

## 絞り込みクエリを作成する
<a name="SP15_Creating_refined_query"> </a>

検索結果は、絞り込みオプションを文字列値または値の範囲の形で示します。各文字列値または数値の範囲は、絞り込み bin と呼ばれます。各絞り込み bin について、対応する **RefinementToken** 値があります。絞り込みオプションは、 **RefinerName** 値で提供される管理プロパティに関連付けられています。
  
    
    
 **RefinementToken** と **RefinerName** の 2 つの値を連結して _refinement filter_ 文字列を作成します。この文字列は、管理プロパティに絞り込み bin 内の値があるアイテムのみが含まれるように検索結果アイテムを限定するのに使用できるフィルターとなります。まとめると次のようになります。
  
    
    
 `refinement filter = <RefinerName>:<RefinementToken>`
  
    
    
 [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.aspx) クラスの [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) プロパティに絞り込みフィルターを追加することによって、絞り込みクエリに 1 つ以上の絞り込みフィルターを設定できます。複数の絞り込みフィルターにより、検索結果を複数レベルで掘り下げたり、複数値を持つプロパティに絞り込みを適用したりすることができます。たとえば、(それぞれ 1 つの絞り込み bin で表される) 2 人の作成者が含まれるアイテムにクエリを絞り込みます。しかし、作成者の 1 人のみが含まれるアイテムは除外します。
  
    
    

### 例 1: ファイルの種類が HTML の絞り込みクエリの作成

次の CSOM の例では、プログラムで絞り込みを実行し、検索結果を、ファイルの種類が HTML のアイテムに限定する方法を示しています。 [例: 絞り込みデータ](#SP15_Example_refinement_data) で説明したように、この絞り込みオプションに関連する絞り込みデータには、 _Filetype_ に設定された **RefinerName** と _"ǂǂ68746d6c"_ に設定された **RefinementToken** が含まれています。
  
    
    

```cs

using (var context = new ClientContext("http://<serverName>/<siteCollectionPath>"))
{
    var query = new KeywordQuery(context)
    {        
        QueryText = "home"
    };
    
    query.RefinementFilters.Add("FileType:\\"ǂǂ68746d6c\\"");
    var executor = new SearchExecutor(context);
    var results = executor.ExecuteQuery(query);

    context.ExecuteQuery();

    ResultTable relevantResultsTable = results.Value[0];
    var resultCount = 1;
    foreach (var relevantResult in relevantResultsTable.ResultRows)
    {
        Console.WriteLine("Relevant result number {0} has file type {1}.", 
            resultCount, relevantResult["FileType"]);
            resultCount++;
    }
}
```


### 例 2: 前に取得した絞り込みデータを使用した絞り込みクエリの作成

次の CSOM の例では、絞り込み条件仕様でクエリを実行し、続いて使用される絞り込みデータを作成して絞り込みを実行する方法を示しています。この例では、最初の絞り込みオプションを選択するエンド ユーザーのプロセスをシミュレートしています。
  
    
    

```cs

using (var context = new ClientContext("http://<serverName>/<siteCollectionPath>"))
{
    // Step 1: Run the query with refiner spec to provide refinement data in search result
    var query = new KeywordQuery(context)
    {
        QueryText = "home",
        Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/
2013-09-15/2013-09-21/2013-09-22),companies"
    };
    
    Console.WriteLine("Run query '{0}' with refiner spec '{1}'.", query.QueryText,
query.Refiners);
    var executor = new SearchExecutor(context);
    var results = executor.ExecuteQuery(query);
    context.ExecuteQuery();

    // The query has been run and we can now look at the refinement data, to view the
    // refinement options
    ResultTable relevantResultsTable = results.Value[0];
    ResultTable refinerResultsTable = results.Value[1];
    Console.WriteLine("Got back {0} refinement options in the result:",
        refinerResultsTable.RowCount);
    
    foreach (var refinementOption in refinerResultsTable.ResultRows)
    {
        Console.WriteLine("RefinerName: '{0}' RefinementName: '{1}'
            RefinementValue: '{2}' RefinementToken: '{3}' RefinementCount: '{4}'",
            refinementOption["RefinerName"],
            refinementOption["RefinementName"],
            refinementOption["RefinementValue"],
            refinementOption["RefinementToken"],
            refinementOption["RefinementCount"]
        );
    }

    // Step 2: Run the refined query with refinement filter to drill down into 
    // the search results. This example uses the first refinement option in the refinement
    // data, if available. This simulates an end user selecting this refinement option.
    var refinementOptionArray = refinerResultsTable.ResultRows.ToArray();

    if (refinementOptionArray.Length > 0)
    {                         
        var firstRefinementOption = refinementOptionArray[6];x
        // Construct the refinement filter by concatenation
        var refinementFilter = firstRefinementOption["RefinerName"] + ":" +
            firstRefinementOption["RefinementToken"];
        var refinedQuery = new KeywordQuery(context)
        {
            QueryText = query.QueryText
        };
        refinedQuery.RefinementFilters.Add(refinementFilter);
        refinedQuery.SelectProperties.Add("FileType");
        refinedQuery.SelectProperties.Add("Write");
        refinedQuery.SelectProperties.Add("Companies");
        Console.WriteLine("Run query '{0}' with refinement filter '{1}'",            
            refinedQuery.QueryText, refinementFilter);
        var refinedResults = executor.ExecuteQuery(refinedQuery);
        context.ExecuteQuery();
        ResultTable refinedRelevantResultsTable = refinedResults.Value[0];
        var resultCount = 1;
        foreach (var relevantResult in refinedRelevantResultsTable.ResultRows)
        {
            Console.WriteLine("Relevant result number {0} has FileType='{1}',
                Write='{2}', Companies='{3}'", 
                resultCount, 
                relevantResult["FileType"],
                relevantResult["Write"],
                relevantResult["Companies"]
            );
            resultCount++;
        }
    }
}
```


## その他の技術情報
<a name="SP15_Query_refinement_addresources"> </a>


-  [SharePoint Server 2013 で絞り込み Web パーツのプロパティを構成する](http://technet.microsoft.com/ja-jp/library/gg549985.aspx)
    
  
-  [SharePoint Server 2013 の検索スキーマの概要](http://technet.microsoft.com/ja-jp/library/jj219669%28office.15%29.aspx)
    
  

## 関連項目
<a name="SP15_Query_refinement_addresources"> </a>


#### その他の技術情報


  
    
    
 [インデックス スキーマ (FAST Search Server for SharePoint)](http://msdn.microsoft.com/library/3e1eed3f-8a6f-4911-9607-f1616e6a44f3%28Office.15%29.aspx)
  
    
    
 [Microsoft.Search.Query スキーマの Refiner 要素](http://msdn.microsoft.com/library/0512c17a-18e9-45e3-9061-9dba9cd45ef4%28Office.15%29.aspx)
