
# Lab 1 - Install Digital.ai Release 22.2.4

This lab is intro in using `xl kube install` to install Digital.ai Release to the K8S cluster.
After installation we will check if everything is running and do some customization on the cluster to setup Azure DNS.

## Intro to `xl kube`

To investigate possible options to use during installation you can check `xl kube -h` and `xl kube install -h`.
Also check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-kube.html) for the additional `xl kube` details.

## Installation

Let's start installation, we will just run plain install and after that we need to answer on list of questions: 

```shell
xl kube install
```

Few notes for the following example:
- we have `xl` command in the path 
- we are installing 22.2.4 version of the release and operator, so we can upgrade it later 
- the license file will be provided on the workshop and it is used from the working directory
- for the domain we use `my-namespace-xlr.northcentralus.cloudapp.azure.com`, see more on [Apply a DNS label to the service](https://learn.microsoft.com/en-us/azure/aks/static-ip#apply-a-dns-label-to-the-service)
  - the hostname must be unique in the location, so use some custom unique name here, for example your name as part of the hostname
- for the storage class we use following two custom storage classes, they already exist on the cluster:
  - azure-aks-test-cluster-file-storage-class based on the [Azure Files Dynamic](https://docs.microsoft.com/en-us/azure/aks/azure-files-dynamic-pv)

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azure-aks-test-cluster-file-storage-class
provisioner: kubernetes.io/azure-file
mountOptions:
  - dir_mode=0777
  - file_mode=0777
  - uid=0
  - gid=0
  - mfsymlinks
  - cache=strict
  - actimeo=30
parameters:
  skuName: Standard_LRS
```
  - azure-aks-test-cluster-disk-storage-class based on the [Azure Disk Dynamic](https://docs.microsoft.com/en-us/azure/aks/azure-disks-dynamic-pv)

```yaml
kind: StorageClass
apiVersion: storage.k8s.io/v1
metadata:
  name: azure-aks-test-cluster-disk-storage-class
provisioner: kubernetes.io/azure-disk
reclaimPolicy: Delete
volumeBindingMode: WaitForFirstConsumer
parameters:
  storageaccounttype: Standard_LRS
```

Following is example of answers and log after starting installation (example on the Azure):

```text
$ xl kube install
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: 
» AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): 
» Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: 
» my-namespace
? Do you want to create custom Kubernetes namespace my-namespace, it does not exist: 
» Yes
? Product server you want to perform install for: 
» dai-release [Digital.ai Release]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): 
» xebialabs
? Enter the image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): 
» xl-release
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 
» 22.2.4
? Enter the release server replica count: 
» 2
? Enter PVC size for Release (Gi): 
» 1
? Select between supported Access Modes: 
» ReadWriteMany [ReadWriteMany]
? Select between supported ingress types: 
» nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): 
» No
? Provide DNS name for accessing UI of the server: 
» my-namespace-xlr.northcentralus.cloudapp.azure.com
? Provide administrator password: 
» admin
? Type of the OIDC configuration: 
» no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): 
» xebialabs/release-operator:22.2.4
? Select source of the license: 
» file [Path to the license file (the file can be in clean text or base64 encoded)]
? Provide license file for the server: 
» ./xlr-license.lic
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: 
» gdbzzXny7Mocuksl
? Provide storage class for the server: 
» azure-aks-test-cluster-file-storage-class
? Do you want to install a new PostgreSQL on the cluster: 
» Yes
? Provide Storage Class to be defined for PostgreSQL: 
» azure-aks-test-cluster-disk-storage-class
? Provide PVC size for PostgreSQL (Gi): 
» 1
? Do you want to install a new RabbitMQ on the cluster: 
» Yes
? Replica count to be defined for RabbitMQ: 
» 1
? Storage Class to be defined for RabbitMQ: 
» azure-aks-test-cluster-file-storage-class
? Provide PVC size for RabbitMQ (Gi): 
» 1
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeRelease              | ReadWriteMany                                      |
	| AdminPassword                  | l2DvSpebufAti0jc                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-131244                                    |
	| ImageNameRelease               | xl-release                                         |
	| ImageTag                       | 22.2.4                                             |
	| IngressHost                    | my-namespace-xlr.northcentralus.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | gdbzzXny7Mocuksl                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogNApQ.. |
	| LicenseFile                    | ./xlr-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageReleaseGeneric    | xebialabs/release-operator:22.2.4                  |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | azure-aks-test-cluster-disk-storage-class          |
	| ProcessType                    | install                                            |
	| PvcSizeRelease                 | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | azure-aks-test-cluster-file-storage-class          |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| StorageClass                   | azure-aks-test-cluster-file-storage-class          |
	| UseCustomNamespace             | true                                               |
	| XlrReplicaCount                | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? 
» Yes
For current process files will be generated in the: digitalai/dai-release/my-namespace/20221031-131244/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_my-namespace_install-20221031-131244.yaml
Starting install processing.
Created keystore digitalai/dai-release/my-namespace/20221031-131244/kubernetes/repository-keystore.jceks
Created namespace my-namespace
Update CR with namespace... \ Using custom resource name dai-xlr-my-namespace
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
Applied resource clusterrole/my-namespace-xlr-operator-proxy-role from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/my-namespace-xlr-operator-manager-role from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/my-namespace-xlr-operator-metrics-reader from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xlr-operator-controller-manager-metrics-service from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaireleases.xlr.digital.ai from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xlr-operator-controller-manager from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/deployment.yaml
Applied resource role/xlr-operator-leader-election-role from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xlr-operator-leader-election-rolebinding from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xlr-operator-manager-rolebinding from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xlr-operator-proxy-rolebinding from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalairelease/dai-xlr-my-namespace from the file digitalai/dai-release/my-namespace/20221031-131244/kubernetes/dai-release_cr.yaml
Install finished successfully!
```

For the other questions and answers details check [Installation Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-install-wizard-release.html)

## Wait for resources with xl kube check

After xl-cli finishes all resources are not yet ready on the cluster, try to run following checks that are waiting for the resources to be fully running and ready on the cluster.

Check for details with `xl kube check --help` for details what each flag is here.

```shell
xl kube check --wait-for-ready 5
xl kube check --wait-for-ready 5 --skip-collecting
xl kube check --wait-for-ready 5 --zip-files
```

For example output for the second command (the helm info on the end will be displayed if you have helm in the path):
Example is on the Azure.

```text
$ xl kube check --wait-for-ready 5 --skip-collecting
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: my-namespace
? Product server you want to perform check for: dai-release [Digital.ai Release]
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-141114                                    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release/my-namespace/20221031-141114/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_my-namespace_check-20221031-141114.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xlr-operator-controller-manager is available in the namespace my-namespace
Deployment deployment/dai-xlr-my-namespace-nginx-ingress-controller is available in the namespace my-namespace
Deployment deployment/dai-xlr-my-namespace-nginx-ingress-controller-default-backend is available in the namespace my-namespace
PVC pvc/data-dai-xlr-my-namespace-rabbitmq-0 is bound in the namespace my-namespace
Pod pod/dai-xlr-my-namespace-rabbitmq-0 is available in the namespace my-namespace
PVC pvc/data-dai-xlr-my-namespace-postgresql-0 is bound in the namespace my-namespace
Pod pod/dai-xlr-my-namespace-postgresql-0 is available in the namespace my-namespace
PVC pvc/dai-xlr-my-namespace-digitalai-release is bound in the namespace my-namespace
Pod pod/dai-xlr-my-namespace-digitalai-release-0 is available in the namespace my-namespace
Pod pod/dai-xlr-my-namespace-digitalai-release-1 is available in the namespace my-namespace
Checking helm installation status
Operator's dai-xlr-my-namespace helm status in the namespace my-namespace for the installation:
NAME: dai-xlr-my-namespace
LAST DEPLOYED: Mon Oct 31 12:15:08 2022
NAMESPACE: my-namespace
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
## To get the application URL, run:
http://my-namespace-xlr.northcentralus.cloudapp.azure.com/

## To get the admin password for xl-release, run:
kubectl get secret --namespace my-namespace dai-xlr-my-namespace-digitalai-release -o jsonpath="{.data.release-password}" | base64 --decode; echo

## To get the password for postgresql, run:
kubectl get secret --namespace  my-namespace dai-xlr-my-namespace-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode; echo

## To get the password for rabbitMQ, run:
kubectl get secret --namespace  my-namespace dai-xlr-my-namespace-rabbitmq   -o jsonpath="{.data.rabbitmq-password}" | base64 --decode; echo

## To edit custom resource dai-xlr-my-namespace
kubectl edit digitalaireleases.xlr.digital.ai dai-xlr-my-namespace -n my-namespace

## To restart release pods use restart of the statefulset
kubectl rollout restart sts dai-xlr-my-namespace-digitalai-release -n my-namespace

Check finished successfully!
```

## Discover how to open the page and login

For now you will be not able to login to the page `http://my-namespace-xlr.northcentralus.cloudapp.azure.com/`.

You can connect directly to pod or via service port forwarding.  
```shell
$ kubectl port-forward --namespace my-namespace svc/dai-xlr-my-namespace-digitalai-release 18080:80
Forwarding from 127.0.0.1:18080 -> 5516
Forwarding from [::1]:18080 -> 5516
```

Now try to open [http://localhost:18080](http://localhost:18080) and log in as `admin`. 

If you forgot the password, you can get it with the command from the helm info (username is as always `admin`):

```shell
## To get the admin password for xl-release, run:
kubectl get secret --namespace my-namespace dai-xlr-my-namespace-digitalai-release -o jsonpath="{.data.release-password}" | base64 --decode; echo
```

Check the version in **(Gear icon) > About Digital.ai Release**. We should be running **Version 22.2.4**

## Set up DNS -- TODO SPLIT INTO SEPARATE SECTION?

To finalize ingress setup with DNS host we will need to update CR YAML. There few options, here are two:

### 1. Edit CR on cluster

CRD name is always `digitalaireleases.xlr.digital.ai`, and CR will be in the namespace:

```shell
$ kubectl get digitalaireleases.xlr.digital.ai -n my-namespace
NAME                   AGE
dai-xlr-my-namespace   89m
```

So to edit CR use following (same as from helm info):

```shell
## To edit custom resource dai-xlr-my-namespace
kubectl edit digitalaireleases.xlr.digital.ai dai-xlr-my-namespace -n my-namespace
```

Update the with selected hostname in the yaml path of the CR file `spec.nginx-ingress-controller.service.annotations`, in our example it is `my-namespace-xlr`:

```yaml
spec:
  
  nginx-ingress-controller:
    
    service:
      
      annotations:
        service.beta.kubernetes.io/azure-dns-label-name: my-namespace-xlr        
```

Save the file and exit the editor.

### 2. Edit CR file and apply it on cluster

Other way to edit CR, open the `digitalai/dai-release/my-namespace/20221031-131244/kubernetes/dai-release_cr.yaml`.
For your path: check your installation log in the line `For current process files will be generated in the: digitalai/dai-release/my-namespace/20221031-131244/kubernetes`.

Update the with selected hostname in the yaml path of the CR file `spec.nginx-ingress-controller.service.annotations`, in our example it is `my-namespace-xlr`:

```yaml
spec:
  
  nginx-ingress-controller:
    
    service:
      
      annotations:
        service.beta.kubernetes.io/azure-dns-label-name: my-namespace-xlr        
```

Save the changes in the file.

Apply the edited file with
```
kubectl apply -n my-namespace -f digitalai/dai-release/my-namespace/20221031-131244/kubernetes/dai-release_cr.yaml
```

### Try to open Release page

Now try to open [http://my-namespace-xlr.northcentralus.cloudapp.azure.com/](http://my-namespace-xlr.northcentralus.cloudapp.azure.com/)

Note: it may take a while for the DNS changes to come through and you may get a 'server not found' page for a while.

