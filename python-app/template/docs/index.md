# Documents for ${{values.app_name}}

This is a template app!

For fun, it shows you a random dev excuse + a random cat

# How to access the app?

You can access the app by accessing this URL: `${{values.app_name}}-${{values.app_env}}.gabrieleweka.dev`

# Extra info?

This application has two API endpoints:

- `/api/v1/info`
- `/api/v1/healthz`

Here you could expand on what each of these endpoints do.

---

# Links for THIS app

| What | URL |
|---|---|
| **Live app**         | https://${{values.app_name}}-${{values.app_env}}.gabrieleweka.dev |
| **Source code**      | https://github.com/octal-engine/${{values.app_name}} |
| **CI / CD pipeline** | https://github.com/octal-engine/${{values.app_name}}/actions |
| **ArgoCD app**       | https://argocd.gabrieleweka.dev/applications/${{values.app_name}} |
| **Grafana dashboard** (per-app metrics) | https://grafana.gabrieleweka.dev/d/app-${{values.app_name}}-${{values.app_env}} |
| **SonarQube project** (code quality) | https://sonar.gabrieleweka.dev/dashboard?id=${{values.app_name}} |
| **Docker image** | `eweka/${{values.app_name}}:<commit-sha>` (public on Docker Hub) |

---

# Platform-wide URLs & Credentials

All infrastructure is publicly reachable. Credentials below are read-only (safe to share). For admin access, ping **@Eweka01**.

| Service | URL | Auth |
|---|---|---|
| **Backstage** (this portal) | https://backstage.gabrieleweka.dev | Sign in with GitHub or Google |
| **ArgoCD** | https://argocd.gabrieleweka.dev | `viewer` / `viewer123` (read-only) |
| **Grafana** | http://grafana.gabrieleweka.dev | No login needed (anonymous Viewer) |
| **Prometheus** | http://prometheus.gabrieleweka.dev | No auth |
| **SonarQube** | https://sonar.gabrieleweka.dev | No login needed for browsing public projects |

## ArgoCD read-only scope

The `viewer` account can **see** applications, repos, projects, and pod logs, but **cannot create, sync, or delete** anything. Safe to share for demos.

## SonarQube read-only scope

The `viewer` / `viewer123` account is in the `sonar-users` group with browse permission only — can see scan results, issues, and quality gates across all scaffolded projects, but cannot trigger scans, edit settings, or delete projects.

## Why no Nexus on this demo

A real production platform would put **Sonatype Nexus** in front of Docker Hub and PyPI to:

- **Cache** upstream artifacts so builds keep working when external registries flap.
- **Scan** every artifact on the way in (Nexus IQ for vulns + license).
- **Air-gap support** — your runners pull only from Nexus, never directly from the internet.
- **Promotion workflows** — `dev` repo → `staging` repo → `prod` repo with policy gates between.

It's deliberately omitted on this single-laptop demo because Nexus 3 needs ~1 GB JVM heap and would starve the kind cluster of RAM. The same Helm values would deploy it on any real cluster — pattern travels, machine doesn't.
