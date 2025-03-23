# argocd-homelab

This repository implements the [app of apps pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern)
for a simple deployment via Argo CD.

## Features

- Self-managed Argo CD
- sealed-secrets for storing encrypted secrets in Git repository
- Distributed cluster storage using Longhorn
- Integration of NFS share for media storage
- Pre-configured monitoring setup with dashboards and alerts

### Full list of applications

- [AdGuard](https://adguard.com/de/adguard-home/overview.html)
- [Argo CD](https://argoproj.github.io/cd/)
- [ddns-updater](https://github.com/qdm12/ddns-updater)
- [Gatus](https://gatus.io/)
- [Grafana](https://grafana.com/)
- [Home Assistant](https://www.home-assistant.io/)
- [Longhorn](https://longhorn.io/)
- [Navidrome](https://www.navidrome.org/)
- [Nextcloud](https://nextcloud.com/)
- [Prometheus](https://prometheus.io/)
- [sealed-secrets](https://github.com/bitnami-labs/sealed-secrets)
- [telegrammy](https://github.com/kymzky/telegrammy)
- [wg-easy](https://github.com/wg-easy/wg-easy)

## Usage

To see an example of how to create a suitable Kubernetes cluster and deploy
this repository in it, see [ansible-homelab](https://github.com/kymzky/ansible-homelab).

### Add new application

To deploy a new application into the cluster follow these steps:

1. [Create SealedSecrets](#create-a-sealedsecret) for any sensitive data
2. Set environment-specific parameters in value files ([multipass](./apps/values-multipass.yaml)/[prod](./apps/values-prod.yaml))
3. Add the endpoints to be monitored to the [Gatus configuration](./apps/templates/gatus.yaml)
4. If you are deploying to a development environment without DNS add necessary
   entries to your /etc/hosts file
5. Write the required YAML files for the new application in the [templates](./apps/templates/)
   directory
6. Commit and push changes to trigger Argo CD to deploy the application
7. Check if Renovate's [Dependency Dashboard](https://github.com/kymzky/argocd-homelab/issues/2)
   has correctly identified all dependencies and versions (customize [renovate.json](./renovate.json)
   if not)
8. Update [README.md](./README.md)

#### Create a SealedSecret

To encrypt a secret using sealed-secrets, proceed as follows:

1. Create a YAML file defining the Secret with the desired key-value-pairs (see
   template below)
2. Copy the output of `kubeseal --format yaml < <PATH_TO_SECRET_YAML>` to a
   suitable location within this repository
3. Customize the desired application(s) to use the new SealedSecret
4. Push & Sync

##### Secret template

```yaml
---
apiVersion: v1
kind: Secret
metadata:
  name: <SECRET_NAME>
  namespace: <NAMESPACE>
type: Opaque
stringData:
  <KEY_1>: <VALUE_1>
  <KEY_2>: <VALUE_2>
```
