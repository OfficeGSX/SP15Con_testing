---
title: Perfectionnement de requête dans SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: ec31782e-1bc5-4dc3-8df7-c29cd5f7f05c
---



# Perfectionnement de requête dans SharePoint 2013
Apprenez à utiliser les fonctionnalités d'affinement des requêtes SharePoint Server 2013 par programme dans les requêtes de recherche et les résultats.
Vous pouvez utiliser les fonctionnalités d'affinement de requête pour fournir à l'utilisateur final des options d'affinement pertinentes pour ses requêtes. Ces fonctionnalités permettent à l'utilisateur final d'affiner les résultats de la recherche à l'aide de données d'affinement calculées pour les résultats. Les données d'affinement sont calculées par le composant d'index, en fonction de l'agrégation des statistiques de propriété gérée pour tous les résultats d'une requête de recherche.
  
    
    

L'affinement de requêtes est généralement utilisé pour les métadonnées associées aux éléments indexés, comme la date de création, l'auteur ou les types de fichier qui apparaissent dans l'élément. Grâce aux options d'affinement, vous pouvez affiner votre requête pour ne présenter que les éléments créés sur une période donnée ou afficher uniquement les éléments d'un certain type de fichier.
## Utilisation des affinements dans le modèle objet requête
<a name="SP15_Using_refiners"> </a>

Il existe deux requêtes impliquées dans l'affinement de requête :
  
    
    

1. Vous pouvez demander qu'un ensemble d'affinements soit renvoyé dans les résultats de recherche en  [ajoutant une spécification d'affinement](#SP15_Adding_refiners) à la requête de l'utilisateur final. Une spécification d'affinement est l'entrée de la propriété [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) . Cette requête est exécutée sur l'index de recherche. Les résultats de la recherche seront composés de résultats pertinents et de données d'affinement.
    
  
2. Vous pouvez utiliser les données d'affinement pour affiner les résultats de la recherche en  [créant une requête affinée](#SP15_Creating_refined_query). Ajoutez la propriété  [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) à la requête afin que les résultats finaux de la recherche répondent aux exigences du texte de la requête d'origine de l'utilisateur final et à l'option d'affinement sélectionnée dans les données d'affinement.
    
  
Les sections suivantes décrivent ces étapes en détail et fournissent des exemples de code.
  
    
    

## Ajout d'affinements à la requête de l'utilisateur final à l'aide des spécifications d'affinement
<a name="SP15_Adding_refiners"> </a>

Vous pouvez spécifier les affinements de requête demandés à l'aide de la propriété  [Refiners](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.Refiners.aspx) de la classe [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.KeywordQuery.aspx) . Pour spécifier les affinements de requête demandés, utilisez la syntaxe suivante :
  
    
    
 `<refiner>[,<refiner>]*`
  
    
    

  
    
    
Chaque affinement («  `refiner` ») présente le format suivant :
  
    
    
 `<refiner-name>[(parameter=value[,parameter=value]*)]?`
  
    
    

  
    
    
Où :
  
    
    

-  `<refiner-name>` est le nom de la propriété gérée associée à l'affinement. Cette propriété gérée doit être définie sur [Refinable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Refinable.aspx) ou [Sortable](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Administration.ManagedPropertyInfo.Sortable.aspx) dans le schéma de recherche.
    
  
- La liste facultative de paires  `parameter=value` spécifie des valeurs de configuration non définies par défaut pour l'affinement nommé. Si un paramètre d'un affinement n'est pas répertorié dans les parenthèses, la configuration du schéma de recherche vous donne la valeur par défaut. Le tableau 1 répertorie les valeurs possibles pour les paires `parameter=value`.
    
  

> **REMARQUE**
> Lorsque vous spécifiez des affinements, l'exigence minimale consiste à spécifier un nom d'affinement («  `refiner-name` »), qui prend la forme d'une propriété gérée.<br/> **Example**<br/> `Refiners = "FileType"`<br/> <br/> Sinon, vous pouvez également utiliser la syntaxe avancée pour ajuster les paramètres d'affinement. <br/> **Example**<br/> `Refiners = "FileType,Write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22),companies"`
  
    
    


**Tableau 1 : Liste des paramètres pour les affinements**


|**Paramètre**|**Description**|
|:-----|:-----|
| _deephits_ <br/> |Remplace le nombre d'accès par défaut qui est utilisé comme base pour le calcul des affinements. Lors de la production des affinements, tous les résultats de la requête seront évalués. Normalement, le fait d'utiliser ce paramètre améliorera les performances de recherche.  <br/> **Syntax**          `deephits=<integer value>` <br/> **Example**          `price(deephits=1000)` <br/> <br/> **REMARQUE**<br/> Cette limite s'applique au sein de chaque partition d'index. Le nombre réel d'accès évalués sera supérieur à cette valeur en raison de l'agrégation entre les partitions de recherche.           |
| _discretize_ <br/> | Spécifie des intervalles personnalisés (bacs d'affinement) pour les affinements numériques. <br/> **Syntax**          `discretize=manual/<threshold>/<threshold>[/<threshold>]*` <br/> **Example**          `write(discretize=manual/2013-01-01/2013-08-22/2013-09-15/2013-09-21/2013-09-22)` <br/>  L'attribut `<threshold>` spécifie le seuil pour chaque bac d'affinement. <br/>  Un seul intervalle est appliqué à l'ensemble des éléments sous le premier seuil spécifié, entre chaque seuil consécutif et pour l'ensemble des éléments au-dessus du dernier seuil. <br/>  Pour un affinement de type **DateTime**, spécifiez le seuil en fonction de l'un des formats conformes à la norme ISO 8601 suivants :  <br/>  _YYYY-MM-DD_ <br/>  _YYYY-MM-DDThh:mm:ss_ <br/>  _YYYY-MM-DDThh:mm:ss:Z_ <br/>  Les valeurs de format spécifient les éléments suivants : <br/>  _YYYY_: année à quatre chiffres. Seuls les années à quatre chiffres sont prises en charge. <br/>  _MM_: mois à deux chiffres. 01 = janvier. <br/>  _DD_: jour du mois à deux chiffres (de 01 à 31). <br/>  _T_: la lettre « T ». <br/>  _hh_: heure à deux chiffres (de 00 à 23). L'indication A.M. ou P.M. n'est pas autorisée. <br/>  _mm_: minute à deux chiffres (de 00 à 59). <br/>  _ss_: seconde à deux chiffres (de 00 à 59). <br/> <br/> **IMPORTANTE**<br/>  Toutes les valeurs de date/d'heure doivent être spécifiées conformément au temps universel coordonné (UTC), aussi connu sous le nom d'heure de Greenwich (GMT). L'identificateur de fuseau horaire UTC (caractère « Z » de fin) est facultatif.          |
| _sort_ <br/> | Définit l'ordre de tri des bacs au sein d'un affinement de chaîne. <br/> **Syntax**          `sort=<property>/<direction>` <br/>  Les attributs effectuent les opérations suivantes : <br/>  _<property>_: spécifie l'algorithme de tri. Ces valeurs en minuscules sont valides : <br/> **frequency**: trie par occurrence dans les bacs. <br/> **name**: trie par nom d'étiquette. <br/> **number**: traite les chaînes comme des chaînes numériques et utilise le tri numérique. Cette valeur peut être utile pour spécifier des valeurs distinctes, par exemple, pour effectuer un tri numérique d'une valeur contenue dans une propriété gérée de type **String**.  <br/>  _<direction>_: spécifie l'ordre de tri. Ces valeurs en minuscules sont valides : <br/> **descending** <br/> **ascending** <br/> **Example**          `sort=name/ascending` <br/> **Default**: frequency/descending <br/> |
| _filter_ <br/> | Définit la manière dont les bacs au sein d'un affinement de type **String** sont filtrés avant d'être renvoyés au client. <br/> **Syntax**          `filter=<bins>/<freq>/<prefix>[<levels>]` <br/>  Les attributs effectuent les opérations suivantes : <br/>  _<bins>_: spécifie le nombre maximal de bacs renvoyés. <br/>  Après avoir trié les bacs au sein de l'affinement, utilisez cet attribut pour tronquer les bacs à droite. Par exemple, si bins=10, seuls les 10 premiers bacs sont renvoyés, conformément à l'algorithme de tri spécifié. <br/>  _<freq>_: limite le nombre de bacs renvoyés. <br/>  Cet attribut permet de supprimer des bacs ayant une fréquence faible. Par exemple, _freq_=2 indique que seuls les bacs avec 2 membres ou plus sont renvoyés.  <br/>  _<prefix>_: spécifie que seuls les bacs dont le nom commence par ce préfixe sont renvoyés. <br/>  Le caractère générique « * » renvoie à tous les noms. <br/> **Example** <br/>  `filter=30/2/*` <br/>  _<levels>_: spécifie les niveaux de taxonomie. <br/>  Cet attribut permet de filtrer la sortie d'affinement hiérarchique en fonction des niveaux de taxonomie. L'attribut doit être un entier positif _n_. La valeur **default** est _n_=0. Si  _n_>0, seules les entrées d'affinement qui contiennent moins de  _n_ symboles de séparateur de chemin d'accès de taxonomie (« / ») seront renvoyées. Si _n_=0, aucun filtrage de niveau n'est effectué. Le séparateur de niveau est la barre oblique (« / »).  <br/>  N'oubliez pas que l'utilisation d'un navigateur de taxonomie volumineux a un impact sur les performances. Le filtrage est effectué dans le cadre du traitement des résultats. <br/> <br/> **REMARQUE**>  Ce paramètre applique un filtrage des résultats spécifique de l'application. Il diffère du paramètre cutoff car ce dernier applique les limites dans chaque partition d'index pour des raisons de performances.          |
| _cutoff_ <br/> | Limite les données qui doivent être transférées et traitées pour les affinements de chaîne en profondeur. Vous pouvez configurer les affinements pour renvoyer uniquement les valeurs les plus pertinentes (bacs). <br/> <br/> **REMARQUE**<br/>  Ce filtrage à l'aide du paramètre cutoff est effectué dans chaque partition d'index. Il diffère du paramètre _filter_, car ce dernier applique un filtrage des résultats uniquement. Vous pouvez combiner les deux paramètres.           **Syntax**          `cutoff=<frequency>/<minbins>/<maxbins>` <br/>  Les attributs effectuent les opérations suivantes : <br/>  _<maxbins>_: limite le nombre de bacs. <br/>  Ce paramètre limite le nombre de valeurs uniques (bacs) qui seront renvoyées pour un affinement. Il s'agit de la manière la plus utilisée d'améliorer les performances de recherche lorsque des affinements de chaîne avec un grand nombre de bacs sont renvoyés. « 0 » indique que la valeur par défaut spécifiée dans le schéma de recherche est utilisée. <br/>  _<frequency>_: limite le nombre de bacs par fréquence. <br/>  Si le nombre d'occurrences d'une valeur d'affinement dans un jeu de résultats est inférieur ou égal à la valeur de la fréquence, la valeur d'affinement n'est pas renvoyée. « 0 » indique que la valeur par défaut selon le schéma de recherche est utilisée. <br/>  _<minbins>_: spécifie la valeur minimale du seuil de fréquence. <br/>  Si le nombre de valeurs d'affinement uniques de la requête est inférieur à cette valeur, aucun seuil de fréquence n'est appliqué, et toutes les valeurs d'affinement de cette partition de recherche sont renvoyées. Lorsque le seuil par fréquence est appliqué, ce paramètre peut être utilisé pour spécifier un nombre minimal de valeurs d'affinement uniques qui seront renvoyées, quel que soit le nombre d'occurrences. La valeur par défaut de cet attribut est « 0 », ce qui indique que le seuil de fréquence est appliqué, quel que soit le nombre de valeurs de navigateur uniques. « 0 » indique que la valeur par défaut spécifiée dans le schéma d'index est utilisée. <br/> <br/> **REMARQUE**>  Utilisez les paramètres _<frequency>_ et _<minbins>_ avec précaution. Nous vous recommandons d'utiliser uniquement _<maxbins>_.           |
   

### Exemple : ajout d'affinements
<a name="SP15_Example_adding_refiners"> </a>

L'exemple de modèle objet client suivant montre comment demander par programme trois affinements : **FileType**, **Write** et **Companies**. **Write** représente la date de dernière modification de l'élément et utilise la syntaxe avancée pour renvoyer les bacs de date/heure de taille fixe.
  
    
    

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


## Explication des données d'affinement que vous obtenez dans le résultat de la recherche
<a name="SP15_Understanding_refinement_data"> </a>

Si vous avez activé l'affinement de requête pour une propriété gérée dans votre requête, le résultat de la requête contient des données d'affinement fractionnées en bacs d'affinement. Ces données se trouvent dans la table **RefinementResults** ( [RefinementResults](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KnownTableTypes.RefinementResults.aspx) ) dans [ResultTableCollection](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.ResultTableCollection.aspx) . Un bac d'affinement représente une valeur ou une plage de valeurs spécifique de la propriété gérée. La table **RefinementResults** contient une ligne par bacs d'affinement et contient les colonnes, comme spécifié dans tableau 2.
  
    
    

**Tableau 2 : données renvoyées pour les bacs d'affinement**


|**Paramètre**|**Description**|
|:-----|:-----|
|**RefinerName** <br/> |Nom de l'affinement de requête.  <br/> |
|**RefinementName** <br/> |Chaîne qui représente le bac d'affinement. Cette chaîne est généralement utilisée lorsque vous présentez les options d'affinement aux utilisateurs sur une page de résultats de recherche.  <br/> |
|**RefinementValue** <br/> |Chaîne mise en forme en fonction de l'implémentation et qui représente l'affinement. Ces données sont renvoyées pour le débogage et ne sont généralement pas requises pour le client.  <br/> |
|**RefinementToken** <br/> |Une chaîne qui représente le bac d'affinement à utiliser avec **RefinerName** lorsque vous effectuez une requête affinée. <br/> |
|**RefinementCount** <br/> |Nombre de résultats pour ce bac d'affinement. Ces données représentent le nombre d'éléments (y compris les doublons) dans le résultat de la recherche avec une valeur pour la propriété gérée donnée qui correspond à ce bac d'affinement.  <br/> |
   

### Exemple : données d'affinement
<a name="SP15_Example_refinement_data"> </a>

Le tableau 3 ci-dessous contient deux lignes de données d'affinement. La première ligne présente les données d'affinement pour les éléments indexés, où le type de fichier est HTML. La deuxième ligne présente les données d'affinement pour les éléments indexés, où les heures de dernière modification s'échelonnent du 21/09/2013 au 22/09/2013.
  
    
    

**Tableau 3 : format et contenu des données d'affinement**


|**RefinerName**|**RefinementName**|**RefinementValue**|**RefinementToken**|**RefinementCount**|
|:-----|:-----|:-----|:-----|:-----|
|FileType  <br/> |Html  <br/> |Html  <br/> |"??68746d6c"  <br/> |50553  <br/> |
|Écriture  <br/> |De 2013-09-21T00:00:00Z à 2013-09-22T00:00:00Z  <br/> |De 2013-09-21T00:00:00Z à 2013-09-22T00:00:00Z  <br/> |range(2013-09-21T00:00:00Z, 2013-09-22T00:00:00Z)  <br/> |37  <br/> |
   

## Création d'une requête affinée
<a name="SP15_Creating_refined_query"> </a>

Un résultat de recherche présente des options d'affinement sous la forme de valeurs de chaîne ou de plages de valeurs. Chaque plage de valeurs numériques ou valeur de chaîne est appelée bac d'affinement, et chaque bac d'affinement est associé à une valeur **RefinementToken**. Une option d'affinement est associée à une propriété gérée, qui est fournie par la valeur **RefinerName**.
  
    
    
Les valeurs **RefinementToken** et **RefinerName** sont toutes deux concaténées pour créer une chaîne _refinement filter_. Cette chaîne représente un filtre qui peut être utilisé pour limiter les éléments de résultat de recherche et n'inclure que les éléments où une propriété gérée a une valeur comprise dans un bac d'affinement. Pour résumer :
  
    
    
 `refinement filter = <RefinerName>:<RefinementToken>`
  
    
    
Vous pouvez fournir un ou plusieurs filtres d'affinement pour une requête affinée en ajoutant des filtres d'affinement à la propriété  [RefinementFilters](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.RefinementFilters.aspx) de la classe [KeywordQuery](https://msdn.microsoft.com/library/Microsoft.Office.Server.Search.Query.KeywordQuery.aspx) . Plusieurs filtres d'affinement permettent de fournir un affinement à plusieurs niveaux dans les résultats de recherche et d'appliquer l'affinement sur des propriétés à valeurs multiples. Par exemple, vous pouvez affiner la requête pour qu'elle ne renvoie que les éléments qui ont deux auteurs précis (chacun étant représenté par un bac d'affinement) mais exclure les éléments qui n'ont que l'un des deux auteurs.
  
    
    

### Exemple 1 : création d'une requête affinée pour les types de fichiers HTML

L'exemple de modèle objet client suivant montre comment exécuter par programme un affinement pour limiter les résultats de recherche à ceux dont le type de fichier est HTML. Comme indiqué dans le  [Exemple : données d'affinement](#SP15_Example_refinement_data), pour les données d'affinement associées à cette option d'affinement, le paramètre **RefinerName** est défini sur _Filetype_ et le paramètre **RefinementToken** est défini sur _"ǂǂ68746d6c"_.
  
    
    

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


### Exemple 2 : création d'une requête affinée à l'aide de données d'affinement obtenues précédemment

L'exemple de modèle objet client suivant montre comment exécuter une requête avec une spécification d'affinement pour créer des données d'affinement qui sont ensuite utilisées pour exécuter l'affinement. Cet exemple simule le processus d'un utilisateur final qui sélectionne la première option d'affinement.
  
    
    

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


## Ressources supplémentaires
<a name="SP15_Query_refinement_addresources"> </a>


-  [Configurer les propriétés du composant WebPart d'affinement dans SharePoint Server 2013](http://technet.microsoft.com/fr-fr/library/gg549985.aspx)
    
  
-  [Vue d'ensemble du schéma de recherche dans SharePoint Server 2013](http://technet.microsoft.com/fr-fr/library/jj219669%28office.15%29.aspx)
    
  

## Voir aussi
<a name="SP15_Query_refinement_addresources"> </a>


#### Autres ressources


  
    
    
 [Schéma d'index (FAST Search Server for SharePoint)](http://msdn.microsoft.com/library/3e1eed3f-8a6f-4911-9607-f1616e6a44f3%28Office.15%29.aspx)
  
    
    
 [Élément d'affinement dans le schéma Microsoft.Search.Query](http://msdn.microsoft.com/library/0512c17a-18e9-45e3-9061-9dba9cd45ef4%28Office.15%29.aspx)
