---
title: Comment créer des types de contenu externe pour SQL Server dans SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: 7fe2fbf0-546b-4a16-b02e-a0960bfb3842
---


# Comment créer des types de contenu externe pour SQL Server dans SharePoint 2013
Découvrez comment créer un type de contenu externe pour SQL Server dans SharePoint 2013.
La création d'un type de contenu externe est une tâche essentielle lorsque vous travaillez avec des données externes. Un type de contenu externe contient des informations importantes sur les connexions, l'accès, les méthodes de fonctionnement, les colonnes, les filtres et les autres métadonnées utilisées pour extraire les données de la source de données externes.
  
    
    


## Avant de commencer
<a name="section1"> </a>

L'utilisation de données externes nécessite plusieurs tâches prérequises pour permettre un accès sécurisé aux données. Les informations suivantes peuvent vous aider à planifier les étapes à suivre. En outre, si vous rencontrez des problèmes lors de l'utilisation de données externes, ces informations peuvent vous aider à les identifier. Pour accéder aux données externes, vous ou un administrateur devez procéder comme suit :
  
    
    
 **Préparer SQL Server** Un administrateur de base de données doit fournir des autorisations pour garantir que les bonnes personnes ont accès aux données et que celles-ci ne terminent pas entre de mauvaises mains. L'administrateur de base de données doit également créer un compte SQL Server disposant de l'autorisation db_owner. Il peut également créer des tableaux, des vues, des requêtes, des alias de colonne et d'autres éléments spécifiques pour limiter les résultats au strict minimum et pour améliorer les performances.
  
    
    
 **Configurer les services SharePoint** Un administrateur doit activer Business Connectivity Services (BCS) et le Service Banque d'informations sécurisée.
  
    
    
 **Configurer le service Banque d'informations sécurisé** Un administrateur doit déterminer le meilleur mode d'accès à la source de données externes, créer une application cible et définir les informations d'identification de l'application cible.
  
    
    
 **Configurer les services Business Data Connectivity** Un administrateur doit s'assurer que l'utilisateur qui crée le type de contenu externe dispose de l'autorisation pour le magasin de métadonnées de connectivité Business Data Connectivity (BDC) et que les utilisateurs appropriés ont accès au type de contenu externe sur lequel est basée la liste externe.
  
    
    
 **Vérifier qu'Office 2013 est prêt à l'emploi** Pour synchroniser des données externes avec les produits Office 2013, vous devez utiliser Windows 7 ou une version ultérieure. Assurez-vous également que l'option d'installation d'Office pour Business Connectivity Services (BCS) est activée (valeur par défaut). Cette option installe le Business Connectivity Services Client Runtime qui effectue les opérations suivantes : mise en cache et synchronisation des données externes, mappage des données métiers aux types de contenu externe, affichage du sélecteur d'éléments externes dans les produits Office et exécution des solutions personnalisées dans les produits Office. Vous devez également disposer de SQL Server Compact 4.0, .NET Framework 4 et WCF Data Services 5.0 pour OData V3 sur chaque ordinateur client (si nécessaire, vous êtes automatiquement invité à télécharger le logiciel).
  
    
    

## Définir les informations générales
<a name="section2"> </a>


1. Démarrez Microsoft SharePoint Designer 2013.
    
  
2. Cliquez sur **Ouvrir le site**, puis entrez le nom de site.
    
  
3. Dans le volet **Navigation**, sous **Objets du site**, sélectionnez **Types de contenu externe**.
    
    > **REMARQUE**
      > SharePoint Designer 2013 regroupe les types de contenu externe par espace de noms dans la première fenêtre du Concepteur de type de contenu externe. 
4. Pour ouvrir le Concepteur de type de contenu externe, dans le ruban, cliquez sur **Type de contenu externe**.
    
  
5. Sur la page **Nouveau type de contenu externe**, procédez comme suit :
    
  - En regard de **Nom**, cliquez sur **Nouveau type de contenu externe**, puis entrez un nom unique pour le type de contenu externe.
    
  
  - En regard de **Nom complet**, entrez un nom différent si vous souhaitez un nom complet plus descriptif.
    
  

## Définir les comportements généraux et les comportements d'Office
<a name="section3"> </a>


1. Dans la liste déroulante **Type d'élément Office**, sélectionnez l'une des options suivantes :
    
  - **Liste générique** Sélectionnez cette option pour tout type de liste.
    
  
  - **Rendez-vous, contact, tâche ou publication** Sélectionnez cette option si vous créez une liste qui se comporte comme un élément Contact, Tâche, Rendez-vous ou Publication d'Outlook. Le type d'élément Office que vous sélectionnez ici détermine le comportement d'Outlook que vous souhaitez associer au type de contenu externe. Par exemple, un type de contenu externe client se comporte comme un élément natif Contact dans Outlook.
    
  
2. Pour la case à cocher **Synchronisation hors connexion pour la liste externe**, vérifiez qu' **Activé** est sélectionné (valeur par défaut).
    
    > **REMARQUE**
      > Si vous désactivez cette option, la commande **Connexion SharePoint à Outlook** n'est pas disponible pour une liste externe.

> **REMARQUE**
> La fonctionnalité de batterie de serveurs et de site, **Synchronisation hors connexion pour les listes externes**, doit également être active. Cette fonctionnalité est activée par défaut au niveau de la batterie, mais pas au niveau du site. 
  
    
    


## Créer une connexion vers les données externes
<a name="section4"> </a>


1. Pour spécifier la base de données SQL Server pour le type de contenu externe, cliquez sur **Cliquez ici pour découvrir les sources de données externes et définir les opérations.**.
    
  
2. Cliquez sur **Ajouter une connexion**, sélectionnez **SQL Server** dans la boîte de dialogue **Sélection du type de source de données externe**, puis cliquez sur **OK**.
    
  
3. Dans la boîte de dialogue **Connexion SQL Server**, entrez le nom du serveur, le nom de la base de données, une description facultative, puis cliquez sur **OK**.
    
  
4. Pour choisir un mode d'authentification, sélectionnez l'une des opérations suivantes :
    
  - **Se connecter avec l'identité de l'utilisateur** Utilise le mode d'authentification directe.
    
  
  - **Se connecter avec l'identité Windows empruntée** Utilise le mode d'authentification WindowsCredentials.
    
  
  - **Se connecter avec l'identité personnalisée empruntée** Utilise le mode d'authentification RDBCredentials.
    
  
5. Dans la zone **ID d'application du service Banque d'informations sécurisé :**, entrez le nom d'ID application cible créé dans le Service Banque d'informations sécurisée.
    
  
6. Cliquez sur **OK**.
    
  
SharePoint Designer 2013 valide et teste les informations de connexion. Si vous recevez des messages, vous devez les résoudre avant de poursuivre.
  
    
    

## Sélectionner un tableau, une vue ou une routine
<a name="section5"> </a>


1. Dans l'Explorateur de source de données, développez la base de données pour afficher les tables, les vues et les routines qu'elle contient.
    
  
2. Sélectionnez un tableau, une vue ou une routine.
    
  

## Définir les opérations
<a name="section6"> </a>


1. Dans l'Explorateur de source de données, cliquez avec le bouton droit sur le tableau, la vue ou la routine, puis sélectionnez l'une des opérations suivantes :
    
  - **Créer toutes les opérations** Définit une opération de création d'élément, de suppression d'élément, de lecture d'élément, de lecture de liste et de mise à jour d'élément.
    
    > **REMARQUE**
      > L'option **Créer toutes les opérations** est disponible uniquement pour les tableaux et les vues. Les routines nécessitent des opérations spécifiques.
  - **Nouvelle opération de lecture d'élément** Définit une opération de lecture d'élément.
    
  
  - **Nouvelle opération de lecture de liste** Définit une opération de lecture de liste.
    
  
  - **Nouvelle opération de mise à jour** Définit une opération de mise à jour d'élément.
    
  
  - **Nouvelle opération de suppression** Définit une opération de suppression d'élément.
    
  
  - **Actualiser** Actualise la liste des tableaux, des vues et des routines dans l'Explorateur de source de données.
    
  
2. Cliquez sur **Suivant**.
    
  
 **Remarques**
  
    
    

- Sur les vues qui s'étendent sur plusieurs tableaux, assurez-vous que les opérations d'écriture sont prises en charge. Sinon, les actions **Créer toutes les opérations** ou **Nouvelle opération de mise à jour** risquent d'échouer.
    
  
- Définissez toujours au moins **Nouvelle opération de lecture d'élément** et **Nouvelle opération de lecture de liste** car les fonctionnalités SharePoint, tels que des listes externes, dépendent de ces opérations.
    
  
- Choisissez des opérations spécifiques, au lieu de **Créer toutes les opérations**, lorsque le tableau ou la vue ne prend pas en charge certaines opérations.
    
  

## Ajouter des colonnes
<a name="section7"> </a>


1. Pour spécifier les colonnes que vous souhaitez afficher à partir du tableau ou de la vue, cliquez sur **Suivant**.
    
  
2. Dans la boîte de dialogue **Configuration des paramètres**, par défaut toutes les colonnes (appelées **Éléments de source de données**) sont sélectionnés. Pour supprimer les colonnes inutiles, désactivez les cases à cocher correspondantes.
    
    > **REMARQUE**
      > Contrairement à une liste SharePoint native, vous ne pouvez pas modifier le nom de colonne d'une liste externe. Utilisez un alias de colonne SQL pour fournir un nom plus significatif ou un nom plus court. 
3. Pour sélectionner un champ d'identificateur, cliquez sur un champ et mettez-le en surbrillance (généralement un champ à valeur unique), puis sous **Propriétés**, cliquez sur **Mapper sur l'identificateur**.
    
  

> **IMPORTANTE**
> Pour empêcher la mise à jour de champs spécifiques, comme un champ d'ID ou de clé primaire, désactivez la case à cocher **Obligatoire**, mais cochez la case **En lecture seule**, qui est nécessaire pour récupérer des éléments afin que vous puissiez mettre à jour les autres champs.
  
    
    

  
    
    


> **CONSEIL**
> Lisez toujours attentivement les messages dans le volet **Erreurs et avertissements**. Ils fournissent des informations utiles pour confirmer vos actions ou résoudre les problèmes. Cliquez régulièrement sur le volet **Erreurs et avertissements** et assurez-vous qu'il ne contient pas d'erreurs ni d'avertissements.
  
    
    


## Mappage des champs Outlook
<a name="section8"> </a>

Si votre type de contenu externe est mappé à un type d'élément Outlook, vous devez mapper un ou plusieurs champs de votre type de contenu externe aux champs d'élément Outlook. Lorsque vous mappez un type de contenu externe, comme un client, à un type d'élément Outlook, tel qu'un contact, vous devez explicitement mapper les champs individuels dans le type de contenu externe, tels que le prénom du client, son nom, son adresse et son numéro de téléphone aux types d'élément Outlook correspondants, tel que les champs FirstName, LastName, BusinessAddress et BusinessPhone d'un contact.
  
    
    

- Pour chaque champ, procédez comme suit :
    
1. Cliquez sur le champ et mettez-le en surbrillance.
    
  
2. Sous **Propriétés**, en regard de **Propriété Office**, cliquez sur la flèche vers le bas, puis sélectionnez le champ correspondant. 
    
  

> **REMARQUE**
> Il est inutile de mapper tous les champs correspondants. Toutefois, les champs affichés dans le tableau suivant doivent être mappés. 
  
    
    


**Tableau : Type d'élément Outlook mappé au champ d'élément Outlook**


|**Type d'élément Outlook**|**Champ d'élément Outlook**|
|:-----|:-----|
|Contact  <br/> |Nom  <br/> |
|Tâche  <br/> |Objet  <br/> |
|Rendez-vous  <br/> |Début, fin et objet  <br/> |
|Publication  <br/> |Objet  <br/> |
   
Les champs non mappés, selon leur nombre, sont affichés en tant que propriétés étendues comme suit :
  
    
    

- **Adjacent** Ajouté à la zone de formulaire au bas d'une page par défaut d'un formulaire Outlook (deux à cinq champs).
    
  
- **Distinct** Ajouté en tant que nouvelle page à un formulaire Outlook (au moins six champs).
    
  

## Configurer le contrôle du sélecteur d'élément externe
<a name="section9"> </a>

Le contrôle du sélecteur d'élément externe permet aux utilisateurs de sélectionner un champ, tel qu'un champ d'ID ou un champ qui comporte des valeurs uniques, pour choisir un élément. Ce contrôle est disponible dans les produits SharePoint et Office 2013. Par exemple, les utilisateurs peuvent utiliser ce contrôle pour choisir un élément dans une liste externe de clients et Word 2013 autorise ce contrôle pour une utilisation avec les contrôles de contenu qui sont liés aux colonnes de données externes. Il est recommandé de sélectionner les colonnes spécifiques que vous souhaitez afficher dans le contrôle du sélecteur d'élément externe, car l'opération par défaut consiste à afficher toutes les colonnes, ce qui, dans la plupart des cas, n'est pas nécessaire.
  
    
    

1. Cliquez sur chaque champ que vous souhaitez afficher dans le sélecteur d'élément de contenu externe, mettez-le en surbrillance, puis sous **Propriétés**, cliquez sur la case à cocher **Afficher dans le sélecteur**.
    
  
2. Cliquez sur **Suivant**.
    
  

> **REMARQUE**
> Tous les filtres que vous définissez sont affichés dans le contrôle du sélecteur d'élément externe. Bien que vous ne puissiez pas supprimer de filtres spécifiques à partir du contrôle du sélecteur d'élément externe, vous pouvez définir un filtre par défaut en cliquant sur **Est la valeur par défaut** dans la boîte de dialogue **Configuration du filtre** lorsque vous créez ou modifiez le filtre.
  
    
    


## Définir les filtres
<a name="section10"> </a>

Si vous ne définissez pas de filtre, une liste externe renvoie toutes les données jusqu'à atteindre la limite Business Connectivity Services (BCS) (par défaut, 2 000 éléments) ou toutes les données définies dans le type de contenu externe, si ce nombre est inférieur à la limite actuelle. En outre, tout le traitement des résultats est effectué dans le produit SharePoint. Il est recommandé de définir au moins un filtre. En règle générale, il existe deux types de filtre que vous devez connaître :
  
    
    

- **Filtre de source de données** Lorsque vous créez un filtre de type de contenu externe qui est connu comme un filtre de source de données, l'opération de filtrage se produit dans la base de données SQL Server. Ceci est important lorsque vous travaillez avec une grande quantité de données car vous pouvez décharger le traitement des produits SharePoint vers la base de données externe et ainsi gagner en performances. Après avoir créé la liste externe, vous pouvez utiliser le filtre de source de données en créant un affichage qui spécifie les valeurs de filtre dans la section **Filtre de source de données** de la page de paramètres **Affichage de liste**.
    
  
- **Filtre SharePoint** Les utilisateurs peuvent toujours filtrer les données à l'aide d'un filtre SharePoint, avec le filtre d'en-tête de colonne ou à l'aide de la section **Filtres** de la page de paramètres **Affichage de liste**. Dans ce cas, l'opération de filtrage est effectuée dans le produit SharePoint, et non dans la base de données SQL Server.
    
  
Nous vous conseillons de créer un ensemble d'affichages de liste externe en fonction des filtres de source de données spécifiques qui garantissent que les données les plus volumineuses sont filtrées en premier dans la source de données externe, et que les utilisateurs peuvent filtrer davantage et affiner les résultats à l'aide de filtres SharePoint.
  
    
    
Vous pouvez créer différents types de filtres. Pour chaque filtre que vous créez, procédez comme suit :
  
    
    

1. Sous **Propriétés**, en regard d' **Élément de source de données**, sélectionnez un champ.
    
  
2. Sous **Propriétés**, en regard de **Filtre**, cliquez sur **Cliquer pour ajouter** pour afficher la boîte de dialogue **Configuration du filtre**.
    
  
3. Cliquez sur **Ajouter un paramètre de filtre**.
    
  
4. Dans la zone **Nouveau filtre**, entrez un nom de filtre.
    
  
5. Dans la zone **Type de filtre**, sélectionnez un type de filtre :
    
    
  
    
    
 **Comparaisons**
  
    
    

    
    Un filtre de comparaison limite les éléments qui sont renvoyés en fonction d'une condition (telles que État = « New Jersey ») et est converti en une clause SQL WHERE.
    
1. Dans la zone **Type de filtre**, activez la case **Comparaison**.
    
  
2. Dans la zone **Opérateur**, sélectionnez une opération.
    
  
3. Dans la zone **Champ de filtre**, assurez-vous que le champ à comparer est sélectionné.
    
  
4. Si vous voulez que les valeurs entrées par l'utilisateur respectent la casse, cliquez sur **Respecter la casse**.
    
  
5. Si vous souhaitez afficher la liste des correspondances possibles dans le contrôle du sélecteur d'élément externe lorsqu'il y a plusieurs éléments correspondants, sélectionnez **Permet de créer une liste de correspondance dans le sélecteur d'éléments externes**.
    
  
6. Cliquez sur **OK**.
    
  
7.  Sous **Propriétés**, en regard de **Valeur par défaut**, entrez la valeur initiale qui servira de filtre si l'utilisateur ne saisit pas de valeur. Si vous n'entrez aucune valeur, la liste externe n'affiche aucun élément.
    
  

    
  
    
    
 **Caractères génériques**
  
    
    

    
    Un filtre de caractères génériques limite les éléments qui sont renvoyés en fonction d'une valeur de chaîne saisie par l'utilisateur (par exemple, l'État contient « New ») et est converti en une clause de type SQL. Les caractères génériques SQL Server valides sont : * (astérisque) qui représente une correspondance avec n'importe quel nombre de caractères, et _ (caractère de soulignement), qui représente une correspondance avec un seul caractère.
  
    
    

    
1. Dans la zone **Type de filtre**, sélectionnez **Caractères génériques**.
    
  
2. Dans la zone **Champ de filtre**, sélectionnez un champ.
    
  
3. Si vous voulez que les valeurs entrées par l'utilisateur respectent la casse, cliquez sur **Respecter la casse**.
    
  
4. Si vous souhaitez afficher la liste des correspondances possibles dans le contrôle du sélecteur d'élément externe lorsqu'il y a plusieurs éléments correspondants, sélectionnez **Permet de créer une liste de correspondance dans le sélecteur d'éléments externes**.
    
  
5. Cliquez sur **OK**.
    
  
6.  Sous **Propriétés**, en regard de **Valeur par défaut**, entrez la valeur initiale qui servira de filtre si l'utilisateur ne saisit pas de valeur. Si vous n'entrez aucune valeur, la liste externe n'affiche aucun élément. Il est recommandé d'entrer un * (astérisque) par défaut.
    
  

    
  
    
    
 **Limite**
  
    
    

    
    Dans la plupart des cas, vous devez définir un filtre de limite pour les opérations de lecture et de lecture de liste. Si vous ne définissez aucune valeur de **Limite par défaut**, aucune donnée n'est extraite de la source de données externe. Vérifiez que la valeur par défaut que vous entrez pour le filtre de limite est inférieure à 2 000, car la limite par défaut Business Connectivity Services (BCS) est fixée à 2 000 éléments. Vous pouvez augmenter cette limite si nécessaire.
    
1. Dans la zone **Type de filtre**, activez la case **Limite**. 
    
  
2. Dans la zone **Nombre**, entrez un nombre.
    
  
3. Si vous souhaitez afficher la liste des correspondances possibles dans le contrôle du sélecteur d'élément externe lorsqu'il y a plusieurs éléments correspondants, sélectionnez **Permet de créer une liste de correspondance dans le sélecteur d'éléments externes**.
    
  
4. Sous **Propriétés**, en regard de **Valeur par défaut**, entrez la valeur initiale qui servira de filtre si l'utilisateur ne saisit pas de valeur. Si vous n'entrez aucune valeur, la liste externe n'affiche aucun élément.
    
  
5. Cliquez sur **OK**.
  
    
    

    
  

    > **REMARQUE**
      > L'administrateur de base de données SQL Server peut créer des tableaux, des vues, des index et des requêtes optimisées pour limiter les résultats au strict minimum et améliorer les performances. 

    
  
    
    
 **Numéro de la page**
  
    
    

    
Utilisez le numéro de la page du type de contenu externe pour remplacer la limite de page SharePoint définie dans la page **Affichage de liste** de la liste externe. Voici la différence :
    
  - Le numéro de la page du type de contenu externe traite d'abord les résultats dans la base de données SQL Server, puis renvoie et affiche uniquement le nombre de lignes déterminé par la valeur Taille de la page.
    
  
  - La limite de la page SharePoint renvoie toutes les lignes jusqu'à la taille de la valeur par défaut à partir de la base de données SQL Server et affiche le nombre de lignes déterminé par la valeur de limite de la page SharePoint dans la page de paramètres **Affichage de liste**.
    
  

    L'utilisation du numéro de la page du type de contenu externe est généralement plus efficace. En outre, la valeur **Ordre** permet de garantir un affichage précis des résultats renvoyés.
    
1. Dans la zone **Type de filtre**, sélectionnez **Numéro de la page**. 
    
  
2. Dans la zone **Taille de la page**, entrez un nombre.
    
  
3. Dans la zone **Ordre**, sélectionnez un sens de tri.
    
  
4. Cliquez sur **OK**. 
    
  
5. Sous **Propriétés**, en regard de **Valeur par défaut**, entrez la valeur initiale qui servira de filtre si l'utilisateur ne saisit pas de valeur. Si vous n'entrez aucune valeur, la liste externe n'affiche aucun élément.
  
    
    

    
  
6. Si vous souhaitez afficher mais ne pas modifier un champ, sélectionnez ce champ et mettez-le en surbrillance, puis sous **Propriétés**, sélectionnez **En lecture seule**.
    
  
7. Si vous voulez vous assurer qu'un champ a toujours une valeur lorsqu'il est créé ou modifié, cliquez sur ce champ et mettez-le en surbrillance, puis sous **Propriétés**, sélectionnez **Obligatoire**.
    
  
8. Pour fournir un nom descriptif dans le contrôle du sélecteur d'élément externe pour le nom d' **Élément de source de données**, qui est dérivé du nom de la colonne SQL Server, cliquez sur ce champ et mettez-le en surbrillance, puis sous **Propriétés**, dans la zone **Nom complet**, entrez un nom.
    
  
9. Pour définir une valeur par défaut pour le filtre (qui s'affiche également dans la section **Filtre de source de données** de la page de paramètres **Affichage de liste**), sous **Paramètres de filtre**, cliquez sur ce champ et mettez-le en surbrillance, puis dans la zone **Valeur par défaut**, entrez une valeur appropriée. Si vous n'entrez aucune valeur, lorsque ce filtre est utilisé pour la première fois, aucun élément ne s'affiche.
    
  

## Définir le champ Titre d'une liste externe
<a name="section11"> </a>


1. Dans le ruban, cliquez sur **Affichage de synthèse**.
    
  
2. Dans la section **Champs**, sous **Nom de champ**, sélectionnez un champ.
    
    > **IMPORTANTE**
      > En règle générale, il est recommandé de définir le champ Titre sur une valeur unique. Le champ Titre est utilisé pour afficher la liste ou des formulaires InfoPath. Une fois que le champ Titre est défini, vous ne pouvez plus le modifier. 
3. Dans le ruban, cliquez sur **Définir comme titre**.
    
  

## Terminer le type de contenu externe
<a name="section12"> </a>


- Dans la barre d'outils Accès rapide, cliquez sur **Enregistrer**. Cela stocke la définition du type de contenu externe dans le magasin de métadonnées Business Data Connectivity.
    
  

> **REMARQUE**
> Pour obtenir de meilleures performances, Business Data Connectivity met en cache tous les objets dans le magasin de métadonnées et met à jour les modifications apportées à l'aide d'un travail du minuteur qui s'exécute chaque minute. Cela peut prendre jusqu'à une minute pour que les modifications se propagent à tous les serveurs de la batterie de serveurs, mais les modifications sont immédiates sur le serveur sur lequel elles sont effectuées. 
  
    
    

Le type de contenu externe est désormais disponible pour être utilisé dans les produits SharePoint et Office 2013.
  
    
    

## Ressources supplémentaires
<a name="AR"> </a>


-  [Types de contenus externes dans SharePoint 2013](external-content-types-in-sharepoint-2013.md)
    
  
-  [Deploy a Business Connectivity Services on-premises solution in SharePoint 2013](http://msdn.microsoft.com/library/4692ebba-90eb-4d72-ac12-e90c4d4ee7ae.aspx)
    
  
-  [À l'aide de sources d'OData avec les Services Business Connectivity dans SharePoint 2013](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [Configure the Secure Store Service in SharePoint 2013](http://msdn.microsoft.com/library/29C0BC76-D835-401B-A2FB-ABB069E84125.aspx)
    
  

