---
title: Cómo Crear tipos de contenido externo para SQL Server en SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 7fe2fbf0-546b-4a16-b02e-a0960bfb3842
---


# Cómo: Crear tipos de contenido externo para SQL Server en SharePoint 2013
Obtenga información sobre cómo crear un tipo de contenido externo para SQL Server en SharePoint 2013.
Crear un tipo de contenido externo es una tarea dinámica cuando se trabaja con datos externos. Un tipo de contenido externo contiene información importante sobre conexiones, acceso, métodos de operación, columnas, filtros y otros metadatos que se usan para recuperar los datos del origen de datos externo.
  
    
    


## Antes de empezar
<a name="section1"> </a>

Trabajar con datos externos requiere varias tareas como requisito previo para habilitar el acceso seguro a los datos. La siguiente información puede ayudarle a planear los pasos siguientes. Además, si tiene problemas al tratar de trabajar con datos externos, esta información puede ayudarle a identificar el problema. Para obtener acceso a datos externos, usted o un administrador debe hacer lo siguiente:
  
    
    
 **Preparar SQL Server** Un administrador de base de datos debe proporcionar permisos para asegurarse de que las personas adecuadas tienen acceso a los datos y que esos datos no acaban en manos equivocadas. El administrador de base de datos también debe crear una cuenta de SQL Server con permisos de db_owner. Es posible que el administrador de base de datos también quiera crear tablas, vistas, consultas, alias de columnas específicas, y limitar los resultados a solo lo necesario y para ayudar a mejorar el rendimiento.
  
    
    
 **Configurar servicios de SharePoint** Un administrador debe activar Servicios de conectividad empresarial (BCS) y Servicio de almacenamiento seguro.
  
    
    
 **Configurar el servicio de almacenamiento seguro** Un administrador debe determinar el mejor modo de acceso a origen de datos externo, crear una aplicación de destino y establecer las credenciales de la aplicación de destino.
  
    
    
 **Configurar los servicios de conectividad a datos empresariales** Un administrador debe asegurarse de que el usuario que crea el tipo de contenido externo tiene permiso para el almacén de metadatos de la conectividad a datos empresariales (BDC) y que los usuarios adecuados tienen acceso al tipo de contenido externo en el que se basa la lista externa.
  
    
    
 **Asegúrese de que Office 2013 está listo para usar** Para sincronizar datos externos con productos de Office 2013, debe usar Windows 7 o una versión posterior y asegúrese de que la opción para la instalación de Office Servicios de conectividad empresarial (BCS) está habilitada (es el valor predeterminado). Esta opción instala Tiempo de ejecución del cliente de Servicios de conectividad empresarial que hace lo siguiente: almacena en caché y se sincroniza con datos externos, asigna datos empresariales a tipos de contenido externo, muestra el selector de elementos externos en productos de Office y ejecuta soluciones personalizadas en los productos de Office. También debe tener SQL Server Compact 4.0, .NET Framework 4 y WCF Data Services 5.0 para OData V3 en cada equipo cliente (si es necesario, se le pedirá automáticamente que descargue el software).
  
    
    

## Definir información general
<a name="section2"> </a>


1. Inicie Microsoft SharePoint Designer 2013.
    
  
2. Haga clic en **Abrir sitio**y luego escriba el nombre de sitio adecuado.
    
  
3. En el panel **Navegación**, en **objetos de sitio**, seleccione **Tipos de contenido externo**.
    
    > **NOTA**
      > SharePoint Designer 2013 agrupa de tipos de contenido externo por espacio de nombres en la ventana inicial del diseñador de tipo de contenido externo. 
4. Para abrir el diseñador externo de tipo de contenido, en la cinta de opciones, haga clic en **Tipo de contenido externo**.
    
  
5. En la página **Nuevo tipo de contenido externo**, haga lo siguiente:
    
  - Junto a **Nombre**, haga clic en **Nuevo tipo de contenido externo**y luego escriba un nombre único para el tipo de contenido externo.
    
  
  - Junto a **Nombre para mostrar**, escriba otro nombre si desea que un nombre más descriptivo.
    
  

## Definir opciones generales y comportamientos de Office
<a name="section3"> </a>


1. En la lista desplegable **Tipo de elemento de Office**, seleccione una de las siguientes opciones:
    
  - **Lista genérica** Seleccione esta opción para cualquier tipo de lista.
    
  
  - **Cita, contacto, tarea o publicación** Seleccione esta opción si va a crear una lista que se comporta como un elemento de contacto de Outlook, tarea, cita o publicación. El tipo de elemento de Office que seleccione aquí determina el comportamiento de Outlook que desea asociar al tipo de contenido externo. Por ejemplo, un tipo de contenido externo de cliente se comporta como un elemento de contacto nativo en Outlook.
    
  
2. En la casilla **Sincronización sin conexión para la lista externa**, asegúrese de que **Habilitada** está activada, que es el valor predeterminado.
    
    > **NOTA**
      > Si deshabilita esta opción, el comando **Conectar SharePoint a Outlook** no está disponible para una lista externa.

> **NOTA**
> La característica Granja de servidores y sitios, **Sincronización sin conexión para la lista externa**, también debe estar activada. Esta característica está activada en el nivel de granja de servidores de forma predeterminada, pero no está activada en el nivel de sitio de forma predeterminada. 
  
    
    


## Crear una conexión a los datos externos
<a name="section4"> </a>


1. Para especificar la base de datos SQL Server para el tipo de contenido externo, haga clic en **Haga clic aquí para detectar orígenes de datos externos y definir operaciones.**.
    
  
2. Haga clic en **Agregar conexión**, seleccione **SQL Server** en cuadro de diálogo **Selección de tipo de origen de datos externo** y haga clic en **Aceptar**.
    
  
3. En el cuadro de diálogo **Conexión de SQL Server**, escriba el nombre del servidor, el nombre de la base de datos, una descripción opcional y luego haga clic en **Aceptar**.
    
  
4. Para elegir un modo de autenticación, seleccione una de las siguientes opciones:
    
  - **Conectar con identidad de usuario** Utiliza el modo de autenticación de paso a través.
    
  
  - **Conectar con identidad de Windows suplantada** Utiliza el modo de autenticación WindowsCredentials.
    
  
  - **Conectar con identidad de personalizada suplantada** Utiliza el modo de autenticación RDBCredentials.
    
  
5. En el cuadro **Id. de aplicación de almacenamiento seguro**, escriba el nombre del identificador de aplicación de destino creado en Servicio de almacenamiento seguro.
    
  
6. Haga clic en **Aceptar**.
    
  
SharePoint Designer 2013 valida y comprueba la información de conexión. Si ve algún mensaje, debe resolverlo antes de continuar.
  
    
    

## Seleccionar una tabla, vista o rutina
<a name="section5"> </a>


1. En el Explorador de origen de datos, expanda la base de datos para ver las tablas, las vistas y las rutinas que contiene.
    
  
2. Seleccione una tabla, vista o rutina.
    
  

## Definir operaciones
<a name="section6"> </a>


1. En el Explorador de origen de datos de la tabla, la vista o la rutina haga clic con el botón secundario y luego seleccione una de las siguientes opciones:
    
  - **Crear todas las operaciones** Define una operación crear elemento, eliminar elemento, leer elemento, leer lista y actualizar elemento.
    
    > **NOTA**
      > **Crear todas las operaciones** Solo está disponible para tablas y vistas. Rutinas requieren operaciones específicas.
  - **Nueva operación Leer elemento** Define una operación leer elemento.
    
  
  - **Nueva operación Leer lista** Define una operación de leer lista.
    
  
  - **Nueva operación Actualizar** Define una operación de actualizar elemento.
    
  
  - **Nueva operación Eliminar elemento** Define una operación de eliminar elemento.
    
  
  - **Actualizar** Actualiza la lista de tablas, vistas y rutinas en el Explorador de origen de datos.
    
  
2. Haga clic en **Siguiente**.
    
  
 **Notas**
  
    
    

- En las vistas que abarcan varias tablas, asegúrese de que se admiten las operaciones de escritura. De lo contrario, **Crear todas las operaciones** o **Nueva operación Actualizar** pueden producir un error.
    
  
- Defina siempre al menos una **Nueva operación Leer elemento** y **Nueva operación Leer lista** porque características de SharePoint, como listas externas, se basan en estas operaciones.
    
  
- Elija operaciones específicas, en lugar de **Crear todas las operaciones**, cuando la tabla o vista no admita determinadas operaciones.
    
  

## Agregar columnas
<a name="section7"> </a>


1. Para especificar las columnas que quiere mostrar en la tabla o la vista, haga clic en **siguiente**.
    
  
2. En el cuadro de diálogo **Parámetros de configuración**, todas las columnas (conocidas como **Elementos de origen de datos**) están seleccionadas de forma predeterminada. Para quitar las columnas innecesarias, desactive las casillas correspondientes.
    
    > **NOTA**
      > A diferencia de una lista de SharePoint nativa, no puede cambiar el nombre de columna de una lista externa. Considere la posibilidad de usar un alias de columna SQL para proporcionar un nombre más significativo o un nombre más corto. 
3. Para seleccionar un campo de identificador, haga clic y resalte un campo (normalmente un campo de único valor) y luego en **Propiedades**, haga clic en **Asignar a identificador**.
    
  

> **IMPORTANTE**
> Para impedir la actualización de determinados campos, como un identificador o el campo de clave principal, desactive la casilla **Obligatorio**, pero active la casilla **Solo lectura**, que es necesaria para recuperar los elementos y así poder actualizar otros campos. 
  
    
    

  
    
    


> **SUGERENCIA**
> Lea siempre detenidamente los mensajes del panel **Errores y advertencias**. Proporcionan información útil para confirmar las acciones o solucionar problemas. Haga clic periódicamente en el panel **Errores y advertencias** y asegúrese de que no hay más errores o advertencias.
  
    
    


## Asignar campos de Outlook
<a name="section8"> </a>

Si su tipo de contenido externo se asigna a un tipo de elemento de Outlook, debe asignar uno o más campos de tipo de contenido externo a los campos del elemento de Outlook. Al asignar un tipo de contenido externo, como un cliente a un tipo de elemento de Outlook, como un contacto, debe asignar cada campo en el tipo de contenido externo, como el nombre del cliente, los apellidos, la dirección y el teléfono, en sus respectivos ámbitos de tipo de elemento de Outlook, como un contacto Nombre, Apellidos, Dirección y Teléfono del cliente explícitamente.
  
    
    

- Para cada campo, haga lo siguiente:
    
1. Haga clic y resalte el campo.
    
  
2. En **Propiedades**, junto a **Propiedad Office**, haga clic en la flecha hacia abajo. y luego seleccione el campo coincidente adecuado. 
    
  

> **NOTA**
> No es necesario asignar todos los campos correspondientes. Sin embargo, se deben asignar los campos mostrados en la tabla siguiente. 
  
    
    


**Tabla: Tipo de elemento de Outlook asignado al campo de elemento de Outlook**


|**Tipo de elemento de Outlook**|**Campo de elemento de Outlook**|
|:-----|:-----|
|Contacto  <br/> |Apellidos  <br/> |
|Tarea  <br/> |Asunto  <br/> |
|Cita  <br/> |Inicio, fin y asunto  <br/> |
|Publicación  <br/> |Asunto  <br/> |
   
Los campos sin asignar, según el número, se muestran como propiedades extendidas de la forma siguiente:
  
    
    

- **Adyacente** Anexado al área de formulario en la parte inferior de la página predeterminada de un formulario de Outlook (campos de dos a cinco).
    
  
- **Independiente** Agregado como una nueva página a un formulario de Outlook (campo seis en adelante).
    
  

## Configurar el control del selector de elementos externos
<a name="section9"> </a>

El control del selector de elementos externos permite a los usuarios seleccionar un campo, como un campo de identificador o un campo, que tiene valores únicos para que elija fácilmente un elemento. Este control está disponible en SharePoint y en los productos de Office 2013. Por ejemplo, los usuarios pueden usar este control para elegir un elemento de una lista externa de clientes y Word 2013 habilita este control para su uso con controles de contenido que están vinculados a columnas de datos externos. Una buena idea es seleccionar columnas específicas que quiera mostrar en el control del selector de elementos externos porque la operación predeterminada es mostrar todas las columnas, que en la mayoría de los casos, no es necesario.
  
    
    

1. Para cada campo que desee que aparezca en el selector de elementos de contenido externo, haga clic y resalte el campo y luego, en **Propiedades**, haga clic en la casilla **Mostrar en el selector**.
    
  
2. Haga clic en **Siguiente**.
    
  

> **NOTA**
> Se muestran todos los filtros definidos en el control de selector de elementos externos. Aunque no puede quitar filtros específicos del control del selector de elementos externos, puede definir un filtro predeterminado, para ello, haga clic en **Predeterminado** en el cuadro de diálogo **Configuración del filtro** cuando vaya a crear o modificar el filtro.
  
    
    


## Definir filtros
<a name="section10"> </a>

Si no define un filtro, una lista externa devuelve todos los datos hasta el límite de Servicios de conectividad empresarial (BCS) (2.000 elementos de forma predeterminada) o todos los datos que se definen en el tipo de contenido externo si es menor que el límite actual. Además, todo el procesamiento de resultados se produce dentro de los productos de SharePoint. Una buena idea es definir al menos un filtro. En general, existen dos tipos de filtros que se deben tener en cuenta:
  
    
    

- **Filtro de origen de datos** Al crear un filtro de tipo de contenido externo, conocido como filtro de origen de datos, la operación de filtrado se produce dentro de la base de datos de SQL Server. Esto es importante cuando se trabaja con una gran cantidad de datos ya se puede descargar el procesamiento de los productos de SharePoint a la base de datos externa y obtener mejoras de rendimiento. Después de crear la lista externa, puede usar el filtro de origen de datos mediante la creación de una vista que especifique valores de filtro diferentes en la sección **Filtro de origen de datos** de la página **Vista de lista** de la configuración.
    
  
- **Filtro de SharePoint** Los usuarios aún pueden filtrar los datos mediante un filtro de SharePoint, el filtro de encabezado de columna o con la sección **Filtros** en la página **Vista de lista** de la configuración. En este caso, la operación de filtrado se produce dentro de los productos de SharePoint, no dentro de la base de datos de SQL Server.
    
  
Una buena estrategia a tener en cuenta consiste en crear un conjunto de vistas de listas externas basadas en filtros de origen de datos específicos que garanticen el filtrado de grandes cantidades de datos en primer lugar en el origen de datos externos y luego los usuarios pueden filtrar y refinar los resultados mediante más filtros de SharePoint.
  
    
    
Puede crear diferentes tipos de filtros. Para cada filtro que cree, haga lo siguiente:
  
    
    

1. En **Propiedades**, junto a **Elemento de origen de datos**, seleccione un campo.
    
  
2. En **Propiedades**, junto a **Filtro**, haga clic en **Haga clic para agregar** para mostrar el cuadro de diálogo **Configuración del filtro**.
    
  
3. Haga clic en **Parámetro para agregar filtro**.
    
  
4. En el cuadro **Nuevo filtro**, escriba un nombre de filtro.
    
  
5. En el cuadro **Tipo de filtro**, seleccione un tipo de filtro:
    
    
  
    
    
 **Comparaciones**
  
    
    

    
Un filtro de comparación limita los elementos devueltos según una condición (como Estado = "Nueva Jersey") y se convierte en una cláusula WHERE de SQL.
    
1. En el cuadro **Tipo de filtro**, seleccione **Comparación**.
    
  
2. En el cuadro **Operador**, seleccione una operación.
    
  
3. En el cuadro **Campo de filtro**, asegúrese de que está seleccionado el campo que desea comparar.
    
  
4. Si quiere que los valores escritos por el usuario coincidan por mayúsculas y minúsculas, haga clic en **Distinguir mayúsculas de minúsculas**.
    
  
5. Si quiere mostrar una lista de posibles coincidencias en el control del selector de elementos externos cuando haya más de un elemento coincidente, seleccione **Usar para crear lista de coincidencia en el selector de elementos externo**.
    
  
6. Haga clic en **Aceptar**.
    
  
7.  En **Propiedades**, junto a **Valor predeterminado**, escriba el valor inicial por el que quiere filtrar, si el usuario no escribe un valor. Si no escribe ningún valor, la lista externa no mostrará ningún elemento.
    
  

    
  
    
    
 **Caracteres comodín**
  
    
    

    
Un filtro comodín limita los elementos devueltos según un valor de cadena especificado por el usuario (por ejemplo, estado contiene "Nuevo") y se convierte en una cláusula SQL como. Los caracteres comodín válidos en SQL Server son * (asterisco), que significa que coincide con cualquier número de caracteres y _ (subrayado), que significa que coinciden con un carácter y solo uno. 
  
    
    

    
1. En el cuadro **Tipo de filtro**, seleccione **Comodín**.
    
  
2. En el **Campo de filtro**, seleccione un campo.
    
  
3. Si quiere que los valores escritos por el usuario coincidan por mayúsculas y minúsculas, haga clic en **Distinguir mayúsculas de minúsculas**.
    
  
4. Si quiere mostrar una lista de posibles coincidencias en el control del selector de elementos externos cuando haya más de un elemento coincidente, seleccione **Usar para crear lista de coincidencia en el selector de elementos externo**.
    
  
5. Haga clic en **Aceptar**.
    
  
6.  En **Propiedades**, junto a **Valor predeterminado**, escriba el valor inicial que quiere filtrar, si el usuario escribe un valor. Si no especifica ningún valor, la lista externa no mostrará ningún elemento. Una buena idea es escribir un * (asterisco) como valor predeterminado.
    
  

    
  
    
    
 **Límite**
  
    
    

    
En la mayoría de los casos, debe definir un filtro de límite para las operaciones de leer y leer lista. Si no define un valor de **Límite predeterminado**, no se recuperan datos del origen de datos externo. Asegúrese de que el valor predeterminado que se especifica para el filtro de límite es inferior a 2.000, ya que la limitación predeterminada de Servicios de conectividad empresarial (BCS) es 2.000 elementos. Puede aumentar este límite si es necesario.
    
1. En el cuadro **Tipo de filtro**, seleccione **Límite**. 
    
  
2. En el cuadro **Número**, escriba un número.
    
  
3. Si quiere mostrar una lista de posibles coincidencias en el control del selector de elementos externos cuando haya más de un elemento coincidente, seleccione **Usar para crear lista de coincidencia en el selector de elementos externo**.
    
  
4. En **Propiedades**, junto a **Valor predeterminado**, escriba el valor inicial por el que quiere filtrar, si el usuario no escribe un valor. Si no escribe ningún valor, la lista externa no mostrará ningún elemento.
    
  
5. Haga clic en **Aceptar**. 
  
    
    

    
  

    > **NOTA**
      > Es posible que el administrador de base de datos de SQL Server quiera crear tablas específicas, vistas, índices y consultas optimizadas para limitar los resultados a solo lo necesario y ayudar a mejorar el rendimiento. 

    
  
    
    
 **Número de página**
  
    
    

    
    Use el número de página de tipo de contenido externo para sustituir el límite de la página de SharePoint definido en la página **Vista de lista** de la lista externa. Aquí esta la diferencia:
    
  - El tipo de contenido externo de número de página procesa primero los resultados de la base de datos de SQL Server y luego devuelve y muestra solo el número de filas determinado por el valor de tamaño de página.
    
  
  - El límite de la página de SharePoint devuelve todas las filas hasta el tamaño de valor predeterminado de la base de datos de SQL Server y luego muestra el número de filas determinado por el valor de límite de página de SharePoint de la página de configuración **Vista de lista**.
    
  

    Usar el tipo de contenido externo de número de página generalmente es más eficaz. Además, el valor **Orden** ayuda garantizar una presentación exacta de los resultados devueltos.
    
1. En el cuadro **Tipo de filtro**, seleccione **Número de página**. 
    
  
2. En el cuadro **Tamaño de página**, escriba un número.
    
  
3. En el cuadro **Orden**, seleccione una dirección para el orden.
    
  
4. Haga clic en **Aceptar**. 
    
  
5. En **Propiedades**, junto a **Valor predeterminado**, escriba el valor inicial que desea filtrar, si el usuario escribe un valor. Si no especifica ningún valor, la lista externa no mostrará ningún elemento. 
  
    
    

    
  
6. Si quiere mostrar, pero no modificar un campo, haga clic y resalte el campo y luego, en **Propiedades**, seleccione **Solo lectura**.
    
  
7. Si quiere asegurarse de que un campo siempre tiene un valor cuando se ha creado o modificado, haga clic y resalte el campo y luego, en **Propiedades**, seleccione **Obligatorio**.
    
  
8. Para proporcionar un nombre descriptivo en el control del selector de elementos externos del nombre **Elemento de origen de datos**, que se deriva del nombre de columna de SQL Server, haga clic y resalte el campo y luego, en **Propiedades**, en el cuadro **Nombre para mostrar**, escriba un nombre.
    
  
9. Para definir un valor predeterminado para el filtro (que también se muestra en la sección **Filtro de origen de datos** de la página de configuración **Vista de lista**), en **Parámetros de filtro**, haga clic y resalte el filtro y luego, en el cuadro **Valor predeterminado**, escriba un valor apropiado. Si no escribe un valor, cuando se utilice el filtro por primera vez, no se mostrará ningún elemento.
    
  

## Establecer el campo de título de una lista externa
<a name="section11"> </a>


1. En la cinta, haga clic en **Vista de resumen**.
    
  
2. En la sección **Campos**, en **Nombre de campo**, seleccione un campo.
    
    > **IMPORTANTE**
      > En general, una buena idea es que el título de un campo tenga un valor único. El campo Título se usa para mostrar la lista o los formularios de InfoPath. Una vez establecido el campo Título, no se puede cambiar. 
3. En la cinta, haga clic en **Establecer como título**.
    
  

## Completar el tipo de contenido externo
<a name="section12"> </a>


- En la barra de herramientas de acceso rápido, haga clic en **Guardar**. La definición de tipo de contenido externo se guarda en el almacén de metadatos de conectividad a datos empresariales.
    
  

> **NOTA**
> Para proporcionar un mejor rendimiento, la conectividad a datos empresariales almacena todos los objetos del almacén de metadatos y actualiza los cambios realizados mediante un trabajo del temporizador que se ejecuta cada minuto. Que los cambios se propaguen a todos los servidores de la granja de servidores puede tardar hasta un minuto, pero los cambios son inmediatos en el servidor donde se realiza el cambio. 
  
    
    

El tipo de contenido externo ahora está disponible para su uso en SharePoint y en los productos de Office 2013.
  
    
    

## Recursos adicionales
<a name="AR"> </a>


-  [Tipos de contenido externo en SharePoint 2013](external-content-types-in-sharepoint-2013.md)
    
  
-  [Deploy a Business Connectivity Services on-premises solution in SharePoint 2013](http://msdn.microsoft.com/library/4692ebba-90eb-4d72-ac12-e90c4d4ee7ae.aspx)
    
  
-  [Uso de orígenes OData con Servicios de conectividad empresarial en SharePoint 2013](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Configure the Secure Store Service in SharePoint 2013](http://msdn.microsoft.com/library/29C0BC76-D835-401B-A2FB-ABB069E84125.aspx)
    
  

