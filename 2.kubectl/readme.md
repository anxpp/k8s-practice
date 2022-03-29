# kubectl

    kubectl是k8s的客户端CLI工具，可可以通过命令行对k8s集群进行操作

## 命令行用法

### 概述

```shell
kubectl [command] [TYPE] [NAME] [flags]
```

- command: 子命令，用于操作资源对象：create、get、describe、delete等
- TYPE: 资源对象的类型（区分大小写）。能以单数、复数或者简写形式表示

```shell
# 等价的三种命令
kubectl get pod podname
kubectl get pods podname
kubectl get po podname
```

- NAME: 资源对象的名称（区分大小写）。如果不指定名称，将返回属于TYPE的全部对象列表， `kubectl get pods` 返回所有Pod列表。

同一个命令可以同时对多个资源对象进行操作，组合多个 TYPE 和 NAME 表示：

- 获取多个相同类型资源的信息（TYPE1 name1 name2）:

```shell
kubectl get pod pod1 pod2
```

- 获取多种不同类型对象的信息(TYPE1/name1 TYPE1/name2 TYPE2/name3):

```shell
kubectl get pod/pod1 replicationcontroller/rc1
```

- 同时应用多个 YAML 文件:

```shell
kubectl get pod -f pod1.yaml -f pod2.yaml
kubectl create -f pod1.yaml -f rc1.yaml -f svc1.yaml
```

### 子命令

    包括资源对象的创建、删除、查看、修改、配置、运行等 

| 子命令          | 语法                                                                                                                               | 说明                                                                    |
|--------------|----------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| alpha        | kubectl alpha SUBCOMMAND [flags]                                                                                                 | 显示Alpha版特性的可用命令                                                       |
| annotate     | kubectl annotate (-f FILENAME &#124; TYPENAME &#124; TYPE/NAME) <br> KEY=VAL [--overwrite] [--all] [--resource-version=v] [flags | 添加或更新资源对象的 annotation 信息                                              |
| api-versions | kubectl api-versions [flags]                                                                                                     | 列出当前系统支持的 API 版本列表，格式为 group/version                                  |
| apply        | kubectl apply -f FILENAME [flags]                                                                                                | 从配置文件或 stdin 中对资源对象进行配置更新                                             |
| attach       | kubectl attach POD -c CONTAINER [flags]                                                                                          | 附着到一个运行中的容器上                                                          |
| auth         | kubectl auth [flags] [options]                                                                                                   | 检查 RBAC 权限设置                                                          |
| autoscale    | kubectl autoscale (-f FILENAME &#124; TYPENAME &#124; TYPE/NAME) <br> [--min=MINPODS] --max=MAXPODS [--cpu-percent=CPU] [flags]  | 对Deployment、ReplicaSet、ReplicationController进行水平自动扩容和缩容的设置            |
| certificate  | kubectl certificate SUBCOMMAND [options]                                                                                         | 修改certificate资源                                                       |
| cluster-info | kubectl cluster-info [flags]                                                                                                     | 显示集群Master和内置服务的信息                                                    |
| completion   | kubectl completion SHELL [flags]                                                                                                 | 输出shell命令的运行结果码(bash zsh)                                             |
| config       | kubectl config SUBCOMMAND [flags]                                                                                                | 修改kubeconfig文件                                                        |
| convert      | kubectl convert -f FILENAME [flags]                                                                                              | 转换配置文件为不同的API版本，文件类型为yaml或json                                        |
| cordon       | kubectl cordon NODE [flags]                                                                                                      | 将Node标记为unshedulable，即“隔离”出集群调度范围                                     |
| cp           | kubectl cp <file-spec-src> <file-spec-dest> [options]                                                                            | 从容器中复制文件/目录到主机或者反向复制                                                  |
| create       | kubectl create -f FILENAME [flags]                                                                                               | 从配置文件或stdin中创建资源对象                                                    |
| delete       |                                                                                                                                  | 根据配置文件、stdin、资源名称或label selector删除资源对象                                |
| describe     |                                                                                                                                  | 描述一个或多个资源对象的详细信息                                                      |
| diff         |                                                                                                                                  | 查看配置文件与当前系统中正在运行的资源对象的差异                                              |
| drain        |                                                                                                                                  | 首先将Node是指为unschedulable，然后删除在该Node上运行的所有Pod，但不会删除不由Api server管理的Pod   |
| edit         |                                                                                                                                  | 编辑资源对象的属性，在线更新                                                        |
| exec         |                                                                                                                                  | 运行一个容器中的命令                                                            |
| explain      |                                                                                                                                  | 对资源对象属性的详细说明                                                          |
| expose       |                                                                                                                                  | 将一个已经存在的RC、Service、Deployment或Pod暴露为一个新的Service                       |
| get          |                                                                                                                                  | 显示一个或多个资源对象的概要信息                                                      |
| kustomize    |                                                                                                                                  | 列出基于kustomization.yaml配置文件生产的api资源对象，参数必须是包含该配置文件的目录名称或者一个git仓库的url地址 |
| label        |                                                                                                                                  | 设置或更新资源对象的labels                                                      |
| logs         |                                                                                                                                  | 在屏幕上打印一个容器的日志                                                         |
| options      |                                                                                                                                  | 显示作用于所有子命令的公共参数                                                       |
| patch        |                                                                                                                                  | 以merge形式对资源对象的部分字段值进行修改                                               |
| plugin       |                                                                                                                                  | 在kubectl命令行使用用户自定义的插件                                                 |
| port-forward |                                                                                                                                  | 将本机的某个端口号映射到Pod的端口号（通常用于测试）                                           |
| proxy        |                                                                                                                                  | 将本机的某个端口号映射带spi server                                                |
| replace      |                                                                                                                                  | 从配置文件或stdin替换资源对象                                                     |
| rollout      |                                                                                                                                  | 管理资源部署，可管理的资源类型包括：Deployment、daemonsets、statefulsets                  |
| run          |                                                                                                                                  | 基于一个镜像在k8s集群中启动一个Deployment                                           |
| scale        |                                                                                                                                  | 扩容、缩容一个Deployment、ReplicaSet、RC或Job中的Pod数量                            |
| set          |                                                                                                                                  | 设置资源对象的某个特性信息，目前仅支持修改容易的镜像                                            |
| taint        |                                                                                                                                  | 设置Node的污点信息，用于将特定的Pod调度到特定的Node的操作                                    |
| top          |                                                                                                                                  | 查看Node或Pod的资源使用情况，需要在集群中运行Metrics Server                              |
| uncordon     |                                                                                                                                  | 将Node设置为schedulable                                                   |
| version      |                                                                                                                                  | 打印系统的版本信息                                                             |
| wait         |                                                                                                                                  | 等待一个或多个资源上的特定条件                                                       |

### kubectl 可操作的资源对象

| 资源对象类型                          | 缩写     | 所属API组 | 受限于命名空间 | 类型（Kind）        | 说明  |
|---------------------------------|--------|--------|:-------:|-----------------|-----|
| bindings                        |        |        |   yes   | Binging         |     |
| componentstatuses               | cs     |        |   no    | ComponentStatus |     |
| configmaps                      | cm     |        |   yes   | ConfigMap       |     |
| endpoints                       | ep     |        |         | EndPoints       |     |
| events                          | ev     |        |         | Event           |     |
| limitranges                     | limits |        |         | LimitRange      |     |
| namespaces                      | ns     |        |         |                 |     |
| nodes                           | no     |        |         |                 |     |
| persistentvolumeclaims          | pvc    |        |         |                 |     |
| persistentvolumes               | pv     |        |         |                 |     |
| pods                            | po     |        |         |                 |     |
| podtemplates                    |        |        |         |                 |     |
| replicationcontrollers          | rc     |        |         |                 |     |
| resourcequotas                  | quota  |        |         |                 |     |
| secrets                         |        |        |         |                 |     |
| serviceaccounts                 | sa     |        |         |                 |     |
| services                        | svc    |        |         |                 |     |
| mutatingwebhookconfigurations   |        |        |         |                 |     |
| validatingwebhookconfigurations |        |        |         |                 |     |
| customresourcedefinitions       |        |        |         |                 |     |
| apiservices                     |        |        |         |                 |     |
| controllerrevisions             |        |        |         |                 |     |
| daemonsets                      |        |        |         |                 |     |
| deployments                     |        |        |         |                 |     |
| replicasets                     |        |        |         |                 |     |
| statefulsets                    |        |        |         |                 |     |
| tokenviews                      |        |        |         |                 |     |
| localsubjectaccessreviews       |        |        |         |                 |     |
| selfsubjectaccessreviews        |        |        |         |                 |     |
| selfsubjectrulereviews          |        |        |         |                 |     |
| subjectaccessreviews            |        |        |         |                 |     |
| horizontalpodautoscalers        |        |        |         |                 |     |
| cronjobs                        |        |        |         |                 |     |
| jobs                            |        |        |         |                 |     |
| certificatesigningrequests      |        |        |         |                 |     |
| leases                          |        |        |         |                 |     |
| endpointslices                  |        |        |         |                 |     |
| flowschemas                     |        |        |         |                 |     |
| prioritylevelconfigurations     |        |        |         |                 |     |
| ingressclasses                  |        |        |         |                 |     |
| ingresses                       |        |        |         |                 |     |
| networkpolicies                 |        |        |         |                 |     |
| runtimeclasses                  |        |        |         |                 |     |
| poddisruptionbudgets            |        |        |         |                 |     |
| podsecuritypolicies             |        |        |         |                 |     |
| clusterroles                    |        |        |         |                 |     |
| rolebindings                    |        |        |         |                 |     |
| roles                           |        |        |         |                 |     |
| priorityclasses                 |        |        |         |                 |     |
| csidrivers                      |        |        |         |                 |     |
| csinodes                        |        |        |         |                 |     |
| storageclasses                  |        |        |         |                 |     |
| volumeattachments               |        |        |         |                 |     |

### kubectl 公共参数说明

--参数名=取值

| 参数名及示例                           | 取值  | 说明                          |
|----------------------------------|-----|-----------------------------|
| --dd-dir-header=false            |     |                             |
| --alsologtostderr=false          |     |                             |
| --as=''                          |     |                             |
| --as-group                       |     |                             |
| --cache-dir='/root/,kube/cache'  |     |                             |
| --certificate-authority=''       |     |                             |
| --client-key=''                  |     |                             |
| --cluster=''                     |     |                             |
| --context=''                     |     |                             |
| --insecure-skip-tls-verify=false |     |                             |
| --kubeconfig=''                  |     |                             |
| --log-backtrace-at=:0            |     |                             |
| --log-dir=''                     |     |                             |
| --log-file=''                    |     |                             |
| --log-file-max-size=1800         |     |                             |
| --log-flush-frequency=5s         |     |                             |
| --logtostderr=true               |     |                             |
| --match-server-version=false     |     |                             |
| -n, --namespace=''               |     |                             |
| --password=''                    |     |                             |
| --profile='none'                 |     |                             |
| --profile-output='profile.pprof' |     |                             |
| --request-timeout='0'            |     |                             |
| -s, --server=''                  |     |                             |
| --skip-headers=false             |     |                             |
| --skip-log-headers=false         |     |                             |
| --stderrthreshold=2              |     |                             |
| --tls-server-name=''             |     |                             |
| --token=''                       |     |                             |
| --username=''                    |     |                             |
| --user=''                        |     |                             |
| --v=0                            |     |                             |
| --vmodule=                       |     |                             |
| --warnings-as-errors=false       |     | 将warning视为error，以非0的退出码直接退出 |

    
### kubectl 格式化输出
    

| 格式                              | 说明                           |
|-----------------------------------|--------------------------------------------|
| -o custom-columns=<spec>          | 根据自定义列名进行输出，以逗号分割          |
| -o custom-columns-file=<filename> | 设置自定义列明的配置文件名称               |
| -o json                           | 以JSON格式显示结果                  |
| -o jsonpath=<template>            | 输出jsonpath表达式定义的字段信息          |
| -o jsonpath-file=<filename>       | 输出jsonpath表达式定义的字段信息，来源于文件 |
| -o name                           | 仅输出资源对象的名称                   |
| -o wide                           | 输出额外信息。对于Pod，将输出Pod所在的Node名称 |
| -o yaml                           | 以yaml格式显示结果                  |



### kubectl 常用操作示例
