# research-redis-gitops

Declarative Kubernetes desired state (Kustomize bases + `kind` overlays) reconciled by **ArgoCD** for [`eka-research/research-redis`](https://github.com/eka-research/research-redis).

## Layout

| Path | Component | Sync Wave |
|------|-----------|-----------|
| `bootstrap/argocd` | ArgoCD v3.5.6 + `argocd-cm` health & reconciliation patches | bootstrap |
| `clusters/kind/root-app.yaml` | Root App-of-Apps | bootstrap |
| `clusters/kind/apps/` | `AppProject` (`-10`) + child `Application` manifests | `-3` to `0` |
| `platform/gateway/controller` | Envoy Gateway v1.9.3 + Gateway API CRDs | `-3` |
| `platform/gateway/config` | `EnvoyProxy` (NodePort 30080), `GatewayClass`, `Gateway` | `-2` |
| `platform/data-namespace` | `eka-data` namespace + zero-trust `NetworkPolicy` | `-2` |
| `platform/redis/overlays/kind` | Redis 8.8.4 StatefulSet + AOF config | `-1` |
| `platform/mysql/overlays/kind` | MySQL 8.4.9 LTS StatefulSet | `-1` |
| `platform/elasticsearch/overlays/kind` | Elasticsearch 9.5.5 single-node StatefulSet | `-1` |
| `apps/api/overlays/kind` | Migration Job (`Sync` hook wave `0`), API Deployment, Service, `HTTPRoute`, `PDB`, `NetworkPolicy` | `0` |

> **Security notice:** `overlays/kind` contains dev-only credentials for an ephemeral local `kind` cluster. Never reuse in shared or production environments.
