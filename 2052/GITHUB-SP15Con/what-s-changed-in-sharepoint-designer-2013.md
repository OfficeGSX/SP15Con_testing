---
title: SharePoint Designer 2013 中的更改内容
ms.prod: SHAREPOINT
ms.assetid: f1cd30c9-9a73-428d-9151-f1813b43b020
---


# SharePoint Designer 2013 中的更改内容
了解 SharePoint Designer 2013 中弃用的功能或从其中移除的功能。弃用的功能包括在 SharePoint 2013 发行版中以与以前的产品版本兼容，但将从未来版本中移除弃用的功能。
## SharePoint Designer 2013 中停用的功能
<a name="WhatsChangedSharePointDesigner2013_DiscontinuedFeatures"> </a>

SharePoint Designer 2013 中弃用了或移除了以下功能。
  
    
    

### SharePoint 2010 工作流平台
<a name="WhatsChangedSharePointDesigner2013_WorkflowPlatform"> </a>

 **更改的说明。** SharePoint 2013 中弃用了 SharePoint 2010 工作流平台的一些依赖 Windows Workflow Foundation 3.0 的功能。
  
    
    
 **更改的原因。** SharePoint 2013 引入了在 Windows Workflow Foundation 4.0 基础上构建并与 工作流管理器 1.0 集成的新 SharePoint 2013 工作流平台。
  
    
    
 **迁移路径。** 在 SharePoint Designer 2013 中，您仍可以通过选择 SharePoint 2010 工作流平台来创建 SharePoint 2010 工作流并使用所有 SharePoint 2010 工作流功能。
  
    
    
您还可以将 SharePoint 2010 工作流平台中的功能集成到新的 SharePoint 2013 工作流平台中。为此，请通过选择 SharePoint 2010 工作流平台来创建 SharePoint 2010 工作流；通过选择 SharePoint 2013 工作流平台来创建 SharePoint 2013 工作流；然后使用 SharePoint 2013 工作流中的"启动列表工作流"和"启动网站工作流"操作来调用 SharePoint 2010 工作流。 
  
    
    
以下功能仅在 SharePoint 2010 工作流平台上可用：
  
    
    

- 操作：
    
  - 停止工作流
    
  
  - 捕获文档集的版本
    
  
  - 将文档集发送至存储库
    
  
  - 设置文档集的内容审批状态
    
  
  - 启动文档集审核流程
    
  
  - 声明记录
    
  
  - 设置内容审批状态
    
  
  - 撤消声明记录
    
  
  - 添加列表项 
    
  
  - 继承列表项父权限
    
  
  - 删除列表项权限
    
  
  - 替换列表项权限
    
  
  - 用户的查阅管理器
    
  
  - 将表单分配给组
    
  
  - 分配待办事项
    
  
  - 从用户处收集数据
    
  
  - 启动审批流程
    
  
  - 启动自定义任务流程
    
  
  - 启动反馈流程
    
  
  - 复制列表项（SharePoint Designer 2013 仅支持文档复制操作。）
    
  
- 条件：
    
  - 如果当前项目域等于值
    
  
  - 检查列表项权限级别
    
  
  - 检查列表项权限
    
  
- 步骤：
    
  - 模拟步骤：
    
  
- 数据源：
    
  - 用户配置文件查找
    
  
- 其他功能：
    
  - Visio 集成
    
  
  - 关联栏
    
  
  - 可重用工作流的内容类型关联
    
  
  - 列表/网站工作流的"需要管理列表/Web 权限"功能
    
  
  - 全局可重用工作流类型
    
  
  - 工作流可视化选项
    
  

### SharePoint 页面设计功能
<a name="WhatsChangedSharePointDesigner2013_PageDesignFeatures"> </a>

以下功能在 SharePoint 2013 中不可用。
  
    
    

#### 设计视图和拆分视图
<a name="WhatsChangedSharePointDesigner2013_DesignViewSplitView"> </a>

 **更改的说明。** SharePoint Designer 2010 具有三个用于编辑 HTML 和 ASPX 页面的视图：代码视图、设计视图和拆分视图。从 SharePoint Designer 2013 中移除了设计视图和拆分视图。设计视图和拆分视图的移除会影响 SharePoint Designer 2013 的用于编辑 Web 部件和母版页的功能。如果您在 SharePoint Designer 2013 中编辑页面，则必须使用代码视图。
  
    
    
 **更改的原因。** 与当前版本的 Internet Explorer 相比，设计视图是不支持许多新的 HTML5 和 CSS 标记的较旧技术。 
  
    
    
 **迁移路径。** 如果您在代码视图中编辑页面，则可以按 **F12** 在浏览器中预览页面。您也可以使用 Visual Studio 来编辑页面。
  
    
    
如果您希望直观地设计网站或为网站打造品牌，并且您需要 WYSIWYG（"所见即所得"）页面编辑体验，则可以使用任何专业的 HTML 编辑器，例如 Microsoft Expression Web。然后您可以通过使用新的设计管理器来将 HTML 文件导入到 SharePoint Server 2013 中，设计管理器是发布网站中包括的一种功能，例如发布门户网站集网站模板。
  
    
    

## 其他资源
<a name="WhatsChangedSharePointDesigner2013_AdditionalResources"> </a>


-  [工作流操作快速参考（SharePoint 2013 工作流平台）](workflow-actions-quick-reference-sharepoint-2013-workflow-platform.md)
    
  
-  [从 SharePoint 2010 更改为 SharePoint 2013](http://technet.microsoft.com/zh-cn/library/ff607742%28office.15%29.aspx)
    
  
-  [SharePoint Server 2013 中工作流的新增功能](http://technet.microsoft.com/zh-cn/library/jj219638%28office.15%29.aspx)
    
  

  
    
    

