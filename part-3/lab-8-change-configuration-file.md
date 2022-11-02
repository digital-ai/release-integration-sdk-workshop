
# Lab 8 - How to change configuration file

Here we will update the configuration file on the central configuration server. We will update `deploy-server.yaml`.

## Custom configuration of `deploy-repository.yaml.template` on CC pods and `logback.xml` on worker pods

### Steps

1. Download current template configuration file that exists on cc Deploy pod that is running.
   For example, if you want to update `deploy-repository.yaml.template`, it is in the 
   `/opt/xebialabs/central-configuration-server/central-conf/deploy-repository.yaml.template` in master pod.

   Download the `deploy-repository.yaml.template` from a CC pod.

    ```shell
    mkdir server-confs
    kubectl cp ns-yourname/dai-xld-ns-yourname-digitalai-deploy-cc-server-0:/opt/xebialabs/central-configuration-server/central-conf/deploy-repository.yaml.template ./server-confs/deploy-repository.yaml.template
    kubectl cp ns-yourname/dai-xld-ns-yourname-digitalai-deploy-worker-0:/opt/xebialabs/deploy-task-engine/conf/logback.xml ./server-confs/logback.xml
    ```

   Download the `logback.xml` from a worker pod.

    ```shell
    kubectl cp ns-yourname/dai-xld-ns-yourname-digitalai-deploy-worker-0:/opt/xebialabs/deploy-task-engine/conf/logback.xml ./server-confs/logback.xml
    ```

2. Update the CR file or CR on the cluster

   Use that file to update your CR file under `spec.deploy.configurationManagement.centralConfiguration.configuration.scriptData` path. Add there the content of the `deploy-repository.yaml.template` file under the `deploy-repository.yaml.template` key.

   Also update the script under the `spec.deploy.configurationManagement.centralConfiguration.configuration.script` path.

   For example:

    ```yaml
    ...
    spec:
      deploy:
        configurationManagement:
          centralConfiguration:
            configuration:
   
              resetCcFiles:
                ...
                - deploy-repository.yaml
              script: |-
                ...
                cp /opt/xebialabs/central-configuration-server/xld-configuration-management/deploy-repository.yaml.template /opt/xebialabs/central-configuration-server/central-conf/deploy-repository.yaml.template && echo "Changing the deploy-repository.yaml.template";
              scriptData:
                ...
                deploy-repository.yaml.template: |-
                  xl.repository:
                    ...
                    database:
                      ...
                      connection-timeout: "60 seconds"
                      max-pool-size: 5
          worker:
            configuration:
              script: |-
                ...
                cp /opt/xebialabs/deploy-task-engine/xld-configuration-management/logback.xml /opt/xebialabs/deploy-task-engine/conf/logback.xml && echo "Changing the logback.xml";
              scriptData: 
                ...
                logback.xml: |-
                  <configuration>
                    ...
                    <root level="debug">
                    ...
                  </configuration>
    ```

3. If you have oidc enabled in CR, in that case disable it. Because the changes that are from there will conflict with your changes in the `deploy-repository.yaml.template` file.

    Just in CR file put `spec.oidc.enabled: false`.
    Note: when you disable oidc in CR, `/opt/xebialabs/central-configuration-server/centralConfiguration/deploy-oidc.yaml` will be removed. For this reason, it is necessary to copy this file also. In `spec.deploy.configurationManagement.centralConfiguration.configuration.script` you would have:

    ```yaml
    ...
    spec:
      deploy:
        configurationManagement:
          centralConfiguration:
            configuration
              script: |-
                ...
                cp /opt/xebialabs/central-configuration-server/xld-configuration-management/deploy-repository.yaml.template /opt/xebialabs/central-configuration-server/central-conf/deploy-repository.yaml.template && echo "Changing the deploy-repository.yaml.template";
                cp /opt/xebialabs/central-configuration-server/xld-configuration-management/deploy-oidc.yaml /opt/xebialabs/central-configuration-server/centralConfiguration/deploy-oidc.yaml && echo "Changing the deploy-oidc.yaml";
              scriptData:
                ...
                deploy-repository.yaml.template: |-
                  xl.repository:
                    ...
                    database:
                      ...
                      connection-timeout: "60 seconds"
                      max-pool-size: 5
                deploy-oidc.yaml: |-
                  deploy.security:
                    ...
    ```

4. Save and apply changes from the CR file. Restart pods.

   Apply the changes:

    ```shell
    kubectl apply -f ./digitalai/dai-deploy/ns-yourname/20221102-205418/kubernetes/dai-deploy_cr.yaml -n ns-yourname
    ```

   Restart the pods

    ```shell
    kubectl rollout restart sts dai-xld-ns-yourname-digitalai-deploy-cc-server -n ns-yourname
    kubectl rollout restart sts dai-xld-ns-yourname-digitalai-deploy-master -n ns-yourname
    kubectl rollout restart sts dai-xld-ns-yourname-digitalai-deploy-worker -n ns-yourname
    ```

---

That's all Folks!
