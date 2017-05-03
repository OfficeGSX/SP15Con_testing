---
title: Procédure  Convertir un fichier HTML en page maître dans SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: a76ab289-3256-45de-ac63-d5112a74e3c7
---


# Procédure : Convertir un fichier HTML en page maître dans SharePoint 2013
Avec le gestionnaire de conception, vous pouvez convertir un fichier HTML en page maître SharePoint 2013, soit en fichier .master. Une fois la conversion effectuée, la page maître et le fichier HTML sont associés. De cette façon, lorsque vous modifiez et enregistrez le fichier HTML, les modifications sont synchronisées sur la page maître associée.
## Introduction à la conversion d'une page maître
<a name="Introduction"> </a>

Avec le gestionnaire de conception, vous pouvez convertir un fichier HTML en page maître SharePoint 2013, soit en fichier .master. Une fois la conversion effectuée, la page maître et le fichier HTML sont associés. De cette façon, lorsque vous modifiez et enregistrez le fichier HTML, les modifications sont synchronisées sur la page maître associée.
  
    
    
Pourquoi convertir un fichier HTML, au lieu de créer directement un fichier .master ? Dans SharePoint 2013, les pages maîtres fonctionnent exactement comme dans ASP.NET. Toutefois, SharePoint exige également que certains éléments, tels que les contrôles et les espaces réservés de contenu propres à SharePoint, soient présents sur la page, pour un rendu de la page maître approprié sur SharePoint. Le gestionnaire de conception permet de convertir un fichier HTML en page maître SharePoint entièrement fonctionnelle. Cette opération ne requiert aucune connaissance sur ASP.NET ou les balises spécifiques de SharePoint ; vous pouvez vous concentrer sur la conception de votre site au format HTML, CSS et JavaScript.
  
    
    
Lorsque vous convertissez un fichier HTML en page maître :
  
    
    

- Un fichier .master portant le même nom que votre fichier HTML est créé dans la galerie de pages maîtres.
    
  
- La totalité des balises requises par SharePoint 2013 sont ajoutées au fichier .master, pour un rendu de la page approprié.
    
  
- Les balises telles que les commentaires, les balises **<div>**, les extraits de code et les espaces réservés de contenu sont ajoutés à votre fichier HTML d'origine.
    
  
- Le fichier HTML et la page maître sont associés, de sorte que toutes les modifications ultérieures effectuées dans le fichier HTML sont synchronisées sur le fichier .master lorsque le fichier HTML est enregistré.
    
  

> **REMARQUE**
> La synchronisation ne fonctionne que dans un sens. Les modifications apportées à la page maître HTML sont synchronisés sur le fichier .master associé. Toutefois, si vous choisissez de modifier directement le fichier .master, les modifications ne sont pas synchronisées sur le fichier HTML. Chaque page maître HTML (et chaque mise en page HTML) possède une propriété appelée **Fichier associé** définie sur **True** par défaut, créant l'association et la synchronisation entre les fichiers.
  
    
    

Si vous possédez deux fichiers associés (HTML et .master) et que vous modifiez le fichier .master sans rompre l'association, les modifications apportées au fichier .master seront enregistrées. Toutefois, vous ne pouvez pas archiver ou publier le fichier .master, de sorte que ces modifications ne sont pas enregistrées de façon significative. Toutes les modifications apportées au fichier HTML remplacent le fichier .master. Si vous archivez ou publiez le fichier HTML, les modifications apportées au fichier HTML remplacent celles effectuées dans le fichier .master. Les modifications apportées au fichier .master sont perdues.
  
    
    
Si vous êtes un développeur qui maîtrise ASP.NET, vous pouvez choisir d'utiliser uniquement le fichier .master en rompant l'association entre les fichiers. Pour ce faire, dans le gestionnaire de conception, choisissez **Modifier les propriétés** pour le fichier HTML, puis décochez la case **Fichier associé**. Vous pouvez ensuite réassocier les fichiers en modifiant les propriétés et en cochant cette case : le fichier HTML remplacera de nouveau le fichier .master et les modifications apportées au fichier .master seront perdues.
  
    
    

## Préparer le fichier HTML pour conversion
<a name="Prepare"> </a>

Avant de convertir votre fichier HTML, voici les meilleures pratiques et conseils à prendre en compte :
  
    
    

- Prenons le modèle de page SharePoint. Pour plus d'informations, voir  [Vue d'ensemble du modèle de page SharePoint 2013](overview-of-the-sharepoint-2013-page-model.md). Lorsque vous concevez les maquettes HTML de votre site, vous disposez probablement de plusieurs fichiers HTML pour différents types de pages, comme une page d'article ou une page de catégorie contenant des composants WebPart qui affichent une catégorie d'éléments à partir d'un catalogue. Toutefois, un seul fichier HTML sera converti en page maître. Un fichier HTML peut être converti en page maître, mais pas directement en mise en page, car une mise en page requiert des champs de page.
    
  
- Assurez-vous que votre fichier HTML est compatible avec le langage XML pour que la conversion fonctionne. Cette exigence remplace certaines normes HTML 5 ; par exemple, au format HTML 5, vous pouvez spécifier le **doctype** en minuscules, mais au format XML, **doctype** doit être en majuscules. En outre, vous devez supprimer les balises **<form>** de votre fichier HTML. Exécutez votre fichier HTML via un validateur XML externe pour identifier les erreurs XML avant d'effectuer la conversion.
    
  
- Prenez en compte ces directives importantes pour vos références CSS :
    
  - N'introduisez pas de blocs **<style>** dans la balise **<head>**. Ces styles sont supprimés lors de la conversion. En revanche, attachez votre fichier HTML à un fichier CSS externe.
    
  
  - Ajoutez  `ms-design-css-conversion="no"` à la balise **<CSS link>** si vous utilisez une police web.
    
  
  - Faites attention lorsque vous appliquez des styles dans des balises HTML générales comme **<body>**, **<div>** et **< img>**. Tous les éléments de votre conception SharePoint, y compris le ruban, figurent dans la balise **<body>**. Utilisez les styles que vous appliquez généralement à la balise **<body>** au lieu de **<div id="s4-bodyContainer">** (balise que SharePoint 2013 utilise pour le corps principal de la page). En outre, SharePoint 2013 utilise de nombreuses images affectées par les styles que vous appliquez à la balise **<img>**.
    
  
  - De nombreux concepteurs définissent le style de la navigation en appliquant des classes aux éléments **<ul>** et **<li>**. Toutefois, SharePoint 2013 utilise un contrôle de navigation dynamique, auquel vous pouvez ajouter votre page maître à partir de la galerie d'extraits de code. Les contrôles de navigation SharePoint 2013 contiennent des styles appliqués par défaut que vous devez remplacer.
    
  
- Prenez en compte ces problèmes potentiels sur la dénomination des fichiers :
    
  - Si vous disposez des fichiers Index.html et Index.htm, ils possèdent le même fichier .master.
    
  
  - Si vous disposez de fichiers Design/Index.html et Design/SubDesign/Index.html, tous deux peuvent être convertis en leurs propres fichiers .master séparés. Cependant, ils apparaitront également comme Index.html dans la liste des pages maîtres dans le gestionnaire de conception. Pour les distinguer, cliquez ou sélectionnez le bouton de sélection de chaque fichier pour consulter le chemin d'accès complet.
    
  
- Si vous ajoutez un widget JavaScript, assurez-vous qu'il existe une balise de début **<script>** par ligne.
    
  ```
  
  <script>
  (function( …

  ```


    Ne les placez pas sur la même ligne, comme suit.
    


  ```
  
  <Script> (function( …
  ```

- Vous devez introduire une référence (externe) à la bibliothèque JQuery avant la balise **</head>**.
    
  

## Convertir le fichier HTML en page maître
<a name="Convert"> </a>

Avant de convertir un fichier HTML, vous devez télécharger tous vos fichiers de conception, y compris votre fichier HTML. Pour plus d'informations, voir  [Comment mapper un lecteur réseau sur la galerie Pages maîtres SharePoint 2013](how-to-map-a-network-drive-to-the-sharepoint-2013-master-page-gallery.md).
  
    
    

### Pour convertir le fichier HTML en fichier .master


1. Accédez à votre site de publication.
    
  
2. Dans le coin supérieur droit de la page, sélectionnez **Paramètres**, puis **Gestionnaire de conception**.
    
  
3. Dans le gestionnaire de conception, dans le volet de navigation de gauche, sélectionnez **Modifier les pages maîtres**.
    
  
4. Sélectionnez **Convertir un fichier HTML en une page maître SharePoint**.
    
  
5. Dans la boîte de dialogue **Sélectionner un bien**, parcourez les fichiers et sélectionnez le fichier HTML à convertir.
    
    > **REMARQUE**
      > Lorsque vous téléchargez vos fichiers de conception, vous devez conserver tous les fichiers étant liés à une conception unique dans leur dossier dans la galerie de pages maîtres. Lorsque vous copiez votre dossier de conception dans le lecteur réseau mappé, la galerie de pages maîtres conserve la structure de dossiers que vous avez créée. 
6. Sélectionnez **Insérer**.
    
    Là, SharePoint 2013 convertit votre fichier HTML en fichier .master portant le même nom.
    
    Dans le gestionnaire de conception, votre fichier HTML s'affiche maintenant avec une colonne d'état indiquant l'un des deux états possibles :
    
  - Erreurs
    
  
  - **La conversion a réussi**
    
  
7. Suivez le lien dans la colonne d'état pour prévisualiser le fichier et découvrir les erreurs ou avertissements relatifs à la page maître.
    
    Erreurs
    
    Pour plus d'informations sur la résolution des erreurs et des avertissements, voir  [Procédure : résoudre les erreurs et avertissements lors de l'aperçu d'une page en SharePoint 2013](how-to-resolve-errors-and-warnings-when-previewing-a-page-in-sharepoint-2013.md).
    
    Pour plus d'informations sur l'aperçu de la page maître avec différentes pages, voir  [Comment modifier la page d'aperçu dans le Gestionnaire de conception SharePoint 2013](how-to-change-the-preview-page-in-sharepoint-2013-design-manager.md).
    
    La page d'aperçu contient également un lien d'extraits de code dans le coin supérieur droit. Ce lien ouvre la galerie d'extraits de code, où vous pouvez initier le remplacement des contrôles statiques ou de maquette dans votre conception par des contrôles dynamiques SharePoint. Pour plus d'informations, voir  [Extraits de code du Gestionnaire de conception SharePoint 2013](sharepoint-2013-design-manager-snippets.md).
    
  
8. Pour corriger les erreurs, modifiez le fichier HTML qui réside directement sur le serveur à l'aide d'un éditeur HTML afin d'ouvrir et de modifier le fichier HTML dans le lecteur mappé. Chaque fois que vous enregistrez le fichier HTML, toutes les modifications sont synchronisées sur le fichier .master associé.
    
  
9. Une fois l'aperçu de votre page maître affiché, une balise **<div>** s'ajoute à votre fichier HTML. Vous devrez peut-être faire défiler la page vers le bas pour voir la balise **<div>**.
    
    Cette balise **<div>** constitue le bloc de contenu principal. Elle se trouve à l'intérieur d'un espace réservé de contenu appelé **ContentPlaceHolderMain**. En exécution, lorsqu'un visiteur parcourt votre site et demande une page, cet espace réservé de contenu se remplit avec le contenu d'une mise en page dans une zone de contenu correspondante. Positionnez la balise **<div>** là où vous souhaitez que vos mises en page apparaissent sur la page maître.
    
    Si votre fichier HTML comprend du contenu statique ou de maquette dans le corps de la page, vous initiez alors le processus de suppression de ce contenu statique de la page maître HTML et l'application de ces styles à d'autres éléments du modèle de page SharePoint, comme les mises en page, les contrôles de champ de page, les extraits de code et les modèles d'affichage. Pour consulter un exemple, voir  [Procédure : Créer une mise en page dans SharePoint 2013](how-to-create-a-page-layout-in-sharepoint-2013.md).
    
  

## Comprendre le fichier HTML après la conversion
<a name="Understand"> </a>

Lorsque vous convertissez un fichier HTML en page maître, de nombreuses lignes de balises sont ajoutées à votre fichier HTML. Vous pouvez ignorer en toute sécurité la plupart de ces balises, car elles n'apparaitront pas dans le balisage final de votre site lorsque vous affichez la source dans le navigateur. Toutefois, ce balisage est essentiel pour convertir votre fichier HTML en fichier .master (fichier utilisé par SharePoint). Chaque fois que vous enregistrez une modification dans votre fichier HTML, ce balisage SharePoint permet d'apporter cette même modification au fichier .master associé à l'arrière-plan.
  
    
    
Le balisage qui a été ajouté inclut d'autres balises avant et dans la balise **<head>** les extraits de code et les espaces réservés de contenu. La plupart des balises sont placées dans les balises de commentaires : chaque fois que vous enregistrez une modification dans le fichier HTML, le processus de conversion supprime les commentaires pour utiliser les balises ASP.NET.
  
    
    

### Types de balisage

Vous trouverez ci-dessous une répartition des types de balisage ajoutés au fichier HTML :
  
    
    

- **Propriétés de document :** la balise **<mso>** contient des métadonnées SharePoint, notamment des informations relatives au fichier lui-même et à certaines propriétés requises pour une conversion en fichier .master réussie.
    
  ```HTML
  
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
  ```

- **Enregistrement d'espace de noms SharePoint :** la balise **<SPM>** (« balisage SharePoint ») fournit une ligne pour l'enregistrement d'un espace de noms SharePoint.
    
  ```HTML
  
<!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
  ```

- **Commentaires :** les balises **<CS>** et **<CE>** (« début du commentaire » et « fin du commentaire ») sont ignorées lors du processus de conversion. Ces balises vous aident à analyser les lignes des balises.
    
  ```HTML
  
<!--CS: Start Page Head Contents Snippet-->
…
<!--CE: End Page Head Contents Snippet-->

  <!--CS: Start Ribbon Snippet-->
…
<!--CE: End Ribbon Snippet-->

<!--CS: Start PlaceHolderMain Snippet-->
…
<!--CE: End PlaceHolderMain Snippet-->
  ```

- **Extraits de code :** les balises **<MS>** et **<ME>** (« début de commentaire » et « fin de commentaire ») désignent le début et la fin d'un contrôle SharePoint ou d'un extrait de code. Un extrait de code est un contrôle SharePoint qui ajoute une fonctionnalité SharePoint à votre page. La galerie d'extraits de code vous permet d'en ajouter. Pour plus d'informations, voir [Extraits de code du Gestionnaire de conception SharePoint 2013](sharepoint-2013-design-manager-snippets.md).
    
  ```HTML
  
<!--MS:<SharePoint:RobotsMetaTag runat="server">-->
        <!--ME:</SharePoint:RobotsMetaTag>-->
        <!--MS:<SharePoint:PageTitle runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server">-->
                <!--MS:<SharePoint:ProjectProperty Property="Title" runat="server">-->
                <!--ME:</SharePoint:ProjectProperty>-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:PageTitle>-->
        <!--MS:<SharePoint:StartScript runat="server">-->
        <!--ME:</SharePoint:StartScript>-->
        <!--MS:<SharePoint:CssLink runat="server" Version="15">-->
        <!--ME:</SharePoint:CssLink>-->
        <!--MS:<SharePoint:CacheManifestLink runat="server">-->
        <!--ME:</SharePoint:CacheManifestLink>-->
        <!--MS:<SharePoint:PageRenderMode runat="server" RenderModeType="Standard">-->
        <!--ME:</SharePoint:PageRenderMode>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="core.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="menu.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="callout.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="sharing.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="suitelinks.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:CustomJSUrl runat="server">-->
        <!--ME:</SharePoint:CustomJSUrl>-->
        <!--MS:<SharePoint:SoapDiscoveryLink runat="server">-->
        <!--ME:</SharePoint:SoapDiscoveryLink>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaPlaceHolderAdditionalPageHead" Container="false" runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderAdditionalPageHead" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
            <!--MS:<SharePoint:DelegateControl runat="server" ControlId="AdditionalPageHead" AllowMultipleControls="true">-->
            <!--ME:</SharePoint:DelegateControl>-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderBodyAreaClass" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--MS:<SharePoint:CssRegistration Name="Themable/corev15.css" runat="server">-->
        <!--ME:</SharePoint:CssRegistration>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaSPWebPartManager" runat="server">-->
            <!--MS:<WebPartPages:SPWebPartManager runat="server">-->
            <!--ME:</WebPartPages:SPWebPartManager>-->
        <!--ME:</SharePoint:AjaxDelta>-->
  ```

- **Blocs d'aperçu :** les balises **<PS>** et **<PE>** (« début d'aperçu» et « fin d'aperçu ») entourent une section de code HTML à ne pas modifier, car cette section n'a d'incidence que sur l'aperçu au moment de la conception. Ces sections d'aperçu sont une capture instantanée dans le temps du contrôle SharePoint inséré par cet extrait de code. L'aperçu vous permet d'utiliser de façon plus significative le fichier HTML dans un éditeur HTML côté client. Toutefois, la modification du contenu ou du style au sein de cet aperçu n'a pas d'effet durable sur le fichier .master, ce que SharePoint utilise, en fin de compte. Pour définir le style d'un extrait de code, vous devez identifier et remplacer les styles SharePoint avec vos propres feuilles CSS personnalisées.
    
  ```HTML
  
<!--PS: Start of READ-ONLY PREVIEW (do not modify) -->
<div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div>
<!--PE: End of READ-ONLY PREVIEW -->
  ```

- **ID SharePoint :** deux des extraits de code ajoutés à votre fichier HTML lors de la conversion (l'extrait de code du contenu d'en-tête de page et le ruban SharePoint) possèdent un ID SharePoint associé, ou SID (00 et 02, respectivement). Ces ID permettre de raccourcir les extraits de code et de faciliter la lecture du code HTML dans la page.
    
  ```HTML
  
<!--SID:00 -->

<!--SID:02 {Ribbon}-->
  ```


### Extraits de code ajoutés

Il est important de connaître deux des extraits de code ajoutés à votre fichier HTML. Ils sont ajoutés automatiquement lors de la conversion, mais vous ne pouvez pas les ajouter à partir de la galerie d'extraits de code.
  
    
    

- **Ruban :** pour que les auteurs de contenu puissent créer des pages et du contenu sur votre site SharePoint, votre page maître requiert le ruban et la « navigation de suite » : une nouveauté dans SharePoint 2013. Le ruban est contenu dans un extrait de code de filtrage de sécurité, de sorte que lorsqu'un visiteur accède à votre site, le ruban est affiché uniquement aux utilisateurs authentifiés et non aux anonymes. Vous pouvez déplacer le ruban à un emplacement différent sur la page ou lui définir un style en remplaçant les classes CSS par défaut. Toutefois, nous vous recommandons de ne pas déplacer ou réorganiser les composants (tels que le menu Actions du site) contenus dans le ruban.
    
  ```HTML
  
<!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
<!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
<!--ME:</wssucw:Welcome>-->
<!--ME:</SharePoint:SPSecurityTrimmedControl>-->
  ```

- **ContentPlaceHolderMain :** au bas de la balise **<div id="s4-bodyContainer">**, avant la balise de fermeture **</body>**, le processus de conversion insère un espace réservé de contenu appelé **PlaceHolderMain**. Dans cet extrait de code se trouve la balise jaune bordée de noir **<div>**, laquelle apparaît dans l'affichage de conception de l'éditeur HTML ou dans l'aperçu côté serveur dans le gestionnaire de conception.
    
    Cette balise **<div>** représente la zone où le contenu spécifié par vos pages et mises en page est dirigé. Déplacez l'extrait de code **PlaceHolderMain** dans votre page maître, à l'endroit où il sera rempli par vos mises en page (la zone de conception de votre site n'étant pas la même dans toutes les pages de votre site).
    


  ```HTML
  
<!--CS: Start PlaceHolderMain Snippet-->
                    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
                    <!--MS:<SharePoint:AjaxDelta ID="DeltaPlaceHolderMain" IsMainContent="true" runat="server">-->
                        <!--MS:<asp:ContentPlaceHolder ID="PlaceHolderMain" runat="server">-->
                            <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
            This div, which you should delete, represents the content area that your Page Layouts and pages will fill. Design your Master Page around this content placeholder.
        
                            </div>
                        <!--ME:</asp:ContentPlaceHolder>-->
                    <!--ME:</SharePoint:AjaxDelta>-->
                    <!--CE: End PlaceHolderMain Snippet-->
  ```


## Exemples
<a name="Reference"> </a>

Vous trouverez ci-dessous un exemple de balisage ajouté à un fichier HTML, une fois celui-ci converti en page maître.
  
    
    

### Balisage ajouté à la balise <head>


```HTML

<head>
        <meta http-equiv="X-UA-Compatible" content="IE=10" />
        <!--CS: Start Page Head Contents Snippet-->
        <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="WebPartPages" Namespace="Microsoft.SharePoint.WebPartPages" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SID:00 -->
        <meta name="GENERATOR" content="Microsoft SharePoint" />
        <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
        <meta http-equiv="Expires" content="0" />
        <!--MS:<SharePoint:RobotsMetaTag runat="server">-->
        <!--ME:</SharePoint:RobotsMetaTag>-->
        <!--MS:<SharePoint:PageTitle runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderPageTitle" runat="server">-->
                <!--MS:<SharePoint:ProjectProperty Property="Title" runat="server">-->
                <!--ME:</SharePoint:ProjectProperty>-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:PageTitle>-->
        <!--MS:<SharePoint:StartScript runat="server">-->
        <!--ME:</SharePoint:StartScript>-->
        <!--MS:<SharePoint:CssLink runat="server" Version="15">-->
        <!--ME:</SharePoint:CssLink>-->
        <!--MS:<SharePoint:CacheManifestLink runat="server">-->
        <!--ME:</SharePoint:CacheManifestLink>-->
        <!--MS:<SharePoint:PageRenderMode runat="server" RenderModeType="Standard">-->
        <!--ME:</SharePoint:PageRenderMode>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="core.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="menu.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="callout.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="sharing.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:ScriptLink language="javascript" name="suitelinks.js" OnDemand="true" runat="server" Localizable="false">-->
        <!--ME:</SharePoint:ScriptLink>-->
        <!--MS:<SharePoint:CustomJSUrl runat="server">-->
        <!--ME:</SharePoint:CustomJSUrl>-->
        <!--MS:<SharePoint:SoapDiscoveryLink runat="server">-->
        <!--ME:</SharePoint:SoapDiscoveryLink>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaPlaceHolderAdditionalPageHead" Container="false" runat="server">-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderAdditionalPageHead" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
            <!--MS:<SharePoint:DelegateControl runat="server" ControlId="AdditionalPageHead" AllowMultipleControls="true">-->
            <!--ME:</SharePoint:DelegateControl>-->
            <!--MS:<asp:ContentPlaceHolder id="PlaceHolderBodyAreaClass" runat="server">-->
            <!--ME:</asp:ContentPlaceHolder>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--MS:<SharePoint:CssRegistration Name="Themable/corev15.css" runat="server">-->
        <!--ME:</SharePoint:CssRegistration>-->
        <!--MS:<SharePoint:AjaxDelta id="DeltaSPWebPartManager" runat="server">-->
            <!--MS:<WebPartPages:SPWebPartManager runat="server">-->
            <!--ME:</WebPartPages:SPWebPartManager>-->
        <!--ME:</SharePoint:AjaxDelta>-->
        <!--CE: End Page Head Contents Snippet-->
        <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
        <!--DC:Business Solutions-->
        <link rel="stylesheet" href="css/style.css" type="text/css" charset="utf-8" />
        <!--[if lte IE 7]>
  <link rel="stylesheet" href="css/ie.css" type="text/css" charset="utf-8"/> 
 <![endif]-->
        <!--[if gte mso 9]><xml>
<mso:CustomDocumentProperties>
<mso:HtmlDesignFromMaster msdt:dt="string"></mso:HtmlDesignFromMaster>
<mso:HtmlDesignStatusAndPreview msdt:dt="string">http://[server_name]/sites/PubSite/_catalogs/masterpage/[site_name]/index.html, Conversion successful.</mso:HtmlDesignStatusAndPreview>
<mso:ContentTypeId msdt:dt="string">0x0101000F1C8B9E0EB4BE489F09807B2C53288F0054AD6EF48B9F7B45A142F8173F171BD10003D357F861E29844953D5CAA1D4D8A3A0084F0F9C7FCB65541A59990D173DA60FA</mso:ContentTypeId>
<mso:HtmlDesignAssociated msdt:dt="string">1</mso:HtmlDesignAssociated>
<mso:HtmlDesignConversionSucceeded msdt:dt="string">True</mso:HtmlDesignConversionSucceeded>
</mso:CustomDocumentProperties>
</xml><![endif]-->
    </head>

```


### Balisage ajouté après la balise de début <body>


#### Extrait de code du ruban


```HTML

<!--CS: Start Ribbon Snippet-->
        <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
        <!--SPM:<%@Register Tagprefix="wssucw" TagName="Welcome" Src="~/_controltemplates/15/Welcome.ascx"%>-->
        <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" HideFromSearchCrawler="true" EmitDiv="true">-->
            <div id="TurnOnAccessibility" style="display:none" class="s4-notdlg noindex">
                <a id="linkTurnOnAcc" href="#" class="ms-accessible ms-acc-button" onclick="SetIsAccessibilityFeatureEnabled(true);UpdateAccessibilityUI();document.getElementById('linkTurnOffAcc').focus();return false;">
                    <!--MS:<SharePoint:EncodedLiteral runat="server" text="&amp;lt;%$Resources:wss,master_turnonaccessibility%&amp;gt;" EncodeMethod="HtmlEncode">-->
                    <!--ME:</SharePoint:EncodedLiteral>-->
                </a>
            </div>
            <div id="TurnOffAccessibility" style="display:none" class="s4-notdlg noindex">
                <a id="linkTurnOffAcc" href="#" class="ms-accessible ms-acc-button" onclick="SetIsAccessibilityFeatureEnabled(false);UpdateAccessibilityUI();document.getElementById('linkTurnOnAcc').focus();return false;">
                    <!--MS:<SharePoint:EncodedLiteral runat="server" text="&amp;lt;%$Resources:wss,master_turnoffaccessibility%&amp;gt;" EncodeMethod="HtmlEncode">-->
                    <!--ME:</SharePoint:EncodedLiteral>-->
                </a>
            </div>
        <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
        <div id="ms-designer-ribbon">
            <!--SID:02 {Ribbon}-->
            <!--PS: Start of READ-ONLY PREVIEW (do not modify) --><div class="DefaultContentBlock" style="background:rgb(0, 114, 198); color:white; width:100%; padding:8px; height:64px; overflow:hidden;">The SharePoint ribbon will be here when your file is either previewed on or applied to your site.</div><!--PE: End of READ-ONLY PREVIEW -->
        </div>
        <!--MS:<SharePoint:SPSecurityTrimmedControl runat="server" AuthenticationRestrictions="AnonymousUsersOnly">-->
            <!--MS:<wssucw:Welcome runat="server" EnableViewState="false">-->
            <!--ME:</wssucw:Welcome>-->
        <!--ME:</SharePoint:SPSecurityTrimmedControl>-->
        <!--CE: End Ribbon Snippet-->

```


#### Deux balises <div> SharePoint


```HTML

<div id="s4-workspace">
            <div id="s4-bodyContainer">

```


### Balisage ajouté avant la balise de fermeture </body> et deux balises de fermeture </div>


```HTML

<div data-name="ContentPlaceHolderMain">
                    <!--CS: Start PlaceHolderMain Snippet-->
                    <!--SPM:<%@Register Tagprefix="SharePoint" Namespace="Microsoft.SharePoint.WebControls" Assembly="Microsoft.SharePoint, Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c"%>-->
                    <!--MS:<SharePoint:AjaxDelta ID="DeltaPlaceHolderMain" IsMainContent="true" runat="server">-->
                        <!--MS:<asp:ContentPlaceHolder ID="PlaceHolderMain" runat="server">-->
                            <div class="DefaultContentBlock" style="border:medium black solid; background:yellow; color:black; margin:20px; padding:10px;">
            This div, which you should delete, represents the content area that your Page Layouts and pages will fill. Design your Master Page around this content placeholder.
        
                            </div>
                        <!--ME:</asp:ContentPlaceHolder>-->
                    <!--ME:</SharePoint:AjaxDelta>-->
                    <!--CE: End PlaceHolderMain Snippet-->
                </div>

```


## Ressources supplémentaires
<a name="Additional"> </a>


-  [Vue d'ensemble du gestionnaire de conception dans SharePoint 2013](overview-of-design-manager-in-sharepoint-2013.md)
    
  
-  [Procédure : Créer une mise en page dans SharePoint 2013](how-to-create-a-page-layout-in-sharepoint-2013.md)
    
  
-  [Extraits de code du Gestionnaire de conception SharePoint 2013](sharepoint-2013-design-manager-snippets.md)
    
  

