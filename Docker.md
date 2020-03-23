# Docker
## 公式URL：[docer docs](https://docs.docker.com/docker-for-mac/docker-toolbox/)
---
## コマンド
参考URL：[いまさらだけどDockerに入門したので分かりやすくまとめてみた](https://qiita.com/gold-kou/items/44860fbda1a34a001fc1)
### 
---
## マルチステージビルドによるアプリケーション開発
マルチステージビルドとは、開発環境用のDockerイメージと本番環境用のDockerイメージを同時に作成できる機能のこと。<br>
できるだけ本番環境は必要なモジュールだけを用意して、軽量な環境にしておくことが望ましい。
### Dockerfileの作成
1. サンプルコードをクローンする
`git clone https://github.com/asashiho/dockertext2`
1. 作業ディレクトリに移動してDockerfileの中身を確認する
`cd dockertext2/chap05/multi-stage`
`open Dockerfile`
### Dockerfileのビルド
1. Dockerfileをビルドする
`docker build -t greet .`
1. Dockerイメージの中身と容量を確認する
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