# GitHub Actions Pets Workshop Rehearsal

- **Target repository:** <https://github.com/austenstone/pets-workshop-rehearsal-20260923>
- **Source template:** <https://github.com/austenstone/pets-workshop>
- **Authoritative source head:** `8630a64a2a739879f022ac69e1c49d80e61cad5f`
- **Started:** 2026-09-23 20:54 PDT
- **Execution mode:** Full Modules 00–13 rehearsal with real GitHub Actions, Azure deployment and teardown, repository governance, artifact attestation, deployment gates, concurrency, and Pages.

## Module results

| Module | Commit SHA | Run URL(s) | Expected outcome | Observed outcome | Duration | Status |
|---|---|---|---|---|---:|---|
| 00 Setup | `6e531c7` | N/A | Public template-generated repository matches the authoritative starter tree and contains no attendee-authored workflows. | Target is public and reports `austenstone/pets-workshop` as its template. Source and target recursive trees contain 166 entries with no differences. The initial commit contains no `.github/workflows/` directory. | 6 min | ✅ Pass |
| 01 Introduction | `5530ff2` | [Hello World](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35953477977) | Create and manually run Hello World. | Manual dispatch succeeded. The `greet` job printed the runner, repository, and actor context and completed in four seconds. | 2 min | ✅ Pass |
| 02 Code scanning | `51fd7f9` | [CodeQL](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35953509507) | Verify Dependabot, secret scanning/push protection, and enable CodeQL default setup. | Dependabot alerts and security updates were enabled; 33 open dependency alerts were indexed. Secret scanning and push protection were enabled with zero open secret alerts. CodeQL default setup analyzed Actions, JavaScript/TypeScript, and Python successfully. | 3 min | ✅ Pass |
| 03 Running tests | Pending | Pending | Unit and Playwright jobs run in parallel on push/PR. | Pending | Pending | ⏳ Pending |
| 04 Caching | Pending | Pending | Pip/npm caches miss, then hit on a subsequent run. | Pending | Pending | ⏳ Pending |
| 05 Matrix | Pending | Pending | Python 3.12, 3.13, and 3.14 matrix plus e2e all pass. | Pending | Pending | ⏳ Pending |
| 06 Azure deployment | Pending | Pending | CI-gated OIDC deployment succeeds, endpoint works, and all Azure resources/identity/RBAC are removed afterward. | Pending | Pending | ⏳ Pending |
| 07 Custom action | Pending | Pending | Composite action seeds one database path and all CI jobs pass. | Pending | Pending | ⏳ Pending |
| 08 Reusable workflows | Pending | Pending | Automated and manual callers use one reusable deployment workflow. | Pending | Pending | ⏳ Pending |
| 09 Rulesets | Pending | Pending | `main-gate` blocks merge until `tests-passed` succeeds without locking out the owner. | Pending | Pending | ⏳ Pending |
| 10 Artifact attestations | Pending | Pending | Real attestation is generated and downloaded artifact verifies with `gh`. | Pending | Pending | ⏳ Pending |
| 11 Environment gates | Pending | Pending | Production waits on a timer and proceeds without human review. | Pending | Pending | ⏳ Pending |
| 12 Concurrency | Pending | Pending | Stale preview run is canceled; all serialized production runs are retained. | Pending | Pending | ⏳ Pending |
| 13 Pages preview | Pending | Pending | Static preview publishes with correct data, routes, base path, and assets. | Pending | Pending | ⏳ Pending |

## Blemishes and issues

No blemishes observed yet.

## Evidence

- [`rehearsal-evidence/baseline.json`](rehearsal-evidence/baseline.json)
- [`rehearsal-evidence/security-settings.json`](rehearsal-evidence/security-settings.json)

## Azure teardown

Pending Module 06.

## Final audit

Pending.
