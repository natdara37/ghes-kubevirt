
# GitHub Enterprise Server running in Kubernetes using KubeVirt

## Install pre-reqs

TODO

## Setup

### minikube

```
minikube config -p kubevirt set memory 4096
minikube config -p kubevirt set disk-size 300g
```

#### Windows-specific
```
minikube config -p kubevirt set vm-driver hyperv
minikube config -p kubevirt set hyperv-virtual-switch kubevirt
```

```
minikube profile kubevirt
minikube start
```

### kubevirt

```
export CDI_VERSION=$(curl -s https://github.com/kubevirt/containerized-data-importer/releases/latest | grep -o "v[0-9]\.[0-9]*\.[0-9]*")
kubectl create -f https://github.com/kubevirt/containerized-data-importer/releases/download/$CDI_VERSION/cdi-operator.yaml
kubectl create -f https://github.com/kubevirt/containerized-data-importer/releases/download/$CDI_VERSION/cdi-operator-cr.yaml
```

```
export KUBEVIRT_VERSION="v0.18.0"
kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/$KUBEVIRT_VERSION/kubevirt-operator.yaml
```

```
kubectl create configmap kubevirt-config -n kubevirt --from-literal debug.useEmulation=true --from-literal feature-gates=DataVolumes
```

```
kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/$KUBEVIRT_VERSION/kubevirt-cr.yaml
```

### GHES VM

#### Create the ghes-vm VM resource

```
kubectl apply -f https://raw.githubusercontent.com/willsmythe/ghes-kubevirt/master/ghes-vm.yml
kubectl get vms
```

#### Start the ghes-vm (create the VirtualMachineInstance)

```
virtctl start ghes-vs
kubectl get vmis
```

```
virtctl console ghes-vs
```

### Cleanup

```
minikube delete -p kubevirt
```

## Other

### Useful commands


```
minikube ip
minikube dashboard
minikube delete
```

### Random commands

```
kubectl delete -f https://github.com/kubevirt/kubevirt/releases/download/${KUBEVIRT_VERSION}/kubevirt-operator.yaml
set PATH=c:\tools;%PATH%;
export PATH=/c/tools/:$PATH
docker run -it -v C:\work\github\ghe-server:/ghe-server -w /ghe-server ubuntu:18.04 bash
kubectl create -f https://github.com/kubevirt/kubevirt/releases/download/%KUBEVIRT_VERSION%/kubevirt-cr.yaml
```

### Original

```
kubectl apply -f https://gist.githubusercontent.com/gnawhleinad/0151195ea6412bfe39d6b341666ebcc2/raw/b4e35c8bb71b957c01a04bcfe39efdeeaa7a0b9c/github-enterprise-server.yaml
kubectl patch virtualmachine ghes-vm --type merge -p "{\"spec\":{\"running\":true}}"
```