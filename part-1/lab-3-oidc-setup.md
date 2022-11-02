
# Lab 3 - OIDC setup (use Identity service)

## Configure authentication on Digital.ai Platform

In this step, we will create an OIDC client that will allow Digital.ai Release to use the authentication service of Digital.ai Platform

### 1. Log in to Digital.ai Platform

Go to Digital.ai Platform at [https://devops-demo.staging.digital.ai/](https://devops-demo.staging.digital.ai/). 

Use the following credentials:

* **Username:** `admin-devops-demo.digital.ai`  
* **Password:** _ask in workshop_

### Create the OIDC client

Go to **Clients** and press the **Add OIDC Client** button.

1. Give the client a unique name:, for example "digitalai-yourname-Release'. 
2. Scroll down to **Valid Redirect URIs** and add `${releaseUrl}/oidc-login`. This should be the valid URL that points to your Release installation, followed by `/oidc-login`.

Create the client, and then go back and edit the client. Got to the **Credentials** section and find the following values
  
* **Client ID**
* **Client Secret**

Open a text editor with the snippet below and paste the values in there. Also add the Release URL 

```yaml
oidc:
  enabled: true
  external: true
  clientId: "[[YOUR CLIENT ID HERE]]"
  clientSecret: "[[YOUR CLIENT SECRET HERE]]"
  issuer: "https://identity-01.staging.digital.ai/auth/realms/devops-demo"
  redirectUri: "[[RELEASE URL HERE]]/oidc-login"
  postLogoutRedirectUri: "[[RELEASE URL HERE]]/oidc-login"
  rolesClaim: groups
  userNameClaim: preferred_username
  scopes: ["openid"]
```

## Configure Release with OIDC configuration

Open the `digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/dai-release_cr.yaml`. 

For your path: check your installation log in the line For current process files will be generated in the: digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes.


Find the `oidc` section. It should look like this:

Now replace it with the snippet you created above and save the file. 

We use the `kubectl` utility to apply the changes directly into Kubernetes. 

```shell
kubectl apply -n digitalai-yourname -f digitalai/dai-release/digitalai-yourname/20221031-131244/kubernetes/dai-release_cr.yaml
```

Open the **k9s** utility and see if the pods are starting.

When everything has restarted, log out as `admin`. You should now be redirected to the Digital.ai Platform log in screen.

Use the following credentials, the same as you used to log in to Digital.ai Platform.

* **Username:** `admin-devops-demo.digital.ai`
* **Password:** _ask in workshop_