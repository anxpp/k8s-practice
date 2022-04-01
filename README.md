# k8s-practice
<!-- TOC -->

## 目录说明

- 0.machine_init: 集群物理环境（虚拟机）的初始化
- 1.cluster_init: k8s集群的初始化
- 2.kubectl: 命令行工具的使用说明
- 3.pod: Pod概念详解
- 4.service概念详解: Pod概念解析
- demo: 示例

## 概述

    

## 核心概念

### Pod

Pod是一个或多个容器的封装，使得这几个容器之间如同在同一个环境下运行的，相互之间可以通过 localhost 访问，各个容易也能共享 Pod 级别的存储卷 Volume。

### Service

Pod只负责环境的封装，不对外提供服务，如需提供外部访问能力，需要通过 Service 进行暴露。

### 控制器

控制器自动维护Pod的多个副本

## 参考文档&书籍

- 《kubernetes 权威指南》