---
title: 如何：将 Document Library 列表中的 Name 字段导出到移动应用程序
ms.prod: SHAREPOINT
ms.assetid: 901c2012-18c6-4dbd-a787-f8650a0cc7a8
---


# 如何：将 Document Library 列表中的 Name 字段导出到移动应用程序
使用 Visual Studio SharePoint 列表向导将文档库列表的名称字段导出到移动应用程序。当用户在 SharePoint 中创建一个文档库的移动应用程序时，名称字段不会自动出现。
在文档库列表中，用户可以上传各种文档。文档的列表项通常包括 **Title**、 **Name** 以及文档链接作为属性。如果开发人员针对文档库创建 Windows Phone 7 应用程序， **Name** 字段无法在 IDE 向导中导出。因而开发人员很难知道该文档的名称。（这是因为在默认情况下 SharePoint 2013 不支持"FILE"类型的字段。）
  
    
    


> **注释**
> **Title** 属性不包括文件扩展名。
  
    
    


所以，如果要读取文档、创建文档的完整 URL 并在默认应用程序（例如 Word）中打开文档，您必须编写代码来获取实际的文件名。
  
    
    


> **重要信息**
> 如果您正在开发适用于 Windows Phone 8 的应用程序，则必须使用 Visual Studio Express 2012（而非 Visual Studio 2010 Express）。除开发环境以外，本文中的所有信息均适用于 Windows Phone 8 和 Windows Phone 7。 > 有关详细信息，请参阅 [如何：设置用于为 SharePoint 开发移动应用程序的环境](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)。 
  
    
    


## 导出文档库名称字段的先决条件


- SharePoint 2013
    
  
- 带有新 SharePoint 模板的 Visual Studio Express 2010
    
  

## 自定义生成的类
<a name="HowToExportTheNameFieldInADocumentLibraryListToAMobileApp_CustomizeTheGeneratedClases"> </a>

若要从文档库中导出 **Name** 字段并访问附件，您必须更改 **ListDataProvider** 类、DisplayItemViewModel.cs 和 **DisplayForm** 类。当开发人员使用 Visual Studio 的新 SPList 向导时，该向导将创建几个符合 Model-View-ViewModel 设计模式的类。（有关详细信息，请参阅 [使用 Model-View-ViewModel 模式](http://msdn.microsoft.com/zh-cn/library/hh821028.aspx)（http://msdn.microsoft.com/zh-cn/library/hh821028.aspx）。）会创建两个文件夹：一个名为 Views，其中包含修改各种列表视图所需的文件（如 DisplayForm、EditForm、List 和 NewForm）。另一个名为 ViewModels，其中包含 DisplayItemViewModel、EditItemViewModel、ListViewModel、NewItemViewModel。您将修改由向导生成的这些类中的某些类。
  
    
    

### 步骤 1：修改 ListDataProvider 以获取 SPListItem.File 属性


1. 在 **ListDataProvider** 类中，在 **LoadItem** 方法中添加以下代码行。
    
     `Context.Load(spListItem, Item => Item.File);`
    
  
2. 此外，在 **ListDataProvider** 类中，在 **LoadData** 方法中添加以下代码。
    
     `Context.Load(items, listItems => listItems.Include(item => item.File));`
    
  

### 步骤 2：添加 FileUrl 和 FileName 属性


- 在 DisplayItemViewModel.cs 中，将以下代码添加到 **DisplayVM**。
    
  ```cs
  
  public string m_fileUrl;
        public string FileUrl
        {
            get
            {
                if (string.IsNullOrEmpty(m_fileUrl))
                {
                    IListDataProvider p = this.DataProvider;
                    p.LoadItem(this.ID, (LoadItemCompletedEventArgs args) =>
                         {
                             FileUrl = this.DataProvider.SiteUrl + 
                                       args.Item.File.ServerRelativeUrl;
                         });
                }
                return m_fileUrl;
            }
            set
            {
                m_fileUrl = value;
                RaisePropertyChanged("FileUrl");
            }
        }
        public string m_fileName;
        public string FileName
        {
            get
            {
                if (string.IsNullOrEmpty(m_fileName))
                {
                    IListDataProvider p = this.DataProvider;
                    p.LoadItem(this.ID, (LoadItemCompletedEventArgs args) =>
                    {
                        FileName = args.Item.File.Name;
                    });
                }

                return m_fileName;
            }
            set
            {
                m_fileName = value;
                RaisePropertyChanged("FileName");
            }
        }
  ```


### 步骤 3：将超链接添加到绑定到 FileUrl 和 FileName 的 DisplayForm


- 在显示窗体中添加以下更改。
    
  ```XML
  
<StackPanel HorizontalAlignment="Left" Orientation="Horizontal" Margin="0,5,0,5">
  <TextBlock TextWrapping="Wrap" Width="150" HorizontalAlignment="Left" 
   Style="{StaticResource PhoneTextNormalStyle}">
    FileUrl :
  </TextBlock>
  <HyperlinkButton Content="{Binding FileName}" NavigateUri="{Binding FileUrl}" 
   x:Name="hypFile" TargetName="_blank" />
</StackPanel>

  ```


## 其他资源
<a name="SP15StoreSPlist_addlresources"> </a>


-  [构建访问 SharePoint 2013 的 Windows Phone 应用程序](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [如何：创建 Windows Phone SharePoint 2013 列表应用程序](how-to-create-a-windows-phone-sharepoint-2013-list-app.md)
    
  
-  [如何：设置用于为 SharePoint 开发移动应用程序的环境](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone 软件开发工具包 (SDK) 7.1](http://www.microsoft.com/zh-cn/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/en-us/download/details.aspx?id=30476)（http://www.microsoft.com/en-us/download/details.aspx?id=30476）
    
  

