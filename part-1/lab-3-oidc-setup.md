
# Lab 3 - OIDC setup (use Identity service)

## Prepare Identity service

Participant runs ‘xl apply setup-oidc-template.yaml’ with manual steps
Participant runs template with instructions. (Mini wizard)
We provide credentials to log in to https://devops-demo.staging.digital.ai/
Participant creates OIDC client in Digital.ai Platform
Participant copies Client ID and Secret into Release
Release renders OIDC snippet
Participant uses OIDC snippet in wizard / answers file / yaml file.

## Configure authentication on Digital.ai Platform

In this step, we will create an OIDC client that will allow Digital.ai Release to use the authentication service of igtial.ai Platform

### 1. Log in to Digital.ai Platform

Got to Digital.ai Platform at [https://devops-demo.staging.digital.ai/](https://devops-demo.staging.digital.ai/). 

Use the following credentials:

* **Username:** `admin-devops-demo.digital.ai`  
* **Password:** _ask in workshop_

### Create the OIDC client

Go to **Clients** and press the **Add OIDC Client** button.

1. Give the client a name, for example 'MyName-Release'. Note: choose a unique name.
2. Scroll down to **Valid Redirect URIs** and add `${releaseUrl}/oidc-login`. This should be the valid URL that points to your Release installation, followed by `/oidc-login`.

Create the client and immediately edit the client. Copy the following values from the **Credentials** section into a text file
  
* **Client ID**
* **Client Secret**

Now fudge them into this snippet:

```
external: true
clientId: "[[YOUR CLIENT ID HERE]]"
clientSecret: "[[YOUR CLIENT SECRET HERE]]"
issuer: "https://identity.staging.digital.ai/auth/realms/devops-demo"
redirectUri: "https://devops-demo.devops.digital.ai/oidc-login"
postLogoutRedirectUri: "https://devops-demo.devops.digital.ai/oidc-login"
rolesClaim: groups
userNameClaim: preferred_username
scopes: ["openid"]
```

or 

```
oidc:
  enabled: true
  clientId: "[[YOUR CLIENT ID HERE]]"
  clientSecret: "[[YOUR CLIENT SECRET HERE]]"
  clientSecret: 0F9VGMRhK4G3Vzzj3ImJjuUFN2GFpVin
  issuer: "https://identity.staging.digital.ai/auth/realms/devops-demo"
  redirectUri: "https://devops-demo.devops.digital.ai/oidc-login"
  postLogoutRedirectUri: "https://devops-demo.devops.digital.ai/oidc-login"
  rolesClaim: groups
  userNameClaim: preferred_username
  scopes: ["openid"]
```

## Upgrade with OIDC configuration

```shell
xl kube upgrade
```

- TODO:
  - answers
  - editor value for OIDC Identity service

For the other questions and answers details check [Upgrade Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-upgrade-wizard-release.html)
