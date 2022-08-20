# cloud-init-isucon

## Overview

[cloud-init](https://cloud-init.io/)に対応した環境で[ISUCON](http://isucon.net/)の過去問を構築するためのcloud-config集です。
Apple Silicon(aarch64)にも対応しているため、[Multipass](https://multipass.run/)などと組み合わせればM1 Mac上で過去問環境の構築が可能です。

汎用的に作ってるので、cloud-initに対応した環境、例えば[AWS](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/user-data.html#user-data-cloud-init)、[Azure](https://docs.microsoft.com/ja-jp/azure/virtual-machines/linux/using-cloud-init)、[Google Cloud](https://blog.woohoosvcs.com/2019/11/cloud-init-on-google-compute-engine/)、[Oracle Cloud](https://docs.oracle.com/ja-jp/iaas/Content/Compute/References/images.htm#Oracle__linux-cloud-init)、[さくらのクラウド](https://manual.sakura.ad.jp/cloud/server/cloud-init.html)、[Multipass](https://multipass.run/)、[VMware](https://kb.vmware.com/s/article/59557?lang=ja)など、クラウドからローカルまで[幅広く使える](https://cloudinit.readthedocs.io/en/latest/topics/availability.html)はずです。

### cloud-config

* 公式
  * [ISUCON10予選](https://github.com/matsuu/cloud-init-isucon/tree/main/isucon10q)
  * [ISUCON11予選](https://github.com/matsuu/cloud-init-isucon/tree/main/isucon11q)
* 非公式
  * [Pixiv社内ISUCON2016](https://github.com/matsuu/cloud-init-isucon/tree/main/private-isu)
  * [ISUCON11事前講習](https://github.com/matsuu/cloud-init-isucon/tree/main/isucon11-prior)
  * [Kayac社内ISUCON2022](https://github.com/matsuu/cloud-init-kayac-isucon-2022)
  * [NRI-ISUCON2022](https://github.com/matsuu/cloud-init-isucon/tree/main/nri-isucon2022)

サーバ内の構築にはAnsibleを使っています。Ansibleのplaybookのみ必要な場合は[matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)をどうぞ。

## Usage

### Multipassでの利用方法

* [Multipass](https://multipass.run/)実行環境を用意します
  * macOSユーザーは[Homebrew](https://brew.sh/)を使ってインストール可能です
  ```sh
  brew install multipass
  ```
  * Windowsユーザーは[アプリ インストーラー](https://apps.microsoft.com/store/detail/app-installer/9NBLGGH4NNS1)を使ってインストール可能です
  ```powershell
  winget.exe install Multipass
  ```
* このリポジトリ内のcfgファイルを手元に用意します
* cloud-initを使って起動します
  ```sh
  git clone https://github.com/matsuu/cloud-init-isucon.git
  cd cloud-init-isucon

  # isucon10予選の場合
  multipass launch --name isucon10q --cpus 2 --disk 20G --mem 4G --timeout 60 --cloud-init isucon10q/isucon10q.cfg 20.04

  # isucon11予選の場合
  multipass launch --name isucon11q --cpus 2 --disk 20G --mem 4G --timeout 60 --cloud-init isucon11q/isucon11q.cfg 20.04

  # isucon11本選の場合
  multipass launch --name isucon11f --cpus 2 --disk 20G --mem 4G --timeout 60 --cloud-init isucon11f/isucon11f.cfg 20.04

  # isucon12予選の場合
  multipass launch --name isucon12q --cpus 2 --disk 20G --mem 4G --timeout 60 --cloud-init isucon12q/isucon12q.cfg 22.04
  ```
  * cpus, disk, memoryは必要に応じて増減させてください
  * 末尾の `20.04` はUbuntuのバージョンです
  * cloud-initは時間がかかるため以下のようなメッセージが表示される場合がありますが、バックグラウンドで構築は継続しています
    ```
    launch failed: The following errors occurred:
    timed out waiting for initialization to complete
    ```
* 進捗は `/var/log/cloud-init-output.log` で確認できます
  ```sh
  multipass exec nri-isucon2022 -- tail -f /var/log/cloud-init-output.log
  ```
* `multipass shell` でシェルが使えます
  ```sh
  multipass shell isucon11q
  ```
* 環境の停止再開は stop/start です
  ```sh
  multipass stop isucon11q
  multipass start isucon11q
  ```
* ディスクの拡張は同梱のqemu-img(qemu-img-hyperkit)で可能です
  ```sh
  multipass stop isucon11q
  sudo /Library/Application\ Support/com.canonical.multipass/bin/qemu-img-hyperkit resize /var/root/Library/Application\ Support/multipassd/qemu/vault/instances/(インスタンス名)/*.img 30G
  multipass start isucon11q
  ```
* 環境の削除は `multipass delete --purge` です
  ```sh
  multipass delete --purge isucon11q
  ```

## Others

* [matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)
* [matsuu/aws-isucon](https://github.com/matsuu/aws-isucon)
* [matsuu/docker-isucon](https://github.com/matsuu/docker-isucon)
* [matsuu/oci-arm-isucon](https://github.com/matsuu/oci-arm-isucon)
* [matsuu/vagrant-isucon](https://github.com/matsuu/vagrant-isucon)
* [matsuu/wsl-isucon](https://github.com/matsuu/wsl-isucon)

## License

MIT
