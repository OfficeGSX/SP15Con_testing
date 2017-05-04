---
title: Comment  utiliser du code aux termes de broche à terme de navigation définit dans SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 4a2811dc-25fd-4eb2-b0ab-1edded64c556
---



# Comment : utiliser du code aux termes de broche à terme de navigation définit dans SharePoint 2013
Découvrez comment utiliser le code pour les termes du contrat de code confidentiel pour les ensembles de termes de navigation.
 * **S'applique à :*** 
  
    
    


|||
|:-----|:-----|
|**Dans cet article**          [Notions de base épinglage de termes](#SP15_H2UseCodeToPinTerms_TermPinningEssentials)           [Utiliser du code pour effectuer les tâches épingles](#SP15_H2UseCodeToPinTerms_UseCodeToCompletePinning)           [Ressources supplémentaires](#SP15_H2UseCodeToPinTerms_AdditionalResources) <br/> ||
   

Dans la création de taxonomie, l'épinglage est la possibilité de joindre un terme à une cible. SharePoint 2013 présente l'épinglage de termes. Un terme épinglé est identique à un terme qui est réutilisé, sauf qu'il est en lecture seule et ne peut pas être modifiée à l'emplacement où le terme est utilisé.
  
    
    

Dans la navigation SharePoint 2013 géré, l'API permet de code confidentiel des termes de nouveau ou existant à un objet  [NavigationTermSet](https://msdn.microsoft.com/library/Microsoft.SharePoint.Publishing.Navigation.NavigationTermSet.aspx) . Dans Microsoft SharePoint Server 2010, les utilisateurs peuvent réutiliser les termes (et tous les termes imbriqués sous les termes réutilisées) dans d'autres emplacements dans la hiérarchie des termes. Une fois ces termes ont été réutilisés, elles peuvent être modifiées dans n'importe quel endroit et les modifications doivent s'afficher partout les termes ont été réutilisés.
## Notions de base épinglage de termes
<a name="SP15_H2UseCodeToPinTerms_TermPinningEssentials"> </a>

Pour comprendre l'épinglage dans SharePoint 2013, vous souhaiterez en savoir plus sur les métadonnées gérées, les termes, les ensembles de termes gérés de la navigation, le magasin de termes et d'autres informations concepts et les API. Le tableau 1 décrit les articles qui fournissent des informations supplémentaires à propos de l'épinglage.
  
    
    

**Le tableau 1. Concepts de base pour l'épinglage**


|**Titre d'article**|**Description**|
|:-----|:-----|
| [Présentation succincte de la gestion des métadonnées d'entreprise à l'attention des développeurs Microsoft SharePoint Server 2010](http://msdn.microsoft.com/library/113a5d75-ac4d-498b-8436-725e04fb685d%28Office.15%29.aspx) <br/> |Écrit pour SharePoint Server 2010, cet article fournit une vue d'ensemble des métadonnées gérées d'entreprise modèle et les concepts fondamentaux, tels que des termes et ensembles de termes de programmation. <br/> |
| [Navigation gérée dans SharePoint 2013](managed-navigation-in-sharepoint-2013.md) <br/> |Introduction à la fonctionnalité de navigation gérée axée sur la taxonomie dans SharePoint 2013. <br/> |
   

## Utiliser du code pour effectuer les tâches épingles
<a name="SP15_H2UseCodeToPinTerms_UseCodeToCompletePinning"> </a>

Vous pouvez utiliser un code personnalisé à partir du .NET server, .NET client (CSOM), Silverlight ou JavaScript modèles de programmation pour effectuer des opérations mis en attente sur les termes et ensembles de termes. Les exemples de code suivants .NET server incluent un test pour les termes épingles à des ensembles de termes de navigation et une méthode que vous pouvez utiliser pour tester si un objet **Term** est mis en attente à un objet spécifié **TermSet**. Ensuite, le test crée des objets **Term** et un d'entre eux à l'objet spécifié **NavigationTermSet** des codes confidentiels.
  
    
    

### Épingler des termes de navigation dans les ensembles de termes


- L'exemple suivant teste des termes épingles à des ensembles de termes de navigation. Il utilise l'objet  [NavigationTermSet](https://msdn.microsoft.com/library/Microsoft.SharePoint.SharePoint.NavigationTermSet.aspx) , qui contient les méthodes et propriétés qui sont utiles dans les scénarios de navigation gérée, telles que la création des menus de navigation de site basé sur la taxonomie.
    
    L'exemple commence par vérifier s'il existe un objet **NavigationTermSet**. S'il n'existe pas, le code crée un **NavigationTermSet**. (Si elle existe déjà, le code supprime l'ancien avant d'en créer une nouvelle). Puis, une fois que le code crée des objets **Term** à choisir parmi, il crée un fichier de page (.aspx) publication à des fins de démonstration, il le définit comme les pages cibles personnalisées pour les termes épinglés et puis épingle certaines propriétés de navigation à la page.
    


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


## Ressources supplémentaires
<a name="SP15_H2UseCodeToPinTerms_AdditionalResources"> </a>


-  [Métadonnées et navigation gérées dans SharePoint 2013](managed-metadata-and-navigation-in-sharepoint-2013.md)
    
  
-  [Microsoft.SharePoint.Publishing.Navigation](https://msdn.microsoft.com/library/Microsoft.SharePoint.Publishing.Navigation.aspx)
    
  
