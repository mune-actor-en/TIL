# Docker
公式URL：[docer docs](https://docs.docker.com/docker-for-mac/docker-toolbox/)

---
## トラブルシューティング
### 「Docker Desktop for Mac」をインストールしたが、ローディングが終了せず起動しなかった
#### 解決した方法
Dockerのメニューバーから「Troubleshoot」をクリックして、「Reset to factory defaults」を実行した。
#### その他試したこと
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