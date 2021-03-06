# Ruby on Rails
Ruby on Rails に関する知識をまとめていく。

## 概念
### Model
役割：データベースとやりとりする
Model 名は**単数形**にする

#### コマンド
- `rails g model モデル名`

---
### View

役割：画面を表示させる

---
### Controller
役割：処理を実行（ロジックを書く）

- コントローラー名は**複数形**にする
- DB と直接紐付かない場合（info, home など）は単数形`にするパターンもある

#### 命名規則

|     |     |     |     |
| :-- | :-- | :-- | :-- |
|     |     |     |     |
|     |     |     |     |
|     |     |     |     |

#### コマンド
- `rails g controller コントローラ名`

---
### Route
役割：URL と処理を紐づける

#### コマンド
- `rails routes`

---
### Migration

役割：処理を実行（ロジックを書く）

#### コマンド

- `rails db:migrate`
  - これをしないと DB が最新の状態にならない

#### カラム名を変更する

- `rename_column :テーブル名, :変更前のカラム名, :変更後のカラム名`
  - 作成されたファイルにある`changeメソッド`に変更したいカラム名を記述する

#### DB の指定
- `rails new アプリケーション名 -d mysql`
- `rails new アプリケーション名 -d postgresql`
- 
#### DB の更新
- `bundle exec rake db:create`

#### 補足
- 一度作成したら`ファイルを直接「変更」「削除」は一切しない`

---
## Gem

---
## Gemfile
### Gemfile から使わなくなった gem を削除

1. `bundle update`を実行し、Gemfile.lock から削除した gem の情報を削除
2. `bundle clean`を実行。すると gem が削除された旨が表示され、インストール先の gems フォルダから実体が削除される。

---

## Gemfile.lock

- Gemfile で指定したバージョンの gem が記録される
- 書き換えるのは禁止（Gemfile との整合性が合わなくなる）
- バージョン変更する場合は、Gemfile で書き換えする
  - 書き換え後、`bundle install` or `bundle update`を実行

---
### gem
#### RubyGemsのバージョンを確認する
`gem -v`
#### RubyGemsによりインストールされているgem一覧を確認する
`gem list`

---
### bundler
#### RubyGemsからbundlerをインストールする
`gem install bundler`
#### バージョンを確認する（デフォルトで最新を表示）
`bundle -v`
#### 特定のバージョンをインストールする
`gem install bundler -v 1.17.3`
#### インストール状態を確認する
`gem list bundler`
#### バージョン指定して「実行」する
`bundle _1.17.3_ install`

---
### APIモード
Railsでアプリを作成するとき、MVCの「V」は不要、つまりフロントエンド側はReactやVueなどにしたい場合にAPIモードを使用する。<br>
RailsでAPIモードを使用するには以下のコマンドを入力する。<br>
`rails new アプリ名 --api`


---
## シチュエーション別コマンド
### 新規アプリケーション作成
- `rails new アプリケーション名`

#### Rails のバージョンを指定して作成する場合(例：5.2.2 バージョンの Rails を指定)
- `rails _5.2.2 new アプリケーション名`

#### scaffold を使って作成する
- `rails g scaffold モデル名 カラム名:データ型`
  最低限必要な「コントローラ」「モデル」「ビュー」「ルーティング」を全て一括作成できる。
  ※MVC を理解してから扱う。

### macOSのソフトウェアアップデート後にrailsコマンドが受け付けられない
`brew upgrade`
Homebrew + Homebrewでインストールしたパッケージのアップデートが実行される

---
## Tips
### ディレクトリを新規作成したときにまずやること
- アプリを作成するディレクトリに移動したら`bundle init`を実行する
  - `bundle init`を実行すると、Gemfile が生成されるのでその中で gem を指定して書き換える
  - PC にある本体に gem をインストールできるが、上記のやり方を推奨

---
### コマンド実行時には \*\*`bundle exec`か`bin/`をつけて実行する

推奨理由は以下の通り。
- 意図しないバージョンが起動
- gem 同士の依存関係を正しく解決できない
  `bundle exec`をつけない場合、通常はインストールされている最新バージョンの gem が実行される。

---
### Module
色々な場所で使えるようにする小さなまとまり。（便利ツール集）
- 例：装備品（ビームサーベル・シールドなど）

---
### Class
一つで処理が完結する大きなまとまり。

- 例：機体（ガンダム・ザクなど）

---
### サービスクラスの考え方
「app」の中に入れる（モデルディレクトリに入れることもあるが特に決まっていない）
どこにも属さないモデルの処理をサービスクラスにまとめる

- Ruby on Rails にはサービスクラスの概念は元々ない
  - 最初からサービスクラスを作るなら Java
- モデルの容量が増えて、コントローラに処理が残るようならサービスクラスに入れていく
- ドメイン駆動設計（DDD）が参考になる

---




---
### エラー対処
#### `ActiveRecord::NoDatabaseError`
- データベースを作成していない場合
  - `rails db:create`

#### `LoadError`

**原因**
- Homebrew でパッケージをアップグレードしたことにより、OpenSSL のバージョンが上がった
- ruby が参照していたバージョンの OpenSSL がなくなったことにより、エラーが発生

**対処**
rbenv で ruby を再インストール

---
## テンプレート

### 新規アプリケーション作成手順

1. バージョン切り替えを考慮して、Ruby のバージョンを指定する
`rbenv local バージョン名`

2. 新規アプリケーションを作成したいディレクトリで Gemfile を作成する
`bundle init`

3. Rails をインストールする
`bundle install`

4. バージョン確認する
`bundle exec rails -v`

5. DB を指定して新規アプリケーションを作成する（例：PostgreSQL を指定）
`bundle exec rails new アプリ名 -d postgresql`

6. DB を構築する
`bin/rails db:create`

---
