---
title: 範囲のアドレスとシート名を指定する方法
keywords: how to,howdoi,howto,set range
f1_keywords:
- how to,howdoi,howto,set range
ms.prod: SHAREPOINT
ms.assetid: 8bfefc48-1fbc-4b65-8156-1b7d0a8453ee
---


# 範囲のアドレスとシート名を指定する方法

次の使用例は、範囲座標、名前付き範囲、行、列を使用して範囲のアドレスを指定する方法を示しています。また、シート名を指定する方法、およびシート名と範囲のアドレスとの関係も示します。
  
    
    

範囲座標は、連続した範囲の選択に使用される 4 つの整数座標です。範囲座標では、「A1」表記の代わりとしてインデックスを作成する直接の整数を使用して Excel の範囲を指定することができます。座標で指定できるのは、最上行、左列、高さ、幅です。ループ内で一連のセルを反復処理するコードや、範囲座標がアルゴリズムの一部として動的に計算される場合、範囲座標の方が簡単に使えます。
範囲指定には、シート名が含まれる必要があります。Excel Web Services は「現在のシート」を認識しません。シート名を指定する方法はいくつかあります。 
  
    
    


- セル範囲の一部として。たとえば "Sheet3!B12:D18" の場合、シート名の引数は空にできます。
    
  ```cs
  
  object[] rangeResult1 = xlservice.GetRangeA1(sessionId, String.Empty, "Sheet2!A12:G18", true, out outStatus);
  ```


  ```VB.net
  Dim rangeResult1() As Object = xlservice.GetRangeA1(sessionId, String.Empty, "Sheet2!A12:G18", True, outStatus)
  ```

- 別個のシート名引数として。この場合、範囲のアドレスの引数にシート名が含まれる必要はありません。:
    
  ```cs
  xlservice.SetCell(sessionId, "Sheet3", 0, 11, 1000);
  ```


  ```VB.net
  xlservice.SetCell(sessionId, "Sheet3", 0, 11, 1000)
  ```

- シート名と範囲のアドレスの両方で指定することもできますが、この場合、シート名は一致する必要があります。
    
  ```cs
  object[] rangeResult = xlservice.GetCellA1(sessionId, "Sheet3", "Sheet3!G18", true, out outStatus);
  ```


  ```VB.net
  Dim rangeResult() As Object = xlservice.GetCellA1(sessionId, "Sheet3", "Sheet3!G18", True, outStatus)
  ```

シート名が不要な唯一の場合は名前付き範囲です。一部の名前付き範囲にはブックのスコープがあるためです。たとえば、次の例のようにシート名引数を指定せずに名前付き範囲を参照できます。


```cs
xlServices.SetCellA1(sessionId, String.Empty, "MyNamedRange", 8);
```




```VB.net
xlServices.SetCellA1(sessionId, String.Empty, "MyNamedRange", 8)
```

シート名を指定する場合、参照する範囲が指定するシートに存在している必要があります。存在していないシートを指定すると、呼び出しが失敗して、シートが存在しないという簡易オブジェクト アクセス プロトコル (SOAP) の例外が発生します。
## 例


> **メモ**
> 既に SharePoint ドキュメント ライブラリが作成され、信頼できる場所になっていることが前提です。詳細は、「 [方法: 場所を信頼する](how-to-trust-a-location.md)」および「 [[方法] スクリプトを使用してブックの場所を信頼する](http://msdn.microsoft.com/library/79ab6ced-7a0c-4275-b852-bb246fc6be57%28Office.15%29.aspx)」を参照してください。 
  
    
    


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


## 堅牢なプログラミング

アクセスできる Excel Web Services サイトに Web 参照が追加されていることを確認してください。次の変更を行います。
  
    
    

- 参照する Web サービス サイトを指すように  `using ExcelWebService.myserver02;` ステートメントを変更します。
    
  
- アクセスできるブックを指すように  `string targetWorkbookPath = "http://myserver02/example/Shared%20Documents/Book1.xlsx";` を変更します。ブックは信頼できる場所にある必要があります。
    
  

## 関連項目


#### タスク


  
    
    
 [範囲から値を取得する方法](how-to-get-values-from-ranges.md)
  
    
    
 [範囲の値を設定する方法](how-to-set-values-of-ranges.md)
#### 概念


  
    
    
 [SOAP API にアクセスする](accessing-the-soap-api.md)
#### その他の技術情報


  
    
    
 [チュートリアル: Excel Web Services を使用してカスタム アプリケーションを開発する](walkthrough-developing-a-custom-application-using-excel-web-services.md)
