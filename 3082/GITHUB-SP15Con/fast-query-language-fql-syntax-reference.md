---
title: Referencia de sintaxis sobre el lenguaje de consulta FAST (FQL)
ms.prod: SHAREPOINT
ms.assetid: bd98a41b-623c-41d4-a15d-26c0d4ba4311
---



# Referencia de sintaxis sobre el lenguaje de consulta FAST (FQL)
Aprenda a construir consultas de búsqueda complejas para Buscar en SharePoint 2013 usando el lenguaje de consulta FAST (FQL). Esta referencia describe los elementos de una consulta FQL y enseña a usar las especificaciones de propiedades, las expresiones de token y los operadores en las consultas FQL.
## Introducción a las subexpresiones y expresiones del lenguaje de consulta FQL en SharePoint Server 2013
<a name="SP15FQL_about"> </a>

El lenguaje de consulta FAST (FQL) es un lenguaje potente que permite a los desarrolladores realizar búsquedas exactas y reducir el ámbito de la búsqueda a valores que pertenezcan a una propiedad administrada concreta o un índice de texto completo.
  
    
    
Una expresión de lenguaje de consulta puede contener subexpresiones anidadas que incluyan términos de consulta, especificaciones de propiedades y operadores, como se ve en la tabla 1.
  
    
    

**Tabla 1. Subexpresiones en expresiones de lenguaje de consulta**


|**Elemento**|**Descripción**|
|:-----|:-----|
|Expresiones de token  <br/> |Uno o varios términos de consulta, frases o valores numéricos que se van a buscar en una consulta.  <br/> |
|Especificación de propiedad  <br/> |Una propiedad o índice de texto completo que coincide con la expresión afectada.  <br/> |
|Operadores  <br/> |Palabras clave que especifican operaciones booleanas (como **AND**, **OR**) u otras restricciones a los operandos (como **FILTER**).  <br/> |
   

### Ejemplo de consulta FQL

En el siguiente ejemplo de consulta FQL se buscan los términos "hello" y "world" en la propiedad administrada **body** de un elemento indizado:
  
    
    
 `body:string("hello world", mode="and")`
  
    
    
En el ejemplo:
  
    
    

-  `body:` limita el ámbito de la consulta en la propiedad administrada de cuerpo del elemento.
    
  
-  `"hello world"` es el operando del operador **STRING**, que indica los términos que se buscan.
    
  
-  `mode="and"` indica que el operador lógico de consulta **AND** se aplicará a `"hello world"`.
    
  
La longitud de las consultas del lenguaje de consulta FAST está limitado a 2.048 caracteres.
  
    
    

## Especificación de propiedades en FQL
<a name="property_specification"> </a>

Una especificación de propiedad limita el ámbito de la expresión afectada a regiones determinadas del contenido indizado. Una región de este tipo se identifica por un índice de texto completo o una propiedad administrada. 
  
    
    
Las propiedades administradas de tipo **Text** y **YesNo** se evalúan como texto. Todos los otros tipos de propiedades administradas, incluido el tipo **Datetime**, se evalúan como valores numéricos.
  
    
    
Si no incluye una especificación de propiedad para una expresión, el motor de búsqueda trata de buscar coincidencias con el índice de texto completo definido en el esquema de índices.
  
    
    
El nombre de la propiedad siempre debe ir antes de dos puntos (operador **In**) y los operadores numéricos deben incluir una especificación de propiedad.
  
    
    
Se puede aplicar una especificación de propiedad (el operador **In**) a las siguientes entidades de consulta:
  
    
    

- Un término o una frase únicos, por ejemplo:
    
     `author:shakespeare`
  
    
    
 `title:"to be or not to be"`
    
  
- Un operador, como el operador **STRING** en el ejemplo siguiente:
    
  ```
  
  title:string("to be or not to be")
  ```


    En este caso, la especificación de propiedad se aplica a la expresión completa del operador.
    
  

### Ejemplos

Cada una de las expresiones siguientes coincide con elementos que tienen "much" y "nothing" en la propiedad administrada **title**.
  
    
    
 `title:and(much, nothing)`
  
    
    
 `and(title:much, title:nothing)`
  
    
    
 `title:string("much nothing", mode="and")`
  
    
    

## Expresiones de token en FQL
<a name="token_expressions"> </a>

Las expresiones de token son palabras, frases o valores numéricos que coinciden con el índice.
  
    
    
Una nueva expresión de token puede ser una sola palabra o una frase entre comillas dobles.
  
    
    
Una expresión de token numérica puede ser un valor único o una expresión de rango de valores.
  
    
    

### Expresiones de carácter comodín

Una expresión de carácter comodín indica un solo término o una frase que incluye el carácter del asterisco (" *****"); el asterisco implica una coincidencia de cero o varios caracteres sin incluir los espacios en blanco. FQL admite la búsqueda de prefijos en propiedades administradas de texto completo y en los índices de texto completo.
  
    
    

#### Ejemplos de expresiones de carácter comodín

La siguiente es una lista de usos válidos de expresiones de carácter comodín en FQL:
  
    
    

-  `text*`
    
  
-  `string("this examp*")`
    
  

### Expresiones de términos numéricos
<a name="fql_token_numeric"> </a>

Cada expresión de término numérico debe incluir una especificación de propiedad de un tipo de datos de esquema de índice compatible. En la tabla 2 se recogen los tipos de datos numéricos que se pueden usar en FQL. 
  
    
    

**Tabla 2. Tipos de datos numéricos que se pueden usar en FQL.**


|**Tipo FQL**|**Tipos de esquemas de índice compatibles**|**Descripción**|
|:-----|:-----|:-----|
|**Int** <br/> |**Integer** <br/> |Entero de 64 bits.  <br/> |
|**Float** <br/> |**Double** <br/> |Punto flotante de 64 bits (doble precisión).  <br/> |
|**Decimal** <br/> |**Decimal** <br/> |Decimal de 128 bits.  <br/> |
|**Datetime** <br/> |**Datetime** <br/> |Valor de fecha y hora.  <br/> La compatibilidad de fecha y hora en FQL permite las mismas operaciones numéricas en los valores de fecha y hora que en otros valores numéricos.  <br/> |
   

#### Expresiones de consulta de fecha y hora
<a name="fql_token_datetime"> </a>

FQL dispone del tipo de dato **datetime** para la fecha y la hora.
  
    
    
En las consultas se admiten los siguientes formatos de **datetime** compatibles con la norma ISO 8601:
  
    
    

- AAAA-MM-DD 
    
  
- AAAA-MM-DDThh:mm:ss 
    
  
- AAAA-MM-DDThh:mm:ssZ 
    
  
- AAAA-MM-DDThh:mm:ssfrZ
    
  
En estos formatos de **datetime**:
  
    
    

-  _YYYY_ especifica un año de cuatro dígitos.
    
    > **NOTA**
      > Se admiten solo cuatro dígitos. 
-  _MM_ especifica un mes de dos dígitos. Por ejemplo, 01 = enero.
    
  
-  _DD_ especifica un día del mes de dos dígitos (de 01 a 31).
    
  
-  _T_ especifica la letra "T".
    
  
-  _hh_ especifica una hora de dos dígitos (de 00 a 23). No se permite la indicación am/pm
    
  
-  _mm_ especifica un minuto de dos dígitos (de 00 a 59).
    
  
-  _ss_ especifica a un segundo de dos dígitos (de 00 a 59).
    
  
-  _fr_ especifica una fracción de segundos opcional, _ss_, de 1 a 7 dígitos detrás del **.** después de los segundos. Por ejemplo, 2012-09-27T11:57:34.1234567.
    
  
Todos los valores de fecha y hora deben especificarse según el formato UTC (hora universal coordinada), también conocido como zona horaria GMT (hora del meridiano de Greenwich). El identificador de zona horaria UTC (un carácter final "Z") es opcional.
  
    
    

### Palabras reservadas, caracteres especiales y escape
<a name="fql_token_numeric"> </a>

Las siguientes son palabras reservadas en FQL.
  
    
    
 `and, or, any, andnot, count, decimal, rank, near, onear, int, in32, int64, float, double, datetime, max, min, range, phrase, scope, filter, not, string, starts-with, ends-with, equals, words, xrank.`
  
    
    
Si quiere expresar una de estas palabras como término en una expresión de consulta, tiene que encerrarla entre comillas dobles, tal como se ve en los ejemplos siguientes: 
  
    
    

-  `or("any", "and", "xrank")`
    
  
-  `string("any and xrank", mode="OR")`
    
  
-  `phrase(this, is, a, "phrase")`
    
  

> **SUGERENCIA**
> Las palabras y los caracteres reservados no distinguen mayúsculas de minúsculas, pero se recomienda usar caracteres en minúscula por cuestiones de compatibilidad futura. 
  
    
    

FQL no siempre exige encerrar una cadena entre comillas dobles. Por ejemplo,  `and(cat, dog)` es una expresión FQL válida aunque `cat` y `dog` no estén entre comillas dobles, pero recomendamos usarlas para evitar conflictos con las palabras reservadas.
  
    
    
Los términos de consulta se ajustan según el token de la configuración regional. El proceso de ajuste por token quita algunos caracteres especiales. Como se quitan estos caracteres, las siguientes expresiones FQL son equivalentes.
  
    
    
 `and("[king]", "<queen>")`
  
    
    
 `and("king", "queen")`
  
    
    
Cuando una consulta incluya términos de la entrada de usuario o de otra aplicación, use el operador  `string("<query terms>", mode="AND|OR|PHRASE")` para evitar conflictos con las palabras reservadas en el lenguaje de consulta. También debe quitar las comillas dobles que pueda haber en la consulta introducida por el usuario.
  
    
    

## Operadores de FQL
<a name="fql_operators"> </a>

Los operadores del lenguaje de consulta FAST (FQL) son palabras clave que especifican operaciones booleanas u otras restricciones a los operandos. La sintaxis de operador FQL es la siguiente:
  
    
    
 `[property-spec:]operator(operand [,operand]* [, parameter="value"]*)`
  
    
    
En la sintaxis:
  
    
    

-  _property-spec_ es una especificación de propiedad opcional seguida por el operador "in".
    
  
-  _operator_ es una palabra clave que especifica una operación que hay que realizar.
    
  
-  _operand_ es una expresión de término u otro operador.
    
  
-  _parameter_ es el nombre de un valor que cambia el comportamiento del operador.
    
  
-  _value_ es el valor que se usa para el nombre del parámetro.
    
  
Los nombres de los operadores y de los parámetros, así como los valores de texto de los parámetros, distinguen mayúsculas y minúsculas. Los espacios en blanco se permiten en el cuerpo del operador, pero se omiten a menos que estén entre comillas dobles. La longitud de las consultas del lenguaje de consulta FAST está limitado a 2.048 caracteres.
  
    
    
La tabla 3 recoge los tipos de operadores admitidos por FQL. 
  
    
    

**Tabla 3. Tipos de operadores admitidos por FQL**


|**Tipo**|**Descripción**|**Operadores**|
|:-----|:-----|:-----|
|String  <br/> |Le permite especificar operaciones de consulta en una cadena de términos. Este es el operador que más se usa en los términos de texto.  <br/> | [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator) <br/> |
|Booleano  <br/> |Le permite combinar términos y subexpresiones en una consulta.  <br/> | [AND](fast-query-language-fql-syntax-reference.md#fql_and_operator),  [O bien](fast-query-language-fql-syntax-reference.md#fql_or_operator),  [ANY](fast-query-language-fql-syntax-reference.md#fql_any_operator),  [ANDNOT](fast-query-language-fql-syntax-reference.md#fql_andnot_operator),  [NOT](fast-query-language-fql-syntax-reference.md#fql_not_operator),  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator),  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator) <br/> |
|Proximidad  <br/> |Le permite especificar la proximidad de los términos de consulta en una secuencia coincidente de texto.  <br/> | [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator),  [ONEAR](fast-query-language-fql-syntax-reference.md#fql_onear_operator),  [PHRASE](fast-query-language-fql-syntax-reference.md#fql_phrase_operator),  [STARTS-WITH](fast-query-language-fql-syntax-reference.md#fql_startswith_operator),  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator),  [EQUALS](fast-query-language-fql-syntax-reference.md#fql_equals_operator) <br/> |
|Numérico  <br/> |Le permite especificar condiciones numéricas en la consulta.  <br/> | [RANGE](fast-query-language-fql-syntax-reference.md#fql_range_operator),  [INT](fast-query-language-fql-syntax-reference.md#fql_int_operator),  [FLOAT](fast-query-language-fql-syntax-reference.md#fql_float_operator),  [DATETIME](fast-query-language-fql-syntax-reference.md#fql_datetime_operator),  [DECIMAL](#fql_decimal_operator) <br/> |
|Relevancia  <br/> |Le permite influir en la evaluación de la relevancia de una consulta.  <br/> | [XRANK](fast-query-language-fql-syntax-reference.md#fql_xrank_operator) y [FILTER](fast-query-language-fql-syntax-reference.md#fql_filter_operator) <br/> |
   
La tabla 4 es la lista de los operadores compatibles.
  
    
    

**Tabla 4. Operadores FQL compatibles**


|**Operador**|**Descripción**|**Tipo**|
|:-----|:-----|:-----|
| [AND](fast-query-language-fql-syntax-reference.md#fql_and_operator) <br/> |Solo devuelve elementos que coinciden con todos los operandos **AND**. <br/> |Booleano  <br/> |
| [ANDNOT](fast-query-language-fql-syntax-reference.md#fql_andnot_operator) <br/> |Solo devuelve elementos que coinciden con el primer operando y que no coinciden con los siguientes.  <br/> |Booleano  <br/> |
| [ANY](fast-query-language-fql-syntax-reference.md#fql_any_operator) <br/> |Se parece al operador **OR**, pero la clasificación dinámica (la puntuación de relevancia del conjunto de resultados) no se ve afectada ni por el número de operandos que coinciden ni por la distancia que hay entre los términos del elemento. <br/> |Booleano  <br/> |
| [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator) <br/> |Le permite especificar el número de repeticiones del término de consulta que un elemento debe incluir para que se devuelva como resultado. El operando puede ser un solo término de consulta, una frase o un término de consulta de carácter comodín.  <br/> |Boolean  <br/> |
| [DATETIME](fast-query-language-fql-syntax-reference.md#fql_datetime_operator) <br/> |Ofrece una escritura explícita de valores numéricos.  <br/> La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.  <br/> |Numérico  <br/> |
| [DECIMAL](fast-query-language-fql-syntax-reference.md#fql_decimal_operator) <br/> |Ofrece una escritura explícita de valores numéricos.  <br/> La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.  <br/> |Numérico  <br/> |
| [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator) <br/> |Especifica que una palabra o frase deben aparecer al final de una propiedad administrada.  <br/> |Proximidad  <br/> |
| [EQUALS](fast-query-language-fql-syntax-reference.md#fql_equals_operator) <br/> |Especifica que una palabra, un término de frase o una frase deben proporcionar una coincidencia de token exacta con la propiedad administrada.  <br/> |Proximidad  <br/> |
| [FILTER](fast-query-language-fql-syntax-reference.md#fql_filter_operator) <br/> |Se usa para consultar metadatos u otros datos estructurados.  <br/> |Relevancia  <br/> |
| [FLOAT](fast-query-language-fql-syntax-reference.md#fql_float_operator) <br/> |Ofrece una escritura explícita de valores numéricos.  <br/> La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.  <br/> |Numérico  <br/> |
| [INT](fast-query-language-fql-syntax-reference.md#fql_int_operator) <br/> |Ofrece una escritura explícita de valores numéricos.  <br/> La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.  <br/> |Numérico  <br/> |
| [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator) <br/> |Restringe el conjunto de resultados a los elementos que tienen  `N` términos a cierta distancia unos de otros. <br/> |Proximidad  <br/> |
| [NOT](fast-query-language-fql-syntax-reference.md#fql_not_operator) <br/> |Solo devuelve elementos que excluyen el operando.  <br/> |Booleano  <br/> |
| [ONEAR](fast-query-language-fql-syntax-reference.md#fql_onear_operator) <br/> |La variante ordenada de **NEAR**; requiere un conjunto ordenado de términos. El operador **ONEAR** se puede usar para restringir el conjunto de resultados a los elementos que tienen `N` términos a cierta distancia unos de otros. Solo devuelve los elementos que no coinciden con el operando. El operando puede ser cualquier expresión FQL. <br/> |Proximidad  <br/> |
| [O bien](fast-query-language-fql-syntax-reference.md#fql_or_operator) <br/> |Solo devuelve los elementos que coinciden, al menos, con uno de los operandos **OR**. Los elementos que coinciden tienen una clasificación dinámica más alta (puntuación de relevancia en el conjunto de resultados) si coinciden más operandos **OR**. <br/> |Booleano  <br/> |
| [PHRASE](fast-query-language-fql-syntax-reference.md#fql_phrase_operator) <br/> | Solo devuelve elementos que coinciden con una cadena exacta de tokens. <br/> |Proximidad  <br/> |
| [RANGE](fast-query-language-fql-syntax-reference.md#fql_range_operator) <br/> | Permite expresiones coincidentes de rango. El operador **RANGE** se usa para propiedades administradas numéricas y de fecha y hora. <br/> |Numérico  <br/> |
| [STARTS-WITH](fast-query-language-fql-syntax-reference.md#fql_startswith_operator) <br/> |Especifica que una palabra o frase deben aparecer al principio de una propiedad administrada.  <br/> |Proximidad  <br/> |
| [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator) <br/> |Define una condición booleana coincidente para una cadena de texto.  <br/> |String  <br/> |
| [XRANK](fast-query-language-fql-syntax-reference.md#fql_xrank_operator) <br/> |Le permite aumentar la clasificación dinámica de elementos basándose en las repeticiones de ciertos términos sin cambiar los elementos que coinciden con la consulta. Una expresión **XRANK** contiene un componente que se debe ofrecer como resultado y uno o varios componentes que contribuyen solo a la clasificación dinámica. <br/> |Relevancia  <br/> |
   

> **NOTA**
> En SharePoint 2013, el operador **RANK** está en desuso y ya no tiene efecto. Ahora hay que usar **XRANK**. 
  
    
    


### AND
<a name="fql_and_operator"> </a>

Solo devuelve elementos que coinciden con todos los operandos **AND**. Los operandos pueden ser un solo término o cualquier subexpresión FQL válida.
  
    
    

#### Sintaxis

 `and(operand, operand [, operand]*)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "cat", "dog" y "fox".
  
    
    
 `and(cat, dog, fox)`
  
    
    

### ANDNOT
<a name="fql_andnot_operator"> </a>

Solo devuelve elementos que coinciden con el primer operando y que no coinciden con los siguientes. Los operandos pueden ser un solo término o cualquier subexpresión FQL válida.
  
    
    

#### Sintaxis

 `andnot(operand, operand [,operand]*)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

 **Ejemplo 1.** La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "cat" pero no "dog".
  
    
    
 `andnot(cat, dog)`
  
    
    
 **Ejemplo 2.** La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "dog" pero no contiene ni "beagle" ni "chihuahua".
  
    
    
 `andnot(dog, beagle, chihuahua)`
  
    
    

### ANY
<a name="fql_any_operator"> </a>


> **NOTA**
> En SharePoint Server 2013, el operador **ANY** está en desuso. Ahora hay que usar el operador **OR**. 
  
    
    

Se parece al operador  [O bien](fast-query-language-fql-syntax-reference.md#fql_or_operator), pero la clasificación dinámica (la puntuación de relevancia del conjunto de resultados) no se ve afectada ni por el número de operandos que coinciden ni por la distancia que hay entre los términos del elemento. Los operandos pueden ser un solo término o cualquier subexpresión FQL válida.
  
    
    
El componente de la clasificación dinámica para esta parte de la consulta se basa en el término de mejor coincidencia dentro de la expresión **ANY**.
  
    
    

> **NOTA**
> La diferencia de **OR** solo se relaciona con la clasificación dentro del conjunto de resultados. El mismo conjunto total de elementos coincidirá con la consulta.
  
    
    


#### Sintaxis

 `any(operand, operand [,operand]*)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

 La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "cat" o "dog".
  
    
    
Si el índice contiene "cat" y "dog", pero "cat" se considera una coincidencia mejor, la clasificación dinámica del elemento se basará en "cat" sin tener en cuanto el término "dog".
  
    
    
 `any(cat, dog)`
  
    
    

### COUNT
<a name="fql_count_operator"> </a>

Especifica el número de repeticiones del término de consulta que un elemento debe incluir para que el elemento se devuelva como resultado. El operando puede ser un solo término de consulta, una frase o un término de consulta de carácter comodín.
  
    
    

#### Sintaxis

 `property-spec:count(operand [,from=<numeric value>, to=<numeric value>])`
  
    
    

#### Parámetros



|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _From_ <br/> | _<valor_numérico>_ <br/> |El valor del parámetro  _from_ debe ser un entero positivo que especifique el número mínimo de veces que el operando especificado se debe mostrar como resultado. <br/> Si el parámetro  _from_ no se especifica, no existirá límite inferior. <br/> |
| _to_ <br/> | _<valor_numérico>_ <br/> |El valor del parámetro  _to_ debe ser un entero positivo que especifique el número máximo y no inclusivo de veces que el operando especificado se debe mostrar como resultado. Por ejemplo, un valor _to_ de **11** indica 10 veces o menos. <br/> Si el parámetro  _to_ no se especifica, no existirá límite superior. <br/> |
   

#### Ejemplos

 **Ejemplo 1.** La siguiente expresión ofrece como resultado al menos 5 repeticiones de la palabra "cat".
  
    
    
 `count(cat, from=5)`
  
    
    
 **Ejemplo 2.** La siguiente expresión ofrece como resultado al menos 5 repeticiones de la palabra "cat", pero no 10 ni más de 10.
  
    
    
 `count(cat, from=5, to=10)`
  
    
    
 **Ejemplo 3.** Todas las expresiones siguientes ofrecen como resultado al menos 3 repeticiones de una palabra, que puede ser "cat" o "dog".
  
    
    
 `count(or(cat, dog), from=3)count(string("cat dog", mode="or"), from=3)`
  
    
    
La tabla siguiente contiene ejemplos de valores de cadena de propiedades administradas; se indica si coinciden con las dos expresiones del ejemplo 3.
  
    
    


|**¿Coincide?**|**Texto**|
|:-----|:-----|
|Sí  <br/> |My cat likes my dog, but my dog hates my cat.  <br/> |
|No  <br/> |My bird likes my newt, but my dog hates my cat.  <br/> |
   

### DATETIME
<a name="fql_datetime_operator"> </a>

Ofrece una escritura explícita de valores numéricos de fecha y hora. El operando es una cadena de fecha y hora cuyo formato se basa en la sintaxis especificada en las  [Expresiones de token en FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.
  
    
    

#### Sintaxis

 `datetime(<date/time string>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

### DECIMAL
<a name="fql_decimal_operator"> </a>

Ofrece una escritura explícita de valores decimales. El operando es un valor decimal según la sintaxis especificada en  [Expresiones de token en FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.
  
    
    

#### Sintaxis

 `decimal(<decimal point value>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

### ENDS-WITH
<a name="fql_endswith_operator"> </a>

Especifica que una palabra o frase deben aparecer al final de una propiedad administrada (coincidencia de límites).
  
    
    
La coincidencia de límites no se admite en las propiedades administradas numéricas. Estas propiedades siempre estarán sujetas a una coincidencia exacta o de rango de valores. 
  
    
    
Algunas aplicaciones pueden necesitar que pueda realizar una coincidencia exacta de una propiedad administrada. Por ejemplo, puede tratarse de una propiedad administrada **product name**, donde el nombre completo de un producto es una subcadena de otro nombre de producto.
  
    
    

#### Sintaxis

 `ends-with(<term or phrase>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

La expresión siguiente ofrece elementos con los valores "Mr Adam Jones" y "Adam Jones" en la propiedad administrada "author". No mostrará resultados con el valor "Adam Jones sr".
  
    
    
 `author:ends-with("adam jones")`
  
    
    

#### Notas

La coincidencia de límites se puede aplicar a todo el texto de la propiedad administrada o a cadenas individuales dentro de una propiedad administrada que contenga una lista de valores de cadena, por ejemplo, una lista de nombres. En este caso, convendría hacer coincidir el contenido exacto de cada cadena y evitar la coincidencia de consultas de un límite de cadena a otro. 
  
    
    
Para aplicar las consultas de coincidencia de límites, hay que configurar la propiedad administrada correspondiente en el esquema de índice. 
  
    
    
Al habilitar la función Coincidencia de límite en la propiedad administrada, puede hacer lo siguiente: 
  
    
    

- Usar consultas de coincidencia de límites explícitas. 
    
  
- Evitar que las frases coincidan de un límite de cadena a otro. Para las propiedades administradas que contienen varias cadenas, esta función asegura que una cadena no coincida con palabras antes o después de una indicación de límite.
    
  

### EQUALS
<a name="fql_equals_operator"> </a>

Especifica que una palabra o frase debe ofrecer una coincidencia de token exacta con la propiedad administrada.
  
    
    

#### Sintaxis

 `equals(<term or phrase>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

El ejemplo siguiente ofrece elementos con los valores "Adam Jones" en la propiedad administrada "author". No mostrará resultados con el valor "Adam Jones sr" ni "Mr Adam Jones".
  
    
    
 `author:equals("adam jones")`
  
    
    

#### Notas

Vea también  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator).
  
    
    

### FILTER
<a name="fql_filter_operator"> </a>

Se usa para consultar metadatos u otros datos estructurados. 
  
    
    
Usar el operador **FILTER** implica automáticamente lo siguiente para la consulta especificada:
  
    
    

- La lingüística se establece en lingüística="OFF".
    
  
- La clasificación de deshabilita.
    
  
- No se usa el resaltado de consulta en el resumen de resultados resaltados para los resultados de consulta.
    
  

> **SUGERENCIA**
> Si usa el operador **STRING** dentro de una expresión **FILTER**, la lingüística se deshabilita de manera predeterminada. Puede habilitar la lingüística dentro de cada expresión **STRING** que hay en **FILTER** usando el operando `linguistics="ON"`. 
  
    
    


#### Sintaxis

 `filter(<any valid FQL operator expression>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

La expresión siguiente ofrece como resultados los elementos que tienen una propiedad administrada **Title** que contiene "sonata" y una propiedad administrada **Doctype** que contiene solo el token "audio". No se realiza ninguna coincidencia lingüística en "audio". Como el token **FILTER** se usará para mostrar "audio", ese texto no se resaltará en el resumen de resultados resaltados.
  
    
    
 `and(title:sonata, filter(doctype:equals("audio")))`
  
    
    

#### Notas

Si tiene que restringir la consulta para que muestre al menos uno de un conjunto amplio de valores enteros en una propiedad numérica, puede expresar esto de dos formas equivalentes funcionalmente: 
  
    
    

-  `and(string("hello world"), filter(property-spec:or(1, 20, 453, ... , 3473)))`
    
  
-  `and(string("hello world"), filter(property-spec:int("1 20 453 ... 3473", mode="or")))`
    
  
En el segundo ejemplo se emplea el operador **INT** usando una cadena con el conjunto de valores numéricos entre comillas. Esto ofrece un rendimiento de la consulta bastante mejor al filtrar con un amplio conjunto de valores numéricos.
  
    
    
Si tiene que filtrar un conjunto grande de valores, puede usar valores numéricos en lugar de valores de cadena y expresar las consultas usando la sintaxis optimizada.
  
    
    

### FLOAT
<a name="fql_float_operator"> </a>

Ofrece una escritura explícita de valores numéricos de punto flotante. El operando es un valor de punto flotante según la sintaxis especificada en las  [Expresiones de token en FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.
  
    
    

#### Sintaxis

 `float(<floating point value>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

### INT
<a name="fql_int_operator"> </a>

Ofrece una escritura explícita de valores enteros. El operando es un valor entero según la sintaxis especificada en las  [Expresiones de token en FQL](fast-query-language-fql-syntax-reference.md#token_expressions).
  
    
    
La conversión de tipo explícito es opcional y no suele ser necesaria. El tipo del término de consulta se detecta según el tipo de la propiedad numérica administrada de destino.
  
    
    
El operador **INT** también se puede usar para expresar un conjunto de valores enteros como argumentos de los operadores booleanos FQL. Esto supone un modo muy eficiente de ofrecer un conjunto de valores enteros en una consulta, ya que los valores que se pasan usando el operador **INT** no los analiza el analizar de consultas FQL, sino que pasan directamente al componente de coincidencias de consulta.
  
    
    

#### Sintaxis

 `int(<integer value>)`
  
    
    
 `int("value, value, … , value")`
  
    
    
La primera sintaxis especifica un solo entero. La segunda sintaxis especifica una lista de valores enteros separados por comas y encerrados entre comillas dobles.
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

Si tiene que restringir la consulta para que muestre al menos uno de un conjunto amplio de valores enteros en una propiedad numérica, puede expresar esto usando el operador **INT**:
  
    
    
 `and(string("hello world"), filter(id:int("1 20 49 124 453 985 3473", mode="or")))`
  
    
    

### NEAR
<a name="fql_near_operator"> </a>

Restringe el conjunto de resultados a los elementos que tienen  _N_ términos a cierta distancia unos de otros.
  
    
    
El orden de los términos de consulta no es importante para la búsqueda de resultados, solo importa la distancia. 
  
    
    
Puede combinar todos los términos que quiera con los operadores **NEAR**.
  
    
    
Los operandos **NEAR** pueden ser términos únicos, frases o las expresiones de operador booleano **OR** o **ANY**. Se pueden usar caracteres comodín.
  
    
    
Si hay varios operandos del operador **NEAR** que coinciden con el mismo token indizado, se considera que están cerca.
  
    
    

#### Sintaxis

 `near(arg, arg [, arg]* [, N=<numeric value>])`
  
    
    

#### Parámetros



|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _N_ <br/> | _<valor_numérico>_ <br/> |Especifica el número máximo de palabras que pueden aparecer entre los términos (proximidad explícita).  <br/> Si **NEAR** incluye más de dos operandos, el número máximo de palabras permitidas entre los términos ( _N_) se cuenta dentro de la expresión completa.  <br/> Valor predeterminado: **4** <br/> |
   

#### Ejemplos

 **Ejemplo 1.** La expresión siguiente muestra como resultados las cadenas que contienen "cat" y "dog" si no están separadas por más de cuatro tokens indizados (valor predeterminado).
  
    
    
 `near(cat, dog)`
  
    
    
 **Ejemplo 2.** La expresión siguiente muestra como resultados las cadenas que contienen "cat", "dog", "fox" y "wolf" si no están separadas por más de cuatro tokens indizados.
  
    
    
 `near(cat, dog, fox, wolf)`
  
    
    
La tabla siguiente contiene ejemplos de valores de cadena de propiedades administradas; se indica si coinciden con la expresión anterior del ejemplo 2.
  
    
    


|**¿Coincide?**|**Texto**|
|:-----|:-----|
|Sí  <br/> |The picture shows a cat, a dog, a fox, and a wolf.  <br/> |
|Sí (con lematización)  <br/> |Dogs, foxes, and wolves are canines, but cats are felines.  <br/> |
|No  <br/> |The picture shows a cat with a dog, a fox, and a wolf.  <br/> |
   
La expresión siguiente muestra como resultados todas las cadenas de la tabla anterior.
  
    
    
 `near(cat, dog, fox, wolf, N=5)`
  
    
    

#### Notas

 **Consideraciones sobre la distancia entre términos de NEAR/ONEAR**
  
    
    
 _N_ indica el número máximo de palabras que pueden aparecer entre los términos de consulta dentro del segmento coincidente del elemento. Si **NEAR** u **ONEAR** incluyen más de dos operandos, el número máximo de palabras permitidas entre los términos de consulta ( _N_) se cuenta dentro del segmento del elemento que coincide con todos los términos de **NEAR** u **ONEAR**.
  
    
    
 **NEAR** y **ONEAR** funcionan en texto ajustado por token. Esto significa que los caracteres especiales, como la coma (" **,** "), el punto (" **.** "), los dos puntos (" **:** ") y el punto y coma (" **;** "), se tratarán como espacios en blanco. El término "distancia" se refiere a los tokens que hay en el texto indizado.
  
    
    
Si usa **ONEAR** o **NEAR** con operandos iguales, el operador funcionará así:
  
    
    
 `near(a, a, n=x)`
  
    
    
Esta consulta siempre devolverá **true** si aparece al menos una instancia de " `a`" dentro del contexto. Esto también significa que **NEAR** no se puede usar como operador **COUNT**. Para más información sobre cómo contar repeticiones de términos, vea el operador  [COUNT](fast-query-language-fql-syntax-reference.md#fql_count_operator). 
  
    
    
 **NEAR** aplicado a frases también ofrecerá como resultado frases superpuestas en el texto.
  
    
    
Si un token del segmento coincidente coincide con más de un operando de la expresión **NEAR** u **ONEAR**, la consulta puede coincidir aunque el número de tokens no coincidentes que hay en el segmento coincidente supere el valor de ' _N_' en la expresión del operador **NEAR** u **ONEAR**. Por ejemplo, una superposición puede ser frases superpuestas. Si el número de coincidencias de superposición de token es ' `O`', la consulta coincidirá si no aparecen más de ' `N+O`' tokens no coincidentes dentro del segmento coincidente del elemento. 
  
    
    
 ** **NEAR** u **ONEAR** con **NOT****
  
    
    
El operador **NOT** no se puede usar dentro del operador **NEAR** u **ONEAR**. La siguiente sintaxis de FQL es incorrecta:
  
    
    
 `near(audi,not(bmw),n=2)`
  
    
    

### NOT
<a name="fql_not_operator"> </a>

Solo devuelve los elementos que no coinciden con el operando. Este puede ser cualquier expresión FQL válida.
  
    
    

#### Sintaxis

 `not(operand)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

### ONEAR
<a name="fql_onear_operator"> </a>

La variante ordenada de **NEAR**; requiere una coincidencia ordenada de los términos. El operador **ONEAR** se puede usar para restringir el conjunto de resultados a los elementos que tienen _N_ términos a cierta distancia unos de otros.
  
    
    

#### Sintaxis

 `onear(arg, arg [, arg]* [, N=<numeric value>])`
  
    
    

#### Parámetros



|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _N_ <br/> | _<valor_numérico>_ <br/> |Especifica el número máximo de palabras que pueden aparecer entre los términos (proximidad explícita).  <br/> Si **ONEAR** incluye más de dos operandos, el número máximo de palabras permitidas entre los términos ( _N_) se cuenta dentro de la expresión completa.  <br/> Valor predeterminado: **4** <br/> |
   

#### Ejemplos

 **Ejemplo 1.** La expresión siguiente muestra como resultados todas las repeticiones de las palabras "cat", "dog", "fox" y "wolf" que aparecen en orden si no están separadas por más de cuatro tokens indizados.
  
    
    
 `onear(cat, dog, fox, wolf)`
  
    
    
La tabla siguiente contiene ejemplos de valores de cadena de propiedades administradas; se indica si coinciden con la expresión anterior.
  
    
    


|**¿Coincide?**|**Texto**|
|:-----|:-----|
|Sí  <br/> |The picture shows a cat, a dog, a fox, and a wolf.  <br/> |
|No  <br/> |Dogs, foxes, and wolves are canines, but cats are felines.  <br/> |
|No  <br/> |The picture shows a cat with a dog, a fox, and a wolf.  <br/> |
   
 **Ejemplo 2.** La expresión siguiente ofrece como resultados (con lematización) el texto de la segunda fila de la tabla anterior.
  
    
    
 `onear(dog, fox, wolf, cat, N=5)`
  
    
    
 **Ejemplo 3.** La expresión siguiente ofrece como resultados el texto de la primera y la tercera fila de la tabla anterior.
  
    
    
 `onear(cat, dog, fox, wolf, N=5)`
  
    
    

#### Notas

Vea también  [NEAR](fast-query-language-fql-syntax-reference.md#fql_near_operator).
  
    
    

### O bien
<a name="fql_or_operator"> </a>

Solo devuelve los elementos que coinciden, al menos, con uno de los operandos **OR**. Los elementos que coinciden tienen una clasificación dinámica más alta (puntuación de relevancia en el conjunto de resultados) si coinciden más operandos **OR**. Los operandos pueden ser un solo término o cualquier subexpresión FQL válida.
  
    
    

#### Sintaxis

 `or(operand, operand [,operand]*)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

La siguiente expresión ofrece como resultados todos los elementos cuyo índice de texto completo predeterminado contiene "cat" o "dog". Si el índice de texto completo predeterminado de un elemento contiene "cat" y "dog", la clasificación dinámica será mayor y ofrecerá más resultados que si solo tuviese uno de los tokens.
  
    
    
 `or(cat, dog)`
  
    
    

### PHRASE
<a name="fql_phrase_operator"> </a>

Busca una cadena exacta de tokens. 
  
    
    
Los operandos **PHRASE** pueden ser términos únicos. Se pueden usar caracteres comodín.
  
    
    

#### Sintaxis

 `phrase(term [, term]*)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Notas

Vea también  [STRING](fast-query-language-fql-syntax-reference.md#fql_string_operator).
  
    
    

### RANGE
<a name="fql_range_operator"> </a>

Use el operador **RANGE** para propiedades administradas numéricas y de fecha y hora. El operador permite usar expresiones de coincidencia de rango.
  
    
    

#### Sintaxis

 `range(start, stop [,from="GE"|"GT"] [,to="LE"|"LT"])`
  
    
    

#### Parámetros



|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _start_ <br/> | _<valor_numérico>|<valor_fecha/hora>_ <br/> |Valor de inicio del rango.  <br/> Para especificar que el rango no tiene límite inferior, use la palabra reservada **min**. <br/> |
| _stop_ <br/> | _<valor_numérico>|<valor_fecha/hora>_ <br/> |Valor de finalización del rango.  <br/> Para especificar que el rango no tiene límite superior, use la palabra reservada **max**. <br/> |
| _from_ <br/> |**GE|GT** <br/> | Parámetro opcional que indica el intervalo de inicio de apertura o cierre. <br/>  Valores válidos: <br/> **GE**: superior o igual al valor de inicio (>= inicio del intervalo). <br/> **GT**: superior al valor de inicio (> inicio del intervalo). <br/>  Valor predeterminado: **GE** <br/> |
| _to_ <br/> |**LE|LT** <br/> | Parámetro opcional que indica el intervalo de finalización de apertura o cierre. <br/>  Valores válidos: <br/> **LE**: inferior o igual al valor de finalización (>= fin del intervalo). <br/> **LT**: inferior al valor de finalización (< fin del intervalo). <br/>  Valor predeterminado: **LT** <br/> |
   

#### Ejemplos

La expresión siguiente ofrece resultados que coinciden con una propiedad de descripción que empieza por la frase "olympic games" y que aparece en elementos que tienen 10.000 bytes como mínimo.
  
    
    
 `and(size:range(10000, max), description:starts-with("olympic games"))`
  
    
    

### STARTS-WITH
<a name="fql_startswith_operator"> </a>

Especifica una palabra o frase que deben aparecer al principio de una propiedad administrada.
  
    
    

#### Sintaxis

 `starts-with(<term or phrase>)`
  
    
    

#### Parámetros

No se aplica.
  
    
    

#### Ejemplos

La expresión siguiente ofrece elementos con los valores "Adam Jones sr" y "Adam Jones" en la propiedad administrada **author**. No mostrará resultados con el valor "Mr Adam Jones".
  
    
    
 `author:starts-with("adam jones")`
  
    
    

#### Notas

Si quiere más información sobre la coincidencia de límites, vea  [ENDS-WITH](fast-query-language-fql-syntax-reference.md#fql_endswith_operator).
  
    
    

### STRING
<a name="fql_string_operator"> </a>

Define una condición booleana coincidente para una cadena de texto.
  
    
    
El operando es una cadena de texto (uno o varios términos) que hay que encontrar. La cadena está seguida por cero o más parámetros. 
  
    
    
El operador **STRING** también se puede usar como conversión de tipo. La consulta `string("24.5")`, por ejemplo, tratará el valor numérico "24.5" como cadena de texto.
  
    
    

#### Sintaxis

 `string("<text string>"`
  
    
    
 ` [, mode=<mode>]`
  
    
    
 ` [, n=<near>]`
  
    
    
 ` [, weight=<n>]`
  
    
    
 ` [, linguistics=<on|off>]`
  
    
    
 ` [, wildcard=<on|off>])`
  
    
    

#### Parámetros



|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _mode_ <br/> | _<modo>_ <br/> | El parámetro _mode_ especifica cómo evaluar el valor <cadena de texto>. En la lista siguiente se recogen los valores válidos. <br/> **"PHRASE"** - `phrase(term [,term]*)` <br/> |**Moda**|**Expresión de operador equivalente**|
|:-----|:-----|
|**"PHRASE"** <br/> | `phrase(term [,term]*)` <br/> |
|**"AND"** <br/> | `and(term, term [,term]*)` <br/> |
|**"OR"** <br/> | `or(term, term [,term]*)` <br/> |
|**"ANY"** <br/> | `any(term, term [,term]*)` <br/> |
|**"NEAR"** <br/> | `near(term, term [,term]*, N)` <br/> |
|**"ONEAR"** <br/> | `onear(term, term [,term]*, N)` <br/> |
   
 Valor predeterminado: **"PHRASE"** <br/> |
| _n_ <br/> | _<valor_numérico>_ <br/> |Este parámetro indica la distancia máxima entre términos para  _mode_= **"NEAR"** u _mode_= **"ONEAR"**. <br/> Las expresiones siguientes son equivalentes:  <br/>  `string("hello world", mode="NEAR", n=5)` <br/>  `near(hello, world, n=5)` <br/> Valor predeterminado: **4** <br/> |
| _weight_ <br/> | _<valor_numérico>_ <br/> |Este parámetro es un valor numérico positivo que indica el peso del término en la clasificación dinámica.  <br/> Un valor pequeño indica que un término debe contribuir menos a la clasificación. Un valor mayor indica que debe contribuir más a la clasificación. Un valor de cero en el parámetro del peso especifica que un término no debe afectar a la clasificación dinámica.  <br/> El parámetro  _weight_ se aplica a todos los términos de la expresión **STRING**. <br/> > **SUGERENCIA**> El parámetro del peso afecta solo a las consultas de índice de texto completo.           Valor predeterminado: **100**. <br/> |
| _linguistics_ <br/> |**on|off** <br/> |Deshabilita/habilita todas las funciones lingüísticas de la cadena (lematización, sinónimos, revisión ortográfica) si están habilitadas para la consulta.  <br/> Puede usar este parámetro para desactivar el procesamiento lingüístico en un término o una cadena concretos y que estos sigan contribuyendo a la clasificación.  <br/> Valor predeterminado: **"ON"** <br/> |
| _wildcard_ <br/> |**on|off** <br/> | Este parámetro controla la expansión de carácter comodín de los términos que hay en la _<cadena de texto>_. Esta opción invalida las opciones de caracteres comodín que hay en los parámetros de consulta y permite que se habiliten o deshabiliten los caracteres comodín en ciertas partes de la consulta.  <br/>  Los valores válidos son los siguientes: <br/> **"ON"**: especifica que el carácter " *****" se evalúa como carácter comodín. Un carácter " *****" coincide con cero o más caracteres.  <br/> **"OFF"**: especifica que el carácter " *****" no se evalúa como carácter comodín.  <br/>  Valor predeterminado: **"ON"** <br/> |
   

> **NOTA**
> En SharePoint 2013, los parámetros  _minexpansion_,  _maxexpansion_ y _annotation_class_ para el operador **STRING** están obsoletos.
  
    
    


#### Ejemplos

 **Ejemplo 1.** Como el modo de cadena predeterminado es " **PHRASE** ", todas las expresiones siguientes devuelven los mismos resultados.
  
    
    
 `"what light through yonder window breaks"string("what light through yonder window breaks")string("what light through yonder window breaks", mode="phrase")phrase(what, light, through, yonder, window, breaks)`
  
    
    
 **Ejemplo 2.** La siguiente expresión de token de cadena y la expresión del operador **AND** devuelven los mismos resultados.
  
    
    
 `string("cat dog fox", mode="and")and(cat, dog, fox)`
  
    
    
 **Ejemplo 3.** La siguiente expresión de token de cadena y la expresión del operador **OR** devuelven los mismos resultados.
  
    
    
 `string("coyote saguaro", mode="or")or(coyote, saguaro)`
  
    
    
 **Ejemplo 4.** La siguiente expresión de token de cadena y la expresión del operador **ANY** devuelven los mismos resultados.
  
    
    
 `string("coyote saguaro", mode="any")any(coyote, saguaro)`
  
    
    
 **Ejemplo 5.** La siguiente expresión de token de cadena y la expresión del operador **NEAR** devuelven los mismos resultados.
  
    
    
 `string("coyote saguaro", mode="near")near(coyote, saguaro)`
  
    
    
 **Ejemplo 6.** La siguiente expresión de token de cadena y la expresión del operador **NEAR** devuelven los mismos resultados.
  
    
    
 `string("cat dog fox wolf", mode="near", N=4)near(cat, dog, fox, wolf, N=4)`
  
    
    
 **Ejemplo 7.** La siguiente expresión de token de cadena y la expresión del operador **ONEAR** devuelven los mismos resultados.
  
    
    
 `string("cat dog fox wolf", mode="onear")onear(cat, dog, fox, wolf)`
  
    
    
 **Ejemplo 8.** La siguiente expresión de token de cadena ofrece como resultado la palabra "nobler" con las funciones lingüísticas deshabilitadas para que no se muestren otras formas de la palabra (como "ennobling") por la lematización.
  
    
    
 `string("nobler", linguistics="off")`
  
    
    
 **Ejemplo 9.** La expresión siguiente ofrece como resultado elementos que contienen "cat" o "dog ", pero la expresión aumenta la clasificación dinámica de elementos que contiene "dog" más que los elementos que contienen "cat".
  
    
    
 `or(string("cat", weight="200"), string("dog", weight="500"))`
  
    
    

#### Notas

 **Peso de relevancia en la clasificación dinámica**
  
    
    
El principal efecto del parámetro **weight** se ejerce en las consultas **OR**, aunque también puede tener algún efecto en las consultas **AND**. El algoritmo de clasificación dinámica puede implicar que términos distintos ofrezcan contribuciones de clasificación diferentes en función del lugar del elemento donde aparezca la coincidencia de término.
  
    
    
La diferencia en la contribución de clasificación también se puede basar en la frecuencia del término y en la frecuencia inversa del elemento. Lo siguiente es un ejemplo:
  
    
    

- Consulta:  `and(string("a"), string("b", weight=200))`
    
  
- Esquema de índice: la propiedad administrada **title** tiene más peso que la propiedad administrada **body**.
    
  
- El elemento de índice 1 incluye el término "a" en el título y el término "b" en el cuerpo. 
    
  
- El elemento de índice 2 incluye el término "a" en el cuerpo y el término "b" en el título. 
    
  
En este ejemplo, el elemento 2 tiene la clasificación total más alta, ya que los elementos con mayor contribución de clasificación dinámica se incrementarán todavía más.
  
    
    

> **SUGERENCIA**
> El aumento de término relativo (positivo o negativo) se aplica al componente de clasificación dinámica de la clasificación total. Sin embargo, los cálculos de clasificación por aumento de proximidad (distancia entre palabras) no se ven afectados por la ponderación de los términos. La ponderación relativa no siempre implica que la clasificación total del elemento se modifique según el porcentaje dado. > Con la consulta siguiente buscamos los términos "peter", "paul" o "mary", donde "peter" tendrá el doble de contribución de clasificación que los otros dos términos. >  `or(peter, string("paul mary", mode="OR", weight=50))`
  
    
    

 **Cadenas con caracteres especiales**
  
    
    
Los caracteres especiales como la coma (","), el punto y coma (";"), los dos puntos (":"), el punto ("."), el menos ("-"), el subrayado ("_") o la barra diagonal ("/") se tratan como espacios en blanco dentro de una expresión de cadena encerrada entre comillas dobles. Esto se debe al proceso de ajuste por token. Estos caracteres también implican una formulación implícita de los tokens separados por estos caracteres. 
  
    
    
Las expresiones de consulta siguientes son equivalentes:
  
    
    
 `title:string("animals birds", mode="phrase")title:"animals/birds"title:string("animals/birds", mode="and")title:string("animals/birds", mode="or")`
  
    
    
Las expresiones de consulta siguientes son equivalentes:
  
    
    
 `title:or(string("animals birds", mode="phrase"), string("animals insects", mode="phrase"))title:string("animals/birds animals/insects", mode="or")`
  
    
    
Las expresiones de consulta siguientes son equivalentes:
  
    
    
 `body:string("help contoso com", mode="phrase")body:string("help@contoso.com")`
  
    
    
 **Coincidencia de frases con ajuste por token**
  
    
    
Puede buscar una cadena exacta de tokens usando el operador **STRING** con _mode_="phrase" o el operador **PHRASE**.
  
    
    
Todas estas operaciones de frase implican una coincidencia de frase por ajuste de token. Esto significa que los caracteres especiales como la coma (" **,** "), el punto y coma (" **;** "), los dos puntos (" **:** "), el subrayado (" **_** "), el menos (" **-** ") y la barra diagonal (" **/** ") se tratan como espacios en blanco. Esto se debe al proceso de ajuste por token.
  
    
    

### XRANK
<a name="fql_xrank_operator"> </a>

Aumenta la clasificación dinámica de los elementos según las repeticiones de ciertas términos dentro de la  _match expression_ sin cambiar qué elementos coinciden con la consulta. Una expresión **XRANK** consta un componente que debe coincidir, la _match expression_ y uno o varios componentes que contribuyen solo a la clasificación dinámica, la _rank expression_. Al menos **uno** de los parámetros, excepto _n_, debe especificarse para que una expresión XRANK sea válida.
  
    
    
Las  _Match expressions_ pueden ser cualquier expresión FQL válida, incluidas las expresiones **XRANK** anidadas. Las _Rank expressions_ pueden ser cualquier expresión FQL válida sin expresiones **XRANK**. Si sus consultas FQL tienen varios operadores **XRANK**, el valor de la clasificación dinámica final se calcula como una suma de aumentos entre todos los operadores **XRANK**.
  
    
    

> **NOTA**
> En SharePoint Server 2010, el operador **XRANK** tiene dos parámetros: _boost_ y _boostall_, así como la sintaxis siguiente:  `xrank(operand, rank-operand [, rank-operand]* [,boost=n] [,boostall=yes])`. Esta sintaxis y sus parámetros están obsoletos en SharePoint Server 2013. Recomendamos usar la sintaxis y los parámetros nuevos en lugar de estos. 
  
    
    


#### Sintaxis

 `xrank(<match expression> [, <rank-expression>]*, rank-parameter[, rank-parameter]*)`
  
    
    

#### Fórmula


  
    
    
![Fórmula del operador XRANK](images/XRANKFormula.gif)
  
    
    

  
    
    

  
    
    

#### Parámetros



|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _N_ <br/> | _<integer_value>_ <br/> |Especifica el número de resultados del cual se computan estadísticas.  <br/> Este parámetro no afecta el número de resultados al que contribuye la clasificación dinámica; es solo un medio de excluir elementos irrelevantes de los cálculos de estadísticas.  <br/> Valor predeterminado: **0**. Un valor cero significa *todos los documentos*  . <br/> |
| _Nb_ <br/> | _<float_value>_ <br/> |El parámetro  _nb_ se refiere al aumento normalizado. Este parámetro especifica el factor que se multiplica con el producto de la puntuación promedio y de variación de los valores de clasificación del conjunto de resultados. <br/>  _f_ en la fórmula XRANK. <br/> |
   
Normalmente, el aumento normalizado,  _nb_, es el único parámetro que se modifica. Este parámetro proporciona el control necesario para promover o degradar un elemento determinado sin tener en cuenta la desviación estándar. 
  
    
    

#### Parámetros avanzados

Los siguientes parámetros avanzados también están disponibles, aunque no suelen usarse.
  
    
    


|**Parámetro**|**Valor**|**Descripción**|
|:-----|:-----|:-----|
| _cb_ <br/> | _<float_value>_ <br/> |El parámetro  _cb_ se refiere al aumento constante. <br/> Valor predeterminado: **0**. <br/>  _a_ en la fórmula XRANK. <br/> |
| _stdb_ <br/> | _<float_value>_ <br/> |El parámetro  _stdb_ se refiere al aumento de desviación estándar. <br/> Valor predeterminado: **0**. <br/>  _e_ en la fórmula XRANK. <br/> |
| _avgb_ <br/> | _<float_value>_ <br/> |El parámetro  _avgb_ se refiere al aumento promedio. Este factor se multiplica por el valor de clasificación promedio del conjunto de resultados. <br/> Valor predeterminado: **0**. <br/>  _d_ en la fórmula XRANK. <br/> |
| _rb_ <br/> | _<float_value>_ <br/> |El parámetro  _rb_ se refiere al aumento de rango. Este factor se multiplica con el rango de valores de clasificación en el conjunto de resultados. <br/> Valor predeterminado: **0**. <br/>  _b_ en la fórmula XRANK. <br/> |
| _pb_ <br/> | _<float_value>_ <br/> |El parámetro  _pb_ se refiere al aumento de porcentaje. Este factor se multiplica por la propia clasificación del elemento en comparación con el valor mínimo del corpus. <br/> Valor predeterminado: **0**. <br/>  _c_ en la fórmula XRANK. <br/> |
   

#### Ejemplos

 **Ejemplo 1.** La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "cat" o "dog". La expresión incrementa la clasificación dinámica de aquellos elementos con un incremento constante de 100 para elementos que también contienen "thoroughbred".
  
    
    
 `xrank(or(cat, dog), thoroughbred, cb=100)`
  
    
    
 **Ejemplo 2.** La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "cat" o "dog". La expresión incrementa la clasificación dinámica de aquellos elementos con un incremento normalizado de 1,5 para elementos que también contienen "thoroughbred".
  
    
    
 `xrank(or(cat, dog), thoroughbred, nb=1.5)`
  
    
    
 **Ejemplo 3.** La siguiente expresión ofrece como resultados los elementos cuyo índice de texto completo predeterminado contiene "cat" o "dog". La expresión incrementa la clasificación dinámica de aquellos elementos con un incremento constante de 100 y un incremento normalizado de 1,5 para elementos que también contienen "thoroughbred".
  
    
    
 `xrank(or(cat, dog), thoroughbred, cb=100, nb=1.5)`
  
    
    
 **Ejemplo 4.** La siguiente expresión ofrece como resultados todos los elementos que contienen el término "animals" y aumenta la clasificación dinámica de la siguiente manera:
  
    
    

- La clasificación dinámica de los elementos que contienen el término "dogs" aumenta 100 puntos.
    
  
- La clasificación dinámica de los elementos que contienen el término "cats" aumenta 200 puntos.
    
  
- La clasificación dinámica de los elementos que contienen tanto "dogs" como "cats" aumenta 300 puntos.
    
  
 `xrank(xrank(animals, dogs, cb=100), cats, cb=200)`
  
    
    

## Recursos adicionales
<a name="SP15FQL_addlresources"> </a>


-  [Generar consultas de búsqueda en SharePoint 2013](building-search-queries-in-sharepoint-2013.md)
    
  
