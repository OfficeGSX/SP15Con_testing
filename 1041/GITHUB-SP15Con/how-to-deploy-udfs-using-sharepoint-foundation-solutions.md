---
title: SharePoint Foundation のソリューションを使用して UDF を配置する方法
keywords: how to,howdoi,howto,udf list,WSS Solutions
f1_keywords:
- how to,howdoi,howto,udf list,WSS Solutions
ms.prod: OFFICE365
ms.assetid: 97751a6c-ef73-4d95-a3c4-98014d84ba48
---


# SharePoint Foundation のソリューションを使用して UDF を配置する方法

この使用例は、Microsoft SharePoint Foundation ソリューション フレームワークを使用してユーザー定義関数 (UDF) の DLL を配置する方法を示しています。
  
    
    

SharePoint Foundation ソリューション フレームワークでは、すべてのコンポーネントをバンドルして、ソリューション ファイル (CAB ベースの形式で拡張子は .wsp) という新しいファイルで SharePoint Foundation を拡張することができます。ソリューションは、サイトに適用でき、個別に有効化または無効化できる一連の機能、サイト定義、アセンブリが含まれる配置と再利用が可能なパッケージです。加えて、ソリューション ファイルを使用すると、アセンブリ、クラスのリソース, .dwp ファイル、その他のパッケージ コンポーネントなどの Web パーツ パッケージのコンテンツを配置できます。SharePoint Foundation ソリューション フレームワークについて、詳細は  [SharePoint Foundation の開発を開始する](http://msdn.microsoft.com/library/ef1187aa-e007-4490-8191-db36a50b3ae4%28Office.15%29.aspx) (http://msdn.microsoft.com/ja-jp/library/ee539432(office.14).aspx) の SharePoint Foundation ノードを参照してください。
SharePoint Foundation ソリューション フレームワークを使用して UDF アセンブリの作成と配置を行う手順は次のとおりです。
  
    
    


1. ソリューション マニフェスト ファイル Manifest.xml を作成します。
    
    ソリューション マニフェスト (常に Manifest.xml と呼ばれます) は、ソリューション ファイルのルートに格納されます。 このファイルでは、機能、サイト定義、リソース ファイル、Web パーツ ファイル、処理されるアセンブリのリストを定義しますが、ファイル構造は定義しません。ファイルがソリューションに含まれているが、マニフェスト XML ファイルには含まれない場合、ファイルが処理されることはありません。
    
    > **メモ**
      > マニフェスト XML ファイルの構造について、詳細は SharePoint Foundation のドキュメントを参照してください。 
2. UDF アセンブリと Manifest.xml を (http://msdn.microsoft.com/ja-jp/library/ee539432(office.14).aspx) を CAB ファイルにパッケージ化します。
    
  
3. SharePoint Foundation 管理サービスがサーバー上で実行していることを確認してください。
    
  
4. Stsadm.exe を使用して、ソリューションをサーバーに追加します。
    
  
5. Stsadm.exe を使用して、ソリューションを配置します。
    
  
Excel Services の信頼できる場所のそれぞれには **AllowUdfs** フラグがあります。
> **メモ**
> **AllowUdfs** フラグは、[Excel Services 信頼できるファイル保存場所] ページにある [ **User-defined functions allowed**] オプションで示されます。[信頼できるファイル保存場所] ページにナビゲートする方法について知るには、「 [ステップ 3: UDF を展開して有効にする](step-3-deploying-and-enabling-udfs.md)」を参照してください。 
  
    
    

特定の信頼できる場所からの UDF の呼び出しを許可するためには、以下を行う必要があります。
- [ **AllowUdfs**] の値を **true** にします。既定値は **false** です。
    
  
- UDF アセンブリを信頼できる UDF リストに追加して、ブックからの UDF の呼び出しを許可します。
    
  
UDF の有効化および信頼できる UDF リストへの UDF の追加について、詳細は「 [UDF を有効にする方法](how-to-enable-udfs.md)」を参照してください。
> **メモ**
> 名前の競合を避けるため、UDF アセンブリとその依存関係には厳密な名前で、できるだけ一意の名前を付けます。詳細は、「 [Excel Services のベスト プラクティス](excel-services-best-practices.md)」および「 [Excel Services の既知の問題およびヒント](excel-services-known-issues-and-tips.md)」を参照してください。 
  
    
    


## 手順


### Manifest.xml ファイルを作成するには


1. [ **ソリューション エクスプローラー**] で、[ **追加**] をポイントしてから、[ **新しいアイテム**] をクリックします。
    
  
2. [ **XML ファイル**] を選択し、ファイル名を「Manifest.xml」とします。
    
  
3. [ **追加**] をクリックします。
    
  
4. ファイルに次の内容を追加します。
    
  ```XML
  
<?xml version="1.0" encoding="utf-8" ?>
<Solution xmlns="http://schemas.microsoft.com/sharepoint/" SolutionId="{57568687-2CC0-45bf-B66A-2D50D57108CA}" DeploymentServerType="ApplicationServer">
  <Assemblies>
    <Assembly DeploymentTarget="GlobalAssemblyCache" Location="EcsUdfsCommonSet.dll"/>
  </Assemblies>
</Solution>
  ```


> **メモ**
> ソリューションごとに一意の GUID を生成する必要があります。 **Solution** 要素について、詳細は「SharePoint Foundation [ソリューションおよび Web パーツ パッケージ](http://msdn.microsoft.com/library/a145a5eb-fbb6-4328-b5b3-96bf5ce89a19%28Office.15%29.aspx)」(http://msdn.microsoft.com/ja-jp/library/ms413687.aspx) を参照してください。 

### ソリューション パッケージを作成するには


- ソリューション ファイルの作成方法について、詳細は SharePoint Foundation SDK の「ソリューションおよび Web パーツ パッケージ」の下にある「ソリューションの作成」トピックを参照してください。 
    
  

### SharePoint Foundation の管理が実行されているかどうかを確認するには


1. [ **開始**] をクリックし、[ **管理ツール**] をポイントしてから、[ **サービス**] をダブルクリックします。 
    
    [ **サービス**] ダイアログ ボックスが表示されます。 
    
  
2. SharePoint Foundation 管理サービスのステータスが「 **開始**」であることを確認します。「開始」でない場合は、[SharePoint Foundation 管理] を右クリックしてから [ **開始**] をクリックします。
    
  

### ソリューションを追加するには


1. [ **スタート** ]、[ **ファイル名を指定して実行** ] の順にクリックしてから、「cmd」と入力します。 
    
    コマンド プロンプト コンソールが表示されます。
    
  
2. SharePoint サーバーにソリューションを追加するには、次のスクリプトを実行します。 
    
    stsadm.exe -o addsolution -filename <pathtoCAB>
    
    > **メモ**
      > Stsadm.exe は、 <br/> C:\\Program Files\\Common Files\\Microsoft Shared\\web server extensions\\12\\BIN にあります。 

    > **メモ**
      > Stsadm.exe コマンド オプションについて、詳細は「 [Stsadm (Windows PowerShell マッピング) (SharePoint Foundation 2010)](http://technet.microsoft.com/ja-jp/library/ff621081.aspx)」 (http://technet.microsoft.com/ja-jp/library/ff621081.aspx) を参照してください。 

  
    
    

### ソリューションを配置するには


1. [ **スタート** ]、[ **ファイル名を指定して実行** ] をクリックしてから、「cmd」と入力します。 
    
    コマンド プロンプト コンソールが表示されます。
    
  
2. ソリューションを SharePoint サーバーに配置するには、次のスクリプトを実行します。 
    
    stsadm.exe -o deploysolution -name <filename of the CAB> -immediate -allowGacDeployment
    
    グローバル アセンブリ キャッシュに UDF DLL が表示されます。
    
  

## 関連項目


#### タスク


  
    
    
 [方法: Web サービスを呼び出す UDF の作成](how-to-create-a-udf-that-calls-a-web-service.md)
  
    
    
 [UDF を有効にする方法](how-to-enable-udfs.md)
  
    
    
 [方法: UDF コード アクセス セキュリティのアクセス許可を制限する](how-to-restrict-udf-code-access-security-permissions.md)
#### 概念


  
    
    
 [チュートリアル: マネージ コード UDF を開発する](walkthrough-developing-a-managed-code-udf.md)
  
    
    
 [Excel Services UDF に関するよく寄せられる質問](frequently-asked-questions-about-excel-services-udfs.md)
  
    
    
 [Excel Services UDF の理解](understanding-excel-services-udfs.md)
