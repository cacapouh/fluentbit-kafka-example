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
