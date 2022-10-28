
# Lab 3 - OIDC setup (use Identity service)

## Prepare Identity service

Participant runs ‘xl apply setup-oidc-template.yaml’ with manual steps
Participant runs template with instructions. (Mini wizard)
We provide credentials to log in to https://devops-demo.staging.digital.ai/
Participant creates OIDC client in Digital.ai Platform
Participant copies Client ID and Secret into Release
Release renders OIDC snippet
Participant uses OIDC snippet in wizard / answers file / yaml file.

## Upgrade with OIDC configuration

```shell
xl kube upgrade
```

- TODO:
  - answers
  - editor value for OIDC Identity service

For the other questions and answers details check [Upgrade Wizard for Digital.ai Release](https://docs.digital.ai/bundle/devops-release-version-v.22.3/page/release/operator/xl-op-upgrade-wizard-release.html)
