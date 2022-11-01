
# Lab 4 - Clean

Now we will learn how to clean the cluster from our installation of the Digital.ai Release.

Run following to clean the cluster from the installed resources:

```shell
xl kube clean
```

Note that in case if you are sharing cluster with others, do not delete the CR that is shared between all installations.
So you need to answer to `Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:` with `Yes`.

With following example we are deleting installed resources along with PVC, only CRD remains on the cluster (example on the Azure):

```text
? xl kube clean
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: 
» AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): 
» Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: 
» my-namespace
? Product server you want to perform clean for: 
» dai-release [Digital.ai Release]
? Enter the name of custom resource definition you want to reuse or replace: 
» digitalaireleases.xlr.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it: 
» Yes
? Enter the name of custom resource: 
» dai-xlr-my-namespace
? Should we preserve persisted volume claims? If not all volume data will be lost: 
» No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xlr-my-namespace                               |
	| CrdName                        | digitalaireleases.xlr.digital.ai                   |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-154651                                    |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release/my-namespace/20221031-154651/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_my-namespace_clean-20221031-154651.yaml
Cleaning the resources on the cluster!
CR dai-xlr-my-namespace is available, deleting
? Do you want to delete the resource digitalaireleases.xlr.digital.ai/dai-xlr-my-namespace: 
» Yes
Deleted digitalaireleases.xlr.digital.ai/dai-xlr-my-namespace from namespace my-namespace
Deleting statefulsets
Deleting deployments
? Do you want to delete the resource deployment/xlr-operator-controller-manager: 
» Yes
Deleted deployment/xlr-operator-controller-manager from namespace my-namespace
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xlr-operator-controller-manager-metrics-service: 
» Yes
Deleted svc/xlr-operator-controller-manager-metrics-service from namespace my-namespace
Deleting secrets
Deleting roles
? Do you want to delete the resource role/xlr-operator-leader-election-role: 
» Yes
Deleted role/xlr-operator-leader-election-role from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xlr-operator-manager-role: 
» Yes
Deleted clusterrole/my-namespace-xlr-operator-manager-role from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xlr-operator-metrics-reader: 
» Yes
Deleted clusterrole/my-namespace-xlr-operator-metrics-reader from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xlr-operator-proxy-role: 
» Yes
Deleted clusterrole/my-namespace-xlr-operator-proxy-role from namespace my-namespace
? Do you want to delete the resource rolebinding/xlr-operator-leader-election-rolebinding: 
» Yes
Deleted rolebinding/xlr-operator-leader-election-rolebinding from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xlr-operator-manager-rolebinding: 
» Yes
Deleted clusterrolebinding/my-namespace-xlr-operator-manager-rolebinding from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xlr-operator-proxy-rolebinding: 
» Yes
Deleted clusterrolebinding/my-namespace-xlr-operator-proxy-rolebinding from namespace my-namespace
Deleting PVCs
? Do you want to delete the resource pvc/dai-xlr-my-namespace-digitalai-release: 
» Yes
Deleted pvc/dai-xlr-my-namespace-digitalai-release from namespace my-namespace
? Do you want to delete the resource pvc/data-dai-xlr-my-namespace-postgresql-0: 
» Yes
Deleted pvc/data-dai-xlr-my-namespace-postgresql-0 from namespace my-namespace
? Do you want to delete the resource pvc/data-dai-xlr-my-namespace-rabbitmq-0:
» Yes
Deleted pvc/data-dai-xlr-my-namespace-rabbitmq-0 from namespace my-namespace
Clean finished successfully!
```

The clean process is cleaning everything from the cluster. For each resource it is asking for confirmation before delete.

For the other questions and answers details check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-kube.html#xl-kube-clean)
