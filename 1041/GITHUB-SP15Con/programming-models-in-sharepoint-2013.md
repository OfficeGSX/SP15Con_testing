---
title: SharePoint 2013 でのプログラミング モデル
ms.prod: SHAREPOINT
ms.assetid: 061985ec-6129-4e91-991b-a72488ce1d34
---



# SharePoint 2013 でのプログラミング モデル
さまざまな種類の SharePoint 開発プロジェクトの概要について説明します。
  
    
    


|||
|:-----|:-----|
|**この記事の内容**          [SharePoint 用アドイン](#Apps)<br/>           [SharePoint 発行サイト](#ECM)<br/>           [SharePoint ファーム ソリューション](#Solutions)<br/>           [SharePoint 用モバイル アドイン](#Mobile)<br/>           [SharePoint 用再利用可能コンポーネント](#Reuse)<br/>           [その他の技術情報](#SP15devinSP_addlresources)|**ビデオを見る: Office 2013 および SharePoint 2013 の開発オプション**<br/>![ビデオ](images/mod_icon_video.png)|
   

SharePoint 2013 プラットフォーム用のアプリケーションはさまざまな方法で開発できます。これらのアプリケーションは、作成に使用するツール、開発に使用するプログラミング モデル、パッケージ化と展開の方法、販売方法、および実行するデバイスに基づいて、次のグループに通常分類されます。
  
    
    


- SharePoint アドイン
    
  
- SharePoint 発行サイト
    
  
- SharePoint ファーム ソリューション
    
  
- SharePoint 用モバイル アドイン
    
  
- SharePoint 用再利用可能コンポーネント
    
  
これらのカテゴリは相互に排他的ではありません。たとえば、発行サイトを SharePoint アドイン として開発できます。以下のセクションではこれらのカテゴリを定義すると共に、それぞれの説明ドキュメントを紹介します。
## SharePoint 用アドイン
<a name="Apps"> </a>

SharePoint アドイン はモバイル デバイス上のアドインと似ています。スタンドアロンの生産性向上ソリューションであり、少数の関連タスクを行い、簡単にインストールでき、きれいにアンインストールできます。ユーザーは SharePoint アドイン をパブリックの SharePoint アドイン ストアまたは組織の企業アドイン カタログから探してダウンロードできます。SharePoint アドイン には、リスト、カスタム Web サイト ページ、Web パーツ、ワークフロー、コンテンツ タイプなどの従来の SharePoint コンポーネントを含めることができます。しかし、SharePoint アドイン は、SharePoint のリモート Web アプリケーションやリモート データを表示することもできます。SharePoint アドイン には SharePoint コンポーネントとリモート コンポーネントの両方を含めることもできます。SharePoint アドイン は非常に安全なアプリケーションであり、カスタム ロジックはいつでもクラウドに「シフト アップ」することもクライアント コンピューターに「シフト ダウン」することもできます。SharePoint サーバー上では実行されません。
  
    
    
SharePoint アドイン用のモデル の概要については、「 [SharePoint アドイン](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)」を参照してください。詳細については、「 [SharePoint アドインと SharePoint ソリューションの比較](sharepoint-add-ins-compared-with-sharepoint-solutions.md)」、および「 [SharePoint 2013 での適切な API セットの選択](choose-the-right-api-set-in-sharepoint-2013.md)」を参照してください。
  
    
    

## SharePoint 発行サイト
<a name="ECM"> </a>

SharePoint 発行サイトは高度な保守性と法令遵守機能を備えた大規模なコンテンツ発行を提供します。また、ドキュメント、記録、分類、コンテンツ タイプの管理も提供します。詳細については、「 [SharePoint サイトの構築](build-sites-for-sharepoint.md)」を参照してください。
  
    
    

## SharePoint ファーム ソリューション
<a name="Solutions"> </a>

SharePoint ファーム ソリューション は、カスタム ロジックで SharePoint サーバー オブジェクト モデルを呼び出し、SharePoint サーバー上で完全信頼で実行される、信頼された SharePoint 拡張です。これらのソリューションは、主にタイマー ジョブ、カスタム Windows PowerShell コマンド、サーバーの全体管理の拡張など、SharePoint のカスタム管理拡張のためのものです。ファーム ソリューションは、ファーム管理者がファーム全体の保存場所にアップロードしてそこから展開が可能な SharePoint ソリューション パッケージとして配布されます。ファーム ソリューション のコンポーネントには、ファーム、Web アプリケーション、サイト コレクション、または Web サイトのスコープを含めることができます。詳細については、「 [SharePoint 2013 でのファーム ソリューションの作成](build-farm-solutions-in-sharepoint-2013.md)」を参照してください。
  
    
    

## SharePoint 用モバイル アドイン
<a name="Mobile"> </a>

Windows Phone アプリ、および Microsoft 以外のモバイル プラットフォームで作成されたアプリは、SharePoint の Web サイトおよびデータにアクセスできます。SharePoint 2013 と連携する Windows Phone アプリを作成するツールは、Visual Studio 2010 と Visual Studio 2012 で利用できます。Windows Phone デバイス上で使用される SharePoint クライアントのマネージ API のみが利用できます。モバイル デバイス (Microsoft 以外のデバイスも含む) は、SharePoint REST/OData エンドポイントを介して SharePoint データにアクセスすることもできます。詳細については、「 [SharePoint 2013 にアクセスする Windows Phone アプリの作成](build-windows-phone-apps-that-access-sharepoint-2013.md)」を参照してください。
  
    
    

## SharePoint 用再利用可能コンポーネント
<a name="Reuse"> </a>

SharePoint 2013 プラットフォームおよび Visual Studio 2012 では、コード、スクリプト、および XML マークアップで作成された要素などの、アプリケーション要素のカプセル化と再利用が可能です。詳細については、「 [再利用可能な SharePoint 2013 用コンポーネントの作成](build-reusable-components-for-sharepoint-2013.md)」を参照してください。
  
    
    

## このセクションの内容
<a name="Reuse"> </a>


-  [SharePoint サイトの構築](build-sites-for-sharepoint.md)
    
  
-  [SharePoint 2013 でのファーム ソリューションの作成](build-farm-solutions-in-sharepoint-2013.md)
    
  
-  [SharePoint 2013 にアクセスする Windows Phone アプリの作成](build-windows-phone-apps-that-access-sharepoint-2013.md)
    
  
-  [再利用可能な SharePoint 2013 用コンポーネントの作成](build-reusable-components-for-sharepoint-2013.md)
    
  

## その他の技術情報
<a name="SP15devinSP_addlresources"> </a>


-  [SharePoint 2013 の一般的な開発環境の設定](set-up-a-general-development-environment-for-sharepoint-2013.md)
    
  
-  [SharePoint 2013 機能の追加](add-sharepoint-2013-capabilities.md)
    
  
-  [SharePoint 2013 におけるアクセシビリティ](accessibility-in-sharepoint-2013.md)
    
  
