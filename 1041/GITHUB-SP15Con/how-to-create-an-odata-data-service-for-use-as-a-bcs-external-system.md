---
title: BCS 外部システムとして使用する OData データ サービスを作成する方法
ms.prod: SHAREPOINT
ms.assetid: 7d7b3aa6-85b7-400d-8ea5-50bebac56a1d
---


# BCS 外部システムとして使用する OData データ サービスを作成する方法
インターネットアドレス指定対応の WCF サービスを作成する方法について確認します。このサービスでは、土台となるデータが変更された場合に OData を使用して SharePoint 2013に通知を送信します。これらの通知は、外部リストに関連付けられたイベントをトリガーするのに使用されます。
この記事では ASP.NET Windows Communication Foundation (WCF) データ サービスを作成し、AdventureWorks 2012 LT サンプル データベースを公開する方法を説明します。これにより、Open Data Protocol (OData) 経由でデータにアクセスできるようになります。OData 経由のアクセスが確立されると、SharePoint 2013での外部データベースのデータの使用を実現する Business Connectivity Services (BCS) 外部コンテンツ タイプを構成できるようになります。この OData ソースをさらに拡張するには、WCF サービスにサービス契約を追加し、外部データの変更を告げる通知を BCS が購読できるようにします。
  
    
    


## OData サービス作成の前提条件
<a name="bkmk_Prerequisites"> </a>

以下に、この記事での OData サービスの作成に必要なものを示します。
  
    
    

- インターネット インフォメーション サービス (IIS) をホストしているサーバー
    
  
- .NET Framework 3.5 以降
    
  
- SharePoint 2013
    
  
-  [AdventureWorks 20012 LT のスクリプト](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
-  [AdventureWorks 2012 LT データ](http://msftdbprodsamples.codeplex.com/releases/view/55330)
    
  
- Visual Studio 2012
    
  
- Office Developer Tools for Visual Studio 2012
    
  
開発環境の設定方法については、「 [SharePoint 2013 の一般的な開発環境の設定](set-up-a-general-development-environment-for-sharepoint-2013.md)」を参照してください。
  
    
    

### OData サービス作成の中心概念

表 1 に、OData および外部コンテンツを使用する WCF サービスの作成の中心概念を理解するのに役立つ記事を示します。
  
    
    

**表 1: OData サービス作成の中心概念**


|**リソース**|**説明**|
|:-----|:-----|
| [SharePoint 2013 の Business Connectivity Services で OData ソースを使用する](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md) <br/> |OData ソースに基づく外部コンテンツ タイプの作成を開始するための情報と、そのデータを SharePoint 2013または Office 2013 コンポーネントで使用するための情報が提供されています。  <br/> |
| [[方法] SharePoint 2013 で OData ソースから外部コンテンツ タイプを作成する](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md) <br/> |Visual Studio 2012 を使用して公開済みの OData ソースを見つけ出し、SharePoint 2013の BCS で使用する、再使用可能な外部コンテンツ タイプを作成する方法を確認します。  <br/> |
| [Open Data Protocol](http://www.odata.org) <br/> |プロトコル定義、アーキテクチャ情報、使用例など、Open Data Protocol に関する情報が提供されています。  <br/> |
| [WCF Data Services の概要](http://msdn.microsoft.com/ja-jp/library/cc668794.aspx) <br/> |WCF Data Services は、OData を使用した Web またはイントラネット向けデータ サービスの作成および使用を実現するサービスです。OData を使用すると、データを URI でアドレス指定可能なリソースとして公開することができます。  <br/> |
| [WCF Data Services の開発と配置](http://msdn.microsoft.com/ja-jp/library/gg258442.aspx) <br/> |WCF Data Services の開発と配置に関する情報が提供されています。  <br/> |
   

### 外部システムを作成するときに必要な手順

次の手順を完了する必要があります。
  
    
    

- AdventureWorks 2012 LT サンプル データベースをインストールする
    
  
- OData サービスを作成する
    
  
- アクセス許可を設定する
    
  
- サービスをテストする
    
  
- 追加の BCS 機能に関する機能を追加する
    
  

## サンプル データベースをインストールする
<a name="bkmk_Prerequisites"> </a>

通常、外部システムはデータベースなので、この例では、専用データソースを示す AdventureWorks 2012 LT サンプル データベースの使用方法について説明します。
  
    
    
手順の内容
  
    
    

## WCF OData サービスの作成方法
<a name="bkmk_CreatingTheService"> </a>

OData プロトコル経由でアクセス可能な Windows Communication Foundation (WCF) サービスは簡単に作成できます。Visual Studio 2012 には、さまざまなデータ ソースからデータを自動的に見つけ出し、モデリングするためのツールがいくつか備わっています。これを使用して SQL Server データベースおよびその他の種類のデータ ストアのデータへの接続とインターフェイスを作成すると、拡張オブジェクト モデルを使用してプログラムで操作できるようになります。
  
    
    
ただし、SharePoint 2013で、土台となるデータが変更された場合に BCS がリモート システムから通知を受信できるようにするには、それらの変更を受信するよう WCF サービスを変更する必要があります。
  
    
    

### 新しい WCF プロジェクトを作成するには


1. Visual Studio の [ **ファイル**] メニューで、[ **新規作成**]、[ **プロジェクト**] を順に選択します。
    
  
2. [ **新しいプロジェクト**] ダイアログ ボックスで、[ **Web**] テンプレートを選択し、[ **ASP.NET Web アプリケーション**] を選択します。
    
  
3. プロジェクト名に「 **AdventureWorksService**」と入力し、[ **OK**] をクリックします。
    
  
4. **ソリューション エクスプローラー**で、作成した ASP.NET プロジェクトのショートカット メニューを開き、[ **プロパティ**] を選択します。
    
  
5. [ **Web**] タブを選択し、[ **Specific port**] (特定のポート) テキスト ボックスを「8008」 に設定します。
    
  

### データ モデルを定義するには


1. **ソリューション エクスプローラー**で、ASP.NET プロジェクトのショートカット メニューを開き、[ **新しい項目の追加**] を選択します。
    
  
2. [ **新しい項目の追加**] ダイアログ ボックスで、[データ] テンプレートを選択し、[ **ADO.NET Entity Data Model**] を選択します。
    
  
3. データ モデルの名前に、「 **AdventureWorks.edmx**」と入力します。
    
  
4. **Entity Data Model ウィザード**で、[ **データベースから生成**] を選択し、[ **次へ**] を選択します。
    
  
5. 次のいずれかの手順を実行してデータ モデルをデータベースに接続し、[ **次へ**] をクリックします。
    
  - 構成済みのデータベース接続がない場合は、[ **新しい接続**] を選択し、新しい接続を作成します。
    
  
  - Northwind データベースに接続するよう構成されているデータベース接続が存在する場合は、一覧からその接続を選択します。
    
  
  - ウィザードの最終ページで、データベース内のすべてのテーブルのチェック ボックスを選択し、ビューおよびストアド プロシージャのチェック ボックスをオフにします。
    
  
6. [ **完了**] を選択してウィザードを終了します。
    
  

### データ サービスを作成するには


1. **ソリューション エクスプローラー**で、ASP.NET プロジェクトのショートカット メニューを開き、[ **新しい項目の追加**] を選択します。
    
  
2. [ **新しい項目の追加**] ダイアログ ボックスで [ **WCF データ サービス**] を選択します。
    
  
3. サービスの名前に「 **AdventureWorks**」と入力します。
    
    Visual Studio では新しいサービスの XML マークアップおよびコード ファイルが作成されます。既定ではコード エディターのウィンドウが開きます。 **ソリューション エクスプローラー**では、サービスの名前に「 **AdventureWorks**」という名前が付き、拡張子 .svc.cs または .svc.vb が付与されます。
    
  
4. データ サービスを定義するクラスの定義にあるコメント  `/* TODO: put your data source class name here */` を、データ モデルのエンティティ コンテナーである種類 (この場合は **AdventureWorksEntities**) に置き換えます。クラス定義は次のようになります。
    
  ```cs
  
public class AdventureWorks : DataService<AdventureWorksEntities>
  ```

既定では、WCF サービスが作成されるとセキュリティ構成のためアクセスできません。次のステップで、アクセス可能なユーザーと、そのユーザーに付与する権限を指定します。
  
    
    

### データ サービス リソースにアクセスできるようにするには


- データ サービスのコードで、 **InitializeService** 関数のプレースホルダーのコードを次の内容に置き換えます。
    
  ```cs
  
      config.SetEntitySetAccessRule("*", EntitySetRights.All);
      config.SetServiceOperationAccessRule("*", ServiceOperationRights.All);
  ```


    これにより、承認されたクライアントに、指定されたエンティティ セットのリソースに対する読み取りと書き込みのアクセス権が付与されます。
    
    > **メモ**
      > ASP.NET アプリケーションにアクセス可能なクライアントはいずれも、データ サービスによって公開されるリソースにもアクセスできます。本番環境のデータ サービスでは、許可されていないリソースへのアクセスを防ぐため、アプリケーション自体をセキュリティで保護する必要があります。詳細については、「 [WCF Data Services のセキュリティ保護](http://msdn.microsoft.com/ja-jp/library/dd728284.aspx)」を参照してください。 
BCS が通知を受信するには、通知購読で追加および削除される要求を受け取る、バックエンドのデータ ソースのメカニズムが必要です。
  
    
    
サービス作成の最後のステップは、BDC モデルに定義されている **Subscribe** および **Unsubscribe** ステレオタイプに対するサービス操作の追加です。
  
    
    

### Subscribe および Unsubscribe ステレオタイプのサービス操作を追加するには


- AdventureWorks.cs ページで、次の文字列変数宣言を追加します。
    
  ```cs
  
public string subscriptionStorePath = @"\\\\[SHARE_NAME]\\SubscriptionStore\\SubscriptionStore.xml";
  ```


    > **メモ**
      >このファイルは新しい購読によって更新される XML ファイルです。このファイルへのアクセスはサーバー プロセスで行われるので、このファイルへのアクセスに必要な権限が付与されていることを確認してください。 <br/> 購読情報の保存用にデータベース ソリューションを作成することもできます。 

    次に、次の 2 つの **WebGet** メソッドを追加して、購読を操作できるようにします。
    


  ```cs
  [WebGet]
        public string Subscribe(string deliveryUrl, string eventType)
        {
            string subscriptionId = Guid.NewGuid().ToString();
            
            XmlDocument subscriptionStore = new XmlDocument();
            
            subscriptionStore.Load(subscriptionStorePath);

            // Add a new subscription element.
            XmlNode newSubNode = subscriptionStore.CreateElement("Subscription");

            // Add subscription ID element to the subscription element.
            XmlNode subscriptionIdStart = subscriptionStore.CreateElement("SubscriptionID");
            subscriptionIdStart.InnerText = subscriptionId;
            newSubNode.AppendChild(subscriptionIdStart);

            // Add delivery URL element to the subscription element.
            XmlNode deliveryAddressStart = subscriptionStore.CreateElement("DeliveryAddress");
            deliveryAddressStart.InnerText = deliveryUrl;
            newSubNode.AppendChild(deliveryAddressStart);

            // Add event type element to the subscription element.
            XmlNode eventTypeStart = subscriptionStore.CreateElement("EventType");
            eventTypeStart.InnerText = eventType;
            newSubNode.AppendChild(eventTypeStart);

            // Add the subscription element to the root element. 
            subscriptionStore.AppendChild(newSubNode);

            
            subscriptionStore.Save(subscriptionStorePath);

            return subscriptionId;
        }

        [WebGet]
        public void Unsubscribe(string subscriptionId)
        {
            XmlDocument subscriptionStore = new XmlDocument();
            subscriptionStore.Load(subscriptionStorePath);

            XmlNodeList subscriptions = subscriptionStore.DocumentElement.ChildNodes;
            foreach (XmlNode subscription in subscriptions)
            {
                XmlNodeList subscriptionList = subscription.ChildNodes;
                if (subscriptionList.Item(0).InnerText == subscriptionId)
                {
                    subscriptionStore.DocumentElement.RemoveChild(subscription);
                    break;
                }
            }

            subscriptionStore.Save(subscriptionStorePath);
        }

  ```


## 次の手順
<a name="bkmk_Next"> </a>

変更が加えられたことを SharePoint に通知するには、このほか、データ ソースに変更を照会するサービスを作成し、通知のすべての購読者に通知を送信する必要があります。
  
    
    

## その他の技術情報
<a name="bkmk_Addresources"> </a>


-  [SharePoint 2013 の Business Connectivity Services](business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の Business Connectivity Services で OData ソースを使用する](using-odata-sources-with-business-connectivity-services-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の外部コンテンツ タイプ](external-content-types-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 の外部イベントおよびアラート](external-events-and-alerts-in-sharepoint-2013.md)
    
  
-  [[方法] SharePoint 2013 で OData ソースから外部コンテンツ タイプを作成する](how-to-create-an-external-content-type-from-an-odata-source-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 Business Connectivity Services プログラマー リファレンス](business-connectivity-services-programmers-reference-for-sharepoint-2013.md)
    
  

