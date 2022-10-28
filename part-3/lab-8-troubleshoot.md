
# Lab 8 - Fix errors - Troubleshoot

## Installation with few errors

```shell
xl kube install
```

- TODO:
  - answers
  - domain
  - license

## Using wrong tag in the operator

Detect the problem with:

```shell
xl kube check --wait-for-ready 1 --skip-collecting
```

Repeat installation with answers file

```shell
xl kube install --answers ...
```

## Using wrong storage class

Detect the problem with:

```shell
xl kube check --wait-for-ready 1 --skip-collecting
```

Repeat installation with answers file

```shell
kubectl apply -n ... -f ...
```
