---
toc: true
layout: post
description: 踩过的一些坑，顺便测试一下fastpages
categories: [markdown]
title: docker添加GPU支持
---

# 起因

安装pytorch的时候发现NVIDIA驱动版本太低，各种不兼容，于是从390（CUDA 8.0）升级到了430（CUDA 10.1），重启后docker挂了，于是重装、各种配置。

虽然过程多少有些曲折，但结果是好的——以后再也不用在GPU相关软件上抓耳挠腮了。

关于软件版本的选择，已经在MongoDB、werkzeug、mongoengine、pymongo、faiss、pytorch上吃过很多亏了，经验就是：

`驱动、软件版本还是尽可能使用最新稳定版！出问题后才考虑二分法回退测试。`

## 遇到的坑

## nvidia-docker安装

[nvidia-docker安装方法](https://github.com/NVIDIA/nvidia-docker#quickstart)

[nvidia-docker WIKI](https://github.com/NVIDIA/nvidia-docker/wiki#setting-up)

### docker-compose的支持

#### --gpus选项不支持

docker最新版本0.19使用`--gpus [all/N]`选项可以原生支持GPU资源：`docker run --gpus all nvidia/cuda:10.0-base nvidia-smi`

但是[docker-compose不支持该选项](https://forums.docker.com/t/how-to-use-gpus-option-with-docker-compose/78558).

所以这条路走不通。

#### 使用runtime方式

- 测试runtime的方法：`docker run --runtime=nvidia nvidia/cuda:10.0-base nvidia-smi`

- 添加nvidia runtime：

    `/etc/docker/daemon.json`

    ```
    {
        "runtimes": {
            "nvidia": {
                "path": "/usr/bin/nvidia-container-runtime",
                "runtimeArgs": []
            }
        }
    }
    ```

    然后重启服务。

- docker-compose.yml

    需要注意两点：只有2.3版本支持runtime选项，设置`runtime: nvidia`。Demo：

    ```
    $ cat docker-compose.yml
    version: '2.3'

    services:
    nvidia-smi-test:
    runtime: nvidia
    image: nvidia/cuda:9.2-runtime-centos7
    ```

参考：https://github.com/docker/compose/issues/6691
