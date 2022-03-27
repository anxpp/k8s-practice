# 留言板例子

## 创建 mysql 服务

### mysql-deploy.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: mysql
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - image: mysql:5.7
        name: mysql
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "123456"
```

查看服务发布状态(启动需要时间，ready有延迟)和pod

```shell
# kubectl get deploy
NAME    READY   UP-TO-DATE   AVAILABLE   AGE
apiVersion: v1
mysql   1/1     1            1           2m37s

# kubectl get pods
NAME                   READY   STATUS    RESTARTS   AGE
mysql-9b877f47-8lfgb   1/1     Running   0          2m34s
```

### mysql-svc.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
    - port: 3306
  selector:
    app: mysql
```

查看 mysql-service 状态：

```shell
# kubectl get svc mysql

NAME    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
mysql   ClusterIP   10.106.90.16   <none>        3306/TCP   10s
```

## 创建 myweb 服务

### myweb-deploy.yaml

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: myweb
  name: myweb
spec:
  replicas: 2
  selector:
    matchLabels:
      app: myweb
  template:
    metadata:
      labels:
        app: myweb
    spec:
      containers:
        - image: kubeguide/tomcat-app:v1
          name: myweb
          ports:
            - containerPort: 8080
          env:
            - name: MYSQL_SERVICE_HOST
              value: 10.106.90.16
```

### myweb-svc.yaml

```yaml
apiVersion: v1
kind: Service
metadata:
  name: myweb
spec:
  type: NodePort
  ports:
    - port: 8080
      nodePort: 30001
  selector:
    app: myweb
```

### 访问浏览器（master或者任意node节点ip）

**http://192.168.153.135:30001/demo**