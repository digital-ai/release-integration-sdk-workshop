
# Lab 0 - Setup kubectl context

## Collect all prerequisites

Check all prerequisites:

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [xl-cli 22.3.2](https://dist.xebialabs.com/public/xl-cli/22.3.2/)
  - [Install the XL CLI](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/how-to/install-the-xl-cli.html)
- [yq](https://github.com/mikefarah/yq)
- Java 11 - keytool (only if you plan to use the generation of the keystore inside the xl-cli kube)
- One directory from where you will run `xl kube` commands

Use one of the following options 

### With the Azure cluster

Run:

```shell
export AZURE_USERNAME=xl-test-azure...
export AZURE_PASSWORD=put_password_from_1pass
export RESOURCE_GROUP=xl-kube-workshop-group-X
export CLUSTER_NAME=xl-kube-workshop-X
az login -u $AZURE_USERNAME -p $AZURE_PASSWORD 
az aks get-credentials --resource-group $RESOURCE_GROUP --name $CLUSTER_NAME --overwrite-existing
```

### With Minikube

Run:

```shell
minikube start --driver=virtualbox --memory 15000 --cpus 6
minikube addons enable ingress
minikube addons enable ingress-dns
```

### With Docker Desktop

Enable Kubernetes under **Preferences > Kubernetes**.

The demo runs comfortably with the following resources allocated under **Preferences > Resources**:

* CPUs: 8
* Memory: 22 GB
* Swap: 4 GB
* Disk image size: 60 GB


## Check cluster connection

```shell
kubectl version
```

## Check `xl` command

The `xl`command version 22.3.2 should be in the path. Check this with the following command:

```shell
xl version
```
