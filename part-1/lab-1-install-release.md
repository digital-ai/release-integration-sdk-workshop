
# Lab 1 - Install Digital.ai Release 22.2.4

## Intro to `xl kube`

Check [XL Kube Command Reference](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-kube.html) for the `xl kube` details.

## Installation

```shell
xl kube install
```

- TODO:
  - answers
  - domain
  - license

For the other questions and answers details check [Installation Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-install-wizard-release.html)

## Wait for resources with xl kube check

```shell
xl kube check --wait-for-ready 5
xl kube check --wait-for-ready 5 --skip-collecting
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
