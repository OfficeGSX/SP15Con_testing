---
title: Desarrollar de flujos de trabajo de SharePoint 2013 mediante Visual Studio
ms.prod: SHAREPOINT
ms.assetid: 28f5d3b1-6fe8-4b1f-8c4e-b11105fe6f46
---



# Desarrollar de flujos de trabajo de SharePoint 2013 mediante Visual Studio
SharePoint 2013 admite dos entornos de desarrollo de flujos de trabajo principales para la creación de flujos de trabajo: SharePoint Designer y Visual Studio. En este artículo se resumen ambos y se habla de las ventajas y desventajas de cada uno.
## Creación de aspectos básicos para flujos de trabajo de SharePoint
<a name="bkm_AuthoringBasics"> </a>


> **NOTA**
> Para obtener orientación para la instalación y configuración de Microsoft SharePoint Server 2013 y el servidor de Cliente del Administrador de flujos de trabajo 1.0, consulte  [Instalar y configurar el Administrador de flujos de trabajo de SharePoint 2013](set-up-and-configure-sharepoint-2013-workflow-manager.md). 
  
    
    

Al igual que en versiones anteriores, Microsoft SharePoint 2013 ofrece dos entornos de desarrollo de flujos de trabajo principales para la creación de flujos de trabajo: Microsoft SharePoint Designer y Microsoft Visual Studio. No obstante, lo que difiere respecto a versiones anteriores es que el empleo de Visual Studio ya no proporciona una estrategia de creación basada en código. Por el contrario, tanto SharePoint Designer como Visual Studio ofrecen un entorno de creación sin código completamente declarativo independientemente de la herramienta de desarrollo que seleccione.
  
    
    

> **NOTA**
> Como complemento de la creación de flujos de trabajo en SharePoint Designer, también puede usar Microsoft Visio 2013 para estructurar la lógica del flujo de trabajo mediante formas de Visio 2013 y luego importar la lógica a SharePoint Designer 2013. Para obtener información sobre el uso de Visio 2013 para crear la lógica del flujo de trabajo, consulte  [Desarrollo de flujos de trabajo en SharePoint Designer y Visio](workflow-development-in-sharepoint-designer-and-visio.md). 
  
    
    


### Flujos de trabajo declarativos

Primero vamos a aclarar lo que se quiere decir con flujos de trabajo "declarativos". Este término significa que en lugar de crearse en código y luego compilarse en ensamblados administrados, el flujo de trabajo se describe (literalmente) en XAML y luego se ejecuta de forma interpretativa en tiempo de ejecución.
  
    
    
El XAML se deriva (o infiere) de los bloques de creación del flujo de trabajo que se manipulan en la superficie de diseño de flujos de trabajo de Diseñador de flujo de trabajo (si se usa Visual Studio) o SharePoint Designer (o Visio, de lo que hablaremos más posteriormente). Los propios bloques de construcción son los objetos de diseño visuales del flujo de trabajo del cuadro de herramientas del diseñador: fases, condiciones, acciones, eventos, etc. El conjunto de herramientas de los respectivos cuadros de herramientas (Visual Studio o SharePoint Designer) difiere algo, pero el concepto de flujo de trabajo declarativo es el mismo.
  
    
    

## Árbol de decisión: SharePoint Designer frente a Visual Studio
<a name="bkm_DecisionTree"> </a>

Entre las principales ventajas del marco de flujo de trabajo en SharePoint 2013 está la facilidad con la que los trabajadores de la información pueden usar el entorno sin código de SharePoint Designer para crear ricos y eficaces flujos de trabajo. Además, en un entorno de creación declarativo como Visual Studio existe un alto grado de flexibilidad y personalización.
  
    
    
Estos dos entornos de creación de flujo de trabajo, SharePoint Designer y Visual Studio, ofrecen ventajas y desventajas concretas. En esta sección, veremos cómo determinar qué entorno de creación resulta más adecuado para sus necesidades de desarrollo de flujos de trabajo.
  
    
    

### Uso de SharePoint Designer


- **Usuarios de destino:** trabajadores de la información, analistas empresariales, desarrolladores de SharePoint.
    
  
- **Nivel de dificultad:** familiaridad con SharePoint Designer, incluidos los componentes principales de los flujos de trabajo, como fases, puertas, acciones, condiciones y bucles.
    
  
Con SharePoint Designer, los usuarios pueden crear un flujo de trabajo asociado a una lista, biblioteca o sitio con un diseñador sin código basado en texto. O bien pueden usar el nuevo entorno de desarrollo visual en el que los elementos gráficos se organizan en una superficie de diseño para representar el flujo lógico de un proceso empresarial. SharePoint Designer es excepcional a la hora de permitir un desarrollo de flujos de trabajo rápido por parte de personal no técnico.
  
    
    

### Uso de Visual Studio


- **Usuarios de destino:** desarrolladores de software de nivel intermedio o avanzado.
    
  
- **Nivel de dificultad:** familiaridad con Visual Studio, incluidos conceptos de desarrollo de software como receptores de eventos, empaquetado e implementación y seguridad.
    
  
La creación de flujos de trabajo en Visual Studio ofrece flexibilidad para crear flujos de trabajo que respalden prácticamente cualquier proceso empresarial, independientemente de su complejidad, y permite depurar y reutilizar definiciones de flujo de trabajo. Quizás lo más importante sea que Visual Studio permite a los desarrolladores incluir flujos de trabajo de SharePoint como parte de una solución o Complemento de SharePoint más amplia de SharePoint.
  
    
    
Visual Studio permite a los desarrolladores crear acciones personalizadas para su uso por parte de SharePoint Designer y proporciona los medios para ejecutar lógica personalizada. Con Visual Studio, los desarrolladores también pueden crear plantillas de flujo de trabajo que se pueden implementar en varios sitios.
  
    
    

## Comparación de SharePoint Designer y Visual Studio
<a name="bkm_Comparing"> </a>

En la siguiente tabla se ofrece una comparación en paralelo de las características y requisitos a la hora de usar SharePoint Designer y Visual Studio para crear flujos de trabajo de SharePoint.
  
    
    

**Tabla 1. Comparación de herramientas de creación de flujos de trabajo**


|**Característica o requisito**|**SharePoint Designer**|**Visual Studio**|
|:-----|:-----|:-----|
|Permite un desarrollo rápido de flujos de trabajo  <br/> |Sí  <br/> |Sí  <br/> |
|Permite reutilizar flujos de trabajo  <br/> |Un flujo de trabajo solo puede ser utilizado por la lista o biblioteca en la que fue desarrollado. No obstante, SharePoint Designer ofrece flujos de trabajo reutilizables que se pueden usar varias veces en el mismo sitio.  <br/> |Se puede escribir un flujo de trabajo como plantilla para que una vez implementado se pueda reutilizar y asociar a cualquier lista o biblioteca.  <br/> |
|Permite incluir un flujo de trabajo como parte de una solución o Complemento de SharePoint de SharePoint  <br/> |No  <br/> |Sí  <br/> |
|Permite crear acciones personalizadas  <br/> |No. No obstante, SharePoint Designer puede usar e implementar acciones personalizadas creadas e implementadas mediante Visual Studio.  <br/> |Sí. No obstante, tenga en cuenta que en Visual Studio se usan las actividades subyacentes y no sus acciones correspondientes.  <br/> |
|Permite escribir código personalizado  <br/> |No  <br/> |No  <br/> <br/> **NOTA**<br/> Esto ha cambiado desde versiones anteriores. En SharePoint 2013, los flujos de trabajo solo son declarativos y Visual Studio se basa en la superficie de diseño visual para el desarrollo de flujos de trabajo.           |
|Se puede usar Visio Professional para crear lógica de flujos de trabajo  <br/> |Sí  <br/> |No  <br/> |
|Implementación  <br/> |Se implementa automáticamente en la lista, biblioteca o sitio en el que se creó.  <br/> |Se crea un archivo de paquete de solución de SharePoint (.wsp) y se implementa el paquete de solución en el sitio (SPWeb).  <br/> |
|Publicación con un clic disponible para flujos de trabajo  <br/> |Sí  <br/> |Sí  <br/> |
|Los flujos de trabajo se pueden empaquetar e implementar en un servidor remoto  <br/> |Sí  <br/> |Sí  <br/> |
|Depuración  <br/> |No se puede depurar.  <br/> |El flujo de trabajo se puede depurar mediante Visual Studio.  <br/> |
|Solo se pueden usar acciones aprobadas por el administrador del sitio  <br/> |Sí  <br/> |Sí  <br/> <br/> **NOTA**<br/> Esto ha cambiado desde versiones anteriores. Anteriormente, los flujos de trabajo y las acciones creados mediante Visual Studio se basaban en código y se implementaban en el ámbito de un conjunto de servidores, así que no se necesitaba aprobación del administrador.           |
   

## Desarrollo de flujos de trabajo con Visual Studio
<a name="bkm_Developing"> </a>

A diferencia de versiones anteriores, los flujos de trabajo de SharePoint 2013 son totalmente declarativos. Ahora integrado en Windows Workflow Foundation 4, Visual Studio proporciona una superficie de diseño visual que permite crear flujos de trabajo personalizados, plantillas de flujo de trabajo, formularios y actividades de flujo de trabajo personalizadas en el entorno de diseño en su totalidad. Después el flujo de trabajo se empaqueta y se implementa como una característica de SharePoint. Para obtener información sobre el empaquetado de características, consulte  [Desarrollo de flujo de trabajo en Visual Studio](http://msdn.microsoft.com/es-es/library/ms461324.aspx).
  
    
    
Quizás el cambio más significativo para los desarrolladores de Visual Studio sea que los flujos de trabajo personalizados ya no se compilan e implementan como ensamblados de .NET Framework. Además, SharePoint 2013 ya no usa formularios de Microsoft InfoPath; en su lugar, la generación de formularios se basa en formularios de Microsoft ASP.NET. 
  
    
    
Por último, las plantillas de proyecto de flujo de trabajo de Visual Studio han cambiado. Mientras que antes se proporcionaban plantillas para flujos de trabajo de máquina de estado y secuenciales, estas distinciones ya no tienen sentido. Ahora hay plantillas de proyecto de Visual Studio disponibles en la compilación de Visual Studio proporcionada en la máquina virtual (VM).
  
    
    

## Habilitación de depuración local de flujos de trabajo
<a name="bkm_Enabling"> </a>

Para depurar flujos de trabajo locales en Visual Studio, tiene que permitir temporalmente que las Herramientas del Administrador de flujos de trabajo accedan al sistema a través del firewall.
  
    
    

1. En el **Panel de control**, elija **Sistema y seguridad**, **Firewall de Windows**.
    
  
2. En la lista **Ventana principal del Panel de control**, elija el vínculo **Configuración avanzada**.
    
  
3. En el panel izquierdo del Firewall de Windows, elija **Reglas de entrada**.
    
  
4. En la lista **Reglas de entrada**, elija **Workflow Manager Tools 1.0 for Visual Studio 2012 - Test Service Host**.
    
  
5. En la lista **Acciones**, elija **Habilitar regla**.
    
  
6. En la página de propiedades del proyecto de SharePoint, elija la pestaña **SharePoint** y active la casilla **Habilitar depuración de flujos de trabajo**.
    
  

## Depuración de flujos de trabajo de SharePoint Online con Visual Studio
<a name="bkm_Debug"> </a>

Para depurar flujos de trabajo de SharePoint Online en Visual Studio, realice los siguientes pasos:
  
    
    

1. Si se encuentra tras un firewall, es posible que tenga que instalar un cliente de proxy (como el  [Cliente de Forefront Threat Management Gateway (TMG)](http://www.microsoft.com/es-es/download/details.aspx?displaylang=en&amp;id=10504)), en función de la topología de su compañía.
    
  
2. Registre una cuenta de Microsoft Azure si no lo ha hecho ya y luego inicie sesión en ella.
    
    Para obtener información sobre cómo registrar una cuenta de Microsoft Azure, consulte  [Microsoft Azure](http://www.windowsazure.com).
    
  
3. Cree un nombre de espacios del bus de servicio de Microsoft Azure que pueda usar para depurar flujos de trabajo remotos. Puede hacerlo en el  [portal de administración de Microsoft Azure](http://manage.windowsazure.com).
    
    Para obtener más información sobre el bus de servicio de Microsoft Azure, consulte  [Administración de espacios de nombres del bus de servicio](http://msdn.microsoft.com/es-es/library/windowsazure/hh690928.aspx).
    
    > **NOTA**
      > La depuración de flujos de trabajo de SharePoint Online usa el componente del servicio de retransmisión del bus de servicio de Microsoft Azure, por lo que usar el bus de servicio tendrá unos costes adicionales. Consulte las  [Preguntas frecuentes sobre precios del bus de servicio](http://msdn.microsoft.com/library/hh667438.aspx). Tendrá acceso gratuito a Microsoft Azure cada mes que se suscriba a Visual Studio Professional con MSDN, Visual Studio Premium con MSDN o Visual Studio Ultimate con MSDN. Gracias a este acceso, podrá usar la retransmisión del bus de servicio durante 1.500 o 3.000 horas en función de la suscripción de MSDN que tenga. Consulte  [Obtenga servicios de Microsoft Azure cada mes sin cargo alguno](http://www.windowsazure.com/es-es/pricing/member-offers/msdn-benefits/). 
4. En Microsoft Azure, elija el espacio de nombres del servicio, el vínculo **Clave de acceso** y, después, copie el texto en el cuadro **Cadena de conexión**.
    
  
5. En la página de propiedades del proyecto de Complemento de SharePoint, elija la pestaña **SharePoint** y active la casilla **Habilitar depuración de flujos de trabajo**.
    
    Debe habilitar esta característica para depurar flujos de trabajo en SharePoint Online. Esta propiedad se aplica a todos los proyectos de SharePoint en Visual Studio. Visual Studio desactiva automáticamente la depuración de flujos de trabajo si empaqueta la aplicación para distribuirla en la tienda Office.
    
  
6. Active la casilla **Habilitar la depuración mediante el bus de servicio de Microsoft Azure**. A continuación, en el cuadro **Cadena de conexión del bus de servicio de Microsoft Azure**, pegue la cadena de conexión que copió.
    
  
Una vez que ha habilitado la depuración de flujos de trabajo y ha especificado una cadena de conexión válida para el bus de servicio de Microsoft Azure, podrá proceder a depurar flujos de trabajo de SharePoint Online.
  
    
    

> **NOTA**
> Si no ha deshabilitado la depuración de flujos de trabajo y no desea recibir una notificación siempre que su proyecto contenga un flujo de trabajo, desactive la casilla **Notificarme si la depuración del bus de servicio de Windows Azure no está configurada**. 
  
    
    


## Recursos adicionales
<a name="workflowbk_addres"> </a>

La mayor parte del proceso de desarrollo de flujos de trabajo de SharePoint permanece inalterada para el desarrollador de Visual Studio. Las secciones fundamentales de la documentación de SharePoint 2010 siguen siendo relevantes:
  
    
    

- Para obtener información sobre el empleo de Visual StudioDiseñador de flujo de trabajo, consulte  [Información general de Visual Studio Designer para Windows Workflow Foundation](http://msdn.microsoft.com/es-es/library/ms441543.aspx).
    
  
- Para obtener información sobre la creación de formularios mediante Microsoft ASP.NET, consulte  [Introducción a los formularios de flujo de trabajo](http://msdn.microsoft.com/es-es/library/ms457061.aspx).
    
  
- Para obtener información sobre el empaquetado de características, consulte  [Desarrollo de flujo de trabajo en Visual Studio](http://msdn.microsoft.com/es-es/library/ms461324.aspx).
    
  
