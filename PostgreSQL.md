# PostgreSQL
## 基本コマンド
### 起動
`brew services start postgresql`
### 停止
`brew services stop postgresql`
### 再起動
`brew services restart postgresql`
### データベースの確認
`psql postgres`
### データベース一覧の確認
`\l`
### データベースへ接続
`\c postgres` or `psql -U ユーザー名`
### データベースの切断
`\q`
### 既存のロールを一覧表示
`\du`
### ロールの作成（ログイン権限なし）
`CREATE ROLE ユーザー名;`
### ロールの作成（ログイン権限あり）
`CREATE USER ユーザー名;`
### ログイン権限を付与
`ALTER ROLE ユーザー名 LOGIN;`
### ログイン権限を剥奪
`ALTER ROLE ユーザー名 NOLOGIN;`
### データベース作成権限を付与
`ALTER ROLE ユーザー名 CREATEDB;`
### データベース作成権限を剥奪
`ALTER ROLE ユーザー名 NOCREATEDB;`
### ロールの削除
`DROP ROLE ユーザー名;`
### PostgreSQLの場所を確認
`which psql(postgresql)`
### バージョン確認
`psql -V` or `postgres --version`
### 起動状況を確認
`brew services list`
### シンボリックを作成(例：postgresqlの11バージョン)
`brew link postgresqlÉ11 --force`
### PostgreSQLのバージョンを指定してパスを通す(例：postgresqlの11バージョン)
`echo 'export PATH="/usr/local/opt/postgresqlÉ11/bin:$PATH"'`
### 「.bash_profile」を更新
`source ~/.bash_profile`

---
## トラブルシューティング
### うまく起動しない場合は「.pid」ファイルを削除する
正常に終了しなかった場合に、プロセスIDを記述している「.pid」ファイルが作成される。<br>
`rm /usr/local/var/postgres/postmaster.pid`