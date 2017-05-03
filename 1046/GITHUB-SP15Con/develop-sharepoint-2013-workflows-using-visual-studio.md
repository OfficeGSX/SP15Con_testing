---
title: Desenvolver fluxos de trabalho do SharePoint 2013 usando o Visual Studio
ms.prod: SHAREPOINT
ms.assetid: 28f5d3b1-6fe8-4b1f-8c4e-b11105fe6f46
---



# Desenvolver fluxos de trabalho do SharePoint 2013 usando o Visual Studio
O SharePoint 2013 dá suporte a dois ambientes de desenvolvimento de fluxo de trabalho para criação de fluxos de trabalho: SharePoint Designer e Visual Studio. Este artigo resume e discute as vantagens e as desvantagens de cada um deles.
## Noções básicas sobre criação para fluxos de trabalho do SharePoint
<a name="bkm_AuthoringBasics"> </a>


> **OBSERVAçãO**
> Para obter orientação sobre a instalação e a configuração do Microsoft SharePoint Server 2013 e do servidor do Workflow Manager Client 1.0, consulte  [Início: Instalar e configurar o Gerenciador de fluxo de trabalho do SharePoint 2013](set-up-and-configure-sharepoint-2013-workflow-manager.md). 
  
    
    

Assim como nas versões anteriores, o Microsoft SharePoint 2013 oferece dois ambientes de desenvolvimento de fluxo de trabalho principais para a criação de fluxos de trabalho: Microsoft SharePoint Designer e Microsoft Visual Studio. Entretanto, o que difere das versões anteriores é que a utilização do Visual Studio não oferece mais uma estratégia de criação baseada em código. Em vez disso, o SharePoint Designer e o Visual Studio oferecem um ambiente de criação sem código totalmente declarativo, independentemente da ferramenta de desenvolvimento escolhida.
  
    
    

> **OBSERVAçãO**
> Como um complemento à criação de fluxos de trabalho no SharePoint Designer, você também pode usar o Microsoft Visio 2013 para estruturar a lógica do seu fluxo de trabalho usando formas do Visio 2013 e então importe sua lógica para o SharePoint Designer 2013. Para saber mais sobre o uso do Visio 2013 para criar a lógica do seu fluxo de trabalho, consulte  [Desenvolvimento de fluxos de trabalho no SharePoint Designer e no Visio](workflow-development-in-sharepoint-designer-and-visio.md). 
  
    
    


### Fluxos de trabalho declarativos

Primeiro, vamos esclarecer o que significa fluxos de trabalho "declarativos". Esse termo significa que, em vez de ser criado em código e então compilado em assemblies gerenciados, o fluxo de trabalho é descrito (literalmente) em XAML e então executado interpretativamente em tempo de execução.
  
    
    
O XAML é derivado (ou presumido) dos blocos de construção do fluxo de trabalho que você manipula no Workflow Designer (se estiver usando o Visual Studio) ou a superfície de design do fluxo de trabalho do SharePoint Designer (ou o Visio, mas mais sobre o último). Os próprios blocos de construção são os objetos de design do fluxo de trabalho visual na caixa de ferramentas do designer  estágios, condições, ações, eventos e assim por diante. O conjunto de ferramentas nas respectivas caixas de ferramentas (Visual Studio ou SharePoint Designer) difere de alguma forma, mas o conceito do fluxo de trabalho declarativo permanece o mesmo.
  
    
    

## Árvore de decisões: SharePoint Designer versus Visual Studio
<a name="bkm_DecisionTree"> </a>

Entre as maiores vantagens da estrutura de fluxo de trabalho no SharePoint 2013, está a facilidade com a qual os operadores de informações podem usar o ambiente sem código do SharePoint Designer para a criação de fluxos de trabalho ricos e eficientes. Além disso, um alto grau de flexibilidade e de personalização estão disponíveis em um ambiente de criação declarativa, como o Visual Studio.
  
    
    
Ambos os ambientes de criação de fluxo de trabalho  o SharePoint Designer e o Visual Studio  oferecem vantagens e desvantagens específicas. Nesta seção, exploramos a forma como determinar qual ambiente de criação se ajusta melhor às suas necessidades de desenvolvimento do fluxo de trabalho.
  
    
    

### Usando o SharePoint Designer


- **Usuários de destino:** operadores de informações, analistas de negócios, desenvolvedores do SharePoint.
    
  
- **Nível de dificuldade:** familiaridade com o SharePoint Designer, incluindo os componentes fundamentais do fluxo de trabalho, como estágios, portões, ações, condições e loops.
    
  
Com o SharePoint Designer, os usuários podem criar um fluxo de trabalho anexado a uma lista, biblioteca ou site usando um designer baseado em texto sem código. Ou eles podem usar o novo ambiente de design visual no qual elementos gráficos são organizados em uma superfície de design para representarem o fluxo lógico de um processo de negócios. O SharePoint Designer se sobressai na habilitação do desenvolvimento rápido de fluxos de trabalho por trabalhadores não técnicos.
  
    
    

### Usando o Visual Studio


- **Usuários de destino:** desenvolvedores de software intermediários ou avançados.
    
  
- **Nível de dificuldade:** familiaridade com o Visual Studio, incluindo conceitos de desenvolvimento de software, como receptores, embalagem e implantação de eventos, além de segurança.
    
  
A criação de fluxos de trabalho no Visual Studio oferece flexibilidade para criar fluxos de trabalho para dar suporte a virtualmente qualquer processo de negócios, independentemente de sua complexidade, além de permitir a depuração e a reutilização de definições de fluxo de trabalho. Talvez o mais importante, o Visual Studio permite que os desenvolvedores incluam fluxos de trabalho do SharePoint como parte de uma solução mais ampla do SharePoint ou do Suplemento do SharePoint.
  
    
    
O Visual Studio permite que desenvolvedores criem ações personalizadas para consumo do SharePoint Designer e oferece os meios de executar a lógica personalizada. Com o Visual Studio, os desenvolvedores também podem criar modelos de fluxo de trabalho, que podem ser implantados para vários sites.
  
    
    

## Comparando o SharePoint Designer ao Visual Studio
<a name="bkm_Comparing"> </a>

A tabela a seguir oferece uma comparação lado a lado dos recursos e dos requisitos para a utilização do SharePoint Designer e do Visual Studio para a criação de fluxos de trabalho do SharePoint.
  
    
    

**Tabela 1. Comparação de ferramentas de criação de fluxo de trabalho**


|**Recurso/requisito**|**SharePoint Designer**|**Visual Studio**|
|:-----|:-----|:-----|
|Permite desenvolvimento rápido do fluxo de trabalho  <br/> |Sim  <br/> |Sim  <br/> |
|Permite a reutilização de fluxos de trabalho  <br/> |Um fluxo de trabalho só pode ser usado pela lista ou pela biblioteca na qual foi desenvolvido. Entretanto, o SharePoint Designer oferece fluxos de trabalho reutilizáveis que podem ser usados várias vezes no mesmo site.  <br/> |Um fluxo de trabalho pode ser escrito como um modelo para que, depois de implantado, possa ser reutilizado e associado a qualquer lista ou biblioteca.  <br/> |
|Permite que você inclua um fluxo de trabalho como parte de uma solução do SharePoint ou do Suplemento do SharePoint.  <br/> |Não  <br/> |Sim  <br/> |
|Permite a criação de ações personalizadas  <br/> |Não. Entretanto, o SharePoint Designer pode consumir e implementar ações personalizadas criadas e implantadas usando o Visual Studio.  <br/> |Sim. Entretanto, esteja ciente de que, no Visual Studio, as atividades subjacentes, e não as ações correspondentes, são usadas.  <br/> |
|Permite escrever um código personalizado  <br/> |Não  <br/> |Não  <br/> <br/> **OBSERVAçãO**<br/> Isso foi alterado das versões anteriores. No SharePoint 2013, os fluxos de trabalho só serão declarativos e o Visual Studio baseia-se na superfície de design visual para o desenvolvimento de fluxo de trabalho.           |
|Pode usar o Visio Professional para criar lógica de fluxo de trabalho  <br/> |Sim  <br/> |Não  <br/> |
|Implantação  <br/> |Automaticamente implantado à lista, biblioteca ou site em que foi criado.  <br/> |Crie um arquivo de pacote de solução do SharePoint (.wsp) e implante o pacote da solução no site (SPWeb).  <br/> |
|Publicação de um clique disponível para fluxos de trabalho  <br/> |Sim  <br/> |Sim  <br/> |
|Os fluxos de trabalho podem ser empacotados e implantados em um servidor remoto  <br/> |Sim  <br/> |Sim  <br/> |
|Depuração  <br/> |Não pode ser depurado.  <br/> |O fluxo de trabalho pode ser depurado usando o Visual Studio.  <br/> |
|Só pode usar ações aprovadas pelo administrador do site  <br/> |Sim  <br/> |Sim  <br/> <br/> **OBSERVAçãO**<br/> Isso foi alterado das versões anteriores. Anteriormente, os fluxos de trabalho e as ações criadas usando o Visual Studio eram baseadas em código e implantadas no escopo do farm e, portanto, a aprovação do administrador não era necessária.           |
   

## Desenvolvimento fluxos de trabalho usando o Visual Studio
<a name="bkm_Developing"> </a>

Ao contrário das versões anteriores, os fluxos de trabalho no SharePoint 2013 são totalmente declarativos. Agora criado com base no Windows Workflow Foundation 4, o Visual Studio oferece uma superfície de designer de fluxo de trabalho visual que permite a criação de fluxos de trabalho personalizados, modelos de fluxo de trabalho, formulários e atividades de fluxo de trabalho personalizadas inteiramente no ambiente do designer. Seu fluxo de trabalho é então empacotado e implantado como um Recurso do SharePoint. Para saber mais sobre o empacotamento do Recurso, consulte  [Usando recursos no SharePoint Foundation](http://msdn.microsoft.com/pt-br/library/ms461324.aspx).
  
    
    
Talvez a alteração mais significativa para desenvolvedores do Visual Studio seja que os fluxos de trabalho personalizados não são mais compilados e implantados como assemblies do .NET Framework. Além disso, o SharePoint 2013 não usa mais formulários do Microsoft InfoPath; em seu lugar, a geração de formulários se baseia em formulários do Microsoft ASP.NET. 
  
    
    
Por fim, os modelos de projeto de fluxo de trabalho do Visual Studio foram alterados. Enquanto os modelos anteriores para máquina de estado e fluxos de trabalho sequenciais eram fornecidos, essas distinções não são mais significativas. Em vez disso, os modelos de projeto do Visual Studio estão disponíveis na compilação do Visual Studio fornecida em sua máquina virtual (VM).
  
    
    

## Habilitando a depuração de fluxo de trabalho local
<a name="bkm_Enabling"> </a>

Para depurar fluxos de trabalho locais no Visual Studio, você precisa permitir temporariamente que as Ferramentas do Gerenciador de Fluxos de Trabalho acessem seu sistema por meio do firewall.
  
    
    

1. No **Painel de Controle**, escolha **Sistema e Segurança**, **Firewall do Windows**.
    
  
2. Na lista **Início do Painel de Controle**, escolha o link **Configurações Avançadas**.
    
  
3. No painel esquerdo do Firewall do Windows, escolha **Regras de Entrada**.
    
  
4. Na lista **Regras de Entrada**, escolha **Ferramentas do Gerenciador de Fluxos de Trabalho 1.0 para Visual Studio 2012 - Testar Host de Serviço**.
    
  
5. Na lista **Ações**, escolha **Habilitar Regra**.
    
  
6. Na página de propriedades de seu projeto do SharePoint, escolha a guia **SharePoint** e então marque a caixa de seleção **Habilitar depuração do Fluxo de Trabalho**.
    
  

## Depurando fluxos de trabalho do SharePoint Online usando o Visual Studio
<a name="bkm_Debug"> </a>

Para depurar fluxos de trabalho do SharePoint Online no Visual Studio, execute as seguintes etapas:
  
    
    

1. Se você estiver por trás de um firewall, talvez seja necessário instalar um cliente proxy (como o  [Cliente do Forefront Threat Management Gateway (TMG)](http://www.microsoft.com/pt-br/download/details.aspx?displaylang=en&amp;id=10504)), dependendo da topologia de rede da sua empresa.
    
  
2. Registre-se em uma conta do Microsoft Azure, caso ainda não tenha se registrado, e entre na conta.
    
    Para saber mais sobre como se registrar em uma conta do Microsoft Azure, consulte  [Microsoft Azure](http://www.windowsazure.com).
    
  
3. Crie um namespace do Microsoft Azure, que você possa usar para depurar fluxos de trabalho remotos. É possível fazer isso no  [portal de gerenciamento do Barramento de Serviço do Microsoft Azure](http://manage.windowsazure.com).
    
    Para saber mais sobre o Barramento de Serviço do Microsoft Azure, consulte  [Gerenciando namespaces do serviço Barramento de Serviço](http://msdn.microsoft.com/pt-br/library/windowsazure/hh690928.aspx).
    
    > **OBSERVAçãO**
      > A depuração de fluxo de trabalho do SharePoint Online usa o componente Serviço de Retransmissão do Microsoft Azure e, portanto, você será cobrado pelo uso do Barramento de Serviço. Consulte  [Perguntas frequentes sobre preço do Barramento de Serviço](http://msdn.microsoft.com/library/hh667438.aspx). Você obtém acesso gratuito ao Microsoft Azure todo mês em que assinar o Visual Studio Professional com MSDN, Visual Studio Premium com MSDN ou Visual Studio Ultimate com MSDN. Com esse acesso, você pode usar a retransmissão do Barramento de Serviço para 1.500, 3.000 ou 3.000 horas, dependendo da sua assinatura MSDN. Consulte  [Obter uma quantidade de Serviços do Microsoft Azure a cada mês sem cobrança adicional](http://www.windowsazure.com/pt-br/pricing/member-offers/msdn-benefits/). 
4. No Microsoft Azure, escolha seu namespace de serviço, escolha o link **Chave de Acesso** e então copie o texto na caixa **Cadeia de Conexão**.
    
  
5. Na página de propriedades do seu projeto do Suplemento do SharePoint, escolha a guia **SharePoint** e então marque a caixa de seleção **Habilitar a depuração do Fluxo de Trabalho**.
    
    Você deve habilitar esse recurso para depurar fluxos de trabalho no SharePoint Online. Essa propriedade se aplica a todos os projetos do SharePoint no Visual Studio. O Visual Studio desativa automaticamente a depuração de fluxo de trabalho caso você empacote seu aplicativo para distribuição na loja do Office.
    
  
6. Marque a caixa de seleção **Habilitar a depuração via Barramento de Serviço do Microsoft Azure**. Em seguida, na caixa **Cadeia de conexão do Barramento de Serviço do Microsoft Azure**, cole a cadeia de conexão que você copiou.
    
  
Depois de habilitar a depuração de fluxo de trabalho e fornecer uma cadeia de conexão válida para o Barramento de Serviço do Microsoft Azure, você poderá depurar fluxos de trabalho do SharePoint Online.
  
    
    

> **OBSERVAçãO**
> Se você não tiver desabilitado a depuração do fluxo de trabalho e não quiser receber uma notificação sempre que seu projeto contiver um fluxo de trabalho, desmarque a caixa de seleção **Avisar se a depuração do Barramento de Serviço do Microsoft Azure não estiver configurada**. 
  
    
    


## Recursos adicionais
<a name="workflowbk_addres"> </a>

Uma grande parte do desenvolvimento de fluxos de trabalho do SharePoint permanece inalterada para o desenvolvedor do Visual Studio. As principais seções do documento para o SharePoint 2010 permanecem relevantes:
  
    
    

- Para saber mais sobre o uso do Visual StudioWorkflow Designer, consulte  [Visão geral do Visual Studio Designer para Windows Workflow Foundation](http://msdn.microsoft.com/pt-br/library/ms441543.aspx).
    
  
- Para saber mais sobre a criação de formulários usando o Microsoft ASP.NET, consulte  [Visão geral sobre formulários de fluxo de trabalho](http://msdn.microsoft.com/pt-br/library/ms457061.aspx).
    
  
- Para saber mais sobre o empacotamento do Recurso, consulte  [Usando recursos do SharePoint Foundation](http://msdn.microsoft.com/pt-br/library/ms461324.aspx).
    
  
