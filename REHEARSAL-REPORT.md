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
| 03 Running tests | `1c8b1d8` | [unit-only](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35953647393), [parallel CI](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35953795494) | Unit and Playwright jobs run in parallel on push/PR. | Unit checkpoint passed in 15 seconds. The completed workflow then ran `test-api` and `test-e2e` concurrently; both passed, with the e2e job completing in 55 seconds. | 5 min | ✅ Pass |
| 04 Caching | `f429be6` | [cold cache](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35953903515), [warm cache](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35953993863) | Pip/npm caches miss, then hit on a subsequent run. | The push run logged missing pip/npm caches and saved both keys. The manual rerun restored pip in both jobs and npm in e2e from the exact primary keys. Both runs passed. | 4 min | ✅ Pass |
| 05 Matrix | `de3d157` | [matrix CI](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35954116225) | Python 3.12, 3.13, and 3.14 matrix plus e2e all pass. | Four jobs ran: the three Python versions passed independently with `fail-fast: false`, and e2e passed. | 3 min | ✅ Pass |
| 06 Azure deployment | Pending next checkpoint | Not run | CI-gated OIDC deployment succeeds, endpoint works, and all Azure resources/identity/RBAC are removed afterward. | `azd init --from-code` detected both services, generated `azure.yaml`/Bicep, and local Bicep compilation succeeded. Runtime is blocked: the user lacks Entra app-registration privilege and effective `Microsoft.Authorization/roleAssignments/write`. The safe existing demo UAMI has Contributor only, which cannot create the generated ACR pull assignments. The correct OIDC workflow is retained but disabled to prevent false failing runs. A temporary identity/RG created while testing the permission boundary had zero assignments and was fully deleted. | 22 min | 🛑 Blocked |
| 07 Custom action | Pending | Pending | Composite action seeds one database path and all CI jobs pass. | Pending | Pending | ⏳ Pending |
| 08 Reusable workflows | Pending | Pending | Automated and manual callers use one reusable deployment workflow. | Pending | Pending | ⏳ Pending |
| 09 Rulesets | Pending | Pending | `main-gate` blocks merge until `tests-passed` succeeds without locking out the owner. | Pending | Pending | ⏳ Pending |
| 10 Artifact attestations | Pending | Pending | Real attestation is generated and downloaded artifact verifies with `gh`. | Pending | Pending | ⏳ Pending |
| 11 Environment gates | Pending | Pending | Production waits on a timer and proceeds without human review. | Pending | Pending | ⏳ Pending |
| 12 Concurrency | Pending | Pending | Stale preview run is canceled; all serialized production runs are retained. | Pending | Pending | ⏳ Pending |
| 13 Pages preview | Pending | Pending | Static preview publishes with correct data, routes, base path, and assets. | Pending | Pending | ⏳ Pending |

## Blemishes and issues

| # | Classification | Module | Observation | Workaround | Source fix needed |
|---:|---|---|---|---|---|
| 1 | Attendee UX rough edge | 04 | The guide says the warm-cache run should be noticeably faster. The warm e2e job took 69 seconds versus 57 seconds cold because Playwright browsers are intentionally not cached and runner/network variance outweighed package-cache savings. Cache-hit logs, not total duration, are the reliable proof. | Compare setup-action logs for exact `Cache hit` and `Cache restored from key` lines. | Yes. Rephrase the timing claim as a possible rather than expected result. |
| 2 | Facilitator-only prerequisite | 06 | The documented macOS `curl ... \| bash` azd installer targets `/opt/microsoft/azd` and `/usr/local/bin`, then requires interactive sudo. It fails in unattended environments. | Run the same installer with `--install-folder "$HOME/.local/share/azd" --symlink-folder "$HOME/.local/bin"`. | Yes. Document a user-local non-sudo installation option. |
| 3 | Tool/schema gap | 06 | `az deployment sub validate` crashed in the macOS Azure CLI .NET certificate chain with `System.AccessViolationException` before ARM validation. | Use local `az bicep build` and let the real `azd up` deployment perform authoritative ARM validation. | No template change; track as Azure CLI/macOS tooling. |
| 4 | Facilitator-only prerequisite | 06 | The verified subscription grant allows ordinary resource writes but excludes `Microsoft.Authorization/*/Write`; the user also cannot create Entra applications. `azd pipeline config` therefore cannot create its OIDC principal and role assignments. | Preflight both Entra app-registration and Azure role-assignment write privileges, not just a wildcard action entry. | Yes. Make the permission probe account for `notActions` and explicitly test Entra app creation rights. |
| 5 | Platform/transient behavior | 06 | Local `azd package` built the client image, then the downloaded Oryx/pack client failed against the macOS Docker daemon because API 1.38 is below the daemon's minimum 1.40. | Treat GitHub-hosted deployment as authoritative; local Bicep compilation still validates IaC syntax. | No template change. |
| 6 | Source-guide defect | 06 | `zizmor` flags the privileged `workflow_run` deployment because it checks out `workflow_run.head_sha` while holding `id-token: write`. A successful PR-originated CI run may cross an untrusted-to-privileged boundary. It also leaves checkout credentials persisted. | Rehearsal retains the attendee snippet exactly and keeps the blocked workflow disabled. | Yes. Gate trusted push runs explicitly and set `persist-credentials: false`. |

## Evidence

- [`rehearsal-evidence/baseline.json`](rehearsal-evidence/baseline.json)
- [`rehearsal-evidence/security-settings.json`](rehearsal-evidence/security-settings.json)
- [`rehearsal-evidence/azure-module.json`](rehearsal-evidence/azure-module.json)

## Azure teardown

The temporary `rg-astone-pets-rehearsal-identity` resource group and `pets-workshop-rehearsal-oidc` identity were deleted after the attempted role assignment failed. The group no longer exists. No rehearsal role assignment, Entra app, shared-identity federated credential, deployment resource, repository Azure variable, or secret remains.

## Final audit

Pending.
