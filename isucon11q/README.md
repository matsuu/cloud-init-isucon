# cloud-init-isucon/isucon11q

## Overview

isucon11予選とほぼ同じ環境を構築するためのcloud-configです。

## Usage

### Multipassでの利用方法

* [Multipass](https://multipass.run/)実行環境を用意する
* このリポジトリ内の `isucon11q.cfg` を手元に用意する
* 以下を実行する
  ```sh
  multipass launch --name isucon11q --cpus 2 --disk 8G --mem 4G --cloud-init isucon11q.cfg 20.04
  ```
* cloud-initは時間がかかるためタイムアウトとなるがバックグラウンドで構築は行われている。ログインする。
  ```sh
  multipass shell isucon11q
  ```
* 進捗確認は以下のコマンドで。
  ```sh
  sudo tail -f /var/log/cloud-init-output.log
  ```

## Bench

* 構築が終わったらベンチマークを実行する
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

### プログラムの動かし方がわからない

以下をご確認ください。

- [ISUCON11予選問題](https://github.com/isucon/isucon11-qualify)

### Multipassで動作確認ができない

以下のコマンドでIPアドレスの確認ができます。

```sh
multipass info isucon11q
```

ブラウザから `https://表示されたIPアドレス/` にアクセスしてみてください。

### Multipassで作成した環境を削除したい

```sh
multipass stop isucon11q
multipass delete isucon11q
multipass purge
```

### cloud-init以外の環境で試したい

[matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)をご利用ください。
