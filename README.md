# cloud-init-isucon

## Overview

[cloud-init](https://cloud-init.io/)に対応した環境で[ISUCON](http://isucon.net/)の過去問を構築するためのcloud-config集です。
Apple Silicon(aarch64)にも対応しているため、[Multipass](https://multipass.run/)などと組み合わせればM1 Mac上で過去問環境の構築が可能です。

- [ISUCON11予選 1台構成](https://github.com/matsuu/multipass-isucon/tree/master/isucon11q)

サーバ内の構築にはAnsibleを使っています。Ansibleのplaybookのみ必要な場合は[matsuu/ansible-isucon](https://github.com/matsuu/ansible-isucon)をどうぞ。

### Others

- [ansible-isucon](https://github.com/matsuu/ansible-isucon)
- [aws-isucon](https://github.com/matsuu/aws-isucon)
- [docker-isucon](https://github.com/matsuu/docker-isucon)
- [vagrant-isucon](https://github.com/matsuu/vagrant-isucon)
- [oci-arm-isucon](https://github.com/matsuu/oci-arm-isucon)
- [wsl-isucon](https://github.com/matsuu/wsl-isucon)

## License

MIT
