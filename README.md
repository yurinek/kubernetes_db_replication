# kubernetes_db_replication

This project shows a prove of concept for creation of Postgres Database Master-Slave-Replication inside different Kubernetes Deployments or StatefulSets which orchestrate pods and containers on multiple Kubernetes Cluster nodes.<br>
It will create pods for one master and one slave database and respectively create containers with ready to use db replication based on Postgres 12.

## How to install

Copy the contents of this repo to your Kubernetes host

## How to run if using Deployment
```hcl 
# create directory for persistent volume
sudo mkdir /mnt/data

# create persistent volume
kubectl apply -f volume-pv-hostpath.yml 

# create persistent volume claim for master db
kubectl apply -f master-volume-pvc-hostpath.yml

# store secret, config in etcd db
kubectl apply -f configmap.yml
kubectl apply -f secret.yml

# make deployment/pod reachable via node's ip + randomly assigned port between 30000 and 32767
kubectl apply -f master-service-node-port.yml

# make deployment/pod reachable via its dns name from within other deployments. its dns name equals to name of cluster-ip service
kubectl apply -f master-service-cluster-ip.yml

# create deployment/pod/container for master db
kubectl apply -f master-deployment.yml

# create deployment/pod/container for slave db
kubectl apply -f slave-deployment.yml
```


## How to run if using StatefulSet
```hcl 
# create directory for persistent volume
sudo mkdir /mnt/data

# create persistent volume
kubectl apply -f volume-pv-hostpath.yml 

# we dont need pvc because for statefulset we use pvc-template

# store secret, config in etcd
kubectl apply -f configmap.yml
kubectl apply -f secret.yml

# make deployment/pod reachable via node's ip + randomly assigned port between 30000 and 32767
kubectl apply -f master-service-node-port.yml

# make deployment/pod reachable via its dns name from within other deployments. its dns name equals to name of cluster-ip service
kubectl apply -f master-service-cluster-ip.yml

# create deployment/pod/container for master db
kubectl apply -f master-statefulset.yml

# create deployment/pod/container for slave db
kubectl apply -f slave-statefulset.yml
```

## Delete everything
```hcl
kubectl delete -f .
```

