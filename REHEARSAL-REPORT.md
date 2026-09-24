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
| 06 Azure deployment | `3c2692a` | [CI gate](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963201398), [Container Apps deployment](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963285456) | CI-gated OIDC deployment succeeds, both client and API work, and all Azure resources/identity/RBAC are removed afterward. | `azd pipeline config` created an isolated federated service principal and the five repository variables. Green CI naturally triggered the reusable deployment for the exact tested SHA. `azd auth login --federated-credential-provider github` and `azd up` provisioned the full Astro/Flask Container Apps stack. The client, detail route, and API returned HTTP 200; the API returned all 100 dogs and the client server-rendered API data. `azd down --purge --force` then removed the resource group, and independent cleanup removed the app/SP/FICs/RBAC/variables. | 4m18s deploy; 15m43s teardown | ✅ Pass |
| 07 Custom action | `72391ae` | [composite-action CI](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35955771551) | Composite action seeds one database path and all CI jobs pass. | The composite action resolved an absolute database path, seeded it, and exposed the output to all three API matrix jobs and the Playwright job. All passed. | 4 min | ✅ Pass |
| 08 Reusable workflows | `3c2692a` | [caller-change CI](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35955894391), [real reusable deployment](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963285456) | Automated and manual callers use one reusable deployment workflow. | Automated and manual callers pass an explicit ref into one reusable workflow. The resumed Module 06 proof exercised that reusable workflow for real: it checked out the exact CI-tested SHA, authenticated `azd` by OIDC, and deployed both Container Apps successfully. | 4m18s live proof | ✅ Pass |
| 09 Rulesets | `c65f15a` | [summary check](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35956007531), [ruleset PR #12](https://github.com/austenstone/pets-workshop-rehearsal-20260923/pull/12), [PR CI](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35956199780) | `main-gate` blocks merge until `tests-passed` succeeds without locking out the owner. | Active rules require a PR, an up-to-date branch, and `tests-passed`. PR #12 showed the check pending, then became mergeable only after all CI completed. It was closed without merge. A repository-administrator bypass is retained for recovery and the explicitly authorized remaining direct checkpoints. | 8 min | ✅ Pass |
| 10 Artifact attestations | `97eb875` | [attestation run](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35958771845) | Real attestation is generated and downloaded artifact verifies with `gh`. | `tailspin-client.tgz` verified against the repository and signer workflow with SHA-256 `2bf6dabc…`; the Rekor timestamp and GitHub-hosted builder identity were present. A one-byte-modified copy was rejected. | 3 min | ✅ Pass |
| 11 Environment gates | `60c0be3` | [gated deployment](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35958870891) | Production waits on a timer and proceeds without human review. | Staging completed at 05:11:58Z. Production began at 05:13:02Z and succeeded after the configured one-minute environment wait, with no reviewer action. | 3 min | ✅ Pass |
| 12 Concurrency | `e9b897d` | [stale run](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959025620), [replacement](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959036830), [queue 1](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959333551), [queue 2](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959343024), [queue 3](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959345198) | Stale preview run is canceled; all serialized production runs are retained. | The stale preview run was canceled and its replacement succeeded. Three production runs all succeeded in strictly serialized intervals: 05:18:12–05:18:33Z, 05:18:36–05:18:58Z, and 05:19:01–05:19:25Z. | 6 min | ✅ Pass |
| 13 Pages preview | `ca57fe7` | [Pages deployment](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959471097), [live site](https://austenstone.github.io/pets-workshop-rehearsal-20260923/) | Static preview publishes with correct data, routes, base path, and assets. | The build exported 100 dogs. The homepage shows 12 available dogs, About and assets load under the repository base path, and all 100 `/dog/1/` through `/dog/100/` deep links returned HTTP 200. | 3 min | ✅ Pass |

## Blemishes and issues

| # | Classification | Module | Observation | Workaround | Source fix needed |
|---:|---|---|---|---|---|
| 1 | Attendee UX rough edge | 04 | The guide says the warm-cache run should be noticeably faster. The warm e2e job took 69 seconds versus 57 seconds cold because Playwright browsers are intentionally not cached and runner/network variance outweighed package-cache savings. Cache-hit logs, not total duration, are the reliable proof. | Compare setup-action logs for exact `Cache hit` and `Cache restored from key` lines. | Yes. Rephrase the timing claim as a possible rather than expected result. |
| 2 | Attendee prerequisite | 06 | The documented macOS `curl ... \| bash` azd installer targets `/opt/microsoft/azd` and `/usr/local/bin`, then requires interactive sudo. It fails in unattended environments. | Run the same installer with `--install-folder "$HOME/.local/bin"` and add that directory to `PATH`. | Yes. Document a user-local non-sudo installation option. |
| 3 | Tool/schema gap | 06 | `az deployment sub validate` crashed in the macOS Azure CLI .NET certificate chain with `System.AccessViolationException` before ARM validation. | Use local `az bicep build` and let the real `azd up` deployment perform authoritative ARM validation. | No template change; track as Azure CLI/macOS tooling. |
| 4 | Facilitator-only prerequisite | 06 | The first subscription allowed ordinary resource writes but excluded `Microsoft.Authorization/*/Write`, and its tenant blocked Entra app creation. The exact upstream path succeeded only after switching to an isolated subscription where the attendee had Owner, User Access Administrator, and permission to create Entra applications. | Preflight Entra app-registration and Azure role-assignment write privileges, not just a wildcard action entry. | Yes. State the effective permissions explicitly and provide a facilitator-prepared identity path for constrained tenants. |
| 5 | Platform/transient behavior | 06 | Local `azd package` built the client image, then the downloaded Oryx/pack client failed against the macOS Docker daemon because API 1.38 is below the daemon's minimum 1.40. | Treat GitHub-hosted deployment as authoritative; local Bicep compilation still validates IaC syntax. | No template change. |
| 6 | Source-guide defect | 06 | `zizmor` flags the privileged `workflow_run` trigger. The Module 06 standalone snippet uses checkout's default ref, so it can deploy the current default-branch head rather than the exact SHA that passed CI; it also persists checkout credentials. The later Module 08 caller does pass `workflow_run.head_sha`, but that is not the Module 06 snippet. | The successful rehearsal used the Module 08 reusable caller, exact-SHA checkout, `persist-credentials: false`, no inherited secrets, and a main-branch-only FIC. | Yes. Verify the workflow run came from this repository/default branch, checkout the tested SHA explicitly, and disable credential persistence. |
| 7 | Failed workaround | 06 | The App Service fallback took 976 seconds, diverged from the documented Container Apps/OIDC route, and produced a client HTTP 503 because the packaged site omitted `dist/server/entry.mjs`. | Stop at the documented capability gate and use a prepared successful deployment for follow-along attendees. | No. Do not document or preserve this fallback. |
| 8 | Source-guide defect | 01–02 | CodeQL reports `actions/missing-workflow-permissions` on the Module 01 Hello World workflow because it omits explicit top-level permissions. Module 02 does not tell attendees to expect this finding. | Treat it as a real least-privilege finding during the rehearsal. | Yes. Add `permissions: {}` to the Hello World workflow or explicitly teach the finding. |
| 9 | Source-code security finding | 02 | CodeQL reports high-severity `py/flask-debug` at `app/server/app.py:83` because the Flask entrypoint always uses `debug=True`. | Do not represent a green CodeQL run as zero findings; the analysis completed successfully with this alert open. | Yes. Disable debug by default and opt in only for local development. |
| 10 | Historical authorized divergence | 06 | Before a suitably privileged subscription was available, a bounded Static Web Apps fallback proved basic Azure deployment but not the full-stack exercise. It is not the final Module 06 acceptance result. | The fallback was torn down and superseded by the successful OIDC Container Apps run. | No. Preserve it only as rehearsal history. |
| 11 | Tool/action rough edge | 06 | `Azure/static-web-apps-deploy@v1` accepted the deployment but warned that `skip_api_build` is not a supported input. | The temporary workflow was removed after proof; the successful deployment did not depend on that input. | No source-template change because this action is used only by the bounded fallback. |
| 12 | Source-guide defect | 06 | Current `azd` 1.34.2 prompts differ from the guide. The federated-auth prompt defaults to managed identity, not service principal, and a second undocumented prompt asks whether to use detected branch/main/PR OIDC subjects. | Explicitly choose **Federated Service Principal (SP + OIDC)**, then review and choose the minimum required detected subjects. | Yes. Update the prompt table and warn attendees not to accept the first auth option blindly. |
| 13 | Attendee UX rough edge | 06 | `azd pipeline config` printed “GitHub Action secrets are now configured,” but this OIDC path created five repository variables and zero secrets. | Verify the Variables tab or `gh variable list`; do not look for a client secret. | Yes. Clarify that OIDC uses variables only and the CLI's generic “secrets” message is misleading. |
| 14 | Source-guide defect | 06 | The module has no cleanup section. `azd down` removes application resources but does not remove the app registration, enterprise service principal, federated credentials, subscription role assignments, or repository variables created by `azd pipeline config`. | Capture identity IDs, run `azd down --purge --force`, delete RBAC/FIC/SP/app and the five variables, then verify zero state. | Yes. Add complete cleanup and verification commands. |
| 15 | Attendee prerequisite | 06 | The guide assumes a writable GitHub remote and authenticated repository administration. `azd pipeline config` needs to discover the remote and write Actions variables; the workshop does not ask attendees to verify `gh auth status`, repository admin access, provider registration rights, regional Container Apps availability, or quota. | Add a preflight for GitHub auth/remote, Azure subscription/tenant, required resource providers, region support, and quota. | Yes. Fail fast before the expensive deployment step. |
| 16 | Environment-specific divergence | 06–08 | The successful proof resumed after Module 08, so `.github/workflows/azure-dev.yml` was already a reusable-workflow caller rather than the standalone Module 06 workflow. | Preserve the Module 08 architecture and put the upstream direct `azd auth login` flow inside the reusable job. | No source defect in Module 06, but the rehearsal result must not be represented as a byte-for-byte replay of the standalone snippet. |

## Module 06 attendee-instructions audit

**Did the module work exactly as written from its stated starting point? No.** The upstream behavior worked, but the successful run required undocumented permissions, current-CLI prompt choices, explicit environment selection, and cleanup. It also ran from the later Module 08 reusable-workflow state rather than recreating the standalone Module 06 file.

### Exact successful sequence

1. Verified GitHub authentication and a writable `origin`, then used an isolated Azure CLI profile signed in as `austenstone@live.com`.
2. Confirmed subscription `2d63aa20-c132-4d37-b9ac-d1852d10ba55`, tenant `a8d1c4d4-8a92-46c2-b538-1e07cd7e7173`, Owner at subscription scope, User Access Administrator at `/`, and Entra `allowedToCreateApps=true`.
3. Reused `azd` 1.34.2 installed without sudo at `$HOME/.local/bin/azd` (`curl -fsSL https://aka.ms/install-azd.sh | bash -s -- --install-folder "$HOME/.local/bin"`), used isolated local state, and ran plain `azd auth login`. On macOS this opened the existing browser session; it did not show the guide's device-code flow.
4. Reused the previously generated `azure.yaml`, `infra/main.bicep`, `infra/main.parameters.json`, `infra/resources.bicep`, `infra/abbreviations.json`, and `infra/modules/fetch-container-image.bicep`. The required `API_SERVER_URL` entry was already present.
5. Created the environment explicitly:

   ```bash
   azd env new astone-pets-oidc-20260923 \
     --subscription 2d63aa20-c132-4d37-b9ac-d1852d10ba55 \
     --location eastus2 \
     --no-prompt
   ```

6. Configured the pipeline explicitly:

   ```bash
   azd pipeline config \
     -e astone-pets-oidc-20260923 \
     --provider github \
     --auth-type federated \
     --principal-name github-actions-astone-pets-oidc-20260923 \
     --remote-name origin
   ```

7. At the current prompts, selected **Federated Service Principal (SP + OIDC)** rather than the default managed identity, then **Use detected subjects (Recommended)**. `azd` created FICs for `main`, the current worktree branch, and pull requests; Contributor and User Access Administrator assignments; and `AZURE_CLIENT_ID`, `AZURE_ENV_NAME`, `AZURE_LOCATION`, `AZURE_SUBSCRIPTION_ID`, and `AZURE_TENANT_ID`.
8. Answered **No** to `azd`'s automatic commit/push prompt so the rehearsal could preserve the required commit trailer and expected-head guard. Committed `3c2692a`, pushed it to `main`, and re-enabled the deployment workflow that had been disabled after the earlier blocked attempt. A fresh attendee on `main` can answer **Yes**, but the guide should explain exactly what will be committed and pushed.
9. Push-triggered CI run [35963201398](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963201398) passed and naturally started deployment run [35963285456](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963285456). The reusable job checked out exact SHA `3c2692a390940f4092018faf024c54a526c2b4e5`, installed `Azure/setup-azd@v2`, authenticated with GitHub OIDC, and completed `azd up --no-prompt` on the first attempt in 4m4s; the job took 4m18s. No repository variable was changed manually and no provider-registration command was required.
10. Verified the client root and `/dog/1` at HTTP 200, six server-rendered dog cards, `Page 1 of 17`, and `Pepper` on both routes. The Flask API returned HTTP 200 and all 100 dogs. The successful endpoints were `client.redcoast-90301e2b.eastus2.azurecontainerapps.io` and `server.redcoast-90301e2b.eastus2.azurecontainerapps.io`.
11. Ran `azd down -e astone-pets-oidc-20260923 --purge --force --no-prompt`. Resource-group deletion took 15m43s, mostly waiting for the Container Apps managed environment. Deleted the two role assignments, three FICs, SP, app registration, five repository variables, and task-created local `azd` state; re-disabled the deployment workflow; and independently verified zero remaining resources or credentials.

### Required source corrections

Add a permission/authentication preflight before installation:

```bash
git remote get-url origin
gh auth status
azd auth login
```

State that the deploying person needs permission to create an Entra application and service principal, assign Contributor and User Access Administrator (or narrowly equivalent roles) at the deployment scope, write repository Actions variables, register required providers, and deploy Container Apps/ACR in the selected region. Owner plus User Access Administrator and `allowedToCreateApps=true` was the verified working combination; ordinary Contributor was not enough.

Update the workflow to bind deployment to the trusted CI-tested commit and avoid persisting Git credentials:

```yaml
if: >-
  github.event_name == 'workflow_dispatch' ||
  (github.event.workflow_run.conclusion == 'success' &&
   github.event.workflow_run.head_repository.full_name == github.repository &&
   github.event.workflow_run.head_branch == github.event.repository.default_branch)
steps:
  - uses: actions/checkout@v7
    with:
      ref: ${{ github.event_name == 'workflow_run' && github.event.workflow_run.head_sha || github.sha }}
      persist-credentials: false
```

Document the current `azd pipeline config` prompts: choose **GitHub**, a unique short environment name, the intended subscription and supported region, **Federated Service Principal (SP + OIDC)**, and review the detected FIC subjects rather than accepting unnecessary branch/PR trust. The OIDC path creates variables, not a client secret.

Verify both halves of the deployment, not only the client URL:

```bash
azd show
curl --fail --show-error --location "$CLIENT_URL/"
curl --fail --show-error --location "$CLIENT_URL/dog/1"
curl --fail --show-error --location "$SERVER_URL/api/dogs?per_page=100"
```

Add an explicit cleanup section. Capture the client/app and object IDs before deleting variables, run `azd down --purge --force`, delete the service principal's role assignments, FICs, service principal, app registration, and all five `AZURE_*` repository variables, then verify the resource group, tagged resources, app/SP, role assignments, variables, and secrets are all absent. `azd down` alone is not sufficient.

## Evidence

- [`rehearsal-evidence/baseline.json`](rehearsal-evidence/baseline.json)
- [`rehearsal-evidence/security-settings.json`](rehearsal-evidence/security-settings.json)
- [`rehearsal-evidence/azure-module.json`](rehearsal-evidence/azure-module.json)
- [`rehearsal-evidence/ruleset.json`](rehearsal-evidence/ruleset.json)
- [`rehearsal-evidence/bonus-modules.json`](rehearsal-evidence/bonus-modules.json)

## Azure teardown

The exact upstream proof created resource group `rg-astone-pets-oidc-20260923`, app/SP `github-actions-astone-pets-oidc-20260923`, three FICs, two subscription role assignments, and five repository variables. `azd down --purge --force` removed the resource group in 15m43s. The role assignments, FICs, SP, app, variables, and task-created local `azd` state were then removed explicitly. Independent checks at 2026-09-24T06:33:33Z returned: resource group `false`; tagged resources `0`; app registrations `0`; service principals `0`; role assignments `0`; repository variables `0`; repository secrets `0`. The deployment workflow is disabled after proof. Earlier temporary identity, App Service, and Static Web Apps resource groups also remain absent. No shared/pre-existing Azure resource or identity was modified.

## Final audit

- Initial template commit `865657f` is manifest-identical to source `8630a64`: 166 entries and SHA-256 `77c6034ed26bd75eec7bbcb02f3bd2ef7b19a79c396cb91dc10816e430b694c0`.
- Module checkpoints are preserved as 16 commits through `ca57fe7`; the final delta contains only expected workflows, the composite action, Azure scaffold, evidence, and this report.
- The original rehearsal's 45 workflow runs were accounted for: 44 succeeded and one was intentionally canceled by the Module 12 stale-preview policy. The final upstream Module 06 proof added green CI run [35963201398](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963201398) and green full-stack Container Apps run [35963285456](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35963285456). The earlier bounded Static Web Apps run [35961396349](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35961396349) remains historical evidence only.
- Final module-checkpoint CI run [35959463424](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959463424) and CodeQL run [35959463543](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35959463543) succeeded at `ca57fe7`. The evidence-only finalization commit `0384e0e` also passed [CI](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35960107086) and [CodeQL](https://github.com/austenstone/pets-workshop-rehearsal-20260923/actions/runs/35960106971).
- The active `main-gate` ruleset requires a pull request, strict `tests-passed`, and non-fast-forward protection on the default branch, with an administrator recovery bypass.
- Dependabot security updates, secret scanning, and push protection are enabled. There are 33 open Dependabot alerts, zero secret-scanning alerts, and the two CodeQL findings documented above.
- Eleven open pull requests and their branches are expected Dependabot bot updates. The ruleset test PR #12 is closed and its branch was deleted.
- Environments remain inspectable: `staging`, `production` with a one-minute wait timer, and `github-pages`.
- The final Module 06 client, detail, and API endpoints were intentionally removed after their HTTP status, content checks, and SHA-256 evidence were captured in `azure-module.json`.
- The rehearsal repository and live Pages preview are intentionally retained for review.
