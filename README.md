# 前提

- LINEのアカウントを持っていて、PCログインができる。
- IBM IDを持っていて、Buluemixにログインができる。

[lineチャットボットとwatsonを連携する](https://medium.com/@taiponrock/lineチャットボットとwatsonを連携する-8a7d89a49e57)

# IBMアカウントを作成し、ログイン

[ダッシュボード](https://console.bluemix.net/dashboard/apps)

# Watson Assistantの作成

1. IBM Cloudのカタログから、Watson Assistantを選択します
2. サービス資格情報メニューから資格情報を作成し、以下のデータを控える
    - APIエンドポイントURL
    - ユーザー
    - パスワード
3. Watson Assistantワークスペースの作成
4. Createボタンをクリックし会話フローの新規作成
5. ダウンロードしたJSONを読み込み

# Watson Assistant に学習データを登録する

[公式](https://www.ibm.com/developerworks/jp/cognitive/library/cc-watson-chatbot-conversation/index.html)

# Node-RED Starterを選択

1. Bluemix -> カタログ-> スターター・キット → Node-RED Starter
2. 必要情報を入力して、アプリを作成する
3. アプリが有効になるまで待つ
4. Go to your Node-RED flow editor

# Node-REDでの準備

1. Watson Assistant APIをバインド
2. フローを作成する
    - 完成版のフローは[こちら](https://github.com/taijihagino/chatbot-lineapi-watsonapi)からダウンロード
3. URLのパスを設定
    - `/line_hook`
    - `https://<IBM Cloudで作った際のNode-REDのアプリ名>.mybluemix.net/<http inで指定したパス>/`
4. AssistantのワークスペースのIDを設定
    - IBM CloudでNode-REDとAssistant APIをバインドしてあればこのような設定画面となり、ユーザー、パスワードの資格情報を使った設定は不要
5. LINEで生成したアクセストークンをセット
    ```
      var post_request = {
        “headers”: {
          “content-type”: “application/json; charset=UTF-8”,
          “Authorization”: “ Bearer “ + “{LINEで生成するアクセストークン}”
        },
        “payload”: {
          “replyToken”: flow.get(“replyToken”),
          “messages”: [
            {
              “type”: “text”,
              “text”: msg.payload.optext + “ฅ^•ﻌ•^ฅ”
            }
          ]
        }
      };
      return post_request;
    ```
6. LINE@のリプライ用URLを指定
    - https://api.line.me/v2/bot/message/reply
