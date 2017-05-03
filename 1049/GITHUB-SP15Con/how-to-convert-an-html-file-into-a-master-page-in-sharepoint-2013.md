---
title: Инструкции. Преобразование HTML-файла в эталонную страницу SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: a76ab289-3256-45de-ac63-d5112a74e3c7
---


# Инструкции. Преобразование HTML-файла в эталонную страницу SharePoint 2013
С помощью Дизайнера вы можете преобразовать HTML-файл в эталонную страницу SharePoint 2013  MASTER-файл. После преобразования HTML-файл и эталонная страница будут связаны, чтобы при редактировании и сохранении HTML-файла эти изменения синхронизировались со связанной эталонной страницей.
## Общие сведения о преобразовании эталонной страницы
<a name="Introduction"> </a>

С помощью Дизайнера вы можете преобразовать HTML-файл в эталонную страницу SharePoint 2013  MASTER-файл. После преобразования HTML-файл и эталонная страница будут связаны, чтобы при редактировании и сохранении HTML-файла эти изменения синхронизировались со связанной эталонной страницей.
  
    
    
Почему вы хотите преобразовать HTML-файл вместо того, чтобы создать MASTER-файл с нуля? В SharePoint 2013 эталонные страницы работают так же, как в ASP.NET, но для правильного отображения эталонной страницы в SharePoint должны присутствовать некоторые специальные для SharePoint элементы, например элементы управления или заполнители контента. Используя Дизайнера для преобразования HTML-файла в полнофункциональную эталонную страницу SharePoint, вам не потребуется знание ASP.NET или специфичной разметки SharePoint. Вместо этого вы можете сосредоточиться на разработке своего сайта с помощью HTML, CSS и JavaScript.
  
    
    
При преобразовании HTML-файла в эталонную страницу:
  
    
    

- В коллекции эталонных страниц создается MASTER-файл с именем, аналогичным имени HTML-файла.
    
  
- В MASTER-файл добавляется вся разметка, необходимая SharePoint 2013, поэтому эталонная страница отображается правильно.
    
  
- В исходный HTML-файл добавляется разметка, например комментарии, теги **<div>**, фрагменты коды и заполнители контента.
    
  
- HTML-файл и эталонная страница связываются, поэтому в дальнейшем при сохранении внесенных в HTML-файл изменений, этот файл будет синхронизирован с MASTER-файлом.
    
  

> **Примечание**
> Синхронизация происходит только в одном направлении. Изменения в эталонной HTML-странице синхронизируются со связанным MASTER-файлом, однако изменения, внесенные в MASTER-файл напрямую, не будут синхронизированы с HTML-файлом. Каждая эталонная HTML-страница (и каждый макет HTML-страницы) имеет свойство **Связанный файл**, которое по умолчанию имеет значение **True** и устанавливает связь и процесс синхронизации между файлами.
  
    
    

Если у вас есть пара связанных файлов (HTML и MASTER) и вы изменяете MASTER-файл, не нарушая связь, внесенные изменения будут сохранены, но вы не сможете отметить или опубликовать этот файл, то есть по большому счету эти изменения не сохраняются. Любые изменения в файле HTML перезаписывают MASTER-файл. Если вы отметите или опубликуете HTML-файл, то его изменения перезапишут любые изменения, которые были сделаны в MASTER-файле. Изменения, внесенные в MASTER-файл, будут изменены.
  
    
    
Если вы являетесь разработчиком и вам удобно работать с ASP.NET, вы можете разорвать связь между файлами и работать только с MASTER-файлом. Чтобы разорвать связь между HTML- и MASTER-файлами, в Дизайнере выберите для HTML-файла команду **Изменить свойства** и снимите флажок напротив пункта **Связанный файл**. Позже вы можете вновь связать эти файлы, изменив свойства и установив флажок; в этом случае изменения, сохраненные в HTML-файле, вновь перезапишут MASTER-файл.
  
    
    

## Подготовка HTML-файла для преобразования
<a name="Prepare"> </a>

Перед тем как проводить преобразование HTML-файла, ознакомьтесь с некоторыми советами и рекомендациями, представленными ниже.
  
    
    

- Рассмотрите модель страниц SharePoint. Более подробную информацию можно посмотреть в статье  [Обзор модели страниц в SharePoint 2013](overview-of-the-sharepoint-2013-page-model.md). Скорее всего, при проектировании HTML-макетов для вашего сайта у вас будет несколько HTML-файлов для различных типов страниц, например страница статьи или страница категории, которая содержит веб-части, отображающие категорию элементов из каталога. Однако в эталонную страницу будет преобразован только один HTML-файл. HTML-файл можно преобразовать в эталонную страницу, а непосредственно в макет страницы  нельзя, так как для макета страницы требуются поля страницы.
    
  
- Убедитесь, что ваш HTML-файл совместим с XML, так как это требуется для выполнения преобразования. К сожалению, из-за этого требования переопределяются некоторые стандарты HTML 5, например в HTML 5 вы можете указать **doctype** в нижнем регистре, однако в XML **doctype** должен быть указан в верхнем регистре. Кроме того, вам следует удалить из своего файла все теги **<form>**. Проверьте работоспособность своего HTML-файла с помощью внешнего средства проверки XML, чтобы выявить ошибки XML до выполнения преобразования.
    
  
- Ознакомьтесь с этими важными рекомендациями для ссылок CSS:
    
  - Не помещайте блоки **<style>** в тег **<head>**. Эти стили будут удалены во время преобразования. Вместо этого укажите в HTML-файле ссылку на внешний CSS-файл.
    
  
  - Если вы используете веб-шрифты, добавьте в тег **<CSS link>** строку `ms-design-css-conversion="no"`.
    
  
  - Соблюдайте осторожность при применении стилей к общим HTML-тегам типа **<body>**, **<div>** и **< img>**. Все элементы оформления SharePoint, включая ленту, находятся внутри тега **<body>**. Стили, которые обычно применяются к тегу **<body>**, примените вместо **<div id="s4-bodyContainer">**  тега, который используется SharePoint 2013 для основной части страницы. Кроме того, SharePoint 2013 использует большое количество изображений, на которые действуют стили, примененные к тегу **<img>**.
    
  
  - Многие разработчики для того, чтобы применить стили к навигации, применяют классы к элементам **<ul>** и **<li>**. Однако в SharePoint 2013 используется динамический элемент управления навигацией, который можно добавить на эталонную страницу из коллекции фрагментов кода. SharePoint 2013 применяет к элементам управления навигацией стили по умолчанию, которые вам необходимо переопределить.
    
  
- Ознакомьтесь с возможными проблемами именования файлов:
    
  - Файлы Index.html и Index.htm будут иметь один и тот же MASTER-файл.
    
  
  - Файлы Design/Index.html и Design/SubDesign/Index.html будут преобразованы в отдельные MASTER-файлы, но они оба будут отображаться в списке эталонной странице в Дизайнере как Index.html. Чтобы устранить неоднозначность, щелкните каждый файл или нажмите кнопку с многоточием, чтобы увидеть полный путь.
    
  
- Если вы добавляете мини-приложение JavaScript, убедитесь, что открывающий тег **<script>** находится на отдельной строке.
    
  ```
  
  <script>
  (function( …

  ```


    Не помещайте их в одну строку, как это показано ниже:
    


  ```
  
  <Script> (function( …
  ```

- Ссылка на библиотеку JQuery (внешняя ссылка) должна находиться перед тегом **</head>**.
    
  

## Преобразование HTML-файла в эталонную страницу
<a name="Convert"> </a>

Перед выполнением преобразования HTML-файла сначала необходимо отправить все файлы разработки, включая HTML-файл. Более подробную информацию можно узнать в статье  [Как: сопоставление сетевого диска коллекции главных страниц SharePoint 2013](how-to-map-a-network-drive-to-the-sharepoint-2013-master-page-gallery.md).
  
    
    

### Чтобы преобразовать HTML-файл в MASTER-файл:


1. Перейдите на сайт публикации.
    
  
2. В правом верхнем углу страницы выберите пункт **Параметры**, а затем выберите **Дизайнер**.
    
  
3. В Дизайнере в левой области панели навигации выберите команду **Изменить главные страницы**.
    
  
4. Выберите пункт **Преобразование HTML-файла в главную страницу SharePoint**.
    
  
5. В диалоговом окне **Выбор актива** найдите и выберите преобразуемый HTML-файл.
    
    > **Примечание**
      > При отправке файлов разработки сохраните все файлы, связанные с одним оформлением, в собственной папке в коллекции главных страниц. Когда выполняется копирование папки разработки на подключенный сетевой диск, коллекция эталонных страниц сохраняет всю созданную вами структуру папок. 
6. Нажмите кнопку **Вставить**.
    
    На этом этапе SharePoint 2013 преобразует ваш HTML-файл в MASTER-файл с тем же именем.
    
    Теперь в Дизайнере для вашего HTML-файла отображается столбец "Состояние", который показывает один из двух возможных состояний:
    
  - Ошибки
    
  
  - **Преобразование выполнено успешно**.
    
  
7. Пройдите по ссылке в столбце "Состояние", чтобы предварительно просмотреть файл и просмотреть ошибки или предупреждения, касающиеся эталонной страницы.
    
    Ошибки
    
    Более подробную информацию по устранению ошибок и предупреждений можно узнать в статье  [Как: Устранение ошибок и предупреждения при предварительном просмотре страницы в SharePoint 2013](how-to-resolve-errors-and-warnings-when-previewing-a-page-in-sharepoint-2013.md).
    
    Более подробную информацию о предварительном просмотре эталонной страницы с различными страницами можно узнать в статье  [Способ: изменение страницы предварительного просмотра в диспетчере оформления SharePoint 2013](how-to-change-the-preview-page-in-sharepoint-2013-design-manager.md).
    
    В верхнем правом углу страницы предварительного просмотра также находится ссылка "Фрагменты кода". При переходе по ней откроется коллекция фрагментов кода, где можно заменить статические элементы управления или элементы управления макета в своем конструкторе на динамические элементы управления SharePoint. Дополнительную информацию можно узнать в статье  [Фрагменты кода дизайнер SharePoint 2013](sharepoint-2013-design-manager-snippets.md).
    
  
8. Чтобы устранить ошибки, с помощью HTML-редактора откройте и измените HTML-файл на подключенном диске на стороне сервера. Каждый раз при сохранении HTML-файла, все изменения синхронизируется со связанным MASTER-файлом.
    
  
9. После успешного выполнения предварительного просмотра эталонной страницы вы увидите тег **<div>**, добавленный в ваш HTML-файл. Прокрутите страницу вниз, чтобы увидеть тег **<div>**.
    
    Тег **<div>**  это блок основного содержимого. Он находится внутри заполнителя контента с именем **ContentPlaceHolderMain**. Во время выполнения, когда посетитель просматривает ваш сайт и запрашивает страницу, в этом заполнителе контента находится содержимое из макета страницы, которая содержит контент в соответствующей области содержимого. Располагайте тег **<div>** в местах, где на эталонной странице должны отображаться макеты страниц.
    
    Если ваш HTML-файл содержит статическое содержимое или содержимое макета в теле страницы, то теперь можно начать процесс удаления данного статического содержимого из эталонной HTML-страницы и применить эти стили к элементам модели страниц SharePoint, например макетам страниц, элементам управления полями страницы, фрагментам кода и шаблона для отображения. Пример можно увидеть в статье  [Инструкции. Создание макета страницы в SharePoint 2013](how-to-create-a-page-layout-in-sharepoint-2013.md).
    
  

## Общие сведения об HTML-файл после преобразования
<a name="Understand"> </a>

После преобразования HTML-файла в эталонную страницу в этот файл будет добавлено большое количество строк разметки. Вы можете не обращать внимания на большую ее часть, так как она не будет отображаться в конечной разметке вашего сайта при просмотре источника в браузере, однако эта разметка критична для преобразования вашего HTML-файла в MASTER-файл, который фактически используется SharePoint. Каждый раз при сохранении HTML-файла эта разметка SharePoint позволяет выполнить аналогичные изменения в связанном MASTER-файле в фоновом режиме.
  
    
    
Добавляемая разметка включает теги перед тегом **<head>** и после него, фрагменты кода и заполнители контента. Большая часть разметки заключена в теги комментариев. Каждый раз при сохранении изменений в HTML-файле процесс преобразования вырезает комментарии, чтобы использовать внутри разметку ASP.NET.
  
    
    

### Типы разметки

Ниже приведена разбивка типов разметки, которые добавляются в HTML-файл:
  
    
    

- **Свойства документа**. Тег **<mso>** содержит метаданные SharePoint, включая информацию о самом файле и некоторых свойствах, требуемых для успешного преобразования MASTER-файла.
    
  ```HTML
  
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
  ```

- **Регистрация пространства имен SharePoint**. Тег **<SPM>** (разметка SharePoint) предоставляет строку для регистрации пространства имен SharePoint.
    
  ```HTML
  
<!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
  ```

- **Комментарии**. Теги **<CS>** и **<CE>** ("comment start" и "comment end"  начало комментария и конец комментария, соответственно) игнорируются во время выполнения преобразования. Они помогают вам разобраться в строках разметки.
    
  ```HTML
  
<!--CS: Start Page Head Contents Snippet-->
…
<!--CE: End Page Head Contents Snippet-->

  <!--CS: Start Ribbon Snippet-->
…
<!--CE: End Ribbon Snippet-->

<!--CS: Start PlaceHolderMain Snippet-->
…
<!--CE: End PlaceHolderMain Snippet-->
  ```

- **Фрагменты кода**. Теги **<MS>** и **<ME>** ("markup start" и "markup end"  начало разметки и конец разметки, соответственно) обозначают начало и конец элемента управления SharePoint или фрагмента кода. Фрагмент кода является элементом управления SharePoint, который добавляет на вашу страницу возможности SharePoint. Вы можете добавить фрагменты кода самостоятельно с использованием коллекции фрагментов кода. Дополнительную информацию можно узнать в статье [Фрагменты кода дизайнер SharePoint 2013](sharepoint-2013-design-manager-snippets.md).
    
  ```HTML
  
<!--MS:<SharePoint:RobotsMetaTag runat="server">-->
        <!--ME:</SharePoint:RobotsMetaTag>-->
        <!--MS:<SharePoint:PageTitle runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server">-->
                <!--MS:<SharePoint:ProjectProperty Property="Title" runat="server">-->
                <!--ME:</SharePoint:ProjectProperty>-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:PageTitle>-->
        <!--MS:<SharePoint:StartScript runat="server">-->
        <!--ME:</SharePoint:StartScript>-->
        <!--MS:<SharePoint:CssLink runat="server" Version="15">-->
        <!--ME:</SharePoint:CssLink>-->
        <!--MS:<SharePoint:CacheManifestLink runat="server">-->
        <!--ME:</SharePoint:CacheManifestLink>-->
        <!--MS:<SharePoint:PageRenderMode runat="server" RenderModeType="Standard">-->
        <!--ME:</SharePoint:PageRenderMode>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="core.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="menu.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="callout.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="sharing.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="suitelinks.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:CustomJSUrl runat="server">-->
        <!--ME:</SharePoint:CustomJSUrl>-->
        <!--MS:<SharePoint:SoapDiscoveryLink runat="server">-->
        <!--ME:</SharePoint:SoapDiscoveryLink>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaPlaceHolderAdditionalPageHead" Container="false" runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderAdditionalPageHead" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
            <!--MS:<SharePoint:DelegateControl runat="server" ControlId="AdditionalPageHead" AllowMultipleControls="true">-->
            <!--ME:</SharePoint:DelegateControl>-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderBodyAreaClass" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--MS:<SharePoint:CssRegistration Name="Themable/corev15.css" runat="server">-->
        <!--ME:</SharePoint:CssRegistration>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaSPWebPartManager" runat="server">-->
            <!--MS:<WebPartPages:SPWebPartManager runat="server">-->
            <!--ME:</WebPartPages:SPWebPartManager>-->
        <!--ME:</SharePoint:AjaxDelta>-->
  ```

- **Блоки предварительного просмотра**. В теги **<PS>** и **<PE>** ("preview start" и "preview end"  начало предварительного просмотра и конец предварительного просмотра) заключается раздел HTML-кода, который не следует редактировать, так как он влияет только на предварительный просмотр во время разработки. Эти разделы представляют собой моментальный снимок во времени для элемента управления SharePoint, фрагмент кода которого вставлен. Предварительный просмотр позволяет вам более осмысленно работать над HTML-файлом в клиентском HTML-редакторе. Однако изменение содержимого или применение стилей в рамках этого предварительного просмотра не имеет продолжительного влияния на MASTER-файл, который в конечном итоге используется SharePoint. Чтобы задать стиль фрагмента кода, вы должны определить и переопределить стили SharePoint с помощью собственного пользовательского CSS.
    
  ```HTML
  
<!--PS: Start of READ-ONLY PREVIEW (do not modify) -->
<div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div>
<!--PE: End of READ-ONLY PREVIEW -->
  ```

- **Идентификаторы SharePoint**. Два фрагмента кода, добавленных в ваш HTML-файл в процессе преобразования (фрагмент кода "Содержимое заголовка страницы" и лента SharePoint), имеют связанный идентификатор SharePoint или идентификатор безопасности (00 и 02, соответственно). Эти идентификаторы позволяют сократить фрагменты кода и упрощают чтение HTML-кода на странице.
    
  ```HTML
  
<!--SID:00 -->

<!--SID:02 {Ribbon}-->
  ```


### Добавленные фрагменты кода

Важно знать о двух фрагментах кода, которые будут добавлены в HTML-файл. Эти фрагменты кода добавляются автоматически во время преобразования, но они могут быть недоступны для добавления из коллекции фрагментов кода.
  
    
    

- **Лента**. Чтобы авторы содержимого могли создавать страницы и авторское содержимое на вашем сайте SharePoint, на вашей эталонной странице должна быть лента и новая "комплексная навигация" для SharePoint 2013. Лента содержится в фрагменте кода фильтрации по ролям безопасности, поэтому когда посетители просматривают ваш сайт, эта лента отображается только для авторизованных пользователей, но не для анонимных. Вы можете переместить ленту в другое место на сайте или определить ее стиль, переопределив классы CSS по умолчанию, однако мы не рекомендуем перемещать или переопределять компоненты (например, меню "Действия сайта"), которые содержатся в ленте.
    
  ```HTML
  
<!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
<!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
<!--ME:</wssucw:Welcome>-->
<!--ME:</SharePoint:SPSecurityTrimmedControl>-->
  ```

- **ContentPlaceHolderMain** В нижней части тега **<div id="s4-bodyContainer">** перед закрывающим тегом **</body>** процесс преобразования вставляет заменитель контента с именем **PlaceHolderMain**. Внутри этот фрагмент кода находится обведенный черной рамкой желтый тег **<div>**, который появляется в режиме конструктора в вашем HTML-редакторе или при предварительном просмотре в Дизайнере на стороне сервера.
    
    Этот тег **<div>** представляет область, где выполняется содержимое, указанное макетом страницы и страницами. Вам следует переместить фрагмент кода **PlaceHolderMain**, чтобы поместить внутри эталонной страницы, которую заполнят макеты страницы, область структуры сайта, которая различается по всем страницам вашего сайта.
    


  ```HTML
  
<!--CS: Start PlaceHolderMain Snippet-->
                    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
                    <!--MS:<SharePoint:AjaxDelta ID="DeltaPlaceHolderMain" IsMainContent="true" runat="server">-->
                        <!--MS:<asp:ContentPlaceHolder ID="PlaceHolderMain" runat="server">-->
                            <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
            This div, which you should delete, represents the content area that your Page Layouts and pages will fill. Design your Master Page around this content placeholder.
        
                            </div>
                        <!--ME:</asp:ContentPlaceHolder>-->
                    <!--ME:</SharePoint:AjaxDelta>-->
                    <!--CE: End PlaceHolderMain Snippet-->
  ```


## Примеры
<a name="Reference"> </a>

Ниже приведен пример разметки, добавленной в HTML-файл после его преобразования в эталонную страницу.
  
    
    

### Разметка, добавленная в тег <head>


```HTML

<head>
        <meta http-equiv="X-UA-Compatible" content="IE=10" />
        <!--CS: Start Page Head Contents Snippet-->
        <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SID:00 -->
        <meta name="GENERATOR" content="Microsoft SharePoint" />
        <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
        <meta http-equiv="Expires" content="0" />
        <!--MS:<SharePoint:RobotsMetaTag runat="server">-->
        <!--ME:</SharePoint:RobotsMetaTag>-->
        <!--MS:<SharePoint:PageTitle runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server">-->
                <!--MS:<SharePoint:ProjectProperty Property="Title" runat="server">-->
                <!--ME:</SharePoint:ProjectProperty>-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:PageTitle>-->
        <!--MS:<SharePoint:StartScript runat="server">-->
        <!--ME:</SharePoint:StartScript>-->
        <!--MS:<SharePoint:CssLink runat="server" Version="15">-->
        <!--ME:</SharePoint:CssLink>-->
        <!--MS:<SharePoint:CacheManifestLink runat="server">-->
        <!--ME:</SharePoint:CacheManifestLink>-->
        <!--MS:<SharePoint:PageRenderMode runat="server" RenderModeType="Standard">-->
        <!--ME:</SharePoint:PageRenderMode>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="core.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="menu.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="callout.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="sharing.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="suitelinks.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:CustomJSUrl runat="server">-->
        <!--ME:</SharePoint:CustomJSUrl>-->
        <!--MS:<SharePoint:SoapDiscoveryLink runat="server">-->
        <!--ME:</SharePoint:SoapDiscoveryLink>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaPlaceHolderAdditionalPageHead" Container="false" runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderAdditionalPageHead" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
            <!--MS:<SharePoint:DelegateControl runat="server" ControlId="AdditionalPageHead" AllowMultipleControls="true">-->
            <!--ME:</SharePoint:DelegateControl>-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderBodyAreaClass" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--MS:<SharePoint:CssRegistration Name="Themable/corev15.css" runat="server">-->
        <!--ME:</SharePoint:CssRegistration>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaSPWebPartManager" runat="server">-->
            <!--MS:<WebPartPages:SPWebPartManager runat="server">-->
            <!--ME:</WebPartPages:SPWebPartManager>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--CE: End Page Head Contents Snippet-->
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <!--DC:Business Solutions-->
        <link rel="stylesheet" href="css/style.css" type="text/css" charset="utf-8" />
        <!--[if lte IE 7]>
  <link rel="stylesheet" href="css/ie.css" type="text/css" charset="utf-8"/> 
 <![endif]-->
        <!--[if gte mso 9]><xml>
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
</xml><![endif]-->
    </head>

```


### Разметка, добавленная после открывающего тега <body>


#### Фрагмент кода ленты


```HTML

<!--CS: Start Ribbon Snippet-->
        <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="wssucw" TagName="Welcome" Src="~/_controltemplates/15/Welcome.ascx"%>-->
        <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" HideFromSearchCrawler="true" EmitDiv="true">-->
            <div id="TurnOnAccessibility" style="display:none" class="s4-notdlg noindex">
                <a id="linkTurnOnAcc" href="#" class="ms-accessible ms-acc-button" onclick="SetIsAccessibilityFeatureEnabled(true);UpdateAccessibilityUI();document.getElementById('linkTurnOffAcc').focus();return false;">
                    <!--MS:<SharePoint:EncodedLiteral runat="server" text="&amp;lt;%$Resources:wss,master_turnonaccessibility%&amp;gt;" EncodeMethod="HtmlEncode">-->
                    <!--ME:</SharePoint:EncodedLiteral>-->
                </a>
            </div>
            <div id="TurnOffAccessibility" style="display:none" class="s4-notdlg noindex">
                <a id="linkTurnOffAcc" href="#" class="ms-accessible ms-acc-button" onclick="SetIsAccessibilityFeatureEnabled(false);UpdateAccessibilityUI();document.getElementById('linkTurnOnAcc').focus();return false;">
                    <!--MS:<SharePoint:EncodedLiteral runat="server" text="&amp;lt;%$Resources:wss,master_turnoffaccessibility%&amp;gt;" EncodeMethod="HtmlEncode">-->
                    <!--ME:</SharePoint:EncodedLiteral>-->
                </a>
            </div>
        <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
        <div id="ms-designer-ribbon">
            <!--SID:02 {Ribbon}-->
            <!--PS: Start of READ-ONLY PREVIEW (do not modify) --><div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div><!--PE: End of READ-ONLY PREVIEW -->
        </div>
        <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
            <!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
            <!--ME:</wssucw:Welcome>-->
        <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
        <!--CE: End Ribbon Snippet-->

```


#### Два тега SharePoint <div>


```HTML

<div id="s4-workspace">
            <div id="s4-bodyContainer">

```


### Разметка, добавленная перед закрывающим тегом </body> и двумя закрывающими тегами </div>


```HTML

<div data-name="ContentPlaceHolderMain">
                    <!--CS: Start PlaceHolderMain Snippet-->
                    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
                    <!--MS:<SharePoint:AjaxDelta ID="DeltaPlaceHolderMain" IsMainContent="true" runat="server">-->
                        <!--MS:<asp:ContentPlaceHolder ID="PlaceHolderMain" runat="server">-->
                            <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
            This div, which you should delete, represents the content area that your Page Layouts and pages will fill. Design your Master Page around this content placeholder.
        
                            </div>
                        <!--ME:</asp:ContentPlaceHolder>-->
                    <!--ME:</SharePoint:AjaxDelta>-->
                    <!--CE: End PlaceHolderMain Snippet-->
                </div>

```


## Дополнительные ресурсы
<a name="Additional"> </a>


-  [Обзор Дизайнера в SharePoint 2013](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [Инструкции. Создание макета страницы в SharePoint 2013](how-to-create-a-page-layout-in-sharepoint-2013.md)
    
  
-  [Фрагменты кода дизайнер SharePoint 2013](sharepoint-2013-design-manager-snippets.md)
    
  

