# cloud-init-isucon/private-isu

## Overview

catatsuy/private-isuの環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 22.04 LTSを用意してください。
* ストレージは8GBでは不足します。16GBあれば問題ないと思います。
* Memoryは1GBだと構築中に不足します。2GB以上あれば問題ないと思います。
* CPUコア数は少なくても問題ないですが、多ければ構築が早く完了するはずです。


## Usage

cloud-initに対応した環境にて、アプリケーションサーバをapp.cfg、ベンチマーカーをbenchmarker.cfgにて構築します。

### Multipassでの利用方法

* [Multipass](https://multipass.run/)実行環境を用意します
* このリポジトリ内の `standalone.cfg` を手元に用意します
* 以下を実行します
  ```sh
  multipass launch --name private-isu --cpus 2 --disk 16G --memory 4G --cloud-init standalone.cfg 22.04
  ```
  * cpus, disk, memoryは必要に応じて増減させてください
  * cloud-initは時間がかかるためタイムアウトとなるもののバックグラウンドで構築は行われています
* ログインします
  ```sh
  multipass shell private-isu
  ```
* 進捗確認は以下のコマンドで確認できます
  ```sh
  sudo tail -f /var/log/cloud-init-output.log
  ```

## Bench

構築が終わったらベンチマークを実行します

ベンチマーカーのサーバからアプリケーションサーバにむけて実行します

```sh
cd /home/isucon/private_isu.git/benchmarker
./bin/benchmarker -u ./userdata -t http://{競技用インスタンスのIPアドレス}/
```

## その他の情報

[catatsuy/private-isu](https://github.com/catatsuy/private-isu/)をご確認ください。
