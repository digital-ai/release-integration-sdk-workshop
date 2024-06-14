# Lab 7 - Installing the Release runner using `xl`

This lab teaches you how to install the Release runner into Kubernetes using the `xl` command line utility, the companion tool to Digital.ai Release and Deploy.


<!-- FIXME no longer needed -->

## Configure Digital.ai Release for Release runner

We need to configure Release with a _service user_ for the Release runner and give it the needed permissions.

Use the following command to create an account for the runner. Feel free to use a different password. The `remote-runner-user.yaml` file can be found in the `part-3` directory of the SDK workshop repository.

    cd part-3
    ./xlw apply -f remote-runnner-user.yaml --values password=Remote123

The runner needs a token to register itself with the Release server. In order to obtain a token, do the following

* Log in to release as the `remote-runner` user with the password you gave as a parameter to the `xl apply` command
* Go to the [Access tokens](http://digitalai.release.local:5516/#/personal-access-token) page: In the top-right corner, click on the **RR** icon and select **Access tokens**
* Enter a token name, for example `Local runner`, and click Generate. Copy the token and store it somewhere for future reference.

### Disable SDK runner

The SDK environment already contains a  runner running in plain Docker. We need to disable it as to make sure new tasks will be run in the Kubernetes runner we are about to install. 

In the Release UI, log in as **admin** and check the **[Connections](http://digitalai.release.local:5516/#/configuration)** page for Release runner connections. You should see an entry for Release runner.

![Job runners in Connections](img/job-runners.png)

Now disable this runner for by choosing **Edit** for the **Local Docker** entry and disabling the **Enabled** switch.


## Set up the runner

Install the Release runner into your local Kubernetes environment with the `xl kube install` command and look closely at the answers below. Note that sometimes you can take the default, sometimes you need to give the value as prompted below, and sometimes you need to give a custom value.

💡 **Note:** You can also use `xl kube install` to install Release or Deploy itself. See the [Installing Digital.ai Release onto Kubernetes](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-before-you-begin.html) on our documentation site.

Start the command with

    ./xlw kube install

We've marked some questions with a warning sign where you need to pay extra attention.

``` 
? Following kubectl context will be used during execution: `docker-desktop`?
» Yes
? Select the Kubernetes setup where the Digital.ai Devops Platform will be installed, updated or cleaned:
»⚠️ AWSEKS [AWS EKS]
⚡️ Choose the 'AWS' option when using Docker Desktop Kubernetes.
? Do you want to use an custom Kubernetes namespace (current default is 'digitalai'):
» No
? Product server you want to perform install for:
»⚠️ dai-release-remote-runner [Remote Runner for Digital.ai Release]
? Select type of image registry:
» default
? Enter the repository name (eg: <repositoryName> from <repositoryName>/<imageName>:<tagName>):
» xebialabs
? Enter the remote runner image name (eg: <imageName> from <repositoryName>/<imageName>:<tagName>):
» release-remote-runner
? Enter the image tag (eg: <tagName> from <repositoryName>/<imageName>:<tagName>):
» 23.3.0
? Enter the Remote Runner Helm Chart release name: 
» remote-runner
? Use default version of the Remote Runner helm chart. 
» Yes
? Enter the Release URL that will be used by remote runner:
»⚠️ http://host.docker.internal:5516
? Enter the Release Token that will be used by remote runner:
»⚠️ rpa_... (Paste token here)
? Enter the remote runner replica count: 
» 1
? Enable truststore for Remote Runner: 
» No
	 -------------------------------- ----------------------------------------------------
	| LABEL                          | VALUE                                              |
	 -------------------------------- ----------------------------------------------------
	| CleanBefore                    | false                                              |
	| CreateNamespace                | true                                               |
	| ExternalOidcConf               | external: false                                    |
	| GenerationDateTime             | 20231003-135901                                    |
	| ImageNameRemoteRunner          | release-remote-runner                              |
	| ImageRegistryType              | default                                            |
	| ImageTagRemoteRunner           | 23.3.0-beta.6                                      |
	| IngressType                    | nginx                                              |
	| IngressTypeGeneric             | nginx                                              |
	| IngressTypeOpenshift           | route                                              |
	| IsCustomImageRegistry          | false                                              |
	| IsRemoteRunnerTruststoreEnab.. | false                                              |
	| K8sSetup                       | AWSEKS                                             |
	| OidcConfigType                 | no-oidc                                            |
	| OsType                         | darwin                                             |
	| ProcessType                    | install                                            |
	| RemoteRunnerCount              | 1                                                  |
	| RemoteRunnerGeneration         | false                                              |
	| RemoteRunnerInstall            | true                                               |
	| RemoteRunnerInstallConfirm     | false                                              |
	| RemoteRunnerReleaseName        | remote-runner                                      |
	| RemoteRunnerReleaseUrl         | http://host.docker.internal:5516                   |
	| RemoteRunnerRepositoryName     | xebialabsunsupported                               |
	| RemoteRunnerToken              | rpa_9623e512832f89c2f8d60d344d681baa3a7cfb6f       |
	| RemoteRunnerUseDefaultLocation | true                                               |
	| ServerType                     | dai-release-remote-runner                          |
	| ShortServerName                | other                                              |
	| UseCustomNamespace             | false                                              |
	 -------------------------------- ----------------------------------------------------
? Do you want to proceed to the deployment with these values? Yes
For current process files will be generated in the: digitalai/dai-release-remote-runner/digitalai/20231003-135901/kubernetes
Generated answers file successfully: digitalai/generated_answers_dai-release-remote-runner_digitalai_install-20231003-135901.yaml 
Starting install processing.
Installing helm chart remote-runner from: digitalai/dai-release-remote-runner/digitalai/20231003-135901/kubernetes/remote-runner-0.1.0.tgz
Using helm chart values from: digitalai/dai-release-remote-runner/digitalai/20231003-135901/kubernetes/values-cli.yaml
Installed helm chart remote-runner to namespace digitalai
```

Check the runner logs to see if it started correctly and is able to connect to Release.

We found **k9s** very helpful when dealing with Kubernetes. In a new terminal window, start the `k9s` utility and open the logs for the `remote-runner` pod to see if it is starting correctly.

## Check Runner in Release

In the Release UI, log in as **admin** and check the **[Connections](http://digitalai.release.local:5516/#/configuration)** page for Release runner connections. You should see an additional entry for the Remote Runner that was just installed.

Run one of the templates you built previously. 

The tasks should run inside Kubernetes. You can check this in `k9s` by looking for pods with random names like `fa6fdfd01554f848a5145343669693d4bc48d901`.

![K9s](img/k9s.png)

## Clean up

To remove the runner from Kubernetes, use the `xl kube clean` command, with the 'answers' file that was created during installation.

    ./xlw kube clean --skip-prompts --answers digitalai/generated_answers_dai-release-runner_digitalai_install-DATE.yaml 

If you are into Unix command line magic, you can use this command to do a clean with the last answers file generated:

    ./xlw kube clean --skip-prompts --answers `ls -t digitalai/generated_answers_dai-release-runner_digitalai_install-* | head -1`
