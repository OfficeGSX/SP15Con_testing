---
title: SharePoint 2013 中的工作流活动类
ms.prod: SHAREPOINT
ms.assetid: 70d5ca8d-520d-40a9-b24e-52bb31bd6c22
---


# SharePoint 2013 中的工作流活动类
提供 SharePoint 2013 中可用的 SharePoint 工作流活动类的摘要。
已针对 SharePoint 2013 修订活动类的调色板。表 1 列出了按其各自的活动类别分组的 49 种可用活动的当前目录。
  
    
    


> **注释**
> 在将来的版本中，此表中列出的活动将提供指向其各自的托管类的 API 参考文档的链接。 
  
    
    


## SharePoint 2013 中可用的工作流活动类
<a name="bk_wfactivityclasses"> </a>


**表 1. 工作流活动类**

||||
|:-----|:-----|:-----|
|**"活动"**  <br/> |**"类别"**  <br/> |**"说明"**  <br/> |
|CreatedBy  <br/> |条件  <br/> |返回当前项是否由指定用户创建。  <br/> |
|CreatedInRange  <br/> |条件  <br/> |返回当前项是否在指定日期范围内创建。  <br/> |
|ModifiedBy  <br/> |条件  <br/> |返回当前项是否由指定用户修改。  <br/> |
|ModifiedInRange  <br/> |条件  <br/> |返回当前项是否在指定日期范围内修改。  <br/> |
|WordsInTitle  <br/> |条件  <br/> |返回当前项的标题中是否存在指定的关键字。  <br/> |
|WorkflowInterop  <br/> |协调  <br/> |启动 SharePoint 2010 工作流 (Windows Workflow Foundation 3.5)。  <br/> |
|WaitForCustomEvent  <br/> |事件  <br/> |等待要发送到工作流中的自定义事件。  <br/> |
|WaitForFieldChange  <br/> |事件  <br/> |等待指定列表项上的指定字段更改为指定的值  <br/> |
|WaitForItemEvent  <br/> |事件  <br/> |等待指定列表项上发生指定事件。  <br/> |
|CheckInItem  <br/> |列表  <br/> |签入指定列表项。  <br/> |
|CheckOutItem  <br/> |列表  <br/> |签出指定列表项。  <br/> |
|CopyItem  <br/> |列表  <br/> |将指定文件箱复制到指定文档库。请勿应用到非文件列表项。  <br/> |
|CreateListItem  <br/> |列表  <br/> |创建一个列表项。  <br/> |
|DeleteListItem  <br/> |列表  <br/> |删除一个列表项。  <br/> |
|LookupSPList  <br/> |列表  <br/> |返回有关与指定列表 ID 对应的列表的信息。  <br/> |
|LookupSPListIte  <br/> |列表  <br/> |返回指定列表项的属性。  <br/> |
|LookupSPListItemBooleanProperty  <br/> |列表  <br/> |以 **Boolean** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListItemDateTimeProperty  <br/> |列表  <br/> |以 **DateTime** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListItemDoubleProperty  <br/> |列表  <br/> |以 **Double** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListItemDynamicValueProperty  <br/> |列表  <br/> |以 **DynamicValue** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListItemGuid  <br/> |列表  <br/> |返回与属性名称和属性值的指定筛选条件匹配的第一个列表项的 GUID 属性。  <br/> |
|LookupSPListItemInt32Property  <br/> |列表  <br/> |以 **Int32** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListItemProperty  <br/> |列表  <br/> |以 **Object** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListItemStringProperty  <br/> |列表  <br/> |以 **String** 的形式返回指定列表项属性的值。 <br/> |
|LookupSPListProperty  <br/> |列表  <br/> |以 **DynamicValue** 的形式返回列表属性。 <br/> |
|UndoCheckOutItem  <br/> |列表  <br/> |撤销指定列表项的签出。  <br/> |
|UpdateListItem  <br/> |列表  <br/> |更新列表项。  <br/> |
|CompositeTask  <br/> |任务  <br/> |运行任务流程 - 将多个任务串行或并行分配给多名人员、等待任务完成，并计算聚合结果。  <br/> |
|SingleTask  <br/> |任务  <br/> |运行简单的任务流程 - 将单个任务分配给单个人员或一个组，等待任务完成。  <br/> |
|ExpandGroupToUsers  <br/> |用户  <br/> |返回身为指定 SharePoint 组主体的成员的用户的 LoginName 的集合。  <br/> |
|IsValidUser  <br/> |用户  <br/> |检查指定的用户主体 ID 是否表示有效的 SharePoint 用户。  <br/> |
|LookupSPGroup  <br/> |用户  <br/> |返回有关与指定主体 ID 对应的 SP 组的信息。  <br/> |
|LookupSPGroupMembers  <br/> |用户  <br/> |返回有关组的每个成员的信息的集合。  <br/> |
|LookupSPPrincipal  <br/> |用户  <br/> |返回有关 SP 主体（主体是指 SharePoint 中可获分配权限的用户或组）的信息。  <br/> |
|LookupSPPrincipalId  <br/> |用户  <br/> |返回与指定用户名对应的主体 ID。  <br/> |
|LookupSPPrincipalProperty  <br/> |用户  <br/> |返回指定 SP 主体的指定属性的值。  <br/> |
|LookupSPUser  <br/> |用户  <br/> |返回有关与指定主体 ID 对应的用户的信息。  <br/> |
|LookupSPUserProperty  <br/> |用户  <br/> |返回与指定 SP 主体对应的用户的指定属性值。  <br/> |
|Email  <br/> |实用工具  <br/> |向 SharePoint 网站用户发送电子邮件消息。  <br/> |
|GetCurrentItemGuid  <br/> |实用工具  <br/> |返回运行工作流程实例的 SharePoint 列表项的 ID 属性。  <br/> |
|GetCurrentListId  <br/> |实用工具  <br/> |返回运行工作流程实例的 SharePoint 列表的 ID 属性。  <br/> |
|GetHistoryListId  <br/> |实用工具  <br/> |如果已为工作流关联指定历史记录列表，则返回工作流实例的历史记录列表的 ID。  <br/> |
|GetTaskListId  <br/> |实用工具  <br/> |如果已为工作流关联指定历史记录列表，则返回工作流实例的任务列表的 ID。  <br/> |
|LookupWorkflowContextProperty  <br/> |实用工具  <br/> |以布尔值的形式返回指定工作流上下文属性的值。  <br/> |
|SetField  <br/> |实用工具  <br/> |在当前项上设置字段。  <br/> |
|SetWorkflowStatus  <br/> |实用工具  <br/> |设置指定文本为当前列表项上指定字段的状态。允许工作流将列表项的任何字符串字段的值设置为"工作流状态"。  <br/> |
|TranslateDocument  <br/> |实用工具  <br/> |使用 SharePoint 翻译服务创建指定文档库中指定文档的已翻译副本。  <br/> |
|WebUri  <br/> |实用工具  <br/> |返回包含工作流的 SP Web 的绝对 URI。  <br/> |
|WriteToHistory  <br/> |实用工具  <br/> |为历史记录列表编写指定注释。  <br/> |
   

## 其他资源
<a name="bkm_addlresource"> </a>


-  [SharePoint 2013 的工作流操作和活动引用](workflow-actions-and-activities-reference-for-sharepoint-2013.md)
    
  
-  [使用 Visual Studio 开发 SharePoint 2013 工作流](develop-sharepoint-2013-workflows-using-visual-studio.md)
    
  
-  [设置和配置 SharePoint 2013 工作流管理器](set-up-and-configure-sharepoint-2013-workflow-manager.md)
    
  

