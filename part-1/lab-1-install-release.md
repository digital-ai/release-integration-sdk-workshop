
# Lab 1 - Install Digital.ai Release 22.2.4

This lab will use `xl kube install` to install Digital.ai Release to the K8s cluster.
After installation we will check if everything is running properly and then configure the hostname in DNS.

Installation on Kubernetes is operator-based. We first install our custom Digital.ai Release or Deploy Operator into Kubernetes. Then we configure the application and the yaml configuration files are applied. The Operator takes care of creating the necessary resources and configuring Release. Currently the Operator uses Kubernetes under the hood, but this may change in the future.

## Introducing `xl kube`

Throughout this workshop , we will use the new `xl kube` command. It can do the following:

* Install a Release or Deploy in a Kubernetes cluster
* Upgrade Release and Deploy from a previous version
* Check if installation was successful and gather log files
* Clean the installation.

To investigate all possible options, please use the `--help` command. For example `xl kube --help`, or `xl kube install --help`. 

Also check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-kube.html) for more details.


## Installation

Installing Release or Deploy in Kubernetes is as easy as running this simple command:

```shell
xl kube install
```

This command will take care of the asking for the relevant configuration using an interative wizard; producing Kubernetes yaml files and applying them to perform the installation.

Before we kick it off, let's get our ducks in a row

- We are installing 22.2.4 version of Release. Later in the workshop we will upgrade it to 22.3.1
- The license files will be provided during the workshop and need to be saved in the working directory.
- Both Kubernetes namespace and hostname need to be unique. For this workshop, we will refer to it as `digitalai-yourname`. Everytime you encounter `digitalai-yourname`, replace it with your own namespace, for example `digitalai-alice`.
- When installing on Azure, you will [create a DNS label](https://learn.microsoft.com/en-us/azure/aks/static-ip#apply-a-dns-label-to-the-service) for `release.digitalai-yourname.northcentralus.cloudapp.azure.com`. When using minikube or Docker you can use any host name you want, for example `release.digitalai-yourname.local`. 
- On Azure we use two custom storage classes.They already exist on the cluster:
  - `azure-aks-test-cluster-file-storage-class` based on [Azure Files Dynamic](https://docs.microsoft.com/en-us/azure/aks/azure-files-dynamic-pv)
  - `azure-aks-test-cluster-disk-storage-class` based on [Azure Disk Dynamic](https://docs.microsoft.com/en-us/azure/aks/azure-disks-dynamic-pv)

Now let's get started!

Kick off the `xl kube install` command and look closely at the answers below. Note that sometimes you can take the default, sometimes you need to give the value as prompted below and sometimes you need to give a custom value. Please take it slow -- this is wizard is a very concentrated form of all parameters that are needed for a Kubernetes based installation and doesn't lend itself very well to rush through it with a next-next-next approach. Kubernetes itself can be bewildering if everything is not specified exactly as it should, so you can save some time debugging by putting in the values with care. 

We've marked some of the questions where you need to pay extra attention with a warning sign.

For more details on the questions and answers, check our documentation:  [Installation Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-install-wizard-release.html)

_The following example is for Azure. For minikube / Docker Desktop choose 'PlainK8s' for K8sSetup and use default storage classes._

```text
$ xl kube install
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: 
»⚠️ AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): 
» Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: 
»⚠️ digitalai-yourname
? Do you want to create custom Kubernetes namespace digitalai-yourname, it does not exist: 
» Yes
? Product server you want to perform install for: 
» dai-release [Digital.ai Release]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): 
» xebialabs
? Enter the image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): 
» xl-release
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 
»⚠️ 22.2.4 
? Enter the release server replica count: 
»⚠️ 2
? Enter PVC size for Release (Gi): 
»⚠️ 1
? Select between supported Access Modes: 
» ReadWriteMany [ReadWriteMany]
? Select between supported ingress types: 
» nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): 
» No
? Provide DNS name for accessing UI of the server: 
»⚠️ release.digitalai-yourname.northcentralus.cloudapp.azure.com
? Provide administrator password: 
» 9M8KgUmLFp5KceHl
? Type of the OIDC configuration: 
» no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): 
»⚠️ xebialabs/release-operator:22.2.4
? Select source of the license: 
» file [Path to the license file (the file can be in clean text or base64 encoded)]
? Provide license file for the server: 
» ./xlr-license.lic
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: 
» gdbzzXny7Mocuksl
? Provide storage class for the server: 
»⚠️ azure-aks-test-cluster-file-storage-class
? Do you want to install a new PostgreSQL on the cluster: 
» Yes
? Provide Storage Class to be defined for PostgreSQL: 
»⚠️ azure-aks-test-cluster-disk-storage-class
? Provide PVC size for PostgreSQL (Gi): 
»⚠️ 1
? Do you want to install a new RabbitMQ on the cluster: 
» Yes
? Replica count to be defined for RabbitMQ: 
»⚠️ 1
? Storage Class to be defined for RabbitMQ: 
»⚠️ azure-aks-test-cluster-file-storage-class
? Provide PVC size for RabbitMQ (Gi): 
»⚠️ 1
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
	| IngressHost                    | release.digitalai-yourname.northcentralus.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | gdbzzXny7Mocuksl                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogNApQ.. |
	| LicenseFile                    | ./xlr-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | digitalai-yourname                                       |
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
```

`xl kube install` gives a moment to breathe and shows the values we have entered. Before we say yes and start the installation, here's an overview of what will happen

1. Store answers in a file for reuse.
2. Generate Kubernetes yaml and other files needed.
3. Use `kubectl` to apply files in bulk and start the installation.

Note that subsequently, we can pick up the process at any time by using command line flags on `xl kube install`

* We can edit the answers file and use it as input for `xl kube install`, avoiding the interactive questions
* We can edit the Kubernetes Yaml file, and directly apply them using `kubectl`
* We can edit the Kubernetes Yaml files, and apply them in bulk using `xl kube install`

Now let's say **Yes** to the question and see what happens

```
For current process files will be generated in the: digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes
```

All files of this installation run will be stored in a directory that has the timestamp in it. We will use these files later so it is useful to note that we get a reference to this directory here.

```
Generated answers file successfully: digitalai/generated_answers_dai-release_digitalai-yourname_install-20221031-131244.yaml
```

The answers are stored in this file and can be reused later.

```
Starting install processing.
Created keystore digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/repository-keystore.jceks
Created namespace digitalai-yourname
Update CR with namespace... \ Using custom resource name dai-xlr-digitalai-yourname
Generated files successfully for AzureAKS installation.
```

All needed Yaml files are being created, as well as a keystore that will be used by the Release server.
On Kubernetes, the namespace is configured automatically.

```
Applying resources to the cluster!
```

This indicates that `xl kube install` will now start calling `kubectl` with the generated yaml. Below you will see the list of all the files that are being applied.

```
Applied resource clusterrole/release.digitalai-yourname-operator-proxy-role from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/release.digitalai-yourname-operator-manager-role from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/release.digitalai-yourname-operator-metrics-reader from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xlr-operator-controller-manager-metrics-service from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaireleases.xlr.digital.ai from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xlr-operator-controller-manager from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/deployment.yaml
Applied resource role/xlr-operator-leader-election-role from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xlr-operator-leader-election-rolebinding from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/release.digitalai-yourname-operator-manager-rolebinding from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/release.digitalai-yourname-operator-proxy-rolebinding from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalairelease/dai-xlr-digitalai-yourname from the file digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/dai-release_cr.yaml
Install finished successfully!
```

And we are done!

Sort of...

This is Kubernetes. We have only told the cluster what the 'desired state' is and Kubernetes has acknowledged that it has gotten the configuration and will now _start_ work on making it so. In other words, we have only shipped over necessary files to K8s and now need to trust that the system will do the work. And that no errors will occur in the process. By design, Kubernetes will _not_ tell you when it is done and will _not_ tell you if an error occured. You need to ask for it and know where to look.

That's why we added the `xl kube check` command. It knows what to check for and will give you timely status updates.

## Wait for resources with xl kube check

The `xl kube check` command will query Kubernetes if the installation is successful or not, and download all necessary configuration and log files for troubleshooting.

Use `xl kube check --help` to see all options and some example invocations.

To see how our installation is doing, invoke the command

```shell
xl kube check --wait-for-ready 5 --skip-collecting
```

After asking the questions of what you want to check, this will wait 5 minutes for succesful installation, and will not download all configuration and logs.

⚠️ Note that for the last part of the check you need to have `helm` installed. 

```text
$ xl kube check --wait-for-ready 5 --skip-collecting
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: digitalai-yourname
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
	| Namespace                      | digitalai-yourname                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release/digitalai-yourname/20221031-141114/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_digitalai-yourname_check-20221031-141114.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xlr-operator-controller-manager is available in the namespace digitalai-yourname
Deployment deployment/dai-xlr-digitalai-yourname-nginx-ingress-controller is available in the namespace digitalai-yourname
Deployment deployment/dai-xlr-digitalai-yourname-nginx-ingress-controller-default-backend is available in the namespace digitalai-yourname
PVC pvc/data-dai-xlr-digitalai-yourname-rabbitmq-0 is bound in the namespace digitalai-yourname
Pod pod/dai-xlr-digitalai-yourname-rabbitmq-0 is available in the namespace digitalai-yourname
PVC pvc/data-dai-xlr-digitalai-yourname-postgresql-0 is bound in the namespace digitalai-yourname
Pod pod/dai-xlr-digitalai-yourname-postgresql-0 is available in the namespace digitalai-yourname
PVC pvc/dai-xlr-digitalai-yourname-digitalai-release is bound in the namespace digitalai-yourname
Pod pod/dai-xlr-digitalai-yourname-digitalai-release-0 is available in the namespace digitalai-yourname
Pod pod/dai-xlr-digitalai-yourname-digitalai-release-1 is available in the namespace digitalai-yourname
Checking helm installation status
Operator's dai-xlr-digitalai-yourname helm status in the namespace digitalai-yourname for the installation:
NAME: dai-xlr-digitalai-yourname
LAST DEPLOYED: Mon Oct 31 12:15:08 2022
NAMESPACE: digitalai-yourname
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
## To get the application URL, run:
http://release.digitalai-yourname.northcentralus.cloudapp.azure.com/

## To get the admin password for xl-release, run:
kubectl get secret --namespace digitalai-yourname dai-xlr-digitalai-yourname-digitalai-release -o jsonpath="{.data.release-password}" | base64 --decode; echo

## To get the password for postgresql, run:
kubectl get secret --namespace  digitalai-yourname dai-xlr-digitalai-yourname-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode; echo

## To get the password for rabbitMQ, run:
kubectl get secret --namespace  digitalai-yourname dai-xlr-digitalai-yourname-rabbitmq   -o jsonpath="{.data.rabbitmq-password}" | base64 --decode; echo

## To edit custom resource dai-xlr-digitalai-yourname
kubectl edit digitalaireleases.xlr.digital.ai dai-xlr-digitalai-yourname -n digitalai-yourname

## To restart release pods use restart of the statefulset
kubectl rollout restart sts dai-xlr-digitalai-yourname-digitalai-release -n digitalai-yourname

Check finished successfully!
```

**Exercise**: find out what combination of command line flags you need to give in order to avoid any prompt.

<!-- xl kube check --answers digitalai/generated_answers_dai-release_kube-hes_install-20221101-132633.yaml --skip-collecting -w 5 -S -->

## Discover how to open the page and login

We have not configured the DNS, so we can't access the public URL yet: `http://release.digitalai-yourname.northcentralus.cloudapp.azure.com/`.

However, we can connect directly to the Release via service port forwarding.  
```shell
$ kubectl port-forward --namespace digitalai-yourname svc/dai-xlr-digitalai-yourname-digitalai-release 18080:80
Forwarding from 127.0.0.1:18080 -> 5516
Forwarding from [::1]:18080 -> 5516
```

Now open [http://localhost:18080](http://localhost:18080) and log in as `admin`. 

If you forgot the password, you can get it with the command from the helm info (username is as always `admin`):

```shell
## To get the admin password for xl-release, run:
kubectl get secret --namespace digitalai-yourname dai-xlr-digitalai-yourname-digitalai-release -o jsonpath="{.data.release-password}" | base64 --decode; echo
```

Check the version in **(Gear icon) > About Digital.ai Release**. We should be running **Version 22.2.4**

## Set up DNS on Azure

To configure the DNS we will need to update the Custom Resource (CR) YAML and tell Kubernetes we changed it.


Open the file `dai-release_cr.yaml` that can be found in the directory `digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/`.

Check the installation console output to find the correct timestamp. It's in the line `For current process files will be generated in the: digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes`.

Add your hostname in the yaml path of the CR file under spec > nginx-ingress-controller > service > annotations

```yaml
spec:
  …  
  nginx-ingress-controller:
    …
    service:
      …
      annotations:
        service.beta.kubernetes.io/azure-dns-label-name: release.digitalai-yourname        
```

Save the changes in the file.

Apply the edited file to Kubernetes with

```shell
kubectl apply -n digitalai-yourname -f digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/dai-release_cr.yaml
```

Now try to open [http://release.digitalai-yourname.northcentralus.cloudapp.azure.com/](http://release.digitalai-yourname.northcentralus.cloudapp.azure.com/)

Note: it may take a while for the DNS changes to come through and you may get a 'server not found' page for a while.


## Set up 'DNS' on localhost for Minikube / Docker Desktop

When using a local kube cluster, edit `/etc/hosts` and add your host name here.

```shell
sudo vi /etc/hosts
```

Add following line somewhere:

```text
127.0.0.1 release.digitalai-yourname.local
```

After saving, go to [https://release.digitalai-yourname.local](https://release.digitalai-yourname.local)
