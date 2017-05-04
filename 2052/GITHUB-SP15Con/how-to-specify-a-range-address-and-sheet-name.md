---
title: 如何：指定范围地址和工作表名称
keywords: how to,howdoi,howto,set range
f1_keywords:
- how to,howdoi,howto,set range
ms.prod: SHAREPOINT
ms.assetid: 8bfefc48-1fbc-4b65-8156-1b7d0a8453ee
---


# 如何：指定范围地址和工作表名称

本示例说明如何使用范围坐标、指定范围、行和列指定范围地址，另外还说明了如何指定工作表名称以及工作表名称和范围地址之间的关系。
  
    
    

范围坐标是由四个整数组成的坐标，用于选择连续范围。范围坐标使您可以通过使用直接整数索引替代"A1"表达式来指定 Excel 范围。您可以指定的坐标包括顶行、左列、高度和宽度。当您的代码在一个循环中遍历单元格集时，或者当范围坐标作为算法的一部分动态计算时，使用范围坐标更简单。
范围规范必须包含工作表名称；Excel Web Services 不会识别"当前工作表"。有几种方法来指定工作表名称： 
  
    
    


- 作为范围地址的一部分例如，"Sheet3!B12:D18"这种情况下工作表名称参数可能为空：
    
  ```cs
  
  object[] rangeResult1 = xlservice.GetRangeA1(sessionId, String.Empty, "Sheet2!A12:G18", true, out outStatus);
  ```


  ```VB.net
  Dim rangeResult1() As Object = xlservice.GetRangeA1(sessionId, String.Empty, "Sheet2!A12:G18", True, outStatus)
  ```

- 在单独的工作表名称参数中，这种情况下范围地址参数不需要包含工作表名称：
    
  ```cs
  xlservice.SetCell(sessionId, "Sheet3", 0, 11, 1000);
  ```


  ```VB.net
  xlservice.SetCell(sessionId, "Sheet3", 0, 11, 1000)
  ```

- 在工作表名称和范围地址中，这种情况下工作表名称必须匹配：
    
  ```cs
  object[] rangeResult = xlservice.GetCellA1(sessionId, "Sheet3", "Sheet3!G18", true, out outStatus);
  ```


  ```VB.net
  Dim rangeResult() As Object = xlservice.GetCellA1(sessionId, "Sheet3", "Sheet3!G18", True, outStatus)
  ```

唯一不需要工作表名称的情况是指定范围，因为有些指定范围具有工作簿作用域。例如，您可以参考指定范围，而无需指定工作表名称参数：


```cs
xlServices.SetCellA1(sessionId, String.Empty, "MyNamedRange", 8);
```




```VB.net
xlServices.SetCellA1(sessionId, String.Empty, "MyNamedRange", 8)
```

如果您指定工作表名称，您参考的范围必须存在于您指定的工作表中。如果您指定的工作表不存在，调用将失败，您将收到简单对象访问协议 (SOAP) 异常，指示工作表不存在。
## 示例


> **注释**
> 假定您已经创建 SharePoint 文档库并将其设为受信任的位置。有关详细信息，请参阅 [如何：信任位置](how-to-trust-a-location.md)和 [如何：使用脚本信任工作簿位置](http://msdn.microsoft.com/library/79ab6ced-7a0c-4275-b852-bb246fc6be57%28Office.15%29.aspx)。 
  
    
    


```cs
using System;
using System.Text;
using System.Web.Services.Protocols;
using ExcelWebService.myserver02;
namespace ExcelWebService
{
/// <summary>
/// Summary description for Class1.
/// </summary>
    class MyExcelWebService
    {
        [STAThread]
        static void Main(string[] args)
        {
            // Instantiate the Web service 
            // and range coordinate array object.
            ExcelService xlservice = new ExcelService();
            Status[] outStatus;
            RangeCoordinates rangeCoordinates = new RangeCoordinates();
            string sheetName = "MySheet1";

            // TODO: Change the path to the workbook
            // to point to a workbook you have access to.
            // The workbook must be in a trusted location.
            // Using the workbook path this way will allow 
            // you to call the workbook remotely.
            string targetWorkbookPath = 
       "http://myserver02/example/Shared%20Documents/MyWorkbook1.xlsx";

            // Set Credentials for requests
            xlservice.Credentials = 
                System.Net.CredentialCache.DefaultCredentials;

            try
            {
                // Call the open workbook, and point to    
                // the workbook to open.
                string sessionId = 
                    xlservice.OpenWorkbook(targetWorkbookPath, 
                        String.Empty, String.Empty, out outStatus);
                // Prepare object to define range coordinates
                // and call the GetRange method.
                // startCol, startRow, startHeight, and startWidth
                // get their values from user input.
                rangeCoordinates.Column = (int)startCol.Value;
                rangeCoordinates.Row = (int)startRow.Value;
                rangeCoordinates.Height = (int)startHeight.Value;
                rangeCoordinates.Width = (int)startWidth.Value;

                object[] rangeResult1 = xlservice.GetRange(sessionId, 
                    sheetName, rangeCoordinates, false, out outStatus);
                Console.WriteLine("Total rows in range: " + 
                    rangeResult1.Length);
                Console.WriteLine("Sum in last column is: " + 
                    ((object[])rangeResult1[18])[11]);

                // Call the SetCell method, which invokes 
                // the Calculate method.
                // Set first row in last column cell to 1000.
                xlservice.SetCell(sessionId, sheetName, 0, 11, 1000);

                // Call the GetRange method again to see if 
                // the Sum total in the last column changed.
                object[] rangeResult2 = xlservice.GetRange(sessionId, 
                    sheetName, rangeCoordinates, false, out outStatus);    
                Console.WriteLine("Sum in the last column after SetCell 
                    is: " + ((object[])rangeResult2[18])[11]); 

                // Close workbook. This also closes the session.
                xlservice.CloseWorkbook(sessionId);
            }

            catch (SoapException e)
            {
                Console.WriteLine("Exception Message: {0}", e.Message);
            }
            catch (Exception e)
            {
                Console.WriteLine("Exception Message: {0}", e.Message);
            }
            Console.ReadLine();
        }
    }
}
```


```VB.net

Imports System
Imports System.Text
Imports System.Web.Services.Protocols
Imports ExcelWebService.myserver02
Namespace ExcelWebService
''' <summary>
''' Summary description for Class1.
''' </summary>
    Friend Class MyExcelWebService
        <STAThread> _
        Shared Sub Main(ByVal args() As String)
            ' Instantiate the Web service 
            ' and range coordinate array object.
            Dim xlservice As New ExcelService()
            Dim outStatus() As Status
            Dim rangeCoordinates As New RangeCoordinates()
            Dim sheetName As String = "MySheet1"

            ' TODO: Change the path to the workbook
            ' to point to a workbook you have access to.
            ' The workbook must be in a trusted location.
            ' Using the workbook path this way will allow 
            ' you to call the workbook remotely.
            Dim targetWorkbookPath As String = "http://myserver02/example/Shared%20Documents/MyWorkbook1.xlsx"

            ' Set Credentials for requests
            xlservice.Credentials = System.Net.CredentialCache.DefaultCredentials

            Try
                ' Call the open workbook, and point to    
                ' the workbook to open.
                Dim sessionId As String = xlservice.OpenWorkbook(targetWorkbookPath, String.Empty, String.Empty, outStatus)
                ' Prepare object to define range coordinates
                ' and call the GetRange method.
                ' startCol, startRow, startHeight, and startWidth
                ' get their values from user input.
                rangeCoordinates.Column = CInt(Fix(startCol.Value))
                rangeCoordinates.Row = CInt(Fix(startRow.Value))
                rangeCoordinates.Height = CInt(Fix(startHeight.Value))
                rangeCoordinates.Width = CInt(Fix(startWidth.Value))

                Dim rangeResult1() As Object = xlservice.GetRange(sessionId, sheetName, rangeCoordinates, False, outStatus)
                Console.WriteLine("Total rows in range: " &amp; rangeResult1.Length)
                Console.WriteLine("Sum in last column is: " &amp; (CType(rangeResult1(18), Object()))(11))

                ' Call the SetCell method, which invokes 
                ' the Calculate method.
                ' Set first row in last column cell to 1000.
                xlservice.SetCell(sessionId, sheetName, 0, 11, 1000)

                ' Call the GetRange method again to see if 
                ' the Sum total in the last column changed.
                Dim rangeResult2() As Object = xlservice.GetRange(sessionId, sheetName, rangeCoordinates, False, outStatus)
                Console.WriteLine("Sum in the last column after SetCell is: " &amp; (CType(rangeResult2(18), Object()))(11))

                ' Close workbook. This also closes the session.
                xlservice.CloseWorkbook(sessionId)

            Catch e As SoapException
                Console.WriteLine("Exception Message: {0}", e.Message)
            Catch e As Exception
                Console.WriteLine("Exception Message: {0}", e.Message)
            End Try
            Console.ReadLine()
        End Sub
    End Class
End Namespace
```


## 可靠编程

确保您添加了有权访问的 Excel Web Services 网站的 Web 引用。更改以下内容：
  
    
    

- 将  `using ExcelWebService.myserver02;` 语句更改为指向您引用的 Web 服务站点。
    
  
- 将  `string targetWorkbookPath = "http://myserver02/example/Shared%20Documents/Book1.xlsx";` 更改为指向您有权访问的工作簿。工作簿必须位于受信任位置。
    
  

## 另请参阅


#### 任务


  
    
    
 [如何：从范围获取值](how-to-get-values-from-ranges.md)
  
    
    
 [如何：设置范围的值](how-to-set-values-of-ranges.md)
#### 概念


  
    
    
 [访问 SOAP API](accessing-the-soap-api.md)
#### 其他资源


  
    
    
 [演练：使用 Excel Web Services 开发自定义应用程序](walkthrough-developing-a-custom-application-using-excel-web-services.md)
