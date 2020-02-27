# Homebrew
## インストール済みパッケージを一覧を表示する
`brew list`
## 最新ではないパッケージ一覧を表示する
`brew outdated`
## パッケージファイルの中身を確認する
`brew cat パッケージ名`
## パッケージをインストールする
`brew install`
## パッケージをアンインストールする
`brew uninstall`
## パッケージの有効化する
`brew link`
## パッケージの無効化する
`brew unlink`
## リンク切れになっているパッケージを削除する
`brew prune`
## brew関連の問題を検出する
`brew doctor`
## brewに関わるローカルの情報を確認する
`brew --config`
## ゴミデータを消去
- 古いバージョンを利用するしている場合は実行しない
`brew cleanup`
## 起動状況を確認
`brew services list`
## インストールされているパッケージの情報を取得する
`brew info パッケージ名`
## インストールされているパッケージのバージョンを切り替える
- `brew info`で確認してから実行すると使いやすい
`brew switch パッケージ名`

---
## 参考URL
- [homebrewとは何者か。仕組みについて調べてみた](https://qiita.com/omega999/items/6f65217b81ad3fffe7e6)
