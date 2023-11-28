# cloud-init-isucon/isucon13

## Overview

isucon13とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 22.04 LTSを用意してください。

## Usage

構築手順は[トップのREADME](../README.md)をご確認ください。

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  ./bench run
  ```

## 本来の設定と異なるところ

* 本番ではドメインとして `*.u.isucon.dev` が使われていましたが、[devトップレベルドメインはHSTS preload-listに含まれており](https://ja.wikipedia.org/wiki/.dev)、正規のSSL証明書がないとアクセスできないため `*.u.isucon.local` に書き換えています
* SSL証明書は自己署名のものを用意しています
* コンテスト実施時のインスタンスタイプはc5.large(2vCPU, 4GBメモリー)が3台構成です

## FAQ

[トップのREADME](../README.md)のFAQもご確認ください

### プログラムの動かし方がわからない

以下をご確認ください。

* [ISUCON13 レギュレーション](https://isucon.net/archives/57768216.html)
* [ISUCON13 当日マニュアル](https://github.com/isucon/isucon13/blob/c52b359fc6e733e1193ac8e9835bea23856566e7/docs/cautionary_note.md)
* [ISUCON13 アプリケーションマニュアル](https://github.com/isucon/isucon13/blob/c52b359fc6e733e1193ac8e9835bea23856566e7/docs/isupipe.md)
* [ISUCON13 リポジトリ](https://github.com/isucon/isucon13)

### ブラウザで動作確認ができない

手元のPCのhostsファイルに以下を追記してください。

```
${サーバのIPアドレス} pipe.u.isucon.local
```

追記したらブラウザから `https://pipe.u.isucon.local/` にアクセスしてみてください。
アクセスすると証明書エラーが発生する可能性があります。証明書は `/etc/nginx/tls/` 配下にあるので手元の証明書ストアに登録することで回避できるはずです。
