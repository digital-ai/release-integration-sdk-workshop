# xl-kube-workshop
Workshop for installing Digital.ai Release and Digital.ai Deploy on a Kubernetes cluster using the `xl kube` command line interface.

## Prerequisites

- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [xl-cli 22.3.2](https://dist.xebialabs.com/public/xl-cli/22.3.2/)
- [yq](https://github.com/mikefarah/yq)
- Java 11 - keytool (only if you plan to use the generation of the keystore inside the xl-cli kube)

## Workshop Content

### Part 0

0. Setup kubectl context

### Part 1

1. Install Digital.ai Release 22.2.4
   a. Wait for resources with xl kube check
   b. Discover how to open the page and login
   c. Discover how to check postgres and rabbitmq password
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
