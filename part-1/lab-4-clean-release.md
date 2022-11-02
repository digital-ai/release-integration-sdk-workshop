
# Lab 4 - Clean

Now we will learn how to clean the cluster from our installation of  Digital.ai Release.

Run the following command to clean the cluster from the installed resources:

```shell
xl kube clean --skip-prompts
```

Note that in case if you are sharing cluster with others, do not delete the CR that is shared between all installations.
So you need to answer to `Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:` with `Yes`.

The following example deletes installed resources along with PVC. Only the CRD remains on the cluster (example on the Azure):

```text
? xl kube clean
? Following kubectl context will be used during execution: `xl-kube-workshop`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: 
» AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): 
» Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: 
» ns-yourname
? Product server you want to perform clean for: 
» dai-release [Digital.ai Release]
? Enter the name of custom resource definition you want to reuse or replace: 
» digitalaireleases.xlr.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it: 
»⚠️ Yes
? Enter the name of custom resource: 
» dai-xlr-ns-yourname
? Should we preserve persisted volume claims? If not all volume data will be lost: 
» No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CrName                         | dai-xlr-ns-yourname                               |
	| CrdName                        | digitalaireleases.xlr.digital.ai                   |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-154651                                    |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                       |
	| OidcConfigType                 | existing                                           |
	| OsType                         | darwin                                             |
	| PreservePvc                    | false                                              |
	| ProcessType                    | clean                                              |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release/ns-yourname/20221031-154651/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_ns-yourname_clean-20221031-154651.yaml
Cleaning the resources on the cluster!
CR dai-xlr-ns-yourname is available, deleting
Deleted digitalaireleases.xlr.digital.ai/dai-xlr-ns-yourname from namespace ns-yourname
Deleting statefulsets
Deleting deployments
Deleted deployment/xlr-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
Deleted svc/xlr-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
Deleting roles
Deleted role/xlr-operator-leader-election-role from namespace ns-yourname
Deleted clusterrole/release.ns-yourname-operator-manager-role from namespace ns-yourname
Deleted clusterrole/release.ns-yourname-operator-metrics-reader from namespace ns-yourname
Deleted clusterrole/release.ns-yourname-operator-proxy-role from namespace ns-yourname
Deleted rolebinding/xlr-operator-leader-election-rolebinding from namespace ns-yourname
Deleted clusterrolebinding/release.ns-yourname-operator-manager-rolebinding from namespace ns-yourname
Deleted clusterrolebinding/release.ns-yourname-operator-proxy-rolebinding from namespace ns-yourname
Deleting PVCs
Deleted pvc/dai-xlr-ns-yourname-digitalai-release from namespace ns-yourname
Deleted pvc/data-dai-xlr-ns-yourname-postgresql-0 from namespace ns-yourname
Deleted pvc/data-dai-xlr-ns-yourname-rabbitmq-0 from namespace ns-yourname
Clean finished successfully!
```

The clean process is cleaning everything from the cluster. 

For the other questions and answers details check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-kube.html#xl-kube-clean)
