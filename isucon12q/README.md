# cloud-init-isucon/isucon12q

## Overview

isucon12予選とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 22.04 LTSを用意してください。

## Usage

### Multipassでの利用方法

[トップのREADME](../README.md)をご確認ください。

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  cd bench
  ./bench
  ```

## 本来の設定と異なるところ

* SSL証明書は自己署名のものを用意しています
* コンテスト実施時のインスタンスタイプはc5.large(2vCPU, 4GBメモリー)が3台構成です
* x86\_64でない環境はMySQL公式パッケージがないためDocker内のmysqlはmariadbになっています

## FAQ

### launchの途中でエラーが発生した

スリープモードなどの影響で失敗した場合は以下で再試行ができます。

```sh
sudo /var/lib/cloud/instance/scripts/runcmd
```

### プログラムの動かし方がわからない

以下をご確認ください。

* [ISUCON12 レギュレーション](https://isucon.net/archives/56671734.html)
* [ISUCON12 予選当日マニュアル](https://gist.github.com/mackee/4320c18919c8f6f1867849378a17e651)
* [ISUCON12 予選リポジトリ](https://github.com/isucon/isucon12-qualify)

### Multipassで動作確認ができない

以下のコマンドでIPアドレスの確認ができます。

```sh
multipass info isucon12q
```

ブラウザから `https://表示されたIPアドレス/` にアクセスしてみてください。自己署名証明書のためブラウザでエラーが発生します。証明書は `/etc/nginx/tls/fullchain.pem` にあるので手元の証明書ストアに登録することで回避できるはずです。

### Multipassで作成した環境を削除したい

```sh
multipass delete -p isucon12q
```
