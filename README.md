# cloud-init-isucon

## Overview

[cloud-init](https://cloud-init.io/)に対応した環境で[ISUCON](http://isucon.net/)の過去問を構築するためのcloud-config集です。
Apple Silicon(aarch64)にも対応しているため、[Multipass](https://multipass.run/)などと組み合わせればM1 Mac上で過去問環境の構築が可能です。

Multipass専用ではなく汎用的に作ってるので、cloud-initに対応した環境、例えば[AWS](https://docs.aws.amazon.com/ja_jp/AWSEC2/latest/UserGuide/user-data.html#user-data-cloud-init)、[Azure](https://docs.microsoft.com/ja-jp/azure/virtual-machines/linux/using-cloud-init)、[Google Cloud](https://cloudinit.readthedocs.io/en/latest/topics/datasources/gce.html)、[Oracle Cloud](https://docs.oracle.com/ja-jp/iaas/Content/Compute/References/images.htm#Oracle__linux-cloud-init)、[さくらのクラウド](https://manual.sakura.ad.jp/cloud/server/cloud-init.html)、[VMware](https://kb.vmware.com/s/article/59557?lang=ja)など、クラウドからローカルまで[幅広く使える](https://cloudinit.readthedocs.io/en/latest/topics/datasources.html)はずです。

- [ISUCON11予選 1台構成](https://github.com/matsuu/cloud-init-isucon/tree/main/isucon11q)

サーバ内の構築にはAnsibleを使っています。Ansibleのplaybookのみ必要な場合は[matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)をどうぞ。

### Others

- [matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)
- [matsuu/aws-isucon](https://github.com/matsuu/aws-isucon)
- [matsuu/docker-isucon](https://github.com/matsuu/docker-isucon)
- [matsuu/oci-arm-isucon](https://github.com/matsuu/oci-arm-isucon)
- [matsuu/vagrant-isucon](https://github.com/matsuu/vagrant-isucon)
- [matsuu/wsl-isucon](https://github.com/matsuu/wsl-isucon)

## License

MIT
