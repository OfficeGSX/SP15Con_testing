---
title: 使用 Visual Studio 开发 SharePoint 2013 工作流
ms.prod: SHAREPOINT
ms.assetid: 28f5d3b1-6fe8-4b1f-8c4e-b11105fe6f46
---



# 使用 Visual Studio 开发 SharePoint 2013 工作流
SharePoint 2013 支持用于创作工作流的两个主要工作流开发环境：SharePoint Designer 和 Visual Studio。本文概述了这两个工作流开发环境，并讨论了其各自的优缺点。
## SharePoint 工作流的创作基础知识
<a name="bkm_AuthoringBasics"> </a>


> **注释**
> 有关设置与配置 Microsoft SharePoint Server 2013 和 Workflow Manager Client 1.0服务器的指南，请参阅  [设置和配置 SharePoint 2013 工作流管理器](set-up-and-configure-sharepoint-2013-workflow-manager.md)。 
  
    
    

与早期版本一样，Microsoft SharePoint 2013 提供了用于创作工作流的两个主要工作流开发环境：Microsoft SharePoint Designer 和 Microsoft Visual Studio。但是，与早期版本不同的是，使用 Visual Studio 不再提供基于代码的创作策略。相反，SharePoint Designer 和 Visual Studio 都提供一个完全声明性的无代码创作环境，不管您选择的开发工具如何。
  
    
    

> **注释**
> 作为对在 SharePoint Designer 中创作工作流的补充，您还可以使用 Microsoft Visio 2013 通过 Visio 2013 形状来构造您的工作流逻辑，然后将您的逻辑导入 SharePoint Designer 2013 中。有关使用 Visio 2013 创作您的工作流逻辑的信息，请参阅 [SharePoint Designer 和 Visio 中的工作流开发](workflow-development-in-sharepoint-designer-and-visio.md)。 
  
    
    


### 声明性工作流

首先，我们来了解一下"声明性"工作流的含义。此术语意指用 XAML 描述（文本方式）工作流然后在运行时以解释性方式执行，而不是用代码进行创作，然后编译到托管程序集中。
  
    
    
从您在 Workflow Designer（如果使用的是 Visual Studio）或 SharePoint Designer 工作流设计图面（或 Visio，稍后将详述）中操作的工作流构建基块派生（或推断）XAML。构建基块本身是设计器工具箱中的可视工作流设计对象 - 阶段、条件、操作、事件等。虽然各个工具箱（Visual Studio 或 SharePoint Designer）中的工具集略为不同，但声明性工作流的概念保持不变。
  
    
    

## 决策树：SharePoint Designer 与 Visual Studio
<a name="bkm_DecisionTree"> </a>

SharePoint 2013 中的工作流框架的最大优点是，信息工作人员可以使用 SharePoint Designer 的无代码环境轻松创建各种功能强大的工作流。此外，声明性创作环境（如 Visual Studio）还实现了较高的灵活性和高度自定义。
  
    
    
这两个工作流创作环境（即 SharePoint Designer 和 Visual Studio）具有特定的优缺点。在本节中，我们探究了如何确定哪个创作环境最能满足您的工作流开发需求。
  
    
    

### 使用 SharePoint Designer


- **目标用户：**信息工作人员、业务分析人员、SharePoint 开发人员。
    
  
- **难度级别：**熟悉 SharePoint Designer，包括核心工作流组件，如阶段、入口、操作、条件和循环。
    
  
利用 SharePoint Designer，用户可以使用无代码的基于文本的设计器创建附加到列表、库或网站的工作流。或者，他们可以使用新的可视设计环境（在此环境中，图形元素将排列在设计图面上）表示业务流程的逻辑流。SharePoint Designer 擅长于使非技术工作人员能够快速开发工作流。
  
    
    

### 使用 Visual Studio


- **目标用户：**中级或高级软件开发人员。
    
  
- **难度级别：**熟悉 Visual Studio，包括软件开发概念，例如事件接收器、打包和部署以及安全性。
    
  
通过在 Visual Studio 中创作工作流，使您能够灵活创建工作流以支持几乎所有业务流程，不管其复杂性如何，并允许调试和重复使用工作流定义。可能最重要的是，Visual Studio 可让开发人员将 SharePoint 工作流作为更大的 SharePoint 解决方案或 SharePoint 外接程序的一部分包含。
  
    
    
Visual Studio 使开发人员能够创建可供 SharePoint Designer 使用的自定义操作，并提供了执行自定义逻辑的方式。利用 Visual Studio，开发人员还可以创建可部署到多个网站的工作流模板。
  
    
    

## 将 SharePoint Designer 与 Visual Studio 进行比较
<a name="bkm_Comparing"> </a>

下表提供了有关使用 SharePoint Designer 和 Visual Studio 创建 SharePoint 工作流的功能和要求的并行比较。
  
    
    

**表 1. 工作流创作工具比较**


|**功能/要求**|**SharePoint Designer**|**Visual Studio**|
|:-----|:-----|:-----|
|允许快速工作流开发  <br/> |可访问  <br/> |可访问  <br/> |
|允许重用工作流  <br/> |工作流只能由在其上开发它的列表或库使用。但是，SharePoint Designer 提供可在同一个网站中多次使用的可重用工作流。  <br/> |可将工作流编写为模板，以便工作流在部署后可重用并与任何列表或库关联。  <br/> |
|允许您将工作流作为 SharePoint 解决方案或 SharePoint 外接程序的一部分包含。  <br/> |不可访问  <br/> |可访问  <br/> |
|允许您创建自定义操作  <br/> |否。但是，SharePoint Designer 可以使用和实现通过 Visual Studio 创建和部署的自定义操作。  <br/> |是。但请注意，在 Visual Studio 中，将使用基础活动而不是对应的操作。  <br/> |
|允许您编写自定义代码  <br/> |不可访问  <br/> |不可访问  <br/> <br/> **注释**<br/> 与早期版本有所不同。在 SharePoint 2013 中，工作流仅是声明性的并且 Visual Studio 依赖工作流开发的可视设计图面。           |
|可以使用 Visio Professional 创建工作流逻辑  <br/> |可访问  <br/> |不可访问  <br/> |
|部署  <br/> |自动部署到在其中创建工作流的列表、库或网站。  <br/> |创建一个 SharePoint 解决方案包 (.wsp) 文件并将该解决方案包部署到网站 (SPWeb)。  <br/> |
|适用于工作流的一键式发布  <br/> |可访问  <br/> |可访问  <br/> |
|可以打包工作流并将其部署到远程服务器  <br/> |可访问  <br/> |可访问  <br/> |
|调试  <br/> |无法调试。  <br/> |可使用 Visual Studio 调试工作流。  <br/> |
|只能使用网站管理员批准的操作  <br/> |可访问  <br/> |可访问  <br/> <br/> **注释**<br/> 与早期版本有所不同。以前，通过使用 Visual Studio 创作的工作流和操作是基于代码的并在场范围内进行部署，因此不需要管理员批准。           |
   

## 使用 Visual Studio 开发工作流
<a name="bkm_Developing"> </a>

与早期版本不同，SharePoint 2013 中的工作流是完全声明性。Visual Studio 现基于 Windows Workflow Foundation 4 构建，它提供了一个可视工作流设计器图面，允许您在设计器环境中完全创建自定义工作流、工作流模板、表单和自定义工作流活动。然后，打包您的工作流并将其作为一项 SharePoint 功能部署。有关功能打包的信息，请参阅 [使用 SharePoint Foundation 中的功能](http://msdn.microsoft.com/zh-cn/library/ms461324.aspx)。
  
    
    
对于 Visual Studio 开发人员而言，最显著的更改可能是不再编译自定义工作流并将其部署为 .NET Framework 程序集。此外，SharePoint 2013 不再使用 Microsoft InfoPath 表单；而表单生成将依赖 Microsoft ASP.NET 表单。 
  
    
    
最后，Visual Studio 工作流项目模板已发生更改。提供了状态机和顺序工作流的以前的模板，但这些区别不再有任何意义。Visual Studio 项目模板可用于虚拟机 (VM) 上提供的 Visual Studio 内部版本。
  
    
    

## 启用内部部署工作流调试
<a name="bkm_Enabling"> </a>

要在 Visual Studio 中调试内部部署工作流，您需要临时允许 Workflow Manager Tools 通过防火墙访问系统。
  
    
    

1. 在"控制面板"中，依次选择"系统和安全"、"Windows 防火墙"。
    
  
2. 在"控制面板主页"列表中，选择"高级设置"链接。
    
  
3. 在 Windows 防火墙的左侧窗格中，选择"入站规则"。
    
  
4. 在"入站规则"列表中，选择"Workflow Manager Tools 1.0 for Visual Studio 2012 - 测试服务主机"。
    
  
5. 在"操作"列表中，选择"启用规则"。
    
  
6. 在 SharePoint 项目的属性页上，选择"SharePoint"选项卡，然后选中"启用工作流调试"复选框。
    
  

## 使用 Visual Studio 调试 SharePoint Online 工作流
<a name="bkm_Debug"> </a>

要在 Visual Studio 中调试 SharePoint Online 工作流，请执行以下步骤：
  
    
    

1. 如果您在防火墙后，您可能需要安装代理客户端（如  [Forefront Threat Management Gateway (TMG) 客户端](http://www.microsoft.com/zh-cn/download/details.aspx?displaylang=en&amp;id=10504)），具体取决于您的公司的网络拓扑。
    
  
2. 注册 Microsoft Azure 帐户（如果尚未注册），然后登录到该帐户。
    
    有关如何注册 Microsoft Azure 帐户的信息，请参阅  [Microsoft Azure](http://www.windowsazure.com)。
    
  
3. 创建 Microsoft Azure 服务总线命名空间，您可以使用它调试远程工作流。您可以在  [Microsoft Azure 管理门户](http://manage.windowsazure.com)上执行此操作。
    
    有关 Microsoft Azure 服务总线的详细信息，请参阅 [管理服务总线服务命名空间](http://msdn.microsoft.com/zh-cn/library/windowsazure/hh690928.aspx)。
    
    > **注释**
      > SharePoint Online 工作流调试使用 Microsoft Azure 服务总线的中继服务组件，所以使用服务总线时，您将要付费。请参阅 [服务总线定价常见问题](http://msdn.microsoft.com/library/hh667438.aspx)。如果您订阅 Visual Studio Professional with MSDN、Visual Studio Premium with MSDN 或 Visual Studio Ultimate with MSDN，每月您将可以免费访问 Microsoft Azure。借助该访问权限，您可以使用 1,500、3,000 或 3,000 小时的服务总线中继服务，具体取决于您的 MSDN 订阅。请参阅 [在每月不另行付费的情况下获取一定量的 Windows Azure 服务](http://azure.microsoft.com/zh-cn/pricing/member-offers/msdn-benefits/)。 
4. 在 Microsoft Azure 中，选择您的服务命名空间，选择"访问密钥"链接，然后复制"连接字符串"框中的文本。
    
  
5. 在您的 SharePoint 外接程序项目属性页上，选择"SharePoint"选项卡，然后选中"启用工作流调试"复选框。
    
    您必须启用该功能以在 SharePoint Online 中调试工作流。该属性将应用于 Visual Studio 中的所有 SharePoint 项目。如果您打包应用程序以在 Office 商店里进行分配，Visual Studio 将自动关闭工作流调试。
    
  
6. 选中"通过 Microsoft Azure 服务总线启用调试"复选框。然后在"Microsoft Azure 服务总线连接字符串"框中，粘贴您复制的连接字符串。
    
  
启用工作流调试并为 Microsoft Azure 服务总线提供有效的连接字符串后，即可调试 SharePoint Online 工作流。
  
    
    

> **注释**
> 如果您尚未禁用工作流调试并且不希望在您的项目包含工作流时接收通知，请取消选中"如果 Microsoft Azure 服务总线调试未配置则通知我"复选框。 
  
    
    


## 其他资源
<a name="workflowbk_addres"> </a>

对于 Visual Studio 开发人员而言，大量正在开发的 SharePoint 工作流将保持不变。SharePoint 2010 的文档的各个主要部分保持相关性：
  
    
    

- 有关使用 Visual StudioWorkflow Designer 的信息，请参阅  [Visual Studio Designer for Windows Workflow Foundation 概述](http://msdn.microsoft.com/zh-cn/library/ms441543.aspx)。
    
  
- 有关使用 Microsoft ASP.NET 创建表单的信息，请参阅 [工作流表单概述](http://msdn.microsoft.com/zh-cn/library/ms457061.aspx)。
    
  
- 有关功能打包的信息，请参阅 [使用 SharePoint Foundation 中的功能](http://msdn.microsoft.com/zh-cn/library/ms461324.aspx)。
    
  
