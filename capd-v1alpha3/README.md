# Create a management Cluster 

```
kind create cluster --name=capi --config=./resources/kind.yaml
```

# Initialize the common provider (The provider url in clusterctl.yaml look for absolute path)

```
clusterctl --config clusterctl.yaml init -i docker -v10
```

# Create the workload cluster

```
clusterctl --config clusterctl.yaml config cluster workload -n workload --control-plane-machine-count=3 --worker-machine-count=3 | kubectl apply -f-
```

# Retrive the workload kubeconfig

```
kubectl get secret workload-kubeconfig -n workload -o=jsonpath='{.data.value}' | base64 --decode > workload.kubeconfig
```

# Try to access the Workload cluster

```
kubectl get nodes --kubeconfig workload.kubeconfig
```

# At this point, all ControlPlane and Worker node are joined but in `NotReady` state


# Install Calico CNI 

```
kubectl apply -f calico.yaml --kubeconfig workload.kubeconfig
```
