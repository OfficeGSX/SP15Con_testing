---
title: 方法 Web サービスを呼び出す UDF の作成
keywords: how to,howdoi,howto,UDF
f1_keywords:
- how to,howdoi,howto,UDF
ms.prod: SHAREPOINT
ms.assetid: 360c5766-4b5d-4a48-9f23-8955036924ce
---


# 方法: Web サービスを呼び出す UDF の作成

この例では、ユーザー定義関数 (UDF) から外部 Web サービスを呼び出す方法を紹介します。この例で使用する Web サービスは次のとおりです。
  
    
    

 `http://webservices.imacination.com/distance/Distance.jws?wsdl`
このサンプルを作成するには、Microsoft Visual Studio 2005 またはそれに類似した Microsoft .NET Framework 2.0 準拠の開発ツールを使用する必要があります。 
  
    
    


> **メモ**
> コードをテストする前に、呼び出し先の Web サービスが使用可能であることを確認してください。この Web サービス サーバーはダウンする可能性があり、Web サービスが提供中止になることもあります。この Web サービスを使用できない場合は、作成するコードから Web サービスへの呼び出しが失敗します。 <br/> Web サービスが使用可能かどうかを確認するには、そのサービスのサイトにアクセスしてください。この例では、URL は次のとおりです。 <br/>  `http://webservices.imacination.com/distance/Distance.jws?wsdl`<br/> Web サービスが使用可能であれば、Web サービス記述言語 (WSDL) が表示されます。使用可能でなければ、「Web ページが見つかりません」という通常のエラーが表示されます。 
  
    
    


## 例

この例の WSDL を調べると、使用している Web サービスの詳細について知ることができます。
  
    
    
提供している 1 つのサービスは、地理的な座標を 10 進数形式で返すサービスです。このサンプルでは、座標を、表示するのに適した度/分/秒に変換する方法を示す  `ToDegreeNotation` 関数が追加されています。
  
    
    



```cs

[UdfMethod]
public string ToDegreeNotation(double angle)
{
    int deg = (int)angle;
    double minutesAndSeconds = Math.Abs(angle - deg) * 60;
    int minutes = (int)minutesAndSeconds;
    int seconds = (int)(Math.Abs(minutesAndSeconds - minutes) * 60);

    return deg.ToString() + "°" + minutes.ToString() + "\\'" + 
        seconds.ToString() + "\\"";
}
```




```VB.net

<UdfMethod> _
Public Function ToDegreeNotation(ByVal angle As Double) As String
    Dim deg As Integer = CInt(Fix(angle))
    Dim minutesAndSeconds As Double = Math.Abs(angle - deg) * 60
    Dim minutes As Integer = CInt(Fix(minutesAndSeconds))
    Dim seconds As Integer = CInt(Fix(Math.Abs(minutesAndSeconds - minutes) * 60))

    Return deg.ToString() &amp; "°" &amp; minutes.ToString() &amp; "'" &amp; seconds.ToString() &amp; """"
End Function
```

Internet Explorer の LAN 設定でプロキシ サーバーを使用するように構成されている場合は、プロキシ サーバーを設定するための呼び出しをコードの中で明示的に実行しなければなりません。そうしない場合、Web サービスの呼び出しは失敗します。プロキシ サーバーの設定は、次のようにコンストラクターの中で実行できます。
  
    
    



```cs

namespace ZipCodeUdfSample
{
    [UdfClass]
    public class ZipCodeUdfs
    {
        DistanceService distanceService = null;
        public ZipCodeUdfs()
        {
            this.distanceService = new DistanceService();
            this.distanceService.Proxy = 
                new WebProxy("http://myproxy:80", true);
        }
```




```VB.net

Namespace ZipCodeUdfSample
    <UdfClass> _
    Public Class ZipCodeUdfs
        Private distanceService As DistanceService = Nothing

        Public Sub New()
            Me.distanceService = New DistanceService()
            'this.distanceService.Proxy = new WebProxy("http://myproxy:80", true);
        End Sub
```

セルから UDF のテストと呼び出しを行う方法の詳細については、「 [チュートリアル: マネージ コード UDF を開発する](walkthrough-developing-a-managed-code-udf.md)」を参照してください。
  
    
    



```cs

using System;
using System.Collections.Generic;
using System.Text;
using System.Net;
using Microsoft.Office.Excel.Server.Udf;
using ZipCodes.com.imacination.webservices;

namespace ZipCodeUdfSample
{
    [UdfClass]
    public class ZipCodeUdfs
    {
        DistanceService distanceService = null;

        public ZipCodeUdfs()
        {
            this.distanceService = new DistanceService();
            //this.distanceService.Proxy = new WebProxy("http://myproxy:80", true);
        }

        [UdfMethod]
        public double GetDistanceBetweenTwoZipCodes(int zip1, int zip2)
        {
            string zip1String = Convert.ToString(zip1);
            string zip2String = Convert.ToString(zip2);

            return (distanceService.getDistance(zip1String, zip2String));
        }

        [UdfMethod]
        public string GetCityFromZip(int zip)
        {
            string zipString = Convert.ToString(zip);

            return (distanceService.getCity(zipString));
        }

        [UdfMethod]
        public string GetStateFromZip(int zip)
        {
            string zipString = Convert.ToString(zip);

            return (distanceService.getState(zipString));
        }

        [UdfMethod]
        public string GetLocationFromZip(int zip)
        {
            string zipString = Convert.ToString(zip);

            return (distanceService.getLocation(zipString));
        }

        [UdfMethod]
        public double GetLatitudeFromZip(int zip)
        {
            string zipString = Convert.ToString(zip);

            return (distanceService.getLatitude(zipString));
        }

        [UdfMethod]
        public double GetLongitudeFromZip(int zip)
        {
            string zipString = Convert.ToString(zip);

            return (distanceService.getLongitude(zipString));
        }

        [UdfMethod]
        public string ToDegreeNotation(double angle)
        {
            int deg = (int)angle;
            double minutesAndSeconds = Math.Abs(angle - deg) * 60;
            int minutes = (int)minutesAndSeconds;
            int seconds = (int)(Math.Abs(minutesAndSeconds - minutes) * 60);

            return deg.ToString() + "°" + minutes.ToString() + "\\'" + seconds.ToString() + "\"";
        }
    }
}
```




```VB.net

Imports System
Imports System.Collections.Generic
Imports System.Text
Imports System.Net
Imports Microsoft.Office.Excel.Server.Udf
Imports ZipCodes.com.imacination.webservices

Namespace ZipCodeUdfSample
    <UdfClass> _
    Public Class ZipCodeUdfs
        Private distanceService As DistanceService = Nothing

        Public Sub New()
            Me.distanceService = New DistanceService()
            'this.distanceService.Proxy = new WebProxy("http://myproxy:80", true);
        End Sub

        <UdfMethod> _
        Public Function GetDistanceBetweenTwoZipCodes(ByVal zip1 As Integer, ByVal zip2 As Integer) As Double
            Dim zip1String As String = Convert.ToString(zip1)
            Dim zip2String As String = Convert.ToString(zip2)

            Return (distanceService.getDistance(zip1String, zip2String))
        End Function

        <UdfMethod> _
        Public Function GetCityFromZip(ByVal zip As Integer) As String
            Dim zipString As String = Convert.ToString(zip)

            Return (distanceService.getCity(zipString))
        End Function

        <UdfMethod> _
        Public Function GetStateFromZip(ByVal zip As Integer) As String
            Dim zipString As String = Convert.ToString(zip)

            Return (distanceService.getState(zipString))
        End Function

        <UdfMethod> _
        Public Function GetLocationFromZip(ByVal zip As Integer) As String
            Dim zipString As String = Convert.ToString(zip)

            Return (distanceService.getLocation(zipString))
        End Function

        <UdfMethod> _
        Public Function GetLatitudeFromZip(ByVal zip As Integer) As Double
            Dim zipString As String = Convert.ToString(zip)

            Return (distanceService.getLatitude(zipString))
        End Function

        <UdfMethod> _
        Public Function GetLongitudeFromZip(ByVal zip As Integer) As Double
            Dim zipString As String = Convert.ToString(zip)

            Return (distanceService.getLongitude(zipString))
        End Function

        <UdfMethod> _
        Public Function ToDegreeNotation(ByVal angle As Double) As String
            Dim deg As Integer = CInt(Fix(angle))
            Dim minutesAndSeconds As Double = Math.Abs(angle - deg) * 60
            Dim minutes As Integer = CInt(Fix(minutesAndSeconds))
            Dim seconds As Integer = CInt(Fix(Math.Abs(minutesAndSeconds - minutes) * 60))

            Return deg.ToString() &amp; "°" &amp; minutes.ToString() &amp; "'" &amp; seconds.ToString() &amp; """"
        End Function
    End Class
End Namespace
```


## 関連項目


#### タスク


  
    
    
 [ステップ 1: プロジェクトを作成し、UDF への参照を追加する](step-1-creating-a-project-and-adding-a-udf-reference.md)
  
    
    
 [ステップ 2: マネージ コード UDF を作成する](step-2-creating-a-managed-code-udf.md)
  
    
    
 [ステップ 3: UDF を展開して有効にする](step-3-deploying-and-enabling-udfs.md)
  
    
    
 [手順 4: セルから UDF のテストと呼び出しを行う](step-4-testing-and-calling-udfs-from-cells.md)
#### 概念


  
    
    
 [SOAP API にアクセスする](accessing-the-soap-api.md)
#### その他の技術情報


  
    
    
 [手順 2: Web 参照を追加する](step-2-adding-a-web-reference.md)
  
    
    
 [手順 3: Web サービスへのアクセス](step-3-accessing-the-web-service.md)
  
    
    
 [チュートリアル: Excel Web Services を使用してカスタム アプリケーションを開発する](walkthrough-developing-a-custom-application-using-excel-web-services.md)
