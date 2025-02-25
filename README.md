# argocd-homelab

This repository implements the [App Of Apps Pattern](https://argo-cd.readthedocs.io/en/stable/operator-manual/cluster-bootstrapping/#app-of-apps-pattern)
for a simple deployment via Argo CD.

## Features

- Self-managed Argo CD
- sealed-secrets for storing encrypted secrets in Git repository
- Distributed cluster storage using Longhorn
- Integration of NFS share for media storage
- Pre-configured monitoring setup with dashboards and alters

### Full list of applications

- [AdGuard](https://adguard.com/de/adguard-home/overview.html)
- [Argo CD](https://argoproj.github.io/cd/)
- [ddns-updater](https://github.com/qdm12/ddns-updater)
- [Gatus](https://gatus.io/)
- [Grafana](https://grafana.com/)
- [Jellyfin](https://jellyfin.org/)
- [Jellyseerr](https://github.com/Fallenbagel/jellyseerr)
- [Longhorn](https://longhorn.io/)
- [Navidrome](https://www.navidrome.org/)
- [Nextcloud](https://nextcloud.com/)
- [Prometheus](https://prometheus.io/)
- [Prowlarr](https://prowlarr.com/)
- [qBittorrent](https://www.qbittorrent.org/)
- [Radarr](https://radarr.video/)
- [SABnzbd](https://sabnzbd.org/)
- [sealed-secrets](https://github.com/bitnami-labs/sealed-secrets)
- [Sonarr](https://sonarr.tv/)
- [telegrammy](https://github.com/kymzky/telegrammy)
- [wg-easy](https://github.com/wg-easy/wg-easy)

## Usage

To see an example of how to create a suitable Kubernetes cluster and deploy
this repository in it, see [ansible-homelab](https://github.com/kymzky/ansible-homelab).

### Manual configurations

While most applications are preconfigured as far as it is reasonable, this does
not apply to the ARR stack. This means that Jellyfin, Jellyseerr, Navidrome,
Prowlarr, qBittorrent, Radarr, SABnzbd and Sonarr must be set up initially via
their respective web interfaces.

*Note: The initial password for qBittorrent can be found in the pod's logs.*

### Create a SealedSecret

To encrypt a secret using sealed-secrets, proceed as follows:

1. Create a YAML file defining the Secret with the desired key-value-pairs (see
template below)
2. Copy the output of `kubeseal --format yaml < <PATH_TO_SECRET_YAML>` to a
suitable location within this repository
3. Customize the desired application(s) to use the new SealedSecret
4. Push & Sync

#### Secret template

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
