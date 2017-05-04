---
title: チュートリアル Excel Web Services を使用してカスタム アプリケーションを開発する
ms.prod: SHAREPOINT
ms.assetid: 2f9bf243-281a-4d70-917e-9eaf0b867631
---


# チュートリアル: Excel Web Services を使用してカスタム アプリケーションを開発する

このセクションのチュートリアルでは、Microsoft Visual C# で作成したアプリケーションから Excel Web Services にアクセスする手順について説明します。
  
    
    

このチュートリアルでは、以下の操作を実行する方法を説明します。
- Visual Studio の [コンソール アプリケーション] プロジェクト テンプレートを使用してクライアント アプリケーションを作成する。
    
  
- Excel Web Services の Web 参照を追加する。
    
  
- Web サービスにアクセスするコードを作成する。ブックを開き、セッション ID を取得し、既定の資格情報を渡し、Web サービスのバージョン情報を取得し、範囲座標オブジェクトを定義し、範囲座標オブジェクトを使用する範囲を取得し、ブックを閉じ、SOAP 例外を catch する方法を説明します。
    
  
- コンソール アプリケーションをデバッグ モードでテストして実行する。
    
  
クライアント コンソール アプリケーションは、Web サービスにアクセスする方法の 1 つにすぎません。より一般的な方法には、Microsoft ASP.NET アプリケーションなどのサーバー アプリケーションを使用する方法があります。このチュートリアルでは、Excel Web Services API という側面に重点を置き、わかりやすくするために、コンソール アプリケーションの例を使用します。
## 前提条件

このチュートリアルを完了するには、以下のものが必要です。 
  
    
    

- Microsoft SharePoint Server 2010。
    
  
- Visual Studio、またはそれに類似した Microsoft .NET Framework 準拠の開発ツール。
    
  
- SharePoint Server 2010 があるコンピューター上の Excel Web Services にアクセスするために必要なアクセス許可 (最低でも、"表示" アクセス許可)。 
    
    > **メモ**
    > ブックのアクセス許可の詳細については、この後の「ブックのアクセス許可」セクションを参照してください。 
- ローカル ドライブまたはローカル SharePoint ドキュメント ライブラリにインストールしたサンプル ブック。 
    
  
- Excel Web Services を使用してアクセスするブックを格納するための信頼できる場所。信頼できる場所にブックを格納しないと、Excel Web Services 呼び出しでブックを開く操作が失敗します。このチュートリアルでは、ブックがローカル コンピューター上にあることを想定しています。 
    
    > **メモ**
    > 場所の信頼方法について、詳細は「 [方法: 場所を信頼する](how-to-trust-a-location.md)」および「 [[方法] スクリプトを使用してブックの場所を信頼する](http://msdn.microsoft.com/library/79ab6ced-7a0c-4275-b852-bb246fc6be57%28Office.15%29.aspx)」を参照してください。 
- Excel を使用してブックを作成すること。
    
  
- そのブックを .xlsx または .xlsb ファイルとして保存すること。
    
  
この例で使用するブックには、"Sheet1" という名前のワークシートがあります。このワークシートは 11 桁 19 行で構成され、A1 から K19 までの各セルに数値 (たとえば、4245.955、6960.673 など) が入っています。
  
    
    

## ブックのアクセス許可


- ブック全体を取得する (たとえば、 **GetWorkbook** メソッドを呼び出す) には、呼び出し元に、ブックに対する "開く" アクセス許可が必要になります。
    
  
- **GetApiVersion** メソッドを呼び出すには、アクセス許可は必要ありません。
    
  
- 残りの Excel Web Services メソッドについては、呼び出し元に、ブックに対する "表示" アクセス許可 (Microsoft SharePoint Foundation 内の場合) または "読み取り" アクセス許可 (ファイル共有の場合) が必要になります。
    
    > **メモ**
    > 許可の設定について、詳細は SharePoint Foundation のドキュメントを参照してください。 

## 関連項目


#### 概念


  
    
    
 [SOAP API にアクセスする](accessing-the-soap-api.md)
  
    
    
 [Excel Services のアラート](excel-services-alerts.md)
  
    
    
 [Excel Services の既知の問題およびヒント](excel-services-known-issues-and-tips.md)
  
    
    
 [SOAP 呼び出しのループバックおよび直接リンク](loop-back-soap-calls-and-direct-linking.md)
#### その他の技術情報


  
    
    
 [ステップ 1: Web サービス クライアント プロジェクトを作成する](step-1-creating-the-web-service-client-project.md)
  
    
    
 [手順 2: Web 参照を追加する](step-2-adding-a-web-reference.md)
  
    
    
 [手順 3: Web サービスへのアクセス](step-3-accessing-the-web-service.md)
  
    
    
 [手順 4: アプリケーションのビルドとテストを行う](step-4-building-and-testing-the-application.md)
