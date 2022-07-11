# cloud-init-isucon/nri-isucon2022

## Overview

nri-isucon2022とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 18.04 LTSを用意してください。
* ベンチマーカーがx86-64用バイナリ配布のためApple Siliconではおそらく動きません。
* ストレージは8GBでは不足します。16GBあれば問題ないと思います。
* Memoryは1GBだと構築中に不足します。2GB以上あれば問題ないと思います。
* CPUコア数は少なくても問題ないですが、多ければ構築が早く完了するはずです。

## Usage

### Multipassでの利用方法

[トップのREADME](../README.md)をご確認ください。

## Bench

* 構築が終わったらベンチマークを実行します
  ```sh
  sudo -i -u isucon
  TARGET_PORT=80 TargetIpAddr=127.0.0.1 DATA_PATH=/home/isucon/isubnb/bench/initial-data /home/isucon/isubnb/bench/benchmarker
  ```

## 本来の設定と異なるところ

* コンテスト実施時のインスタンスタイプはt3.small(2vCPU, 2GBメモリー)が1台、t2.small(1vCPU, 2GBメモリー)が2台の3台構成です

## FAQ

### launchの途中でエラーが発生した

スリープモードなどの影響で失敗した場合は以下で再試行ができます。

```sh
sudo /var/lib/cloud/instance/scripts/runcmd
```

### プログラムの動かし方がわからない

以下をご確認ください。

* [nri-isucon2022 レギュレーション](https://github.com/nri-isucon/nri-isucon2022/blob/main/docs/regulation.md)
* [nri-isucon2022 当日マニュアル](https://github.com/nri-isucon/nri-isucon2022/blob/main/docs/manual.md)
* [nri-isucon2022 リポジトリ](https://github.com/nri-isucon/nri-isucon2022)

### Multipassで動作確認ができない

以下のコマンドでIPアドレスの確認ができます。

```sh
multipass info nri-isucon2022
```

ブラウザから `http://表示されたIPアドレス/` にアクセスしてみてください。

### Multipassで作成した環境を削除したい

```sh
multipass delete -p nri-isucon2022
```
