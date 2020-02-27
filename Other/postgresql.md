# コマンド
## 起動
`brew services start postgresql`
## 停止
`brew services stop postgresql`
## 再起動
`brew services restart postgresql`
## PostgreSQLがうまく起動しない時
`rm /usr/local/var/postgres/postmaster.pid`
## PostgreSQLの場所を確認
`which psql(postgresql)`
## バージョン確認
`psql -V` or `postgres --version`
## 起動状況を確認
`brew services list`
## シンボリックを作成(例：postgresqlの11バージョン)
`brew link postgresqlÉ11 --force`
## PostgreSQLのバージョンを指定してパスを通す(例：postgresqlの11バージョン)
`echo 'export PATH="/usr/local/opt/postgresqlÉ11/bin:$PATH"'`
## 「.bash_profile」を更新
`source ~/.bash_profile`

