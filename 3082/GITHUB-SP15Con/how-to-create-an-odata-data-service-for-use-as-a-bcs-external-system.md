---
title: Procedimientos Crear un servicio de datos OData para su uso como un sistema externo de BCS
ms.prod: SHAREPOINT
ms.assetid: 7d7b3aa6-85b7-400d-8ea5-50bebac56a1d
---


# Procedimientos: Crear un servicio de datos OData para su uso como un sistema externo de BCS
Aprenda a crear un servicio WCF direccionable a Internet que usa OData para enviar notificaciones a SharePoint 2013 cuando los datos subyacentes cambian. Estas notificaciones se usan para desencadenar eventos asociados a listas externas.
En este artículo se describe cómo crear un servicio de datos ASP.NETWindows Communication Foundation (WCF) para exponer la base de datos de ejemplo AdventureWorks 2012 LT. Esto le permite tener acceso a datos a través del protocolo de datos abierto (OData). Cuando se establece el acceso mediante OData, puede configurar un tipo de contenido externo Servicios de conectividad empresarial (BCS) que permitirá a SharePoint 2013 usar los datos de la base de datos externa. Para mejorar este origen OData, puede agregar contratos de servicio al servicio WCF que permitirá a BCS suscribirse a notificaciones que indiquen que los datos externos han cambiado.
  
    
    


## Requisitos previos para la creación del servicio OData
<a name="bkmk_Prerequisites"> </a>

para crear el servicio OData de este artículo se necesita lo siguiente:
  
    
    

- Un servidor que hospede Internet Information Services (IIS)
    
  
- .NET Framework 3.5 o posterior
    
  
- SharePoint 2013
    
  
-  [Script de AdventureWorks 2012 LT](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
-  [Datos de AdventureWorks 2012 LT](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
- Visual Studio 2012
    
  
- Office Developer Tools para Visual Studio 2012
    
  
Para obtener información sobre la configuración del entorno de desarrollo, consulte  [Configurar un entorno de desarrollo general para SharePoint 2013](set-up-a-general-development-environment-for-sharepoint-2013.md).
  
    
    

### Conceptos básicos para crear un servicio OData

La tabla 1 enumera los artículos que le ayudarán a comprender los conceptos básicos para la creación de un servicio WCF con OData y contenido externo.
  
    
    

**Tabla 1. Conceptos básicos para crear un servicio OData**


|**Recurso**|**Descripción**|
|:-----|:-----|
| [Uso de orígenes OData con Servicios de conectividad empresarial en SharePoint 2013](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md) <br/> |Proporciona información para ayudarle a comenzar a crear tipos de contenido externo basados en orígenes OData y usar esos datos en componentes de SharePoint 2013 o Office 2013.  <br/> |
| [Cómo crear un tipo de contenido externo desde un origen de OData en SharePoint 2013](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md) <br/> |Aprenda a usar Visual Studio 2012 para descubrir un origen OData publicado y crear un tipo de contenido externo reutilizable para usarlo en BCS en SharePoint 2013.  <br/> |
| [Protocolo de datos abierto](http://www.odata.org) <br/> |Proporciona información sobre el protocolo de datos abierto, incluidas definiciones de protocolo, información sobre la arquitectura y ejemplos de uso.  <br/> |
| [Información general acerca de WCF Data Services](http://msdn.microsoft.com/es-es/library/cc668794.aspx) <br/> |WCFLos servicios de datos permiten la creación y el uso de servicios de datos para la web o una intranet mediante OData. OData permite exponer sus datos como recursos que son direccionables por URI.  <br/> |
| [Desarrollar e implementar WCF Data Services](http://msdn.microsoft.com/es-es/library/gg258442.aspx) <br/> |Proporciona información sobre el desarrollo y la implementación de los servicios de datos de WCF.  <br/> |
   

### Pasos necesarios para crear el sistema externo

Se deben completar los pasos siguientes 
  
    
    

- Instalar la base de datos de ejemplo de AdventureWorks 2012 LT
    
  
- Crear el servicio OData
    
  
- Establecer los permisos de acceso
    
  
- Probar el servicio
    
  
- Agregar características para la funcionalidad adicional de BCS
    
  

## Instalar la base de datos de ejemplo
<a name="bkmk_Prerequisites"> </a>

Un sistema externo suele ser una base de datos y, por esa razón, en este ejemplo se muestra cómo usar la base de datos de ejemplo AdventureWorks 2012 LT para representar un origen de datos de propietario.
  
    
    
Los pasos siguientes 
  
    
    

## Cómo crear el servicio OData de WCF
<a name="bkmk_CreatingTheService"> </a>

La creación de un servicio Windows Communication Foundation (WCF) al que se puede obtener acceso a través del protocolo OData es relativamente sencillo. Visual Studio 2012 proporciona varias herramientas para descubrir y modelar datos de diversos orígenes de datos automáticamente. Esto le permite crear conexiones e interfaces de datos en bases de datos SQL Server y otros tipos de almacenes de datos con los que se puede trabajar mediante programación utilizando un modelo de objeto extensivo.
  
    
    
Sin embargo, para que SharePoint 2013 habilite BCS para que reciba notificaciones de sistemas remotos cuando han cambiado los datos subyacentes, debe modificar el servicio WCF para responder a esos cambios.
  
    
    

### Para crear un nuevo proyecto de WCF


1. En Visual Studio, en el menú **Archivo**, elija **Nuevo**, **Proyecto**.
    
  
2. En el cuadro de diálogo **Nuevo proyecto**, elija la plantilla **Web** y, a continuación, **Aplicación web ASP.NET**.
    
  
3. Escriba **AdventureWorksService** como nombre del proyecto y haga clic en **Aceptar**.
    
  
4. En el **Explorador de soluciones**, abra el menú contextual del proyecto ASP.NET que acaba de crear y elija **Propiedades**.
    
  
5. Seleccione la pestaña **Web** y establezca el valor del cuadro de texto **Puerto específico** en8008.
    
  

### Para definir el modelo de datos


1. En el **Explorador de soluciones**, abra el menú contextual del proyecto ASP.NET y elija **Agregar nuevo elemento**.
    
  
2. En el cuadro de diálogo **Agregar nuevo elemento**, elija la plantilla Datos y, a continuación, **Modelo de datos de entidad ADO.NET**.
    
  
3. En el nombre del modelo de datos, escriba **AdventureWorks.edmx**.
    
  
4. En el **Asistente para el modelo de datos de entidad**, elija **Generar desde base de datos** y, a continuación, **Siguiente**.
    
  
5. Conecte el modelo de datos a la base de datos mediante uno de los pasos siguientes y, a continuación, elija **siguiente**.
    
  - Si no tiene una conexión de base de datos ya configurada, elija **Nueva conexión** y cree una conexión nueva.
    
  
  - Si tiene una conexión de base de datos ya configurada para conectarse a la base de datos Northwind, elija esa conexión en la lista de conexiones.
    
  
  - En la última página del asistente, active las casillas de todas las tablas de la base de datos y, a continuación, desactive las casillas de vistas y procedimientos almacenados.
    
  
6. Elija **Finalizar** para cerrar el asistente.
    
  

### Para crear el servicio de datos


1. En el **Explorador de soluciones**, abra el menú contextual del proyecto ASP.NET y, a continuación, elija **Agregar nuevo elemento**.
    
  
2. En el cuadro de diálogo **Agregar nuevo elemento**, elija **WCF Data Service**.
    
  
3. En el nombre del servicio, escriba **AdventureWorks**.
    
    Visual Studio crea los archivos de marcado y código XML para el nuevo servicio. De forma predeterminada, se abre la ventana del editor de código. En el **Explorador de soluciones**, el servicio tendrá el nombre **AdventureWorks**, con la extensión .svc.cs o .svc.vb.
    
  
4. Reemplace el comentario  `/* TODO: put your data source class name here */` en la definición de la clase que define el servicio de datos con el tipo que es el contenedor de la entidad del modelo de datos, que en este caso es **AdventureWorksEntities**. La definición de clase debe tener el siguiente aspecto:
    
  ```cs
  
public class AdventureWorks : DataService<AdventureWorksEntities>
  ```

De forma predeterminada, no se puede obtener acceso al crear el servicio WCF debido a su configuración de seguridad. El siguiente paso le permite especificar quién puede acceder al mismo y qué derechos tiene.
  
    
    

### Para permitir el acceso a recursos del servicio de datos


- En el código del servicio de datos, cambie el código del marcador de posición de la función **InitializeService** con el siguiente.
    
  ```cs
  
      config.SetEntitySetAccessRule("*", EntitySetRights.All);
      config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


    Esto permite que clientes autorizados tengan acceso de lectura y escritura a los recursos de los conjuntos de la entidad especificada.
    
    > **NOTA**
      <br/> Cualquier cliente que pueda acceder a la aplicación ASP.NET también puede obtener acceso a los recursos expuestos por el servicio de datos. En un servicio de datos de producción, para evitar el acceso no autorizado a los recursos, debería también asegurar la propia aplicación. Para más información, consulte  [Proteger WCF Data Services](http://msdn.microsoft.com/es-es/library/dd728284.aspx). 
Para que BCS reciba notificaciones, debe haber un mecanismo en el origen de datos back-end que acepte una solicitud para agregar y eliminar suscripciones de notificación.
  
    
    
El último paso en la creación del servicio es agregar operaciones de servicio a los estereotipos **Subscribe** y **Unsubscribe** que se definen en el modelo BDC.
  
    
    

### Para agregar operaciones de servicio a los estereotipos Suscribir y Cancelar suscripción


- En la página AdventureWorks.cs, agregue la siguiente declaración de variables de cadena.
    
  ```cs
  
  public string subscriptionStorePath = @"\\\\[SHARE_NAME]\\SubscriptionStore\\SubscriptionStore.xml";
  ```


    > **NOTA**
      <br/> Este archivo es un archivo XML que se actualiza con las nuevas suscripciones. El acceso a este archivo se hará por el proceso del servidor, así que asegúrese de que ha concedido los derechos necesarios para el acceso a este archivo. <br/> También puede crear una solución de base de datos para almacenar la información de suscripción. 

    A continuación, agregue los dos métodos siguientes **WebGet** para controlar las suscripciones.
    


  ```cs
  [WebGet]
        public string Subscribe(string deliveryUrl, string eventType)
        {
            string subscriptionId = Guid.NewGuid().ToString();
            
            XmlDocument subscriptionStore = new XmlDocument();
            
            subscriptionStore.Load(subscriptionStorePath);

            // Add a new subscription element.
            XmlNode newSubNode = subscriptionStore.CreateElement("Subscription");

            // Add subscription ID element to the subscription element.
            XmlNode subscriptionIdStart = subscriptionStore.CreateElement("SubscriptionID");
            subscriptionIdStart.InnerText = subscriptionId;
            newSubNode.AppendChild(subscriptionIdStart);

            // Add delivery URL element to the subscription element.
            XmlNode deliveryAddressStart = subscriptionStore.CreateElement("DeliveryAddress");
            deliveryAddressStart.InnerText = deliveryUrl;
            newSubNode.AppendChild(deliveryAddressStart);

            // Add event type element to the subscription element.
            XmlNode eventTypeStart = subscriptionStore.CreateElement("EventType");
            eventTypeStart.InnerText = eventType;
            newSubNode.AppendChild(eventTypeStart);

            // Add the subscription element to the root element. 
            subscriptionStore.AppendChild(newSubNode);

            
            subscriptionStore.Save(subscriptionStorePath);

            return subscriptionId;
        }

        [WebGet]
        public void Unsubscribe(string subscriptionId)
        {
            XmlDocument subscriptionStore = new XmlDocument();
            subscriptionStore.Load(subscriptionStorePath);

            XmlNodeList subscriptions = subscriptionStore.DocumentElement.ChildNodes;
            foreach (XmlNode subscription in subscriptions)
            {
                XmlNodeList subscriptionList = subscription.ChildNodes;
                if (subscriptionList.Item(0).InnerText == subscriptionId)
                {
                    subscriptionStore.DocumentElement.RemoveChild(subscription);
                    break;
                }
            }

            subscriptionStore.Save(subscriptionStorePath);
        }

  ```


## Pasos siguientes
<a name="bkmk_Next"> </a>

Para notificar a SharePoint que se han realizado cambios, también necesita crear un servicio que consulte los cambios en el origen de datos y luego envíe notificaciones a todos los suscritos a notificaciones.
  
    
    

## Recursos adicionales
<a name="bkmk_Addresources"> </a>


-  [Servicios de conectividad empresarial de SharePoint 2013](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Uso de orígenes OData con Servicios de conectividad empresarial en SharePoint 2013](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Tipos de contenido externo en SharePoint 2013](external-content-types-in-sharepoint-2013.md)
    
  
-  [Alertas y eventos externos en SharePoint 2013](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [Cómo crear un tipo de contenido externo desde un origen de OData en SharePoint 2013](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md)
    
  
-  [Referencia a los programadores de Servicios de conectividad empresarial para SharePoint 2013](business-connectivity-services-programmers-reference-for-sharepoint-2013.md)
    
  

