
# Lab 8 - How to change configuration file

Here we will update the configuration file on the central configuration server. We will update `deploy-server.yaml`.

## Custom configuration of `deploy-server.yaml.template` from cc pods

### Steps

1. Download current template configuration file that exists on cc Deploy pod that is running.
   For example, if you want to update `deploy-server.yaml.template`, it is in the 
   `/opt/xebialabs/central-configuration-server/central-conf/deploy-server.yaml.template` in master pod.

    ```shell
    kubectl cp digitalai/dai-xld-my-namespace-deploy-cc-server-0:/opt/xebialabs/central-configuration-server/central-conf/deploy-server.yaml.template .
    ```

2. Update the CR file or CR on the cluster

   Use that file to update your CR file under `spec.deploy.configurationManagement.centralConfiguration.configuration.scriptData` path. Add there the content of the `deploy-server.yaml.template` file under the `deploy-server.yaml.template` key.

   Also update the script under the `spec.deploy.configurationManagement.centralConfiguration.configuration.script` path.

   For example:

    ```yaml
    ...
            script: |-
                ...
                cp /opt/xebialabs/central-configuration-server/xld-configuration-management/deploy-server.yaml.template /opt/xebialabs/central-configuration-server/central-conf/deploy-server.yaml.template && echo "Changing the deploy-server.yaml.template";
            scriptData:
                ...
                deploy-server.yaml.template: |-
                  deploy.server:
                    ...
    ```

3. If you have oidc enabled in CR, in that case disable it. Because the changes that are from there will conflict with your changes in the `deploy-server.yaml.template` file.

    Just in CR file put `spec.oidc.enabled: false`.
    Note: when you disable oidc in CR, `/opt/xebialabs/central-configuration-server/centralConfiguration/deploy-oidc.yaml` will be removed. For this reason, it is necessary to copy this file also. In `spec.deploy.configurationManagement.centralConfiguration.configuration.script` you would have:

    ```yaml
    ...
        script: |-
            ...
            cp /opt/xebialabs/central-configuration-server/xld-configuration-management/deploy-server.yaml.template /opt/xebialabs/central-configuration-server/central-conf/deploy-server.yaml.template && echo "Changing the deploy-server.yaml.template";
            cp /opt/xebialabs/central-configuration-server/xld-configuration-management/deploy-oidc.yaml /opt/xebialabs/central-configuration-server/centralConfiguration/deploy-oidc.yaml && echo "Changing the deploy-oidc.yaml";
        scriptData:
            ...
            deploy-server.yaml.template: |-
              deploy.server:
                ...
            deploy-oidc.yaml: |-
              deploy.security:
                ...
    ```

4. Save and apply changes from the CR file. Restart pods.

    ```shell
    kubectl rollout restart sts dai-xld-digitalai-deploy-cc-server -n digitalai
    kubectl rollout restart sts dai-xld-digitalai-deploy-master -n digitalai
    kubectl rollout restart sts dai-xld-digitalai-deploy-worker -n digitalai
    ```
