# Kubernetes-monitor

## Kube-state-metrics

```
kubectl create namespace ops-monit
cd kube-state-metrics
kubectl apply -f ./
```

参考文档    
https://github.com/starsliao/Prometheus/tree/master/kubernetes

## Prometheus

创建名为monitoring的namespace

```
kubectl create namespace monitoring
```

创建集群角色

```
kubectl apply -f prometheus-clusterrole.yaml
```

创建prometheus-configmap，里面是prometheus.yml的配置，以及多个服务发现的配置。可按需修改。

```
kubectl apply -f prometheus-configmap.yaml
```

创建告警规则的configmap，可按需修改

```
kubectl apply -f prometheus-rules-configmap.yaml
```

创建PV和PVC

```
kubectl apply -f prometheus-pv.yaml
kubectl apply -f prometheus-pvc.yaml
```

创建Service和Deployment

```
kubectl apply -f prometheus-service.yaml
kubectl apply -f prometheus-deployment.yaml
```

检查Pod状态正常后，使用`NodeIP:30000`来访问prometheus



**参考文档**

常见rule配置: https://www.cnblogs.com/xiajq/p/11395316.html 

Promethues配置: https://devopscube.com/setup-prometheus-monitoring-on-kubernetes/ 



---

## Alertmanager

修改alertmanager-configmap.yaml

在配置文件中设置mail的发件邮箱，发件邮箱SMTP ，发件邮箱用户名，发件邮箱密码，收件邮箱等等，然后添加此Configmap。

```
kubectl apply -f alertmanager-configmap.yaml
```

创建告警模板的Configmap，这里是一个示例，可按需修改。

```
kubectl apply -f alertmanager-template-configmap.yaml
```

创建Service和Deployment

```
kubectl apply -f alertmanager-service.yaml
kubectl apply -f alertmanager-deployment.yaml
```

检查Pod状态正常后，使用`NodeIP:31000`来访问alertmanager



**参考文档**

https://devopscube.com/alert-manager-kubernetes-guide/ 



---



## Node_exporter

这个简单，也就是部署一个DaemonSet

```
kubectl apply -f node_exporter-daemonset.yaml
```

为什么不用创建Service呢？

因为Pod已经共享了宿主机的各种namespace，已经挂载了需要读取数据的目录。另外，服务发现的配置已经在prometheus的配置文件中设置过啦~

现在只需访问`NodeIP:9100`就可以了



**参考文档**

https://www.cnblogs.com/jiangwenhui/p/11989470.html 



---

## Grafana

全部执行就完事儿了

```
kubectl apply -f grafana/
```

推荐Dashboard:    
```
8919  (Node_exporter)
13105 (Kubernetes)
```

**参考文档**

https://devopscube.com/setup-grafana-kubernetes/ 


