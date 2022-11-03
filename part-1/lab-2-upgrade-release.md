
# Lab 2 - Upgrade Release 22.3.1

In this lab we will upgrade the previous installation to  Digital.ai Release 22.3.1.

Just start upgrade process by running:

```shell
xl kube upgrade --skip-prompts
```

...and answer all the questions. Some of the questions are repeated from the installation.

* In this case we are sharing the cluster with others, so do NOT delete the Custom Resource that is shared between all installations.
  Answer the question `Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:` with **Yes**.

* All custom changes in the CR after installation must be preserved. So for the answer  `Edit list of custom resource keys that will migrate to the new Release CR:` you need to add all keys under which you changed the values. We have one example from previous lab `spec.nginx-ingress-controller.service.annotations`, because of that we need to add key in the list, in the new line `.spec.nginx-ingress-controller.service.annotations`.

* We are upgrading from Operator to Operator. Note: the `xl kube upgrade` command can be used to upgrade helm-based installations from 10.2 and higher XXX Check this value 

For the other questions and answers details check [Upgrade Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-upgrade-wizard-release.html)


Here is example of the upgrade answers, based on Azure:

```text
$ xl kube upgrade --skip-prompts
? Following kubectl context will be used during execution: `xl-kube-workshop`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned:
»⚠️ AzureAKS [Azure AKS]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'):
»⚠️ Yes
? Enter the name of the Kubernetes namespace where the Digital.ai DevOps Platform will be installed, updated or cleaned:
»⚠️ ns-yourname
? Product server you want to perform upgrade for:
» dai-release [Digital.ai Release]
? Select the type of upgrade you want:
» operatorToOperator [Operator to Operator]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>):
» xebialabs
? Enter the image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>):
» xl-release
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>):
» 22.3.1
? Type of the OIDC configuration:
» no-oidc [No OIDC Configuration]
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>):
» xebialabs/release-operator:22.3.1
? Enter the name of custom resource definition you want to reuse or replace:
» digitalaireleases.xlr.digital.ai
? Should CRD be reused, if No we will delete the CRD digitalaireleases.xlr.digital.ai, and all related CRs will be deleted with it:
»⚠️ Yes
? Enter the name of custom resource:
» dai-xlr-ns-yourname
? Edit list of custom resource keys that will migrate to the new Release CR: 
»⚠️ Add: 
.spec.nginx-ingress-controller.service.annotations
? Should we preserve persisted volume claims? If not all volume data will be lost: 
» Yes
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | true                                               |
	| CrName                         | dai-xlr-ns-yourname                                |
	| CrdName                        | digitalaireleases.xlr.digital.ai                   |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20221031-152355                                    |
	| ImageNameRelease               | xl-release                                         |
	| ImageTag                       | 22.3.1                                             |
	| IngressType                    | nginx                                              |
	| IsCrdReused                    | true                                               |
	| K8sSetup                       | AzureAKS                                           |
	| Namespace                      | ns-yourname                                        |
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
? Do you want to proceed to the deployment with these values? 
» Yes
For current process files will be generated in the: digitalai/dai-release/ns-yourname/20221031-152355/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release_ns-yourname_upgrade-20221031-152355.yaml
Starting upgrade processing
Generated files successfully for operatorToOperator upgrade for AzureAKS.
CRD creation will be skipped, expecting to have CRD already on cluster
Cleaning the resources on the cluster!
CR dai-xlr-ns-yourname is available, deleting
Deleted digitalaireleases.xlr.digital.ai/dai-xlr-ns-yourname from namespace ns-yourname
Deleting statefulsets
Deleted sts/dai-xlr-ns-yourname-digitalai-release from namespace ns-yourname (already deleted)
Deleting deployments
Deleted deployment/xlr-operator-controller-manager from namespace ns-yourname
Deleting jobs
Deleting services
Deleted svc/xlr-operator-controller-manager-metrics-service from namespace ns-yourname
Deleting secrets
Deleted secret/sh.helm.release.v1.dai-xlr-ns-yourname.v2 from namespace ns-yourname
Deleted ingressclass/nginx-dai-xlr-ns-yourname from namespace ns-yourname
Deleting roles
Deleted role/xlr-operator-leader-election-role from namespace ns-yourname
Deleted clusterrole/dai-xlr-ns-yourname-nginx-ingress-controller from namespace ns-yourname
Deleted clusterrole/release-ns-yourname-operator-manager-role from namespace ns-yourname
Deleted clusterrole/release-ns-yourname-operator-metrics-reader from namespace ns-yourname
Deleted clusterrole/release-ns-yourname-operator-proxy-role from namespace ns-yourname
Deleted rolebinding/xlr-operator-leader-election-rolebinding from namespace ns-yourname
Deleted clusterrolebinding/dai-xlr-ns-yourname-nginx-ingress-controller from namespace ns-yourname
Deleted clusterrolebinding/release-ns-yourname-operator-manager-rolebinding from namespace ns-yourname
Deleted clusterrolebinding/release-ns-yourname-operator-proxy-rolebinding from namespace ns-yourname
Applying resources to the cluster!
Applied resource clusterrole/release-ns-yourname-operator-proxy-role from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/cluster-role-digital-proxy-role.yaml
Applied resource clusterrole/release-ns-yourname-operator-manager-role from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/cluster-role-manager-role.yaml
Applied resource clusterrole/release-ns-yourname-operator-metrics-reader from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/cluster-role-metrics-reader.yaml
Applied resource service/xlr-operator-controller-manager-metrics-service from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/controller-manager-metrics-service.yaml
Applied resource deployment/xlr-operator-controller-manager from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/deployment.yaml
Applied resource role/xlr-operator-leader-election-role from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/leader-election-role.yaml
Applied resource rolebinding/xlr-operator-leader-election-rolebinding from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/leader-election-rolebinding.yaml
Applied resource clusterrolebinding/release-ns-yourname-operator-manager-rolebinding from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/manager-rolebinding.yaml
Applied resource clusterrolebinding/release-ns-yourname-operator-proxy-rolebinding from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/template/proxy-rolebinding.yaml
Applied resource digitalairelease/dai-xlr-ns-yourname from the file digitalai/dai-release/ns-yourname/20221031-152355/kubernetes/dai-release_cr.yaml
Upgrade finished successfully!
```

The upgrade process is first cleaning everything from the cluster and after that applying the new version.


Use the following command to wait for the upgrade to be complete:

```shell
xl kube check --skip-collecting --skip-prompts --wait-for-ready 5
```

When done, reload Release in the browser and check the version number again. It should now be **Version 22.3.1**

---

[Next](../part-1/lab-3-oidc-setup.md)
