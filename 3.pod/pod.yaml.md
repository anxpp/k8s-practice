# YAML 格式的 Pod 定义文件

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: string
  namespace: string
  labels:
    - name: string
  annotations:
    - name: string
spec:
  containers:
    - name: string
      image: string
      imagePullPolicy: [Always | Never | ifNotPresent]
      command: [string]
      args: [string]
      workingDir: string
      volumeMounts:
        - name: string
          mountPath: string
          readOnly: boolean
      ports:
        - name: string
          containerPort: int
          hostPort: int
          protocol: string
      env:
        - name: string
          value: string
      resources:
        limits:
          cpu: string
          memory: string
        requests:
          cpu: string
          memory: string
      livenessProbe:
        exec:
          command: [string]
        httpGet:
          path: string
          port: int
          host: string
          scheme: string
          httpHeaders:
            - name: string
              value: string
        tcpSocket:
          port: int
        initialDelaySeconds: int
        timeoutSeconds: int
        periodSeconds: int
        sucessThreshold: int
        failureThreshold: int
      securityContext: 
        privileged: boolean
  restartPolicy: [Always | Never | OnFailure]
  nodeSelector: object
  imagePullSecrets:
    - name: string
  hostNetwork: boolean
  volumes:
    - name: string
      emptyDir: {}
      hostPath:
        path: string
      secret:
        secretName: string
        items:
          - key: string
            path: string
      configMap:
        name: string
        items:
          - key: string
            path: string
          
```

| 属性                 | 类型   | 必选     | 默认值 | 说明  |
|--------------------|------|-----|-----|-----|
| apiVersion         | true | string | -   | 版本号 |
| kind               |      |     |     |     |
| metadata           |      |     |     |     |
| metadata.name      |      |     |     |     |
| metadata.namespace |      |     |     |     |
|                    |      |     |     |     |
|                    |      |     |     |     |


