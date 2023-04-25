# Lab 7 - Installing the Remote Runner using `xl`

This lab teaches you how to install the Remote Runner using the `xl` command line utility, the companion tool to Digital.ai Release and Deploy.

## Configure Digital.ai Release for Remote Runner

We need to configure Release with a _service user_ for the Remote Runner and give it the needed permissions.

Use the following command to create an account for the Remote Runner. Feel free to use a different password.

    docker run -it \
        -v ${PWD}:/opt/xebialabs/xl-client/config \
        --network dev-environment_default \
        xebialabsunsupported/xl-client:23.1.0-425.900 \
        apply -f /opt/xebialabs/xl-client/config/remote-runnner-user.yaml --values password=Remote123 --xl-release-url=http://dev-environment-digitalai-release-1:5516/
OR

    docker run -it \
        -v ${PWD}:/opt/xebialabs/xl-client/config \
        xebialabsunsupported/xl-client:23.1.0-425.900 \
        apply -f /opt/xebialabs/xl-client/config/remote-runnner-user.yaml --values password=Remote123 --xl-release-url=http://host.docker.internal:5516/



The Remote Runner needs a token to register itself with the Release server. In order to obtain a token, do the following

* Log in to release as the `remote-runner` user with the password you gave as a parameter to the `xl apply` command
* Go to the [Access tokens](http://digitalai.release.local:5516/#/personal-access-token) page: In the top-right corner, click on the **RR** icon and select **Access tokens**
* Enter a token name, for example `Local runner`, and click Generate. Copy the token and store it somewhere for future reference.


## Set up the runner

Install the Remote Runner into your local Kubernetes environment with the `xl kube install` command and look closely at the answers below. Note that sometimes you can take the default, sometimes you need to give the value as prompted below, and sometimes you need to give a custom value.

💡 **Note:** You can also use `xl kube install` to install Release or Deploy itself. XXX Link to documentation and workshop.

In case of k3d (replace the `cluster_name` with your k3d cluster - `k3d cluster list`):

    k3d kubeconfig get cluster_name | sed 's/0.0.0.0/host.docker.internal/g' > k3d-kubeconfig.json

We've marked some questions with a warning sign where you need to pay extra attention.

```
$ docker run -it \
    -e KUBECONFIG=/opt/xebialabs/.kube/config \
    -v ${PWD}/k3d-kubeconfig.json:/opt/xebialabs/.kube/config \
    -v ${PWD}:/opt/xebialabs/xl-client/config \
    xebialabsunsupported/xl-client:23.1.0-425.900 \
    kube install

? Following kubectl context will be used during execution: `k3d-xlrcluster`? 
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned: 
»⚠️ PlainK8s [Plain multi-node K8s cluster]
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'): 
» No
? Product server you want to perform install for: 
»⚠️ dai-release-runner [Remote Runner for Digital.ai Release]
? Select type of image registry: 
» default
? Enter the remote runner image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>): 
» xlr-remote-runner
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>):
» 0.1.33
? Enter the Remote Runner Helm Chart release name: 
» remote-runner
? Use default version of the Remote Runner helm chart. 
» Yes
? Enter the Release URL that will be used by remote runner: 
»⚠️ http://host.k3d.internal:5516/
? Enter the Release Token that will be used by remote runner: 
»⚠️ rpa_...
? Enter the remote runner replica count: 
» 1
? Provide storage class for the remote runner: 
» local-path
? Provide storage class base host path: 
» /kube
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20230424-181416                                    |
	| ImageNameRemoteRunner          | xlr-remote-runner                                  |
	| ImageRegistryType              | default                                            |
	| ImageTagRemoteRunner           | 0.1.33                                             |
	| IngressType                    | nginx                                              |
	| IsCustomImageRegistry          | false                                              |
	| K8sSetup                       | PlainK8s                                           |
	| OidcConfigType                 | no-oidc                                            |
	| OsType                         | linux                                              |
	| ProcessType                    | install                                            |
	| RemoteRunnerBaseHostPath       | /kube                                              |
	| RemoteRunnerCount              | 1                                                  |
	| RemoteRunnerReleaseName        | remote-runner                                      |
	| RemoteRunnerReleaseUrl         | http://host.k3d.internal:5516/                     |
	| RemoteRunnerStorageClass       | local-path                                         |
	| RemoteRunnerToken              | rpa_...                                            |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| ServerType                     | dai-release-runner                                 |
	| ShortServerName                | other                                              |
	| UseCustomNamespace             | false                                              |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-remote-runner/digitalai/20230424-181416/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release-runner_digitalai_install-20230424-181416.yaml
Starting install processing.
Installing helm chart remote-runner from /opt/xebialabs/xl-client/digitalai/dai-remote-runner/digitalai/20230424-181416/kubernetes/remote-runner-0.7.33.tgz
Installed helm chart remote-runner to namespace digitalai
```

XXX Start k9s

Check the remote runner logs to see if it started correctly and is able to connect to Release.

In the Release UI, log in as **admin** and check the **Connections** page for Remote Runner connections.


## Build & publish the plugin

Run the build script

Unix/macOS

* Builds the jar, image and pushes the image to the configured registry  
  ``` sh build.sh ```
* Builds the jar  
  ``` sh build.sh --jar ```
* Builds the image and pushes the image to the configured registry  
  ```  sh build.sh --image ```

Windows

* Builds the jar, image and pushes the image to the configured registry  
  ``` build.bat ```
* Builds the jar  
  ``` build.bat --jar ```
* Builds the image and pushes the image to the configured registry  
  ``` build.bat --image ```

## Install plugin into Release

In Release UI, use the Plugin Manager interface to upload the jar from `build`.
The jar takes the name of the project, for example `release-integration-template-python-1.0.0.jar`.

Then:
* Restart Release container and wait for it to come up
* Refresh the UI by pressing Reload in the browser.

## Test it!
Create a template with the task **Example: Hello** and run it!

## Clean up

To remove the Remote Runner, issue the following command

    helm delete remote-runner -n digitalai

