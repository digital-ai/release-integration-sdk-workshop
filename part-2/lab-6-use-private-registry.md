
# Lab 6 - Use private image registry for all images

## Prepare

### Download matching version of the xl-op-blueprints

Check what you need with `xl kube help`, for example:

```shell
$ xl kube help

Install, upgrade or clean Digital.ai Deploy or Digital.ai Release on a Kubernetes cluster using operator technology.

Installation blueprint files are used from https://dist.xebialabs.com/public/xl-op-blueprints/22.3.1/.

You need to have kubectl installed and configured for the target Kubernetes cluster.
```

You can see from here that `xl kube` needs blueprints from location [https://dist.xebialabs.com/public/xl-op-blueprints/22.3.1/](https://dist.xebialabs.com/public/xl-op-blueprints/22.3.1/).
You need to download and put all files from that location to the server where you will execute `xl kube`.

:::TIP
You can download zipped version of the xl-op-blueprints from here:
[https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/](https://nexus.xebialabs.com/nexus/content/repositories/digitalai-public/ai/digital/xlclient/blueprints/xl-op-blueprints/)

Unzip it to the server where you will execute `xl kube`.
:::

## Installation

### Use `xl kube install` with dry-run to generate all yaml files

Run on your air-gapped environment the installation dry-run to generate all the needed files.
Use as repository reference your image repository.

For example here is example of the dry-run installation on the minikube.
Example is with the repository myrepo_host/myrepo, so operator image would be: myrepo_host/myrepo/deploy-operator:22.3.1

```shell
xl kube install -D -l ./xl-op-blueprints
```

```text
? Following kubectl context will be used during execution: `minikube`? Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): No
? Product server you want to perform install for: dai-deploy [Digital.ai Deploy]
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): localhost:5000/myrepo
? Enter the deploy server image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): xl-deploy
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>): 22.3.1
? Enter the deploy task engine image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): deploy-task-engine
? Enter the central configuration image name for version 22 and above (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): central-configuration
? Enter the deploy master server replica count: 1
? Enter PVC size for Deploy master (Gi): 1
? Select between supported Access Modes: ReadWriteOnce [ReadWriteOnce]
? Enter the deploy worker replica count: 1
? Enter PVC size for Deploy worker (Gi): 1
? Enter PVC size for Central Configuration (Gi): 0.500000
? Select between supported ingress types: haproxy [HAProxy]
? Do you want to enable an TLS/SSL configuration (if yes, requires existing TLS secret in the namespace): No
? Provide DNS name for accessing UI of the server: test.com
? Provide administrator password: 30Q5utfMV6O9wnHF
? Type of the OIDC configuration: embedded [Embedded Keycloak Configuration]
? Use embedded DB for keycloak: Yes
? Enter Keycloak public URL: k.test.com
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): localhost:5000/myrepo/deploy-operator:22.3.1
? Select source of the license: file [Path to the license file (the file can be in clean text or base64 encoded)]
? Provide license file for the server: /Users/vpugar/Downloads/xld-license.lic
? Select source of the repository keystore: generate [Generate the repository keystore during installation (you need to have keytool utility installed in your path)]
? Provide keystore passphrase: 1uwAFCtUJEdwmaDi
? Provide storage class for the server: standard
? Do you want to install a new PostgreSQL on the cluster: Yes
? Provide Storage Class to be defined for PostgreSQL: standard
? Provide PVC size for PostgreSQL (Gi): 1
? Do you want to install a new RabbitMQ on the cluster: Yes
? Replica count to be defined for RabbitMQ: 1
? Storage Class to be defined for RabbitMQ: standard
? Provide PVC size for RabbitMQ (Gi): 1

...

? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20221020-001911/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_install-20221020-001911.yaml
Starting install processing.
...
```

Dry run will generate the files in the working folder, in the `digitalai/dai-deploy/digitalai/20221020-001911/kubernetes` folder like the example says.

### Edit generated files and update the image repository

Go through generated files and update the image repository.
There are 3 files that we need to update in the `kubernetes` directory (check the previous step example for details).

The `spec.centralConfiguration.image.repository`, `spec.ServerImageRepository` and `spec.WorkerImageRepository` should have already correct value.

#### kubernetes/dai-deploy_cr.yaml

- spec.TinyToolsImageRepository: "myrepo_host/myrepo/tiny-tools"

##### If you are using nginx

- spec.nginx-ingress-controller.defaultBackend.image.registry: myrepo_host
- spec.nginx-ingress-controller.defaultBackend.image.repository: myrepo/nginx
- spec.nginx-ingress-controller.image.registry: myrepo_host
- spec.nginx-ingress-controller.image.repository: myrepo/nginx-ingress-controller

##### If you are using haproxy

- spec.haproxy-ingress.controller.image.repository: myrepo_host/myrepo/haproxy-ingress

##### If you are using embedded keycloak

- spec.keycloak.image.repository: myrepo_host/myrepo/keycloak
- spec.keycloak.postgresql.image.registry: myrepo_host
- spec.keycloak.postgresql.image.repository: myrepo/postgresql

##### If you are using embedded postgresql

- spec.postgresql.image.registry: myrepo_host
- spec.postgresql.image.repository: myrepo/postgresql

##### If you are using embedded rabbitmq

- spec.rabbitmq.image.registry: myrepo_host
- spec.rabbitmq.image.repository: myrepo/rabbitmq
- spec.rabbitmq.volumePermissions.image.registry: myrepo_host
- spec.rabbitmq.volumePermissions.image.repository: myrepo/bitnami-shell

#### kubernetes/template/deployment.yaml

- spec.template.spec.containers[0].image: myrepo_host/myrepo/kube-rbac-proxy:v0.8.0

The `spec.template.spec.containers[1].image` should have already correct value.

#### kubernetes/template/postgresql-init-keycloak-db.yaml

- spec.template.spec.initContainers[0].image: myrepo_host/myrepo/tiny-tools:22.2.0
- spec.template.spec.containers[0].image: myrepo_host/myrepo/tiny-tools:22.2.0

### Use `xl kube install` with changed files to apply everything to the cluster

Following command will apply the just changed files on the K8S cluster:

```shell
xl kube install -f 20221020-001911 -l ./xl-op-blueprints
```

After everything is on the cluster, you will see operator other resources pods running.

## Upgrade

pgrade steps should be same as for usual installation with dry-run.

During upgrade for the question `Edit list of custom resource keys that will migrate to the new Deploy CR:` append to the list following keys:

```text
spec.TinyToolsImageRepository
spec.nginx-ingress-controller.defaultBackend.image.registry
spec.nginx-ingress-controller.defaultBackend.image.repository
spec.nginx-ingress-controller.image.registry
spec.nginx-ingress-controller.image.repository
spec.haproxy-ingress.controller.image.repository
spec.keycloak.image.repository
spec.keycloak.postgresql.image.registry
spec.keycloak.postgresql.image.repository
spec.postgresql.image.registry
spec.postgresql.image.repository
spec.rabbitmq.image.registry
spec.rabbitmq.image.repository
spec.rabbitmq.volumePermissions.image.registry
spec.rabbitmq.volumePermissions.image.repository
```

### Example of running dry-run upgrade

```shell
xl kube upgrade -D -l ./xl-op-blueprints
```

```text
...
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>): localhost:5000/myrepo
...
? Enter the operator image to use (eg: <repositoryName>/<imageName>:<tagName>): localhost:5000/myrepo/deploy-operator:22.3.1
...
? Edit list of custom resource keys that will migrate to the new Deploy CR: 
...
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-deploy/digitalai/20221020-011911/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-deploy_digitalai_upgrade-20221020-011911.yaml
Starting upgrade processing.
...
```

### Edit kubernetes/template/deployment.yaml

Update:

- spec.template.spec.containers[0].image: myrepo_host/myrepo/kube-rbac-proxy:v0.8.0

The `spec.template.spec.containers[1].image` should have already correct value.

### Edit kubernetes/template/postgresql-init-keycloak-db.yaml

Update:

- spec.template.spec.initContainers[0].image: myrepo_host/myrepo/tiny-tools:22.2.0
- spec.template.spec.containers[0].image: myrepo_host/myrepo/tiny-tools:22.2.0

### Example of running upgrade with generated files

After doing dry-run upgrade apply the files to the cluster:

```shell
xl kube upgrade -f 20221020-011911 -l ./xl-op-blueprints
...
```
