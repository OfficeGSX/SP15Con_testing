---
title: プログラムを使用して Excel Web Access の Web パーツをページに追加する方法
keywords: how to,howdoi,howto,webpart
f1_keywords:
- how to,howdoi,howto,webpart
ms.prod: SHAREPOINT
ms.assetid: 858bb0f6-654a-4f12-ba0b-4776bda5ff6d
---


# プログラムを使用して Excel Web Access の Web パーツをページに追加する方法

次の使用例は、プログラムを使用して Excel Web Access Web パーツを SharePoint ページに追加する方法を示しています。また、プログラムを使用して Excel Web Access Web パーツに Excel ブックを表示する方法も示します。 
  
    
    

次のプロジェクトでは Microsoft Visual Studio を使用します。
> **メモ**
> 使用する Visual Studio のバージョンと Visual Studio の統合開発環境 (IDE) 設定によって、Visual Studio プロジェクトを作成するためのプロセスと手順は、このトピックに示す手順と若干異なる可能性があります。 
  
    
    


> **メモ**
> SharePoint ドキュメント ライブラリが既に作成され、信頼できる場所になっていることが前提です。詳細は、「 [方法: 場所を信頼する](how-to-trust-a-location.md)」を参照してください。 
  
    
    


## 参照の追加

Microsoft.Office.Excel.WebUI.dll の検索方法および参照の追加方法を次の手順に示します。Microsoft.Office.Excel.WebUI.Internal.dll と Microsoft.SharePoint.dll にもこれを繰り返してください。
  
    
    

> **メモ**
> Microsoft.Office.Excel.WebUI.dll および Microsoft.Office.Excel.WebUI.Internal.dll が、既にグローバル アセンブリ キャッシュからお好みのフォルダーにコピーされていることが前提になっています。Microsoft.Office.Excel.WebUI.dll および Microsoft.Office.Excel.WebUI.Internal.dll の検索方法およびコピー方法について、詳細は「 [方法: Microsoft.Office.Excel.WebUI.dll および Microsoft.Office.Excel.WebUI.Internal.dll を検索してコピーする](how-to-locate-and-copy-microsoft-office-excel-webui-dll-and-microsoft-office-exc.md)」を参照してください。 
  
    
    


### Microsoft.Office.Excel.WebUI.dll に参照を追加するには


1. [ **プロジェクト**] メニューで、[ **参照の追加**] をクリックします。
    
  
2. [ **参照の追加**] ダイアログ ボックスで、[ **参照**] をクリックします。
    
    > **メモ**
      > また、[ **ソリューション エクスプローラー**] ウィンドウにある [ **参照の追加**] ダイアログ ボックスを、[ **参照**] を右クリックした後 [ **参照の追加**] を選択して開くこともできます。 
3. Microsoft.Office.Excel.WebUI.dll の場所を参照します。
    
  
4. Microsoft.Office.Excel.WebUI.dll を選択してから [ **OK**] をクリックします。
    
  
5. [ **参照の追加**] をクリックします。Microsoft.Office.Excel.WebUI.dll への参照がプロジェクトに追加されます。
    
  

## Web パーツのインスタンスの作成


### Excel Web Access Web パーツのインスタンスを作成するには


1. この名前空間の型を使用する際、完全修飾しなくてもよいように、Microsoft.Office.Excel.WebUI 名前空間をディレクティブとしてコードに追加します。
    
  ```cs
  
using Microsoft.Office.Excel.WebUI;
  ```


  ```VB.net
  Imports Microsoft.Office.Excel.WebUI
  ```

2. 次のように、Excel Web Access Web パーツのインスタンスを作成して初期化します。
    
  ```cs
  
 ExcelWebRenderer ewaWebPart = new ExcelWebRenderer();
  ```


  ```VB.net
  
Dim ewaWebPart As New ExcelWebRenderer()
  ```


### プログラムを使用してブックを表示するには


1. この例では、 **AddWebPart** メソッドは、パスを引数として Excel ブックの場所に取り込みます。ユーザーは、Windows フォームのテキスト ボックスに入力し、ボタンをクリックして、パスを指定します。
    
    **サンプル コードの提供者:** Daniel Mullowney (Microsoft Corporation)
    


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


> **重要**
> ブックが保存されている場所が信頼できる場所であることを確認します。 
2. 次のコードを使用して Excel ブックをプログラムで表示できます。
    
    **サンプル コードの提供者:** Daniel Mullowney (Microsoft Corporation)
    


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


## 例

次の使用例は、ユーザーがプログラムを使用して SharePoint サイトに情報を入力し、信頼できる場所に保存された Excel ブックを表示できるようにする Windows フォーム アプリケーションです。このアプリケーションは、プログラムを使用して指定されたサイトの default.aspx ページに Excel Web Access Web パーツを作成し、指定された Excel ブックを表示します。
  
    
    
コードのサンプルは、前の手順で説明した使用例のファイル Form1.cs および Form1.vb のコードです。サンプル コードでは、2 つのテキスト ボックス、進行状況バー、およびボタンを使用します。コードは、Windows フォーム プロジェクトの一部に過ぎません。たとえば、フォームのレイアウトに関連するコードは示されていません。 
  
    
    
 **サンプル コードの提供者:** Daniel Mullowney (Microsoft Corporation)
  
    
    



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


## 堅牢なプログラミング

ご使用の Excel ブックは信頼できる場所にある必要があります。
  
    
    

## 関連項目


#### タスク


  
    
    
 [方法: Microsoft.Office.Excel.WebUI.dll および Microsoft.Office.Excel.WebUI.Internal.dll を検索してコピーする](how-to-locate-and-copy-microsoft-office-excel-webui-dll-and-microsoft-office-exc.md)
#### 概念


  
    
    
 [Excel Services のアラート](excel-services-alerts.md)
  
    
    
 [Excel Services の既知の問題およびヒント](excel-services-known-issues-and-tips.md)
