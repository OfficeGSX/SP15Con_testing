---
title: Référence rapide relative aux actions de flux de travail (plateforme de flux de travail SharePoint 2013)
ms.prod: SHAREPOINT
ms.assetid: eb3434e5-bc75-4474-8873-4980062fd29c
---



# Référence rapide relative aux actions de flux de travail (plateforme de flux de travail SharePoint 2013)
Cette référence répertorie les actions de flux de travail qui sont prises en charge dans la version actuelle de SharePoint Designer 2013, en plus de celles qui ne sont pas disponibles.
## Actions de flux de travail dans SharePoint Designer 2013
<a name="bkm_WorkflowActions"> </a>

Vous trouverez ci-dessous une référence répertoriant les actions de flux de travail disponibles pour la plateforme de flux de travail SharePoint 2013. En plus de la plateforme de flux de travail SharePoint 2013, SharePoint Designer 2013 prend également en charge la plateforme SharePoint 2010. Pour connaître les actions de flux de travail de la plateforme 2010, voir  [Référence rapide relative aux actions de flux de travail (plateforme de flux de travail SharePoint 2010)](workflow-actions-quick-reference-sharepoint-2010-workflow-platform.md)
  
    
    

### Actions de base
<a name="bkm_CoreActions"> </a>

Les actions de base sont celles qui sont le plus couramment utilisées. Elles sont regroupées pour permettre un accès facilité.
  
    
    

**Tableau 1. Référence des actions de base**


|**Opération**|**Description**|
|:-----|:-----|
|Ajouter un commentaire  <br/> |Permet de laisser des commentaires dans le concepteur de flux de travail à des fins de référence. Ceci est particulièrement utile quand plusieurs utilisateurs collaborent sur le flux de travail.  <br/> |
|Ajouter l'heure à la date  <br/> |Ajoute un nombre représentant des minutes, heures, jours ou mois à une valeur de date (l'année n'est pas prise en charge) et stocke la valeur de sortie dans une variable. La valeur de date peut être la date du jour, une date spécifique ou le résultat d'une recherche. La valeur « Date en cours » renvoie à minuit UTC.  <br/> |
|Générer un dictionnaire  <br/> |Crée une variable de dictionnaire de paires clé/valeur.  <br/> <br/> **REMARQUE**<br/> Le dictionnaire utilise la notation JSON pour stocker les données.           Pour plus d'informations sur la variable de dictionnaire, voir  [Présentation des actions de dictionnaire dans SharePoint Designer 2013](understanding-dictionary-actions-in-sharepoint-designer-2013.md).  <br/> |
|Appeler le service web HTTP  <br/> |Fonctionne comme un appel de méthode vers un service web HTTP et renvoie des données au format JSON. L'authentification de base est prise en charge par le biais de l'élément RequestHeader.  <br/> Pour plus d'informations sur la variable de dictionnaire, voir  [Présentation des actions de dictionnaire dans SharePoint Designer 2013](understanding-dictionary-actions-in-sharepoint-designer-2013.md).  <br/> |
|Compter le nombre d'éléments dans un dictionnaire  <br/> |Renvoie le nombre d'éléments présents dans le dictionnaire spécifié.  <br/> |
|Effectuer un calcul  <br/> |Effectue un calcul arithmétique et stocke la valeur de sortie dans une variable.  <br/> <br/> **REMARQUE**<br/> Dans SharePoint 2013, cette action ne prend en charge que le type numérique **Double**. Les entiers ne sont pas pris en charge. L'utilisation de l'opérateur « + » (concaténation) pour les chaînes n'est pas pris en charge.           |
|Obtenir un élément à partir d'un dictionnaire  <br/> |Renvoie un élément spécifique à partir d'une variable de dictionnaire.  <br/> |
|Consigner dans l'historique  <br/> |Écrit un message dans la liste d'historique des flux de travail à partir d'une liste d'éléments de messages prédéfinis.  <br/> |
|Mettre en pause pour une certaine durée  <br/> |Suspend le flux de travail pour un intervalle spécifié en jours, heures et minutes.  <br/> |
|Mettre en pause jusqu'à une date donnée  <br/> |Suspend le flux de travail jusqu'à une date et une heure spécifiées.  <br/> |
|Envoyer un message électronique  <br/> |Envoie automatiquement un message électronique qui contient un message prédéterminé à un utilisateur ou un groupe lorsqu'un événement de flux de travail spécifié se produit.  <br/> <br/> **IMPORTANTE**<br/> Si le site n'est pas ajouté à la liste des sites de confiance, les messages électroniques sont routés vers le dossier de courrier indésirable d'Outlook.           |
|Définir la partie heure du champ Date/Heure  <br/> |Crée un horodatage et stocke la valeur de sortie dans une variable. Vous pouvez définir l'heure en heures et en minutes, ainsi qu'ajouter la date du jour, une date spécifique ou le résultat d'une recherche.  <br/> |
|Définir l'état du flux de travail  <br/> |Définit l'état du flux de travail.  <br/> |
|Définir la variable de flux de travail  <br/> |Affecte une valeur à une variable de flux de travail. Vous pouvez également utiliser cette action pour que le flux de travail affecte des données à une variable.  <br/> |
|Passer à l'étape suivante  <br/> |Spécifie l'étape suivante à laquelle le contrôle de flux doit être transmis.  <br/> |
   

### Actions de coordination
<a name="bkm_CoreActions"> </a>

Les actions de coordination permettent d'appeler un flux de travail basé sur la plateforme de flux de travail SharePoint 2010. Pour plus d'informations sur les actions de coordination, voir  [Présentation des actions de Coordination dans SharePoint Designer 2013](understanding-coordination-actions-in-sharepoint-designer-2013.md).
  
    
    

**Tableau 2. Référence des actions de coordination**


|**Opération**|**Description**|
|:-----|:-----|
|Démarrer un flux de travail de liste  <br/> |Démarre un flux de travail de liste basé sur la plateforme de flux de travail SharePoint 2010.  <br/> <br/> **REMARQUE**<br/>  Le démarrage d'un flux de travail de liste présente les problèmes suivants :<br/>  Le champ de type « Affectation » ne peut pas être utilisé comme un paramètre lorsque le flux de travail 2010 renferme une action TaskProcess.<br/>  Lorsque plusieurs appels sont effectués sur le même flux de travail 2010, des sources de données multiples sont générées dans la fonctionnalité de recherche de flux de travail 2013. Ces sources de données sont toutes identiques.<br/>  Les noms des variables dans la version 2013 ne peuvent pas contenir de caractères spéciaux tels que « ? » et « # ». Si un flux de travail 2010 contient des caractères spéciaux, il est converti en code hexadécimal dans le flux de travail 2013.          |
|Démarrer un flux de travail de site  <br/> |Démarre un flux de travail de site basé sur la plateforme de flux de travail SharePoint 2010.  <br/> <br/> **REMARQUE**<br/>  Le démarrage d'un flux de travail de site présente les problèmes suivants :<br/>  Le champ de type « Affectation » ne peut pas être utilisé comme un paramètre lorsque le flux de travail 2010 renferme une action TaskProcess.<br/>  Lorsque plusieurs appels sont effectués sur le même flux de travail 2010, des sources de données multiples sont générées dans la fonctionnalité de recherche de flux de travail 2013. Ces sources de données sont toutes identiques.<br/>  Les noms des variables dans la version 2013 ne peuvent pas contenir de caractères spéciaux tels que « ? » et « # ». Si un flux de travail 2010 contient des caractères spéciaux, il est converti en code hexadécimal dans le flux de travail 2013.          |
   

### Actions de liste
<a name="bkm_ListActions"> </a>

Les actions de liste regroupent les actions utilisées pour manipuler les listes et les éléments de liste.
  
    
    

**Tableau 3. Référence des actions de liste**


|**Opération**|**Description**|
|:-----|:-----|
|Archiver l'élément  <br/> |Archive un élément qui est extrait. Vous pouvez archiver des éléments uniquement à partir d'une bibliothèque de documents.  <br/> <br/> **ATTENTION**<br/> Le flux de travail se bloque si vous essayez d'archiver un élément qui n'est pas extrait.           |
|Extraire l'élément  <br/> |Extrait un élément. Le flux de travail vérifie si l'élément est archivé avant d'extraire un document. Vous pouvez extraire des éléments uniquement à partir d'une bibliothèque de votre site.  <br/> <br/> **ATTENTION**<br/> Le flux de travail se bloque si vous essayez d'extraire un élément qui n'est pas archivé.           |
|Copier le document  <br/> |Copie un document à partir de la liste actuelle vers une autre liste de la bibliothèque de documents.  <br/> |
|Créer un élément dans la liste  <br/> |Crée un élément de liste dans la liste que vous spécifiez. Vous pouvez fournir les champs et les valeurs dans le nouvel élément. Vous pouvez utiliser cette action pour créer un élément avec des informations spécifiques.  <br/> |
|Supprimer l'élément  <br/> |Supprime un élément.  <br/> <br/> **REMARQUE**<br/> Cette action est effectuée sur l'ordinateur qui exécute le moteur de flux de travail du Gestionnaire de workflow et génère une exception **System.InvalidOperationException**. Il n'existe aucune solution de contournement.           |
|Annuler l'extraction de l'élément  <br/> |Annule les modifications et archive à nouveau l'élément si celui-ci a été extrait et que des modifications y ont été apportées.  <br/> <br/> **ATTENTION**<br/> Le flux de travail se bloque si vous essayez d'archiver un élément qui n'est pas extrait.           |
|Définir le champ dans l'élément actif  <br/> |Affecte la valeur spécifiée à un champ donné dans l'élément actuel.  <br/> <br/> **REMARQUE**<br/> Si vous avez besoin de suspendre le flux de travail jusqu'à ce que la valeur du champ ait changé, utilisez l'action **Attendre l'événement dans l'élément de liste** à la place de cette action.          |
|Traduire le document  <br/> |Traduit un document dans une langue spécifique.  <br/> <br/> **REMARQUE**<br/> Nécessite une application de service de traduction automatique préconfigurée.           |
|Mettre à jour l'élément de la liste  <br/> |Met à jour un élément de liste. Vous pouvez spécifier les champs et les nouvelles valeurs que vous souhaitez leur attribuer.  <br/> |
|Attendre l'événement dans l'élément de liste  <br/> |[Version améliorée de l'action Office 2010.] Suspend l'instance de flux de travail en cours pour attendre l'événement d'élément de liste spécifié. Cette action fonctionne pour deux types d'événement : **ItemUpdated** et **ItemAdded**.  <br/> |
|Attendre la modification du champ dans l'élément actif  <br/> |Attend qu'un champ de l'élément actuel soit égal à une valeur spécifique.  <br/> |
   

### Actions de projet
<a name="bkm_ProjectActions"> </a>

Les actions de projet prennent en charge l'intégration de Microsoft Project. Elles permettent de créer des flux de travail basés sur un projet. Toutes les actions de projet sont nouvelles dans SharePoint Designer 2013.
  
    
    

**Tableau 4. Référence des actions de projet**


|**Opération**|**Description**|
|:-----|:-----|
|Créer un projet à partir de l'élément actuel  <br/> |Prend l'élément actuel et crée un projet dans le site PWA de batterie de serveurs SharePoint.  <br/> |
|Définir le champ de projet  <br/> |Définit une valeur pour un champ spécifique dans Project Server.  <br/> <br/> **REMARQUE**<br/> Cette action nécessite d'archiver d'abord le projet. Si le projet n'est pas archivé, le flux de travail est arrêté et les utilisateurs ne peuvent pas ouvrir ce projet dans Project Web App.           |
|Définir l'état de l'étape du projet  <br/> |Définit l'état de l'étape du projet.  <br/> <br/> **REMARQUE**<br/> Une exception se produit lorsqu'un projet est extrait.           |
|Définir le champ d'état dans la liste d'idées  <br/> |Met à jour l'état de l'élément de liste d'origine qui est associé au projet actuel.  <br/> |
|Attendre un événement de projet  <br/> |Attend un événement de projet spécifique.  <br/> |
   

### Actions de tâche
<a name="bkm_TaskActions"> </a>

Les actions de tâche permettent d'appeler un flux de travail basé sur la plateforme SharePoint 2010 à partir d'un flux de travail basé sur la plateforme SharePoint 2013.
  
    
    

**Tableau 5. Référence des actions de tâche**


|**Opération**|**Description**|
|:-----|:-----|
|Affecter une tâche  <br/> |Affecte une tâche de flux de travail à un utilisateur et définit une échéance pour la fin de cette tâche.  <br/> |
|Démarrer un processus de tâche  <br/> |Crée des tâches pour plusieurs utilisateurs et permet de les prendre par le biais d'un processus personnalisé.  <br/> |
   

### Actions utilitaires
<a name="bkm_UtilityActions"> </a>

Les actions utilitaires sont des actions qui manipulent des chaînes ou trouvent l'intervalle entre des dates.
  
    
    

**Tableau 6. Référence des actions utilitaires**


|**Opération**|**Description**|
|:-----|:-----|
|Extraire la sous-chaîne de la fin de la chaîne  <br/> |Copie un nombre spécifié de caractères à partir de la fin d'une chaîne et stocke la sortie dans une variable.  <br/> |
|Extraire la sous-chaîne de l'index de la chaîne  <br/> |Copie une sous-chaîne à partir d'un index spécifié dans la chaîne et place la valeur dans une variable.  <br/> <br/> **REMARQUE**<br/> Notez que Microsoft SharePoint Designer 2013 utilise un index de base zéro, tandis que les valeurs de SharePoint Designer 2010 sont indexées à partir de 1.           |
|Extraire la sous-chaîne depuis le début de la chaîne  <br/> |Copie un nombre spécifié de caractères à partir du début d'une chaîne et stocke la sortie dans une variable.  <br/> |
|Extraire la sous-chaîne de la chaîne depuis l'index avec la longueur  <br/> |Copie une sous-chaîne comprenant un nombre spécifié de caractères, à partir d'un index spécifié dans la chaîne, et place la valeur dans une variable.  <br/> <br/> **REMARQUE**<br/> Notez que Microsoft SharePoint Designer 2013 utilise un index de base zéro, tandis que les valeurs de SharePoint Designer 2010 sont indexées à partir de 1.           |
|Rechercher l'intervalle entre les dates  <br/> |Calcule l'intervalle en minutes, heures ou jours entre deux dates et stocke la sortie dans une variable.  <br/> |
|Découper la chaîne  <br/> |Supprime les espaces en début et en fin de chaîne.  <br/> |
|Rechercher la sous-chaîne dans la chaîne  <br/> |Recherche une sous-chaîne spécifique dans une chaîne et renvoie l'index de la position de départ de la sous-chaîne.  <br/> |
|Remplacer la sous-chaîne dans la chaîne  <br/> |Remplace une sous-chaîne spécifique par une autre sous-chaîne.  <br/> |
|Découper la chaîne  <br/> |Supprime les espaces en début et en fin de chaîne.  <br/> |
   

## Actions de flux de travail déconseillées dans SharePoint 2013
<a name="bkm_NotAvailable"> </a>

Pour obtenir la liste des actions de SharePoint 2010 qui sont déconseillées et qui n'apparaissent pas dans SharePoint 2013, voir  [Actions de flux de travail disponibles via la passerelle interopérabilité de base de flux de travail](workflow-actions-available-using-the-workflow-interop-bridge.md).
  
    
    

## Ressources supplémentaires
<a name="bkm_addlresource"> </a>


-  [Référence d'actions et les activités de flux de travail pour SharePoint 2013](workflow-actions-and-activities-reference-for-sharepoint-2013.md)
    
  
-  [Notions de base sur les flux de travail SharePoint 2013](sharepoint-2013-workflow-fundamentals.md)
    
  
-  [Mise en route avec les flux de travail dans SharePoint 2013](get-started-with-workflows-in-sharepoint-2013.md)
    
  
-  [Développement de flux de travail dans SharePoint Designer et Visio](workflow-development-in-sharepoint-designer-and-visio.md)
    
  
