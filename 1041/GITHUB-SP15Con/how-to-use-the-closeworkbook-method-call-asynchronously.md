---
title: 非同期で CloseWorkbook メソッドの呼び出しを使用する方法
keywords: async,how to,howdoi,howto
f1_keywords:
- async,how to,howdoi,howto
ms.prod: SHAREPOINT
ms.assetid: 6febe7dc-a552-4c79-aa3e-203d882286e3
---


# 非同期で CloseWorkbook メソッドの呼び出しを使用する方法

Excel Web Services を使用する場合、セッションが終了したら **CloseWorkbook** メソッドを呼び出してブックを閉じることをお勧めします。こうすることで、セッションがクローズし、Excel Services は予測可能なやり方でリソースを開放できます。これにより、潜在的にサーバーのパフォーマンスと堅牢性が向上します。
  
    
    

ただし、Web サービス呼び出しには時間がかかります。サーバーのインストール方法、サーバーへのアクセス方法、サーバーにかかる負荷の量によって、呼び出しには 50 ミリ秒から 500 ミリ秒かかることがあります。さらに時間がかかることもありますが、それはサーバーの負荷が深刻な場合のみです。 
 **CloseWorkbook** メソッドの呼び出しが失敗しても対処できないので、成功したかどうかを確認するために終了を待つ必要はありません。このため、通常は非同期で呼び出しを実行すれば、操作の時間を節約できます。
  
    
    


> **メモ**
> アプリケーションが Excel Services に対して呼び出しを行ってから終了する場合、ブックを非同期ではなく同期で閉じることが必要な場合があります。この場合は、 **CloseWorkbookAsync** メソッドではなく **CloseWorkbook** メソッドを呼び出します。理由は、非同期の呼び出しを発行した直後にプロセスを終了すると、呼び出しが最後まで行われないことがあるためです。
  
    
    

ブックを非同期で閉じるためには、2 つのことを実行する必要があります。
- 絶対に Excel Web Services プロキシ クラスを破棄しないでください。破棄すると、Excel Services 以外の例外が発生するおそれがあります。 
    
  
- **CloseWorkbook** メソッドではなく **CloseWorkbookAsync** メソッドを呼び出します。 **CloseWorkbookAsync** メソッドのシグネチャは次のとおりです。
    
  ```
  
  public void CloseWorkbookAsync(string sessionId)
  ```


  ```VB.net
  Public Sub CloseWorkbookAsync(ByVal sessionId As String)
  End Sub
  ```

 **CloseWorkbookAsync** メソッドが呼び出される際に呼び出されるイベントは、実装する必要はありません。シグネチャは、プロジェクトの Web References ディレクトリの Reference.cs ファイルにあります。 
> **メモ**
> Microsoft Visual Studio 2005 を使用して Web 参照を追加する際に生成されるプロキシ クラスに **CloseWorkbookAsync** メソッドがあります。Visual Studio 2003 を使用している場合、ブックを非同期で閉じるには、代わりに **BeginCloseWorkbook** メソッドを呼び出します。
  
    
    

 **CloseWorkbookAsync** メソッドまたは **BeginCloseWorkbook** メソッドを呼び出すと、ブックを閉じる呼び出しは非同期で実行されるため、アプリケーションには目立った時間がかからないことを意味します。
## 例

次の例は、Visual Studio 2005 を使用してブックを非同期で閉じる方法を示します。
  
    
    

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


## 堅牢なプログラミング

必ず、アクセスする Excel Web Services サイトへの Web 参照を追加してください。参照する Web サービス サイトを指すように  `using SampleApplication.ExcelWebService;` ステートメントを変更します。
  
    
    
さらに、必要に応じてブックのパス、シート名などを変更します。
  
    
    

## 関連項目


#### タスク


  
    
    
 [例外をキャッチする方法](how-to-catch-exceptions.md)
  
    
    
 [方法: 場所を信頼する](how-to-trust-a-location.md)
  
    
    
 [方法: Excel クライアントからサーバーへの保存](how-to-save-from-excel-client-to-the-server.md)
  
    
    
 [SubCode プロパティを使用してエラー コードをキャプチャする方法](how-to-use-the-subcode-property-to-capture-error-codes.md)
#### 概念


  
    
    
 [SOAP API にアクセスする](accessing-the-soap-api.md)
  
    
    
 [Excel Services のアラート](excel-services-alerts.md)
  
    
    
 [Excel Services の既知の問題およびヒント](excel-services-known-issues-and-tips.md)
  
    
    
 [SOAP 呼び出しのループバックおよび直接リンク](loop-back-soap-calls-and-direct-linking.md)
#### その他の技術情報


  
    
    
 [チュートリアル: Excel Web Services を使用してカスタム アプリケーションを開発する](walkthrough-developing-a-custom-application-using-excel-web-services.md)
