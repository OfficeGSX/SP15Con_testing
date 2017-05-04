---
title: Procédure  Déployer un thème personnalisé dans SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# Procédure : Déployer un thème personnalisé dans SharePoint 2013
Apprenez à déployer un thème personnalisé sur un site SharePoint 2013 à l'aide de l'interface utilisateur ou via l'implémentation d'un récepteur de fonctionnalités.
SharePoint 2013 inclut des thèmes préinstallés. Vous pouvez créer des thèmes personnalisés en créant des palettes de couleurs, des jeux de polices et des pages maîtres supplémentaires. Une fois les fichiers chargés dans la galerie de thèmes et la galerie de pages maîtres, vous pouvez déployer un thème sur un site à l'aide de l'interface utilisateur ou en utilisant du code. Pour plus d'informations sur les palettes de couleurs et les jeux de polices, voir  [Palettes de couleurs et polices dans SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md).
  
    
    


## Concepts de base à connaître pour déployer un thème
<a name="core"> </a>

Le tableau 1 répertorie les articles qui peuvent vous aider à comprendre les concepts de base du déploiement de thèmes.
  
    
    

**Tableau 1. Concepts de base pour le déploiement d'un thème**


|**Titre d'article**|**Description**|
|:-----|:-----|
| [Vue d'ensemble des thèmes pour SharePoint 2013](themes-overview-for-sharepoint-2013.md) <br/> |Apprenez à utiliser des thèmes dans SharePoint 2013.  <br/> |
| [Événements de fonctionnalité](http://msdn.microsoft.com/fr-fr/library/ms469501.aspx) <br/> |Découvrez les événements de fonctionnalité, qui permettent d'interrompre un événement qui se produit lorsqu'une fonctionnalité est installée dans la batterie de serveurs ou d'y répondre.  <br/> |
   

## Charger des fichiers dans la galerie de thèmes et la galerie de pages maîtres
<a name="section1"> </a>

Vous pouvez créer des thèmes personnalisés en créant des palettes de couleurs et des jeux de polices supplémentaires, que vous chargez dans la galerie de thèmes. Les nouveaux jeux de polices et palettes de couleurs sont dès lors disponibles lorsque vous modifiez une conception dans l'interface de thèmes ou lorsque vous appliquez un thème par programmation. De la même manière, si vous souhaitez disposer de modèles de mise en page de site supplémentaires, vous pouvez charger des pages maîtres supplémentaires et les fichiers d'aperçu correspondants dans la galerie de pages maîtres. La liste suivante indique où placer les fichiers :
  
    
    

- **Galerie de pages maîtres** Regroupe les fichiers de page maître et les fichiers d'aperçu correspondants (fichiers .preview). Pour pouvoir être utilisée dans l'Assistant **Modifier l'apparence**, une page maître doit être associée à un fichier d'aperçu. Les fichiers JavaScript et d'autres éléments de conception peuvent également être chargés dans la galerie de pages maîtres.
    
    Pour accéder à la galerie de pages maîtres à partir de l'interface utilisateur de SharePoint, ouvrez la page **Paramètres du site**, puis, sous **Galeries du concepteur web**, sélectionnez **Pages maîtres**. Vous pouvez également accéder directement au site (http:// _SiteName__catalogs/masterpage/).
    
  
- **Galerie de thèmes** Regroupe les palettes de couleurs et les jeux de polices qui sont disponibles dans l'interface de thèmes. SharePoint explore le dossier **15** pour déterminer les palettes de couleurs et les jeux de polices disponibles.
    
    Pour accéder à la galerie de thèmes à partir de l'interface utilisateur de SharePoint, ouvrez la page **Paramètres du site**, puis, sous **Galeries du concepteur web**, sélectionnez **Thèmes**. Vous pouvez également accéder directement au site (http:// _SiteCollectionName_/_catalogs/theme/15/).
    
  
- **Bibliothèque de styles** Regroupe les fichiers CSS personnalisés que vous souhaitez utiliser dans l'interface de thèmes. Vous pouvez accéder directement à la bibliothèque de styles (remplacez _ SiteCollectionName_ et _language_ dans cette URL : http:// _SiteCollectionName_/Style Library/ _language_/Themable/).
    
> **REMARQUE**
> Placez les fichiers CSS personnalisés dans le dossier Themable de la bibliothèque de styles et pas dans le dossier Themable de la galerie de pages maîtres. Seuls les fichiers CSS stockés dans le dossier Themable de la bibliothèque de styles sont reconnus par le moteur de thèmes. 

> **REMARQUE**
> Si le contrôle de version est activé dans la galerie de pages maîtres et la galerie de thèmes, vous devez également publier les fichiers de conception pour qu'ils puissent être utilisés par le moteur de thèmes. 
  
    
    


## Déployer un thème à l'aide de l'interface utilisateur
<a name="section2"> </a>

Une présentation ou une conception composée correspond à la palette de couleurs, au jeu de polices, à l'image d'arrière-plan et à la page maître qui déterminent l'apparence du site. La liste Présentations composées regroupe les présentations composées disponibles dans la bibliothèque de présentations. Vous créez une conception en ajoutant un élément dans la liste Présentations composées et en spécifiant la page maître, la palette de couleurs, le jeu de polices et l'image d'arrière-plan à utiliser.
  
    
    

> **REMARQUE**
> Pour être disponible dans la bibliothèque de présentations, une page maître doit être associée à un fichier d'aperçu. 
  
    
    


### Pour ajouter une présentation composée


1. Sélectionnez l'icône **Paramètres**, puis choisissez **Paramètres du site**.
    
  
2. Dans **Galeries du concepteur web**, sélectionnez **Présentations composées**.
    
  
3. Dans la liste **Présentations composées**, sélectionnez **nouvel élément**.
    
  
4. Dans la zone de texte **Titre**, entrez le titre de la conception.
    
  
5. Dans la zone de texte **Nom**, entrez le nom de la conception. Ce nom apparaît dans la liste Présentations composées et dans la bibliothèque de présentations.
    
  
6. Dans la zone de texte **URL de page maître**, entrez l'URL de la page maître. Il peut s'agir d'une URL relative.
    
  
7. Dans la zone de texte **URL de thème**, entrez l'URL de la palette de couleurs (URL du fichier .spcolor). Il peut s'agir d'une URL relative.
    
  
8. Dans la zone de texte **URL d'image**, entrez l'URL de l'image d'arrière-plan. Cette option est facultative. Il peut s'agir d'une URL relative.
    
  
9. Dans la zone de texte **URL de jeu de polices**, entrez l'URL du jeu de polices (URL du fichier .spfont). Il peut s'agir d'une URL relative.
    
  
10. Dans la zone de texte **Ordre d'affichage**, entrez le numéro de l'ordre d'affichage. Cette option détermine l'endroit où la conception apparaît dans la bibliothèque de présentations.
    
  
11. Sélectionnez **Enregistrer**.
    
    > **REMARQUE**
      > Si une valeur de présentation composée est incorrecte, la présentation n'est pas ajoutée à la bibliothèque de présentations et aucun message n'est enregistré dans les fichiers journaux. Les raisons pour lesquelles une présentation composée peut ne pas être ajoutée sont les suivantes : un fichier est introuvable, un problème de formatage s'est produit dans l'un des fichiers ou SharePoint n'est pas en mesure d'accéder aux fichiers. 
Vous pouvez maintenant utiliser la bibliothèque de présentations pour appliquer une nouvelle conception à votre site. Pour plus d'informations, voir  [Choisir un thème pour votre site de publication](http://office.microsoft.com/fr-fr/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx) sur Office.com.
  
    
    

## Déployer un thème à l'aide de code
<a name="section3"> </a>

Vous pouvez déployer un thème en implémentant un récepteur de fonctionnalités.
  
    
    

### Pour déployer un thème à l'aide d'un récepteur de fonctionnalités


1. Créez une classe de récepteur de fonctionnalités qui hérite de la classe [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) .
    
  
2. Dans la méthode  [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) , créez un objet [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) qui utilise la palette de couleurs et le jeu de polices, puis appliquez le thème à votre site.
    
    L'exemple de code suivant montre comment déployer une palette de couleurs et un jeu de polices personnalisés sur un site.
    


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


> **REMARQUE**
> Le paramètre  _shareGenerated_ de la méthode **ApplyTo** indique si les fichiers de thème peuvent être communs à plusieurs sites d'une collection de sites. Généralement, il est défini sur **true** pour SharePoint Server et les sites SharePoint Online, et sur **false** pour les sites SharePoint Foundation. Le paramètre _shareGenerated_ doit être défini sur **true** si vous souhaitez que les fichiers de thème soient partagés. Pour plus d'informations, voir [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) .

Lorsqu'un utilisateur applique un thème dans l'Assistant **Modifier l'apparence**, ce dernier met également à jour le thème actuel dans la liste Présentations composées et la bibliothèque de présentations. Lorsque vous appliquez un thème par programmation, vous devez mettre manuellement à jour le thème actuel. L'exemple suivant montre comment mettre à jour le thème actuel.
    


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


## Ressources supplémentaires
<a name="bk_addresources"> </a>


-  [Développer la conception de site dans SharePoint 2013](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [Palettes de couleurs et polices dans SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [Comment créer un fichier d'aperçu de page maître dans SharePoint 2013](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [Blog de l'équipe SharePoint : Affichez votre style avec les thèmes SharePoint](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

