---
title: 如何：在 SharePoint 2013 中使用代码将术语固定到导航术语集
ms.prod: SHAREPOINT
ms.assetid: 4a2811dc-25fd-4eb2-b0ab-1edded64c556
---


# 如何：在 SharePoint 2013 中使用代码将术语固定到导航术语集
了解如何使用代码以便将术语固定到导航术语集。
在分类法创建中，固定是将术语附加到目标的功能。SharePoint 2013 介绍了术语固定。固定的术语与再用的术语类似（只读除外）并且在使用该术语的位置中进行更改。
  
    
    

在 SharePoint 2013 托管导航中，API 可让您将新或现有术语固定到  [NavigationTermSet](https://msdn.microsoft.com/library/Microsoft.SharePoint.Publishing.Navigation.NavigationTermSet.aspx) 对象。在 Microsoft SharePoint Server 2010 中，用户可以在术语层次结构的其他位置中再用术语（并且所有术语嵌套在再用的术语下）。再用这些术语后，可以在其他位置中对其进行修改，并且在再用这些术语的任何位置应该能够见到更改。
## 术语固定要领
<a name="SP15_H2UseCodeToPinTerms_TermPinningEssentials"> </a>

若要理解在 SharePoint 2013 中固定，可能想要了解托管元数据、术语、术语集、托管导航、术语库以及其他相关概念和 API。表 1 描述提供有关固定详细信息的文章。 
  
    
    

**表 1. 用于固定的核心概念**


|**文章标题**|**说明**|
|:-----|:-----|
| [针对 Microsoft SharePoint Server 2010 开发人员的企业元数据管理简介](http://msdn.microsoft.com/library/113a5d75-ac4d-498b-8436-725e04fb685d%28Office.15%29.aspx) <br/> |在对 SharePoint Server 2010 进行编写后，本文章提供有关企业托管编程模型和核心概念（例如术语和术语集）的基本概述。  <br/> |
| [SharePoint 2013 中的托管导航](managed-navigation-in-sharepoint-2013.md) <br/> |SharePoint 2013 中的分类法驱动的托管导航功能的简介。  <br/> |
   

## 将代码用于完成固定任务
<a name="SP15_H2UseCodeToPinTerms_UseCodeToCompletePinning"> </a>

您可以使用 .NET 服务器, .NET 客户端 (CSOM)、Silverlight 或 JavaScript 编程模型中的自定义代码来完成术语与术语集的固定操作。以下 .NET 服务器代码示例包括将术语固定到导航术语集的测试，以及可用于测试 **Term** 对象是否固定到指定的 **TermSet** 对象的方法。然后，该测试创建 **Term** 对象，并将其中一个对象固定到指定的 **NavigationTermSet** 对象。
  
    
    

### 将术语固定到导航术语集


- 以下示例测试如何将术语固定到导航术语集。使用包含托管导航方案（例如创建分类法驱动的网站导航菜单）所需方法和属性的  [NavigationTermSet](https://msdn.microsoft.com/library/Microsoft.SharePoint.SharePoint.NavigationTermSet.aspx) 对象。
    
    该示例首先检查 **NavigationTermSet** 对象是否存在。如果其中一个不存在，则该代码创建 **NavigationTermSet**。（如果其中一个已存在，则该代码将在创建新的一个之前删除旧的那个）然后，在该代码创建从中选择的某些 **Term** 对象之后，将为演示而创建发布页面 (.aspx) 文件，将其设置为针对已固定术语的自定义目标页面，然后将某些导航属性固定到该页面。
    


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


## 其他资源
<a name="SP15_H2UseCodeToPinTerms_AdditionalResources"> </a>


-  [SharePoint 2013 中的托管元数据和导航](managed-metadata-and-navigation-in-sharepoint-2013.md)
    
  
-  [Microsoft.SharePoint.Publishing.Navigation](https://msdn.microsoft.com/library/Microsoft.SharePoint.Publishing.Navigation.aspx)
    
  

