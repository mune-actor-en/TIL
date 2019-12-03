# よく使うコマンド

# シチュエーション別コマンド集

# 設定
## MacのGitバージョンアップ手順
1. 現在のバージョンを確認する
- `git --version`
2. 現在のGitがある場所を確認する
- `git which`
3. Homebrewが管理しているGitの最新バージョンを確認する
- `brew info git | grep stable`
4. HomebrewでGitをインストールする
- `brew install git`
5. Gitの場所（パス）を確認する
- 古いバージョンのgitの環境変数と紐づいている
  - `git /usr/bin/git --version`
- HomebrewでインストールしたGit
  - `/usr/local/bin/git --version`
6. パス設定の優先順位を確認する（表示が上にあるほど優先度が高い）
- `cat /etc/paths`
```rb
/usr/local/bin # ← これが一番優先順位が高い。
/usr/bin
/bin
/usr/sbin
/sbin
```
7. 環境設定の隠しファイルをコマンドラインで開く
- `open ~/.bash_profile`
8. Gitだけに以下の内容を追記して個別パスを指定する
```rb
if [ -f ~/.bashrc ]; then
 . ~/.bashrc
fi
# 以下の2行を追記する
PATH=/usr/local/git/bin:$PATH
export PATH
```
9.  隠しファイルを再読み込みする
- `source ~/.bash_profile`
10. Gitのバージョンを確認する
- `git --version`
```rb
git version 2.24.0
```
11. 場所（パス）を確認する
- `which git`
```rb
/usr/local/bin/git
```
## Macのターミナルでコマンド補完を有効にする
- `.bashrc`に設定すると補完される
- `bash-completion`はコマンド補完のツールのこと
`source /usr/local/etc/bash_completion.d/git-prompt.sh`
`source /usr/local/etc/bash_completion.d/git-completion.bash`