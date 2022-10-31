
# Lab 2 - Upgrade Release 22.3.1

In this lab we will upgrade previous installation to the Digital.ai Release 22.3.1 version of product and operator.


You just need to start upgrade process by running:

```shell
xl kube upgrade
```

... and after that answer to the questions. Some of the questions are repeating from the installation.

Note that we are upgrading to 22.3.1. In case if you are sharing cluster with others, do not delete the CR that is shared between all installations.
So you need to answer to `Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:` with `Yes`.

All custom changes in the CR after installation must be preserved. So for the answer  `Edit list of custom resource keys that will migrate to the new Release CR:` 
you need to add all keys under which you changed the values. We have one example from previous lab `spec.nginx-ingress-controller.service.annotations`, because of that
we need to add key in the list, in the new line `.spec.nginx-ingress-controller.service.annotations`.

Here is example of the upgrade answers (example on the Azure):

```text
$ xl kube upgrade
? Following kubectl context will be used during execution: `azure-aks-test-cluster`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned: my-namespace
? Product server you want to perform upgrade for: dai-release [Digital.ai Release]
? Select the type of upgrade you want: operatorToOperator [Operator to Operator]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): xebialabs
? Enter the image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-release
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 22.3.1
? Type of the OIDC configuration: no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): xebialabs/release-operator:22.3.1
? Enter the name of custom resource definition you want to reuse or replace: digitalaireleases.xlr.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it: Yes
? Enter the name of custom resource: dai-xlr-my-namespace
? Edit list of custom resource keys that will migrate to the new Release CR: <Received>
? Should we preserve persisted volume claims? If not all volume data will be lost: Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | true                                               |
	| CrName                         | dai-xlr-my-namespace                               |
	| CrdName                        | digitalaireleases.xlr.digital.ai                   |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-152355                                    |
	| ImageNameRelease               | xl-release                                         |
	| ImageTag                       | 22.3.1                                             |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | my-namespace                                       |
	| OidcConfigType                 | no-oidc                                            |
	| OidcConfigTypeUpgrade          | no-oidc                                            |
	| OperatorImageReleaseGeneric    | xebialabs/release-operator:22.3.1                  |
	| OsType                         | darwin                                             |
	| PreserveCrValuesRelease        | .metadata.name\n.spec.AdminPassword\n.spec.repli.. |
	| PreservePvc                    | true                                               |
	| ProcessType                    | upgrade                                            |
	| RepositoryName                 | xebialabs                                          |
	| ServerType                     | dai-release                                        |
	| ShortServerName                | xlr                                                |
	| UpgradeType                    | operatorToOperator                                 |
	| UseCustomNamespace             | true                                               |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release/my-namespace/20221031-152355/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_my-namespace_upgrade-20221031-152355.yaml
Starting upgrade processing
Generated files successfully for operatorToOperator upgrade for AzureAKS.
CRD creation will be skipped, expecting to have CRD already on cluster
Cleaning the resources on the cluster!
CR dai-xlr-my-namespace is available, deleting
? Do you want to delete the resource digitalaireleases.xlr.digital.ai/dai-xlr-my-namespace: Yes
Deleted digitalaireleases.xlr.digital.ai/dai-xlr-my-namespace from namespace my-namespace
Deleting statefulsets
? Do you want to delete the resource sts/dai-xlr-my-namespace-digitalai-release: Yes
Deleted sts/dai-xlr-my-namespace-digitalai-release from namespace my-namespace (already deleted)
Deleting deployments
? Do you want to delete the resource deployment/xlr-operator-controller-manager: Yes
Deleted deployment/xlr-operator-controller-manager from namespace my-namespace
Deleting jobs
Deleting services
? Do you want to delete the resource svc/xlr-operator-controller-manager-metrics-service: Yes
Deleted svc/xlr-operator-controller-manager-metrics-service from namespace my-namespace
Deleting secrets
? Do you want to delete the resource secret/sh.helm.release.v1.dai-xlr-my-namespace.v2: Yes
Deleted secret/sh.helm.release.v1.dai-xlr-my-namespace.v2 from namespace my-namespace
? Do you want to delete the resource ingressclass/nginx-dai-xlr-my-namespace: Yes
Deleted ingressclass/nginx-dai-xlr-my-namespace from namespace my-namespace
Deleting roles
? Do you want to delete the resource role/xlr-operator-leader-election-role: Yes
Deleted role/xlr-operator-leader-election-role from namespace my-namespace
? Do you want to delete the resource clusterrole/dai-xlr-my-namespace-nginx-ingress-controller: Yes
Deleted clusterrole/dai-xlr-my-namespace-nginx-ingress-controller from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xlr-operator-manager-role: Yes
Deleted clusterrole/my-namespace-xlr-operator-manager-role from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xlr-operator-metrics-reader: Yes
Deleted clusterrole/my-namespace-xlr-operator-metrics-reader from namespace my-namespace
? Do you want to delete the resource clusterrole/my-namespace-xlr-operator-proxy-role: Yes
Deleted clusterrole/my-namespace-xlr-operator-proxy-role from namespace my-namespace
? Do you want to delete the resource rolebinding/xlr-operator-leader-election-rolebinding: Yes
Deleted rolebinding/xlr-operator-leader-election-rolebinding from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/dai-xlr-my-namespace-nginx-ingress-controller: Yes
Deleted clusterrolebinding/dai-xlr-my-namespace-nginx-ingress-controller from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xlr-operator-manager-rolebinding: Yes
Deleted clusterrolebinding/my-namespace-xlr-operator-manager-rolebinding from namespace my-namespace
? Do you want to delete the resource clusterrolebinding/my-namespace-xlr-operator-proxy-rolebinding: Yes
Deleted clusterrolebinding/my-namespace-xlr-operator-proxy-rolebinding from namespace my-namespace
Applying resources to the cluster!
Applied resource clusterrole/my-namespace-xlr-operator-proxy-role from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/my-namespace-xlr-operator-manager-role from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/my-namespace-xlr-operator-metrics-reader from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xlr-operator-controller-manager-metrics-service from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource deployment/xlr-operator-controller-manager from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/deployment.yaml
Applied resource role/xlr-operator-leader-election-role from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xlr-operator-leader-election-rolebinding from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xlr-operator-manager-rolebinding from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/my-namespace-xlr-operator-proxy-rolebinding from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalairelease/dai-xlr-my-namespace from the file digitalai/dai-release/my-namespace/20221031-152355/kubernetes/dai-release_cr.yaml
Upgrade finished successfully!
```

The upgrade process is first cleaning everything from the cluster and after that applying the new version.
For each resource it is asking for confirmation before delete.

For the other questions and answers details check [Upgrade Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-upgrade-wizard-release.html)
