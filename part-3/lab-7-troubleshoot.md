
# Lab 7 - Fix errors - Troubleshoot

We will now try to detect and correct problems that can happen during installation.

## Installation with few errors

Do installation with following errors:
- not existing operator image: xebialabs/deploy-operator:22.3.X
- wrong storage class on the postgresql PVC, we will use azure-aks-test-cluster-file-storage-class, that is not required block storage

```shell
xl kube install
```

```text
$ xl kube install
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: my-namespace
? Product server you want to perform install for: dai-deploy [Digital.ai Deploy]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabs
? Enter the deploy server image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-deploy
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 22.3.1
? Enter the deploy task engine image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-task-engine
? Enter the central configuration image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): central-configuration
? Enter the deploy master server replica count: 2
? Enter PVC size for Deploy master (Gi): 1
? Select between supported Access Modes: ReadWriteOnce [ReadWriteOnce]
? Enter the deploy worker replica count: 2
? Enter PVC size for Deploy worker (Gi): 1
? Enter PVC size for Central Configuration (Gi): 0.500000
? Select between supported ingress types: nginx [NGINX]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): No
? Provide DNS name for accessing UI of the server: my-namespace-xld.northcentralus.cloudapp.azure.com
? Provide administrator password: ARTLY8Qgl6FPLXN7
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): xebialabs/deploy-operator:22.3.X
? Select source of the license: file [Path to the license file (the file can be in clean text or base64 encoded)]
? Provide license file for the server: ./xld-license.lic
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: HEXRzENbwPvn85YB
? Provide storage class for the server: azure-aks-test-cluster-file-storage-class
? Do you want to install a new PostgreSQL on the cluster: Yes
? Provide Storage Class to be defined for PostgreSQL: azure-aks-test-cluster-file-storage-class
? Provide PVC size for PostgreSQL (Gi): 1
? Do you want to install a new RabbitMQ on the cluster: Yes
? Replica count to be defined for RabbitMQ: 1
? Storage Class to be defined for RabbitMQ: azure-aks-test-cluster-file-storage-class
? Provide PVC size for RabbitMQ (Gi): 1
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-230614                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | my-namespace-xld.northcentralus.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.X                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | azure-aks-test-cluster-file-storage-class          |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | azure-aks-test-cluster-file-storage-class          |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | azure-aks-test-cluster-file-storage-class          |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-230614.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/repository-keystore.jceks
Skip creating namespace my-namespace, already exists
Creating namespace my-namespace... - Using custom resource name dai-xld-my-namespace
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
Applied resource clusterrole/my-namespace-xld-operator-proxy-role from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/my-namespace-xld-operator-manager-role from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/my-namespace-xld-operator-metrics-reader from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xld-operator-manager-rolebinding from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-my-namespace from the file digitalai/dai-deploy/my-namespace/20221031-230614/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

Installation process successfully finished!

## Using wrong tag in the operator

We can use `xl kube check` to additionally check is everything is running correctly and detect what is happening with installation.
Detect the problem with:

```shell
xl kube check --wait-for-ready 1 --skip-collecting
```

Here is example of the running that line:

```text
? xl kube check --wait-for-ready 1 --skip-collecting
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: my-namespace
? Product server you want to perform check for: dai-deploy [Digital.ai Deploy]
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-231543                                    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221031-231543/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_check-20221031-231543.yaml
Collecting the CR data
Waiting for resources to be ready
Failed with waiting for the deployment xld-operator-controller-manager in the namespace my-namespace with error: timeout while waiting for deployment/xld-operator-controller-manager to be Available
 For more details check:
 - Check the deployment describe events with the describe command: kubectl describe deployment xld-operator-controller-manager -n my-namespace
 - Check the deployment logs for the more details with commands: kubectl logs deployment/xld-operator-controller-manager -n my-namespace -f --all-containers=true
Saved resource data for deployment/xld-operator-controller-manager
 - Check the operator pod events with the describe command: kubectl describe pod xld-operator-controller-manager-5757db45fb-4lv8q -n my-namespace
Skiped saving resource data for pod/xld-operator-controller-manager-5757db45fb-4lv8q
Saved resource data for events/xld-operator-controller-manager-5757db45fb.172346ce0176d8af
Saved resource data for events/xld-operator-controller-manager.172346cdfefae2ca
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce2257fbf5
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce29898bcf
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce2ef95966
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce2f0893dc
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce4ee0c4ff
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce4ee11aef
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce5ccb113f
Saved resource data for events/xld-operator-controller-manager-5757db45fb-4lv8q.172346ce5ccb3b0b
Stopping because error during wait for the deployment xld-operator-controller-manager
```

As you can see the check failed with final message `Stopping because error during wait for the deployment xld-operator-controller-manager`.
So we need to check more previous log is there something additionally halpfull.
One of the notes is 

    Check the operator pod events with the describe command: kubectl describe pod xld-operator-controller-manager-5757db45fb-4lv8q -n my-namespace

So we run that command, and on the end we can see events:
```text
$ kubectl describe pod xld-operator-controller-manager-5757db45fb-4lv8q -n my-namespace
...
Events:
  Type     Reason     Age                  From               Message
  ----     ------     ----                 ----               -------
  Normal   Scheduled  11m                  default-scheduler  Successfully assigned my-namespace/xld-operator-controller-manager-5757db45fb-4lv8q to aks-nodepool1-22570493-vmss000001
  Normal   Pulled     11m                  kubelet            Container image "gcr.io/kubebuilder/kube-rbac-proxy:v0.8.0" already present on machine
  Normal   Created    11m                  kubelet            Created container kube-rbac-proxy
  Normal   Started    11m                  kubelet            Started container kube-rbac-proxy
  Warning  Failed     10m (x3 over 11m)    kubelet            Failed to pull image "xebialabs/deploy-operator:22.3.X": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/xebialabs/deploy-operator:22.3.X": failed to resolve reference "docker.io/xebialabs/deploy-operator:22.3.X": docker.io/xebialabs/deploy-operator:22.3.X: not found
  Warning  Failed     10m (x3 over 11m)    kubelet            Error: ErrImagePull
  Warning  Failed     9m50s (x6 over 11m)  kubelet            Error: ImagePullBackOff
  Normal   Pulling    9m38s (x4 over 11m)  kubelet            Pulling image "xebialabs/deploy-operator:22.3.X"
  Normal   BackOff    57s (x43 over 11m)   kubelet            Back-off pulling image "xebialabs/deploy-operator:22.3.X"
```

The events have reason of the failure: `Failed to pull image "xebialabs/deploy-operator:22.3.X": rpc error: code = NotFound desc = failed to pull and unpack image "docker.io/xebialabs/deploy-operator:22.3.X": failed to resolve reference "docker.io/xebialabs/deploy-operator:22.3.X": docker.io/xebialabs/deploy-operator:22.3.X: not found`.

Ok, we can fix that by editing answers file from previous installation and run same installation again.
We edit the digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-230614.yaml and change the line from:

```
OperatorImageDeployGeneric: xebialabs/deploy-operator:22.3.X
```

to

```
OperatorImageDeployGeneric: xebialabs/deploy-operator:22.3.1
```

Repeat installation with answers file

```shell
xl kube install --answers digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-230614.yaml
```

```text
$ xl kube install --answers digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-230614.yaml
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-232810                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | my-namespace-xld.northcentralus.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.1                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | azure-aks-test-cluster-file-storage-class          |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | azure-aks-test-cluster-file-storage-class          |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | azure-aks-test-cluster-file-storage-class          |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-232810.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/repository-keystore.jceks
Skip creating namespace my-namespace, already exists
Update central configuration values... - Using custom resource name dai-xld-my-namespace
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
? Do you want to replace the resource clusterrole/my-namespace-xld-operator-proxy-role with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/cluster-role-digital-proxy-role.yaml: Yes
Applied resource clusterrole/my-namespace-xld-operator-proxy-role from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/cluster-role-digital-proxy-role.yaml
? Do you want to replace the resource clusterrole/my-namespace-xld-operator-manager-role with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/cluster-role-manager-role.yaml: Yes
Applied resource clusterrole/my-namespace-xld-operator-manager-role from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/cluster-role-manager-role.yaml
? Do you want to replace the resource clusterrole/my-namespace-xld-operator-metrics-reader with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/cluster-role-metrics-reader.yaml: Yes
Applied resource clusterrole/my-namespace-xld-operator-metrics-reader from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/cluster-role-metrics-reader.yaml
? Do you want to replace the resource service/xld-operator-controller-manager-metrics-service with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/controller-manager-metrics-service.yaml: Yes
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/controller-manager-metrics-service.yaml
? Do you want to replace the resource customresourcedefinition/digitalaideploys.xld.digital.ai with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/custom-resource-definition.yaml: Yes
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/custom-resource-definition.yaml
? Do you want to replace the resource deployment/xld-operator-controller-manager with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/deployment.yaml: Yes
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/deployment.yaml
? Do you want to replace the resource role/xld-operator-leader-election-role with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/leader-election-role.yaml: Yes
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/leader-election-role.yaml
? Do you want to replace the resource rolebinding/xld-operator-leader-election-rolebinding with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/leader-election-rolebinding.yaml: Yes
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/leader-election-rolebinding.yaml
? Do you want to replace the resource clusterrolebinding/my-namespace-xld-operator-manager-rolebinding with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/manager-rolebinding.yaml: Yes
Applied resource clusterrolebinding/my-namespace-xld-operator-manager-rolebinding from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/manager-rolebinding.yaml
? Do you want to replace the resource clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/proxy-rolebinding.yaml: Yes
Applied resource clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/template/proxy-rolebinding.yaml
? Do you want to replace the resource digitalaideploy/dai-xld-my-namespace with specification from file
digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/dai-deploy_cr.yaml: Yes
Applied resource digitalaideploy/dai-xld-my-namespace from the file digitalai/dai-deploy/my-namespace/20221031-232810/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

So now we have progress, the operator pod will be ready and running. We can go on the next problem.

## Using wrong storage class

To detect next problem run again, reuse the answers file from previous check run:

```shell
xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_my-namespace_check-20221031-231543.yaml
```

Here is example of running that command:

```text
$ xl kube check --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_my-namespace_check-20221031-231543.yaml
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-233539                                    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221031-233539/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_check-20221031-233539.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace my-namespace
Deployment deployment/dai-xld-my-namespace-nginx-ingress-controller is available in the namespace my-namespace
Deployment deployment/dai-xld-my-namespace-nginx-ingress-controller-default-backend is available in the namespace my-namespace
PVC pvc/data-dai-xld-my-namespace-rabbitmq-0 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-rabbitmq-0 is available in the namespace my-namespace
PVC pvc/data-dai-xld-my-namespace-postgresql-0 is bound in the namespace my-namespace
Failed with waiting for the pod dai-xld-my-namespace-postgresql-0 in the namespace my-namespace with error: timeout while waiting for pod/dai-xld-my-namespace-postgresql-0 to be Ready
 For more details check:
 - Check the pod describe events with the describe command: kubectl describe pod dai-xld-my-namespace-postgresql-0 -n my-namespace
 - Check the pod logs for the more details with commands: kubectl logs pod/dai-xld-my-namespace-postgresql-0 -n my-namespace -f --all-containers=true
 - Check if cluster nodes can provide the pod requests for memory and CPU
Saved resource data for pod/dai-xld-my-namespace-postgresql-0
Saved resource data for events/dai-xld-my-namespace-digitalai-deploy-worker-0.172347e59800aac0
Saved resource data for events/dai-xld-my-namespace-rabbitmq-0.172347e5b390aae5
Saved resource data for events/dai-xld-my-namespace-rabbitmq-0.172347e5a766ac56
Saved resource data for events/dai-xld-my-namespace-rabbitmq-0.172347e5883deb98
Saved resource data for events/dai-xld-my-namespace-rabbitmq-0.172347e58249ce8a
Saved resource data for events/dai-xld-my-namespace-digitalai-deploy-cc-server-0.172347e57e1c73e8
Saved resource data for events/dai-xld-my-namespace-postgresql-0.172347e59b633e10
Saved resource data for events/dai-xld-my-namespace-nginx-ingress-controller.172347e6de5e2ae4
Saved resource data for events/dai-xld-my-namespace-rabbitmq-0.1667255352292
Saved resource data for events/dai-xld-my-namespace-rabbitmq-0.1667255365377
Stopping because error during wait for the pod dai-xld-my-namespace-postgresql-0
```

In the last line we can see that postgresql pod has problems `Stopping because error during wait for the pod dai-xld-my-namespace-postgresql-0`.
From log we have suggestion to again check describe of the pod:

    - Check the pod logs for the more details with commands: kubectl logs pod/dai-xld-my-namespace-postgresql-0 -n my-namespace -f --all-containers=true

That command is displaying following in the events section:

```text
? kubectl logs pod/dai-xld-my-namespace-postgresql-0 -n my-namespace -f --all-containers=true
postgresql 22:50:02.92
postgresql 22:50:02.93 Welcome to the Bitnami postgresql container
postgresql 22:50:02.93 Subscribe to project updates by watching https://github.com/bitnami/bitnami-docker-postgresql
postgresql 22:50:02.93 Submit issues and feature requests at https://github.com/bitnami/bitnami-docker-postgresql/issues
postgresql 22:50:02.94
postgresql 22:50:02.96 INFO  ==> ** Starting PostgreSQL setup **
postgresql 22:50:02.99 INFO  ==> Validating settings in POSTGRESQL_* env vars..
postgresql 22:50:03.00 INFO  ==> Loading custom pre-init scripts...
postgresql 22:50:03.00 INFO  ==> Initializing PostgreSQL database...
chmod: changing permissions of '/bitnami/postgresql/data': Operation not permitted
postgresql 22:50:03.05 WARN  ==> Lack of permissions on data directory!
chmod: changing permissions of '/bitnami/postgresql/data': Operation not permitted
postgresql 22:50:03.06 WARN  ==> Lack of permissions on data directory!
postgresql 22:50:03.06 INFO  ==> pg_hba.conf file not detected. Generating it...
postgresql 22:50:03.06 INFO  ==> Generating local authentication configuration
```

After googling the error we can find that postgresql is not working on the current storageclass:
[https://github.com/bitnami/bitnami-docker-postgresql/issues/285#issuecomment-814925197](https://github.com/bitnami/bitnami-docker-postgresql/issues/285#issuecomment-814925197)

So PVC's storage class is not correct. We can again edit same answers file and change it with correct disk storage class:
From:

```
PostgresqlStorageClass: 'azure-aks-test-cluster-file-storage-class'
```

To

```
PostgresqlStorageClass: 'azure-aks-test-cluster-disk-storage-class'
```

Repeat installation with answers file, but clean before, because storage class cannot be updated on the PVC and use `--skip-prompts` to avoid answering all checks.

Run following:

```shell
xl kube clean --skip-prompts
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-230614.yaml
```

During running clean, do not delete CRD because there are other installation that are reusing same CRD.

Here is example of the installation output:

```text
xl kube install --skip-prompts --answers digitalai/generated_answers_dai-deploy_my-namespace_install-20221031-230614.yaml
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| AccessModeDeploy               | ReadWriteOnce                                      |
	| AdminPassword                  | ARTLY8Qgl6FPLXN7                                   |
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| EnableIngressTls               | false                                              |
	| EnablePostgresql               | true                                               |
	| EnableRabbitmq                 | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-000553                                    |
	| ImageNameCc                    | central-configuration                              |
	| ImageNameDeploy                | xl-deploy                                          |
	| ImageNameDeployTaskEngine      | deploy-task-engine                                 |
	| ImageTag                       | 22.3.1                                             |
	| IngressHost                    | my-namespace-xld.northcentralus.cloudapp.azure.com |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| KeystorePassphrase             | HEXRzENbwPvn85YB                                   |
	| License                        | LS0tIExpY2Vuc2UgLS0tCkxpY2Vuc2UgdmVyc2lvbjogMwpQ.. |
	| LicenseFile                    | ./xld-license.lic                                  |
	| LicenseSource                  | file                                               |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeInstall          | no-oidc                                            |
	| OperatorImageDeployGeneric     | xebialabs/deploy-operator:22.3.1                   |
	| OsType                         | darwin                                             |
	| PostgresqlPvcSize              | 1                                                  |
	| PostgresqlStorageClass         | azure-aks-test-cluster-disk-storage-class          |
	| ProcessType                    | install                                            |
	| PvcSizeCc                      | 0.500000                                           |
	| PvcSizeDeploy                  | 1                                                  |
	| PvcSizeDeployTaskEngine        | 1                                                  |
	| RabbitmqPvcSize                | 1                                                  |
	| RabbitmqReplicaCount           | 1                                                  |
	| RabbitmqStorageClass           | azure-aks-test-cluster-file-storage-class          |
	| RepositoryKeystoreSource       | generate                                           |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| StorageClass                   | azure-aks-test-cluster-file-storage-class          |
	| UseCustomNamespace             | true                                               |
	| XldMasterCount                 | 2                                                  |
	| XldWorkerCount                 | 2                                                  |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_install-20221101-000553.yaml
Starting install processing.
Created keystore digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/repository-keystore.jceks
Skip creating namespace my-namespace, already exists
Update central configuration values... - Using custom resource name dai-xld-my-namespace
Generated files successfully for AzureAKS installation.
Applying resources to the cluster!
Applied resource clusterrole/my-namespace-xld-operator-proxy-role from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/my-namespace-xld-operator-manager-role from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/my-namespace-xld-operator-metrics-reader from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xld-operator-controller-manager-metrics-service from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource customresourcedefinition/digitalaideploys.xld.digital.ai from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/custom-resource-definition.yaml
Applied resource deployment/xld-operator-controller-manager from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/deployment.yaml
Applied resource role/xld-operator-leader-election-role from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xld-operator-leader-election-rolebinding from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xld-operator-manager-rolebinding from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalaideploy/dai-xld-my-namespace from the file digitalai/dai-deploy/my-namespace/20221101-000553/kubernetes/dai-deploy_cr.yaml
Install finished successfully!
```

We can now run final check to see if everything is running:

```text
xl kube check --skip-prompts --wait-for-ready 1 --skip-collecting --answers digitalai/generated_answers_dai-deploy_my-namespace_check-20221031-231543.yaml
```

Output is now OK:

```text
$ xl kube check --skip-prompts --wait-for-ready 5 --skip-collecting --answers digitalai/generated_answers_dai-deploy_my-namespace_check-20221031-231543.yaml
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-001131                                    |
	| IngressType                    | nginx                                              |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| ProcessType                    | check                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221101-001131/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_check-20221101-001131.yaml
Collecting the CR data
Waiting for resources to be ready
Deployment deployment/xld-operator-controller-manager is available in the namespace my-namespace
Deployment deployment/dai-xld-my-namespace-nginx-ingress-controller is available in the namespace my-namespace
Deployment deployment/dai-xld-my-namespace-nginx-ingress-controller-default-backend is available in the namespace my-namespace
PVC pvc/data-dai-xld-my-namespace-rabbitmq-0 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-rabbitmq-0 is available in the namespace my-namespace
PVC pvc/data-dai-xld-my-namespace-postgresql-0 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-postgresql-0 is available in the namespace my-namespace
PVC pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-cc-server-0 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-digitalai-deploy-cc-server-0 is available in the namespace my-namespace
PVC pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-0 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-digitalai-deploy-master-0 is available in the namespace my-namespace
PVC pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-1 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-digitalai-deploy-master-1 is available in the namespace my-namespace
PVC pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-0 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-digitalai-deploy-worker-0 is available in the namespace my-namespace
PVC pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-1 is bound in the namespace my-namespace
Pod pod/dai-xld-my-namespace-digitalai-deploy-worker-1 is available in the namespace my-namespace
Checking helm installation status
Operator's dai-xld-my-namespace helm status in the namespace my-namespace for the installation:
NAME: dai-xld-my-namespace
LAST DEPLOYED: Mon Oct 31 23:06:42 2022
NAMESPACE: my-namespace
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
## To get the application URL, run:
http://my-namespace-xld.northcentralus.cloudapp.azure.com/

## To get the admin password for xl-deploy, run:
kubectl get secret --namespace my-namespace dai-xld-my-namespace-digitalai-deploy -o jsonpath="{.data.deploy-password}" | base64 --decode; echo
## To get the password for postgresql, run:
kubectl get secret --namespace  my-namespace dai-xld-my-namespace-postgresql -o jsonpath="{.data.postgresql-password}" | base64 --decode; echo

## To get the password for rabbitMQ, run:
kubectl get secret --namespace  my-namespace dai-xld-my-namespace-rabbitmq   -o jsonpath="{.data.rabbitmq-password}" | base64 --decode; echo

## To edit custom resource dai-xld-my-namespace
kubectl edit digitalaideploys.xld.digital.ai dai-xld-my-namespace -n my-namespace

## To restart deploy central configuration pods use restart of the statefulset
kubectl rollout restart sts dai-xld-my-namespace-digitalai-deploy-cc-server -n my-namespace

## To restart deploy master pods use restart of the statefulset
kubectl rollout restart sts dai-xld-my-namespace-digitalai-deploy-master -n my-namespace

## To restart deploy worker pods use restart of the statefulset
kubectl rollout restart sts dai-xld-my-namespace-digitalai-deploy-worker -n my-namespace

Check finished successfully!
```
