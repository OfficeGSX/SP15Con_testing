---
title: Développer des flux de travail SharePoint 2013 à l'aide de Visual Studio
ms.prod: SHAREPOINT
ms.assetid: 28f5d3b1-6fe8-4b1f-8c4e-b11105fe6f46
---



# Développer des flux de travail SharePoint 2013 à l'aide de Visual Studio
SharePoint 2013 prend en charge deux principaux environnements de développement pour la création de flux de travail, à savoir SharePoint Designer et Visual Studio. Cet article les résume et aborde les avantages et les inconvénients de chacun.
## Notions de base concernant la création de flux de travail SharePoint
<a name="bkm_AuthoringBasics"> </a>


> **REMARQUE**
> Pour obtenir des instructions sur l'installation et la configuration de Microsoft SharePoint Server 2013 et du serveur Workflow Manager Client 1.0, voir  [Installer et configurer le Gestionnaire de workflow SharePoint 2013](set-up-and-configure-sharepoint-2013-workflow-manager.md). 
  
    
    

Comme pour les versions précédentes, Microsoft SharePoint 2013 fournit deux principaux environnements de développement pour la création de flux de travail, à savoir Microsoft SharePoint Designer et Microsoft Visual Studio. Cependant, contrairement aux versions précédentes, l'utilisation de Visual Studio ne fournit plus de stratégie de création basée sur du code. Au lieu de cela, SharePoint Designer et Visual Studio fournissent un environnement de création entièrement déclaratif et sans code, indépendamment de l'outil de développement que vous sélectionnez.
  
    
    

> **REMARQUE**
> En complément de la création de flux de travail dans SharePoint Designer, vous pouvez également utiliser Microsoft Visio 2013 pour structurer votre logique de flux de travail en utilisant des formes Visio 2013, puis importer votre logique dans SharePoint Designer 2013. Pour plus d'informations sur l'utilisation de Visio 2013 pour la création de votre logique de flux de travail, voir  [Développement de flux de travail dans SharePoint Designer et Visio](workflow-development-in-sharepoint-designer-and-visio.md). 
  
    
    


### Flux de travail déclaratifs

Commençons par clarifier le sens de « flux déclaratifs ». Ce terme signifie qu'au lieu d'être créé avec du code et d'être ensuite compilé dans des assemblys managés, le flux de travail est décrit (littéralement) avec du code XAML, puis exécuté de façon interprétative au moment de l'exécution.
  
    
    
Le code XAML est dérivé (ou déduit) des blocs de construction de flux de travail que vous manipulez dans le Workflow Designer (si vous utilisez Visual Studio) ou dans l'aire de conception de flux de travail SharePoint Designer (ou Visio, mais nous y reviendrons plus tard). Les blocs de construction sont eux-mêmes les objets de conception de flux de travail visuels dans la boîte à outils du concepteur : les étapes, les conditions, les actions, les événements, et ainsi de suite. L'ensemble d'outils dans les boîtes à outils respectives (Visual Studio ou SharePoint Designer) diffère quelque peu, mais le concept de flux de travail déclaratif reste le même.
  
    
    

## Arbre de décision : SharePoint Designer ou Visual Studio
<a name="bkm_DecisionTree"> </a>

L'un des principaux avantages de l'infrastructure de flux de travail dans SharePoint 2013 est la simplicité d'utilisation de l'environnement sans code de SharePoint Designer pour créer des flux de travail riches et puissants. En outre, un environnement de création déclaratif tel que Visual Studio offre un haut degré de flexibilité et de personnalisation.
  
    
    
Ces deux environnements de création de flux de travail (SharePoint Designer et Visual Studio) offrent des avantages et des inconvénients spécifiques. Cette section vous aide à déterminer l'environnement de création qui convient le mieux à vos besoins en matière de développement de flux de travail.
  
    
    

### Utilisation de SharePoint Designer


- **Utilisateurs cibles :** travailleurs de l'information, analystes d'entreprise et développeurs SharePoint.
    
  
- **Niveau de difficulté :** connaissance de SharePoint Designer, y compris les composants de flux de travail de base, tels que les étapes, les portails, les actions, les conditions et les boucles.
    
  
Avec SharePoint Designer, les utilisateurs peuvent créer un flux de travail qui est attaché à une liste, une bibliothèque ou un site en utilisant un concepteur texte sans code. Autrement, ils peuvent utiliser le nouvel environnement de conception visuel dans lequel les éléments graphiques sont disposés sur une aire de conception pour représenter le flux logique d'un processus d'entreprise. SharePoint Designer permet aux travailleurs non techniques de développer rapidement des flux de travail.
  
    
    

### Utilisation de Visual Studio


- **Utilisateurs cibles :** développeurs de logiciels intermédiaires ou avancés.
    
  
- **Niveau de difficulté :** connaissance de Visual Studio, y compris les concepts de développement de logiciels, tels que les récepteurs d'événements, l'empaquetage et le déploiement, ainsi que la sécurité.
    
  
La création de flux de travail dans Visual Studio fournit la flexibilité nécessaire pour créer des flux de travail qui prennent en charge quasiment tous les processus d'entreprise, quelle que soit leur complexité, et permet le débogage et la réutilisation des définitions de flux de travail. Le plus important est peut-être que Visual Studio permet aux développeurs d'inclure des flux de travail SharePoint dans le cadre d'une solution SharePoint plus large ou d'une Complément SharePoint.
  
    
    
Visual Studio permet aux développeurs de créer des actions personnalisées pour une utilisation dans SharePoint Designer et fournit les moyens d'exécuter une logique personnalisée. Avec Visual Studio, les développeurs peuvent également créer des modèles de flux de travail qui peuvent être déployés sur plusieurs sites.
  
    
    

## Comparaison de SharePoint Designer et de Visual Studio
<a name="bkm_Comparing"> </a>

Le tableau suivant présente une comparaison côte à côte des fonctionnalités proposées par SharePoint Designer et Visual Studio et des conditions requises à leur utilisation pour la création de flux de travail SharePoint.
  
    
    

**Tableau 1. Comparaison des outils de création de flux de travail**


|**Fonctionnalité/condition requise**|**SharePoint Designer**|**Visual Studio**|
|:-----|:-----|:-----|
|Permet le développement rapide de flux de travail  <br/> |Oui  <br/> |Oui  <br/> |
|Permet la réutilisation des flux de travail  <br/> |Un flux de travail peut être utilisé uniquement par la liste ou la bibliothèque sur laquelle il a été développé. Toutefois, SharePoint Designer fournit des flux de travail réutilisables qui peuvent être utilisés plusieurs fois dans le même site.  <br/> |Un flux de travail peut être écrit comme modèle pour pouvoir, après avoir été déployé, être réutilisé et associé à une liste ou une bibliothèque.  <br/> |
|Permet d'inclure un flux de travail dans le cadre d'une solution SharePoint ou d'une Complément SharePoint  <br/> |Non  <br/> |Oui  <br/> |
|Permet de créer des actions personnalisées  <br/> |Non, mais SharePoint Designer peut utiliser et implémenter des actions personnalisées créées et déployées à l'aide de Visual Studio.  <br/> |Oui, mais sachez que, dans Visual Studio, les activités sous-jacentes sont utilisées, pas leurs actions correspondantes.  <br/> |
|Permet d'écrire du code personnalisé  <br/> |Non  <br/> |Non  <br/> <br/> **REMARQUE**<br/> Ceci a été modifié par rapport aux versions précédentes. Dans SharePoint 2013, les flux de travail sont uniquement déclaratifs et le développement de flux de travail dans Visual Studio est basé sur l'utilisation de l'aire de conception visuelle.           |
|Peut utiliser Visio Professionnal pour créer une logique de flux de travail  <br/> |Oui  <br/> |Non  <br/> |
|Déploiement  <br/> |Les flux de travail sont déployés automatiquement sur la liste, la bibliothèque ou le site sur lesquels ils ont été créés.  <br/> |Crée un fichier de package de solution (.wsp) SharePoint et déploie le package de solution sur le site (SPWeb).  <br/> |
|Publication en un clic disponible pour les flux de travail  <br/> |Oui  <br/> |Oui  <br/> |
|Les flux de travail peuvent être packagés et déployés vers un serveur distant  <br/> |Oui  <br/> |Oui  <br/> |
|Débogage  <br/> |Les flux de travail ne peuvent pas être débogués.  <br/> |Les flux de travail peuvent être débogués à l'aide de Visual Studio.  <br/> |
|Peut utiliser uniquement les actions approuvées par l'administrateur du site  <br/> |Oui  <br/> |Oui  <br/> > **REMARQUE**> Ceci a été modifié par rapport aux versions précédentes. Auparavant, les flux de travail et les actions qui étaient créés à l'aide de Visual Studio étaient basés sur du code et déployés sur toute l'étendue de la batterie ; l'approbation de l'administrateur n'était donc pas nécessaire.           |
   

## Développement de flux de travail à l'aide de Visual Studio
<a name="bkm_Developing"> </a>

Contrairement aux versions précédentes, les flux de travail dans SharePoint 2013 sont entièrement déclaratifs. Désormais intégré à Windows Workflow Foundation 4, Visual Studio fournit une aire de conception de flux de travail visuelle qui vous permet de créer des flux de travail personnalisés, des modèles de flux de travail, des formulaires et des activités de flux de travail personnalisées entièrement dans l'environnement de conception. Votre flux de travail est ensuite packagé et déployé en tant que fonctionnalité SharePoint. Pour plus d'informations sur l'empaquetage des fonctionnalités, voir  [Développement de flux de travail dans Visual Studio](http://msdn.microsoft.com/fr-fr/library/ms461324.aspx).
  
    
    
Peut-être que le changement le plus important pour les développeurs Visual Studio est que les flux de travail personnalisés ne sont plus compilés et déployés en tant qu'assemblys .NET Framework. En outre, SharePoint 2013 n'utilise plus de formulaires Microsoft InfoPath ; au lieu de cela, la génération de formulaires repose sur les formulaires Microsoft ASP.NET. 
  
    
    
Enfin, les modèles de projet de flux de travail Visual Studio ont changé. Alors qu'auparavant des modèles étaient fournis pour la machine à états et les flux de travail séquentiels, ces distinctions ne sont plus significatives. Au lieu de cela, les modèles de projet Visual Studio sont disponibles dans le build Visual Studio fourni sur votre machine virtuelle (VM).
  
    
    

## Activation du débogage de flux de travail sur site
<a name="bkm_Enabling"> </a>

Pour déboguer des flux de travail sur site dans Visual Studio, vous devez autoriser temporairement Workflow Manager Tools à accéder à votre système à travers le pare-feu.
  
    
    

1. Dans le **Panneau de configuration**, choisissez **Système et sécurité**, puis **Pare-feu Windows**.
    
  
2. Dans la liste **Page d'accueil du panneau de configuration**, choisissez le lien **Paramètres avancés**.
    
  
3. Dans le volet gauche du pare-feu Windows, choisissez **Règles de trafic entrant**.
    
  
4. Dans la liste **Règles de trafic entrant**, choisissez **Workflow Manager outils 1.0 pour Visual Studio 2012 - Test Service hôte**.
    
  
5. Dans la liste **Actions** choisissez **Activer la règle**.
    
  
6. Sur la page des propriétés de votre projet SharePoint, sélectionnez l'onglet **SharePoint**, puis cochez la case **Activer le débogage de flux de travail**.
    
  

## Débogage de flux de travail SharePoint Online à l'aide de Visual Studio
<a name="bkm_Debug"> </a>

Pour déboguer des flux de travail SharePoint Online dans Visual Studio, procédez comme suit :
  
    
    

1. Si vous êtes protégé par un pare-feu, vous devrez peut-être installer un client proxy (tel que le client  [Forefront Threat Management Gateway (TMG)](http://www.microsoft.com/fr-fr/download/details.aspx?displaylang=en&amp;id=10504)), en fonction de la topologie du réseau de votre entreprise.
    
  
2. Si vous ne l'avez pas encore fait, créez un compte Microsoft Azure, puis connectez-vous.
    
    Pour plus d'informations sur la création d'un compte Microsoft Azure, voir  [Microsoft Azure](http://www.windowsazure.com).
    
  
3. Créez un espace de noms Microsoft Azure Service Bus pour pouvoir déboguer les flux de travail distants. Vous pouvez le faire sur le  [portail de gestion Microsoft Azure](http://manage.windowsazure.com).
    
    Pour plus d'informations sur Microsoft Azure Service Bus, voir  [Gestion des espaces de noms Service Bus](http://msdn.microsoft.com/fr-fr/library/windowsazure/hh690928.aspx).
    
    > **REMARQUE**
      > Le débogage des flux de travail SharePoint Online a recours au composant Service de relais de Microsoft Azure Service Bus. Vous devrez donc payer pour utiliser Service Bus. Voir  [Forum aux questions sur la tarification de Service Bus](http://msdn.microsoft.com/library/hh667438.aspx). Vous disposez d'un accès gratuit à Microsoft Azure pour chaque mois d'abonnement à Visual Studio Professional avec MSDN, Visual Studio Premium avec MSDN ou Visual Studio Ultimate avec MSDN. Cet accès vous permet d'utiliser Service Bus Relay pendant 1 500 ou 3 000 heures selon la nature de votre abonnement MSDN.  [Profitez gratuitement des avantages des services Microsoft Azure tous les mois](http://www.windowsazure.com/fr-fr/pricing/member-offers/msdn-benefits/). 
4. Dans Microsoft Azure, sélectionnez votre espace de noms de service, cliquez sur le lien **Clé d'accès**, puis copiez le texte qui se trouve dans la zone **Chaîne de connexion**.
    
  
5. Sur la page des propriétés de votre projet d'Complément SharePoint, sélectionnez l'onglet **SharePoint**, puis cochez la case **Activer le débogage via le flux de travail**.
    
    Vous devez activer cette fonctionnalité pour déboguer les flux de travail SharePoint Online. Cette propriété s'applique à tous vos projets SharePoint dans Visual Studio. Visual Studio désactive automatiquement le débogage des flux de travail si vous distribuez votre application sur Office Store.
    
  
6. Cochez la case **Activer le débogage via Windows Azure Service Bus**. Ensuite, dans la zone **Chaîne de connexion de Bus des services Microsoft Azure**, collez la chaîne de connexion que vous venez de copier.
    
  
Après avoir activé le débogage de flux de travail et fourni une chaîne de connexion valide pour Microsoft Azure Service Bus, vous pouvez déboguer les flux de travail SharePoint Online.
  
    
    

> **REMARQUE**
> Si vous n'avez pas désactivé le débogage des flux de travail et que vous ne souhaitez pas recevoir de notification à chaque fois que votre projet contient un flux de travail, désélectionnez la case **M'avertir si le débogage de Bus des services Microsoft Azure n'est pas configuré**. 
  
    
    


## Ressources supplémentaires
<a name="workflowbk_addres"> </a>

Une grande partie du développement des flux de travail SharePoint reste identique pour le développeur Visual Studio. Les principales sections de la documentation de SharePoint 2010 restent pertinentes :
  
    
    

- Pour plus d'informations sur l'utilisation de Visual StudioWorkflow Designer, voir  [Vue d'ensemble de Visual Studio Designer pour Windows Workflow Foundation](http://msdn.microsoft.com/fr-fr/library/ms441543.aspx).
    
  
- Pour plus d'informations sur la création de formulaires à l'aide de Microsoft ASP.NET, voir  [Vue d'ensemble des formulaires de flux de travail](http://msdn.microsoft.com/fr-fr/library/ms457061.aspx).
    
  
- Pour plus d'informations sur l'empaquetage des fonctionnalités, voir  [Développement de flux de travail dans Visual Studio](http://msdn.microsoft.com/fr-fr/library/ms461324.aspx).
    
  
