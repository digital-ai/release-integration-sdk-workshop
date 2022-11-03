
# Lab 0 - Setup kubectl context

## Collect all prerequisites

Check all prerequisites:

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [xl-cli 22.3.2](https://dist.xebialabs.com/public/xl-cli/22.3.2/)
  - [Install the XL CLI](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/how-to/install-the-xl-cli.html)
- [yq](https://github.com/mikefarah/yq)
- Java 11 - keytool (only if you plan to use the generation of the keystore inside the xl-cli kube)
- A directory from where you will run `xl kube` commands

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

XXX TEST cluster connectivity! Hes could not connect to xl-kube-workshop-2

### With Minikube

Run:

```shell
minikube start --driver=virtualbox --memory 22000 --cpus 8 --disk-size 60000mb
minikube addons enable ingress
minikube addons enable ingress-dns
```

After minikube start, kubectl will use the minikube context.

### With Docker Desktop

Enable Kubernetes under **Preferences > Kubernetes**.

The demo runs comfortably with the following resources allocated under **Preferences > Resources**:

* CPUs: 8
* Memory: 22 GB
* Swap: 4 GB
* Disk image size: 60 GB




## Check cluster connection

```shell
kubectl cluster-info
```

For example it will return for minikube:

```text
$ kubectl version
WARNING: This version information is deprecated and will be replaced with the output from kubectl version --short.  Use --output=yaml|json to get the full version.
Client Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.2", GitCommit:"5835544ca568b757a8ecae5c153f317e5736700e", GitTreeState:"clean", BuildDate:"2022-09-21T14:33:49Z", GoVersion:"go1.19.1", Compiler:"gc", Platform:"linux/amd64"}
Kustomize Version: v4.5.7
Server Version: version.Info{Major:"1", Minor:"25", GitVersion:"v1.25.2", GitCommit:"5835544ca568b757a8ecae5c153f317e5736700e", GitTreeState:"clean", BuildDate:"2022-09-21T14:27:13Z", GoVersion:"go1.19.1", Compiler:"gc", Platform:"linux/amd64"}
```

## Check `xl` command

The `xl`command version 22.3.2 should be in the path. Check this with the following command:

```shell
xl version
```

Example of the response on linux:

```text
$ xl version
CLI version:             22.3.2
Git version:             v22.3.1-3-gf1e275f
API version XL Deploy:   xl-deploy/v1
API version XL Release:  xl-release/v1
Git commit:              f1e275f548795ee0624d2add34376053c09effe5
Build date:              2022-10-27T12:58:14.038Z
GO version:              go1.16
OS/Arch:                 linux/amd64
```

## Monitor Kubernetes

Our favorite tool to check what's going on is [K9s](https://k9scli.io). Launch it with

```shell
k9s
```

You will see everything that is going in the cluster! That is probably too much information. Narrow down the scope by typing `:ns`. This will allow you to select a namespace. In the subsequent exercises you will create your own namespace. This will be your private spot on the cluster where all necessary components are installed.

---

[Next](../part-1/lab-1-install-release.md)

