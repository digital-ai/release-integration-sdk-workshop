
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
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform clean for: dai-deploy [Digital.ai Deploy]
? Enter the name of custom resource definition you want to reuse or replace: digitalaideploys.xld.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it: Yes
? Enter the name of custom resource: dai-xld-ns-yourname
? Should we preserve persisted volume claims? If not all volume data will be lost: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xld-ns-yourname                               |
	| CrdName                        | digitalaideploys.xld.digital.ai                    |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-002021                                    |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221101-002021/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_clean-20221101-002021.yaml
Cleaning the resources on the cluster!
CR dai-xld-ns-yourname is available, deleting
? Do you want to delete the resource digitalaideploys.xld.digital.ai/dai-xld-ns-yourname: Yes
Deleted digitalaideploys.xld.digital.ai/dai-xld-ns-yourname from namespace ns-yourname
Deleting statefulsets
? Do you want to delete the resource sts/dai-xld-ns-yourname-digitalai-deploy-master: Yes
Deleted sts/dai-xld-ns-yourname-digitalai-deploy-master from namespace ns-yourname (already deleted)
? Do you want to delete the resource sts/dai-xld-ns-yourname-postgresql: Yes
Deleted sts/dai-xld-ns-yourname-postgresql from namespace ns-yourname (already deleted)
? Do you want to delete the resource sts/dai-xld-ns-yourname-rabbitmq: Yes
Deleted sts/dai-xld-ns-yourname-rabbitmq from namespace ns-yourname (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xld-operator-controller-manager: Yes
Deleted deployment/xld-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xld-operator-controller-manager-metrics-service: Yes
Deleted svc/xld-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
? Do you want to delete the resource secret/sh.helm.release.v1.dai-xld-ns-yourname.v1: Yes
Deleted secret/sh.helm.release.v1.dai-xld-ns-yourname.v1 from namespace ns-yourname
? Do you want to delete the resource ingressclass/nginx-dai-xld-ns-yourname: Yes
Deleted ingressclass/nginx-dai-xld-ns-yourname from namespace ns-yourname
Deleting roles
? Do you want to delete the resource role/xld-operator-leader-election-role: Yes
Deleted role/xld-operator-leader-election-role from namespace ns-yourname
? Do you want to delete the resource clusterrole/dai-xld-ns-yourname-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xld-ns-yourname-nginx-ingress-controller from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-manager-role: Yes
Deleted clusterrole/ns-yourname-xld-operator-manager-role from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-metrics-reader: Yes
Deleted clusterrole/ns-yourname-xld-operator-metrics-reader from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-proxy-role: Yes
Deleted clusterrole/ns-yourname-xld-operator-proxy-role from namespace ns-yourname
? Do you want to delete the resource rolebinding/xld-operator-leader-election-rolebinding: Yes
Deleted rolebinding/xld-operator-leader-election-rolebinding from namespace ns-yourname
Fetching clusterrolebinding from ns-yourname namespace... /
? Do you want to delete the resource clusterrolebinding/dai-xld-ns-yourname-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xld-ns-yourname-nginx-ingress-controller from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/ns-yourname-xld-operator-manager-rolebinding: Yes
Deleted clusterrolebinding/ns-yourname-xld-operator-manager-rolebinding from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/ns-yourname-xld-operator-proxy-rolebinding: Yes
Deleted clusterrolebinding/ns-yourname-xld-operator-proxy-rolebinding from namespace ns-yourname
Deleting PVCs
? Do you want to delete the resource pvc/data-dai-xld-ns-yourname-postgresql-0: Yes
Deleted pvc/data-dai-xld-ns-yourname-postgresql-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dai-xld-ns-yourname-rabbitmq-0: Yes
Deleted pvc/data-dai-xld-ns-yourname-rabbitmq-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-cc-server-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-cc-server-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-1: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-1 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-1: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-1 from namespace ns-yourname
Clean finished successfully!❯ xl kube clean
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: ns-yourname
? Product server you want to perform clean for: dai-deploy [Digital.ai Deploy]
? Enter the name of custom resource definition you want to reuse or replace: digitalaideploys.xld.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaideploys.xld.digital.ai, and all related CRs will be deleted with it: Yes
? Enter the name of custom resource: dai-xld-ns-yourname
? Should we preserve persisted volume claims? If not all volume data will be lost: No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xld-ns-yourname                               |
	| CrdName                        | digitalaideploys.xld.digital.ai                    |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221101-002021                                    |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
	| ServerType                     | dai-deploy                                         |
	| ShortServerName                | xld                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/ns-yourname/20221101-002021/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_ns-yourname_clean-20221101-002021.yaml
Cleaning the resources on the cluster!
CR dai-xld-ns-yourname is available, deleting
? Do you want to delete the resource digitalaideploys.xld.digital.ai/dai-xld-ns-yourname: Yes
Deleted digitalaideploys.xld.digital.ai/dai-xld-ns-yourname from namespace ns-yourname
Deleting statefulsets
? Do you want to delete the resource sts/dai-xld-ns-yourname-digitalai-deploy-master: Yes
Deleted sts/dai-xld-ns-yourname-digitalai-deploy-master from namespace ns-yourname (already deleted)
? Do you want to delete the resource sts/dai-xld-ns-yourname-postgresql: Yes
Deleted sts/dai-xld-ns-yourname-postgresql from namespace ns-yourname (already deleted)
? Do you want to delete the resource sts/dai-xld-ns-yourname-rabbitmq: Yes
Deleted sts/dai-xld-ns-yourname-rabbitmq from namespace ns-yourname (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xld-operator-controller-manager: Yes
Deleted deployment/xld-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xld-operator-controller-manager-metrics-service: Yes
Deleted svc/xld-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
? Do you want to delete the resource secret/sh.helm.release.v1.dai-xld-ns-yourname.v1: Yes
Deleted secret/sh.helm.release.v1.dai-xld-ns-yourname.v1 from namespace ns-yourname
? Do you want to delete the resource ingressclass/nginx-dai-xld-ns-yourname: Yes
Deleted ingressclass/nginx-dai-xld-ns-yourname from namespace ns-yourname
Deleting roles
? Do you want to delete the resource role/xld-operator-leader-election-role: Yes
Deleted role/xld-operator-leader-election-role from namespace ns-yourname
? Do you want to delete the resource clusterrole/dai-xld-ns-yourname-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xld-ns-yourname-nginx-ingress-controller from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-manager-role: Yes
Deleted clusterrole/ns-yourname-xld-operator-manager-role from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-metrics-reader: Yes
Deleted clusterrole/ns-yourname-xld-operator-metrics-reader from namespace ns-yourname
? Do you want to delete the resource clusterrole/ns-yourname-xld-operator-proxy-role: Yes
Deleted clusterrole/ns-yourname-xld-operator-proxy-role from namespace ns-yourname
? Do you want to delete the resource rolebinding/xld-operator-leader-election-rolebinding: Yes
Deleted rolebinding/xld-operator-leader-election-rolebinding from namespace ns-yourname
Fetching clusterrolebinding from ns-yourname namespace... /
? Do you want to delete the resource clusterrolebinding/dai-xld-ns-yourname-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xld-ns-yourname-nginx-ingress-controller from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/ns-yourname-xld-operator-manager-rolebinding: Yes
Deleted clusterrolebinding/ns-yourname-xld-operator-manager-rolebinding from namespace ns-yourname
? Do you want to delete the resource clusterrolebinding/ns-yourname-xld-operator-proxy-rolebinding: Yes
Deleted clusterrolebinding/ns-yourname-xld-operator-proxy-rolebinding from namespace ns-yourname
Deleting PVCs
? Do you want to delete the resource pvc/data-dai-xld-ns-yourname-postgresql-0: Yes
Deleted pvc/data-dai-xld-ns-yourname-postgresql-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dai-xld-ns-yourname-rabbitmq-0: Yes
Deleted pvc/data-dai-xld-ns-yourname-rabbitmq-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-cc-server-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-cc-server-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-1: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-master-1 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-0 from namespace ns-yourname
? Do you want to delete the resource pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-1: Yes
Deleted pvc/data-dir-dai-xld-ns-yourname-digitalai-deploy-worker-1 from namespace ns-yourname
Clean finished successfully!
```

The clean process is cleaning everything from the cluster. For each resource it is asking for confirmation before delete.

For the other questions and answers details check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-deploy-version-v.22.3/page/deploy/operator/xl-kube.html#xl-kube-clean)
