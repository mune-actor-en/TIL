# Firebase
## API
- 参考URL：[Firebase CLI リファレンス](https://firebase.google.com/docs/cli?hl=ja#setup_update_cli)
### CLIコマンド
#### ログイン
`fireabase login`
#### Fireabaseプロジェクトに関連付けされた`firebase.json`構成ファイルを生成する
`firebase init`
#### デプロイ
`firebase deploy`
#### スクリプトで実行されるタスクを設定する
`curl -X POST -H 'Content-type: application/json' -d 渡すデータ名を指定（.jsonなど） Cloud FunctionsのURLを指定`
- 【例】`curl -X POST -H "Content-Type: application/json" -d @dataset.json URL`
  - `-X`:POSTメソッドを指定
  - `-H`:データ形式に｀JSON｀を指定
  - `-d`:渡すデータ名を指定
  - `@`:ローカルのJSONファイルを指定
  - `URL`:Cloud FunctionsのURLを指定
#### デプロイされたCloud Functionsのログを出力
`firebase functions:log`
#### Firebaseプロジェクト一覧を確認する
`firebase projects:list`


---