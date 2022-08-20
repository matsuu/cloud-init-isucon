# cloud-init-isucon/isucon11f

## Overview

isucon11本選とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 20.04 LTSを用意してください。
* ストレージは8GBでは不足します。16GBあれば問題ないと思います。
* Memoryは1GBだと構築中に不足します。2GB以上あれば問題ないと思います。
* CPUコア数は少なくても問題ないですが、多ければ構築が早く完了するはずです。

## Usage

構築手順は[トップのREADME](../README.md)をご確認ください。

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  cd benchmarker
  ./bin/benchmarker -target localhost:443 -tls
  ```

## 本来の設定と異なるところ

* 本来のサーバはc5.largeをベースにメモリを2GB制限(CPU 2コア、メモリ2GB)の3台構成です
* 本来のベンチマークサーバはc4.xlarge(CPU 4コア、メモリ7.5GB)です
* SSL証明書は自己署名のものを用意しています

## FAQ

[トップのREADME](../README.md)のFAQもご確認ください

### プログラムの動かし方がわからない

以下をご確認ください。

* [ISUCON11 本選当日マニュアル](https://github.com/isucon/isucon11-final/blob/main/docs/manual.md)
* [ISUCON11 本選リポジトリ](https://github.com/isucon/isucon11-final)

### ブラウザで動作確認ができない

手元のPCのhostsファイルに以下を追記してください。

```
${サーバのIPアドレス} isucholar.t.isucon.dev
```

追記したらブラウザから `https://isucholar.t.isucon.dev/` にアクセスしてみてください。
EdgeやChromeであれば [thisisunsafe](https://www.google.com/search?q=thisisunsafe) でアクセスを試みてください。
アクセスすると証明書エラーが発生する可能性があります。証明書は `/etc/nginx/tls/fullchain.pem` にあるので手元の証明書ストアに登録することで回避できるはずです。
