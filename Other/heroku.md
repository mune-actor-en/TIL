# 手順
## ログインする(メールアドレスとパスワードを入力)
- `heroku login`
- `heroku open` // ブラウザ表示してこちらでログインでも可
## リポジトリ初期化＆アプリ作成
- `git init`
- `git add .`
- `git commit -m "first commit"`
- `heroku create アプリ名` // herokuのリポジトリをgitに追加
## デプロイ
- `git push heroku master`
## ブラウザで確認
- `heroku open`
---
# 操作コマンド
## Heroku上でコマンド実行
- `heroku run "コマンド"`
## アプリを指定してコマンド実行
- `heroku コマンド --app アプリ名`
## 作成したアプリ一覧を表示
- `heroku list`
## アプリ情報を表示
- `heroku apps:info`
## ログアウト
- `heroku logout`
##  ログ確認
- `heroku logs`
##  ログ確認(リアルタイム)
- `heroku logs -t`
## アプリのステータス確認
- `heroku ps`
## Rails console
- `heroku run rails c`
## rakeコマンド
- `heroku run bundle exec rake db:migrate`
## 削除
- `heroku apps:destroy --app アプリ名`
- アプリ名を再入力すると削除完了
---
## Herokuで実行しているアプリのRubyバージョンを表示
- `heroku run "ruby -v"`
## PostgreSQLへログイン
- `heroku pg:psql`
### ログイン中のPostgreSQLコマンド操作
#### DB一覧を表示
- `\l`
#### テーブル一覧を表示
- `\d`
#### カラム一覧を表示
- `\d カラム名`
#### データ閲覧時、コマンド入力に戻る
- `q`
#### ログアウト
- `\q`

# エラー
```html
//「名前は小文字か数字かダッシュのみ」という意味
Name must start with a letter, end with a letter or digit and can only contain lowercase letters, digits,and dashes.
```