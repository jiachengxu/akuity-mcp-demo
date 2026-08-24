# akuity-mcp-demo

nginx deployed to three environments on one cluster, promoted through them by Kargo.
Everything is driven through the Akuity platform MCP endpoint.

## Layout

- `envs/{dev,staging,prod}/` — deployment YAMLs per environment. Kargo bumps the
  image tag here on each promotion; nothing else writes to these files.
- `bootstrap/argocd/` — one Argo CD `Application` per environment (deploying to
  cluster `demo-mcp`, namespace `demo-<env>`), plus the `kargo-pipeline` Application
  that syncs `bootstrap/kargo/` to the Kargo control plane (cluster `kargo-ctl`).
- `bootstrap/kargo/` — the Kargo `Project` (`demo`), a `Warehouse` watching the
  `nginx` image, and one `Stage` per environment chained `dev -> staging -> prod`.
  Managed by Argo CD via the `kargo-pipeline` Application: change these by commit,
  not by direct apply.

Credentials (the Kargo git push credential) are never stored in this repo.
