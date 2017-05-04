---
title: Ce qui a changé dans SharePoint Designer 2013
ms.prod: SHAREPOINT
ms.assetid: f1cd30c9-9a73-428d-9151-f1813b43b020
---


# Ce qui a changé dans SharePoint Designer 2013
Découvrez les fonctionnalités qui sont déconseillées dans SharePoint Designer 2013 ou qui ont été supprimées. Les fonctionnalités déconseillées sont incluses dans la mise à jour SharePoint 2013 pour assurer la compatibilité avec les versions de produit précédentes, mais seront supprimées dans les versions ultérieures.
## Fonctionnalités SharePoint Designer 2013 abandonnées
<a name="WhatsChangedSharePointDesigner2013_DiscontinuedFeatures"> </a>

Les fonctionnalités suivantes sont déconseillées dans SharePoint Designer 2013 ou ont été supprimées.
  
    
    

### Plateforme de flux de travail SharePoint 2010
<a name="WhatsChangedSharePointDesigner2013_WorkflowPlatform"> </a>

 **Description du changement.** Certaines fonctionnalités de la plateforme de flux de travail SharePoint 2010 qui dépendent de Windows Workflow Foundation 3.0 sont déconseillées dans SharePoint 2013.
  
    
    
 **Motif du changement.** SharePoint 2013 introduit une nouvelle plateforme de flux de travail SharePoint 2013 qui repose sur Windows Workflow Foundation 4.0 et qui est intégrée à Workflow Manager 1.0.
  
    
    
 **Chemin de migration.** Dans SharePoint Designer 2013, vous pouvez créer un flux de travail SharePoint 2010 et utiliser toutes les fonctionnalités de la plateforme de flux de travail SharePoint 2010 en choisissant la plateforme de flux de travail SharePoint 2010.
  
    
    
Vous pouvez également intégrer les fonctionnalités de la plateforme de flux de travail SharePoint 2010 dans la nouvelle plateforme de flux de travail SharePoint 2013. Pour ce faire, créez un flux de travail SharePoint 2010 en choisissant la plateforme de flux de travail SharePoint 2010 ; créez un flux de travail SharePoint 2013 en choisissant la plateforme de flux de travail SharePoint 2013, puis utilisez les actions **Démarrer un flux de travail de liste** et **Démarrer un flux de travail du site** dans le flux de travail SharePoint 2013 pour appeler le flux de travail SharePoint 2010.
  
    
    
Les fonctionnalités suivantes sont disponibles uniquement sur la plateforme de flux de travail SharePoint 2010 :
  
    
    

- Actions :
    
  - Arrêter le flux de travail
    
  
  - Capturer une version de l'ensemble de documents
    
  
  - Envoyer l'ensemble de documents au référentiel
    
  
  - Définir le statut d'approbation du contenu pour l'ensemble de documents
    
  
  - Démarrer le processus d'approbation de l'ensemble de documents
    
  
  - Déclarer un enregistrement
    
  
  - Définir le statut d'approbation du contenu
    
  
  - Annuler la déclaration d'un enregistrement
    
  
  - Ajouter un élément de liste 
    
  
  - Hériter des autorisations parentes des éléments de liste
    
  
  - Supprimer les autorisations associées aux éléments de liste
    
  
  - Remplacer les autorisations associées aux éléments de liste
    
  
  - Rechercher le directeur d'un utilisateur
    
  
  - Assigner un formulaire à un groupe
    
  
  - Assigner quelque chose à faire
    
  
  - Collecter les données d'un utilisateur
    
  
  - Démarrer le processus d'approbation
    
  
  - Démarrer le processus de tâche personnalisée
    
  
  - Démarrer le processus de commentaires
    
  
  - Copier un élément de la liste (SharePoint Designer 2013 prend en charge uniquement l'action de copie de document.)
    
  
- Conditions :
    
  - Si le champ de l'élément actif est égal à la valeur
    
  
  - Vérifier les niveaux d'autorisation associés aux éléments de la liste
    
  
  - Vérifier les autorisations associées aux éléments de liste
    
  
- Étapes :
    
  - Étape d'emprunt d'identité :
    
  
- Sources de données :
    
  - Recherche des profils utilisateur
    
  
- Autres fonctionnalités :
    
  - Intégration de Visio
    
  
  - Colonne d'association
    
  
  - Association de type de contenu pour la réutilisation de flux de travail
    
  
  - Fonctionnalité « Exiger des autorisations de gestion de liste/du web » pour le flux de travail de site/de liste
    
  
  - Type de flux de travail globalement réutilisable
    
  
  - Option de visualisation de flux de travail
    
  

### Fonctionnalités de création de page SharePoint
<a name="WhatsChangedSharePointDesigner2013_PageDesignFeatures"> </a>

La fonctionnalité suivante n'est pas disponible dans SharePoint 2013.
  
    
    

#### Mode Création et mode Fractionné
<a name="WhatsChangedSharePointDesigner2013_DesignViewSplitView"> </a>

 **Description du changement.** SharePoint Designer 2010 propose trois modes d'affichage pour modifier les pages HTML et ASPX : mode Code, mode Création et mode Fractionné. Le mode Création et le mode Fractionné sont supprimés de SharePoint Designer 2013. La suppression du mode Création et du mode Fractionné a une incidence sur les fonctionnalités de SharePoint Designer 2013 utilisées pour modifier des composants WebPart et des pages maîtres. Si vous modifiez des pages dans SharePoint Designer 2013, vous devez utiliser le mode Code.
  
    
    
 **Motif du changement.** Par rapport aux versions actuelles d'Internet Explorer, le mode Création est une technologie plus ancienne qui ne prend pas en charge les nouvelles balises HTML5 et CSS.
  
    
    
 **Chemin de migration.** Si vous modifiez des pages en mode Code, vous pouvez appuyer sur **F12** pour afficher un aperçu de la page dans le navigateur. Vous pouvez également utiliser Visual Studio pour modifier des pages.
  
    
    
Si vous souhaitez concevoir ou personnaliser votre site de manière visuelle, et que vous souhaitez une expérience de modification de pages WYSIWYG (« ce que vous voyez est ce qui s'affichera »), vous pouvez utiliser n'importe quel éditeur HTML professionnel, par exemple Microsoft Expression Web. Vous pouvez ensuite importer vos fichiers HTML dans SharePoint Server 2013 en utilisant le nouveau gestionnaire de conception. Il s'agit d'une fonctionnalité incluse dans les sites de publication, comme le modèle de site de la collection de sites du portail de publication.
  
    
    

## Ressources supplémentaires
<a name="WhatsChangedSharePointDesigner2013_AdditionalResources"> </a>


-  [Référence rapide relative aux actions de flux de travail (plateforme de flux de travail SharePoint 2013)](workflow-actions-quick-reference-sharepoint-2013-workflow-platform.md)
    
  
-  [Changements entre SharePoint 2010 et SharePoint 2013](http://technet.microsoft.com/fr-fr/library/ff607742%28v=office.15%29.aspx)
    
  
-  [Nouveautés des flux de travail dans SharePoint Server 2013](http://technet.microsoft.com/fr-fr/library/jj219638%28v=office.15%29.aspx)
    
  

  
    
    

