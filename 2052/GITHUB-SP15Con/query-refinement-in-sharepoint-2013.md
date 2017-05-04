---
title: SharePoint 2013 中的查询细化
ms.prod: SHAREPOINT
ms.assetid: ec31782e-1bc5-4dc3-8df7-c29cd5f7f05c
---



# SharePoint 2013 中的查询细化
了解在使用搜索查询和结果时，如何以编程方式使用 SharePoint Server 2013 查询细化功能。
你可以使用查询细化功能为最终用户提供与其查询相关的细化选项。通过这些功能，最终用户使用针对结果计算的细化数据深入了解搜索结果。细化数据由索引组件进行计算，这基于搜索查询所有结果的托管属性统计的聚合。
  
    
    

通常，查询细化用于与索引项（如项目中出现的创建日期、作者或文件类型）关联的元数据。通过使用细化选项，你可以优化查询以仅显示特定时段内创建的项，或仅显示特定文件类型的项。
## 在查询对象模型中使用精简条件
<a name="SP15_Using_refiners"> </a>

查询细化所涉及的两类查询：
  
    
    

1. 你可以通过 [添加细化规格](#SP15_Adding_refiners)到最终用户的查询，以请求一组要在搜索结果中返回的精简条件。细化规格是  [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) 属性的输入。此查询针对搜索索引运行。搜索结果由相关结果和细化数据组成。
    
  
2. 你可以通过 [创建精简的查询](#SP15_Creating_refined_query)使用细化数据深入了解搜索结果。将  [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) 属性添加到查询中，以使最终搜索结果满足原始最终用户查询文本和细化数据选定的细化选项的要求。
    
  
以下各部分详细说明这些步骤，并提供代码示例。
  
    
    

## 使用精简条件规格将精简条件添加到最终用户的查询中
<a name="SP15_Adding_refiners"> </a>

你可以使用  [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.KeywordQuery.aspx) 类的 [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) 属性指定请求的查询精简条件。请使用以下语法指定请求的查询精简条件：
  
    
    
 `<refiner>[,<refiner>]*`
  
    
    

  
    
    
每个  `refiner` 都具有以下格式：
  
    
    
 `<refiner-name>[(parameter=value[,parameter=value]*)]?`
  
    
    

  
    
    
其中：
  
    
    

-  `<refiner-name>` 是与精简条件关联的托管属性的名称。在搜索架构中必须将此托管属性设置为 [Refinable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Refinable.aspx) 或 [Sortable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Sortable.aspx) 。
    
  
-  `parameter=value` 对的可选列表指定命名精简条件的非默认配置值。如果精简条件的参数未在括号中列出，搜索架构配置会提供默认设置。表 1 列出了 `parameter=value` 对可能的值。
    
  

> **注释**
> 如果要指定精简条件，最低要求是指定托管属性  `refiner-name`。 <br/> **Example** <br/>`Refiners = "FileType"`<br/> <br/> 或者你还可以使用高级语法调整精简条件设置。 <br/> **Example**<br/> `Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22),companies"`
  
    
    


**表 1：精简条件参数列表**


|**参数**|**说明**|
|:-----|:-----|
| _deephits_ <br/> |替换作为精简条件计算基数的默认点击量。如果生成精简条件，则将计算查询的所有结果。正常情况下，使用此参数将提升搜索性能。  <br/> **Syntax**          `deephits=<integer value>` <br/> **Example**          `price(deephits=1000)` <br/> <br/> **注释**<br/> 此限制应用于每个索引分区内。由于各搜索分区的聚合，计算的实际点击量将大于此值。           |
| _discretize_ <br/> | 指定数值精简条件的自定义时间间隔（细化箱） <br/> **Syntax**          `discretize=manual/<threshold>/<threshold>[/<threshold>]*` <br/> **Example**          `write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22)` <br/>  `<threshold>` 属性指定每个细化箱的阈值。 <br/>  在指定的第一个阈值下面有一个针对所有内容的时间间隔，每个连续阈值间有一个时间间隔，最后一个阈值上面也有一个针对所有内容的时间间隔。 <br/>  对于类型 **DateTime** 的精简条件，按照以下 ISO 8601 兼容的格式之一指定阈值： <br/>  _YYYY-MM-DD_ <br/>  _YYYY-MM-DDThh:mm:ss_ <br/>  _YYYY-MM-DDThh:mm:ss:Z_ <br/>  格式的值指定以下内容： <br/>  _YYYY_ - 四位数年份。仅支持四位数年份。 <br/>  _MM_ - 两位数月份。01 = 一月。 <br/>  _DD_ - 月份中两位数的天数（ 从 01 到 31）。 <br/>  _T_ - 字母 "T"。 <br/>  _hh_ - 两位数的小时数（从 00 到 23）。不允许用 A.M. 或 P.M. 表示。 <br/>  _mm_ - 两位数的分钟数（从 00 到 59）。 <br/>  _ss_ - 两位数的秒数（从 00 到 59）。 <br/> <br/> **重要信息**<br/>  必须根据协调世界时 (UTC)，也称为格林威治标准时间 (GMT) 区域指定所有日期/时间值。UTC 区域标识符（尾部字符"Z"）可选。          |
| _sort_ <br/> | 定义如何在字符串精简条件内对箱进行排序。 <br/> **Syntax**          `sort=<property>/<direction>` <br/>  属性执行以下操作： <br/>  _<property>_ - 指定排序算法。这些小写字母的值均有效： <br/> **frequency** - 按箱内出现情况排序。 <br/> **name** - 按标签名称排序。 <br/> **number** - 将字符串视为数字，并使用数字分类。此值可用于指定离散值，例如，执行包含在类型 **String** 的托管属性中的值的数字分类。 <br/>  _<direction>_ - 指定排序方向。这些小写字母的值均有效： <br/> **descending** <br/> **ascending** <br/> **Example**          `sort=name/ascending` <br/> **Default**：频率/降序  <br/> |
| _filter_ <br/> | 定义在将类型 **String** 精简条件中的箱返回到客户端之前，对其进行筛选的方式。 <br/> **Syntax**          `filter=<bins>/<freq>/<prefix>[<levels>]` <br/>  属性执行以下操作： <br/>  _<bins>_ - 指定返回的箱的最大数量。 <br/>  对精简条件中的箱进行排序后，使用此属性截取所有排在尾部的箱。例如，如果箱=10，则根据指定的排序算法仅返回前 10 个箱。 <br/>  _<freq>_ - 限制返回的箱的数量。 <br/>  使用此属性删除具有低频率计数的箱。例如， _freq_=2 表示仅返回包含 2 个或多个数量的箱。  <br/>  _<prefix>_ - 定义仅返回包含以此前缀开头的名称的箱。 <br/>  通配符"*"匹配所有名称。 <br/> **Example** <br/>  `filter=30/2/*` <br/>  _<levels>_ - 指定分类级别。 <br/>  使用此属性筛选基于分类级别的分层精简条件输出。此属性应为正整数 _n_。 **default** 值为 _n_=0。如果  _n_>0，则仅返回包含少于  _n_ 个分类路径分隔符 ("/") 的精简条件条目。如果 _n_=0，则不执行任何级别筛选。级别分隔符为正斜杠字符 ("/")。  <br/>  请注意使用大型分类导航器的性能成本。将分类作为结果处理的一部分来执行。 <br/> <br/> **注释**>  此参数应用特定于应用程序的结果筛选。这与截止参数有所不同，由于性能原因，截止参数在每个索引分区中应用限制。          |
| _cutoff_ <br/> | 限制必须针对深入字符串精简条件转移和处理的数据。你可以仅将精简条件配置为返回相关度最高的值（箱）。 <br/> <br/> **注释**<br/>  在每个索引分区内执行截止筛选。这与 _filter_ 参数有所不同，后者仅执行结果筛选。你可以合并这两个参数。          **Syntax**          `cutoff=<frequency>/<minbins>/<maxbins>` <br/>  属性执行以下操作： <br/>  _<maxbins>_ - 限制箱的数量。 <br/>  此参数限制精简条件将返回的唯一值（箱）的数量。这是在返回字符串精简条件和大量箱时，提高搜索性能的首选方式。"0"表示已使用搜索架构中指定的默认值。 <br/>  _<frequency>_ - 根据频率限制箱的数量。 <br/>  如果结果集中精简条件值出现的次数小于或等于频率值，则不返回精简条件值。"0"表示已使用搜索架构的默认值。 <br/>  _<minbins>_ - 指定频率截止的最小值。 <br/>  如果查询的唯一精简条件值的数量小于此值，则不执行任何频率截止，并且从该搜索分区中返回所有精简条件值。如果已使用按频率截止，则此参数可用于指定将要返回的唯一精简条件值的最小数量，而不考虑出现的次数。此属性的默认值为"0"，表示无论唯一导航器值是多少，都会执行频率截止。"0"表示已使用索引架构中指定的默认值。 <br/> <br/> **注释**<br/>  需小心谨慎地使用 _<frequency>_ 和 _<minbins>_。建议你仅使用  _<maxbins>_。           |
   

### 示例：添加精简条件
<a name="SP15_Example_adding_refiners"> </a>

以下 CSOM 示例说明如何以编程方式请求三个精简条件： **FileType**、 **Write** 和 **Companies**。 **Write** 代表项目最后修改日期，并使用高级语法返回固定大小日期/时间箱。
  
    
    

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


## 了解搜索结果中的精简数据
<a name="SP15_Understanding_refinement_data"> </a>

如果你已为查询中的托管属性启用查询细化，则查询结果会包含拆分为细化箱的精简数据。此数据位于  [ResultTableCollection](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ResultTableCollection.aspx) . **RefinementResults** 中的表 ( [RefinementResults](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KnownTableTypes.RefinementResults.aspx) ) 内。一个细化箱代表托管属性的一个特定值或值的范围。 **RefinementResults** 表包含每个细化箱的一行，还包含所有列，如表 2 中所指定的内容。
  
    
    

**表 2：为细化箱返回的数据**


|**参数**|**说明**|
|:-----|:-----|
|**RefinerName** <br/> |查询精简条件的名称。  <br/> |
|**RefinementName** <br/> |表示细化箱的字符串。如果要在搜索结果页面上向用户呈现精简选项，则通常使用此字符串通。  <br/> |
|**RefinementValue** <br/> |表示细化的特定于实现的格式化字符串。为调试返回此数据，且客户端通常不需要此数据。  <br/> |
|**RefinementToken** <br/> |表示在执行精简的查询时使用 **RefinerName** 的细化箱。 <br/> |
|**RefinementCount** <br/> |此细化箱的结果计数。此数据表示搜索结果中的项目（包括副本）数量，该值为针对此细化箱给定托管属性的值。  <br/> |
   

### 示例：精简数据
<a name="SP15_Example_refinement_data"> </a>

下面的表 3 包含两行精简数据。第一行是索引项的精简数据，其中文件类型为 HTML。第二行是索引项的精简数据，其中最后修改时间为 2013/09/21 到 2013/09/22。
  
    
    

**表 3：精简数据的格式和内容**


|**RefinerName**|**RefinementName**|**RefinementValue**|**RefinementToken**|**RefinementCount**|
|:-----|:-----|:-----|:-----|:-----|
|FileType  <br/> |Html  <br/> |Html  <br/> |"??68746d6c"  <br/> |50553  <br/> |
|写入  <br/> |从 2013-09-21T00:00:00Z 到 2013-09-22T00:00:00Z  <br/> |从 2013-09-21T00:00:00Z 到 2013-09-22T00:00:00Z  <br/> |range(2013-09-21T00:00:00Z, 2013-09-22T00:00:00Z)  <br/> |37  <br/> |
   

## 创建精简的查询
<a name="SP15_Creating_refined_query"> </a>

搜索结果表示字符串值或值范围形式的精简选项。每个字符串值或数值范围称为细化箱，每个细化箱具有一个关联的 **RefinementToken** 值。精简选项与 **RefinerName** 值提供的托管属性关联。
  
    
    
连接 **RefinementToken** 和 **RefinerName** 值以创建 _refinement filter_ 字符串。此字符串表示一个筛选器，该筛选器可用于将搜索结果项限制为仅包含这样的项目：项目中的托管属性具有细化箱中的值。简言之：
  
    
    
 `refinement filter = <RefinerName>:<RefinementToken>`
  
    
    
你可以将精简筛选器添加到  [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.aspx) 类的 [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) 属性中，为精简的查询提供一个或多个精简筛选器。通过多个精简筛选器，你可以提供对搜索结果的深入了解，并在多值属性中应用细化。例如，你可以将查询优化到具有两个作者（每个作者由细化箱表示）的项目，排除仅包含其中一个作者的项目。
  
    
    

### 示例 1：为 HTML 文件类型创建精简的查询

以下 CSOM 示例显示如何以编程方式执行细化，以将搜索结果限制为只包含 HTML 文件类型。如 [示例：精简数据](#SP15_Example_refinement_data) 中所述，与此精简选项关联的精简数据已将 **RefinerName** 设置为 _Filetype_，将 **RefinementToken** 设置为 _"ǂǂ68746d6c"_。
  
    
    

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


### 示例 2：使用之前获得的精简数据创建精简的查询

以下 CSOM 示例显示如何使用精简条件规格运行查询，以创建后续用于执行细化的精简数据。此示例模拟最终用户选择第一个精简选项的过程。
  
    
    

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


## 其他资源
<a name="SP15_Query_refinement_addresources"> </a>


-  [SharePoint Server 2013 中的精简 Web 部件的配置属性](http://technet.microsoft.com/zh-cn/library/gg549985.aspx)
    
  
-  [SharePoint Server 2013 中的搜索架构概述](http://technet.microsoft.com/zh-cn/library/jj219669%28office.15%29.aspx)
    
  

## 另请参阅
<a name="SP15_Query_refinement_addresources"> </a>


#### 其他资源


  
    
    
 [索引架构 (FAST Search Server for SharePoint)](http://msdn.microsoft.com/library/3e1eed3f-8a6f-4911-9607-f1616e6a44f3%28Office.15%29.aspx)
  
    
    
 [Microsoft.Search.Query 架构中的 Refiner 元素](http://msdn.microsoft.com/library/0512c17a-18e9-45e3-9061-9dba9cd45ef4%28Office.15%29.aspx)
