# kafka-kube
This repo contains kubernetes manifests to deploy zookeeper, kafka and mirrormaker.

# Deploy zookeeper:

```
kubectl apply -f zookeeper/zookeeper-deployment.yaml
```

# Deploy kafka:

```
kubectl apply -f kafka/kafka-deployment.yaml
```

# Deploy mirrormaker:

```
kubectl apply -f mirrormaker/mirrormaker-configmap.yaml
kubectl apply -f mirrormaker/mirrormaker-deployment.yaml
```

# Notes

After thd deployments, if you would like to create a topic in kafka please follow the below steps:

# Deploy kafka-client pod:

```
kubectl run kafka-client --rm -ti --image docker.io/bitnami/kafka:2.8.1 -- bash
```
# Kafka-client
When the pod is created, you will be prompted with a terminal. Navigate kafka bin directory and create the topics.

```
cd /opt/bitnami/kafka/bin/
./kafka-topics.sh --bootstrap-server kafka:9092 --create metrics
```

# Create topics
You can list the topics once you've created it:

```
cd /opt/bitnami/kafka/bin/
./kafka-topics.sh --bootstrap-server kafka:9092 --list
```

# Deploy host-metrics
If you would like to produce any messages to a kafka cluster, deploy the host-metrics deployment.

```
kubectl apply -f host-metrics/host-metrics.yaml
```

The pod will write host metrics such as CPU, Memory and Disk usage for the topic named `logs`.

# Consumer

```
cd /opt/bitnami/kafka/bin/
./kafka-console-consumer.sh --bootstrap-server kafka:9092 --topic logs
```

It will print the messages from `logs` topic which is produced by host-metrics pod.

