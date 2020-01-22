# Git
## シチュエーション別コマンド集
### 新規リポジトリ作成〜初めてのpushまでの手順
1. Githubのサイトへ行き、「New Repogitory」をクリック
2. リポジトリ名を入力し、「Create」をクリック
3. ローカル環境で以下のコマンドを入力する（新しくファイル等を作る）
`git init`
`git add`
`git commit -m "first commit"`
4. Githubのサイトで新規作成したリポジトリの「HTTPS」か「SSH」のパスをコピーする
5. 再びローカル環境に戻り、以下のコマンドを入力する
`git remote add origin HTTPSかSSH`
6. 向き先を登録したので、先ほどコミットした資源をpushする
`git push -u origin master`

---
### プルリクからローカル反映までの手順
1. Githubのサイトへ行き、「Pull request」のタブをクリック
2. 「Marge pull request」ボタンをクリック後、コメントを入力して「Confirm marge」ボタンをクリック
3. 「Pull request successfully marged」と表示されればOK
4. ローカルに反映させるため、ターミナルで以下のコマンドを入力
- `git fetch`
- `git merge origin/master`(リモートブランチ名)
### 旧リポジトリの資源を新規リポジトリへクローンしたい
1. リモート元から資源をクローンする  
- ` git clone リモート元（URLの場合：https:～.git）`
- ` git clone リモート元（SSHの場合：〇〇@github.com:～.git）`
2. 「.git」ディレクトリを削除する （リモート元のコミットなどの履歴が残っているため）
- `git rm -r .git`
3. 新しく「.git」ディレクトリを作成する
- `git init`
4. リモート先を追加する（「.git」ディレクトリを新規作成したが、リモート先情報が空なので指定する必要がある）
- ` git remote add 新しいリモート先（URLの場合：https:～.git）`
- ` git remote add 新しいリモート先（SSHの場合：〇〇@github.com:～.git）`
5. 新しいリモート先になっているか確認する
- `git remote -v`
6. リモート元でクローンした資源を「add」「commit」する（「4. リモート先を追加する」と同様、「.git」ディレクトリを新規作成したため）
- `git add .`
- `git commit -m "first commit"`
7. 新しいリモート先に資源をアップロードする
- `git push 新しいリモート先（URLの場合：https:～.git）`
- `git push 新しいリモート先（SSHの場合：〇〇@github.com:～.git）`
#### 参考
- [gitリポジトリの複製](https://qiita.com/syuji-higa/items/e380289502c7896daf0f)
- [Github公式（Adding a remote）](https://help.github.com/en/github/using-git/adding-a-remote)
## 設定
### MacのGitバージョンアップ手順
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
### Macのターミナルでコマンド補完を有効にする
- `.bashrc`に設定すると補完される
- `bash-completion`はコマンド補完のツールのこと
  - `source /usr/local/etc/bash_completion.d/git-prompt.sh`
  - `source /usr/local/etc/bash_completion.d/git-completion.bash`
### すでにGit管理下にある、またはコミット済みのファイルを無視したい
`git rm --cached 相対パス名`
「--cached」オプションは、Gitの管理対象から外すが、手元からは削除されず残しておきたい場合につける