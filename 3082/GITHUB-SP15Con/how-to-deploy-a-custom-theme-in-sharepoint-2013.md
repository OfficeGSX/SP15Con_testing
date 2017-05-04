---
title: Procedimiento para implementar un tema personalizado en SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# Procedimiento para implementar un tema personalizado en SharePoint 2013
Obtenga información para la implementación de un tema personalizado en un sitio de SharePoint 2013 mediante la interfaz de usuario o la implementación de un receptor de características.
SharePoint 2013 incluye temas preinstalados. Es posible crear temas personalizados mediante la creación de paletas de colores, combinaciones de fuentes y páginas maestras adicionales. Una vez que los archivos se han cargado en la Galería de temas y en la Galería de páginas maestras, puede implementar un tema en un sitio mediante la interfaz de usuario o mediante código. Para obtener más información sobre las paletas de colores y las combinaciones de fuentes, consulte  [Paletas de colores y fuentes de SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md).
  
    
    


## Conceptos básicos para implementar un tema
<a name="core"> </a>

En la tabla 1 se enumeran los artículos que pueden ayudarle a comprender los conceptos básicos de la implementación de temas.
  
    
    

**Tabla 1. Conceptos básicos para implementar un tema**


|**Título del artículo**|**Descripción**|
|:-----|:-----|
| [Introducción a los temas para SharePoint 2013](themes-overview-for-sharepoint-2013.md) <br/> |Obtenga información sobre la experiencia de creación de temas en SharePoint 2013.  <br/> |
| [Eventos Feature](http://msdn.microsoft.com/es-es/library/ms469501.aspx) <br/> |Obtenga información sobre los eventos Feature, que permiten capturar un evento que se produce al instalar una característica en el conjunto de servidores y responder a él.  <br/> |
   

## Cargar archivos en la Galería de temas y en la Galería de páginas maestras
<a name="section1"> </a>

Puede crear temas personalizados mediante la creación de paletas de colores y combinaciones de fuentes adicionales y la carga de estas en la Galería de temas. Así, las nuevas paletas de colores y las combinaciones de fuentes estarán a su disposición cuando modifique un diseño en la experiencia de creación de temas o cuando aplique un tema mediante programación. De igual modo, si desea contar con diseños de sitio adicionales entre los que elegir, puede cargar páginas maestras adicionales, y archivos de vista previa correspondientes, a la Galería de páginas maestras. En la siguiente lista se describe dónde colocar los archivos:
  
    
    

- **Galería de páginas maestras** Enumera los archivos de páginas maestras y sus archivos de vista previa correspondientes (archivos .preview). Un archivo de vista previa de página maestra es necesario si se desea que la página maestra esté disponible en el asistente **Cambiar la apariencia**. Los archivos de JavaScript y otros activos de diseño también se pueden cargar en la Galería de páginas maestras.
    
    Para obtener acceso a la Galería de páginas maestras desde la interfaz de usuario de SharePoint, en la página **Configuración del sitio**, en **Galerías del diseñador web**, elija **Páginas maestras**. También puede ir directamente al sitio (http://  _NombreDelSitio_/_catalogs/masterpage/).
    
  
- **Galería de temas** Enumera las paletas de colores y las combinaciones de fuentes disponibles para la experiencia de creación de temas. SharePoint mira en la carpeta **15** para determinar las paletas de colores y las combinaciones de fuentes disponibles.
    
    Para obtener acceso a la Galería de temas desde la interfaz de usuario de SharePoint, en la página **Configuración del sitio**, en **Galerías del diseñador web**, elija **Temas**. También puede ir directamente al sitio (http:// _NombreDeLaColecciónDeSitios_/_catalogs/theme/15/).
    
  
- **Biblioteca de estilos** Enumera los archivos CSS personalizados que desea usar en la experiencia de creación de temas. Puede ir directamente a la Biblioteca de estilos (sustituya _NombreDeLaColecciónDeSitios_ e _idioma_ en esta dirección URL: http:// _NombreDeLaColecciónDeSitios_/Style Library/ _idioma_/Themable/).
    
> **NOTA**
> Coloque los archivos CSS personalizados en la carpeta Themable de la Biblioteca de estilos, no en la carpeta Themable de la Galería de páginas maestras. Únicamente los archivos CSS almacenados en la carpeta Themable de la Biblioteca de estilos son reconocidos por el motor de creación de temas. 

> **NOTA**
> Si tiene habilitado el control de versiones en la Galería de páginas maestras y la Galería de temas, también tiene que publicar los archivos de diseño para que el motor de creación de temas los pueda usar. 
  
    
    


## Implementar un tema mediante la interfaz de usuario
<a name="section2"> </a>

Un aspecto o diseño compuesto es la paleta de colores, combinación de fuentes, imagen de fondo y página maestra que determinan el aspecto de un sitio. La lista Diseños compuestos contiene los diseños compuestos disponibles en la galería de diseño. Puede crear un diseño si agrega un elemento de lista de la lista Diseños compuestos y especifica la página maestra, la paleta de colores, la combinación de fuentes y la imagen de fondo que se va a usar.
  
    
    

> **NOTA**
> Si quiere que la página maestra esté disponible en la galería de diseño, necesitará un archivo de vista previa de página maestra. 
  
    
    


### Para agregar un diseño compuesto


1. Elija el icono **Configuración** y luego **Configuración del sitio**.
    
  
2. En **Galerías del diseñador web**, elija **Diseños compuestos**.
    
  
3. En la lista **Diseños compuestos**, seleccione **nuevo elemento**.
    
  
4. En el cuadro de texto **Título**, escriba un título para el diseño.
    
  
5. En el cuadro de texto **Nombre**, escriba un nombre para el diseño. El nombre aparece en la lista Diseños compuestos y en la galería de diseño.
    
  
6. En el cuadro de texto **URL de página maestra**, escriba la dirección URL de la página maestra. Puede ser una dirección URL relativa.
    
  
7. En el cuadro de texto **URL de tema**, escriba la dirección URL de la paleta de colores (la dirección URL del archivo .spcolor). La dirección URL puede ser relativa.
    
  
8. En el cuadro de texto **Dirección URL de la imagen**, escriba la dirección URL de la imagen de fondo. Es opcional. La dirección URL puede ser relativa.
    
  
9. En el cuadro de texto **URL de combinación de fuentes**, escriba la dirección URL de la combinación de fuentes (la dirección URL del archivo .spfont). Es opcional. La dirección URL puede ser relativa.
    
  
10. En el cuadro de texto **Mostrar orden**, escriba el número de orden de visualización. Este determina el lugar en que aparece el diseño en la galería de diseño.
    
  
11. Elija **Guardar**.
    
    > **NOTA**
      > Si hubiera algún problema con los valores del diseño compuesto, este no se agregaría a la galería de diseño y no se registraría ningún mensaje en los archivos de registro. A continuación se enumeran las posibles razones por las que un diseño compuesto podría no agregarse: no se puede encontrar un archivo, hay un problema de formato con uno de los archivos o SharePoint no puede obtener acceso a los archivos. 
Ahora puede usar la galería de diseño para aplicar el nuevo diseño al sitio. Para obtener más información, consulte  [Elegir un tema para el sitio de su publicación](http://office.microsoft.com/es-es/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx) en Office.com.
  
    
    

## Implementar un tema mediante código
<a name="section3"> </a>

Puede implementar un tema mediante la implementación de un receptor de características.
  
    
    

### Para implementar un tema mediante un receptor de características


1. Cree una clase de receptor de características que herede de la clase  [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) .
    
  
2. En el método  [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) , cree un objeto [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) que use la paleta de colores y la combinación de fuentes y luego aplique el tema al sitio.
    
    En el siguiente ejemplo de código se muestra cómo implementar una paleta de colores y una combinación de fuentes personalizadas a un sitio.
    


  ```cs
  
// Get the SPColor file. Replace with the path to your SPColor file.
SPFile colorPaletteFile = Web.GetFile("path to .spcolor file");
if (null == colorPaletteFile || !colorPaletteFile.Exists)
{
    // TODO: handle the error.
    return;
}

// Get the SPFont file. Replace with the path to your SPFont file.
SPFile fontSchemeFile = Web.GetFile("path to .spfont file");
if (null == fontSchemeFile || !fontSchemeFile.Exists)
{
    // TODO: handle the error.
    return;
}

// Open an SPTheme with the two files. Replace NewTheme with the name for your theme.
// Note: If you have a background image, you can specify the following:
// SPTheme.Open("NewTheme", colorPaletteFile, fontSchemeFile, backgroundURI)
SPTheme theme = SPTheme.Open("NewTheme", colorPaletteFile, fontSchemeFile);


// Now apply your theme to the site.
// The themed CSS output files are stored in the Themed folder of the Theme Gallery of the root web
// of the site collection. To specify that the files should be stored in the _themes folder within the root 
// web, pass false to the ApplyTo method.
theme.ApplyTo(Web, true);
  ```


> **NOTA**
> El parámetro  _shareGenerated_ del método **ApplyTo** especifica si los archivos de tema se pueden compartir entre sitios de una colección de sitios. En general, se establece en **true** para sitios de SharePoint Server y SharePoint Online y en **false** para sitios de SharePoint Foundation. Si quiere que los archivos de tema se compartan, tiene que establecer el parámetro _shareGenerated_ en **true**. Para obtener más información, consulte  [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) .

Cuando un usuario aplica un tema en el asistente **Cambiar la apariencia**, este también actualiza un tema llamado Current de la lista Diseños compuestos y la galería de diseño. Al aplicar un tema mediante programación, tiene que actualizar el tema Current de forma manual. En el siguiente ejemplo se muestra cómo actualizar el tema Current.
    


  ```cs
  
SPList designGallery = Web.GetCatalog(SPListTemplateType.DesignCatalog);
if (null == designGallery)
{
    // TODO: Handle the error.
    return;
}

SPQuery q = new SPQuery();
q.RowLimit = 1;
q.Query = "<Where><Eq><FieldRef Name='DisplayOrder'/><Value Type='Number'>0</Value></Eq></Where>";
q.ViewFields = "<FieldRef Name='DisplayOrder'/>";
q.ViewFieldsOnly = true;

SPListItemCollection currentItems = designGallery.GetItems(q);

If (currentItems.Count == 1)
{
    // Remove the old Current item.
    currentItems[0].Delete();
}

SPListItem currentItem = designGallery.AddItem();

currentItem["Name"] = SPResource.GetString(CultureInfo.CurrentUICulture, Strings.DesignGalleryCurrentItemName);
currentItem["Title"] = SPResource.GetString(CultureInfo.CurrentUICulture, Strings.DesignGalleryCurrentItemName);

// Change this line if you want to specify a different master page.
currentItem["MasterPageUrl"] = Web.MasterUrl;

// Replace with the path to your SPColor file.
currentItem["ThemeUrl"] = "path to .spcolor file";

// Delete the following line if you do not have a background image. Otherwise, replace with the path to
// the background image.
currentItem["ImageUrl"] = "path to background image"; 

// Replace with the path to your SPFont file. Or, you can delete this line if you want to use
// the default font scheme of the selected master page.
currentItem["FontSchemeUrl"] = "path to .spfont file"; 

currentItem["DisplayOrder"] = 0;
currentItem.Update();

  ```


## Recursos adicionales
<a name="bk_addresources"> </a>


-  [Desarrollar el diseño del sitio en SharePoint 2013](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [Paletas de colores y fuentes de SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [Cómo: crear un archivo de vista previa de página maestra en SharePoint 2013](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [Blog del equipo de SharePoint: presuma de estilo con los temas de SharePoint](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

