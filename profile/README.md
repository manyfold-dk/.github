<img src="manyfold-logo-white.png" alt="manyfold" >

Manyfold is the platform engineering practice of [Thomas Berg von Linde](https://github.com/tbvl) in Copenhagen: consulting on Kubernetes platforms, and a small production platform run on European infrastructure for companies that want their systems looked after.

## The platform

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="diagrams/platform-dark.svg">
  <img src="diagrams/platform-light.svg" alt="The platform: Git holds the desired state and Argo CD applies it to a four-layer stack (tenants, network, cluster, infrastructure on Hetzner Cloud in Helsinki). Operations agents answer alerts within a policy and ask the operator for anything else. Backups, break-glass access and a failover DNS zone sit outside the platform.">
</picture>

Applications and data are hosted in the EU. Everything is defined in code and delivered through Git, significant decisions are recorded as architecture decision records, and AI agents handle first-line operations around the clock.

## How it is run

**Git is the only way onto the cluster.** Every change is a commit on main, usually through a pull request. CI builds, tests and scans it for secrets, pushes a versioned image and writes the new image tag back to Git. Argo CD applies what Git says and reverts anything that drifts from it.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="diagrams/delivery-dark.svg">
  <img src="diagrams/delivery-light.svg" alt="Delivery: a change on main runs the checks, CI builds and pushes a versioned image and commits its tag to Git, Argo CD applies Git to the cluster and reverts drift, and the cluster pulls the image from the registry.">
</picture>

**Agents take the routine; policy decides what they may do.** An alert goes to an operations agent that gathers read-only evidence. Remediation runs only through executor scripts that enforce a severity × risk matrix, so a prompt cannot widen what the agent is allowed to do. Anything outside the matrix becomes an approval request. The agent runs outside the cluster, so it still works when the cluster is what is broken. The reasoning is in [decision record 0004](https://manyfold.dk/decisions/0004-single-ops-agent-with-constrained-execution).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="diagrams/operations-dark.svg">
  <img src="diagrams/operations-light.svg" alt="Operations: an alert goes to triage, then to a policy gate in the executor. Low-severity, low-risk actions run within their class; anything else becomes an approval card for the operator. Every invocation, approval and outcome is appended to an audit log.">
</picture>

**Tenants share the platform, not each other's boundaries.** Each company gets a landing zone that the operator owns: delivery scope, namespaces and quota, a default-deny network, its own identity realm, admission policies and scoped secrets. Workloads come from the tenant's own repository, so a tenant can ship what it likes and still cannot widen its own quota, access or network policy.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="diagrams/tenancy-dark.svg">
  <img src="diagrams/tenancy-light.svg" alt="Tenancy: two tenants share the platform services; each has an operator-owned landing zone of delivery scope, namespaces and quota, default-deny network, own identity realm, admission policies and scoped secrets, around workloads shipped from the tenant's own repository.">
</picture>

## What is public, and what is not

The repositories that hold the running configuration of the production platform and its tenants are private. Tooling that is useful outside this estate is published: [estate-baseline](https://github.com/manyfold-dk/estate-baseline) — drift checks and handoffs for a set of repositories run to one standard, by people and by coding agents (Apache-2.0). The live status, the architecture and selected decision records are at [manyfold.dk](https://manyfold.dk).

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="diagrams/estate-dark.svg">
  <img src="diagrams/estate-light.svg" alt="The estate: the public estate-baseline holds the standard, a private overlay pins it and adds the values, and every repository pulls both. Checks fail rather than warn: a secret scan on every commit, a weekly conformance run over every repository, a publication gate before every public push, and the same vendored rules for every coding agent session.">
</picture>

## Contact

[manyfold.dk](https://manyfold.dk) · thomas@manyfold.dk
