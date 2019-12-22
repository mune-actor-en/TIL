# Ruby on Railsの基礎
コマンド実行時には **`bundle exec`か`bin/`をつけて実行すること**を推奨。理由は以下の通り。
- 意図しないバージョンが起動
- gem同士の依存関係を正しく解決できない

`bundle exec`をつけない場合、通常はインストールされている最新バージョンのgemが実行される。
## アプリケーション作成
### コマンド
- `rails new アプリケーション名`

Railsのバージョンを指定して作成する場合(例：5.2.2バージョンのRailsを指定)
- `rails _5.2.2 new アプリケーション名`
### scaffoldを使って作成する
- `rails g scaffold モデル名 カラム名:データ型`
#### 概要
 役割：最低限必要な「コントローラー」「モデル」「ビュー」「ルーティング」を全て一括作成できる。
※MVCを理解してから扱う方をオススメする

---
## Model
役割：データベースとやりとりする
Model名は**単数形**にする
### コマンド
- `rails g model モデル名`
---
## View
役割：画面を表示させる
---
## Controller
役割：処理を実行（ロジックを書く）
- コントローラー名は**複数形**にする
- DBと直接紐付かない場合（info, homeなど）は単数形`にするパターンもある
### コマンド
- `rails g controller コントローラ名`
---
## Route
役割：URLと処理を紐づける
### コマンド
- `rails routes`
---
## Gem
### Gemfile
### Gemfile.lock
---
## Migration
役割：処理を実行（ロジックを書く）
### コマンド
- `rails db:migrate`
  - これをしないとDBが最新の状態にならない
#### カラム名を変更する
- `rename_column :テーブル名, :変更前のカラム名, :変更後のカラム名`
  - 作成されたファイルにある`changeメソッド`に変更したいカラム名を記述する
#### DBの指定
- rails new アプリケーション名 -d mysql
- rails new アプリケーション名 -d postgresql
#### DBの更新
- `bundle exec rake db:create`
### 補足
-  一度作成したら`ファイルを直接「変更」「削除」は一切しない`
---
### Error
#### `ActiveRecord::NoDatabaseError`
- データベースを作成していない場合
  - `rails db:create`