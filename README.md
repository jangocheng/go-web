<!-- TOC -->

- [go-web](#go-web)
    - [特性](#特性)
    - [如何使用](#如何使用)
    - [各文件介绍](#各文件介绍)
        - [docker.sh](#dockersh)
        - [Dockerfile](#dockerfile)
        - [Makefile](#makefile)
        - [cache](#cache)
        - [config](#config)
        - [AMQP](#amqp)
    - [ETC](#etc)

<!-- /TOC -->

# go-web
一个基于 Go语言 的Web项目模板, 以便快速高效的使用Go构建服务端.

## 特性
1. 使用 dep 管理项目依赖
1. 完整的项目结构, 并且可以使用Docker容器技术一键生成镜像并上传到阿里云服务器.
    - 阿里云私有容器服务上传需要先登录 `sudo docker login --username=name registry.cn-hangzhou.aliyuncs.com`
2. 使用 `.ini` 配置文件管理配置
3. 使用dao层管理数据库的查询
    - 目前已经封装了 mysql/redis/influx 数据库. 如需连接池支持, 还需开发.
4. 部分消息队列封装
    - 目前支持 kafka
5. 服务层使用原生 `net/http` 实现.
    - 基于iris框架的模板: https://github.com/everywan/go-web-iris
6. 支持微信公众号, 可以接收/响应微信公众号消息.

## 如何使用
1. 全项目替换 `github.com/everywan/go-web` 为自己的项目路径.
2. 使用dep初始化项目, 确保所有的引用初始化到vendor中(dep/手动均可)
3. 符合 restful 规范

## 各文件介绍
### docker.sh
> [源码](docker.sh)
1. 用于自动构建docker镜像, 并且上传到阿里云容器.
    - 默认镜像名称: `basename $PWD`:$version`

### Dockerfile
> [源码](Dockerfile)
1. 实际构建docker的程序. 使用 Dockerfile 方式构建镜像

### Makefile
> [源码](Makefile)
1. 用于构建可执行的二进制文件

### cache
1. redis 的客户端, 用于缓存

### config
1. 配置各数据库客户端

### AMQP

## ETC
没必要死记整个结构, 其实整个结构都是项目需要才添加进来的

首先, main函数, 构建服务层, 创建handler或者service

handler中, 需要更改数据库, 为了降低耦合, 对数据库分层, 所以有了 创建dao

在dao中, 分开对底层和操作层, 所以创建db层, db层负责数据库链接的获取, dao层负责各表的CURD.

在db层中, 需要获取数据库的配置信息, 所以有两种方案, 一是使用配置文件, 二是使用配置类, 所以有了 config层.

在db层中, 可以对某些数据缓存, 所以有了cache层

在handler中, 有些需要与其他服务通信. 所以有了client层, 负责与其他进程通信.

在handler中, 需要明确数据的结构. 所以有了schema层, 负责定义数据的结构.

日志库 使用iris自带日志库.
