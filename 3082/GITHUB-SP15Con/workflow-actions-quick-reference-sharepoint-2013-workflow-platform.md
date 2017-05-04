---
title: Referencia rápida sobre acciones de flujo de trabajo (plataforma de flujo de trabajo de SharePoint 2013)
ms.prod: SHAREPOINTDESIGNER
ms.assetid: eb3434e5-bc75-4474-8873-4980062fd29c
---



# Referencia rápida sobre acciones de flujo de trabajo (plataforma de flujo de trabajo de SharePoint 2013)
En esta referencia se enumeran las acciones de flujo de trabajo compatibles con la compilación actual de SharePoint Designer 2013, además de las que no están disponibles.
## Acciones de flujo de trabajo de SharePoint Designer 2013
<a name="bkm_WorkflowActions"> </a>

A continuación se refieren las acciones de flujo de trabajo disponibles para la plataforma de flujo de trabajo de SharePoint 2013. Además de la plataforma de flujo de trabajo de SharePoint 2013, SharePoint Designer 2013 también admite la plataforma de flujo de trabajo de SharePoint 2010. Para ver acciones de flujo de trabajo de la plataforma 2010, consulte  [Referencia rápida sobre acciones de flujo de trabajo (plataforma de flujo de trabajo de SharePoint 2010)](workflow-actions-quick-reference-sharepoint-2010-workflow-platform.md)
  
    
    

### Acciones principales
<a name="bkm_CoreActions"> </a>

Las acciones principales son aquellas que se realizan con más frecuencia; están agrupadas para facilitar el acceso.
  
    
    

**Tabla 1. Referencia de acciones principales**


|**Acción**|**Descripción**|
|:-----|:-----|
|Agregar un comentario  <br/> |Permite dejar comentarios informativos en el diseñador de flujos de trabajo para referencia. Esto resulta de especial utilidad cuando hay otros usuarios colaborando en el flujo de trabajo.  <br/> |
|Agregar hora a la fecha  <br/> |Agrega una hora concreta en minutos, horas, días o meses a una fecha (el año no es compatible) y almacena el valor de resultado como una variable. La fecha puede ser la actual, una concreta o una búsqueda. El valor "Fecha actual" devuelve medianoche UTC.  <br/> |
|Crear diccionario  <br/> |Crea una variable de diccionario de pares de clave y valor.  <br/> <br/> **NOTA**<br/> El diccionario usa notación JSON para almacenar datos.           Para obtener más información sobre la variable de diccionario, consulte  [Información sobre acciones del diccionario en SharePoint Designer 2013](understanding-dictionary-actions-in-sharepoint-designer-2013.md) <br/> |
|Llamar al servicio web HTTP  <br/> |Funciona como una llamada de método a un servicio web HTTP y devuelve datos con el formato JSON. Se admite autenticación básica a través de RequestHeader.  <br/> Para obtener más información sobre la variable de diccionario, consulte  [Información sobre acciones del diccionario en SharePoint Designer 2013](understanding-dictionary-actions-in-sharepoint-designer-2013.md) <br/> |
|Contar elementos en un diccionario  <br/> |Devuelve un recuento del número de elementos de un diccionario determinado.  <br/> |
|Realizar el cálculo  <br/> |Realiza un cálculo aritmético y almacena el valor de resultado en una variable.  <br/> <br/> **NOTA**<br/> En SharePoint 2013, esta acción solo admite el tipo numérico **Double**. No se admiten enteros. No se admite el empleo del operador "+" (concatenación) para cadenas.           |
|Obtener un elemento de un diccionario  <br/> |Devuelve un elemento concreto de una variable de diccionario.  <br/> |
|Registrar en lista de historial  <br/> |Escribe un mensaje de una lista de elementos de mensaje predefinidos en la lista de historial del flujo de trabajo.  <br/> |
|Detener durante  <br/> |Hace que un flujo de trabajo deje de ejecutarse durante un intervalo temporal concreto en días, horas y minutos.  <br/> |
|Detener hasta fecha  <br/> |Hace que un flujo de trabajo deje de ejecutarse hasta una fecha y una hora especificadas.  <br/> |
|Enviar correo electrónico  <br/> |Envía automáticamente un mensaje de correo electrónico que contiene un mensaje predeterminado a un usuario o grupo cuando se produce un evento de flujo de trabajo determinado.  <br/> <br/> **IMPORTANTE**<br/> Si el sitio no está agregado a la lista Sitios de confianza, los mensajes se enrutan a la carpeta Correo no deseado de Outlook.           |
|Configurar parte de la hora del campo Fecha y hora  <br/> |Crea una marca de tiempo y almacena el valor de resultado en una variable. Puede establecer la hora en horas y minutos y agregar una fecha actual, una concreta o una búsqueda.  <br/> |
|Establecer estado del flujo de trabajo  <br/> |Establece el estado del flujo de trabajo.  <br/> |
|Establecer variable de flujo de trabajo  <br/> |Establece una variable de flujo de trabajo en un valor. También puede usar esta acción si desea que el flujo de trabajo asigne datos a una variable.  <br/> |
|Ir a fase  <br/> |Especifica la siguiente fase a la que debería entregarse el control de flujo.  <br/> |
   

### Acciones de coordinación
<a name="bkm_CoreActions"> </a>

Las acciones de coordinación se usan para invocar un flujo de trabajo basado en la plataforma de flujo de trabajo de SharePoint 2010. Para obtener más información sobre la acciones de coordinación, consulte  [Información sobre acciones de coordinación en SharePoint Designer 2013](understanding-coordination-actions-in-sharepoint-designer-2013.md)
  
    
    

**Tabla 2. Referencia de acciones de coordinación**


|**Acción**|**Descripción**|
|:-----|:-----|
|Iniciar un flujo de trabajo de lista  <br/> |Inicia un flujo de trabajo de lista basado en la plataforma de flujo de trabajo de SharePoint 2010.  <br/> <br/> **NOTA**<br/>  Esta acción tiene los siguientes problemas:<br/>  No se puede usar el campo de tipo "Asignaciones" como un parámetro si el flujo de trabajo de 2010 tiene una acción TaskProcess.<br/>  Si se realizan varias llamadas al mismo flujo de trabajo de 2010, el resultado será varios orígenes de datos en la funcionalidad de búsqueda de flujos de trabajo de 2013. Dichos orígenes de datos serán todos iguales.<br/>  Los nombres de variable de 2013 no pueden contener caracteres especiales como '?' y '#'. Si un flujo de trabajo de 2010 contiene caracteres especiales, se convertirán en código hexadecimal en el flujo de trabajo de 2013.          |
|Iniciar un flujo de trabajo de sitio  <br/> |Inicia un flujo de trabajo de sitio basado en la plataforma de flujo de trabajo de SharePoint 2010.  <br/> <br/> **NOTA**<br/>  Esta acción tiene los siguientes problemas:<br/>  No se puede usar el campo de tipo "Asignaciones" como un parámetro si el flujo de trabajo de 2010 tiene una acción TaskProcess.<br/>  Si se realizan varias llamadas al mismo flujo de trabajo de 2010, el resultado será varios orígenes de datos en la funcionalidad de búsqueda de flujos de trabajo de 2013. Dichos orígenes de datos serán todos iguales.<br/>  Los nombres de variable de 2013 no pueden contener caracteres especiales como '?' y '#'. Si un flujo de trabajo de 2010 contiene caracteres especiales, se convertirán en código hexadecimal en el flujo de trabajo de 2013.          |
   

### Acciones de lista
<a name="bkm_ListActions"> </a>

El grupo de acciones de lista agrupa acciones que se utilizan para manipular listas y elementos de lista.
  
    
    

**Tabla 3. Referencia de acciones de lista**


|**Acción**|**Descripción**|
|:-----|:-----|
|Proteger elemento  <br/> |Protege un elemento que está desprotegido. Solo se pueden proteger los elementos de una biblioteca de documentos.  <br/> > **PRECAUCIóN**> Si intenta proteger un elemento que no está desprotegido, el flujo de trabajo se bloquea.           |
|Desproteger elemento  <br/> |Desprotege un elemento. El flujo de trabajo comprueba que el elemento esté protegido antes de desproteger un documento. Solo es posible desproteger elementos de una biblioteca del sitio.  <br/> <br/> **PRECAUCIóN**<br/> Si intenta desproteger un elemento que no está protegido, el flujo de trabajo se bloquea.           |
|Copiar documento  <br/> |Copia un documento de la lista actual en una lista distinta de la biblioteca de documentos.  <br/> |
|Crear elemento de lista  <br/> |Crea un nuevo elemento de lista en la lista que especifique. Puede proporcionar los campos y valores del nuevo elemento. Puede usar esta acción siempre que quiera crear un nuevo elemento con información concreta.  <br/> |
|Eliminar elemento  <br/> |Elimina un elemento.  <br/> <br/> **NOTA**<br/> Esta acción finaliza en el equipo que ejecuta el motor de flujo de trabajo del Administrador de flujos de trabajo y produce una excepción **System.InvalidOperationException**. No hay ninguna solución.           |
|Descartar elemento desprotegido  <br/> |Descarta los cambios y vuelve a proteger el elemento si está desprotegido y se han realizado cambios en él.  <br/> <br/> **PRECAUCIóN**<br/> Si intenta proteger un elemento que no está desprotegido, el flujo de trabajo se bloquea.           |
|Establecer campo en elemento actual  <br/> |Establece un campo especificado del elemento actual en un valor concreto.  <br/> <br/> **NOTA**<br/> Si necesita que el flujo de trabajo se detenga hasta que el valor del campo haya cambiado, use la acción **Esperar al evento del elemento de lista** en lugar de esta.          |
|Traducir documento  <br/> |Traduce un documento a un idioma concreto.  <br/> <br/> **NOTA**<br/> Exige una aplicación de servicio de traducción automática preconfigurada.           |
|Actualizar elemento de lista  <br/> |Actualiza un elemento de lista. Puede especificar los campos y los nuevos valores de esos campos.  <br/> |
|Esperar al evento del elemento de lista  <br/> |[Versión mejorada de acción de Office 2010]. Detiene la instancia actual del flujo de trabajo para esperar a un evento de elemento de lista concreto. Esta acción escucha a dos eventos: **ItemUpdated** y **ItemAdded**.  <br/> |
|Esperar cambio de campo en elemento actual  <br/> |Espera a que un campo del elemento actual sea igual a un valor determinado.  <br/> |
   

### Acciones de Project
<a name="bkm_ProjectActions"> </a>

Las acciones de Project admiten la integración de Microsoft Project. Se usan para crear flujos de trabajo basados en Project. Todas las acciones de Project son nuevas en SharePoint Designer 2013.
  
    
    

**Tabla 4. Referencia de acciones de Project**


|**Acción**|**Descripción**|
|:-----|:-----|
|Crear proyecto a partir del elemento actual  <br/> |Toma el elemento actual y crea un nuevo proyecto en el sitio de PWA del conjunto de servidores de SharePoint.  <br/> |
|Establecer campo de Project  <br/> |Establece un valor para un campo concreto en Project Server.  <br/> <br/> **NOTA**<br/> Esta acción exige que el proyecto esté protegido. Si no lo está, el flujo de trabajo finalizará y los usuarios no podrán abrir ese proyecto en Project Web App.           |
|Establecer estado de fase de Project  <br/> |Establece el estado de la fase de Project.  <br/> <br/> **NOTA**<br/> Si un proyecto actual está desprotegido, se produce una excepción.           |
|Establecer campo de estado en lista de ideas  <br/> |Actualiza el estado en el elemento de lista original asociado al proyecto actual.  <br/> |
|Esperar a evento de Project  <br/> |Espera a un evento concreto de Project.  <br/> |
   

### Acciones de tarea
<a name="bkm_TaskActions"> </a>

Las acciones de tarea ofrecen la posibilidad de invocar un flujo de trabajo basado en la plataforma de flujo de trabajo de SharePoint 2010 desde un flujo de trabajo basado en la plataforma de flujo de trabajo de SharePoint 2013.
  
    
    

**Tabla 5. Referencia de acciones de tarea**


|**Acción**|**Descripción**|
|:-----|:-----|
|Asignar una tarea  <br/> |Asigna una tarea de flujo de trabajo a un usuario y establece una fecha de vencimiento para la finalización de la misma.  <br/> |
|Iniciar un proceso de tarea  <br/> |Crea tareas en varios usuarios y permite que se lleven a través de un proceso personalizado.  <br/> |
   

### Acciones de utilidad
<a name="bkm_UtilityActions"> </a>

Las acciones de utilidad son acciones que manipulan cadenas o encuentran el intervalo entre fechas.
  
    
    

**Tabla 6. Referencia de acciones de utilidad**


|**Acción**|**Descripción**|
|:-----|:-----|
|Extraer subcadena de final de cadena  <br/> |Copia un número especificado de caracteres a partir del final de una cadena y almacena el resultado en una variable.  <br/> |
|Extraer subcadena de índice de cadena  <br/> |Copia una subcadena a partir de un índice especificado en la cadena y coloca el valor en una variable.  <br/> <br/> **NOTA**<br/> Tenga en cuenta que aunque el valor de índice de Microsoft SharePoint Designer 2013 se basa en cero, los valores de SharePoint Designer 2010 se indizaron a partir de 1.           |
|Extraer subcadena de principio de cadena  <br/> |Copia un número especificado de caracteres a partir del principio de una cadena y almacena el resultado en una variable.  <br/> |
|Extraer subcadena de cadena de índice con longitud  <br/> |Copia una subcadena compuesta por un número especificado de caracteres a partir de un índice especificado en la cadena y coloca el valor en una variable.  <br/> <br/> **NOTA**<br/> Tenga en cuenta que aunque el valor de índice de Microsoft SharePoint Designer 2013 se basa en cero, los valores de SharePoint Designer 2010 se indizaron a partir de 1.           |
|Encontrar intervalo entre fechas  <br/> |Calcula el intervalo temporal en minutos, horas o días entre dos fechas y almacena el resultado en una variable.  <br/> |
|Recortar cadena  <br/> |Quita los espacios en blanco del principio y el final de una cadena.  <br/> |
|Encontrar subcadena en cadena  <br/> |Encuentra una subcadena concreta dentro de una cadena y devuelve el índice de la posición inicial de la subcadena.  <br/> |
|Sustituir subcadena en cadena  <br/> |Sustituye una subcadena concreta por otra.  <br/> |
|Recortar cadena  <br/> |Quita los espacios en blanco del principio y el final de una cadena.  <br/> |
   

## Acciones de flujo de trabajo obsoletas en SharePoint 2013
<a name="bkm_NotAvailable"> </a>

Para obtener una lista de acciones de SharePoint 2010 que están obsoletas y no aparecerán en SharePoint 2013, consulte  [Acciones del flujo de trabajo disponibles con el puente de interoperatividad del flujo de trabajo](workflow-actions-available-using-the-workflow-interop-bridge.md).
  
    
    

## Recursos adicionales
<a name="bkm_addlresource"> </a>


-  [Referencia a las acciones y actividades del flujo de trabajo para SharePoint 2013](workflow-actions-and-activities-reference-for-sharepoint-2013.md)
    
  
-  [Aspectos básicos de los flujos de trabajo de SharePoint 2013](sharepoint-2013-workflow-fundamentals.md)
    
  
-  [Introducción a los flujos de trabajo de SharePoint 2013](get-started-with-workflows-in-sharepoint-2013.md)
    
  
-  [Desarrollo de flujos de trabajo en SharePoint Designer y Visio](workflow-development-in-sharepoint-designer-and-visio.md)
    
  
