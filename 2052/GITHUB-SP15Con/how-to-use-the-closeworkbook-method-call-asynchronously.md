---
title: 如何：异步使用 CloseWorkbook 方法调用
keywords: async,how to,howdoi,howto
f1_keywords:
- async,how to,howdoi,howto
ms.prod: OFFICE365
ms.assetid: 6febe7dc-a552-4c79-aa3e-203d882286e3
---


# 如何：异步使用 CloseWorkbook 方法调用

使用 Excel Web Services 时，一种好的做法是在结束会话时，通过调用 **CloseWorkbook** 方法关闭工作簿。这将关闭会话，并允许 Excel Services 以一种可预测的方式释放资源。这可以提高您的服务器性能和可靠性。
  
    
    

但是，任何 Web 服务调用都需要时间。根据您的服务器的安装方式、您访问它的方式以及服务器所承受的压力，调用可能需要 50 到 500 毫秒，也可能更长，但除非是您的服务器处于严重压力之下时。 
如果 **CloseWorkbook** 方法调用失败，您无法采取任何措施，因此您无需等待它完成来查看其是否成功。因此，通常可以异步方式进行调用并节省一些操作时间。
  
    
    


> **注释**
> 如果您的应用程序对 Excel Services 进行一些调用然后退出，您可能希望同步关闭工作簿而非异步。在这种情况下，您应调用 **CloseWorkbook** 方法，而非 **CloseWorkbookAsync** 方法。原因是，如果您在发出异步调用后立即退出此过程，调用很可能无法完成。
  
    
    

要异步关闭工作簿，必须执行以下两项操作：
- 确保您没有释放 Excel Web Services 代理类如果您释放了，可能会发生非 Excel Services 异常。 
    
  
- 调用 **CloseWorkbookAsync** 方法，而非 **CloseWorkbook** 方法。 **CloseWorkbookAsync** 方法的签名为：
    
  ```
  
  public void CloseWorkbookAsync(string sessionId)
  ```


  ```VB.net
  Public Sub CloseWorkbookAsync(ByVal sessionId As String)
  End Sub
  ```

您无需实施在调用 **CloseWorkbookAsync** 方法时调用的事件。您可以在项目的"Web References"目录下的"Reference.cs"文件中找到签名。 
> **注释**
> 您可以在使用 Microsoft Visual Studio 2005 添加 Web 引用时生成的代理类中查找 **CloseWorkbookAsync** 方法。如果您使用的是 Visual Studio 2003，您可以调用 **BeginCloseWorkbook** 方法，转为以异步方式关闭工作簿。
  
    
    

调用 **CloseWorkbookAsync** 方法或 **BeginCloseWorkbook** 方法意味着将以异步方式执行关闭工作簿的调用，且不会占用您的应用程序太多时间。
## 示例

以下示例说明如何使用 Visual Studio 2005 以异步方式关闭工作簿。
  
    
    

```cs

using System;
using SampleApplication.ExcelWebService;
using System.Web.Services.Protocols;
namespace SampleApplication
{
    class Class1
    {
        [STAThread]
        static void Main(string[] args)
        {            
            // Instantiate the Web service 
            // and create a status array object.
            ExcelService es = new ExcelService();
            Status[] outStatus;

            string sheetName = "Sheet1";
            // TODO: change the workbook path to 
            // point to workbook in a trusted location
            // that you have access to. 
            string targetWorkbookPath = 
             "http://myserver02/example/Shared%20Documents/Book1.xlsx";

            // Set credentials for requests.
            es.Credentials = 
                System.Net.CredentialCache.DefaultCredentials;
            
            try
            {
                // Call open workbook, and point to the trusted   
                // location of the workbook to open.
                string sessionId = es.OpenWorkbook(targetWorkbookPath, 
                    "en-US", "en-US", out outStatus);
                // Call the GetCell method 
                // to retrieve a value from a cell.
                // The cell is in the first row and ninth column.
                object[] rangeResult2 = xlservice.GetCell(sessionId, 
                    sheetName, 0, 8, false, out outStatus);
 
                // Close the workbook asynchronously. 
                // This also closes session.
                es.CloseWorkbookAsync(sessionId);
            }
            catch (SoapException e)
            {
                Console.WriteLine("SOAP Exception Message: {0}", 
                   e.Message);
                Console.WriteLine("SOAP Exception Error Code: {0}", 
                   e.SubCode.Code.Name);
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception Message: {0}", e.Message);
            }
            // Console.ReadLine();
        }
    }
}
 
```


```VB.net

Imports System
Imports SampleApplication.ExcelWebService
Imports System.Web.Services.Protocols
Namespace SampleApplication
    Friend Class Class1
        <STAThread> _
        Shared Sub Main(ByVal args() As String)
            ' Instantiate the Web service 
            ' and create a status array object.
            Dim es As New ExcelService()
            Dim outStatus() As Status

            Dim sheetName As String = "Sheet1"
            ' TODO: change the workbook path to 
            ' point to workbook in a trusted location
            ' that you have access to. 
            Dim targetWorkbookPath As String = "http://myserver02/example/Shared%20Documents/Book1.xlsx"

            ' Set credentials for requests.
            es.Credentials = System.Net.CredentialCache.DefaultCredentials

            Try
                ' Call open workbook, and point to the trusted   
                ' location of the workbook to open.
                Dim sessionId As String = es.OpenWorkbook(targetWorkbookPath, "en-US", "en-US", outStatus)
                ' Call the GetCell method 
                ' to retrieve a value from a cell.
                ' The cell is in the first row and ninth column.
                Dim rangeResult2() As Object = xlservice.GetCell(sessionId, sheetName, 0, 8, False, outStatus)

                ' Close the workbook asynchronously. 
                ' This also closes session.
                es.CloseWorkbookAsync(sessionId)
            Catch e As SoapException
                Console.WriteLine("SOAP Exception Message: {0}", e.Message)
                Console.WriteLine("SOAP Exception Error Code: {0}", e.SubCode.Code.Name)
            Catch e As Exception
                Console.WriteLine("Exception Message: {0}", e.Message)
            End Try
            ' Console.ReadLine();
        End Sub
    End Class
End Namespace
```


## 可靠编程

确保您添加了有权访问的 Excel Web Services 网站的 Web 引用。将  `using SampleApplication.ExcelWebService;` 语句更改为指向您引用的 Web 服务站点。
  
    
    
此外，根据需要对工作簿路径、工作表名称等进行更改。
  
    
    

## 另请参阅


#### 任务


  
    
    
 [如何：捕获异常](how-to-catch-exceptions.md)
  
    
    
 [如何：信任位置](how-to-trust-a-location.md)
  
    
    
 [如何：从 Excel 客户端保存到服务器](how-to-save-from-excel-client-to-the-server.md)
  
    
    
 [如何：使用 SubCode 属性捕获错误代码](how-to-use-the-subcode-property-to-capture-error-codes.md)
#### 概念


  
    
    
 [访问 SOAP API](accessing-the-soap-api.md)
  
    
    
 [Excel Services 警告](excel-services-alerts.md)
  
    
    
 [Excel Services 已知问题和提示](excel-services-known-issues-and-tips.md)
  
    
    
 [环回 SOAP 调用和直接链接](loop-back-soap-calls-and-direct-linking.md)
#### 其他资源


  
    
    
 [演练：使用 Excel Web Services 开发自定义应用程序](walkthrough-developing-a-custom-application-using-excel-web-services.md)
