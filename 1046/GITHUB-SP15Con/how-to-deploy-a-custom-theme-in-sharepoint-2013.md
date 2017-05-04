---
title: Como implantar um tema personalizado no SharePoint 2013
ms.prod: SHAREPOINT
ms.assetid: f703df24-8e56-4e6a-bc37-95acbb3c83e8
---


# Como implantar um tema personalizado no SharePoint 2013
Saiba como implantar um tema personalizado em um site do SharePoint 2013 usando a interface do usuário ou implementando um receptor de recursos.
O SharePoint 2013 inclui temas pré-instalados. Você pode criar temas personalizados criando paletas de cores, esquemas de fontes e páginas mestras adicionais. Depois que os arquivos forem carregados na Galeria de Temas e na Galeria de Páginas Mestras, você poderá implantar um tema em um site usando a interface do usuário ou usando código. Para saber mais sobre paletas de cores e esquemas de fontes, consulte  [Paletas de cores e fontes do SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md).
  
    
    


## Conceitos principais para implantar um tema
<a name="core"> </a>

A Tabela 1 lista artigos que poderão ajudar você a compreender os conceitos principais da implantação de temas.
  
    
    

**Tabela 1. Conceitos principais para implantar um tema**


|**Título do artigo**|**Descrição**|
|:-----|:-----|
| [Visão geral de temas para o SharePoint 2013](themes-overview-for-sharepoint-2013.md) <br/> |Saiba mais sobre a experiência de criação de temas no SharePoint 2013.  <br/> |
| [Eventos futuros](http://msdn.microsoft.com/pt-br/library/ms469501.aspx) <br/> |Saiba mais sobre eventos futuros, o que permitirá que você capture e responda a um evento que ocorre quando um recurso é instalado no farm de servidores.  <br/> |
   

## Carregar arquivos na Galeria de Temas e na Galeria de Páginas Mestras
<a name="section1"> </a>

Você pode criar temas personalizados ao criar paletas de cores e esquemas de fontes adicionais, carregando-os para a Galeria de Temas. Os novos esquemas de fontes e paletas de cores estarão disponíveis quando você modificar um design na experiência de criação de temas ou quando você aplicar um tema programaticamente. De maneira semelhante, se você quiser ter layouts de site adicionais disponíveis, poderá carregar páginas mestras adicionais, e os arquivos de visualização correspondentes, na Galeria de Páginas Mestras. A lista a seguir descreve onde colocar os arquivos:
  
    
    

- **Galeria de Páginas Mestras** Lista os arquivos de página mestra e seus arquivos de visualização correspondentes (arquivos .preview). Um arquivo de visualização de página mestra será necessário se você quiser que a página mestra esteja disponível no assistente **Alterar a aparência**. Arquivos JavaScript e outros ativos de design também podem ser carregados na Galeria de Páginas Mestras.
    
    Para acessar a Galeria de Páginas Mestras da interface do usuário do SharePoint, na página **Configurações do Site**, em **Galerias de Web Designer**, escolha **Páginas mestras**. Você também pode navegar diretamente para o site (http://  _NomeDoSite_/_catálogo/masterpage/).
    
  
- **Galeria de Temas** Lista as paletas de cores e os esquemas de fontes disponíveis para a experiência de criação de temas. O SharePoint examina a pasta **15** para determinar as paletas de cores e os esquemas de fontes disponíveis.
    
    Para acessar a Galeria de Temas da interface do usuário do SharePoint, na página **Configurações do Site**, em **Galerias de Web Designer**, escolha **Temas**. Você também pode navegar diretamente para o site (http:// _NomeDoConjuntoDeSites_/_catalogs/theme/15/).
    
  
- **Biblioteca de estilos** Lista arquivos CSS personalizados que você deseja usar na experiência de criação de temas. Você pode navegar diretamente para a Biblioteca de estilos (substitua _NomeDoConjuntoDeSites_ e _idioma_ nesta URL: http:// _NomeDoConjuntoDeSites_/Style Library/ _idioma_/Themable/).
    
> **OBSERVAçãO**
> Coloque os arquivos CSS personalizados na pasta Themable na biblioteca de Estilos, e não na pasta Themable da Galeria de Páginas Mestras. Somente os arquivos CSS armazenados na pasta Themable na biblioteca de Estilos são reconhecidos pelo mecanismo de temas. 

> **OBSERVAçãO**
> Se você tiver o controle de versão habilitado na Galeria de Páginas Mestras e na Galeria de Temas, também deverá publicar os arquivos de design antes que eles possam ser usados pelo mecanismo de criação de temas. 
  
    
    


## Implantar um tema usando a interface do usuário
<a name="section2"> </a>

Uma aparência, ou design, composta é a paleta de cores, o esquema de fontes, a imagem do plano de fundo e a página mestra que determinam a aparência de um site. A lista Aparências Compostas contém as aparências compostas disponíveis na galeria de designs. Você cria um design ao adicionar um item de lista à lista Aparências Compostas e ao especificar a página mestra, a paleta de cores, o esquema de fontes e a imagem de plano de fundo a serem usados.
  
    
    

> **OBSERVAçãO**
> Um arquivo de visualização da página mestra será necessário se você quiser que a página mestra esteja disponível na galeria de designs. 
  
    
    


### Para adicionar uma aparência composta


1. Escolha o ícone **Configurações** e então escolha **Configurações do site**.
    
  
2. Em **Galerias de Web Designer**, escolha **Aparências compostas**.
    
  
3. Na lista **Aparências Compostas**, selecione **novo item**.
    
  
4. Na caixa de texto **Título**, insira um título para o design.
    
  
5. Na caixa de texto **Nome**, insira um nome para o design. O nome aparecerá na lista Aparências Compostas e na galeria de designs.
    
  
6. Na caixa de texto **URL da Página Mestra**, insira a URL da página mestra. A URL pode ser uma URL relativa.
    
  
7. Na caixa de texto **URL do Tema**, insira a URL da paleta de cores (a URL para o arquivo .spcolor). A URL pode ser uma URL relativa.
    
  
8. Na caixa de texto **URL da imagem**, insira a URL da imagem de plano de fundo. Isso é opcional. A URL pode ser uma URL relativa.
    
  
9. Na caixa de texto **URL do Esquema de Fontes**, insira a URl do esquema de fontes (a URL para o arquivo .spfont). Isso é opcional. A URL pode ser uma URL relativa.
    
  
10. Na caixa de texto **Ordem de Exibição**, insira o número da ordem de exibição. Isso determina onde o designa aparecerá na galeria de designs.
    
  
11. Escolha **Salvar**.
    
    > **OBSERVAçãO**
      > Se houver um problema com os valores da aparência composta, a aparência composta não será adicionada à galeria de designs e nenhuma mensagem será registrada nos arquivos de log. A seguir, os possíveis motivos pelos quais uma aparência composta não poderá ser adicionada: um arquivo não encontrado, há um problema de formatação em um dos arquivos ou o SharePoint não pode acessar os arquivos. 
Agora você pode usar a galeria de designs para aplicar o novo design ao seu site. Para saber mais, consulte  [Escolher um tema para seu site de publicação](http://office.microsoft.com/pt-br/office365-sharepoint-online-enterprise-help/choose-a-theme-for-your-publishing-site-HA102891580.aspx) no Office.com.
  
    
    

## Implantar um tema usando código
<a name="section3"> </a>

Você pode implantar um tema implementando um receptor de recursos.
  
    
    

### Para implantar um tema usando um receptor de recursos.


1. Crie uma classe de receptor de recursos que herde da classe  [SPFeatureReceiver](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.aspx) .
    
  
2. No método [FeatureActivated](https://msdn.microsoft.com/library/Microsoft.SharePoint.SPFeatureReceiver.FeatureActivated.aspx) , crie um objeto [SPTheme](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.aspx) que use a paleta de cores e o esquema de fontes e então aplique o tema ao seu site.
    
    O exemplo de código a seguir mostra como implantar uma paleta de cores e um esquema de fontes personalizados em um site.
    


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


> **OBSERVAçãO**
> O parâmetro  _shareGenerated_ no método **ApplyTo** especifica se os arquivos com tema podem ser compartilhados entre sites de um conjunto de sites. Em geral, ele é definido como **true** para o SharePoint Server e sites do SharePoint Online e definido como **false** para sites do SharePoint Foundation. O parâmetro _shareGenerated_ deverá ser definido como **true** caso você pretenda que os arquivos com tema sejam compartilhados. Para saber mais, consulte [ApplyTo(SPWeb, Boolean)](https://msdn.microsoft.com/library/Microsoft.SharePoint.Utilities.SPTheme.ApplyTo.aspx) .

Quando um usuário aplicar um tema no assistente **Alterar a aparência**, o assistente também atualizará um tema chamado Atual na lista Aparências Compostas e a galeria de designs. Quando você aplicar um tema programaticamente, terá de atualizar o tema Atual manualmente. O exemplo a seguir mostra como atualizar o tema Atual.
    


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


## Recursos adicionais
<a name="bk_addresources"> </a>


-  [Desenvolver o design do site do SharePoint 2013](develop-the-site-design-in-sharepoint-2013.md)
    
  
-  [Paletas de cores e fontes do SharePoint 2013](color-palettes-and-fonts-in-sharepoint-2013.md)
    
  
-  [Como: criar um arquivo de visualização de página mestra do SharePoint 2013](how-to-create-a-master-page-preview-file-in-sharepoint-2013.md)
    
  
-  [Blog da equipe do SharePoint: mostre seu estilo com temas do SharePoint](http://blogs.office.com/b/sharepoint/archive/2012/10/29/show-off-your-style-with-sharepoint-theming.aspx)
    
  

