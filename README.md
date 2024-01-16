kafka:
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

