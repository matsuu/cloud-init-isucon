# cloud-init-isucon/isucon11-prior

## Overview

isucon11事前講習([isucon11-prior](https://github.com/isucon/isucon11-prior))とほぼ同じ環境を構築するためのcloud-configです。

## Requirements

* Ubuntu 20.04 LTSを用意してください。
* ストレージは8GBでは不足します。16GBあれば問題ないと思います。
* Memoryは1GBだと構築中に不足します。2GB以上あれば問題ないと思います。
* CPUコア数は少なくても問題ないですが、多ければ構築が早く完了するはずです。
* 2コアで10分から15分程度かかります

## Usage

cloud-initに対応した環境にて isucon11-prior.cfg を使いサーバを構築します。

## Bench

構築が終わったらsshでログインし、ベンチマークを実行します

```
$ sudo su - isucon
$ ./bin/benchmarker 
```
