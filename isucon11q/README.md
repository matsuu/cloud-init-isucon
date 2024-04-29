# cloud-init-isucon/isucon11q

## Overview

isucon11予選とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 20.04 LTSを用意してください。
* ストレージは8GBでは不足します。16GBあれば問題ないと思います。
* Memoryは1GBだと構築中に不足します。2GB以上あれば問題ないと思います。
* CPUコア数は少なくても問題ないですが、多ければ構築が早く完了するはずです。

## Usage

### Multipassでの利用方法

* [Multipass](https://multipass.run/)実行環境を用意します
* このリポジトリ内の `isucon11q.cfg` を手元に用意します
* 以下を実行します
  ```sh
  multipass launch --name isucon11q --cpus 2 --disk 16G --memory 4G --cloud-init isucon11q.cfg 20.04
  ```
  * cpus, disk, memoryは必要に応じて増減させてください
  * cloud-initは時間がかかるためタイムアウトとなるもののバックグラウンドで構築は行われています
* ログインします
  ```sh
  multipass shell isucon11q
  ```
* 進捗確認は以下のコマンドで確認できます
  ```sh
  sudo tail -f /var/log/cloud-init-output.log
  ```

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  cd bench
  # 本番同様にnginx(https)へアクセスを向けたい場合
  ./bench -all-addresses 127.0.0.11 -target 127.0.0.11:443 -tls -jia-service-url http://127.0.0.1:4999
  # isucondition(3000)へ直接アクセスを向けたい場合
  ./bench -all-addresses 127.0.0.11 -target 127.0.0.11:3000 -jia-service-url http://127.0.0.1:4999
  ```

## 本来の設定と異なるところ

* SSL証明書は自己署名のものを用意しています
* 1台構成で動作するように以下のファイルを書き換えています
    * /etc/hosts
    * /var/lib/cloud/scripts/per-instance/generate-env.sh

## FAQ

### 途中でエラーが発生した

スリープモードなどの影響で失敗した場合は以下で再試行ができます。

```sh
sudo /var/lib/cloud/instance/scripts/runcmd
```

### プログラムの動かし方がわからない

以下をご確認ください。

* [ISUCON11 予選当日マニュアル](https://github.com/isucon/isucon11-qualify/blob/main/docs/manual.md)
* [ISUCON11 予選リポジトリ](https://github.com/isucon/isucon11-qualify)

### Multipassで動作確認ができない

以下のコマンドでIPアドレスの確認ができます。

```sh
multipass info isucon11q
```

ブラウザから `https://表示されたIPアドレス/` にアクセスしてみてください。自己署名証明書のためブラウザでエラーが発生します。証明書は `/etc/nginx/certificates/tls-cert.pem` にあるので手元の証明書ストアに登録することで回避できるはずです。

### Multipassで作成した環境を削除したい

```sh
multipass stop isucon11q
multipass delete isucon11q
multipass purge
```

### cloud-init以外の環境で試したい

[matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)をご利用ください。


### ブラウザでサイトに正常にアクセスできない

Firefoxだとダメかもしれません。他のブラウザを試してみてください。

### ブラウザでアクセスした際にログインができない

ISUCON11予選は認証周りが複雑な構成になっている関係で、SSH接続時にポートフォワードを行う必要があります。

```sh
ssh -L 5000:127.0.0.1:5000 ubuntu@サーバのパブリックIPアドレス
```

このAMIではJIA Mockが常時起動するようになっています。そのため手動で起動する必要はありませんが制御は jiaapi-mock.service で可能です。
```sh
sudo systemctl stop jiaapi-mock
sudo systemctl start jiaapi-mock
sudo systemctl status jiaapi-mock
```

### ↑を試したがmacOSでログインできない

macOS環境の場合、TCPポート5000はすでに使用されている場合があります。その場合は適宜5001などに変更してください。
```sh
ssh -L 5001:127.0.0.1:5000 ubuntu@サーバのパブリックIPアドレス
```

5000から別ポートに変更した場合、各種プログラムのDEFAULT_JIA_SERVICE_APIの変更も必要です。以下は5001に変更した場合の例です。
```patch
diff --git a/webapp/go/main.go b/webapp/go/main.go
index 80a9d95..3e6a80c 100644
--- a/webapp/go/main.go
+++ b/webapp/go/main.go
@@ -32,7 +32,7 @@ const (
        frontendContentsPath        = "../public"
        jiaJWTSigningKeyPath        = "../ec256-public.pem"
        defaultIconFilePath         = "../NoImage.jpg"
-       defaultJIAServiceURL        = "http://localhost:5000"
+       defaultJIAServiceURL        = "http://localhost:5001"
        mysqlErrNumDuplicateEntry   = 1062
        conditionLevelInfo          = "info"
        conditionLevelWarning       = "warning"
diff --git a/webapp/nodejs/src/app.ts b/webapp/nodejs/src/app.ts
index 76bf7b0..db5ab70 100644
--- a/webapp/nodejs/src/app.ts
+++ b/webapp/nodejs/src/app.ts
@@ -110,7 +110,7 @@ const conditionLimit = 20;
 const frontendContentsPath = "../public";
 const jiaJWTSigningKeyPath = "../ec256-public.pem";
 const defaultIconFilePath = "../NoImage.jpg";
-const defaultJIAServiceUrl = "http://localhost:5000";
+const defaultJIAServiceUrl = "http://localhost:5001";
 const mysqlErrNumDuplicateEntry = 1062;
 const conditionLevelInfo = "info";
 const conditionLevelWarning = "warning";
diff --git a/webapp/perl/lib/IsuCondition/Web.pm b/webapp/perl/lib/IsuCondition/Web.pm
index 7bc57a0..06f30fa 100644
--- a/webapp/perl/lib/IsuCondition/Web.pm
+++ b/webapp/perl/lib/IsuCondition/Web.pm
@@ -23,7 +23,7 @@ use constant {
     FRONTEND_CONTENTS_PATH       => "../public",
     JIA_JWT_SIGNING_KEY_PATH     => "../ec256-public.pem",
     DEFAULT_ICON_FILE_PATH       => "../NoImage.jpg",
-    DEFAULT_JIA_SERVICE_URL      => "http://localhost:5000",
+    DEFAULT_JIA_SERVICE_URL      => "http://localhost:5001",
     MYSQL_ERRNUM_DUPLICATE_ENTRY => 1062,
 };

diff --git a/webapp/python/main.py b/webapp/python/main.py
index 469b076..fc46442 100644
--- a/webapp/python/main.py
+++ b/webapp/python/main.py
@@ -27,7 +27,7 @@ CONDITION_LIMIT = 20
 FRONTEND_CONTENTS_PATH = "../public"
 JIA_JWT_SIGNING_KEY_PATH = "../ec256-public.pem"
 DEFAULT_ICON_FILE_PATH = "../NoImage.jpg"
-DEFAULT_JIA_SERVICE_URL = "http://localhost:5000"
+DEFAULT_JIA_SERVICE_URL = "http://localhost:5001"
 MYSQL_ERR_NUM_DUPLICATE_ENTRY = 1062


diff --git a/webapp/ruby/app.rb b/webapp/ruby/app.rb
index b097e84..9a08277 100644
--- a/webapp/ruby/app.rb
+++ b/webapp/ruby/app.rb
@@ -18,7 +18,7 @@ module Isucondition
     FRONTEND_CONTENTS_PATH = '../public'
     JIA_JWT_SIGNING_KEY_PATH = '../ec256-public.pem'
     DEFAULT_ICON_FILE_PATH = '../NoImage.jpg'
-    DEFAULT_JIA_SERVICE_URL = 'http://localhost:5000'
+    DEFAULT_JIA_SERVICE_URL = 'http://localhost:5001'

     MYSQL_ERR_NUM_DUPLICATE_ENTRY = 1062
     CONDITION_LEVEL_INFO = 'info'
diff --git a/webapp/rust/src/main.rs b/webapp/rust/src/main.rs
index 67ad587..643610b 100644
--- a/webapp/rust/src/main.rs
+++ b/webapp/rust/src/main.rs
@@ -12,7 +12,7 @@ const CONDITION_LIMIT: usize = 20;
 const FRONTEND_CONTENTS_PATH: &str = "../public";
 const JIA_JWT_SIGNING_KEY_PATH: &str = "../ec256-public.pem";
 const DEFAULT_ICON_FILE_PATH: &str = "../NoImage.jpg";
-const DEFAULT_JIA_SERVICE_URL: &str = "http://localhost:5000";
+const DEFAULT_JIA_SERVICE_URL: &str = "http://localhost:5001";
 const MYSQL_ERR_NUM_DUPLICATE_ENTRY: u16 = 1062;
 const CONDITION_LEVEL_INFO: &str = "info";
 const CONDITION_LEVEL_WARNING: &str = "warning";
```

コンパイルが必要な言語は必要に応じてコンパイルしてください。サービス再起動もおそらく必要です。
