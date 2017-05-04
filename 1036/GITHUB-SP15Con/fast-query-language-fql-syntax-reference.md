---
title: Référence de la syntaxe du langage de requête FQL (FAST Query Language)
ms.prod: SHAREPOINT
ms.assetid: bd98a41b-623c-41d4-a15d-26c0d4ba4311
---



# Référence de la syntaxe du langage de requête FQL (FAST Query Language)
Découvrez comment créer des requêtes de recherche complexes pour Recherche dans SharePoint 2013 à l'aide du langage FAST Query Language (FQL). Cette référence décrit les éléments d'une requête FQL et la façon d'utiliser les spécifications de propriété, les expressions de jeton et les opérateurs dans vos requêtes FQL.
## Introduction à FQL et aux expressions et sous-expressions de langage de requête dans SharePoint Server 2013
<a name="SP15FQL_about"> </a>

FAST Query Language (FQL) est un langage de requête puissant qui permet aux développeurs d'effectuer des recherches exactes et de réduire l'étendue de votre recherche aux valeurs qui appartiennent à une propriété gérée ou un index de recherche en texte intégral spécifique.
  
    
    
Une expression de langage de requête peut contenir des sous-expressions imbriquées qui comprennent les termes de la requête, des spécifications de propriété et des opérateurs, comme décrit dans le tableau 1.
  
    
    

**Tableau 1. Sous-expressions dans les expressions de langage de requête**


|**Élément**|**Description**|
|:-----|:-----|
|Expressions de jeton  <br/> |Un ou plusieurs termes de requête, expressions ou valeurs numériques à rechercher dans une requête.  <br/> |
|Spécification de propriété  <br/> |Propriété ou index de recherche en texte intégral à mettre en correspondance avec l'expression concernée.  <br/> |
|Opérateurs  <br/> |Mots clés qui spécifient des opérations booléennes (telles que **AND**, **OR**) ou autres contraintes s'appliquant aux opérandes (telles que **FILTER**).  <br/> |
   

### Exemple de requête FQL

L'exemple de requête FQL suivant recherche les termes « hello » et « world » dans la propriété gérée **body** d'un élément indexé :
  
    
    
 `body:string("hello world", mode="and")`
  
    
    
Dans l'exemple :
  
    
    

-  `body:` limite la portée de la requête à la propriété gérée body dans l'élément ;
    
  
-  `"hello world"` est l'opérande de l'opérateur **STRING**, qui indique les termes à rechercher ;
    
  
-  `mode="and"` indique que l'opérateur de requête logique **AND** sera appliqué à `"hello world"`.
    
  
Les requêtes FAST Query Language sont limitées à une longueur de 2 048 caractères.
  
    
    

## Spécification de propriété dans le langage FQL
<a name="property_specification"> </a>

Une spécification de propriété limite l'étendue de l'expression concernée à des zones spécifiques du contenu indexé. Ces zones peuvent être identifiées par un index de recherche en texte intégral ou une propriété gérée. 
  
    
    
Les propriétés gérées de type **Text** et **YesNo** sont évaluées en tant que texte. Tous les autres types de propriétés gérées, y compris le type **Datetime**, sont évalués en tant que valeurs numériques.
  
    
    
Si vous n'incluez pas de spécification de propriété pour une expression, le moteur de recherche tente de mettre en correspondance l'index de recherche en texte intégral par défaut défini dans le schéma d'index.
  
    
    
Le nom de la propriété doit toujours précéder le signe deux points (opérateur **In**) et les opérateurs numériques doivent toujours inclure une spécification de propriété.
  
    
    
Une spécification de propriété (opérateur **In**) peut être appliquée aux entités de requête suivantes :
  
    
    

- Un seul terme ou une seule expression, comme suit :
    
     `author:shakespeare`
  
    
    
 `title:"to be or not to be"`
    
  
- Un opérateur, par exemple l'opérateur **STRING**, comme suit :
    
  ```
  
  title:string("to be or not to be")
  ```


    Dans ce cas, la spécification de propriété s'applique à l'expression d'opérateur complète.
    
  

### Exemples

Chacune des expressions suivantes correspond à des éléments qui contiennent à la fois « much » et « nothing » dans la propriété gérée **title**.
  
    
    
 `title:and(much, nothing)`
  
    
    
 `and(title:much, title:nothing)`
  
    
    
 `title:string("much nothing", mode="and")`
  
    
    

## Expressions de jeton dans le langage FQL
<a name="token_expressions"> </a>

Les expressions de jeton sont des mots, des expressions ou des valeurs numériques qui sont mis en correspondance avec l'index.
  
    
    
Une expression de jeton de texte peut être un seul mot ou une expression entre guillemets doubles.
  
    
    
Une expression de jeton numérique peut être une valeur unique ou une expression de plage de valeurs.
  
    
    

### Expressions génériques

Une expression générique indique un seul mot ou une expression qui inclut un astérisque (« ***** ») ; l'astérisque signifie une correspondance de zéro, un ou plusieurs caractères, excepté l'espace blanc. FQL prend en charge la recherche de préfixe pour les propriétés gérées de texte individuel et les index de recherche en texte intégral.
  
    
    

#### Exemples d'expressions génériques

Voici la liste des utilisations valides des expressions génériques dans le langage FQL :
  
    
    

-  `text*`
    
  
-  `string("this examp*")`
    
  

### Expressions de terme numérique
<a name="fql_token_numeric"> </a>

Chaque expression de terme numérique doit inclure une spécification de propriété d'un type de données de schéma d'index compatible. Le tableau 2 répertorie les types de données numériques pouvant être utilisés dans le langage FQL. 
  
    
    

**Tableau 2. Types de données numériques pouvant être utilisés dans le langage FQL**


|**Type FQL**|**Types de schéma d'index compatibles**|**Description**|
|:-----|:-----|:-----|
|**Int** <br/> |**Integer** <br/> |Entier 64 bits.  <br/> |
|**Float** <br/> |**Double** <br/> |Nombre 64 bits (double précision) à virgule flottante.  <br/> |
|**Decimal** <br/> |**Decimal** <br/> |Nombre décimal 128 bits.  <br/> |
|**Datetime** <br/> |**Datetime** <br/> |Valeur de date et d'heure.  <br/> La prise en charge de date/heure dans le langage FQL permet les mêmes opérations numériques sur les valeurs de date/heure que sur les autres valeurs numériques.  <br/> |
   

#### Expressions de requête de date et heure
<a name="fql_token_datetime"> </a>

Le langage FQL fournit le type de données **datetime** pour la date et l'heure.
  
    
    
Les formats **datetime** compatibles ISO 8601 suivants sont pris en charge dans les requêtes :
  
    
    

- YYYY-MM-DD 
    
  
- YYYY-MM-DDThh:mm:ss 
    
  
- YYYY-MM-DDThh:mm:ssZ 
    
  
- YYYY-MM-DDThh:mm:ssfrZ
    
  
Dans ces formats **datetime**:
  
    
    

-  _YYYY_ spécifie une année à quatre chiffres.
    
    > **REMARQUE**
      > Seules les années à quatre chiffres sont prises en charge. 
-  _MM_ spécifie un mois à deux chiffres. Par exemple, 01 = janvier.
    
  
-  _DD_ spécifie un jour du mois à deux chiffres (01 à 31).
    
  
-  _T_ spécifie la lettre « T ».
    
  
-  _hh_ spécifie une heure à deux chiffres (00 à 23) ; l'indication AM/PM n'est pas autorisée.
    
  
-  _mm_ spécifie une minute à deux chiffres (00 à 59).
    
  
-  _ss_ spécifie une seconde à deux chiffres (00 à 59).
    
  
-  _fr_ spécifie une fraction facultative de secondes, _ss_, entre 1 et 7 chiffres suivant le **.** après les secondes. Par exemple, 2012-09-27T11:57:34.1234567.
    
  
Toutes les valeurs de date/d'heure doivent être spécifiées suivant l'UTC (temps universel coordonné), aussi connu comme heure de Greenwich (GMT). L'identificateur de fuseau horaire UTC (caractère « Z » de fin) est facultatif.
  
    
    

### Mots réservés, caractères spéciaux et échappement
<a name="fql_token_numeric"> </a>

Les mots suivants sont réservés dans le langage FQL.
  
    
    
 `and, or, any, andnot, count, decimal, rank, near, onear, int, in32, int64, float, double, datetime, max, min, range, phrase, scope, filter, not, string, starts-with, ends-with, equals, words, xrank.`
  
    
    
Si vous voulez exprimer l'un de ces mots en tant que terme dans une expression de requête, vous devez l'entourer de guillemets doubles, comme indiqué dans les exemples suivants : 
  
    
    

-  `or("any", "and", "xrank")`
    
  
-  `string("any and xrank", mode="OR")`
    
  
-  `phrase(this, is, a, "phrase")`
    
  

> **CONSEIL**
> Les mots et les caractères réservés ne respectent pas la casse, mais l'utilisation de minuscules est recommandée en vue d'une compatibilité future. 
  
    
    

Le langage FQL n'exige pas toujours qu'une chaîne soit entourée de guillemets doubles. Par exemple,  `and(cat, dog)` correspond à du langage FQL valide, même si `cat` et `dog` ne sont pas entre guillemets doubles. Toutefois, nous vous recommandons d'utiliser des guillemets doubles pour éviter les conflits avec les mots réservés.
  
    
    
Les termes de la requête sont tokenisés selon vos paramètres régionaux. Le processus de création de jetons supprime certains caractères spéciaux. Étant donné que les caractères spéciaux sont supprimés, les expressions FQL suivantes sont équivalentes.
  
    
    
 `and("[king]", "<queen>")`
  
    
    
 `and("king", "queen")`
  
    
    
Lorsqu'une requête comprend des termes provenant d'une entrée utilisateur ou d'une autre application, utilisez l'opérateur  `string("<query terms>", mode="AND|OR|PHRASE")` pour éviter tout conflit avec des mots réservés dans le langage de requête. Vous devez également supprimer les éventuels guillemets doubles de la requête fournie par l'utilisateur.
  
    
    

## Opérateurs FQL
<a name="fql_operators"> </a>

Les opérateurs FAST Query Language (FQL) sont des mots clés qui spécifient les opérations booléennes ou d'autres contraintes relatives aux opérandes. La syntaxe d'opérateur FQL est la suivante :
  
    
    
 `[property-spec:]operator(operand [,operand]* [, parameter="value"]*)`
  
    
    
Dans la syntaxe :
  
    
    

-  _property-spec_ est une spécification de propriété facultative suivie de l'opérateur « in ».
    
  
-  _operator_ est un mot clé qui spécifie une opération à effectuer.
    
  
-  _operand_ est une expression de terme ou un autre opérateur.
    
  
-  _parameter_ est le nom d'une valeur qui modifie le comportement de l'opérateur.
    
  
-  _value_ est la valeur à utiliser pour le nom du paramètre.
    
  
Les noms d'opérateur et de paramètre et les valeurs de texte des paramètres ne respectent pas la casse. L'espace blanc est autorisé dans le corps de l'opérateur, mais il est ignoré à moins qu'il ne soit entouré de guillemets doubles. Les requêtes FAST Query Language sont limitées à une longueur de 2 048 caractères.
  
    
    
Le tableau 3 répertorie les types d'opérateurs pris en charge par FQL. 
  
    
    

**Tableau 3. Types d'opérateurs pris en charge par FQL**


|**Type**|**Description**|**Opérateurs**|
|:-----|:-----|:-----|
|String  <br/> |Vous permet de spécifier des opérations de requête sur une chaîne de termes. Il s'agit de l'opérateur le plus couramment utilisé sur les termes de texte.  <br/> | [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator) <br/> |
|Valeur booléenne  <br/> |Vous permet de combiner des termes et des sous-expressions dans une requête.  <br/> | [AND](fast-query-language-fql-syntax-reference.md#fql_and_operator),  [OU](fast-query-language-fql-syntax-reference.md#fql_or_operator),  [ANY](fast-query-language-fql-syntax-reference.md#fql_any_operator),  [ANDNOT](fast-query-language-fql-syntax-reference.md#fql_andnot_operator),  [NOT](fast-query-language-fql-syntax-reference.md#fql_not_operator),  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator),  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator) <br/> |
|Proximité  <br/> |Vous permet de spécifier la proximité des termes de la requête dans une séquence de texte correspondante.  <br/> | [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator),  [ONEAR](fast-query-language-fql-syntax-reference.md#fql_onear_operator),  [PHRASE](fast-query-language-fql-syntax-reference.md#fql_phrase_operator),  [STARTS-WITH](fast-query-language-fql-syntax-reference.md#fql_startswith_operator),  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator),  [EQUALS](fast-query-language-fql-syntax-reference.md#fql_equals_operator) <br/> |
|Numérique  <br/> |Vous permet de spécifier des conditions numériques dans la requête.  <br/> | [RANGE](fast-query-language-fql-syntax-reference.md#fql_range_operator) , [INT](fast-query-language-fql-syntax-reference.md#fql_int_operator),  [FLOAT](fast-query-language-fql-syntax-reference.md#fql_float_operator),  [DATETIME](fast-query-language-fql-syntax-reference.md#fql_datetime_operator),  [DECIMAL](#fql_decimal_operator) <br/> |
|Pertinence  <br/> |Vous permet d'avoir un impact sur l'évaluation de la pertinence d'une requête.  <br/> | [XRANK](fast-query-language-fql-syntax-reference.md#fql_xrank_operator) et [FILTER](fast-query-language-fql-syntax-reference.md#fql_filter_operator) <br/> |
   
Le tableau 4 fournit la liste des opérateurs pris en charge.
  
    
    

**Tableau 4. Opérateurs pris en charge par FQL**


|**Opérateur**|**Description**|**Type**|
|:-----|:-----|:-----|
| [AND](fast-query-language-fql-syntax-reference.md#fql_and_operator) <br/> |Renvoie uniquement les éléments qui correspondent à tous les opérandes **AND**. <br/> |Valeur booléenne  <br/> |
| [ANDNOT](fast-query-language-fql-syntax-reference.md#fql_andnot_operator) <br/> |Renvoie uniquement les éléments qui correspondent au premier opérande et qui ne correspondent pas aux opérandes suivants.  <br/> |Valeur booléenne  <br/> |
| [ANY](fast-query-language-fql-syntax-reference.md#fql_any_operator) <br/> |Semblable à l'opérateur **OR**, mais le rang dynamique (score de pertinence dans le jeu de résultats) n'est concerné ni par le nombre d'opérandes correspondants ni par la distance entre les termes dans l'élément. <br/> |Valeur booléenne  <br/> |
| [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator) <br/> |Vous permet de spécifier le nombre d'occurrences de terme de requête qu'un élément doit inclure pour être renvoyé comme résultat. L'opérande peut être un seul terme de requête, une expression ou un terme de requête générique.  <br/> |Valeur booléenne  <br/> |
| [DATETIME](fast-query-language-fql-syntax-reference.md#fql_datetime_operator) <br/> |Fournit un typage explicite des valeurs numériques.  <br/> La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.  <br/> |Numérique  <br/> |
| [DECIMAL](fast-query-language-fql-syntax-reference.md#fql_decimal_operator) <br/> |Fournit un typage explicite des valeurs numériques.  <br/> La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.  <br/> |Numérique  <br/> |
| [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator) <br/> |Spécifie qu'un mot ou une expression doit apparaître à la fin d'une propriété gérée.  <br/> |Proximité  <br/> |
| [EQUALS](fast-query-language-fql-syntax-reference.md#fql_equals_operator) <br/> |Spécifie qu'un mot, un terme d'expression ou une expression doit fournir une correspondance de jeton exacte avec la propriété gérée.  <br/> |Proximité  <br/> |
| [FILTER](fast-query-language-fql-syntax-reference.md#fql_filter_operator) <br/> |Permet d'interroger des métadonnées ou d'autres données structurées.  <br/> |Pertinence  <br/> |
| [FLOAT](fast-query-language-fql-syntax-reference.md#fql_float_operator) <br/> |Fournit un typage explicite des valeurs numériques.  <br/> La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.  <br/> |Numérique  <br/> |
| [INT](fast-query-language-fql-syntax-reference.md#fql_int_operator) <br/> |Fournit un typage explicite des valeurs numériques.  <br/> La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.  <br/> |Numérique  <br/> |
| [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator) <br/> |Restreint le jeu de résultats aux éléments qui ont  `N` termes à une certaine distance les uns des autres. <br/> |Proximité  <br/> |
| [NOT](fast-query-language-fql-syntax-reference.md#fql_not_operator) <br/> |Renvoie uniquement les éléments qui excluent l'opérande.  <br/> |Valeur booléenne  <br/> |
| [ONEAR](fast-query-language-fql-syntax-reference.md#fql_onear_operator) <br/> |Variante ordonnée de **NEAR**, exige une correspondance ordonnée des termes. Vous pouvez utiliser l'opérateur **ONEAR** pour restreindre le jeu de résultats aux éléments qui ont `N` termes à une certaine distance les uns des autres. Cet opérateur renvoie uniquement les éléments qui ne correspondent pas à l'opérande. L'opérande peut être n'importe quelle expression FQL valide. <br/> |Proximité  <br/> |
| [OU](fast-query-language-fql-syntax-reference.md#fql_or_operator) <br/> |Renvoie uniquement les éléments qui correspondent à au moins l'un des opérandes **OR**. Les éléments correspondants obtiendront un rang dynamique plus élevé (score de pertinence dans le jeu de résultats) si plusieurs opérandes **OR** sont mis en correspondance. <br/> |Valeur booléenne  <br/> |
| [PHRASE](fast-query-language-fql-syntax-reference.md#fql_phrase_operator) <br/> | Renvoie uniquement les éléments qui correspondent à une chaîne exacte de jetons. <br/> |Proximité  <br/> |
| [RANGE](fast-query-language-fql-syntax-reference.md#fql_range_operator) <br/> | Active les expressions de correspondance de plage. L'opérateur **RANGE** est utilisé pour les propriétés gérées numériques et de date/heure. <br/> |Numérique  <br/> |
| [STARTS-WITH](fast-query-language-fql-syntax-reference.md#fql_startswith_operator) <br/> |Spécifie qu'un mot ou une expression doit apparaître au début d'une propriété gérée.  <br/> |Proximité  <br/> |
| [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator) <br/> |Permet de définir une condition de correspondance booléenne avec une chaîne de texte.  <br/> |String  <br/> |
| [XRANK](fast-query-language-fql-syntax-reference.md#fql_xrank_operator) <br/> |Vous permet d'augmenter le rang dynamique des éléments en fonction de certaines occurrences de termes sans modifier les éléments qui correspondent à la requête. Une expression **XRANK** contient un composant qui doit être mis en correspondance et des composants qui contribuent uniquement au classement dynamique. <br/> |Pertinence  <br/> |
   

> **REMARQUE**
> Dans SharePoint 2013, l'opérateur **RANK** est déconseillé et n'a plus d'effet. Utilisez **XRANK** à la place.
  
    
    


### AND
<a name="fql_and_operator"> </a>

Renvoie uniquement les éléments qui correspondent à tous les opérandes **AND**. Les opérandes peuvent être un seul terme ou une sous-expression FQL valide.
  
    
    

#### Syntaxe

 `and(operand, operand [, operand]*)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

L'expression suivante correspond aux éléments pour lesquels l'index de texte intégral par défaut contient « cat », « dog » et « fox ».
  
    
    
 `and(cat, dog, fox)`
  
    
    

### ANDNOT
<a name="fql_andnot_operator"> </a>

Renvoie uniquement les éléments qui correspondent au premier opérande et qui ne correspondent pas aux opérandes suivants. Les opérandes peuvent être un seul terme ou une sous-expression FQL valide.
  
    
    

#### Syntaxe

 `andnot(operand, operand [,operand]*)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

 **Exemple 1.** L'expression suivante correspond aux éléments pour lesquels l'index de recherche en texte intégral par défaut contient « cat », mais pas « dog ».
  
    
    
 `andnot(cat, dog)`
  
    
    
 **Exemple 2.** L'expression suivante correspond aux éléments pour lesquels l'index de recherche en texte intégral par défaut contient « dog », mais ni « beagle » ni « chihuahua ».
  
    
    
 `andnot(dog, beagle, chihuahua)`
  
    
    

### ANY
<a name="fql_any_operator"> </a>


> **REMARQUE**
> Dans SharePoint Server 2013, l'opérateur **ANY** est déconseillé. Utilisez l'opérateur **OR** à la place.
  
    
    

Semblable à l'opérateur  [OU](fast-query-language-fql-syntax-reference.md#fql_or_operator), mais le rang dynamique (score de pertinence dans le jeu de résultats) n'est concerné ni par le nombre d'opérandes correspondants ni par la distance entre les termes dans l'élément. Les opérandes peuvent être un seul terme ou une sous-expression FQL valide.
  
    
    
Le composant de classement dynamique pour cette partie de la requête est basé sur le terme proposant la meilleure correspondance dans l'expression **ANY**.
  
    
    

> **REMARQUE**
> La différence par rapport à l'opérateur **OR** ne concerne que le classement dans le jeu de résultats. Le même ensemble total d'éléments correspondra à la requête.
  
    
    


#### Syntaxe

 `any(operand, operand [,operand]*)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

 L'expression suivante correspond aux éléments pour lesquels l'index de texte intégral par défaut contient « cat » ou « dog ».
  
    
    
Si l'index contient « cat » et « dog », mais que « cat » est considéré comme une meilleure correspondance, le rang dynamique de l'élément sera basé sur « cat », sans prendre en compte « dog ».
  
    
    
 `any(cat, dog)`
  
    
    

### COUNT
<a name="fql_count_operator"> </a>

Indique le nombre d'occurrences de terme de requête qu'un élément doit inclure pour être renvoyé en tant que résultat. L'opérande peut être un seul terme de requête, une expression ou un terme de requête générique.
  
    
    

#### Syntaxe

 `property-spec:count(operand [,from=<numeric value>, to=<numeric value>])`
  
    
    

#### Parameters



|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _From_ <br/> | _<numeric_value>_ <br/> |La valeur du paramètre  _from_ doit être un entier positif indiquant le nombre minimal de fois où l'opérande spécifié doit être mis en correspondance. <br/> Si le paramètre  _from_ n'est pas spécifié, il n'existe aucune limite inférieure. <br/> |
| _to_ <br/> | _<numeric_value>_ <br/> |La valeur du paramètre  _to_ doit être un entier positif indiquant le nombre maximal non inclusif de fois où l'opérande spécifié doit être mis en correspondance. Par exemple, une valeur _to_ de **11** correspond à 10 fois ou moins. <br/> Si le paramètre  _to_ n'est pas spécifié, il n'existe aucune limite supérieure. <br/> |
   

#### Exemples

 **Exemple 1.** L'expression suivante correspond à au moins 5 occurrences du mot « cat ».
  
    
    
 `count(cat, from=5)`
  
    
    
 **Exemple 2.** L'expression suivante correspond à au moins 5, mais moins de 10 occurrences du mot « cat ».
  
    
    
 `count(cat, from=5, to=10)`
  
    
    
 **Exemple 3.** Chacune des expressions suivantes correspond à au moins 3 occurrences d'un mot, et celui-ci peut être « cat » ou « dog ».
  
    
    
 `count(or(cat, dog), from=3)count(string("cat dog", mode="or"), from=3)`
  
    
    
Le tableau suivant contient des exemples de valeurs de chaîne de propriété gérée et indique si elles correspondent aux deux expressions présentées dans l'exemple 3.
  
    
    


|**Correspondance ?**|**Texte**|
|:-----|:-----|
|Oui  <br/> |My cat likes my dog, but my dog hates my cat.  <br/> |
|Non  <br/> |My bird likes my newt, but my dog hates my cat.  <br/> |
   

### DATETIME
<a name="fql_datetime_operator"> </a>

Fournit un typage explicite des valeurs numériques de date/heure. L'opérande est une chaîne de date/heure formatée selon la syntaxe spécifiée dans  [Expressions de jeton dans le langage FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.
  
    
    

#### Syntaxe

 `datetime(<date/time string>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

### DECIMAL
<a name="fql_decimal_operator"> </a>

Fournit un typage explicite des valeurs décimales. L'opérande est une valeur décimale selon la syntaxe spécifiée dans les  [Expressions de jeton dans le langage FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.
  
    
    

#### Syntaxe

 `decimal(<decimal point value>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

### ENDS-WITH
<a name="fql_endswith_operator"> </a>

Spécifie qu'un mot ou une expression doit apparaître à la fin d'une propriété gérée (correspondance de limite).
  
    
    
La correspondance de limite n'est pas prise en charge sur les propriétés gérées numériques. Ces dernières font toujours l'objet d'une correspondance exacte ou de plage de valeurs. 
  
    
    
Certaines applications peuvent exiger que vous puissiez réaliser une correspondance exacte d'une propriété gérée. Par exemple, il peut s'agir d'une propriété gérée **product name** où le nom complet d'un produit est une sous-chaîne d'un autre nom de produit.
  
    
    

#### Syntaxe

 `ends-with(<term or phrase>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

L'expression suivante correspond aux éléments avec les valeurs « Mr Adam Jones » et « Adam Jones » dans la propriété gérée « author ». Elle ne correspond pas aux éléments contenant la valeur « Adam Jones sr ».
  
    
    
 `author:ends-with("adam jones")`
  
    
    

#### Notes

La correspondance de limite peut être appliquée à l'intégralité du texte de la propriété gérée ou à des chaînes individuelles dans une propriété gérée qui contient une liste de valeurs de chaîne, par exemple, une liste de noms. Dans ce cas, vous devez mettre en correspondance le contenu exact de chaque chaîne et éviter la correspondance de requête entre les limites de chaîne. 
  
    
    
Pour appliquer des requêtes de correspondance de limite, vous devez configurer la propriété gérée concernée dans le schéma d'index. 
  
    
    
En activant la fonctionnalité de correspondance de limite pour la propriété gérée, vous pouvez effectuer les opérations suivantes : 
  
    
    

- Utiliser des requêtes de correspondance de limite explicites. 
    
  
- Empêcher la correspondance d'expressions entre les limites de chaîne. Pour les propriétés gérées qui contiennent plusieurs chaînes, cette fonctionnalité garantit qu'une chaîne ne correspond pas à des mots avant ou après une indication de limite.
    
  

### EQUALS
<a name="fql_equals_operator"> </a>

Indique qu'un mot ou une expression doit fournir une correspondance de jeton exacte avec la propriété gérée.
  
    
    

#### Syntaxe

 `equals(<term or phrase>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

L'exemple suivant correspond aux éléments avec les valeurs « Adam Jones » dans la propriété gérée « author ». Il ne correspond pas aux éléments contenant les valeurs « Adam Jones sr » et « Mr Adam Jones ».
  
    
    
 `author:equals("adam jones")`
  
    
    

#### Notes

Voir également  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator).
  
    
    

### FILTER
<a name="fql_filter_operator"> </a>

Permet d'interroger des métadonnées ou d'autres données structurées. 
  
    
    
L'utilisation de l'opérateur **FILTER** implique automatiquement les éléments suivants pour la requête spécifiée :
  
    
    

- La linguistique sera définie sur linguistics="OFF".
    
  
- Le classement sera désactivé.
    
  
- Aucune mise en surbrillance de requête ne sera utilisée dans le résumé avec résultats mis en surbrillance pour le résultat de requête.
    
  

> **CONSEIL**
> Si vous utilisez l'opérateur **STRING** dans une expression **FILTER**, par défaut, la linguistique est désactivée. Vous pouvez activer le traitement de la linguistique au sein de chaque expression **STRING** dans l'expression **FILTER** à l'aide de l'opérande `linguistics="ON"`. 
  
    
    


#### Syntaxe

 `filter(<any valid FQL operator expression>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

L'expression suivante correspond aux éléments qui ont une propriété gérée **Title** contenant « sonata » et une propriété gérée **Doctype** ne contenant que le jeton « audio ». Aucune correspondance linguistique ne sera effectuée sur « audio ». Étant donné que le jeton **FILTER** sera utilisé pour la correspondance avec « audio », ce texte ne sera pas mis en surbrillance dans le résumé avec résultats mis en surbrillance.
  
    
    
 `and(title:sonata, filter(doctype:equals("audio")))`
  
    
    

#### Notes

Si vous devez restreindre votre requête afin qu'elle corresponde à au moins un grand ensemble de valeurs entières dans une propriété numérique, vous pouvez exprimer cela de deux manières équivalentes sur le plan du fonctionnement : 
  
    
    

-  `and(string("hello world"), filter(property-spec:or(1, 20, 453, ... , 3473)))`
    
  
-  `and(string("hello world"), filter(property-spec:int("1 20 453 ... 3473", mode="or")))`
    
  
Le deuxième exemple utilise l'opérateur **INT** à l'aide d'une chaîne contenant l'ensemble de valeurs numériques entre guillemets doubles. Cette méthode fournit des performances de requête significativement plus élevées lors du filtrage avec un grand ensemble de valeurs numériques.
  
    
    
Si vous devez filtrer un grand ensemble de valeurs, envisagez d'utiliser des valeurs numériques à la place des valeurs de chaîne et exprimez vos requêtes à l'aide de la syntaxe optimisée.
  
    
    

### FLOAT
<a name="fql_float_operator"> </a>

Fournit un typage explicite des valeurs numériques à virgule flottante. L'opérande est une valeur à virgule flottante selon la syntaxe spécifiée dans  [Expressions de jeton dans le langage FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.
  
    
    

#### Syntaxe

 `float(<floating point value>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

### INT
<a name="fql_int_operator"> </a>

Fournit un typage explicite des valeurs entières. L'opérande est une valeur entière selon la syntaxe spécifiée dans  [Expressions de jeton dans le langage FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversion de type explicite est facultative et n'est généralement pas nécessaire. Le type de terme de requête est détecté en fonction du type de propriété gérée numérique cible.
  
    
    
L'opérateur **INT** peut également être utilisé pour exprimer un ensemble de valeurs entières en tant qu'arguments pour les opérateurs FQL booléens. Cela permet de fournir de façon efficace un ensemble de valeurs entières dans une requête, étant donné que les valeurs qui sont transmises à l'aide de l'opérateur **INT** ne sont pas analysées par l'analyseur de requête FQL, mais transmises directement au composant de mise en correspondance de requête.
  
    
    

#### Syntaxe

 `int(<integer value>)`
  
    
    
 `int("value, value, … , value")`
  
    
    
La première syntaxe spécifie un seul entier. La seconde syntaxe spécifie une liste de valeurs entières entre guillemets doubles séparées par des virgules.
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

Si vous devez restreindre votre requête pour qu'elle corresponde à au moins un grand ensemble de valeurs entières dans une propriété numérique, vous pouvez exprimer cela à l'aide de l'opérateur **INT**:
  
    
    
 `and(string("hello world"), filter(id:int("1 20 49 124 453 985 3473", mode="or")))`
  
    
    

### NEAR
<a name="fql_near_operator"> </a>

Restreint le jeu de résultats aux éléments qui ont  _N_ termes à une certaine distance les uns des autres.
  
    
    
L'ordre des termes de la requête n'est pas important pour la correspondance, seule la distance compte. 
  
    
    
N'importe quel nombre de termes peut être combiné avec les opérateurs **NEAR**.
  
    
    
Les opérandes **NEAR** peuvent être des termes uniques, des expressions ou des expressions d'opérateur booléen **OR** ou **ANY**. Les caractères génériques sont acceptés.
  
    
    
Si plusieurs opérandes de l'opérateur **NEAR** correspondent au même jeton indexé, ils sont considérés comme proches l'un de l'autre.
  
    
    

#### Syntaxe

 `near(arg, arg [, arg]* [, N=<numeric value>])`
  
    
    

#### Parameters



|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _N_ <br/> | _<numeric_value>_ <br/> |Indique le nombre maximal de mots autorisés entre les termes (proximité explicite).  <br/> Si l'opérateur **NEAR** comprend plus de deux opérandes, le nombre maximal de mots autorisés entre les termes ( _N_) est compté dans l'intégralité de l'expression.  <br/> Valeur par défaut : **4**. <br/> |
   

#### Exemples

 **Exemple 1.** L'expression suivante correspond aux chaînes qui contiennent « cat » et « dog », à condition qu'un maximum de quatre jetons indexés (valeur par défaut) les séparent.
  
    
    
 `near(cat, dog)`
  
    
    
 **Exemple 2.** L'expression suivante correspond aux chaînes qui contiennent « cat », « dog », « fox » et « wolf », à condition qu'un maximum de quatre jetons indexés les séparent.
  
    
    
 `near(cat, dog, fox, wolf)`
  
    
    
Le tableau suivant contient des exemples de valeurs de chaîne de propriété gérée et indique si elles correspondent à l'expression précédente de l'exemple 2.
  
    
    


|**Correspondance ?**|**Texte**|
|:-----|:-----|
|Oui  <br/> |The picture shows a cat, a dog, a fox, and a wolf.  <br/> |
|Oui (avec recherche de radical)  <br/> |Dogs, foxes, and wolves are canines, but cats are felines.  <br/> |
|Non  <br/> |The picture shows a cat with a dog, a fox, and a wolf.  <br/> |
   
L'expression suivante correspond à toutes les chaînes du tableau précédent.
  
    
    
 `near(cat, dog, fox, wolf, N=5)`
  
    
    

#### Notes

 **Considérations relatives à la distance entre les termes avec les opérateurs NEAR/ONEAR**
  
    
    
 _N_ indique le nombre maximal de mots autorisés entre les termes de la requête dans le segment correspondant de l'élément. Si l'opérateur **NEAR** ou **ONEAR** comprend plus de deux opérandes, le nombre maximal de mots autorisés entre les termes de la requête ( _N_) est compté dans le segment de l'élément correspondant à tous les termes **NEAR** ou **ONEAR**.
  
    
    
L'opérateur **NEAR** ou **ONEAR** fonctionne sur le texte tokenisé. Cela signifie que les caractères spéciaux tels que la virgule (« **,** »), le point (« **.** »), les deux points (« **:** ») ou le point-virgule (« **;** ») seront traités comme des espaces blancs. Le terme « distance » concerne les jetons au sein du texte indexé.
  
    
    
Si vous utilisez **ONEAR** ou **NEAR** avec des opérandes égaux, l'opérateur fonctionne comme suit :
  
    
    
 `near(a, a, n=x)`
  
    
    
Cette requête renverra toujours **true** si au moins une instance de « `a` » apparaît dans le contexte. Cela signifie également que l'opérateur **NEAR** ne peut pas être utilisé en tant qu'opérateur **COUNT**. Pour plus d'informations sur le comptage des occurrences des termes, voir l'opérateur  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator). 
  
    
    
L'opérateur **NEAR** appliqué à des expressions correspondra également aux expressions se chevauchant dans le texte.
  
    
    
Si un jeton du segment correspondant met en correspondance plus d'un opérande avec l'expression **NEAR** ou **ONEAR**, la requête peut correspondre, même si le nombre de jetons sans correspondance dans le segment correspondant dépasse la valeur de « _N_ » dans l'expression de l'opérateur **NEAR** ou **ONEAR**. Par exemple, un chevauchement peut se traduire par des expressions qui se chevauchent. Si le nombre de correspondances de chevauchement de jeton est «  `O` », la requête correspondra si un nombre maximal de « `N+O` » jetons sans correspondance apparaît dans le segment correspondant de l'élément.
  
    
    
 ** **NEAR** ou **ONEAR** associé à **NOT****
  
    
    
L'opérateur **NOT** ne peut pas être utilisé dans l'opérateur **NEAR** ou **ONEAR**. La syntaxe FQL suivante est incorrecte :
  
    
    
 `near(audi,not(bmw),n=2)`
  
    
    

### NOT
<a name="fql_not_operator"> </a>

Renvoie uniquement les éléments qui ne correspondent pas à l'opérande. Celui-ci peut être n'importe quelle expression FQL valide.
  
    
    

#### Syntaxe

 `not(operand)`
  
    
    

#### Parameters

Non applicable.
  
    
    

### ONEAR
<a name="fql_onear_operator"> </a>

Variante ordonnée de **NEAR**, exige une correspondance ordonnée des termes. Vous pouvez utiliser l'opérateur **ONEAR** pour restreindre le jeu de résultats aux éléments qui ont _N_ termes à une certaine distance les uns des autres.
  
    
    

#### Syntaxe

 `onear(arg, arg [, arg]* [, N=<numeric value>])`
  
    
    

#### Parameters



|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _N_ <br/> | _<numeric_value>_ <br/> |Indique le nombre maximal de mots autorisés entre les termes (proximité explicite).  <br/> Si l'opérateur **ONEAR** comprend plus de deux opérandes, le nombre maximal de mots autorisés entre les termes ( _N_) est compté dans l'intégralité de l'expression.  <br/> Valeur par défaut : **4**. <br/> |
   

#### Exemples

 **Exemple 1.** L'expression suivante met en correspondance toutes les occurrences des mots « cat », « dog », « fox » et « wolf » qui apparaissent dans l'ordre, à condition qu'un maximum de quatre jetons indexés les séparent.
  
    
    
 `onear(cat, dog, fox, wolf)`
  
    
    
Le tableau suivant contient des exemples de valeurs de chaîne de propriété gérée et indique si elles correspondent à l'expression précédente.
  
    
    


|**Correspondance ?**|**Texte**|
|:-----|:-----|
|Oui  <br/> |The picture shows a cat, a dog, a fox, and a wolf.  <br/> |
|Non  <br/> |Dogs, foxes, and wolves are canines, but cats are felines.  <br/> |
|Non  <br/> |The picture shows a cat with a dog, a fox, and a wolf.  <br/> |
   
 **Exemple 2.** L'expression suivante établit une correspondance (avec recherche du radical) avec le texte de la deuxième ligne du tableau précédent.
  
    
    
 `onear(dog, fox, wolf, cat, N=5)`
  
    
    
 **Exemple 3.** L'expression suivante correspond au texte de la première et de la troisième lignes du tableau précédent.
  
    
    
 `onear(cat, dog, fox, wolf, N=5)`
  
    
    

#### Notes

Voir aussi l'opérateur  [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator).
  
    
    

### OU
<a name="fql_or_operator"> </a>

Renvoie uniquement les éléments qui correspondent à au moins l'un des opérandes **OR**. Les éléments correspondants obtiendront un rang dynamique supérieur (score de pertinence dans le jeu de résultats) en cas de correspondance de plusieurs opérandes **OR**. Les opérandes peuvent être des termes uniques ou n'importe quelle sous-expression FQL valide.
  
    
    

#### Syntaxe

 `or(operand, operand [,operand]*)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

L'expression suivante met en correspondance tous les éléments pour lesquels l'index de recherche en texte intégral par défaut contient « cat » ou « dog ». Si l'index de recherche en texte intégral par défaut d'un élément contient « cat » et « dog », il correspondra et aura un rang dynamique plus élevé que s'il ne contenait qu'un des jetons.
  
    
    
 `or(cat, dog)`
  
    
    

### PHRASE
<a name="fql_phrase_operator"> </a>

Recherche une chaîne exacte de jetons. 
  
    
    
Les opérandes **PHRASE** peuvent être des termes uniques. Les caractères génériques sont acceptés.
  
    
    

#### Syntaxe

 `phrase(term [, term]*)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Notes

Voir aussi l'opérateur  [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator).
  
    
    

### RANGE
<a name="fql_range_operator"> </a>

Utilisez l'opérateur **RANGE** pour les propriétés gérées numériques et de date/heure. L'opérateur autorise les expressions de correspondance de plage.
  
    
    

#### Syntaxe

 `range(start, stop [,from="GE"|"GT"] [,to="LE"|"LT"])`
  
    
    

#### Parameters



|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _start_ <br/> | _<numeric_value>|<date/time_value>_ <br/> |Valeur de début de la plage.  <br/> Pour spécifier que la plage ne comporte pas de limite inférieure, utilisez le mot réservé **min**. <br/> |
| _stop_ <br/> | _<numeric_value>|<date/time_value>_ <br/> |Valeur de fin de la plage.  <br/> Pour spécifier que la plage ne comporte pas de limite supérieure, utilisez le mot réservé **max**. <br/> |
| _from_ <br/> |**GE|GT** <br/> | Paramètre facultatif qui indique l'intervalle de début ouvert ou fermé. <br/>  Valeurs valides : <br/> **GE**: supérieur ou égal à la valeur de début (>= début de l'intervalle). <br/> **GT**: supérieur à la valeur de début (> début de l'intervalle). <br/>  Valeur par défaut : **GE**. <br/> |
| _to_ <br/> |**LE|LT** <br/> | Paramètre facultatif qui indique l'intervalle de fin ouvert ou fermé. <br/>  Valeurs valides : <br/> **LE**: inférieur ou égal à la valeur de fin (<= fin de l'intervalle). <br/> **LT**: inférieur à la valeur de fin (< fin de l'intervalle). <br/>  Valeur par défaut : **LT**. <br/> |
   

#### Exemples

L'expression suivante met en correspondance une propriété de description commençant par l'expression « olympic games » figurant dans les éléments d'une taille d'au moins 10 000 octets.
  
    
    
 `and(size:range(10000, max), description:starts-with("olympic games"))`
  
    
    

### STARTS-WITH
<a name="fql_startswith_operator"> </a>

Spécifie un mot ou une expression devant apparaître au début d'une propriété gérée.
  
    
    

#### Syntaxe

 `starts-with(<term or phrase>)`
  
    
    

#### Parameters

Non applicable.
  
    
    

#### Exemples

L'expression suivante mettra en correspondance les éléments avec les valeurs « Adam Jones sr » et « Adam Jones » dans la propriété gérée **author**. Elle ne mettra pas en correspondance les éléments avec la valeur « Mr Adam Jones ».
  
    
    
 `author:starts-with("adam jones")`
  
    
    

#### Notes

Pour consulter des remarques supplémentaires sur la correspondance de limite, voir l'opérateur  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator).
  
    
    

### STRING
<a name="fql_string_operator"> </a>

Définit une condition de correspondance booléenne avec une chaîne de texte.
  
    
    
L'opérande est une chaîne de texte (un ou plusieurs termes) qui doit être mise en correspondance. La chaîne est suivie par zéro, un ou plusieurs paramètres. 
  
    
    
L'opérateur **STRING** peut également être utilisé en tant que conversion de type. La requête `string("24.5")`, par exemple, traite la valeur numérique « 24.5 » comme une chaîne de texte.
  
    
    

#### Syntaxe

 `string("<text string>"`
  
    
    
 ` [, mode=<mode>]`
  
    
    
 ` [, n=<near>]`
  
    
    
 ` [, weight=<n>]`
  
    
    
 ` [, linguistics=<on|off>]`
  
    
    
 ` [, wildcard=<on|off>])`
  
    
    

#### Parameters



|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _mode_ <br/> | _<mode>_ <br/> | Le paramètre _mode_ spécifie le mode d'évaluation de la valeur <text string>. La liste suivante présente les valeurs valides. <br/> **"PHRASE"** - `phrase(term [,term]*)` <br/> |**Mode**|**Expression d'opérateur équivalente**|
|:-----|:-----|
|**"PHRASE"** <br/> | `phrase(term [,term]*)` <br/> |
|**"AND"** <br/> | `and(term, term [,term]*)` <br/> |
|**"OR"** <br/> | `or(term, term [,term]*)` <br/> |
|**"ANY"** <br/> | `any(term, term [,term]*)` <br/> |
|**"NEAR"** <br/> | `near(term, term [,term]*, N)` <br/> |
|**"ONEAR"** <br/> | `onear(term, term [,term]*, N)` <br/> |
   
 Valeur par défaut : **"PHRASE"** <br/> |
| _n_ <br/> | _<numeric_value>_ <br/> |Ce paramètre indique la distance maximale entre les termes pour  _mode_= **"NEAR"** ou _mode_= **"ONEAR"**. <br/> Les expressions suivantes sont équivalentes :  <br/>  `string("hello world", mode="NEAR", n=5)` <br/>  `near(hello, world, n=5)` <br/> Valeur par défaut : **4**. <br/> |
| _weight_ <br/> | _<numeric_value>_ <br/> |Ce paramètre est une valeur numérique positive indiquant la pondération du terme pour le classement dynamique.  <br/> Une valeur inférieure indique qu'un terme doit moins contribuer au classement. Une valeur plus élevée indique qu'un terme doit davantage y contribuer. Une valeur de zéro pour le paramètre de pondération spécifie qu'un terme ne doit pas avoir d'influence sur le rang dynamique.  <br/> Le paramètre  _weight_ s'applique à tous les termes de l'expression **STRING**. <br/> > **CONSEIL**> Le paramètre de pondération affecte uniquement les requêtes d'index de recherche en texte intégral.           Valeur par défaut : **100**. <br/> |
| _linguistics_ <br/> |**on|off** <br/> |Désactive/Active toutes les fonctionnalités linguistiques de la chaîne (lemmatisation, synonymes, vérification de l'orthographe) si elles sont activées pour la requête.  <br/> Vous pouvez utiliser ce paramètre pour désactiver le traitement linguistique pour une chaîne ou un terme donné si vous voulez tout de même que le terme ou la chaîne contribue au classement.  <br/> Valeur par défaut : **"ON"** <br/> |
| _wildcard_ <br/> |**on|off** <br/> | Ce paramètre contrôle l'expansion générique des termes à l'intérieur de la valeur _<text string>_. Ce paramètre remplace tous les paramètres génériques dans les paramètres de la requête et permet l'activation ou la désactivation des caractères génériques étendus sur des parties spécifiques de la requête.  <br/>  Les valeurs valides sont les suivantes : <br/> **"ON"** indique que le caractère « ***** » est évalué en tant que caractère générique. Le caractère « ***** » correspond à zéro, un ou plusieurs caractères. <br/> **"OFF"** indique que le caractère « ***** » n'est pas évalué en tant que caractère générique. <br/>  Valeur par défaut : **"ON"** <br/> |
   

> **REMARQUE**
> Dans SharePoint 2013, les paramètres  _minexpansion_,  _maxexpansion_ et _annotation_class_ pour l'opérateur **STRING** sont obsolètes.
  
    
    


#### Exemples

 **Exemple 1.** Étant donné que le mode chaîne par défaut est « **PHRASE** », chacune des expressions suivantes renvoie les mêmes résultats.
  
    
    
 `"what light through yonder window breaks"string("what light through yonder window breaks")string("what light through yonder window breaks", mode="phrase")phrase(what, light, through, yonder, window, breaks)`
  
    
    
 **Exemple 2.** L'expression de jeton de la chaîne suivante et l'expression d'opérateur **AND** renvoient les mêmes résultats.
  
    
    
 `string("cat dog fox", mode="and")and(cat, dog, fox)`
  
    
    
 **Exemple 3.** L'expression de jeton de la chaîne suivante et l'expression d'opérateur **OR** renvoient les mêmes résultats.
  
    
    
 `string("coyote saguaro", mode="or")or(coyote, saguaro)`
  
    
    
 **Exemple 4.** L'expression de jeton de la chaîne suivante et l'expression d'opérateur **ANY** renvoient les mêmes résultats.
  
    
    
 `string("coyote saguaro", mode="any")any(coyote, saguaro)`
  
    
    
 **Exemple 5.** L'expression de jeton de la chaîne suivante et l'expression d'opérateur **NEAR** renvoient les mêmes résultats.
  
    
    
 `string("coyote saguaro", mode="near")near(coyote, saguaro)`
  
    
    
 **Exemple 6.** L'expression de jeton de la chaîne suivante et l'expression d'opérateur **NEAR** renvoient les mêmes résultats.
  
    
    
 `string("cat dog fox wolf", mode="near", N=4)near(cat, dog, fox, wolf, N=4)`
  
    
    
 **Exemple 7.** L'expression de jeton de la chaîne suivante et l'expression d'opérateur **ONEAR** renvoient les mêmes résultats.
  
    
    
 `string("cat dog fox wolf", mode="onear")onear(cat, dog, fox, wolf)`
  
    
    
 **Exemple 8.** L'expression de jeton de la chaîne suivante correspond au mot « nobler » avec les fonctionnalités linguistiques désactivées, de sorte que les autres formes du mot (telles que « ennobling ») ne sont pas mises en correspondance à l'aide de la recherche de radical.
  
    
    
 `string("nobler", linguistics="off")`
  
    
    
 **Exemple 9.** L'expression suivante correspond aux éléments contenant « cat » ou « dog », mais l'expression augmente le rang dynamique des éléments qui contiennent « dog » davantage que celui des éléments qui contiennent « cat ».
  
    
    
 `or(string("cat", weight="200"), string("dog", weight="500"))`
  
    
    

#### Notes

 **Pondération liée à la pertinence pour le classement dynamique**
  
    
    
L'effet principal du paramètre **weight** concerne les requêtes **OR**. Il peut également avoir une influence sur les requêtes **AND**. L'algorithme de rang dynamique peut impliquer que différents termes fournissent une contribution de rang différente en fonction de l'emplacement de l'élément où se produit la correspondance de terme.
  
    
    
La différence de contribution de rang peut également être basée sur la fréquence des termes et la fréquence de l'élément inverse. Voici un exemple :
  
    
    

- Requête :  `and(string("a"), string("b", weight=200))`
    
  
- Schéma d'index : La propriété gérée **title** a plus de poids que la propriété gérée **body**.
    
  
- L'élément d'index 1 comprend le terme « a » dans le titre et le terme « b » dans le corps. 
    
  
- L'élément d'index 2 comprend le terme « a » dans le corps et le terme « b » dans le titre. 
    
  
Dans cet exemple, l'élément 2 aura le rang total le plus élevé, étant donné que les éléments dont la contribution au rang dynamique est plus élevée ont un poids encore plus fort.
  
    
    

> **CONSEIL**
> Le renforcement de terme relatif (positif ou négatif) s'applique au composant de rang dynamique du rang total. Cependant, le calcul de rang lié au renforcement de proximité (distance entre les mots) n'est pas concerné par la pondération de terme. La pondération relative ne signifie pas toujours que le rang total de l'élément est modifié selon le pourcentage indiqué. > La requête suivante recherche les termes « peter », « paul » ou « mary », où « peter » a une contribution de rang deux fois plus importante que les deux autres termes. >  `or(peter, string("paul mary", mode="OR", weight=50))`
  
    
    

 **Gestion des chaînes contenant des caractères spéciaux**
  
    
    
Les caractères spéciaux tels que la virgule (« , »), le point-virgule (« ; »), les deux points (« : »), le point (« . »), le signe moins (« - »), le trait de soulignement (« _ ») ou la barre oblique (« / ») sont traités comme des espaces blancs dans une expression de chaîne entre guillemets doubles. Ceci est lié au processus de création de jetons. Ces caractères impliquent également un phrasé implicite des jetons séparés par ces caractères. 
  
    
    
Les expressions de requête suivantes sont équivalentes.
  
    
    
 `title:string("animals birds", mode="phrase")title:"animals/birds"title:string("animals/birds", mode="and")title:string("animals/birds", mode="or")`
  
    
    
Les expressions de requête suivantes sont équivalentes.
  
    
    
 `title:or(string("animals birds", mode="phrase"), string("animals insects", mode="phrase"))title:string("animals/birds animals/insects", mode="or")`
  
    
    
Les expressions de requête suivantes sont équivalentes.
  
    
    
 `body:string("help contoso com", mode="phrase")body:string("help@contoso.com")`
  
    
    
 **Mise en correspondance d'expressions tokenisées**
  
    
    
Vous pouvez rechercher une chaîne exacte de jetons à l'aide de l'opérateur **STRING** avec _mode_="phrase" ou de l'opérateur **PHRASE**.
  
    
    
Toutes ces opérations d'expressions impliquent une mise en correspondance d'expressions tokenisées. Cela signifie que les caractères spéciaux tels que la virgule (« **,** »), le point-virgule (« **;** »), les deux points (« **:** »), le trait de soulignement (« **_** »), le signe moins (« **-** ») ou la barre oblique (« **/** ») sont traités comme des espaces blancs. Ceci est lié au processus de création de jetons.
  
    
    

### XRANK
<a name="fql_xrank_operator"> </a>

Améliore le rang dynamique des éléments en fonction de certaines occurrences du terme dans l' _match expression_, sans modifier les éléments correspondant à la requête. Une expression **XRANK** comprend un composant qui doit être respecté, l' _match expression_, et des composants qui contribuent uniquement au rang dynamique, l' _rank expression_. Au moins **un** des paramètres, excepté _n_, doit être spécifié pour qu'une expression XRANK soit valide.
  
    
    
Les  _Match expressions_ peuvent être toute expression valide de FQL, y compris des expressions **XRANK** imbriquées. Les _Rank expressions_ peuvent être toute expression valide de FQL sans expressions **XRANK**. Si vos requêtes FQL possèdent plusieurs opérateurs **XRANK**, la valeur de rang dynamique finale est calculée comme étant la somme des renforcements sur tous les opérateurs **XRANK**.
  
    
    

> **REMARQUE**
> Dans SharePoint Server 2010, l'opérateur **XRANK** possédait deux paramètres : _boost_ et _boostall_, ainsi que la syntaxe suivante :  `xrank(operand, rank-operand [, rank-operand]* [,boost=n] [,boostall=yes])`. Cette syntaxe et ses paramètres sont déconseillés dans SharePoint Server 2013. Nous vous recommandons d'utiliser la nouvelle syntaxe et les nouveaux paramètres à la place. 
  
    
    


#### Syntaxe

 `xrank(<match expression> [, <rank-expression>]*, rank-parameter[, rank-parameter]*)`
  
    
    

#### Formule


  
    
    
![Formule pour l'opérateur XRANK](images/XRANKFormula.gif)
  
    
    

  
    
    

  
    
    

#### Parameters



|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _N_ <br/> | _<integer_value>_ <br/> |Spécifie le nombre de résultats pour le calcul des statistiques.  <br/> Ce paramètre n'a pas d'incidence sur le nombre de résultats auquel contribue le rang dynamique ; il s'agit juste d'un moyen d'exclure les éléments non pertinents des calculs statistiques.  <br/> Par défaut : **0**. Une valeur de zéro porte la sémantique de *tous les documents*  . <br/> |
| _Nb_ <br/> | _<float_value>_ <br/> |Le paramètre  _nb_ fait référence à un renforcement normalisé. Ce paramètre indique le facteur multiplié par le produit de la variance et le score moyen des valeurs de rang de l'ensemble des résultats. <br/>  _f_ dans la formule XRANK. <br/> |
   
En règle générale, le renforcement normalisé,  _nb_, est le seul paramètre modifié. Il fournit le contrôle nécessaire pour promouvoir ou abaisser un élément en particulier, sans prendre en compte l'écart type. 
  
    
    

#### Paramètres avancés

Les paramètres avancés suivants sont également disponibles. Cependant, ils ne sont généralement pas utilisés.
  
    
    


|**Paramètre**|**Valeur**|**Description**|
|:-----|:-----|:-----|
| _cb_ <br/> | _<float_value>_ <br/> |Le paramètre  _cb_ fait référence au renforcement constant. <br/> Valeur par défaut : **0**. <br/>  _a_ dans la formule XRANK. <br/> |
| _stdb_ <br/> | _<float_value>_ <br/> |Le paramètre  _stdb_ fait référence à l'augmentation de l'écart type. <br/> Valeur par défaut : **0**. <br/>  _e_ dans la formule XRANK. <br/> |
| _avgb_ <br/> | _<float_value>_ <br/> |Le paramètre  _avgb_ fait référence au renforcement moyen. Ce facteur est multiplié par la moyenne de valeurs de rang dans l'ensemble des résultats. <br/> Valeur par défaut : **0**. <br/>  _d_ dans la formule XRANK. <br/> |
| _rb_ <br/> | _<float_value>_ <br/> |Le paramètre  _rb_ fait référence au renforcement de la plage. Ce facteur est multiplié par la plage de valeurs de rang dans l'ensemble des résultats. <br/> Valeur par défaut : **0**. <br/>  _b_ dans la formule XRANK. <br/> |
| _pb_ <br/> | _<float_value>_ <br/> |Le paramètre  _pb_ fait référence au renforcement du pourcentage. Ce facteur est multiplié par le rang de chaque élément par rapport à la valeur minimale dans le corpus. <br/> Valeur par défaut : **0**. <br/>  _c_ dans la formule XRANK. <br/> |
   

#### Exemples

 **Exemple 1.** L'expression suivante correspond aux éléments pour lesquels l'index de texte intégral par défaut contient les termes « chat » ou « chien ». L'expression augmente le rang dynamique de ces éléments avec un renforcement constant de 100 pour les éléments qui comprennent également les termes « pur-sang ».
  
    
    
 `xrank(or(cat, dog), thoroughbred, cb=100)`
  
    
    
 **Exemple 2.** L'expression suivante correspond aux éléments pour lesquels l'index de texte intégral par défaut contient les termes « chat » ou « chien ». L'expression augmente le rang dynamique de ces éléments avec un renforcement normalisé de 1,5 pour les éléments qui comprennent également les termes « pur-sang ».
  
    
    
 `xrank(or(cat, dog), thoroughbred, nb=1.5)`
  
    
    
 **Exemple 3.** L'expression suivante correspond aux éléments pour lesquels l'index de texte intégral par défaut contient les termes « chat » ou « chien ». L'expression augmente le rang dynamique de ces éléments avec un renforcement constant de 100 et un renforcement normalisé de 1,5 pour les éléments qui comprennent également les termes « pur-sang ».
  
    
    
 `xrank(or(cat, dog), thoroughbred, cb=100, nb=1.5)`
  
    
    
 **Exemple 4.** L'expression suivante correspond aux éléments contenant le terme « animaux » et augmente le rang dynamique comme suit :
  
    
    

- Le rang dynamique des éléments qui contiennent le terme « chiens » est augmenté de 100 points.
    
  
- Le rang dynamique des éléments qui contiennent le terme « chats » est augmenté de 200 points.
    
  
- Le rang dynamique des éléments qui contiennent les termes « chiens » et « chats » est renforcé de 300 points.
    
  
 `xrank(xrank(animals, dogs, cb=100), cats, cb=200)`
  
    
    

## Ressources supplémentaires
<a name="SP15FQL_addlresources"> </a>


-  [Création de requêtes de recherche dans SharePoint 2013](building-search-queries-in-sharepoint-2013.md)
    
  
