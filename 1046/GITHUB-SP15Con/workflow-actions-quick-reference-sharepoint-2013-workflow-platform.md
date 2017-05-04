---
title: Referência rápida de ações de fluxo de trabalho (plataforma de Fluxo de Trabalho do SharePoint 2013)
ms.prod: SHAREPOINTDESIGNER
ms.assetid: eb3434e5-bc75-4474-8873-4980062fd29c
---



# Referência rápida de ações de fluxo de trabalho (plataforma de Fluxo de Trabalho do SharePoint 2013)
Esta referência lista as ações de fluxo de trabalho que têm suporte na compilação atual do SharePoint Designer 2013, além daquelas que não estão disponíveis.
## Ações de fluxo de trabalho no SharePoint Designer 2013
<a name="bkm_WorkflowActions"> </a>

A seguir há uma referência das ações de fluxo de trabalho disponíveis para a plataforma de Fluxo de Trabalho do SharePoint 2013. Além da plataforma de Fluxo de Trabalho do SharePoint 2013, o SharePoint Designer 2013 também dá suporte à plataforma de Fluxo de Trabalho do SharePoint 2010. Para exibir ações de fluxo de trabalho da plataforma 2010, confira  [Referência rápida de ações de fluxo de trabalho (plataforma de fluxo de trabalho do SharePoint 2010)](workflow-actions-quick-reference-sharepoint-2010-workflow-platform.md)
  
    
    

### Ações principais
<a name="bkm_CoreActions"> </a>

As ações principais são aquelas que são executadas mais comumente. Elas são agrupadas para facilitar o acesso. 
  
    
    

**Tabela 1. Referência de ações principais**


|**Ação**|**Descrição**|
|:-----|:-----|
|Adicionar um Comentário  <br/> |Habilita você a deixar comentários informativos no designer de fluxo de trabalho para fins de referência. Isso é particularmente útil quando há outros usuários colaborando no fluxo de trabalho.  <br/> |
|Adicionar Hora à Data  <br/> |Adiciona uma hora específica em minutos, horas, dias ou meses a uma data (não há suporte ao Ano) e armazena o valor de saída como uma variável. A data pode ser uma data atual, uma data específica ou uma pesquisa. O valor de "Data Atual" retorna meia-noite UTC.  <br/> |
|Criar Dicionário  <br/> |Cria uma variável de Dicionário de pares de chave/valor.  <br/> <br/> **OBSERVAçãO**<br/> O Dicionário usa a notação JSON para armazenar dados.           Para obter mais informações sobre a variável de Dicionário, confira  [Noções básicas sobre ações de dicionário do SharePoint Designer 2013](understanding-dictionary-actions-in-sharepoint-designer-2013.md) <br/> |
|Chamar Serviço Web HTTP  <br/> |Funciona como uma chamada de método para um serviço Web HTTP e retorna dados usando o formato JSON. Há suporte à autenticação básica por meio de RequestHeader.  <br/> Para obter mais informações sobre a variável de Dicionário, confira  [Noções básicas sobre ações de dicionário do SharePoint Designer 2013](understanding-dictionary-actions-in-sharepoint-designer-2013.md) <br/> |
|Contar Itens em um Dicionário  <br/> |Retorna uma contagem do número de itens em um dicionário especificado.  <br/> |
|Fazer Cálculo  <br/> |Efetua um cálculo aritmético e armazena o valor resultante em uma variável.  <br/> <br/> **OBSERVAçãO**<br/> Para o SharePoint 2013, essa ação dá suporte somente ao tipo numérico **Double**. Não há suporte a inteiros. Não há suporte ao uso do operador "+" (concatenação) para cadeias de caracteres.           |
|Obter um Item de um Dicionário  <br/> |Retorna um item específico de uma variável de dicionário.  <br/> |
|Registrar na Lista de Histórico  <br/> |Grava uma mensagem de uma lista de itens de mensagem predefinidos na lista de histórico do fluxo de trabalho.  <br/> |
|Pausar Durante  <br/> |Faz com que um fluxo de trabalho pause a execução por um intervalo de tempo especificado em dias, horas e minutos.  <br/> |
|Pausar até a Data  <br/> |Faz com que um fluxo de trabalho pause a execução até uma data e uma hora especificadas.  <br/> |
|Enviar um Email  <br/> |Envia automaticamente uma mensagem de email que contém uma mensagem predeterminada para um usuário ou grupo quando ocorre um evento de fluxo de trabalho especificado.  <br/> <br/> **IMPORTANTE**<br/> Se o site não for adicionado à lista de Sites Confiáveis, os emails serão roteados para a pasta Lixo Eletrônico do Outlook.           |
|Definir Parte de Hora do Campo Data/Hora  <br/> |Cria um carimbo de data/hora e armazena o valor de saída em uma variável. Você pode definir a hora em horas e minutos e adicionar uma data atual, uma data específica ou uma pesquisa.  <br/> |
|Definir Status do Fluxo de Trabalho  <br/> |Define o status do fluxo de trabalho.  <br/> |
|Definir Variável de Fluxo de Trabalho  <br/> |Define uma variável de fluxo de trabalho como um valor. Você também pode usar essa ação quando desejar que o fluxo de trabalho atribua dados a uma variável.  <br/> |
|Ir para Estágio  <br/> |Especifica o próximo estágio para o qual o controle de fluxo deve ser entregue.  <br/> |
   

### Ações de coordenação
<a name="bkm_CoreActions"> </a>

Ações de coordenação são usadas para invocar um fluxo de trabalho com base na plataforma de Fluxo de Trabalho SharePoint 2010. Para obter mais informações sobre as ações de Coordenação, confira  [Noções básicas sobre ações de coordenação do SharePoint Designer 2013](understanding-coordination-actions-in-sharepoint-designer-2013.md)
  
    
    

**Table2. Referência de ações de coordenação**


|**Ação**|**Descrição**|
|:-----|:-----|
|Iniciar um Fluxo de Trabalho de Lista  <br/> |Inicia um Fluxo de Trabalho de Lista com base na plataforma de Fluxo de Trabalho SharePoint 2010.  <br/> <br/> **OBSERVAçãO**<br/>  Iniciar um Fluxo de Trabalho de Lista tem os seguintes problemas:<br/>  O campo do tipo "Atribuições" não pode ser usado como um parâmetro quando o fluxo de trabalho 2010 contém uma ação TaskProcess.<br/>  Quando são feitas várias chamadas ao mesmo fluxo de trabalho da versão 2010, o resultado são várias fontes de dados na funcionalidade de pesquisa do fluxo de trabalho da versão 2013. Essas fontes de dados são as mesmas.<br/>  Nomes de variável na versão 2013 não podem conter caracteres especiais, como "?" e "#". Se um fluxo de trabalho da versão 2010 contiver caracteres especiais, eles serão convertidos em código hexadecimal no fluxo de trabalho da versão 2013.          |
|Iniciar um Fluxo de Trabalho de Site  <br/> |Inicia um Fluxo de Trabalho de Site com base na plataforma de Fluxo de Trabalho SharePoint 2010.  <br/> <br/> **OBSERVAçãO**<br/>  Iniciar um Fluxo de Trabalho de Lista tem os seguintes problemas:<br/>  O campo do tipo "Atribuições" não pode ser usado como um parâmetro quando o fluxo de trabalho 2010 contém uma ação TaskProcess.<br/>  Quando são feitas várias chamadas ao mesmo fluxo de trabalho da versão 2010, o resultado são várias fontes de dados na funcionalidade de pesquisa do fluxo de trabalho da versão 2013. Essas fontes de dados são as mesmas.<br/>  Nomes de variável na versão 2013 não podem conter caracteres especiais, como "?" e "#". Se um fluxo de trabalho da versão 2010 contiver caracteres especiais, eles serão convertidos em código hexadecimal no fluxo de trabalho da versão 2013.          |
   

### Ações da lista
<a name="bkm_ListActions"> </a>

Ações da lista agrupa ações que são usadas para manipular listas e itens de lista.
  
    
    

**Table3. Referência de ações da lista**


|**Ação**|**Descrição**|
|:-----|:-----|
|Check-in de Item  <br/> |Faz check-in de um item que foi submetido a check-out. Você só pode fazer check-in de itens de uma biblioteca de documentos.  <br/> <br/> **CUIDADO**<br/> O fluxo de trabalho sofrerá uma falha se você tentar fazer check-in de um item que não foi submetido a check-out.           |
|Fazer Check-out de Item  <br/> |Faz check-out de um item. O fluxo de trabalho verifica se o item foi submetido a check-in antes de fazer check-out de um documento. Você só pode fazer check-out de itens de uma biblioteca em seu site.  <br/> <br/> **CUIDADO**<br/> O fluxo de trabalho sofrerá uma falha se você tentar fazer check-out de um item que não foi submetido a check-in.           |
|Copiar Documento  <br/> |Copia um documento da lista atual para uma lista de Biblioteca de Documentos diferente.  <br/> |
|Criar Item de Lista  <br/> |Cria um novo item de lista na lista que você especificar. Você pode fornecer os campos e os valores no novo item. Você pode usar essa ação sempre que quiser que um novo item seja criado com informações específicas.  <br/> |
|Excluir Item  <br/> |Exclui um item.  <br/> <br/> **OBSERVAçãO**<br/> Essa ação será encerrada no computador que está executando o mecanismo de fluxo de trabalho do Gerenciador de Fluxo de Trabalho e gerará uma exceção **System.InvalidOperationException**. Não existe uma solução alternativa.           |
|Descartar Check-Out de Item  <br/> |Descarta as alterações e faz check-in do item novamente se um item tiver sido submetido a check-out e tiverem sido feitas alterações nele.  <br/> <br/> **CUIDADO**<br/> O fluxo de trabalho sofrerá uma falha se você tentar fazer check-in de um item que não foi submetido a check-out.           |
|Definir Campo no Item Atual  <br/> |Define um campo especificado no item atual para um valor especificado.  <br/> <br/> **OBSERVAçãO**<br/> Se for preciso que o fluxo de trabalho seja pausado até o valor do campo ser alterado, use a ação **Aguardar Evento no Item de Lista** em vez desta ação.          |
|Traduzir Documento  <br/> |Traduz um documento para um idioma específico  <br/> <br/> **OBSERVAçãO**<br/> Requer um Aplicativo de Serviço de Tradução Automática pré-configurado.           |
|Atualizar Item de Lista  <br/> |Atualiza um item de lista. Você pode especificar os campos e os novos valores nesses campos.  <br/> |
|Aguardar Evento no Item de Lista  <br/> |[Versão aprimorada da ação do Office 2010.] Pausa a instância atual do fluxo de trabalho para esperar um evento de item de lista especificado. Esta ação escuta dois eventos: **ItemUpdated** e **ItemAdded**.  <br/> |
|Aguardar Alteração de Campo no Item Atual  <br/> |Aguarda até que um campo no item atual seja igual a um valor específico.  <br/> |
   

### Ações do Project
<a name="bkm_ProjectActions"> </a>

Ações do Project dão suporte à integração do Microsoft Project. São usadas para criar fluxos de trabalho baseados no Project. Todas as ações do Project são novas no SharePoint Designer 2013.
  
    
    

**Table4. Referência de ações do Project**


|**Ação**|**Descrição**|
|:-----|:-----|
|Criar Projeto do Item Atual  <br/> |Utiliza o item atual para criar um novo projeto no site do PWA do farm do SharePoint.  <br/> |
|Definir Campo de Projeto  <br/> |Define um valor para um campo específico no Project Server.  <br/> <br/> **OBSERVAçãO**<br/> Essa ação requer que primeiro seja feito o check-in do projeto. Se não for feito o check-in do projeto, o fluxo de trabalho será encerrado, e os usuários não poderão abrir o projeto no Project Web App.           |
|Definir Status de Estágio de Projeto  <br/> |Define o status do Estágio do Projeto.  <br/> <br/> **OBSERVAçãO**<br/> Ocorre uma exceção quando um projeto atual está submetido a check-out.           |
|Definir Campo de Status na Lista de Ideias  <br/> |Atualiza o status no item de lista original que está associado ao projeto atual.  <br/> |
|Aguardar o Evento do Projeto  <br/> |Aguarda um evento específico do projeto.  <br/> |
   

### Ações de tarefa
<a name="bkm_TaskActions"> </a>

Ações de tarefa fornecem a capacidade de invocar um fluxo de trabalho com base na plataforma de Fluxo de Trabalho do SharePoint 2010 de um fluxo de trabalho com base na plataforma de Fluxo de Trabalho do SharePoint 2013.
  
    
    

**Tabela 5. Referência de ações de tarefa**


|**Ação**|**Descrição**|
|:-----|:-----|
|Atribuir uma Tarefa  <br/> |Atribui uma tarefa de fluxo de trabalho a um usuário e estabelece uma data de conclusão para a conclusão da tarefa.  <br/> |
|Iniciar um Processo da Tarefa  <br/> |Cria tarefas em vários usuários e habilita as tarefas a serem selecionadas por meio de um processo personalizado.  <br/> |
   

### Ações de utilitário
<a name="bkm_UtilityActions"> </a>

Ações de utilitário são ações que manipulam cadeias de caracteres ou localizam o intervalo entre datas. 
  
    
    

**Tabela 6. Referência de ações de utilitário**


|**Ação**|**Descrição**|
|:-----|:-----|
|Extrair Subcadeia do Fim da Cadeia de Caracteres  <br/> |Copia um número especificado de caracteres começando na extremidade de uma cadeia de caracteres e armazena o resultado em uma variável.  <br/> |
|Extrair Subcadeia do Índice da Cadeia de Caracteres  <br/> |Copia uma subcadeia começando em um índice especificado na cadeia de caracteres e coloca o valor em uma variável.  <br/> <br/> **OBSERVAçãO**<br/> Lembre-se de que, embora o valor de índice no Microsoft SharePoint Designer 2013 seja baseada em zero, os valores no SharePoint Designer 2010 foram indexados começando em 1.           |
|Extrair Subcadeia do Início da Cadeia de Caracteres  <br/> |Copia um número especificado de caracteres começando no início de uma cadeia de caracteres e armazena o resultado em uma variável.  <br/> |
|Extrair Subcadeia da Cadeia de Caracteres do Índice com Tamanho  <br/> |Copia uma subcadeia de caracteres que consiste em um número especificado de caracteres, começando em um índice especificado na cadeia de caracteres, e coloca o valor em uma variável.  <br/> <br/> **OBSERVAçãO**<br/> Lembre-se de que, embora o valor de índice no Microsoft SharePoint Designer 2013 seja baseada em zero, os valores no SharePoint Designer 2010 foram indexados começando em 1.           |
|Localizar Intervalo entre Datas  <br/> |Calcula o intervalo de tempo em minutos, horas ou dias entre duas datas e armazena o resultado em uma variável.  <br/> |
|Aparar Cadeia de Caracteres  <br/> |Remove espaços em branco do início e do fim de uma cadeia de caracteres.  <br/> |
|Localizar Subcadeia de Caracteres na Cadeia de Caracteres  <br/> |Localiza uma subcadeia de caracteres específica dentro de uma cadeia de caracteres e retorna o índice da posição inicial da subcadeia de caracteres.  <br/> |
|Substituir Subcadeia de Caracteres na Cadeia de Caracteres  <br/> |Substitui uma subcadeia de caracteres específica por outra subcadeia de caracteres.  <br/> |
|Aparar Cadeia de Caracteres  <br/> |Remove espaços em branco do início e do fim de uma cadeia de caracteres.  <br/> |
   

## Ações de fluxo de trabalho preteridas no SharePoint 2013
<a name="bkm_NotAvailable"> </a>

Para obter uma lista de ações do SharePoint 2010 que são preteridas e não aparecerão no SharePoint 2013, confira  [Ações de fluxo de trabalho disponíveis usando o bridge de interoperabilidade de fluxo de trabalho](workflow-actions-available-using-the-workflow-interop-bridge.md).
  
    
    

## Recursos adicionais
<a name="bkm_addlresource"> </a>


-  [Referência de ações e as atividades de fluxo de trabalho para o SharePoint 2013](workflow-actions-and-activities-reference-for-sharepoint-2013.md)
    
  
-  [Conceitos básicos de fluxos de trabalho no SharePoint 2013](sharepoint-2013-workflow-fundamentals.md)
    
  
-  [Iniciar fluxos de trabalho no SharePoint 2013](get-started-with-workflows-in-sharepoint-2013.md)
    
  
-  [Desenvolvimento de fluxos de trabalho no SharePoint Designer e no Visio](workflow-development-in-sharepoint-designer-and-visio.md)
    
  
