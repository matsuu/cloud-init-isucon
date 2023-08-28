# cloud-init-isucon/isucon12q

## Overview

isucon12予選とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 22.04 LTSを用意してください。

## Usage

構築手順は[トップのREADME](../README.md)をご確認ください。

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  cd bench
  ./bench -target-addr 127.0.0.1:443
  ```

## 本来の設定と異なるところ

* 本番ではドメインとして `*.t.isucon.dev` が使われていましたが、[devトップレベルドメインはHSTS preload-listに含まれており](https://ja.wikipedia.org/wiki/.dev)、正規のSSL証明書がないとアクセスできないため `*.t.isucon.local` に書き換えています
* SSL証明書は自己署名のものを用意しています
* コンテスト実施時のインスタンスタイプはc5.large(2vCPU, 4GBメモリー)が3台構成です
* x86\_64でない環境はMySQL公式パッケージがないためDocker内のmysqlはmariadbになっています

## FAQ

[トップのREADME](../README.md)のFAQもご確認ください

### プログラムの動かし方がわからない

以下をご確認ください。

* [ISUCON12 レギュレーション](https://isucon.net/archives/56671734.html)
* [ISUCON12 予選当日マニュアル](https://gist.github.com/mackee/4320c18919c8f6f1867849378a17e651)
* [ISUCON12 予選リポジトリ](https://github.com/isucon/isucon12-qualify)

### ブラウザで動作確認ができない

手元のPCのhostsファイルに以下を追記してください。

```
${サーバのIPアドレス} admin.t.isucon.local
${サーバのIPアドレス} isucon.t.isucon.local
${サーバのIPアドレス} kayac.t.isucon.local
```

追記したらブラウザから `https://admin.t.isucon.local/` や `https://isucon.t.isucon.local/` にアクセスしてみてください。
アクセスすると証明書エラーが発生する可能性があります。証明書は `/etc/nginx/tls/fullchain.pem` にあるので手元の証明書ストアに登録することで回避できるはずです。
