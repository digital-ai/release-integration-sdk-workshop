# Lab 6 - Production setup: Kubernetes & Release runner 

⚡️ **Note:** Some Kubernetes experience is needed to make the best of this exercise. Feel free to skip this part of Kubernetes is all new to you (or be prepared for a steep learning curve)

In previous labs you have used the development environment that is purely Docker-based. However, this set up is not suitable for production use, due to issues with  scalability, manageability and security.

This part of the workshop walks you through setting up Digital.ai Release with the **Release runner in Kubernetes** to run container-based tasks.

The Release runner is the glue between the main Release application and the container tasks that are being run. It lives inside Kubernetes, registers itself with Digital.ai Release and then waits for work. When a task needs to be executed, it launches a pod to do so and takes care of the communication between task and Release.

Note that for this setup, it is not required that the Release server itself runs inside Kubernetes. In fact, we will keep the same development server and make it use Kubernetes instead of Docker. 

Another thing that is useful to know is that the Release runner establishes its connection to Release through an outbound connection to the Release server. The Release server needs to be reachable, but the Release runner inside Kubernetes does not need to be exposed. This is useful for scenarios where container-based task run in a priviliged network, for example to do production deployments. Only the tasks will have network access then, not the entire Release server.

## Prerequisites

To run container-based integration plugins on a Kubernetes cluster, you will need the following components.

* Access to a local Kubernetes cluster. This could be Docker Desktop (recommended), minikube or K3d.
* [kubectl](https://kubernetes.io/docs/tasks/tools/) (This may have been installed already with Docker Desktop)
* [Helm](https://helm.sh/docs/intro/install/)
* [yq](https://mikefarah.gitbook.io/yq/v/v3.x/) - Command line utility to process Yaml. This is used by `xl` under the hood.
* A Java JDK - The `xl` utility has a dependency on the `keytool` command that is bundled with the JDK. 
* [k9s](https://k9scli.io/topics/install/) - _(Optional)_ Kubernetes CLI to "Manage Your Clusters In Style". This is a very handy utility.


Depending on the flavor of Kubernetes you are using, install the following tools provided by the vendor.

### Docker Desktop

In Docker Desktop, just enable Kubernetes in the Settings and restart.

### Minikube

If you plan to use Minikube during workshop, please use the latest version
* [minikube](https://minikube.sigs.k8s.io/docs/start/)

We have tested minikube with **virtualbox driver**
* [virtualbox driver](https://minikube.sigs.k8s.io/docs/drivers/virtualbox/)

### Cloud-based Kubernetes

A cloud-based environment like AWS EKS, Azure AKS, Google Cloud or OpenShift is possible, but may be a bit tricky to set up. The Release runner opens a connection to the Release server, not the other way around. When running Release locally in Docker and the runner in a cloud service, you would need to expose the Release port to the internet using a tool like `ngrok`. Another option would be to install Release into Kubernetes as well, using the `xl kube install` tool. Both approaches take a bit more work and are beyond the scope of this workshop.

## Configure your `hosts` file

The Release runner needs to be able to find the other components in the system: the Release server and the registry we just installed. The easiest way to do so is to add it to your local machine's `hosts` file. 

You probably have done this already in [Lab 1](../part-1/lab-1-run-hello-world.md#configure-your-hosts-file).

If not, here are the steps for reference:

**Windows**

Add the following entries to `C:\Windows\System32\drivers\etc\hosts` (Run as administrator permission is required to edit):

    127.0.0.1 digitalai.release.local
    127.0.0.1 container-registry
    127.0.0.1 host.docker.internal

**Unix / macOS**

Add the following lines to `/etc/hosts` (sudo privileges is required to edit):

    127.0.0.1 digitalai.release.local
    127.0.0.1 container-registry
    127.0.0.1 host.docker.internal

### Check out the workshop repo

You will need to use some code examples that are in the workshop repository itself.

Check out the repository and navigate to `part-3`

**Http:**

    git clone https://github.com/digital-ai/release-integration-sdk-workshop.git

**SSH:**  

    git clone git@github.com:digital-ai/release-integration-sdk-workshop.git

Then:

    cd release-integration-sdk-workshop/part-3

### Run Digital.ai Release

We assume you have Digital.ai Release running in Docker. If not, please revisit [Lab 0](../part-1/lab-0-checkout-project-and-run-release.md).


### Set up the `xl` client

In the next lab, you wil install the Release runner using the `xl` command line utility, the companion tool to Digital.ai Release and Deploy.

💡 **Note:** The name 'xl' comes from 'XebiaLabs', the company that created Release and Deploy and is now part of Digital.ai

Let's first check that it works. 

For the workshop, we will use `xlw`, the "xl wrapper", a wrapper script that takes care of downloading and running the correct version of `xl`.

In the `part-3` directory, issue the following command:

**Windows**

    xlw.bat version 

**Unix / macOS**

    ./xlw version

The result should be something like:

```
Downloading xl binary to /Users/hsiemelink/.xebialabs/wrapper/24.1.3/xl
CLI version:             24.1.3
Git version:             v24.1.2-1-gfaaee99
Deploy API version:      xl-deploy/v1
Release API version:     xl-release/v1
Git commit:              faaee99d4b6e3a72b672c17ae29091edfe891345
Build date:              2024-06-12T12:24:42.326Z
GO version:              go1.21.8
OS/Arch:                 darwin/amd64
```


You are now set to start the installation procedure of the Release runner!

---
[Next](lab-7-install-release-runner)