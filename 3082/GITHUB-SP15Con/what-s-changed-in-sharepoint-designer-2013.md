---
title: Novedades de SharePoint Designer 2013
ms.prod: SHAREPOINT
ms.assetid: f1cd30c9-9a73-428d-9151-f1813b43b020
---


# Novedades de SharePoint Designer 2013
Obtenga información sobre las características que se han quitado o están en desuso en SharePoint Designer 2013. Las características que están en desuso se incluyen en la versión de SharePoint 2013 para ofrecer compatibilidad con versiones anteriores del producto, pero se quitarán en versiones futuras.
## Características descontinuadas en SharePoint Designer 2013
<a name="WhatsChangedSharePointDesigner2013_DiscontinuedFeatures"> </a>

Las siguientes características se han quitado o están en desuso en SharePoint Designer 2013.
  
    
    

### Plataforma de flujo de trabajo de SharePoint 2010
<a name="WhatsChangedSharePointDesigner2013_WorkflowPlatform"> </a>

 **Descripción del cambio.** Algunas características de la plataforma de flujo de trabajo de SharePoint 2010 que dependen de Windows Workflow Foundation 3.0 están en desuso en SharePoint 2013.
  
    
    
 **Motivo del cambio.** SharePoint 2013 presenta una nueva plataforma de flujo de trabajo de SharePoint 2013 que se basa en Windows Workflow Foundation 4.0 y se integra con Administrador de flujos de trabajo 1.0.
  
    
    
 **Ruta de migración.** En SharePoint Designer 2013, aún puede crear un flujo de trabajo de SharePoint 2010 y usar todas las características del flujo de trabajo de SharePoint 2010 eligiendo la plataforma de flujo de trabajo de SharePoint 2010.
  
    
    
También puede integrar características de la plataforma de flujo de trabajo de SharePoint 2010 en la nueva plataforma de flujo de trabajo de SharePoint 2013. Para ello, elija la plataforma de flujo de trabajo de SharePoint 2010 y cree un flujo de trabajo de SharePoint 2010; después, cree un flujo de trabajo de SharePoint 2013 en la plataforma de flujo de trabajo de SharePoint 2013; y, a continuación, use las acciones **Iniciar un flujo de trabajo de lista** e **Iniciar un flujo de trabajo de sitio** en el flujo de trabajo de SharePoint 2013 para llamar al flujo de trabajo de SharePoint 2010.
  
    
    
Las siguientes características están disponibles solo en la plataforma de flujo de trabajo de SharePoint 2010:
  
    
    

- Acciones:
    
  - Detener flujo de trabajo
    
  
  - Capturar una versión del conjunto de documentos
    
  
  - Enviar conjunto de documentos al repositorio
    
  
  - Estado de aprobación del contenido del conjunto para el conjunto de documentos
    
  
  - Iniciar proceso de aprobación de conjunto de documentos
    
  
  - Declarar como registro
    
  
  - Establecer estado de aprobación de contenido
    
  
  - Revocar declaración como registro
    
  
  - Agregar elemento de lista 
    
  
  - Heredar permisos principales de elemento de lista
    
  
  - Quitar permisos de elemento de lista
    
  
  - Reemplazar permisos de elemento de lista
    
  
  - Buscar administrador de un usuario
    
  
  - Asignar un formulario a un grupo
    
  
  - Asignar un elemento de tarea
    
  
  - Recopilar datos de un usuario
    
  
  - Iniciar proceso de aprobación
    
  
  - Iniciar proceso de tarea personalizado
    
  
  - Iniciar proceso de comentarios
    
  
  - Copiar elemento de lista (SharePoint Designer 2013 solo admite la acción de copia de documentos)
    
  
- Condiciones:
    
  - Si el campo de elemento actual es igual al valor
    
  
  - Comprobar niveles de permisos de elemento de lista
    
  
  - Comprobar permisos de elemento de lista
    
  
- Pasos:
    
  - Paso de suplantación:
    
  
- Orígenes de datos:
    
  - Búsqueda de perfil de usuario
    
  
- Otras características:
    
  - Integración con Visio
    
  
  - Columna de asociación
    
  
  - Asociación de tipo de contenido para flujo de trabajo reutilizable
    
  
  - Característica "Requerir permisos de administración de listas/web" para el flujo de tareas de lista/sitios
    
  
  - Tipo de flujo de trabajo reutilizable globalmente
    
  
  - Opción de visualización del flujo de trabajo
    
  

### Características de diseño de página de SharePoint
<a name="WhatsChangedSharePointDesigner2013_PageDesignFeatures"> </a>

La siguiente característica no está disponible en SharePoint 2013.
  
    
    

#### Vista de diseño y vista en dos paneles
<a name="WhatsChangedSharePointDesigner2013_DesignViewSplitView"> </a>

 **Descripción del cambio.** SharePoint Designer 2010 tiene tres vistas para editar las páginas HTML y ASPX: la vista de código, la vista de diseño y la vista en dos paneles. La vista de diseño y la vista en dos paneles se han quitado de SharePoint Designer 2013. La eliminación de la vista de diseño y la vista en dos paneles afecta a las características de SharePoint Designer 2013 que se utilizan para editar elementos web y páginas maestras. Si modifica las páginas en SharePoint Designer 2013, debe utilizar la vista de código.
  
    
    
 **Motivo del cambio.** En comparación con las versiones actuales de Internet Explorer, la vista de diseño es una tecnología más antigua que no es compatible con muchas de las nuevas etiquetas de HTML5 y CSS.
  
    
    
 **Ruta de migración.** Si edita páginas en la vista de código, puede presionar **F12** para obtener una vista previa de la página en el explorador. Como alternativa, puede usar Visual Studio para editar las páginas.
  
    
    
Si desea diseñar o personalizar visualmente su sitio y desea una experiencia de edición de páginas WYSIWYG ("lo ve es lo que se obtiene"), puede usar cualquier editor de HTML profesional, como Microsoft Expression Web. A continuación, puede importar los archivos HTML a SharePoint Server 2013 usando el nuevo Administrador de diseño, que es una característica incluida en los sitios de publicación, como la plantilla de sitio de la colección de sitios del portal de publicación.
  
    
    

## Recursos adicionales
<a name="WhatsChangedSharePointDesigner2013_AdditionalResources"> </a>


-  [Referencia rápida sobre acciones de flujo de trabajo (plataforma de flujo de trabajo de SharePoint 2013)](workflow-actions-quick-reference-sharepoint-2013-workflow-platform.md)
    
  
-  [Cambios entre SharePoint 2010 y SharePoint 2013](http://technet.microsoft.com/es-es/library/ff607742%28v=office.15%29.aspx)
    
  
-  [Novedades del flujo de trabajo en SharePoint Server 2013](http://technet.microsoft.com/es-es/library/jj219638%28v=office.15%29.aspx)
    
  

  
    
    

