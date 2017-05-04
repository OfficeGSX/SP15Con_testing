---
title: [方法] ドキュメント ライブラリ リストの名前フィールドをモバイル アプリにエクスポートする
ms.prod: SHAREPOINT
ms.assetid: 901c2012-18c6-4dbd-a787-f8650a0cc7a8
---


# [方法] ドキュメント ライブラリ リストの名前フィールドをモバイル アプリにエクスポートする
Visual Studio SharePoint リスト ウィザードを使用して、ドキュメント ライブラリ リストの名前フィールドをモバイル アプリにエクスポートします。ユーザーが SharePoint でドキュメント ライブラリ用のモバイル アプリを作成する場合、名前フィールドは自動的には表示されません。
ドキュメント ライブラリ リストでは、各種のドキュメントをアップロードできます。ドキュメントのリスト項目には、通常は **Title**、 **Name**、およびドキュメントへのリンクがプロパティとして含まれます。開発者がドキュメント ライブラリに対して Windows Phone 7 アプリを作成した場合、 **Name** フィールドは IDE ウィザードにエクスポートされません。したがって、開発者が簡単にドキュメント名を調べられる方法はありません (これは、SharePoint 2013で "FILE" タイプのフィールドが既定ではサポートされていないからです)。
  
    
    


> **メモ**
> **Title** プロパティにはファイル名の拡張子は含まれません。
  
    
    


したがって、ドキュメントを取得して、ドキュメントの完全な URL を作成し、既定のアプリケーション (Word など) でドキュメントを開くためには、実際のファイル名を取得するコードを記述する必要があります。
  
    
    


> **重要**
> Windows Phone 8 用アプリを開発している場合、, Visual Studio 2010 Express ではなく Visual Studio Express 2012 を使用する必要があります。開発環境を除き、この記事のすべての情報は、Windows Phone 8 用アプリと Windows Phone 7 用アプリを作成する場合に適用されます。 > 詳細については、「 [[方法]: SharePoint 用モバイル アプリの開発環境をセットアップする](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)」を参照してください。 
  
    
    


## ドキュメント ライブラリの名前フィールドのエクスポートの前提条件


- SharePoint 2013
    
  
- 新しい SharePoint テンプレートを備えた Visual Studio Express 2010
    
  

## 生成されたクラスのカスタマイズ
<a name="HowToExportTheNameFieldInADocumentLibraryListToAMobileApp_CustomizeTheGeneratedClases"> </a>

 **Name** フィールドをエクスポートしてドキュメント ライブラリから添付ファイルにアクセスするには、 **ListDataProvider** クラスの DisplayItemViewModel.cs、および **DisplayForm** クラスにいくつかの変更を加える必要があります。開発者が Visual Studio から新しい SPList ウィザードを使用している場合、ウィザードは Model-View-ViewModel 設計パターンに従っていくつかのクラスを作成します (詳細については、「 [Using the Model-View-ViewModel Pattern](http://msdn.microsoft.com/ja-jp/library/hh821028.aspx) を参照してください)。 次の 2 つのフォルダーが作成されます。1 つは Views という名前で、各種のリスト ビュー (DisplayForm、EditForm、List、NewForm など) の編集に必要なすべてのファイルが含まれます。もう 1 つは ViewModels という名前で、DisplayItemViewModel、EditItemViewModel、ListViewModel、および NewItemViewModel が含まれます。ウィザードによって生成されるこれらのクラスのうちのいくつかを編集します。
  
    
    

### 手順 1: ListDataProvider を編集して SPListItem.File プロパティを取得する


1. **ListDataProvider** クラスで、 **LoadItem** メソッドに次のコードの行を追加します。
    
     `Context.Load(spListItem, Item => Item.File);`
    
  
2. 同様に、 **ListDataProvider** クラスで **LoadData** メソッドに次のコードを追加します。
    
     `Context.Load(items, listItems => listItems.Include(item => item.File));`
    
  

### 手順 2: FileUrl および FileName プロパティを追加する


- DisplayItemViewModel.cs で、 **DisplayVM** に次のコードを追加します。
    
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


### 手順 3: FileUrl および FileName にバインドする DisplayForm にハイパーリンクを追加する


- Display フォームに次の変更を加えます。
    
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


## その他の技術情報
<a name="SP15StoreSPlist_addlresources"> </a>


-  [SharePoint 2013 にアクセスする Windows Phone アプリの作成](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [[方法] Windows Phone 用の SharePoint 2013 リスト アプリを作成する](how-to-create-a-windows-phone-sharepoint-2013-list-app.md)
    
  
-  [[方法]: SharePoint 用モバイル アプリの開発環境をセットアップする](how-to-set-up-an-environment-for-developing-mobile-apps-for-sharepoint.md)
    
  
-  [Windows Phone SDK 7.1](http://www.microsoft.com/ja-jp/download/details.aspx?id=27570)
    
  
-  [Microsoft SharePoint SDK for Windows Phone 7.1](http://www.microsoft.com/ja-jp/download/details.aspx?id=30476)
    
  

