# Pod 详解

## Pod是什么

Pod是一个或多个容器的封装。

比如紧耦合的两个服务：web 服务和 redis 服务，可以打包到一个 Pod 中，他们之间互相访问只需要通过 localhost 就能通信，相当于在同一个环境中。

对应的 web-redis-pod.yaml 文件大致如下

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: web-redis
  labels:
    name: web-redis
spec:
  containers:
    - name: web
      image: abc.xyz/web:version
      ports:
        - containerPort: 80
    - name: redis
      image: abc.xyz/redis:version
      ports:
        - containerPort: 6379
```

使用 kubectl create 命令创建 Pod：

```yaml
kubectl create -f web-redis-pod.yaml
```