
# Lab 6 - Clean

Now we will learn how to clean the cluster from our installation of the Digital.ai Deploy.

Run following to clean the cluster from the installed resources:

```shell
xl kube clean
```

Note that in case if you are sharing cluster with others, do not delete the CR that is shared between all installations.
So you need to answer to `Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it:` with `Yes`.

With following example we are deleting installed resources along with PVC, only CRD remains on the cluster (example on the Azure):

```text
$ xl kube clean
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: my-namespace
? Product server you want to perform clean for: dai-deploy [Digital.ai Deploy]
? Enter the name of custom resource definition you want to reuse or replace: digitalaideploys.xld.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it: Yes
? Enter the name of custom resource: dai-xld-my-namespace
? Should we preserve persisted volume claims? If not all volume data will be lost: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xld-my-namespace                               |
	| CrdName                        | digitalaideploys.xld.digital.ai                    |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-002021                                    |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221101-002021/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_clean-20221101-002021.yaml
Cleaning the resources on the cluster!
CR dai-xld-my-namespace is available, deleting
? Do you want to delete the resource digitalaideploys.xld.digital.ai/dai-xld-my-namespace: Yes
Deleted digitalaideploys.xld.digital.ai/dai-xld-my-namespace from namespace my-namespace
Deleting statefulsets
? Do you want to delete the resource sts/dai-xld-my-namespace-digitalai-deploy-master: Yes
Deleted sts/dai-xld-my-namespace-digitalai-deploy-master from namespace my-namespace (already deleted)
? Do you want to delete the resource sts/dai-xld-my-namespace-postgresql: Yes
Deleted sts/dai-xld-my-namespace-postgresql from namespace my-namespace (already deleted)
? Do you want to delete the resource sts/dai-xld-my-namespace-rabbitmq: Yes
Deleted sts/dai-xld-my-namespace-rabbitmq from namespace my-namespace (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xld-operator-controller-manager: Yes
Deleted deployment/xld-operator-controller-manager from namespace my-namespace
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xld-operator-controller-manager-metrics-service: Yes
Deleted svc/xld-operator-controller-manager-metrics-service from namespace my-namespace
Deleting secrets
? Do you want to delete the resource secret/sh.helm.release.v1.dai-xld-my-namespace.v1: Yes
Deleted secret/sh.helm.release.v1.dai-xld-my-namespace.v1 from namespace my-namespace
? Do you want to delete the resource ingressclass/nginx-dai-xld-my-namespace: Yes
Deleted ingressclass/nginx-dai-xld-my-namespace from namespace my-namespace
Deleting roles
? Do you want to delete the resource role/xld-operator-leader-election-role: Yes
Deleted role/xld-operator-leader-election-role from namespace my-namespace
? Do you want to delete the resource clusterrole/dai-xld-my-namespace-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xld-my-namespace-nginx-ingress-controller from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xld-operator-manager-role: Yes
Deleted clusterrole/my-namespace-xld-operator-manager-role from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xld-operator-metrics-reader: Yes
Deleted clusterrole/my-namespace-xld-operator-metrics-reader from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xld-operator-proxy-role: Yes
Deleted clusterrole/my-namespace-xld-operator-proxy-role from namespace my-namespace
? Do you want to delete the resource rolebinding/xld-operator-leader-election-rolebinding: Yes
Deleted rolebinding/xld-operator-leader-election-rolebinding from namespace my-namespace
Fetching clusterrolebinding from my-namespace namespace... /
? Do you want to delete the resource clusterrolebinding/dai-xld-my-namespace-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xld-my-namespace-nginx-ingress-controller from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xld-operator-manager-rolebinding: Yes
Deleted clusterrolebinding/my-namespace-xld-operator-manager-rolebinding from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding: Yes
Deleted clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding from namespace my-namespace
Deleting PVCs
? Do you want to delete the resource pvc/data-dai-xld-my-namespace-postgresql-0: Yes
Deleted pvc/data-dai-xld-my-namespace-postgresql-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dai-xld-my-namespace-rabbitmq-0: Yes
Deleted pvc/data-dai-xld-my-namespace-rabbitmq-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-cc-server-0: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-cc-server-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-0: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-1: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-1 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-0: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-1: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-1 from namespace my-namespace
Clean finished successfully!❯ xl kube clean
? Following kubectl context will be used during execution: `xl-kube-workshop`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: my-namespace
? Product server you want to perform clean for: dai-deploy [Digital.ai Deploy]
? Enter the name of custom resource definition you want to reuse or replace: digitalaideploys.xld.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it: Yes
? Enter the name of custom resource: dai-xld-my-namespace
? Should we preserve persisted volume claims? If not all volume data will be lost: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xld-my-namespace                               |
	| CrdName                        | digitalaideploys.xld.digital.ai                    |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-002021                                    |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/my-namespace/20221101-002021/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_my-namespace_clean-20221101-002021.yaml
Cleaning the resources on the cluster!
CR dai-xld-my-namespace is available, deleting
? Do you want to delete the resource digitalaideploys.xld.digital.ai/dai-xld-my-namespace: Yes
Deleted digitalaideploys.xld.digital.ai/dai-xld-my-namespace from namespace my-namespace
Deleting statefulsets
? Do you want to delete the resource sts/dai-xld-my-namespace-digitalai-deploy-master: Yes
Deleted sts/dai-xld-my-namespace-digitalai-deploy-master from namespace my-namespace (already deleted)
? Do you want to delete the resource sts/dai-xld-my-namespace-postgresql: Yes
Deleted sts/dai-xld-my-namespace-postgresql from namespace my-namespace (already deleted)
? Do you want to delete the resource sts/dai-xld-my-namespace-rabbitmq: Yes
Deleted sts/dai-xld-my-namespace-rabbitmq from namespace my-namespace (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xld-operator-controller-manager: Yes
Deleted deployment/xld-operator-controller-manager from namespace my-namespace
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xld-operator-controller-manager-metrics-service: Yes
Deleted svc/xld-operator-controller-manager-metrics-service from namespace my-namespace
Deleting secrets
? Do you want to delete the resource secret/sh.helm.release.v1.dai-xld-my-namespace.v1: Yes
Deleted secret/sh.helm.release.v1.dai-xld-my-namespace.v1 from namespace my-namespace
? Do you want to delete the resource ingressclass/nginx-dai-xld-my-namespace: Yes
Deleted ingressclass/nginx-dai-xld-my-namespace from namespace my-namespace
Deleting roles
? Do you want to delete the resource role/xld-operator-leader-election-role: Yes
Deleted role/xld-operator-leader-election-role from namespace my-namespace
? Do you want to delete the resource clusterrole/dai-xld-my-namespace-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xld-my-namespace-nginx-ingress-controller from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xld-operator-manager-role: Yes
Deleted clusterrole/my-namespace-xld-operator-manager-role from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xld-operator-metrics-reader: Yes
Deleted clusterrole/my-namespace-xld-operator-metrics-reader from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xld-operator-proxy-role: Yes
Deleted clusterrole/my-namespace-xld-operator-proxy-role from namespace my-namespace
? Do you want to delete the resource rolebinding/xld-operator-leader-election-rolebinding: Yes
Deleted rolebinding/xld-operator-leader-election-rolebinding from namespace my-namespace
Fetching clusterrolebinding from my-namespace namespace... /
? Do you want to delete the resource clusterrolebinding/dai-xld-my-namespace-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xld-my-namespace-nginx-ingress-controller from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xld-operator-manager-rolebinding: Yes
Deleted clusterrolebinding/my-namespace-xld-operator-manager-rolebinding from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding: Yes
Deleted clusterrolebinding/my-namespace-xld-operator-proxy-rolebinding from namespace my-namespace
Deleting PVCs
? Do you want to delete the resource pvc/data-dai-xld-my-namespace-postgresql-0: Yes
Deleted pvc/data-dai-xld-my-namespace-postgresql-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dai-xld-my-namespace-rabbitmq-0: Yes
Deleted pvc/data-dai-xld-my-namespace-rabbitmq-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-cc-server-0: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-cc-server-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-0: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-1: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-master-1 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-0: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-1: Yes
Deleted pvc/data-dir-dai-xld-my-namespace-digitalai-deploy-worker-1 from namespace my-namespace
Clean finished successfully!
```

The clean process is cleaning everything from the cluster. For each resource it is asking for confirmation before delete.

For the other questions and answers details check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-deploy-version-v.22.3/page/deploy/operator/xl-kube.html#xl-kube-clean)
