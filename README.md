![Release +_Deploy on Kubernetes in minutes!](xl kube install logo.jpg)

Workshop for installing Digital.ai Release and Digital.ai Deploy on a Kubernetes cluster using the `xl kube` command line interface.

## Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [xl-cli 22.3.2](https://dist.xebialabs.com/public/xl-cli/22.3.2/) - [Installation instructions](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/how-to/install-the-xl-cli.html)
- [yq](https://github.com/mikefarah/yq)

Optional

- Java 11 - keytool (only if you plan to use the generation of the keystore inside the xl-cli kube)
- `az` ?
- `helm` ?
- `k9s`


## Workshop Content

### General remarks

* `xl something --help` is your friend! For example: `xl kube install --help`
* Passwords are not in this repo. Ask workshop teachers for the needed credentials.


### Part 0

0. Setup kubectl context

### Part 1

1. Install Digital.ai Release 22.2.4
   a. Wait for resources with xl kube check
   b. Discover how to open the page and login
2. OIDC setup (use Identity service)
3. Upgrade Release 22.3.1
4. Clean

### Part 2

5. Install Deploy with –files 22.3.1
6. Use private image registry for all images
    a. Installation
    b. Upgrade
7. Clean

### Part 3

8. Fix errors - Troubleshoot
    a. Use wrong tag
    b. Use for postgres wrong storageclass
9. How to change configuration file
    a. Which file??? - nexus/maven settings (deploy)

## Workshop Agenda

1. Introduction - PowerPoint
2. Work on Labs
    a. Part 1
    b. Part 2
    c. Part 3

## TODOs

- License for participants 
- Azure Cluster
- Test multiple installation of release and deploy on the same cluster
- Test on minikube
- Test on docker
