## ingress
管理对集群中的服务(通常是HTTP)的外部访问的API对象。
Ingress可以提供负载平衡、SSL终端和基于名称的虚拟主机。

## 服务部署
`执行一下yaml文件`
```
## 命名空间
kubectl create -f ingress-namespace.yaml

## 对外暴露端口
kubectl create -f ingress-service.yaml

## deployment服务部署
kubectl create -f ingress-deployment.yaml
```
## 查看执行结果
`查看pod`
>[root@k8s-master ~]# kubectl get pod -n ingress-nginx 
```
NAME                                        READY   STATUS    RESTARTS   AGE
nginx-ingress-controller-6889cffb4d-gshdf   1/1     Running   0          6d4h
```
`查看service`
>[root@k8s-master ~]# kubectl get service -n ingress-nginx 
```
NAME            TYPE       CLUSTER-IP    EXTERNAL-IP   PORT(S)                      AGE
ingress-nginx   NodePort   10.99.99.42   <none>        80:31076/TCP,443:30652/TCP   6d4h
```

## 使用ingress
>ingress-test.yaml 内容为怎么使用ingress
```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: lzy-ingress
  namespace: ingress-nginx
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:             #规则 
  - host: ingress.liangzy.com   #域名
    http:
      paths:
      - path: /
        backend:
          serviceName: httpd-svc       #关联service
          servicePort: 80              #关联service的映射端口
      - path: /tomcat
        backend:
          serviceName: tomcat-svc      #关联service
          servicePort: 8080                #关联service的映射端口
```
