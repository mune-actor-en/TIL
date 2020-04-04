# Docker
オープンソースのコンテナ管理ソフトウェア。<br>
公式URL：[docer docs](https://docs.docker.com/docker-for-mac/docker-toolbox/)
## メリット
- 「Dockerfile」「Dockerイメージ」「Dockerレジストリ」を使って、柔軟にアプリケーション環境やインフラ環境をまとめて管理できる
- 必要な時に起動する、不要な時は破棄するという使い捨ての運用が可能
## デメリット
- 永続的なデータを保存する場合には不向き
---
## Dockerのコンポーネント
コアな機能である「Docker Engine」を中心に各コンポーネントと連携して環境を構築する。
### Docker Engine(Dockerのコア機能)
Dockerイメージの生成、コンテナの起動などを担うコアコンポーネント。
### Docker Registry(イメージの公開・共有)
Dockerイメージの公開や共有するコンポーネント。
### Docker Compose(複数のコンテナを一元管理)
「docker-compose.yml」に複数のコンテナの構成情報を定義し、コマンドを実行することで一元管理するコンポーネント。
### Docker Machine(Dockerの実行環境を構築)
AWSのEC2やMicrosoft Azureなどのクラウド環境などにDockerの実行環境をコマンドで自動生成するコンポーネント。
### Docker Swarm(クラスタ管理)
クラスタとは「群れ・集団」という意味で、密集しているということ。<br>
複数のDockerホストをクラスタ化するコンポーネント。<br>
ただし、昨今ではオープンソースである「Kubernetes」がその役割を担うことが多い。

---
## Linux
DockerはLinuxを使った技術のため、Linuxの基礎知識を知っておく必要がある。<br>
LinuxはオープンソースのサーバOSのこと。
### Linuxカーネル
OSとしてハードウェアやアプリケーションを制御する基本的な機能を有したコアな部分のこと。
### Linuxディストリビューション
パッケージ化して配布されているLinuxのこと。<br>
それぞれにコマンドやライブラリ、アプリケーションに違いがある。<br>
Linuxカーネル以外の部分を「ユーザーランド」という。<br>
ユーザーランドからは直接デバイスにアクセスできないため、Linuxカーネルを軽油して処理が実行される。
### Linuxの主要ディレクトリ
|名称|機能|
|:--|:--|
|/bin|lsコマンドやcpコマンドなどの基本的なコマンド群を格納しているディレクトリ。 |
|/boot|OSの起動に必要なファイルを格納しているディレクトリ。<br>Linuxカーネルは「vmlinuz」というファイル。|
|/dev|ハードディスク・キーボード・デバイスファイルを格納しているディレクトリ。<br>「何もない」を表す「dev/null」も用意されている。「dev/null」はからのファイルの生成や不要な出力を破棄する場合に使われる。|
|/etc|OSやアプリケーションが動作する上で必要な設定ファイルを格納しているディレクトリ。|
|/home|一般ユーザーのホームディレクトリを指す。<br>特権ユーザー(root)は「/root」をホームディレクトリとして使う。|
|/proc|カーネルやプロセスに関する情報を格納しているディレクトリ。<br>配下にある数字のディレクトリはプロセスIDを意味する。|
|/sbin|システム管理用コマンドが格納されているディレクトリ。<br>mountコマンドやrebootコマンドなど。|
|/tmp|一時的に使用するファイルなどを格納しているテンポラリディレクトリ。<br>メモリ上に展開されているため、再起動すると消えてしまう。|
|/usr|各種プログラムやカーネルソースを格納しているディレクトリ。|
|/var|システムの稼働とともに変化するファイルを格納するディレクトリ。<br>稼働ログは「/var/log」、アプリケーションが一時ファイルとして使用するスプールが保存されているのは「/var/spool」|

### Linuxコマンド
|説明|コマンド|補足|
|:--|:--|:--|
|ディレクトリの移動|cd|`cd ディレクトリ名`|
|ディレクトリの中身を確認|ls|ディレクトリに移動後に、`ls`|
|ディレクトリ・ファイルの移動|mv|`mv 移動元ディレクトリ名 移動先ディレクトリ名`|
|ディレクトリ・ファイルのコピー|cp|`cp コピー元ディレクトリ コピー先ディレクトリ名`|
|ターミナルの履歴を消去|clear| |
|ディレクトリの作成|mkdir|`mkdir ディレクトリ名`|
|ファイルの作成|touch|`touch ファイル名`|
|対象コマンドのマニュアルを表示|man|`man 対象コマンド名`|
|コマンドの履歴を表示|history| |
|ファイルの中身を表示|cat|`cat ファイル名`|
|buファイルとファイルの差分を確認|diff|`diff 旧ファイル 新ファイル`|
|tar形式のファイル作成または展開|tar| |
|カレンダーの表示|cal|`cal 4 2020`|
|日付の表示|date| |

---
## Dockerコマンド
|説明|コマンド|補足|
|:--|:--|:--|
|コンテナの起動|docker container start||
|コンテナの停止|docker container stop||
|コンテナの再起動|docker container restart||
|コンテナの削除|docker container rm||
|コンテナの中断|docker container pause||
|コンテナの再開|docker container unpause||
|コンテナの生成／起動|docker container run|`docker container run オプション イメージ名:タグ名 引数`|
|docker container runの対話的実行|docker container run -it --name "test1" centos /bin/cal|コンテナを作成／実行 コンソールに結果を出すオプション コンテナ名 イメージ名 コンテナで実行するコマンド（カレンダーをコンソールに上に表示）|
|コンテナのバックグラウンド実行|docker container run -d centos /bin/ping localhost||
|コンテナの名前変更|docker container rename|`docker container rename 旧コンテナ名 新コンテナ名`|
|コンテナ操作の差分確認|docker container diff|`docker container diff コンテナ識別子（コンテナ名）` <br>・ファイル追加：「A」<br>・ファイル更新：「C」 <br>・ファイル削除：「D」|
|稼働コンテナの一覧表示|docker container ls||
|稼働コンテナへの接続|docker container attach||
|稼働コンテナでプロセス実行|docker container exec|バックグラウンドで実行しているWebサーバのコンテナにアクセスする場合に実行する。<br>※シェルが動作していない場合は「`docker container attach`」を実行して接続しても受け付けられない。|
|稼働コンテナのプロセス確認|`docker container top|docker container top コンテナ名`|
|稼働コンテナのポート転送確認|docker container port|`docker container port コンテナ名`|
|コンテナの稼働を確認|docker container stats||
|コンテナからイメージを作成|docker container commit|`docker container commit オプション コンテナ識別子 イメージ名:タグ名` <br> 【主なオプション】 <br> ・`-author, -a`:作成者を指定する <br>・-message, -m:メッセージを指定する <br>・`-change, -c`:コミット時のDockerfile命令を指定 <br>・`-pause, -p`:コンテナを一時停止してコミットする <br>【例】`docker container commit -a "YUSUKE GODAI" webserver yusukegodai/webfront:1.0`|
|コンテナをtarファイルに出力する|docker container export|`docker container export コンテナ識別子（コンテナ名）` <br> ※コンテナのディレクトリやファイルをまとめて「tarファイル」に作成でき、tarファイルをもとにしてコンテナを稼働できる。<br>・ファイル出力 <br>`docker container export コンテナ名 > 出力ファイル名.tar` <br>・生成されたtarファイルの詳細確認 <br>`tar -tf 出力ファイル名.tar`|
|tarファイルからイメージを作成|docker image import|`docker image import ファイルまたはURL - イメージ名:タグ名`|
|イメージの保存|docker image save|`docker image save -o 保存ファイル名` <br>※保存するファイル名の指定は「`-o`」を使う。|
|イメージの読み込み|docker image load|`docker image load -i 読み込みファイル名` <br>※読み込むファイル名の指定は「`-i`」を使う。<br><br>※`export`と`save`はもとのイメージは同じだが、内部的なディレクトリやファイルの構造に違いがある。そのため、読み込む場合は以下のように使い分ける。<br>・docker image `export` ⇨ docker image `import` <br>・docker image `save` ⇨ docker image `load`|
|不要なイメージやコンテナを一括削除|docker system purne|`docker system prune オプション` <br>・`--all,-a`:使用していないリソースを全て削除 <br>・`--force,-f`:強制的に削除|
|ネットワークの一覧表示|docker network ls||
|ネットワークへの接続|docker network connect オプション ネットワーク名 コンテナ名||
|ネットワークへの切断|docker network disconnect オプション ネットワーク名 コンテナ名||
|ネットワークの詳細確認|docker network inspect オプション ネットワーク名||

|説明|オプション|
|:--|:--|
|標準入力・標準出力・標準エラー出力にアタッチする|--attach, -a|
|コンテナIDをファイルに出力する|-cidfile|
|バックグラウンドで実行する|--detach, -d|
|標準入力を開く|-interactive, -i|
|端末デバイスを確保する|--tty, -t|

---
## Dockerfile
Dockerfileとはテキスト形式のファイルで、エディタで作成し、コンテナを動作させるためのコマンドや設定を記述する。
- Dockerイメージの指定
- Dockerコンテナ内で実行するコマンド
- 環境変数の設定内容
- Dcokerコンテナ内で動作させておくデーモン実行の内容

### Dockerfileの命令
Dockerfileからイメージを作成する：`docker build -t イメージ名:タグ名 Dockerfileの場所（パス）`

|説明|命令文|補足|
|:--|:--|:--|
|ベースとなるイメージを指定|FROM|`FROM イメージ名` <br>`FROM イメージ名:タグ名`：タグを省略した場合、最新バージョン（latest）が適用される。<br> `FROM イメージ名@ダイジェスト名`：ダイジェストとはDockerHubにアップロードすると自動で付与される識別子のことで、「＠マーク」を付けることでイメージを一意に指定できる。<br>`docker image ls --digests`でイメージのダイジェストを確認する。|
|コマンドを実行|RUN|`RUN 実行するコマンド` <br>コマンドの記述は「Shell形式」と「Exec形式」があり、記述が異なる。例として、「Nginx」のインストールコマンドは以下のようになる。<br>【**Shell形式**】：`RUN apt-get install -y ngix` <br>シェルで実行する形式を使って記述する方法で、「/bin/sh -c」を使ってコマンドを実行した時と同じ動作をする。<br>【**Exec形式**】：`RUN ["/bin/bash", "-c", "apt-get install -y ngix"]` <br>シェルを介さずに直接コマンドが実行されるため、コマンドの引数に慣用変数を指定できない。また、コマンドはJSON配列で指定する。|
|コンテナの実行コマンド|CMD|`CMD 実行するコマンド` <br>イメージをもとに生成したコンテナ内でコマンドを実行する場合、CMD命令を使用する。<br> 複数命令を記述した場合、最後のコマンドが有効となる。<br>記述方法は3種類あり、RUN命令と同様に「Shell形式」と「Exec形式」がある。加えて、ENTRYPOINT命令の引数として記述することも可能。|
|コンテナの実行コマンド|ENTRYPOINT|`ENTRYPOINT 実行するコマンド` <br>「CMD命令」は`docker container run`のコマンド実行時に引数（オプション）を上書きできるが、「ENTRYPOINT命令」は必ず実行されるため、上書きできない。そのため、「CMD命令」と「ENTRYPOINT命令」は組み合わせて使う。<br>使い方としては、コンテナ起動時に**毎回必ず実行したいコマンド**は「ENTRYPOINT命令」に記述し、**デフォルトで実行したいコマンド**を「CMD命令」に記述しておくことで、柔軟にオプションを**上書き**できるようにする。|
|ビルド完了後に実行される命令|ONBUILD|`ONBUILD 実行するコマンド`|
|ラベルを指定|LABEL| |
|ポートを出力（指定）|EXPOSE| |
|環境変数|ENV| |
|ファイルやディレクトリを追加|ADD| |
|ファイルのコピー|COPY| |
|ボリュームのマウント|VOLUME| |
|ユーザーの指定|USER| |
|作業ディレクトリ|WORKDIR| |
|Dockerfile内の変数|ARG| |
|システムコールシグナルの設定|STOPSIGNAL| |
|コンテナのヘルスチェック|HEALTHCHECK| |
|デフォルトシェルの設定|SHELL| |


---
## Dockerfile作成〜コンテナ起動の手順
1. 新規ディレクトリを作成して移動する
`mkdir ディレクトリ名 cd $_`
2. Dockerfileを作成する
`touch Dockerfile`
3. エディタでDockerfileに命令を記述する
4. 


---
### Tips
#### プロンプト
$(プロンプト)とは、コマンドを入力できる表示のこと。<br>
- `docker@default:~$`
上記の例は、「docker」というユーザー名で「default」というホスト名のサーバーを操作しているという内容になる。<br>
一般ユーザーは「$」、管理ユーザーは「#」の表記となる。



---
## マルチステージビルドによるアプリケーション開発
マルチステージビルドとは、開発環境用のDockerイメージと本番環境用のDockerイメージを同時に作成できる機能のこと。<br>
できるだけ本番環境は必要なモジュールだけを用意して、軽量な環境にしておくことが望ましい。
### Dockerfileの作成
1. サンプルコードをクローンする<br>
`git clone https://github.com/asashiho/dockertext2`
1. 作業ディレクトリに移動してDockerfileの中身を確認する<br>
`cd dockertext2/chap05/multi-stage`<br>
`open Dockerfile`
### Dockerfileのビルド
1. Dockerfileをビルドする<br>
`docker build -t greet .`
1. Dockerイメージの中身と容量を確認する<br>
`docker image ls`
```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
greet               latest              d4bb30eb92bb        43 seconds ago      6.02MB
<none>              <none>              c440b9aeb038        50 seconds ago      836MB
golang              1.13                d48f500a56fb        3 days ago          803MB
busybox             latest              83aa35aa1c79        13 days ago         1.22MB
nginx               latest              6678c7c2e56c        2 weeks ago         127MB
postgres            latest              dd4fa36a9c2f        3 weeks ago         395MB
ubuntu              latest              72300a873c2c        4 weeks ago         64.2MB
hello-world         latest              fce289e99eb9        14 months ago       1.84kB
```
「golang」が803MBに対して、「greet」はわずか6.02MB。<br>
本番環境用のベースイメージである「busybox」1.22MBにアプリケーション実行に必要なモジュールのみを付加した程度となっている。<br>
軽量なイメージを使用することで、システム全体のリソースを有効に活用することができる。
### Dockerコンテナの起動
1. Dockerコンテナの実行
`docker container run -it --rm greet asa`
`docker container run -it --rm greet --lang=es asa`

---
## トラブルシューティング
### 「Docker Desktop for Mac」をインストールしたが、ローディングが終了せず起動しなかった
#### 解決した方法
Dockerのメニューバーから「Troubleshoot」をクリックして、「Reset to factory defaults」を実行した。
#### その他試したこと
- 前バージョンのDockerをインストールしていたので、Docker Toolboxのアンインストールするバッチを実行
  1. `git clone git@github.com:docker/toolbox.git`で公式が提供している資源を取り込む
    - [Qiita:Mac OSXでDocker Toolboxのアンインストール](https://qiita.com/minamijoyo/items/ec5b35382797ac08e067)
  2. `cd toolbox`でディレクトリに移動して、`sudo osx/uninstall.sh`を実行してアンインストール
- MacがHypervisorフレームワークをサポートしているかどうかを確認
  1. `sysctl kern.hv_support`
  2. MacがHypervisor Frameworkをサポートしている場合、`kern.hv_support: 1`となり、サポートされていない場合は`kern.hv_support: 0`となる。
- ログのライブフローを確認
  1. `pred='process matches ".*(ocker|vpnkit).*" || (process in {"taskgated-helper","launchservicesd", "kernel"} && eventMessage contains[c] "docker")'`
  2. `/usr/bin/log stream --style syslog --level=debug --color=always --predicate "$pred"`
  3. `/usr/bin/log show --debug --info --style syslog --last 1d --predicate "$pred" >/tmp/logs.txt`でログの最終日を確認
- ログを確認
「com.docker.diagnose」というツールが原因を調べてくれる
  1. `/Applications/Docker.app/Contents/MacOS/com.docker.diagnose gather -upload`を実行
  2. 診断ファイルが発行されるので、診断ファイルを`open /tmp/ユーザーID/タイムスタンプ.zip`で開く

---
## 参考URL
- [いまさらだけどDockerに入門したので分かりやすくまとめてみた](https://qiita.com/gold-kou/items/44860fbda1a34a001fc1)