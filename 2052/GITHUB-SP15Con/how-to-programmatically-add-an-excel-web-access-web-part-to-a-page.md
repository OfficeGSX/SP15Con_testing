---
title: 如何：以编程方式将 Excel Web Access Web 部件添加到页面
keywords: how to,howdoi,howto,webpart
f1_keywords:
- how to,howdoi,howto,webpart
ms.prod: SHAREPOINT
ms.assetid: 858bb0f6-654a-4f12-ba0b-4776bda5ff6d
---


# 如何：以编程方式将 Excel Web Access Web 部件添加到页面

本示例说明如何以编程方式将 Excel Web Access Web 部件添加到 SharePoint 页面，另外还说明在 Excel Web Access Web 部件中如何以编程方式显示 Excel 工作簿。 
  
    
    

以下项目使用 Microsoft Visual Studio。
> **注释**
> 根据 Visual Studio 版本以及您使用的 Visual Studio 集成开发环境 (IDE) 设置，创建 Visual Studio 项目的过程和步骤可能与本主题中所示的过程略有不同。 
  
    
    


> **注释**
> 假定您已经创建 SharePoint 文档库并将其设为受信任的位置。有关详细信息，请参阅 [如何：信任位置](how-to-trust-a-location.md)。 
  
    
    


## 添加引用

以下步骤演示如何查找 Microsoft.Office.Excel.WebUI.dll 以及如何添加对它的引用。对 Microsoft.Office.Excel.WebUI.Internal.dll 和 Microsoft.SharePoint.dll 重复这些步骤。
  
    
    

> **注释**
> 假定您已将 Microsoft.Office.Excel.WebUI.dll 和 Microsoft.Office.Excel.WebUI.Internal.dll 从全局程序集缓存复制到所需文件夹。有关如何查找和复制 Microsoft.Office.Excel.WebUI.dll 与 Microsoft.Office.Excel.WebUI.Internal.dll 的信息，请参阅 [如何：查找和复制 Microsoft.Office.Excel.WebUI.dll 与 Microsoft.Office.Excel.WebUI.Internal.dll](how-to-locate-and-copy-microsoft-office-excel-webui-dll-and-microsoft-office-exc.md)。 
  
    
    


### 添加对 Microsoft.Office.Excel.WebUI.dll 的引用


1. 在"项目"菜单上单击"添加引用"。
    
  
2. 在"添加引用"对话框中，单击"浏览"。
    
    > **注释**
      > 您也可以打开"解决方案资源管理器"窗格中的"添加引用"对话框，方法是右键单击"引用"并选择"添加引用"。 
3. 浏览到 Microsoft.Office.Excel.WebUI.dll 的位置。
    
  
4. 选择 Microsoft.Office.Excel.WebUI.dll，然后单击"确定"。
    
  
5. 单击"添加引用"。对 Microsoft.Office.Excel.WebUI.dll 的引用已添加到您的项目中。
    
  

## 实例化 Web 部件


### 实例化 Excel Web Access Web 部件


1. 将 Microsoft.Office.Excel.WebUI 命名空间作为指令添加到您的代码中，以便当您使用此命名空间中的类型时，您无需完全符合这些类型：
    
  ```cs
  
using Microsoft.Office.Excel.WebUI;
  ```


  ```VB.net
  Imports Microsoft.Office.Excel.WebUI
  ```

2. 实例化并初始化 Excel Web Access Web 部件，如下所示：
    
  ```cs
  
 ExcelWebRenderer ewaWebPart = new ExcelWebRenderer();
  ```


  ```VB.net
  
Dim ewaWebPart As New ExcelWebRenderer()
  ```


### 以编程方式显示工作簿


1. 在本示例中， **AddWebPart** 方法将 Excel 工作簿位置的路径作为参数引入。用户可在 Windows 表单文本框中键入，然后单击按钮来提供此路径。
    
    **示例代码提供者：** Daniel Mullowney，Microsoft Corporation
    


  ```cs
  
public bool AddWebPart(string sitename, string book)
{
...
}
            private void AddEWAButton_Click(object sender, 
                EventArgs e)
            {
                siteurl = textBox1.Text;
                bookuri = textBox2.Text;
                succeeded = AddWebPart(siteurl, bookuri);
                if (succeeded)
                {
                    MessageBox.Show(
                        success,
                        appname,
                        MessageBoxButtons.OK,
                        MessageBoxIcon.Information);
                    progressBar1.Value = 1;
                }
            }
  ```




  ```VB.net
  
Public Function AddWebPart(ByVal sitename As String, ByVal book As String) As Boolean
...
End Function
Private Sub AddEWAButton_Click(ByVal sender As Object, ByVal e As EventArgs)
        siteurl = textBox1.Text
        bookuri = textBox2.Text
        succeeded = AddWebPart(siteurl, bookuri)
        If succeeded Then
            MessageBox.Show(success, appname, MessageBoxButtons.OK, MessageBoxIcon.Information)
            progressBar1.Value = 1
        End If
End Sub
  ```


> **重要信息**
> 确保工作簿保存的位置为受信任的位置。 
2. 您可以通过使用下面的代码，以编程方式显示 Excel 工作簿。
    
    **示例代码提供者：** Daniel Mullowney，Microsoft Corporation
    


  ```cs
  
...
// Instantiate Excel Web Access Web Part.
// Add an Excel Web Access Web Part in a shared view.
ExcelWebRenderer ewaWebPart = new ExcelWebRenderer();
ewaWebPart.WorkbookUri = book;
progressBar1.PerformStep();

try
{
    webPartManager.AddWebPart(ewaWebPart, "Left", 0);
}
catch (Exception exc)
{
    MessageBox.Show(
        addWebPartError + "\\n" + exc.Message,
        appName,
        MessageBoxButtons.OK,
        MessageBoxIcon.Asterisk);
    progressBar1.Value = 1;
    return b;

  ```




  ```VB.net
  
'Instantiate Excel Web Access Web Part.
'Add an Excel Web Access Web Part in a shared view.
Dim ewaWebPart As New ExcelWebRenderer()
ewaWebPart.WorkbookUri = book
progressBar1.PerformStep()

Try
    webPartManager.AddWebPart(ewaWebPart, "Left", 0)
Catch exc As Exception
    MessageBox.Show(addWebPartError &amp; vbLf &amp; exc.Message, appName, 
        MessageBoxButtons.OK, MessageBoxIcon.Asterisk)
    progressBar1.Value = 1
    Return b
End Try
  ```


## 示例

以下示例是 Windows 表单应用程序，它允许用户在 SharePoint 站点上输入信息并以编程方式在受信任位置显示 Excel 工作簿。它以编程方式在指定网站的 default.aspx 页面上创建 Excel Web Access Web 部件，并显示指定的 Excel 工作簿。
  
    
    
代码示例为上述步骤中所述的 Form1.cs 和 Form1.vb 示例文件中的代码。代码示例使用两个文本框、进度栏和按钮。代码只是 Windows 表单项目的一部分。例如，涉及表单布局的代码不会显示。 
  
    
    
 **示例代码提供者：** Daniel Mullowney，Microsoft Corporation
  
    
    



```cs

namespace AddEWATool
{
    using System;
    using System.Windows.Forms;
    using Microsoft.Office.Excel.WebUI;
    using Microsoft.SharePoint;
    using Microsoft.SharePoint.WebPartPages;

    /// <summary>
    /// Form1 class derived from System.Windows.Forms.
    /// </summary>
    public partial class Form1 : Form
    {
        private string appName = "AddEWATool";
        private string specifyInputError = "Please add a site URL, for example: http://myserver/site/";
        private string openSiteError = "There was a problem with the site name. Please check that the site exists.";
        private string addWebPartError = "There was a problem adding the Web Part.";
        private string successMessage = "Web Part successfully added.";

        /// <summary>
        /// Add the Excel Web Access Web Part to the Default.aspx page of the specified site.
        /// </summary>
        /// <param name="siteName">URL of the SharePoint site</param>
        /// <param name="book">URI to the workbook</param>
        /// <returns>Returns true if the WebPart was successfully added; otherwise, false.</returns>
        public bool AddWebPart(string siteName, string book)
        {
            SPSite site = null;
            SPWeb targetWeb = null;
            SPLimitedWebPartManager webPartManager = null;

            bool b = false;
            progressBar1.Visible = true;
            progressBar1.Minimum = 1;
            progressBar1.Maximum = 4;
            progressBar1.Value = 1;
            progressBar1.Step = 1;

            if (String.IsNullOrEmpty(siteName))
            {
                MessageBox.Show(
                    specifyInputError,
                    appName,
                    MessageBoxButtons.OK,
                    MessageBoxIcon.Asterisk);
                return b;
            }

            try
            {
                try
                {
                    site = new SPSite(siteName);
                    targetWeb = site.OpenWeb();
                }
                catch (Exception exc)
                {
                    MessageBox.Show(
                        openSiteError + "\\n" + exc.Message,
                        appName,
                        MessageBoxButtons.OK,
                        MessageBoxIcon.Asterisk);
                    progressBar1.Value = 1;
                    return b;
                }

                progressBar1.PerformStep();

                try
                {
                    // Get the shared Web Part manager on the Default.aspx page.
                    webPartManager = targetWeb.GetLimitedWebPartManager(
                        "Default.aspx",
                        System.Web.UI.WebControls.WebParts.PersonalizationScope.Shared);
                }
                catch (Exception exc)
                {
                    MessageBox.Show(
                        openSiteError + "\\n" + exc.Message,
                        appName,
                        MessageBoxButtons.OK,
                        MessageBoxIcon.Asterisk);
                    progressBar1.Value = 1;
                    return b;
                }

                progressBar1.PerformStep();

                // Instantiate Excel Web Access Web Part.
                // Add an Excel Web Access Web Part in a shared view.
                ExcelWebRenderer ewaWebPart = new ExcelWebRenderer();
                ewaWebPart.WorkbookUri = book;
                progressBar1.PerformStep();

                try
                {
                    webPartManager.AddWebPart(ewaWebPart, "Left", 0);
                }
                catch (Exception exc)
                {
                    MessageBox.Show(
                        addWebPartError + "\\n" + exc.Message,
                        appName,
                        MessageBoxButtons.OK,
                        MessageBoxIcon.Asterisk);
                    progressBar1.Value = 1;
                    return b;
                }
            }
            finally
            {
                if (site != null)
                {
                    site.Dispose();
                }

                if (targetWeb != null)
                {
                    targetWeb.Dispose();
                }

                if (webPartManager != null)
                {
                    webPartManager.Dispose();
                }
            }

            progressBar1.PerformStep();
            b = true;
            return b;
        }

        /// <summary>
        /// AddEWAButton click handler.
        /// </summary>
        /// <param name="sender">caller</param>
        /// <param name="e">event</param>
        private void AddEWAButton_Click(object sender, EventArgs e)
        {
            string siteUrl = textBox1.Text;
            string bookUri = textBox2.Text;
            bool succeeded = AddWebPart(siteUrl, bookUri);
            if (succeeded)
            {
                MessageBox.Show(
                    successMessage,
                    appName,
                    MessageBoxButtons.OK,
                    MessageBoxIcon.Information);
                progressBar1.Value = 1;
            }
        }
    }
}

```




```VB.net

Imports System
Imports System.Windows.Forms
Imports Microsoft.Office.Excel.WebUI
Imports Microsoft.SharePoint
Imports Microsoft.SharePoint.WebPartPages

Namespace AddEWATool
    ''' <summary>
    ''' Form1 class derived from System.Windows.Forms.
    ''' </summary>
    Partial Public Class Form1
        Inherits Form

        Private appName As String = "AddEWATool"
        Private specifyInputError As String = "Please add a site URL, for example, http://myserver/site/"
        Private openSiteError As String = "There was a problem with the site name. Please check that the site exists."
        Private addWebPartError As String = "There was a problem adding the Web Part."
        Private successMessage As String = "Web Part successfully added."

        ''' <summary>
        ''' Add the Excel Web Access Web Part to the Default.aspx page of the specified site.
        ''' </summary>
        ''' <param name="siteName">URL of the SharePoint site</param>
        ''' <param name="book">URI to the workbook</param>
        ''' <returns>Returns true if the WebPart was successfully added; otherwise, false.</returns>
        Public Function AddWebPart(ByVal siteName As String, ByVal book As String) As Boolean
            Dim site As SPSite = Nothing
            Dim targetWeb As SPWeb = Nothing
            Dim webPartManager As SPLimitedWebPartManager = Nothing

            Dim b As Boolean = False
            progressBar1.Visible = True
            progressBar1.Minimum = 1
            progressBar1.Maximum = 4
            progressBar1.Value = 1
            progressBar1.Step = 1

            If String.IsNullOrEmpty(siteName) Then
                MessageBox.Show(specifyInputError, appName, MessageBoxButtons.OK, MessageBoxIcon.Asterisk)
                Return b
            End If

            Try
                Try
                    site = New SPSite(siteName)
                    targetWeb = site.OpenWeb()
                Catch exc As Exception
                    MessageBox.Show(openSiteError &amp; vbLf &amp; exc.Message, appName, MessageBoxButtons.OK, MessageBoxIcon.Asterisk)
                    progressBar1.Value = 1
                    Return b
                End Try

                progressBar1.PerformStep()

                Try
                    ' Get the shared Web Part manager on the Default.aspx page.
                    webPartManager = targetWeb.GetLimitedWebPartManager( _
                            "Default.aspx", _
                            System.Web.UI.WebControls.WebParts.PersonalizationScope.Shared)
                Catch exc As Exception
                    MessageBox.Show(openSiteError &amp; vbLf &amp; exc.Message, appName, MessageBoxButtons.OK, MessageBoxIcon.Asterisk)
                    progressBar1.Value = 1
                    Return b
                End Try

                progressBar1.PerformStep()

                'Instantiate Excel Web Access Web Part.
                'Add an Excel Web Access Web Part in a shared view.
                Dim ewaWebPart As New ExcelWebRenderer()
                ewaWebPart.WorkbookUri = book
                progressBar1.PerformStep()

                Try
                    webPartManager.AddWebPart(ewaWebPart, "Left", 0)
                Catch exc As Exception
                    MessageBox.Show(addWebPartError &amp; vbLf &amp; exc.Message, appName, MessageBoxButtons.OK, MessageBoxIcon.Asterisk)
                    progressBar1.Value = 1
                    Return b
                End Try
            Finally
                If Not IsNothing(site) Then
                    site.Dispose()
                End If

                If Not IsNothing(targetWeb) Then
                    targetWeb.Dispose()
                End If

                If Not IsNothing(webPartManager) Then
                    webPartManager.Dispose()
                End If
            End Try

            progressBar1.PerformStep()
            b = True
            Return b
        End Function

        ''' <summary>
        ''' AddEWAButton click handler.
        ''' </summary>
        ''' <param name="sender">caller</param>
        ''' <param name="e">event</param>
        Private Sub AddEWAButton_Click(ByVal sender As Object, ByVal e As EventArgs)
            Dim siteUrl As String = textBox1.Text
            Dim bookUri As String = textBox2.Text
            Dim succeeded As Boolean = AddWebPart(siteUrl, bookUri)
            If succeeded Then
                MessageBox.Show(successMessage, appName, MessageBoxButtons.OK, MessageBoxIcon.Information)
                progressBar1.Value = 1
            End If
        End Sub
    End Class
End Namespace



```


## 可靠编程

您使用的 Excel 工作簿必须位于受信任位置。
  
    
    

## 另请参阅


#### 任务


  
    
    
 [如何：查找和复制 Microsoft.Office.Excel.WebUI.dll 与 Microsoft.Office.Excel.WebUI.Internal.dll](how-to-locate-and-copy-microsoft-office-excel-webui-dll-and-microsoft-office-exc.md)
#### 概念


  
    
    
 [Excel Services 警告](excel-services-alerts.md)
  
    
    
 [Excel Services 已知问题和提示](excel-services-known-issues-and-tips.md)
