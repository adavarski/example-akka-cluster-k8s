# Simple akka cluster on k8s cluster

This repository demonstrates how to deploy akka cluster application on kubernetes. 

To try it locally on minikube:
```bash
minikube start
eval $(minikube docker-env)

#install sbt
curl https://bintray.com/sbt/rpm/rpm | sudo tee /etc/yum.repos.d/bintray-sbt-rpm.repo
sudo yum install sbt

#build/push docker image
docker login
sbt docker:publish

#create k8s namespace
kubectl create ns example-akka-cluster

# create deployment
kubectl create -f k8s/example-akka-cluster-deployment.yml -n example-akka-cluster

# create service
kubectl create -f k8s/example-akka-cluster-service.yml -n example-akka-cluster

#Check

KUBE_IP=$(minikube ip)
MANAGEMENT_PORT=$(kubectl get svc example-akka-cluster -n example-akka-cluster -ojsonpath="{.spec.ports[?(@.name==\"management\")].nodePort}")
curl http://$KUBE_IP:$MANAGEMENT_PORT/cluster/members | jq
```
