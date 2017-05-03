---
title: 如何：创建调用 Web 服务的 UDF
keywords: how to,howdoi,howto,UDF
f1_keywords:
- how to,howdoi,howto,UDF
ms.prod: SHAREPOINT
ms.assetid: 360c5766-4b5d-4a48-9f23-8955036924ce
---


# 如何：创建调用 Web 服务的 UDF

本示例说明如何从用户定义函数调用外部 Web 服务。本示例中使用的 Web 服务为：
  
    
    

 `http://webservices.imacination.com/distance/Distance.jws?wsdl`
您必须使用 Microsoft Visual Studio 2005 或者与 Microsoft .NET Framework 2.0 兼容的相似开发工具来创建此示例。 
  
    
    


> **注释**
> 测试代码之前，请确保您调用的 Web 服务可用。Web 服务服务器可能已停机或者 Web 服务已终止。如果 Web 服务不可用，从代码向 Web 服务发起的调用将失败。 <br/> 您可以访问 Web 服务的网站，检查其是否可用。在此示例中，URL 为： <br/>  `http://webservices.imacination.com/distance/Distance.jws?wsdl`<br/> 如果 Web 服务可用，您将能够查看 Web Services 描述语言 (WSDL)。如果不可用，您通常将收到"找不到网页"的错误。 
  
    
    


## 示例

您可以通过检查 Web 服务的 WSDL 获取在此示例中所使用的 Web 服务的详细信息。
  
    
    
它提供的一项服务是以十进制形式返回地理坐标，在本示例中，已添加  `ToDegreeNotation` 函数以说明如何将坐标转换为度/分钟/秒，这对于显示坐标更为合适。
  
    
    



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

如果您的 Internet Explorer LAN 设置配置为使用代理服务器，您的代码必须明确调用代理服务器集。否则，您的 Web 服务调用将失败。您可以在构建器中设置代理服务器，如下所示：
  
    
    



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

有关如何测试 UDF 以及从单元格调用 UDF 的详细信息，请参阅 [演练：开发托管代码 UDF](walkthrough-developing-a-managed-code-udf.md)。
  
    
    



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


## 另请参阅


#### 任务


  
    
    
 [步骤 1：创建项目并添加 UDF 引用](step-1-creating-a-project-and-adding-a-udf-reference.md)
  
    
    
 [步骤 2：创建托管代码 UDF](step-2-creating-a-managed-code-udf.md)
  
    
    
 [步骤 3：部署和启用 UDF](step-3-deploying-and-enabling-udfs.md)
  
    
    
 [步骤 4：从单元格测试和调用 UDF](step-4-testing-and-calling-udfs-from-cells.md)
#### 概念


  
    
    
 [访问 SOAP API](accessing-the-soap-api.md)
#### 其他资源


  
    
    
 [步骤 2：添加 Web 引用](step-2-adding-a-web-reference.md)
  
    
    
 [步骤 3：访问 Web 服务](step-3-accessing-the-web-service.md)
  
    
    
 [演练：使用 Excel Web Services 开发自定义应用程序](walkthrough-developing-a-custom-application-using-excel-web-services.md)
