### Production setup (optional)

Container-based integration plugins are meant to run on a Kubernetes cluster. To have a hands-on experience you will need the following components.

⚡️ **Note:** Some Kubernetes experience is needed to make the best of this exercise. Feel free to skip this part of Kubernetes is all new to you (or be prepared for a steep learning curve)

* Access to a Kubernetes cluster. This could be Docker Desktop or minikube; or a cloud-based environment like AWS EKS, Azure AKS, Google Cloud or OpenShift.
* [kubectl](https://kubernetes.io/docs/tasks/tools/)
* Helm
* [k9s](https://k9scli.io/topics/install/) - _(Optional)_ Kubernetes CLI to "Manage Your Clusters In Style". This is a very handy utility.

When using one of the latter, make sure to install the command line tools provided by the vendor.

#### Mandatory for Azure

- [Azure Cli - az](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) - if you are working with Azure during workshop

#### Mandatory for Minikube

- [minikube](https://minikube.sigs.k8s.io/docs/start/) - if you plan to use Minikube during workshop (use the latest version), we are testing only on minikube with [virtualbox driver](https://minikube.sigs.k8s.io/docs/drivers/virtualbox/)
- [virtualbox driver](https://minikube.sigs.k8s.io/docs/drivers/virtualbox/)

#### Mandatory for Docker Desktop

- [docker](https://docs.docker.com/get-docker/) - if you plan to use Docker during workshop (use the latest version)
