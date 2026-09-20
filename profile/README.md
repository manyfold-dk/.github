<img src="manyfold-logo-white.png" alt="manyfold" >

Manyfold is the platform engineering practice of [Thomas Berg von Linde](https://github.com/tbvl) in Copenhagen: consulting on Kubernetes platforms, and a small production platform run on European infrastructure for companies that want their systems looked after.

## The platform

| Layer | What runs there |
| --- | --- |
| Edge | Cloudflare DNS with a failover zone |
| Identity and secrets | Keycloak · OpenBao |
| Delivery | Argo CD GitOps · Tekton · GitHub Actions |
| Observability | Prometheus · Loki · Tempo · Grafana |
| Tenants | Isolated per company, provisioned with Crossplane |
| Network | Cilium · Hubble |
| Cluster | Kubernetes on Talos Linux · Velero backups |
| Infrastructure | Hetzner Cloud, Helsinki |

Applications and data are hosted in the EU. Everything is defined in code and delivered through Git, significant decisions are recorded as architecture decision records, and AI agents handle first-line operations around the clock.

## Why the repositories are private

They hold the running configuration of a production platform and its tenants. The live status, the architecture and selected decision records are public at [manyfold.dk](https://manyfold.dk).

## Contact

[manyfold.dk](https://manyfold.dk) · thomas@manyfold.dk
