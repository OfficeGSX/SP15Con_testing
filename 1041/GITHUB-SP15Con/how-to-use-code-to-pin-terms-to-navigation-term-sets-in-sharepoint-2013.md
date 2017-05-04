---
title: [方法] SharePoint 2013 で用語セット内を移動するために用語を固定するコードを使用する
ms.prod: SHAREPOINT
ms.assetid: 4a2811dc-25fd-4eb2-b0ab-1edded64c556
---


# [方法] SharePoint 2013 で用語セット内を移動するために用語を固定するコードを使用する
コードを使用して用語をナビゲーション用語セットに固定する方法を説明します。
分類作成において、固定 (ピン留め) とは、用語をターゲットにアタッチする機能のことです。SharePoint 2013 では、固定という語を使用します。固定された用語は、読み取り専用であり、その用語が使用される場所では変更できない点を除いて、再使用される用語と同じです。
  
    
    

SharePoint 2013 管理ナビゲーションでは、API を使用することにより、新規または既存の用語を  [NavigationTermSet](https://msdn.microsoft.com/library/Microsoft.SharePoint.Publishing.Navigation.NavigationTermSet.aspx) オブジェクトに固定できます。Microsoft SharePoint Server 2010 の場合、ユーザーは、用語階層内の他の場所にある用語 (および、再使用される用語の下でネストされたすべての用語) を再使用できました。それらの用語は、再使用された後、任意の場所で変更することができ、変更内容は用語が再使用されたすべての場所で確認できました。
## 用語固定の基本
<a name="SP15_H2UseCodeToPinTerms_TermPinningEssentials"> </a>

SharePoint 2013 での固定について理解するため、管理されたメタデータ、用語、用語セット、管理ナビゲーション、用語ストア、および他の関連概念と API について学ぶことができます。表 1 に、固定について詳細に説明している記事の一覧を示します。 
  
    
    

**表 1. 固定の基本概念**


|**記事のタイトル**|**説明**|
|:-----|:-----|
| [エンタープライズ メタデータ管理の概要 (Microsoft SharePoint Server 2010 の開発者向け)](http://msdn.microsoft.com/library/113a5d75-ac4d-498b-8436-725e04fb685d%28Office.15%29.aspx) <br/> |SharePoint Server 2010 についての記事です。この記事では、用語や用語セットなど、エンタープライズ管理のメタデータ プログラミング モデルと基本概念についての概要を述べています。  <br/> |
| [SharePoint 2013 でのマネージ ナビゲーション](managed-navigation-in-sharepoint-2013.md) <br/> |SharePoint 2013 での分類駆動型管理ナビゲーション機能の概要  <br/> |
   

## コードを使用した固定タスクの実行
<a name="SP15_H2UseCodeToPinTerms_UseCodeToCompletePinning"> </a>

.NET サーバー, .NET クライアント (CSOM)、Silverlight、または JavaScript プログラミング モデルからのカスタム コードを使用して、用語および用語セットの固定操作を実行できます。次の .NET サーバーのコード サンプルには、用語をナビゲーション用語セットに固定する場合のテストと、 **Term** オブジェクトが指定した **TermSet** オブジェクトに固定されているかどうかをテストするために使用できるメソッドが含まれています。その後テストでは、 **Term** オブジェクトを作成し、それらの 1 つを指定した **NavigationTermSet** オブジェクトに固定します。
  
    
    

### ナビゲーション用語セットに用語を固定するには


- 次のサンプルでは、ナビゲーション用語セットへの用語の固定をテストします。テストでは、用語駆動型サイト ナビゲーション メニューの作成などの管理ナビゲーションのシナリオで便利なメソッドやプロパティが含まれている  [NavigationTermSet](https://msdn.microsoft.com/library/Microsoft.SharePoint.SharePoint.NavigationTermSet.aspx) オブジェクトを使用します。
    
    サンプルでは、まず **NavigationTermSet** オブジェクトが存在するかどうかをチェックします。そのオブジェクトが存在しない場合、コードは **NavigationTermSet** を作成します (オブジェクトが既に存在する場合、コードは古いオブジェクトを削除してから新しいオブジェクトを作成します)。選択肢となるいくつかの **Term**オブジェクトが作成された後、コードにより、デモ用の発行ページ (.aspx) ファイルが作成され、固定される用語のカスタム ターゲット ページとしてそのページが設定され、いくつかのナビゲーション プロパティがそのページに固定されます。
    


  ```cs
  
  public void TermPinningTest()
        {
  using (SPSite site = new SPSite(TestConfig.ServerUrl))
            {
                using (SPWeb web = site.OpenWeb())
                {
                    TaxonomySession taxonomySession = new TaxonomySession(site, updateCache: true);

                    // Create the navigation term set.
                    NavigationTermSet menuNavTermSet = DemoUtilities.SetUpSampleNavTermSet(
                        this.TestContext, taxonomySession, web);
                    TermSet menuTaxTermSet = menuNavTermSet.GetTaxonomyTermSet();

                    TermStore termStore = menuTaxTermSet.TermStore;
                    Group group = menuTaxTermSet.Group;

                    // Does the tagging Taxonomy term set already exist?
                    TermSet taggingTaxTermSet = group.TermSets.FirstOrDefault(
                        ts => ts.Id == TestConfig.TaggingTermSetId);

                    if (taggingTaxTermSet != null)
                    {
                        Log("Deleting old tagging term set");

                        // If the tagging Taxonomy term set already exists, delete the old one.
                        taggingTaxTermSet.Delete();
                        termStore.CommitAll();
                    }

                    Log("Creating the tagging term set");

                    taggingTaxTermSet = group.CreateTermSet("Demo Tagging TermSet", TestConfig.TaggingTermSetId);

                    int lcid = termStore.WorkingLanguage;

                    // Create some terms.
                    Term taggingProductsTaxTerm = taggingTaxTermSet.CreateTerm("Products", lcid);
                    taggingProductsTaxTerm.CreateTerm("Electronics", lcid);
                    taggingProductsTaxTerm.CreateTerm("Footwear", lcid);
                    taggingProductsTaxTerm.CreateTerm("Sports", lcid);

                    termStore.CommitAll();

                    /// Pinning the products subtree. Pins the "Products" Term object to the NavigationTermSet object.
                    Term menuProductsTaxTerm = menuTaxTermSet.ReuseTermWithPinning(taggingProductsTaxTerm);
                    termStore.CommitAll();

                    /// Creating the publishing page template DemoTargetPage.aspx");
                    PublishingWeb publishingWeb = PublishingWeb.GetPublishingWeb(web);

                    SPListItem pageListItem = null;
                    PublishingPage publishingPage;
                    try
                    {
                        pageListItem = web.GetListItem(web.Url + "/Pages/DemoTargetPage.aspx");
                        publishingPage = PublishingPage.GetPublishingPage(pageListItem);
   
                    }
                    catch (FileNotFoundException)
                    {
                        Log("Creating new target page");
                        publishingPage = publishingWeb.AddPublishingPage("DemoTargetPage.aspx", publishingWeb.DefaultPageLayout);
                        Log("Checking in target page draft");
                        publishingPage.CheckIn("TermPinningTest");
                    }

                    // Set a custom target page for the pinned terms and then set some navigation properties.

                    // Normally the navigation objects are obtained by way of an optimized function such as
                    // TaxonomyNavigation.GetTermSetForWeb() or TaxonomyNavigationContext.Current.NavigationTerm.
                    // The function guarantees that URLs will be resolved using a valid NavigationTerm.View.
                    // But because we are populating totally new data, the cache will probably not be updated
                    // yet, so instead we manually construct a view using GetAsResolvedByWeb().
                    NavigationTerm menuProductsNavTerm = NavigationTerm.GetAsResolvedByWeb(menuProductsTaxTerm,
                        web, StandardNavigationProviderNames.GlobalNavigationTaxonomyProvider);

                    menuProductsNavTerm.TargetUrl.Value = publishingPage.Uri.AbsolutePath;
                    menuProductsNavTerm.TargetUrlForChildTerms.Value = publishingPage.Uri.AbsolutePath;

                    termStore.CommitAll();

                    Log("TermPinningTest completed successfully");
                }
            }

}
  ```


## その他のリソース
<a name="SP15_H2UseCodeToPinTerms_AdditionalResources"> </a>


-  [SharePoint 2013 のマネージ メタデータとナビゲーション](managed-metadata-and-navigation-in-sharepoint-2013.md)
    
  
-  [Microsoft.SharePoint.Publishing.Navigation](https://msdn.microsoft.com/library/Microsoft.SharePoint.Publishing.Navigation.aspx)
    
  

