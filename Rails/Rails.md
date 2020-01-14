# Ruby on Railsの基礎
コマンド実行時には **`bundle exec`か`bin/`をつけて実行すること**を推奨。理由は以下の通り。
- 意図しないバージョンが起動
- gem同士の依存関係を正しく解決できない

`bundle exec`をつけない場合、通常はインストールされている最新バージョンのgemが実行される。
## アプリケーション作成
- アプリを作成するディレクトリに移動したら`bundle init`を実行する
  - `bundle init`を実行すると、Gemfileが生成されるのでその中でgemを指定して書き換える
  - PCにある本体にgemをインストールできるが上記のやり方を推奨
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
#### Gemfileから使わなくなったgemを削除
1. `bundle update`を実行し、Gemfile.lockから削除したgemの情報を削除
2. `bundle clean`を実行。するとgemが削除された旨が表示され、インストール先のgemsフォルダから実体が削除される。
### Gemfile.lock
- Gemfileで指定したバージョンのgemが記録される
- 書き換えるのは禁止（Gemfileとの整合性が合わなくなる）
- バージョン変更する場合は、Gemfileで書き換えする
  - 書き換え後、`bundle install` or `bundle update`を実行
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
- `rails new アプリケーション名 -d mysql`
- `rails new アプリケーション名 -d postgresql`
#### DBの更新
- `bundle exec rake db:create`
### 補足
-  一度作成したら`ファイルを直接「変更」「削除」は一切しない`
---
### Error
#### `ActiveRecord::NoDatabaseError`
- データベースを作成していない場合
  - `rails db:create`

### 新規アプリケーション作成手順
1. バージョン切り替えを考慮して、Rubyのバージョンを指定する
- `rbenv local バージョン名`
2. 新規アプリケーションを作成したいディレクトリでGemfileを作成する
- `bundle init`
3. Railsをインストールする
- `bundle install`
4. バージョン確認する
- `bundle exec rails -v`
5. DBを指定して新規アプリケーションを作成する
- `bundle exec rails new アプリ名 -d postgresql`
6. DBを構築する
- `bin/rails db:create`