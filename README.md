kafka周りのツールのデプロイ:

```
# Minikubeをスタートする, デフォルト設定ではメモリが足りないので4GBを指定
minikube start --memory=4096

# ネームスペース作成
kubectl create namespace kafka

# Strimzi(Kafkaのデプロイツール)をデプロイ
kubectl create -f 'https://strimzi.io/install/latest?namespace=kafka' -n kafka

# Kafkaをデプロイ
kubectl apply -f https://strimzi.io/examples/latest/kafka/kafka-persistent-single.yaml -n kafka

# 起動するまで待つ 
kubectl wait kafka/my-cluster --for=condition=Ready --timeout=300s -n kafka

# kafka-ui(Kafkaの画面ツール)のデプロイ
kubectl apply -n kafka -f https://raw.githubusercontent.com/cacapouh/kafka-ui-k8s/main/kafka-ui.yml  
```

kafkaデプロイ結果:

```
$ kubectl get pods -n kafka
NAME                                          READY   STATUS    RESTARTS   AGE
kafka-ui-deployment-756764f444-td7km          1/1     Running   0          5m28s
my-cluster-entity-operator-5b794584d8-n8tn6   2/2     Running   0          9m55s
my-cluster-kafka-0                            1/1     Running   0          10m
my-cluster-zookeeper-0                        1/1     Running   0          11m
strimzi-cluster-operator-7bb5468c59-88284     1/1     Running   0          11m

$ kubectl get services -n kafka
NAME                          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)                                        AGE
kafka-ui-service              NodePort    10.96.142.163   <none>        8080:31006/TCP                                 24m
my-cluster-kafka-bootstrap    ClusterIP   10.110.155.3    <none>        9091/TCP,9092/TCP,9093/TCP                     29m
my-cluster-kafka-brokers      ClusterIP   None            <none>        9090/TCP,9091/TCP,8443/TCP,9092/TCP,9093/TCP   29m
my-cluster-zookeeper-client   ClusterIP   10.102.11.104   <none>        2181/TCP                                       30m
my-cluster-zookeeper-nodes    ClusterIP   None            <none>        2181/TCP,2888/TCP,3888/TCP                     30m
```


サンプルアプリのデプロイ:

```
docker build . -t simple-app
minikube image load simple-app:latest
kubectl apply -f deployment.yml
```

サンプルアプリのデプロイ結果:

```
$ kubectl get pods
NAME                                    READY   STATUS    RESTARTS   AGE
simple-app-deployment-dd44b5ff4-58nrb   1/1     Running   0          2m45s

$ kubectl logs simple-app-deployment-dd44b5ff4-58nrb | tail
Count 183
Count 184
Count 185
Count 186
Count 187
Count 188
Count 189
Count 190
Count 191
Count 192
```

ブラウザからkafka-uiにアクセス:

```
minikube service kafka-ui-service -n kafka
```
