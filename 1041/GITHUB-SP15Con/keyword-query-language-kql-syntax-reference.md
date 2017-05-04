---
title: キーワード クエリ言語 (KQL) 構文のリファレンス
ms.prod: SHAREPOINT
ms.assetid: d8489f59-522f-433c-b9c1-69e597be51c7
---



# キーワード クエリ言語 (KQL) 構文のリファレンス
SharePoint 2013 の検索 の KQL クエリの構築について説明します。この構文リファレンスは KQL クエリ要素と、KQL クエリのプロパティ制限および演算子の使用方法を説明します。
## KQL クエリの要素
<a name="SP15KQL_elements"> </a>

KQL クエリは、次の 1 つ以上の要素で構成されます。 
  
    
    

- 自由形式のキーワード (単語または語句) 
    
  
- プロパティ制限 
    
  
KQL クエリ要素と 1 つ以上の使用可能な演算子を組み合わせることができます。
  
    
    
KQL クエリに演算子しかないか、または KQL クエリが空の場合、そのクエリは有効ではありません。KQL クエリは大文字と小文字が区別されませんが、演算子は区別されます (大文字)。
  
    
    

> **メモ**
> KQL クエリの長さ制限は、その作成方法によって異なります。既定の SharePoint 検索フロント エンドを使用して KQL クエリを作成した場合、長さは 2,048 文字に制限されます。ただし、クエリ オブジェクト モデルを使用して、プログラムによって作成する KQL クエリの既定の長さ制限は、4,096 文字になります。この制限は、 [MaxKeywordQueryTextLength](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.MaxKeywordQueryTextLength.aspx) プロパティまたは [DiscoveryMaxKeywordQueryTextLength](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.DiscoveryMaxKeywordQueryTextLength.aspx) プロパティを使用して、20,480 文字まで増やすことができます (電子情報開示の場合)。
  
    
    


## KQL を使用した自由形式のクエリの構築
<a name="SP15KQL_constructing_freetext_queries"> </a>

自由形式の式を使用して KQL クエリを構築すると、SharePoint 2013 の検索 が、フルテキスト インデックスに格納されている用語に基づいて、クエリに対して選択した用語と結果を照合します。これには、 [FullTextQueriable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.FullTextQueriable.aspx) が **true** に設定されている管理プロパティ値が含まれます。
  
    
    
自由形式の KQL クエリは、大文字小文字の区別がありませんが、演算子は大文字である必要があります。以下の 1 つ以上の項目を自由形式の式として使用し、KQL クエリを構築できます。
  
    
    

- **word** (空白または句読点のない 1 つ以上の文字を含みます)
    
  
- **phrase** (空白で区切られた 2 つ以上の単語を一緒に含みます。ただし単語は二重引用符で囲まれている必要があります)
    
  
複雑なクエリを構築する場合は、複数の自由形式の式を KQL クエリの演算子と組み合わせることができます。式の間に演算子が指定されていない自由形式の式が複数ある場合、クエリ動作は **AND** 演算子を使用した場合と同じになります。
  
    
    

### 自由形式の KQL クエリでの単語の使用

自由形式の KQL クエリで単語を使用すると、SharePoint 2013 の検索は、その単語と、フルテキスト インデックスに格納された用語との完全一致する結果を返します。ワイルドカード演算子 (*) を使用して前方一致を有効にすることで、単語の一部だけを単語の最初から使用することができます。前方一致では、SharePoint 2013 の検索は、ゼロ以上の文字が続く単語を含む用語と結果を照合します。
  
    
    
たとえば、以下の KQL クエリでは用語 "federated" および "search" を含むコンテンツ項目を返します。 
  
    
    
 `federated search`
  
    
    
 `federat* search`
  
    
    
 `search fed*`
  
    
    
KQL クエリでは後方一致をサポートしていません。
  
    
    

### 自由形式の KQL クエリでの語句の使用

自由形式の KQL クエリで語句を使用すると、SharePoint 2013 の検索は、指定された語句内の単語が隣合っている項目のみを返します。KQL クエリで語句を指定するには、二重引用符を使用する必要があります。 
  
    
    
KQL クエリは後方一致をサポートしないため、自由形式のクエリの語句の前にワイルドカード演算子を使用することはできません。ただし、語句の後にはワイルドカード演算子を使用することができます。
  
    
    

## KQL でのプロパティ制限クエリ
<a name="kql_property_restriction_queries"> </a>

KQL を使用すると、プロパティ制限を使用するクエリを作成し、指定した条件に基づく結果のみを照合するようにクエリの焦点を絞り込むことができます。
  
    
    

### プロパティ制限の指定

基本的なプロパティ制限は以下から構成されます。
  
    
    
 `<Property Name><Property Operator><Property Value>`
  
    
    
表 1 に、KQL クエリでの有効なプロパティ制限構文の例をいくつか示します。
  
    
    

**表 1. 有効なプロパティ制限構文**


|**構文**|**返される値**|
|:-----|:-----|
| `author:"John Smith"` <br/> |John Smith によって作成されたコンテンツ項目を返します。  <br/> |
| `filetype:docx` <br/> |Microsoft Word ドキュメントを返します。  <br/> |
| `filename:budget.xlsx` <br/> |ファイル名が  `budget.xlsx` のコンテンツ項目を返します。 <br/> |
   
プロパティ制限では、プロパティ名、プロパティ演算子、プロパティ値の間に空白を含めることはできません。空白が含まれていると、プロパティ制限は自由形式のクエリとして処理されます。プロパティ制限の長さは、2,048 文字までです。 
  
    
    
以下の例では、空白が含まれているために、クエリは、John Smith が作成したコンテンツ項目ではなく、用語 "author" と "John Smith" が含まれているコンテンツ項目を返します。
  
    
    
 `author: "John Smith"`
  
    
    
 `author :"John Smith"`
  
    
    
 `author : "John Smith"`
  
    
    
つまり、前述のプロパティ制限は、次と同等のものになります。
  
    
    
 `author "John Smith"`
  
    
    

### プロパティ制限のプロパティ名の指定

プロパティ制限に、有効な管理プロパティ名を指定する必要があります。SharePoint 2013 の検索には、既定で、ドキュメントの管理プロパティがいくつか含まれています。
  
    
    
クロールされたプロパティの値にプロパティ制限を指定するには、まずクロールされたプロパティを管理プロパティにマップする必要があります。「 [エンド ユーザーの検索操作性を計画する (Office SharePoint Server)](http://technet.microsoft.com/ja-jp/library/cc263089%28office.12%29.aspx)」の「 **管理プロパティおよびクロールされたプロパティ** 」を参照してください。
  
    
    
管理プロパティは、ドキュメントで検索できるように、 [Queryable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Queryable.aspx) である必要があります。さらに管理プロパティは取得するために [Retrievable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Retrievable.aspx) にすることもできます。ただし、管理プロパティはプロパティ検索を実行するために [Retrievable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Retrievable.aspx) である必要はありません。
  
    
    

### プロパティ制限でサポートされているプロパティ演算子

SharePoint 2013 の検索 では、表 2 に示すように、プロパティ制限用のいくつかのプロパティ演算子をサポートしています。 
  
    
    

**表 2. プロパティ制限の有効なプロパティ演算子**


|**演算子**|**説明**|**サポートされている管理プロパティの型**|
|:-----|:-----|:-----|
|:  <br/> |プロパティ制限で指定された値がプロパティ ストア データベースに格納されているプロパティ値と等しい結果を返すか、フルテキスト インデックスに格納されているプロパティ値の各用語を照合します。  <br/> | [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|=  <br/> |プロパティ値がプロパティ制限で指定された値と等しい検索結果を返します。  <br/> <br/> **メモ**<br/> 完全一致検索を実行する場合、 **=** 演算子とアスタリスク ( *****) を組み合わせて使用することはお勧めしません。           | [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|<  <br/> |プロパティ値がプロパティ制限で指定された値より小さい検索結果を返します。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|>  <br/> |プロパティ値がプロパティ制限で指定された値より大きい検索結果を返します。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|<=  <br/> |プロパティ値がプロパティ制限で指定された値以下の検索結果を返します。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|>=  <br/> |プロパティ値がプロパティ制限で指定された値以上の検索結果を返します。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|<>  <br/> |プロパティ値がプロパティ制限で指定された値と等しくない検索結果を返します。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|..  <br/> |プロパティ値がプロパティ制限で指定された範囲内にある検索結果を返します。  <br/> たとえば、範囲 A..B は A から B までの一連の値を表します。日付の範囲の場合、これは A 日の開始から、B 日の終了を意味します。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
   

### プロパティ値の指定

管理プロパティの型には、有効なデータ型であるプロパティ値を指定する必要があります。表 3 に、このような型のマッピングを示します。
  
    
    

**表 3. 管理プロパティの型の有効なデータ型のマッピング**


|**管理プロパティの型**|**データ型**|
|:-----|:-----|
| [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/> | [String](https://msdn.microsoft.com/library/System.String.aspx) <br/> |
| [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/> | [Int64](https://msdn.microsoft.com/library/System.Int64.aspx) <br/> |
| [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> | [System.Double](https://msdn.microsoft.com/library/System.Double.aspx) <br/> |
| [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/> | [Decimal](https://msdn.microsoft.com/library/System.Decimal.aspx) <br/> |
| [DateTime()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/> | [DateTime](https://msdn.microsoft.com/library/System.DateTime.aspx) <br/> |
| [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> | [Boolean](https://msdn.microsoft.com/library/System.Boolean.aspx) <br/> |
   

#### テキスト プロパティ値

テキスト プロパティ値の場合、プロパティが格納されているのがフルテキスト インデックスか検索インデックスかによって、照合の動作が異なります。 
  
    
    

#### フルテキスト インデックス内のプロパティ値

 **FullTextQueriable** プロパティが、管理プロパティで **true** に設定されている場合は、プロパティ値はフルテキスト インデックスに格納されます。これは文字列プロパティに対してのみ設定できます。クエリに指定されたプロパティ値は、フルテキスト インデックスに格納されている個々の用語と照合されます。プロパティ値全体と照合するかどうかを指定するには、 [NoWordBreaker](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.NoWordBreaker.aspx) プロパティを使用します。
  
    
    
たとえば、Paul Shakespear が作成したコンテンツ項目を検索している場合、以下の KQL クエリが一致した結果を返します。
  
    
    
 `author:Shakespear`
  
    
    
 `author:Paul`
  
    
    
前方一致もサポートされています。ワイルドカード演算子 (*) を使用することもできますが、個々の単語を指定する場合は不要です。前述の例を続けると、次の KQL クエリは Paul Shakespear が作成したコンテンツ項目を一致したものとして返します。 
  
    
    
 `author:Shakesp*`
  
    
    
プロパティ値に語句を指定する場合、一致した結果にはフルテキスト インデックスに格納されているプロパティ値内に指定された語句が含まれている必要があります。以下のクエリ例では、"Advanced Search XML"、"Learning About the Advanced Search Web Part" など、タイトルにテキスト "Advanced Search" があるコンテンツ項目を返します。
  
    
    
 `title:"Advanced Search"`
  
    
    
プロパティ値で指定された語句では先頭一致もサポートされていますが、クエリでワイルドカード演算子 (*) を使用する必要があり、ワイルドカード演算子は、以下のように、語句の最後でのみサポートされています。
  
    
    
 `title:"Advanced Sear*"`
  
    
    
以下のクエリでは予想した結果は返されません。
  
    
    
 `title:"Advan* Search"`
  
    
    
 `title:"Advanced Sear"`
  
    
    

#### プロパティの数値

 **Integer**、 **Double**、および **Decimal** の管理プロパティの型を含む数値プロパティ値では、プロパティ制限はプロパティ値全体と照合されます。
  
    
    

### プロパティの日付または時刻の値

KQL では日付と時刻に対して **datetime** データ型を提供しています。クエリでは、次の ISO 8601 互換の datetime 形式がサポートされます。
  
    
    

- YYYY-MM-DD
    
  
- YYYY-MM-DDThh:mm:ss
    
  
- YYYY-MM-DDThh:mm:ssZ
    
  
- YYYY-MM-DDThh:mm:ssfrZ
    
  
これらの **datetime** 形式では、
  
    
    

-  _YYYY_ には、4 桁の年を指定します。
    
    > **メモ**
      > サポートされるのは、4 桁の年だけです。 
-  _MM_ には、2 桁の月を指定します。たとえば、01 は 1 月を意味します。
    
  
-  _DD_ には、2 桁の日 (01 ～ 31) を指定します。
    
  
-  _T_ には、文字 "T" を指定します。
    
  
-  _hh_ には、2 桁の時間 (00 ～ 23) を指定します。A.M. と P.M. は使用できません。
    
  
-  _mm_ には、2 桁の分 (00 ～ 59) を指定します。
    
  
-  _ss_ には、2 桁の秒 (00 ～ 59) を指定します。
    
  
-  _fr_ には、秒 ss の省略可能な小数部を指定します。秒の後の **.** に続けて 1 ～ 7 桁で指定します。たとえば、2012-09-27T11:57:34.1234567 のように指定します。
    
  
すべての日付と時刻の値は、UTC (協定世界時) (GMT (グリニッジ標準時) とも呼ばれます) タイム ゾーンに従って指定する必要があります。UTC タイムゾーンの識別子 (末尾の "Z" 文字) はオプションです。
  
    
    

#### KQL でサポートされている関連する日付間隔

KQL では、相対的な"day"範囲のクエリをサポートする検索クエリを作成することができます。予約済みのキーワードは表 4 に示されています。日付間隔には二重引用符 ("") を使用し、名前の間にはスペースを 1 つ入れます。
  
    
    


|**日付間隔の名前**|**説明**|
|:-----|:-----|
|today  <br/> |現在の日付の初めから現在の日付の終わりまでの時間を表します。  <br/> |
|yesterday  <br/> |現在の日付の直前の日の初めから終わりまでの時間を表します。  <br/> |
|this week  <br/> |現在の週の初めから現在の週の終わりまでの時間を表します。クエリ テキストがどのカルチャのもとで形成されたかに応じて、週の初めの日が決定されます。  <br/> |
|this month  <br/> |現在の月の初めから現在の月の終わりまでの時間を表します。  <br/> |
|last month  <br/> |現在の月の直前の月の全期間を表します。  <br/> |
|this year  <br/> |現在の年の初めから現在の年の終わりまでの時間を表します。  <br/> |
|last year  <br/> |現在の年の直前の年の全期間を表します。  <br/> |
   

### KQL クエリ内での複数のプロパティ制限の使用

SharePoint 2013 の検索では、同じ KQL クエリで複数のプロパティ制限を使用できます。複数のプロパティ制限で同じプロパティを使用するか、プロパティ制限ごとに異なるプロパティを使用することができます。 
  
    
    
同じプロパティ制限の複数のインスタンスを使用する場合、照合は KQL クエリの一連のプロパティ制限に基づいて行われます。照合には、以下のように John Smith または Jane Smith によって作成されたコンテンツ項目が含まれます。
  
    
    
 `author:"John Smith" author:"Jane Smith"`
  
    
    
この機能は、以下のように **OR** ブール演算子を使用するのと同じです。
  
    
    
 `author:"John Smith" OR author:"Jane Smith"`
  
    
    
異なるプロパティ制限を使用する場合、以下のように、照合は KQL クエリのプロパティ制限の共通部分に基づいて行われます。
  
    
    
 `author:"John Smith" filetype:docx`
  
    
    
照合には、John Smith が作成した Microsoft Word ドキュメントが含まれます。これは、以下のように **AND** ブール演算子を使用するのと同じです。
  
    
    
 `author:"John Smith" AND filetype:docx`
  
    
    

## 複雑なクエリの KQL 演算子
<a name="kql_operators"> </a>

KSQL 構文には、複雑なクエリを作成するときに使用できる複数の演算子が用意されています。 
  
    
    

### ブール演算子

ブール演算子を使用して、検索の範囲を広げたり、狭くしたりします。ブール演算子は、KQL クエリのフリー テキストの式とプロパティ制限で使用できます。表 5 では、サポートされるブール演算子を列挙します。
  
    
    

**表 5. KQL でサポートされるブール演算子**


|**演算子**|**説明**|
|:-----|:-----|
|**AND** <br/> |**AND** 演算子によって指定したすべてのフリー テキストの式またはプロパティ制限を含む検索結果を返します。 **AND** 演算子の前後両方に、有効なフリー テキストの式または有効なプロパティ制限を指定する必要があります。これは、正符号 ("+") を使用したときと同じです。 <br/> |
|**NOT** <br/> |指定したフリー テキストの式またはプロパティ制限を含まない検索結果を返します。 **NOT** 演算子の後に有効なフリー テキストの式または有効なプロパティ制限を指定する必要があります。これは、負符号 ("-") を使用したときと同じです。 <br/> |
|**OR** <br/> |指定したフリー テキストの式またはプロパティ制限のうち、1 つ以上を含む検索結果を返します。 **OR** 演算子の前後両方に、有効なフリー テキストの式または有効なプロパティ制限を指定する必要があります。 <br/> |
   

  
    
    

### 近接演算子

近接演算子を使用して、指定した検索用語が互いに至近距離にある結果を返します。近接演算子は、フリー テキストの式でのみ使用できます。KQL クエリのプロパティ制限での使用はサポートされません。 **NEAR** と **ONEAR** の 2 つの近接演算子があります。
  
    
    

#### NEAR 演算子

 **NEAR** 演算子は、指定された検索用語が互いに至近距離にある結果を返します。用語の順序は保持されません。 **NEAR** の構文は次のとおりです。
  
    
    
 `<expression> NEAR(n=4) <expression>`
  
    
    
 _n_ は、用語間の最大距離を示すオプションのパラメーターです。 _n_ の値は 0 以上の整数で、既定値は **8** です。
  
    
    
パラメーター  _n_ は、 `n=v` として指定できます。 _v_ は値を表すか、単に _v_ と短縮できます。たとえば `NEAR(4)` などで、 _v_ は 4 です。
  
    
    
例:
  
    
    
 `"acquisition" NEAR "debt"`
  
    
    
このクエリは、"acquisition" および "debt" という用語が同じアイテム内に出現し、"acquisition" のインスタンスの後に最大 8 つの他の用語が続き、さらに "debt" という用語のインスタンスが続くか、その反対の順序で出現するアイテムを返します。用語の順序は照合に影響しません。
  
    
    
用語間の距離を短くする必要がある場合は、それを指定できます。次のクエリは、"acquisition" および "debt" という用語が同じアイテム内に出現し、用語間の最大距離が 3 であるアイテムに一致します。ここでも、用語の順序は照合に影響しません。
  
    
    
 `"acquisition" NEAR(n=3) "debt"`
  
    
    

> **メモ**
> SharePoint 2013では、 **NEAR** 演算子はトークンの順序を保持しなくなります。また、 **NEAR** 演算子は、最大トークン距離を示すオプションのパラメーターを受け取ります。ただし、既定値は依然として **8** です。以前の動作を使用する必要がある場合は、代わりに **ONEAR** を使用します。
  
    
    


#### ONEAR 演算子

 **ONEAR** 演算子は、指定された検索用語が互いに至近距離にある結果を返し、用語の順序を保持します。 **ONEAR** の構文は次のとおりです。 _n_ は、用語間の最大距離を示すオプションのパラメーターです。 _n_ の値は 0 以上の整数で、既定値は **8** です。
  
    
    
 `<expression> ONEAR(n=4) <expression>`
  
    
    
パラメーター  _n_ は、 `n=v` として指定できます。 _v_ は値を表すか、単に _v_ と短縮できます。たとえば `ONEAR(4)` などで、 _v_ は 4 です。
  
    
    
たとえば、次のクエリは、"acquisition" および "debt" という用語が同じアイテム内に出現し、"acquisition" のインスタンスの後に最大 8 つの他の用語が続き、最後に "debt" という用語のインスタンスが続くアイテムを返します。用語の順序は、返すアイテムと一致する **必要があります** 。
  
    
    
 `"acquisition" ONEAR "debt"`
  
    
    
用語間の距離を短くする必要がある場合は、それを次のように指定できます。このクエリは、"acquisition" および "debt" という用語が同じアイテム内に出現し、用語間の最大距離が 3 であるアイテムに一致します。用語の順序は、返すアイテムと一致する **必要があります** 。
  
    
    
 `"acquisition" ONEAR(n=3) "debt"`
  
    
    

### シノニム演算子

 **WORDS** 演算子を使用して、クエリの用語は同意語であり、返される結果は、指定した用語のどちらかに一致する必要があることを指定します。 **WORDS** 演算子は、フリー テキストの式でのみ使用できます。KQL クエリのプロパティ制限での使用はサポートされません。
  
    
    
次のクエリの例では、用語 "TV" と用語 "television" のどちらかを含んでいる結果に一致します。この一致動作は、次のクエリを使用したときと同じです。
  
    
    
 `WORDS(TV, Television)`
  
    
    
 `TV OR Television`
  
    
    
この 2 つのクエリでは、結果のランク付けの方法が異なります。 **WORDS** 演算子を使用すると、用語 "TV" と "television" は、個別の用語ではなく同意語として処理されます。このため、どちらかの用語のインスタンスは、すべてのインスタンスが同じ用語の場合と同じようにランク付けされます。たとえば、用語 "television" の 1 つのインスタンスと用語 "TV" の 5 つのインスタンスを含んでいるコンテンツ項目は、用語 "TV" のインスタンスを 6 つ含んでいるコンテンツ項目と同じようにランク付けされます。
  
    
    

### ワイルドカード演算子

ワイルドカード演算子、つまりアスタリスク (" ***** ") を使用して、前方一致を有効にします。クエリでは、次に示すように、単語の先頭部分から単語の一部を指定し、その後にワイルドカード演算子を指定します。このクエリは、"serv" で始まり、他の文字がゼロ個以上続く用語を含む結果に一致します。たとえば、serve、server、service が一致します。
  
    
    
 `serv*`
  
    
    

### 包含演算子と除外演算子

包含演算子と除外演算子を使用して、フリー テキストの式またはプロパティ制限で指定した値に一致するコンテンツを、返される結果に含めるか、除外するかを指定できます。表 6 で、この演算子について説明します。
  
    
    

**表 6. 結果にコンテンツを含めるまたは除外するための演算子**


|**名前**|**演算子**|**動作**|
|:-----|:-----|:-----|
|包含  <br/> |" **+** " <br/> |包含する値に一致する値を含んでいるコンテンツが返されます。  <br/> これは、文字が指定されていない場合の既定の動作です。これは **AND** 演算子を使用したときと同じ動作です。 <br/> |
|除外  <br/> |" **-** " <br/> |除外する値に一致する値を含んでいるコンテンツが除外されます。これは **NOT** 演算子を使用したときの動作と同じです。 <br/> |
   

### 動的ランク付け演算子

 **XRANK** 演算子を使用して、 _match expression_内の特定の用語出現回数に基づいてアイテムの動的ランクをアップします。どのアイテムがクエリに一致するかに変更はありません。 **XRANK** 式には、一致する必要のある 1 つのコンポーネントである _match expression_、および動的ランク付けのみに貢献する 1 つ以上のコンポーネントである _rank expression_が含まれます。 **XRANK** 式を有効にするには、 _n_ を除く **1** つ以上のパラメーターを指定する必要があります。
  
    
    
 _Match expressions_ は入れ子にされた **XRANK**式を含む任意の有効な KQL 式です。 _Rank expressions_ は **XRANK** 式を含まない任意の有効な KQL 式です。KQL クエリに複数の **XRANK** 演算子がある場合、最終的な動的ランク値は、すべての **XRANK** 演算子のブーストの合計として計算されます。
  
    
    

> **メモ**
> 同じレベルの複数の **XRANK** 演算子がある KQL クエリの計算の順序を明示的に指示するには、かっこを使います。
  
    
    

 **XRANK** 演算子は、次の構文で使用できます。
  
    
    
 `<match expression> XRANK(cb=100, rb=0.4, pb=0.4, avgb=0.4, stdb=0.4, nb=0.4, n=200) <rank expression>`
  
    
    
 **XRANK** 演算子の動的ランク付けは、次の数式に基づいて計算されます。
  
    
    

  
    
    
![XRANK 演算子用の式](images/XRANKFormula.gif)
  
    
    
表 7 は、 **XRANK** 演算子で使用可能な基本パラメーターを示しています。
  
    
    

**表 7. XRANK 演算子パラメーター**


|**パラメーター**|**値**|**説明**|
|:-----|:-----|:-----|
| _n_ <br/> |<integer_value>  <br/> |統計を計算する結果の数を指定します。  <br/> このパラメーターは、動的ランクが貢献する結果の数には影響しません。統計の計算から無関係なアイテムを除外するだけです。  <br/> 既定値: **0** 。ゼロの値は、すべての *ドキュメント*  のセマンティックを引き継ぎます。 <br/> |
| _nb_ <br/> |<float_value>  <br/> | _nb_ パラメーターは、正規化されたブーストを表します。このパラメーターは、結果セットのランク値の差異および平均スコアの積に乗算する係数を指定します。 <br/> XRANK 数式の  _f_。  <br/> |
   
通常、正規化されたブーストである  _nb_ は、変更される唯一のパラメーターです。このパラメーターは、標準偏差を考慮せずに、特定のアイテムの昇格または降格に必要なコントロールを提供します。
  
    
    
次の詳細パラメーターも使用可能ですが、通常は使用しません。
  
    
    

**表 8. XRANK の詳細パラメーター**


|**パラメーター**|**値**|**説明**|
|:-----|:-----|:-----|
| _cb_ <br/> |<float_value>  <br/> | _cb_ パラメーターは定数ブーストを表します。 <br/> 既定値: **0** 。 <br/> XRANK 数式の  _a_。  <br/> |
| _stdb_ <br/> |<float_value>  <br/> | _stdb_ パラメーターは標準偏差ブーストを表します。 <br/> 既定値: **0** 。 <br/> XRANK 数式の  _e_。  <br/> |
| _avgb_ <br/> |<float_value>  <br/> | _avgb_ パラメーターは平均ブーストを表します。 <br/> 既定値: **0** 。 <br/> XRANK 数式の  _d_。  <br/> |
| _rb_ <br/> |<float_value>  <br/> | _rb_ パラメーターは範囲ブーストを表します。この係数は結果セットのランク値の範囲に乗算されます。 <br/> 既定値: **0** 。 <br/> XRANK 数式の  _b_。  <br/> |
| _pb_ <br/> |<float_value>  <br/> | _pb_ パラメーターはパーセンテージ ブーストを表します。この係数は、集成内の最小値と比較されるアイテム自身のランクに乗算されます。 <br/> 既定値: **0** 。 <br/> XRANK 数式の  _c_。  <br/> |
   

#### 例

 **例 1.** 次の式は、既定のフルテキスト インデックスに "cat" または "dog" が含まれるアイテムに一致します。定数ブーストが 100 で "thoroughbred" も含まれるアイテムについては、動的ランクが上昇します。
  
    
    
 `(cat OR dog) XRANK(cb=100) thoroughbred`
  
    
    
 **例 2.** 次の式は、既定のフルテキスト インデックスに "cat" または "dog" が含まれるアイテムに一致します。正規化されたブーストが 1.5 で "thoroughbred" も含まれるアイテムについては、動的ランクが上昇します。
  
    
    
 `(cat OR dog) XRANK(nb=1.5) thoroughbred`
  
    
    
 **例 3.** 次の式は、既定のフルテキスト インデックスに "cat" または "dog" が含まれるアイテムに一致します。定数ブーストが 100、正規化されたブーストが 1.5 で、"thoroughbred" も含まれるアイテムについては、動的ランクが上昇します。
  
    
    
 `(cat OR dog) XRANK(cb=100, nb=1.5) thoroughbred`
  
    
    
 **例 4.** 次の式は、"animals" が含まれるすべてのアイテムに一致し、次のように、動的ランクを上げます。
  
    
    

- "dogs" を含むアイテムの動的ランクは、100 ポイント上昇します。
    
  
- "cats" を含むアイテムの動的ランクは、200 ポイント上昇します。
    
  
- "dogs" および "cats" を含むアイテムの動的ランクは、300 ポイント上昇します。
    
  
 `(animals XRANK(cb=100) dogs) XRANK(cb=200) cats`
  
    
    

### かっこ

始めかっこ " **(** " と終わりかっこ " **)** " を使用して、キーワード クエリの異なる部分を組み合わせることができます。各始めかっこ " **(** " には、対応する終わりかっこ " **)** " が必要です。かっこの前後にある空白はクエリに影響しません。
  
    
    

## その他の技術情報
<a name="SP15KQL_addlresources"> </a>


-  [SharePoint 2013 で検索クエリを作成する](building-search-queries-in-sharepoint-2013.md)
    
  
