---
title: 关键字查询语言 (KQL) 语法参考
ms.prod: SHAREPOINT
ms.assetid: d8489f59-522f-433c-b9c1-69e597be51c7
---



# 关键字查询语言 (KQL) 语法参考
了解如何构造 SharePoint 2013 中的搜索功能 的 KQL 查询。该语法参考介绍了 KQL 查询元素和如何在 KQL 中使用属性限制和运算符。
## KQL 查询元素
<a name="SP15KQL_elements"> </a>

KQL 查询由以下一个或多个元素构成： 
  
    
    

- 自由文本关键字 - 字词或短语 
    
  
- 属性限制 
    
  
您可以将 KQL 查询元素与一个或多个可供使用的运算符组合起来。
  
    
    
如果 KQL 查询仅包含运算符或为空，则无效。KQL 查询不区分大小写，而运算符区分大小写（大写）。
  
    
    

> **注释**
> KQL 查询的长度限制取决于您创建它的方式。如果您使用默认的 SharePoint 搜索前端创建 KQL 查询，则其长度限制为 2,048 个字符。但是，使用查询对象模型以编程方式创建的 KQL 查询的默认长度限制是 4,096 个字符。您可以使用  [MaxKeywordQueryTextLength](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.MaxKeywordQueryTextLength.aspx) 属性或 [DiscoveryMaxKeywordQueryTextLength](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.SearchServiceApplication.DiscoveryMaxKeywordQueryTextLength.aspx) 属性（用于电子数据展示）将该限制最高增加到 20,480 个字符。
  
    
    


## 使用 KQL 构造自由文本查询
<a name="SP15KQL_constructing_freetext_queries"> </a>

使用自由文本表达式构造 KQL 查询时，SharePoint 2013 中的搜索功能 根据存储在全文本索引中的字词为您所选的查询字词匹配结果。这包括  [FullTextQueriable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.FullTextQueriable.aspx) 设为 **true** 的托管属性值。
  
    
    
自由文本 KQL 查询不区分大小写，但运算符必须为大写。您可以使用以下一项或多项作为自由文本表达式来构造 KQL 查询：
  
    
    

- **word**（包括一个或多个字符，不含空格或标点）
    
  
- **phrase**（包括两个或多个由空格分隔的字词；但是，这些字词必须括在双引号内）
    
  
若要构造复杂查询，您可以组合多个自由文本表达式与 KQL 查询运算符。如果多个自由文本表达式之间没有任何运算符，则查询行为与使用 **AND** 运算符一样。
  
    
    

### 在自由文本 KQL 查询中使用字词

若在自由文本 KQL 查询中使用字词，SharePoint 2013 中的搜索功能 将根据您的字词与存储在全文本索引中的字词的完全匹配情况返回结果。您可以使用通配符 (*) 启用前缀匹配，仅使用字词开头的一部分进行查询。在前缀匹配中，SharePoint 2013 中的搜索功能 将结果与包含后跟零个或多个字符的字词的字词相匹配。
  
    
    
例如，下面的 KQL 查询将返回包含词"联合"和"搜索"的内容项： 
  
    
    
 `federated search`
  
    
    
 `federat* search`
  
    
    
 `search fed*`
  
    
    
KQL 查询不支持后缀匹配。
  
    
    

### 在自由文本 KQL 查询中使用短语

若在自由文本 KQL 查询中使用短语，SharePoint 2013 中的搜索功能 将仅返回您的短语中的字词在一起的项。若要在 KQL 查询中指定短语，则必须使用双引号。 
  
    
    
KQL 查询不支持后缀匹配，所以在自由文本查询中，您无法在短语前面使用通配符。但是，您可以在短语后面使用通配符。
  
    
    

## KQL 中的属性限制查询
<a name="kql_property_restriction_queries"> </a>

使用 KQL 时，您可以构造使用属性限制的查询来缩小查询重点，以便仅根据指定条件匹配结果。
  
    
    

### 指定属性限制

基本属性限制由以下内容组成：
  
    
    
 `<Property Name><Property Operator><Property Value>`
  
    
    
表 1 列出了 KQL 查询中一些有效属性限制语法的示例。
  
    
    

**表 1. 有效属性限制语法**


|**语法**|**返回**|
|:-----|:-----|
| `author:"John Smith"` <br/> |返回 John Smith 创作的内容项。  <br/> |
| `filetype:docx` <br/> |返回 Microsoft Word 文档。  <br/> |
| `filename:budget.xlsx` <br/> |返回文件名为  `budget.xlsx` 的内容项。 <br/> |
   
属性限制不应在属性名称、属性运算符和属性值之间包括空格，否则它将被视为自由文本查询。属性限制长度限制为 2,048 个字符。 
  
    
    
在下面的示例中，空格导致查询返回包含"创作"和"John Smith"的内容项，而不是由 John Smith 编写的内容项：
  
    
    
 `author: "John Smith"`
  
    
    
 `author :"John Smith"`
  
    
    
 `author : "John Smith"`
  
    
    
换言之，上述属性限制等同于以下内容：
  
    
    
 `author "John Smith"`
  
    
    

### 为属性限制指定属性名称

您必须为属性限制指定有效的托管属性名称。默认情况下，SharePoint 2013 中的搜索功能 包括文档的多个托管属性。
  
    
    
若要为爬网属性值指定属性限制，您首先必须将爬网属性映射到托管属性。请参阅 [规划最终用户搜索体验](http://technet.microsoft.com/zh-cn/library/cc263089.aspx)中的 **托管与爬网属性** 。
  
    
    
托管属性必须是  [Queryable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Queryable.aspx) ，以便您可以在文档中搜索该托管属性。此外，对于要检索的托管属性，它可以是 [Retrievable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Retrievable.aspx) 。但是，托管属性不需要一定是 [Retrievable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.Retrievable.aspx) 才能执行属性搜索。
  
    
    

### 在属性限制中受支持的属性运算符

SharePoint 2013 中的搜索功能 支持属性限制的多个属性运算符，如表 2 所示。 
  
    
    

**表 2. 属性限制的有效属性运算符**


|**运算符**|**说明**|**受支持的托管属性类型**|
|:-----|:-----|:-----|
|:  <br/> |返回在属性限制中指定的值等于存储在属性存储数据库中的属性值的结果，或与存储在全文本索引中的属性值中的单个词相匹配的结果。  <br/> | [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|=  <br/> |返回属性值等于在属性限制中指定的值的搜索结果。  <br/> <br/> **注释**<br/> 在执行精确匹配时，我们建议不要将 **=** 运算符和星号 ( *****) 组合在一起。           | [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|<  <br/> |返回属性值小于在属性限制中指定的值的搜索结果。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|>  <br/> |返回属性值大于在属性限制中指定的值的搜索结果。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|<=  <br/> |返回属性值小于或等于在属性限制中指定的值的搜索结果。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|>=  <br/> |返回属性值大于或等于在属性限制中指定的值的搜索结果。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
|<>  <br/> |返回属性值不等于在属性限制中指定的值的搜索结果。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/>  [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> |
|..  <br/> |返回属性值属于在属性限制中指定的范围的搜索结果。  <br/> 例如，范围 A..B 表示从 A 到 B（包括 A 和 B 在内）的一组值。对于日期范围，这表示从 A 日的开始到 B 日的结束。  <br/> | [DateTime](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/>  [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/>  [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/>  [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> |
   

### 指定属性值

您必须为托管属性的类型指定数据类型有效的属性值。表 3 列出了这些类型映射。
  
    
    

**表 3. 对托管属性类型有效的数据类型映射**


|**托管类型**|**数据类型**|
|:-----|:-----|
| [Text](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Text.aspx) <br/> | [String](https://msdn.microsoft.com/library/System.String.aspx) <br/> |
| [Integer](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Integer.aspx) <br/> | [Int64](https://msdn.microsoft.com/library/System.Int64.aspx) <br/> |
| [Double](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Double.aspx) <br/> | [System.Double](https://msdn.microsoft.com/library/System.Double.aspx) <br/> |
| [Decimal](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.Decimal.aspx) <br/> | [Decimal](https://msdn.microsoft.com/library/System.Decimal.aspx) <br/> |
| [DateTime()](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.DateTime.aspx) <br/> | [DateTime](https://msdn.microsoft.com/library/System.DateTime.aspx) <br/> |
| [YesNo](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedDataType.YesNo.aspx) <br/> | [Boolean](https://msdn.microsoft.com/library/System.Boolean.aspx) <br/> |
   

#### 文本属性值

对于文本属性值来说，匹配行为取决于属性存储在全文本索引或搜索索引中。 
  
    
    

#### 全文本索引中的属性值

如果将托管属性的 **FullTextQueriable** 属性设置为 **true**，则属性值将存储在全文本索引中。您仅可以为字符串属性执行该配置。在查询中指定的属性值将与存储在全文本索引中的单个词进行匹配。使用  [NoWordBreaker](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedProperty.NoWordBreaker.aspx) 属性指定是否匹配整个属性值。
  
    
    
例如，如果您搜索由 Paul Shakespear 创作的内容项，则下面的 KQL 查询将返回匹配结果：
  
    
    
 `author:Shakespear`
  
    
    
 `author:Paul`
  
    
    
同样支持前缀匹配。您可以使用通配符 (*)，但是当您指定单个字词时，则不需要通配符。继续上面的示例，下面的 KQL 查询将返回由 Paul Shakespear 创作的内容项作为匹配结果： 
  
    
    
 `author:Shakesp*`
  
    
    
当您为属性值指定短语时，匹配的结果必须在全文本索引中存储的属性值内包含指定短语。下面的查询示例将返回标题中含"高级搜索"文本的内容项，如"高级搜索 XML"和"了解高级搜索 Web 部件"等：
  
    
    
 `title:"Advanced Search"`
  
    
    
在属性值中指定的短语也支持前缀匹配，但是必须在查询中使用通配符 (*)，而且仅支持在短语末尾使用，如下所示：
  
    
    
 `title:"Advanced Sear*"`
  
    
    
下面的查询将不返回预期结果：
  
    
    
 `title:"Advan* Search"`
  
    
    
 `title:"Advanced Sear"`
  
    
    

#### 属性数值

对于数值型属性值（包括 **Integer**、 **Double** 和 **Decimal** 托管类型）来说，属性限制将与属性的整个值进行匹配。
  
    
    

### 属性的日期或时间值

KQL 提供日期和时间的 **datetime** 数据类型。在查询中支持与 ISO 8601 兼容的以下日期时间格式：
  
    
    

- YYYY-MM-DD
    
  
- YYYY-MM-DDThh:mm:ss
    
  
- YYYY-MM-DDThh:mm:ssZ
    
  
- YYYY-MM-DDThh:mm:ssfrZ
    
  
在这些 **datetime** 格式中：
  
    
    

-  _YYYY_ 指定四位数年份。
    
    > **注释**
      > 仅支持四位数年份。 
-  _MM_ 指定两位数月份。例如，01 表示 1 月。
    
  
-  _DD_ 指定两位数日期（01 到 31）。
    
  
-  _T_ 指定字母"T"。
    
  
-  _hh_ 指定两位数小时（00 到 23）；不允许使用 A.M./P.M. 指示法。
    
  
-  _mm_ 指定两位数分钟（00 到 59）。
    
  
-  _ss_ 指定两位数秒钟（00 到 59）。
    
  
-  _fr_ 指定秒数的可选分数，ss；跟随秒数后 **.** 的 1 到 7 位数。例如，2012-09-27T11:57:34.1234567。
    
  
必须根据 UTC（协调世界时，也称为 GMT，即格林威治标准时间）指定所有日期/时间值。
  
    
    

#### KQL 支持的相关日期间隔

KQL 使您可以构建支持相对"日"范围查询的搜索查询，保留关键字如表 4 所示。使用双引号 ("") 表示日期间隔，名称之间留出一个空格。
  
    
    


|**日期间隔的名称**|**说明**|
|:-----|:-----|
|today  <br/> |表示从当前日期开始到当前日期结束的时间。  <br/> |
|yesterday  <br/> |表示当前日期前一天从开始到结束的时间。  <br/> |
|this week  <br/> |表示从本周开始到本周结束的时间。确定一周的第一天需考虑查询文本所使用的文化。  <br/> |
|this month  <br/> |表示从本月开始到本月结束的时间。  <br/> |
|last month  <br/> |表示本月之前的整个月。  <br/> |
|this year  <br/> |表示从今年开始到今年结束的时间。  <br/> |
|last year  <br/> |表示今年之前的一整年。  <br/> |
   

### 在 KQL 查询内使用多个属性限制

SharePoint 2013 中的搜索功能 支持在同一 KQL 查询内使用多个属性限制。您可以对多个属性限制使用相同的属性，或对每一个属性限制使用不同的属性。 
  
    
    
当您使用同一属性限制的多个实例时，匹配将以 KQL 查询中属性限制的组合为基础。匹配将包括由 John Smith 或 Jane Smith 创作的内容项，如下所示：
  
    
    
 `author:"John Smith" author:"Jane Smith"`
  
    
    
该功能与使用 **OR** 布尔运算符作用相同，如下所示：
  
    
    
 `author:"John Smith" OR author:"Jane Smith"`
  
    
    
当您使用不同的属性限制时，匹配将以 KQL 查询中的属性限制交集为基础，如下所示：
  
    
    
 `author:"John Smith" filetype:docx`
  
    
    
匹配将包括由 John Smith 创作的 Microsoft Word 文档。这与使用 **AND** 布尔运算符作用相同，如下所示：
  
    
    
 `author:"John Smith" AND filetype:docx`
  
    
    

## 用于复杂查询的 KQL 运算符
<a name="kql_operators"> </a>

KQL 语法包括多个可用于构造复杂查询的运算符。 
  
    
    

### 布尔运算符

您可以使用布尔运算符扩大或缩小搜索范围。您可以在 KQL 查询中，结合自由文本表达式和属性限制来使用布尔运算符。表 5 列出了受支持的布尔运算符。
  
    
    

**表 5. KQL 支持的布尔运算符**


|**运算符**|**说明**|
|:-----|:-----|
|**AND** <br/> |返回包括使用 **AND** 运算符指定的自由文本表达式或属性限制的所有搜索结果。您必须在 **AND** 运算符的前面和后面都指定有效的自由文本表达式和/或有效的属性限制。这与使用加号（"+"）字符作用相同。 <br/> |
|**NOT** <br/> |返回不包括指定的自由文本表达式或属性限制的搜索结果。您必须在 **NOT** 运算符后面指定有效的自由文本表达式和/或有效的属性限制。这与使用减号（"-"）字符作用相同。 <br/> |
|**OR** <br/> |返回包括一个或多个指定的自由文本表达式或属性限制的搜索结果。您必须在 **OR** 运算符的前面和后面都指定有效的自由文本表达式和/或有效的属性限制。 <br/> |
   

  
    
    

### 邻近运算符

您使用邻近运算符来匹配指定的搜索字词彼此接近的结果。邻近运算符仅可与自由文本表达式一起使用；不支持在 KQL 查询中与属性限制一起使用。有两种邻近运算符： **NEAR** 和 **ONEAR**。
  
    
    

#### NEAR 运算符

 **NEAR** 运算符匹配指定搜索字词彼此相邻近的结果，但不保留字词顺序。 **NEAR** 语法如下所示：
  
    
    
 `<expression> NEAR(n=4) <expression>`
  
    
    
 _n_ 是指示字词之间最大距离的可选参数。 _n_ 的值是 >= 0 的整数，默认为 **8** 。
  
    
    
可以将参数  _n_ 指定为 `n=v`，其中  _v_ 代表值，或者可以缩短为 _v_；例如  `NEAR(4)`，其中  _v_ 是 4。
  
    
    
例如：
  
    
    
 `"acquisition" NEAR "debt"`
  
    
    
该查询将匹配"acquisition"和"debt"出现在相同项内的结果，其中"acquisition"实例后面最多跟 8 个其他字词，然后再跟"debt"实例，反之亦然。字词顺序对匹配的影响不大。
  
    
    
如果您需要缩短字词之间的距离，您可以进行指定。下面的查询将匹配"acquisition"和"debt"出现在相同项内的结果，两个字词之间的最大距离为 3。同样，字词顺序不影响匹配。
  
    
    
 `"acquisition" NEAR(n=3) "debt"`
  
    
    

> **注释**
> 在 SharePoint 2013 中， **NEAR** 运算符不再保留标记的排序。此外， **NEAR** 运算符现在接收了一个指示最大标记距离的可选参数。但是，默认值仍为 **8** 。如果您必须使用以前的行为，则改用 **ONEAR**。 
  
    
    


#### ONEAR 运算符

 **ONEAR** 运算符匹配指定搜索字词彼此相邻近的结果，同时保留字词顺序。 **ONEAR** 语法如下所示， _n_ 是指示字词之间最大距离的可选参数。 _n_ 值是 >= 0 的整数，默认为 **8** 。
  
    
    
 `<expression> ONEAR(n=4) <expression>`
  
    
    
可以将参数  _n_ 指定为 `n=v`，其中  _v_ 代表值，或者可以缩短为 _v_；例如  `ONEAR(4)`，其中  _v_ 是 4。
  
    
    
例如，下面的查询将匹配"acquisition"和"debt"出现在相同项内的结果，其中"acquisition"实例后面最多跟 8 个其他字词，然后再跟"debt"实例。字词顺序 **必须** 与将返回的项匹配：
  
    
    
 `"acquisition" ONEAR "debt"`
  
    
    
如果您需要缩短字词之间的距离，您可以按如下所示进行指定。该查询将匹配"acquisition"和"debt"出现在相同项内的结果，两字词之间的最大距离为 3。字词顺序 **必须** 与将返回的项匹配：
  
    
    
 `"acquisition" ONEAR(n=3) "debt"`
  
    
    

### 同义词运算符

您可以使用 **WORDS** 运算符指定查询的字词为同义词，返回的结果应匹配指定字词的其中一个。 **WORDS** 运算符仅可以与自由文本表达式一起使用；它不支持在 KQL 查询中与属性限制一起使用。
  
    
    
下面的查询示例将匹配包含"TV"或"television"的结果。该匹配行为与您使用下面的查询作用相同：
  
    
    
 `WORDS(TV, Television)`
  
    
    
 `TV OR Television`
  
    
    
这些查询的差异在于结果的排名方式。使用 **WORDS** 运算符时，"TV"和"television"被当做同义词而非单独的字词。因此，为二者之中任一字词的实例排名时，会将其视为相同的字词。例如，包含一个"television"实例和五个"TV"实例的内容项与包含六个"TV"实例的内容项排名相同。
  
    
    

### 通配符运算符

您可以使用通配符，即星号字符（" ***** "）来启用前缀匹配。您可以在查询中指定从首字母开始的部分字词，后跟通配符，如下所示。该查询将匹配包含以"serv"开始的字词的结果，"serv"后面可跟零个或多个字符，如 serve、server 和 service 等：
  
    
    
 `serv*`
  
    
    

### 包含和不包含运算符

您可以使用包含和不包含运算符，以指定返回的结果是否应包含或不包含与通过自由文本表达式或属性限制指定的值匹配的内容，如表 6 所示。
  
    
    

**表 6. 在结果中包含和不包含内容的运算符**


|**名称**|**运算符**|**行为**|
|:-----|:-----|:-----|
|包含  <br/> |" **+** " <br/> |包含具有与包含项匹配的值的内容。  <br/> 如果未指定字符，则这是默认行为。这与使用 **AND** 运算符作用相同。 <br/> |
|不包含  <br/> |" **-** " <br/> |不包含具有与不包含项匹配的值的内容。这与使用 **NOT** 运算符作用相同。 <br/> |
   

### 动态排名运算符

您可以使用 **XRANK** 运算符，基于特定字词在 _match expression_中出现的次数来提升项的动态排名，而无需更改要与查询匹配的项。 **XRANK** 表达式包含一个必须匹配的组件（即 _match expression_）以及一个或多个仅对动态排名产生作用的组件（即 _rank expression_）。必须指定至少 **1** 个参数（不包括 _n_），才能使 **XRANK** 表达式有效。
  
    
    
 _Match expressions_ 可能是任何有效的 KQL 表达式，包括嵌套的 **XRANK** 表达式。 _Rank expressions_ 可能是任何有效的 KQL 表达式，不包括 **XRANK** 表达式。如果您的 KQL 查询具有多个 **XRANK** 运算符，则最终的动态排名值将按所有 **XRANK** 运算符的提升总和计算。
  
    
    

> **注释**
> 使用圆括号明确指示在同一级别具有多个 **XRANK** 运算符的 KQL 查询的计算顺序。
  
    
    

您可以在下面的语法中使用 **XRANK** 运算符：
  
    
    
 `<match expression> XRANK(cb=100, rb=0.4, pb=0.4, avgb=0.4, stdb=0.4, nb=0.4, n=200) <rank expression>`
  
    
    
 **XRANK** 运算符的动态排名根据以下公式进行计算：
  
    
    

  
    
    
![用于 XRANK 运算符的公式](images/XRANKFormula.gif)
  
    
    
表7 列出了可供 **XRANK** 运算符使用的基本参数。
  
    
    

**表 7. XRANK 运算符参数**


|**参数**|**值**|**说明**|
|:-----|:-----|:-----|
| _n_ <br/> | _<integer_value>_ <br/> |指定根据其来计算统计数据的结果数。  <br/> 此参数不会影响受动态排名影响的结果数；仅意味着从统计数据计算中排除不相关的项。  <br/> 默认值： **0** 。0 值表示 *所有文档*  的语义。 <br/> |
| _nb_ <br/> | _<float_value>_ <br/> | _nb_ 参数引用归一化提高。此参数指定结果集排名值的方差和平均分之积所乘以的因素。 <br/> XRANK 公式中的  _f_。  <br/> |
   
通常，归一化提高  _nb_ 是唯一受到修改的参数。此参数对特定项目的升级或降级提供必要的控制，而不考虑标准偏差。
  
    
    
以下高级参数也可用。但是，通常不使用它们。
  
    
    

**表 8. XRANK 高级参数**


|**参数**|**值**|**说明**|
|:-----|:-----|:-----|
| _cb_ <br/> | _<float_value>_ <br/> | _cb_ 参数引用恒定提高。 <br/> 默认值： **0** 。 <br/> XRANK 公式中的  _a_。  <br/> |
| _stdb_ <br/> | _<float_value>_ <br/> |The  _stdb_ 参数引用标准偏差提高。 <br/> 默认值： **0** 。 <br/> XRANK 公式中的  _e_。  <br/> |
| _avgb_ <br/> | _<float_value>_ <br/> | _avgb_ 参数引用平均提高。 <br/> 默认值： **0** 。 <br/> XRANK 公式中的  _d_。  <br/> |
| _rb_ <br/> | _<float_value>_ <br/> |The  _rb_ 参数引用平均提高。此因数乘以结果集中排名值的范围。 <br/> 默认值： **0** 。 <br/> XRANK 公式中的  _b_。  <br/> |
| _pb_ <br/> | _<float_value>_ <br/> | _pb_ 参数引用比例提高。此因数乘以项本身相较于语料库中最小值的排名。 <br/> 默认值： **0** 。 <br/> XRANK 公式中的  _c_。  <br/> |
   

#### 示例

 **示例 1.** 下面的表达式与其默认全文本索引包含"cat"或"dog"的项匹配。此表达式会提升还包含"thoroughbred"且恒定提高 100 的项的动态排名。
  
    
    
 `(cat OR dog) XRANK(cb=100) thoroughbred`
  
    
    
 **示例 2.** 下面的表达式与其默认全文本索引包含"cat"或"dog"的项匹配。此表达式会提升还包含"thoroughbred"且归一化提高 1.5 的项的动态排名。
  
    
    
 `(cat OR dog) XRANK(nb=1.5) thoroughbred`
  
    
    
 **示例 3.** 下面的表达式与其默认全文本索引包含"cat"或"dog"的项匹配。此表达式会提升还包含"thoroughbred"且恒定提高 100，归一化提高 1.5 的项的动太排名。
  
    
    
 `(cat OR dog) XRANK(cb=100, nb=1.5) thoroughbred`
  
    
    
 **示例 4.** 以下表达式将与所有包含字词"动物"的项匹配，并将提升动态排名，如下所示：
  
    
    

- 包含字词"狗"的项的动态排名提升了 100 点。
    
  
- 包含字词"猫"的项的动态排名提升了 200 点。
    
  
- 包含字词"狗"和"猫"的项的动态排名提升了 300 点。
    
  
 `(animals XRANK(cb=100) dogs) XRANK(cb=200) cats`
  
    
    

### 圆括号

您可以使用开括号字符" **(** "和闭括号字符" **)** "来组合关键字查询的不同部分。每一个开括号" **(** "都必须有匹配的闭括号" **)** "。圆括号前面或后面的空格将不影响查询。
  
    
    

## 其他资源
<a name="SP15KQL_addlresources"> </a>


-  [在 SharePoint 2013 中生成搜索查询](building-search-queries-in-sharepoint-2013.md)
    
  
