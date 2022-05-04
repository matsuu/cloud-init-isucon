# cloud-init-isucon

## Overview

[cloud-init](https://cloud-init.io/)に対応した環境で[ISUCON](http://isucon.net/)の過去問を構築するためのcloud-config集です。
Apple Silicon(aarch64)にも対応しているため、[Multipass](https://multipass.run/)などと組み合わせればM1 Mac上で過去問環境の構築が可能です。

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
