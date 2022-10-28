
# Lab 5 - Install Deploy with –files 22.3.1

## Installation

```shell
xl kube install --dry-run
```

- TODO:
  - answers
  - domain
  - license

For the other questions and answers details check [Installation Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-deploy-version-v.22.3/page/deploy/operator/xl-op-install-wizard-deploy.html)

```shell
xl kube install --files ...
```

## Wait for resources with xl kube check

```shell
xl kube check --wait-for-ready 5
xl kube check --wait-for-ready 5 --skip-collecting
xl kube check --wait-for-ready 5 --zip-files
```

## Discover how to open the page and login

Use

```shell
kubectl edit ...
```

Update the with hostname:

```yaml
    spec.nginx-ingress-controller.service.annotations:
        service.beta.kubernetes.io/azure-dns-label-name: xlr...
```

Restart the release STS:

```shell
kubect sts restart ...
```

## Discover how to check postgres and rabbitmq password
