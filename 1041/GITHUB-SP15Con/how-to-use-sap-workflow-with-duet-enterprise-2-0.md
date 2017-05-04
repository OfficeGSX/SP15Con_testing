---
title: Duet Enterprise 2.0 で SAP ワークフローを使用する方法
ms.prod: SHAREPOINT
ms.assetid: 816e28ed-8cea-4e33-98e5-d3d27136e2e6
---


# Duet Enterprise 2.0 で SAP ワークフローを使用する方法

## 概要
<a name="bkmk_Introduction"> </a>

SAP Business Workflow では、ビジネス プロセスを自動化する方法を提供します。さらに、Duet Enterprise 2.0 を使用することで、これらのワークフローを SharePoint アドインに統合できます。
  
    
    
以下の手順では、アプリを作成して構成し、SAP ワークフローから取得した情報を表示する方法について説明します。
  
    
    

## SharePoint および Duet Enterprise 2.0 用アプリの作成
<a name="bkmk_CreateApp"> </a>

最初の手順では、Business Connectivity Services (BCS) の外部コンテンツ タイプ、外部リスト、データを表示するための任意のカスタマイズを使用して、接続情報を保持する SharePoint アドインを作成します。
  
    
    

### SharePoint 用アプリを作成するには:


1. 管理者として Visual Studio 2012 を起動します。
    
  
2. [ **ファイル**]、[ **新規**]、[ **新しいプロジェクト**] を選択します。
    
  
3. [ **新しいプロジェクト**] ダイアログ ボックスで、[ **Visual C#**] ノードを展開し、[ **Office/SharePoint**] ノードを展開して、[ **アプリ**] ノードを選択します。
    
  
4. [ **SharePoint 2013 用アプリ**] を選択します。
    
  
5. プロジェクトに名前を付けて、[ **OK**] をクリックします。
    
  
6. 1 つ目の [ **SharePoint 用アプリの設定を指定する**] ダイアログ ボックスでアプリに名前を付け、[ **SharePoint 用アプリをホストする方法**] で [ **SharePoint ホスト型**] を選択します。また、[ **アプリのデバッグに使用する SharePoint サイト**] でデバッグを実行する **Duet ワークフロー サイト**の URL を指定し、[ **完了**] を選択します。
    
  
7. [ビルド] メニューで [ **配置**] [ *<アプリ名>*  ] を選択します。
    
  
アプリを配置すると、Visual Studio によってアプリの既定のページが表示されます。
  
    
    

## アプリへの外部コンテンツ タイプと外部リストの追加
<a name="bkmk_AddECT"> </a>

BCS によって外部データ ソースが認識されるように設定する必要があります。そのためには、外部コンテンツ タイプを使用します。
  
    
    

### 外部コンテンツ タイプを追加するには:


1. **ソリューション エクスプローラー**で、プロジェクトのショートカット メニューを開き、[ **追加**]、[ **外部データ ソースのコンテンツ タイプ**] の順に選択します。
    
  
2. [ **OData ソースの指定**] ページに Duet Enterprise ワークフロー サービスの URL を入力します。以下に、Duet サービスの URL の例を示します。
    
     *https://<<DUETGATEWAY>>:<<PORT>>/sap/opu/odata/IWWRK/DUET_WORKFLOW_CORE;mo;c=SHAREPOINT_DE* 
    
  
3. ODATA ソースの名前 (SAPWorkflows など) を選択して、[ **次へ**] をクリックします。
    
  
4. SAP のユーザー名とパスワードを指定して、サービス メタデータに接続します。
    
  
5. OData サービスにより公開されているデータ エンティティの一覧が表示されます。外部コンテンツ タイプに追加するエンティティを選択します。この例では、SAP ワークフロー サービスを使用して、以下の中から選択します。
    
  - **AttachmentCollection**
    
  
  - **CommentCollection**
    
  
  - **DecisionOptionCollection**
    
  
  - **ExtensibleElementCollection**
    
  
  - **ParticipantCollection**
    
  
  - **ServiceOperations**
    
  
  - **TaskDescriptionCollection**
    
  
  - **WorkflowTaskCollection**
    
  
6. [ **選択されたデータ エンティティのリスト インスタンスを作成します**] チェックボックスをオンにします。
    
  
7. [ **完了**] を選択します。
    
  

> **メモ**
> SAP ワークフロー サービスが、Visual Studio の BDC 自動生成ツールと同様に、基本認証を許可することを確認してください。 
  
    
    


## Duet Enterprise 2.0 ワークフロー リストへのカスタム アクション機能の追加
<a name="bkmk_AddCustomAction"> </a>

ユーザーが SharePoint リストに追加された新しい機能を使用して作業できるように、以下の手順で [サイトの操作] メニューにアイテムを追加します。
  
    
    

### カスタム アクションを追加するには:


1. SharePoint アドイン プロジェクトを右クリックし、新しい [ **UI カスタム動作**] アイテムを追加します。
    
  
2. カスタム アクションのホスト Web 機能の **Elements.xml** ファイルを開きます。ファイル内のコードを以下の XML に置き換えることで、カスタム アクションで以下の処理が行われます。
    
  - ECB カスタム アクションとその属性を宣言します。
    
  
  - リボン カスタム アクションとその属性を宣言します。
    
  
  - アプリの Web ページ ターゲットと、クエリ文字列を使ってそこに渡される値を宣言します。 
    
    **UrlAction** 要素はいくつかのトークンを使用します。詳しくは、「 [SharePoint アドインの URL 文字列とトークン](http://msdn.microsoft.com/library/800ec8cd-a448-46bc-b41e-d4030eeb4048%28Office.15%29.aspx)」をご覧ください。
    
  

  ```XML
  
<?xml version="1.0" encoding="utf-8"?>
<Elements xmlns="http://schemas.microsoft.com/sharepoint/">
  <CustomAction Id="ViewDetailsECBCustomAction"
          RegistrationType="List"
          RegistrationId="107"
          Location="EditControlBlock"
          Sequence="1"
          ImageUrl="_layouts/15/images/placeholder16x16.png"
          Title="View Details">
     <UrlAction Url="~appWebUrl/Pages/ViewDetails.aspx?
            {StandardTokens}&amp;amp;ListId={ListId}&amp;amp;ItemId={ItemId}"/>
  </CustomAction>

  <CustomAction
          Id="ViewDetailsRibbonAction"
          RegistrationId="107"
          RegistrationType="List"
          Location="CommandUI.Ribbon"
          Title="View Details"
          HostWebDialog="false"
          HostWebDialogHeight="500"
          HostWebDialogWidth="500">
    <CommandUIExtension>
      <CommandUIDefinitions>
        <CommandUIDefinition 
              Location="Ribbon.ListItem.Workflow.Controls._children">
          <Button
              Id="Ribbon.ListItem.Workflow.ViewDetails"
              Alt="View Details"
              Sequence="25"
              Command="Invoke_ViewDetailsSAPWorkflow"
              LabelText="View Details"
              TemplateAlias="o1"
              Image32by32="_layouts/15/images/placeholder32x32.png"
              Image16by16="_layouts/15/images/placeholder16x16.png"/>
        </CommandUIDefinition>
      </CommandUIDefinitions>
      <CommandUIHandlers>
        <CommandUIHandler
          Command="Invoke_ViewDetailsSAPWorkflow"
          CommandAction="~appWebUrl/Pages/ViewDetails.aspx?
           {StandardTokens}&amp;amp;ListId={ListId}&amp;amp;ItemId={SelectedItemId}"/>
      </CommandUIHandlers>
    </CommandUIExtension>
  </CustomAction>
</Elements>
  ```

3. ViewDetails.aspx という新しいページをプロジェクトに追加します。
    
  
4. **F5** キーを押して SharePoint アプリをビルドおよび配置します。
    
  
5. ホスト Web の **WorkItem** リストに移動し、コンテキスト メニューから [ **詳細の表示**] を選択します。 **ViewDetails.aspx** にリダイレクトされます。
    
  

> **メモ**
> SAP ワークフロー サービスが、基本認証を許可することを確認してください。Visual Studio の BDC 自動生成ツールでは現在、匿名認証と基本認証のみがサポートされています。 
  
    
    

アプリを配置してから、アプリ内で **Lists/WorkflowTaskCollection** に移動すると、以下のメッセージが表示されます。
  
    
    
「外部システムからのメッセージ: 'LobSystem (外部システム) が認証エラーを返しました。'」
  
    
    
これを修正するには、アプリにシングル サインオンを追加して、BCS と SAP バックエンドに認証資格情報を提供する必要があります。
  
    
    

## アプリへのシングル サインオン セキュリティの追加
<a name="bkmk_AddSSO"> </a>

Duet Enterprise のシングル サインオンにより、ユーザーが 1 回のサインインで SharePoint 側と SAP 側の両方のリソースにアクセスできるように承認されます。
  
    
    

### シングル サインオンを追加するには:


1. SharePoint Management Shell で以下のコマンドを実行して、OData 接続を作成します。
    
  ```XML
  
New-SPODataConnectionSetting -Name SAPWorkflow
-ServiceContext "http://localhost" -ExtensionProvider 
"OBA.Server.Canary.ObaOdataServerExtensionProvider, OBA.Server.SSOProvider, 
Version=15.0.0.0, Culture=neutral, PublicKeyToken=71e9bce111e9429c" 
-AuthenticationMode passthrough -ServiceAddressURL 
"https://<<DUETGATEWAY>>:<<PORT>>/sap/opu/odata/IWWRK/
DUET_WORKFLOW_CORE;mo;c=SHAREPOINT_DE"

  ```

2. **WorkflowTaskCollection.ect** をダブルクリックします。
    
  
3. [プロパティ] ウィンドウで、 `ODataConnectionSettingId` の値を *SAPWorkflow*  に更新します。
    
  
4. アプリによる接続の使用を許可します。
    
  
5. **AppManifest.xml** を開きます。
    
  
6. [ **アクセス許可の要求**] で、[ **スコープ**]、[ **BCS**]、[ **アクセス許可**] の順に選択し、値を [読み取り] に設定します。
    
  
7. [ **F5**] キーを押してアプリを配置します。
    
  
8. アプリの [ **アクセス許可**] ページで、このアプリが使用する OData 接続を選択します。
    
  
9. [ **信頼する**] ボタンをクリックします。
    
  
10. **../Lists/WorkflowTaskCollection** に移動します。割り当てられている SAP タスクが SAP システムから直接表示されます。
    
  

## JSOM を使用したワークフロー タスクと関連項目へのアクセス
<a name="bkmk_UseJSOM"> </a>

SharePoint アドインで SharePoint と通信するためにはクライアント コードを使用する必要があります。以下に、JavaScript オブジェクト モデルを使用して SAP ワークフロー タスクを操作する方法を示します。
  
    
    

### コードを作成するには:


1. **ソリューション エクスプローラー**の **Pages** フォルダーを右クリックし, .NET ページを追加して **ViewDetails.aspx** という名前を付けます。
    
  
2. 以下の HTML を  `PlaceHolderAdditionalPageHead` セクションに貼り付けます。これにより、スタイル シート参照が設定され、スクリプトがロードされて SAP から関連するワークフロー タスクが返されます。
    
  ```XML
  
<link
    rel="Stylesheet" 
    type="text/css"
    href="../Content/App.css"/>

<script 
    src="//ajax.aspnetcdn.com/ajax/4.0/1/MicrosoftAjax.js" 
    type="text/javascript">
</script>
<script 
    src="//ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js" 
    type="text/javascript">
</script>      
<script 
    src="../Scripts/ViewDetails.js" 
    type="text/javascript">
</script>

  ```

3. [ **Scripts**] フォルダーを右クリックし、JavaScript ファイルを追加します。ファイルに **ViewDetails.js** という名前を付けます。
    
  
4. このマークアップにより、以下の処理が実行されます。
    
  - 親 Webの  `ClientContext` を取得します。
    
  
  - Duet ワークフロー リスト GUID と選択したアイテム ID を使用して、選択した ワークフロー タスクの  `TaskInstanceParentId` を取得します。
    
  
  -  `TaskInstanceParentId` を解析して、SAP 由来と SAP 内のワークフローの ID を取得します。
    
  
  - 外部コンテンツ タイプのエンティティをロードします。
    
  
  - SAP 由来と SAP の ワークフロー タスク ID をパラメーターとして渡すことで、特定の検索を使用して SAP からの特定のワークフロー タスクを読み取ります。
    
  
  - SAP からのワークフロー タスクに関連する要素を読み取ります。
    
  
5. 以下の、このページ用の完全な HTML と JavaScript を示します。
    
  ```
  
// This code runs when the DOM is ready. It ensures the SharePoint
// script files are loaded and then executes execOperation().
$(document).ready(function () {
  SP.SOD.executeFunc("sp.js", "SP.ClientContext", execOperation);
});

function execOperation() {
  retrieveTaskFromList();
}

function retrieveTaskFromList() {
  // Retrieves the ClientContext of the parent web.
  var clientContext = 
  new SP.ClientContext.get_current();
  var appContextSite = 
  new SP.AppContextSite(clientContext, 
      decodeURIComponent(getQueryStringParameter("SPHostUrl")));
  var web = appContextSite.get_web();
  // Loads the selected workflow task from the duet workflow list.
  var listGuid = decodeURIComponent(getQueryStringParameter("ListId"));
  var itemId = decodeURIComponent(getQueryStringParameter("ItemId"))
  this.workflowItem = web.get_lists().getById(
      listGuid.substring(1, listGuid.length - 1)).getItemById(itemId);
  clientContext.load(workflowItem);

  clientContext.executeQueryAsync(
    Function.createDelegate(this, this.onLoadingTaskSucceeded),
    Function.createDelegate(this, this.onError)
  );
}

function onLoadingTaskSucceeded() {
  var sapParameters = parseParentTaskId(workflowItem.
      get_item("TaskInstanceParentId"));
  getDataFromSAP(sapParameters);
}

// Parses the TaskInstanceParentId 
// to get SAP Origin and the id for the workflow in SAP.
function parseParentTaskId(parentTaskId) {
  var idlength = parseInt(parentTaskId.substring(0, 2));
  var sapParameters = new Object();
  if (parentTaskId.length > (17 + idlength)) {
    sapParameters.workitemId = parentTaskId.substring(5, 5 + idlength);
    sapParameters.sapOrigin = parentTaskId.substring(17 + idlength);
  }
  return sapParameters;
}
    
// Retrieves the workflow task and associated elements from SAP.
function getDataFromSAP(sapParameters) {
  context = new SP.ClientContext.get_current();
  //Loads the entities for the external content types.
  wfEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "WorkflowTaskCollection");
  context.load(wfEntity);
  wfExtensibleElementEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "ExtensibleElementCollection");
  context.load(wfExtensibleElementEntity);
  wfCommentsEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "CommentCollection");
  context.load(wfCommentsEntity);
  wfDecisionsEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "DecisionOptionCollection");
  context.load(wfDecisionsEntity);
  wfParticipantEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "ParticipantCollection");
  context.load(wfParticipantEntity);
  wfDescriptionEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "TaskDescriptionCollection");
  context.load(wfDescriptionEntity);
  wfAttachmentEntity = context.get_web().getAppBdcCatalog().
      getEntity("DUET_WORKFLOW_CORE", "AttachmentCollection");
  context.load(wfAttachmentEntity);

  // Loads a LOB system instances.
  lobSystem = wfEntity.getLobSystem();
  lobSystemInstanceCollection = lobSystem.getLobSystemInstances();
  context.load(lobSystemInstanceCollection);

  context.executeQueryAsync(onLoadingLOBInstanceSucceeded, onError);

  function onLoadingLOBInstanceSucceeded() {
    lobSystemInstance = lobSystemInstanceCollection.get_item(0);
    // Reads the workflow task from SAP using specific finder.
    var identifierValues = [sapParameters.sapOrigin, sapParameters.workitemId];
    var entityIdentity = SP.BusinessData.Runtime.EntityIdentity.
          newObject(context, identifierValues);
    wfEntityInstance = wfEntity.findSpecific(entityIdentity, 
          "ReadSpecificWorkflowTask", lobSystemInstance);
    context.load(wfEntityInstance);

    context.executeQueryAsync(onLoadingWorkflowSucceeded, onError);
  }

  function onLoadingWorkflowSucceeded() {
    // Reads the associated elements for the workflow task.
    var exFilterCollection = wfExtensibleElementEntity.getFilters(
        "GetExtensibleElementsFromWorkflowTaskCollection");
    context.load(exFilterCollection);
    exElementsCollection = wfExtensibleElementEntity.findAssociated(
        wfEntityInstance, "GetExtensibleElementsFromWorkflowTaskCollection", 
        exFilterCollection, lobSystemInstance);
    context.load(exElementsCollection);

    var comFilterCollection = wfCommentsEntity.getFilters(
        "GetCommentsFromWorkflowTaskCollection");
    context.load(comFilterCollection);
    commentsCollection = wfCommentsEntity.findAssociated(
        wfEntityInstance, "GetCommentsFromWorkflowTaskCollection", 
        comFilterCollection, lobSystemInstance);
    context.load(commentsCollection);

    var decFilterCollection = wfDecisionsEntity.getFilters(
        "GetDecisionOptionsFromWorkflowTaskCollection");
    context.load(decFilterCollection);
    decisionsCollection = wfDecisionsEntity.findAssociated(
        wfEntityInstance, "GetDecisionOptionsFromWorkflowTaskCollection", 
        decFilterCollection, lobSystemInstance);
    context.load(decisionsCollection);

    var partFilterCollection = wfParticipantEntity.getFilters(
        "GetParticipantsFromWorkflowTaskCollection");
    context.load(partFilterCollection);
    participantsCollection = wfParticipantEntity.findAssociated(
        wfEntityInstance, "GetParticipantsFromWorkflowTaskCollection", 
        partFilterCollection, lobSystemInstance);
    context.load(participantsCollection);

    var descFilterCollection = wfDescriptionEntity.getFilters(
        "GetDescriptionFromWorkflowTaskCollection");
    context.load(descFilterCollection);
    descriptionCollection = wfDescriptionEntity.findAssociated(
        wfEntityInstance, "GetDescriptionFromWorkflowTaskCollection", 
        descFilterCollection, lobSystemInstance);
    context.load(descriptionCollection);

    var attaFilterCollection = wfAttachmentEntity.getFilters(
        "GetAttachmentsFromWorkflowTaskCollection");
    context.load(attaFilterCollection);
    attachmentCollection = wfAttachmentEntity.findAssociated(
        wfEntityInstance, "GetAttachmentsFromWorkflowTaskCollection", 
        attaFilterCollection, lobSystemInstance);
    context.load(attachmentCollection);

    context.executeQueryAsync(getUpdatedDataSucceeded, onError);
  }

  function getUpdatedDataSucceeded() {
    alert("# of extensible elements: " + exElementsCollection.get_count() + 
      "\\n# of comments: " + commentsCollection.get_count() + 
      "\\n# of decision options: " + decisionsCollection.get_count() + 
      "\\n# of attachments: " + attachmentCollection.get_count() + 
      "\\n# of participants: " + participantsCollection.get_count());
  }

  function onError(sender, e) {
    alert("Request failed. " + e.get_message() +
          "\\n" + e.get_stackTrace());
  }
}

// Retrieves a query string value.
function getQueryStringParameter(paramToRetrieve) {
  var params = document.URL.split("")[1].split("&amp;");
  var strParams = "";
  for (var i = 0; i < params.length; i = i + 1) {
    var singleParam = params[i].split("=");
    if (singleParam[0] == paramToRetrieve)
      return singleParam[1];
  }
}

  ```

6. **AppManifest.xml** ファイルを開きます。
    
  
7. [ **アクセス許可の要求**] で、[ **スコープ**]、[ **Web**]、[アクセス許可] の順に選択し、値を [ *読み取り*  ] に設定します。
    
  
8. [ **F5**] を押して、ソリューションを Duet ワークフロー サイトに配置します。
    
  
9. [ **マイ ワークフロー タスク**] に移動します。ECB のメニューで [ **詳細の表示**] を選択するか、タスクを選択してリボンの [ **詳細の表示**] をクリックします。 **ViewDetails.aspx** にリダイレクトされ、ワークフローに関連付けられた一部の要素のカウントを含む警告が表示されます。
    
  

## HTML と JavaScript を使用したカスタム UI のレンダリング
<a name="bkmk_UseJSOM"> </a>

ViewDetails.aspx で、以下のコードを独自の HTML と JavaScript に置き換えて、独自のカスタム UI をレンダリングします。
  
    
    

```

alert("# of extensible elements: " + exElementsCollection.get_count() +
"\\n# of comments: " + commentsCollection.get_count() + 
"\\n# of decision options: " + decisionsCollection.get_count() + 
"\\n# of attachments: " + attachmentCollection.get_count() + 
"\\n# of participants: " + participantsCollection.get_count());

```


## 詳細ページへの Lync コントロールの追加
<a name="bkmk_UseJSOM"> </a>

以下に、カスタム ユーザー インターフェイスに使用できるオプションを示します。Lync コントロールを追加することで、カスタム ページから Lync の連絡先に通信できるようになります。
  
    
    

### Lync コントロールを追加するには:


1. ソリューション エクスプローラーの [ **Scripts**] フォルダーを右クリックし、JavaScript ファイルを追加して **People.js** という名前を付けます。
    
  
2. 以下のマークアップにより、以下の処理が実行されます。
    
  - タスクの参加者にプレゼンス コントロールを追加します。
    
  
  - Lync 統合を吹き出しに表示し、参加者がクリック 1 回でタスクのコラボレーションを行うことができます。
    
  

    以下のコードを **People.js** ページに貼り付けます。
    


  ```
  
var presenceControlCount = 0;
var appWebURL = '';

function AddPeopleControl(userName) {
  var id = "pc_participants_" + presenceControlCount++;
  var prevId = null;

  return "<div id='" + id + "' ></div>" + 
    "<script>" + 
      "LoadPeopleControl('" +id+ "', '" +prevId+ "', '" +userName+ "');" + 
    "</script>";
}

function LoadPeopleControl(elemId, prevId, user) {
  var prevElement = null;
  var element = $("#" + elemId);
  user = "fareast\\\\" + user.toLowerCase();

  GetUsersInfo(user, element, prevElement);
}

function GetUsersInfo(accountName, htmlElement, waitforElement) {
  var clientContext = SP.ClientContext.get_current();
  var website = clientContext.get_web();

  clientContext.load(website);
  clientContext.executeQueryAsync(onRequestSucceeded, onRequestFailed);

  function onRequestSucceeded() {
    appWebURL = website.get_url();
    var queryURL = appWebURL + 
    "/_api/SP.UserProfiles.PeopleManager/GetPropertiesFor(accountName=@v)?@v='"
     + accountName + "'";

    jQuery.ajax({
      url: queryURL,
      type: "GET",
      headers: {
        "ACCEPT": "application/json;odata=verbose"
      },
      success: function (data) {
        var html = [];
        var i = 0;

        var id = htmlElement[0].id + "_";

        var about_info;

        if (data.d.UserProfileProperties != null) {
          for (i = 0; i < data.d.UserProfileProperties.results.length; i++) {
            if (data.d.UserProfileProperties.results[i].Key == "AboutMe") {
              about_info = data.d.UserProfileProperties.results[i].Value;
            }
          }
        }

        var email = data.d.Email;
        var profileUrl = data.d.UserUrl;
        var pictureUrl = data.d.PictureUrl;
        var name = data.d.DisplayName;

        if (name == null) {
          name = accountName;
        }

        var isFollowed = data.d.IsFollowed;

        var html = [
          "<table>",
          "<tr>",
          "<td>",
            "<span class='ms-spimn-presenceWrapper ms-spimn-imgSize-5x48'> ",
              "<img id='imn_" + id + "_" + i++ + ",type=smtp'" ,
                "onload=\\"IMNRC('", email, "')\\" ",
                "class='ms-spimn-img ms-spimn-presence-offline-5x48x32 '", 
                "title='' name='imnmark' alt='Offline' ", 
                "src='/_layouts/15/images/spimn.png' ", 
                "sip='" + email + "' showofflinepawn='1'>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-imnSpan'>",
              "<a class='ms-imnlink' tabindex='-1'", 
                "onclick='IMNImageOnClick(event);return false;' href='#'>",
                "<img id='imn_" + id + "_" + i++ + ",type=smtp'",
                  "onload=\\"IMNRC('", email, "')\\"",
                  "class=' ms-hide' title='' name='imnmark' alt='Away' ",
                  "src='/_layouts/15/images/spimn.png' ",
                  "sip='" + email + "' showofflinepawn='1'>",
              "</a>",
              "<a class='ms-subtleLink ms-peopleux-imgUserLink' ",
                "onclick='GoToLinkOrDialogNewWindow(this);return false;' ",
                "href='" + profileUrl + "'>",
                "<span style='width: 48px; height: 48px;'" ,
                  "class='ms-peopleux-userImgWrapper'>",
                "<img style='cliptop: 0px; clipright: 48px;" ,
                  "clipbottom: 48px; clipleft: 0px; ",
                  "min-height: 48px; min-width: 48px; max-width: 48px;' ",
                  "class='ms-peopleux-userImg' alt='' src='" +pictureUrl+ "'>",
                "</span>",
              "</a>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-spimn-presenceWrapper ms-spimn-imgSize-5x48'>",
            "<img id='imn_" + id + "_" + i++ + ",type=smtp' ",
              "onload=\\"IMNRC('", email, "')\\" ",
              "class='ms-spimn-img ms-spimn-presence-offline-5x48x32' ",
              "title='' name='imnmark' alt='Offline' ",
              "src='/_layouts/15/images/spimn.png' ",
              "sip='" + email + "' showofflinepawn='1'>",
            "</span>",
          "</td>",
          "<td>",
            "<span class='ms-microfeed-userName ms-textLarge' ",
              "style='font-size: medium;'>" + name + "<br/>",
              "<div id='people_callout_" + id + "_" + i + "' ",
                "class='ms-microfeed-button ms-textSmall" + 
                "ms-secondaryCommandLink ms-microfeed-footerButton' ",
                "align='center' ",
                "style='cursor:pointer; font-size:small;' > more... </div>",
                  "<script>",
                    "RegisterCallOut(\\"people_callout_" + id + "_" + i + 
                      "\\",\\"" + name + "\\",\\"" + encodeURI(about_info) + 
                      "\\",\\"" + profileUrl + "\\",\\"" + isFollowed + "\\");",
                  "</script>",
            "</span>",
          "</td>",
          "</tr>",
          "</table>",
        ];

        htmlElement.html(html.join(''));
      }

    });
  }
  function onRequestFailed(sender, args) {
    alert('Error: ' + args.get_message());
  }

}

function RegisterCallOut(divId, displayName, aboutme, userUrl, isFollowed) {
  if (typeof CalloutManager !== "object" || typeof Callout !== "function" || typeof CalloutAction !== "function")
    return;

  var launchdiv = document.getElementById(divId);
  var calloutId = divId + "_callout";
  var html = [];

  html.push("<br/>");

  if (aboutme == "") {
    html.push("No Information Found about this person.");
  }
  else {
    html.push(decodeURI(aboutme));
  }

  html.push("<hr/>");

  if (isFollowed == true) {
    html.push("<div>You are <b>following</b> this person</div>");
  }
  else {
    html.push("<div >You are <b>not following</b> this person</div>");
  }

  var callout = CalloutManager.createNew({
    launchPoint: launchdiv,
    openOptions: {
      closeCalloutOnBlur: true,
      event: "click",
      showCloseButton: true
    },
    ID: calloutId,
    title: displayName,
    content: html.join("")
  });

  callout.addAction(new CalloutAction({
    text: "View Profile", onClickCallback: 
    function (calloutActionClickEvent, calloutAction) {
      window.open(userUrl);
    }
  }));
}

  ```


> **メモ**
> 企業ネットワークの参加者のユーザー名は SAP のユーザー名と同じです。 
3. **AppManifest.xml** ページを開きます。
    
  
4. [ **アクセス許可の要求**] で、[ **スコープ**]、[ **ユーザー プロファイル**]、[アクセス許可] の順に選択し、値を [ *読み取り*  ] に設定します。
    
  
5. 以下のマークアップをコピーして、 **ViewDetails.aspx** の `PlaceHolderAdditionalPageHead` セクションに貼り付けます。
    
  ```HTML
  
<script src="../Scripts/People.js" type="text/javascript"></script>
  ```

6. 参加者にプレゼンス コントロールを追加するには、以下を呼び出します。 
    
  ```
  AddPeopleControl(userName);
  ```


## その他のリソース
<a name="bk_addresources"> </a>


-  [SharePoint アドイン](http://msdn.microsoft.com/library/cd1eda9e-8e54-4223-93a9-a6ea0d18df70%28Office.15%29.aspx)
    
  
-  [SharePoint 2013 のクライアント ライブラリ コードを使用して基本的な操作を完了する](http://msdn.microsoft.com/library/5a69c9e3-73bf-4ed5-bc19-182056bdb394%28Office.15%29.aspx)
    
  
-  [An introduction to SAP Business Workflow](http://scn.sap.com/docs/DOC-31056)
    
  

