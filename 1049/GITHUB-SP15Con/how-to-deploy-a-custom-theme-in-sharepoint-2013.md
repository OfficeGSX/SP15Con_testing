---
title: Инструкции. Развертывание пользовательской темы в SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# Инструкции. Развертывание пользовательской темы в SharePoint 2013
Узнайте, как развертывать пользовательскую тему на сайте SharePoint 2013, используя пользовательский интерфейс или приемник компонента.
SharePoint 2013 содержит предустановленные темы. Кроме того, можно создавать пользовательские темы с помощью собственных дополнительных цветовых палитр, эталонных страниц и схем шрифтов. После отправки файлов в коллекцию тем и коллекцию эталонных страниц можно приступать к развертыванию темы на сайте с помощью пользовательского интерфейса или кода. Сведения о цветовых палитрах и схемах шрифтов см. в статье  [Цветовые палитры и шрифты в SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md).
  
    
    


## Основные понятия, которые нужно знать при развертывании темы
<a name="core"> </a>

В таблице 1 перечислены статьи, содержащие основные понятия, которые нужно знать при развертывании тем.
  
    
    

**Таблица 1. Основные понятия, которые нужно знать при развертывании темы**


|**Название статьи**|**Описание**|
|:-----|:-----|
| [Общие сведения о темах для SharePoint 2013](themes-overview-for-sharepoint-2013.md) <br/> |Сведения о работе с темами в SharePoint 2013.  <br/> |
| [События компонента](http://msdn.microsoft.com/ru-ru/library/ms469501.aspx) <br/> |Сведения о событиях компонента, позволяющих отслеживать события, возникающие при установке компонента в ферму серверов, и реагировать на них должным образом.  <br/> |
   

## Отправка файлов в коллекцию тем и коллекцию эталонных страниц
<a name="section1"> </a>

Для создания пользовательских тем можно использовать собственные дополнительные цветовые палитры и схемы шрифтов. Отправьте их в коллекцию тем, чтобы получить к ним доступ при изменении дизайна во время работы с темами или при программном применении темы. Аналогично в коллекцию эталонных страниц можно отправить дополнительные эталонные страницы и соответствующие файлы предварительного просмотра для добавления дополнительных макетов сайта. Ниже описано, куда помещать эти файлы.
  
    
    

- **Коллекция эталонных страниц**. Содержит файлы эталонных страниц и соответствующие им файлы предварительного просмотра (файлы PREVIEW). Файл предварительного просмотра эталонной страницы требуется, если необходимо, чтобы эталонная страница была доступна в мастере **изменения оформления**. Файлы JavaScript и другие файлы для оформления также можно отправлять в коллекцию эталонных страниц.
    
    Чтобы получить доступ к коллекции эталонных страниц из пользовательского интерфейса SharePoint, в разделе **Коллекция веб-дизайнера** на странице **Параметры сайта** выберите пункт **Главные страницы**. Вы также можете перейти непосредственно на сайт по ссылке http:// _ИмяСайта_/_catalogs/masterpage/.
    
  
- **Коллекция тем**. Содержит цветовые палитры и схемы шрифтов, доступные для работы с темами. SharePoint ищет доступные цветовые палитры и схемы шрифтов в папке **15**.
    
    Чтобы получить доступ к коллекции тем из пользовательского интерфейса SharePoint, в разделе **Коллекция веб-дизайнера** на странице **Параметры сайта** выберите пункт **Темы**. Вы также можете перейти непосредственно на сайт по ссылке http:// _ИмяСемействаСайтов_/_catalogs/theme/15/.
    
  
- **Библиотека стилей**. Содержит пользовательские файлы CSS, необходимые при работе с темами. Чтобы перейти непосредственно к библиотеке стилей, замените  _ИмяСемействаСайтов_ и _язык_ в этом URL-адресе: http:// _ИмяСемействаСайтов_/Style Library/ _язык_/Themable/.
    
> **Примечание**
> Поместите пользовательские файлы CSS в папку Themable, которая находится в библиотеке стилей, а не в коллекции эталонных страниц. Обработчик тем распознает только файлы CSS, хранящиеся в папке Themable в библиотеке стилей. 

> **Примечание**
> Если в коллекции эталонных страниц и коллекции тем включено управление версиями, необходимо также опубликовать файлы для оформления прежде, чем обработчик тем сможет их использовать. 
  
    
    


## Развертывание темы с помощью пользовательского интерфейса
<a name="section2"> </a>

Вариант оформления, или макет, включает в себя цветовую палитру, схему шрифтов, фоновое изображение и эталонную страницу, которые определяют внешний вид сайта. Список вариантов оформления содержит те из них, которые доступны в коллекции макетов. Чтобы создать макет, добавьте элемент в список вариантов оформления и укажите для него эталонную страницу, цветовую палитру, схему шрифтов и фоновое изображение.
  
    
    

> **Примечание**
> Если вы хотите, чтобы эталонная страница была доступна в коллекции макетов, потребуется файл предварительного просмотра эталонной страницы. 
  
    
    


### Чтобы добавить вариант оформления:


1. Щелкните значок **Параметры**, затем выберите пункт **Параметры сайта**.
    
  
2. В разделе **Коллекция веб-дизайнера** щелкните ссылку **Варианты оформления**.
    
  
3. В списке **Варианты оформления** щелкните ссылку **Создайте элемент**.
    
  
4. В текстовом поле **Название** введите название макета.
    
  
5. В текстовом поле **Имя** введите имя макета, которое отображается в списке вариантов оформления и в коллекции макетов.
    
  
6. В текстовом поле **URL-адрес главной страницы** введите URL-адрес эталонной страницы (можно использовать относительный URL-адрес).
    
  
7. В текстовом поле **URL-адрес темы** введите URL-адрес цветовой палитры, т. е. URL-адрес, ведущий к файлу SPCOLOR (можно использовать относительный URL-адрес).
    
  
8. В текстовом поле **URL-адрес изображения** введите URL-адрес фонового изображения. Этот шаг необязательный. Можно использовать относительный URL-адрес.
    
  
9. В текстовом поле **URL-адрес шрифтовой схемы** введите URL-адрес схемы шрифтов, т. е. URL-адрес, ведущий к файлу SPFONT. Этот шаг необязательный. Можно использовать относительный URL-адрес.
    
  
10. В текстовом поле **Порядок отображения** укажите для макета порядковый номер, определяющий место макета в коллекции макетов.
    
  
11. Нажмите кнопку **Сохранить**.
    
    > **Примечание**
      > Если в значениях варианта оформления допущена ошибка, он не будет добавлен в коллекцию макетов, а в файлах журнала не появится соответствующая запись. Вот некоторые из возможных причин, по которым вариант оформления не может быть добавлен: не удалось найти файл, возникла проблема с форматированием одного из файлов либо SharePoint не удалось получить доступ к файлам. 
Теперь можете применить новый макет на своем сайте, воспользовавшись коллекцией макетов. Дополнительные сведения см. в статье  [Выбор темы для сайта публикации](http://office.microsoft.com/ru-ru/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx) на сайте Office.com.
  
    
    

## Развертывание темы с помощью кода
<a name="section3"> </a>

Вы можете выполнить развертывание темы, используя приемник компонента.
  
    
    

### Для этого:


1. Создайте класс приемника компонента, который наследует классу  [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) .
    
  
2. В методе  [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) создайте объект [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) , использующий нужную цветовую палитру и схему шрифтов, а затем примените тему к своему сайту.
    
    Ниже приводится пример кода, в котором показано, как выполнить развертывание пользовательской цветовой палитры и схемы шрифтов на сайте.
    


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


> **Примечание**
> Параметр  _shareGenerated_ метода **ApplyTo** указывает на возможность совместного использования файлов темы другими сайтами семейства веб-сайтов. В большинстве случаев задается значение **true** для сайтов SharePoint Server и SharePoint Online, а также значение **false** для сайтов SharePoint Foundation. Для параметра _shareGenerated_ должно быть задано значение **true**, если файлы темы предназначены для совместного использования. Дополнительные сведения см. в статье  [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) .

Если пользователь применяет тему в мастере **изменения оформления**, мастер также обновляет тему с именем "Текущая" в списке вариантов оформления и коллекции макетов. При программном применении такую тему необходимо будет обновить вручную. В следующем примере показано, как обновить эту тему.
    


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


## Дополнительные ресурсы
<a name="bk_addresources"> </a>


-  [Разработка макета сайта в SharePoint 2013](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [Цветовые палитры и шрифты в SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [Инструкции. Создание файла предварительного просмотра эталонной страницы в SharePoint 2013](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [Блог команды разработчиков SharePoint. Свой собственный стиль с помощью тем SharePoint](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

