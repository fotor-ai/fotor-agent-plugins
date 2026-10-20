<a id="install-and-use-fotor-for-dsh"></a>

# Install and use Fotor for DeepSeek Harness

DeepSeek Harness is abbreviated as `dsh` in commands and native identifiers.

**Fotor does not install mcp-remote automatically.** You, or an Agent acting on your installation request, install the pinned bridge in a separate directory. After configuration, DeepSeek Harness starts that existing bridge. Bundle installation, skill loading, Fotor authorization and website login are separate steps.

This guide travels with the complete Bundle. Start here for a first installation, a missing bridge, an unconfigured path or missing skills. For an already configured connection, use [authentication and recovery](references/authentication.md). Keep one Fotor installation per selected profile.

## 1. Check prerequisites and choose locations

The supported preparation baseline is **Linux, DeepSeek Harness Web `0.2.1-rc.2`, Node `24.17.0`, mcp-remote `0.15.1`**. DeepSeek Harness must already run, with its native package manager available. Git-source installation also requires Git. Use the [official DeepSeek Harness quickstart](https://deepseek-harness.github.io/deepseek-harness/en/guide/quickstart) and [Node downloads](https://nodejs.org/en/download/) to prepare a missing runtime separately; preserve the user's daily installation. A different version needs its own compatibility check.

Set these shell variables once, replacing every `/absolute/path/...` value. They are installation parameters, not credentials:

```bash
FOTOR_NODE=/absolute/path/to/node
FOTOR_NPM_CLI=/absolute/path/to/npm-cli.js
FOTOR_SOURCE=/absolute/path/to/complete/fotor
FOTOR_BRIDGE=/absolute/path/to/fotor-bridge-0.15.1
FOTOR_PROFILE=web
FOTOR_PATCH=/absolute/path/to/fotor.patch.yml
FOTOR_AUTH=/absolute/path/to/fotor-authorization
FOTOR_WORKSPACE=/absolute/path/to/workspace
```

| Parameter | How to choose it |
| --- | --- |
| `FOTOR_NODE` | Absolute executable from the selected Node installation. Resolve a version-manager shim to its real executable. |
| `FOTOR_NPM_CLI` | Absolute npm JavaScript CLI entry, usually `lib/node_modules/npm/bin/npm-cli.js` under an existing npm installation. Verify it runs under `FOTOR_NODE`; an npm shell wrapper is not this entry. |
| `FOTOR_SOURCE` | Complete DeepSeek Harness package directory acquired in step 2, containing this guide and `package.json`. |
| `FOTOR_BRIDGE` | New persistent directory outside the Bundle and any source checkout; only the bridge and its install inputs live here. |
| `FOTOR_PROFILE` | Existing intended DeepSeek Harness profile; `web` is the accepted baseline. Use this same profile for all native commands. |
| `FOTOR_PATCH` | New external override file, retained across package updates. Its parent directory must exist. |
| `FOTOR_AUTH` | Fotor-only bridge credential directory outside the Bundle. Keep the same directory through updates and ordinary recovery. |
| `FOTOR_WORKSPACE` | Existing absolute working directory for this DeepSeek Harness session and its MCP process. |

For a rehearsal, first select dedicated `DSH_HOME`, `DSH_AGENTS_HOME` and working directories, and keep them set in the same shell for installation, configuration checks and launch. For an existing installation, retain the user's intended locations instead. Do not replace an existing Fotor registration or override without resolving the conflict.

On this baseline, unset home overrides default to `~/.dsh` and `~/.agents`. Resolve them to absolute paths for the YAML. For an isolated rehearsal, use new locations instead:

```bash
export DSH_HOME=/absolute/path/to/rehearsal/dsh-home
export DSH_AGENTS_HOME=/absolute/path/to/rehearsal/agents-home
mkdir -p "$DSH_HOME" "$DSH_AGENTS_HOME" "$FOTOR_WORKSPACE"
```

```bash
dsh --version
"$FOTOR_NODE" --version
"$FOTOR_NODE" "$FOTOR_NPM_CLI" --version
git --version
```

**Done when:** DeepSeek Harness reports the accepted version, Node reports `v24.17.0`, npm runs under that Node, and the selected locations are known. Stop on a missing or mismatched runtime; do not silently install a newer bridge or upgrade daily Node.

## 2. Obtain a complete package

Use the DeepSeek Harness package from the intended source revision: `plugins/dsh/fotor`. Retain the entire directory, including its skills, references, examples and license. The repository root is a Pi Package and is not the DeepSeek Harness install entry.

The source README provides the internal/local installation entry; an exported README provides its channel's Git entry. Select a revision containing both the Bundle and this guide's JSON examples. Older snapshots may contain a Bundle without these setup materials. Do not assemble files from different revisions.

For a public Git checkout, select the intended available branch before taking `plugins/dsh/fotor`. The test selector is `release/test`; production uses `main` only after that branch contains DeepSeek Harness. A test suffix still connects to production Fotor services. Availability in an internal source does not establish a public release.

For example, once the test branch contains these setup materials:

```bash
git clone --branch release/test --single-branch https://github.com/fotor-ai/fotor-agent-plugins.git /absolute/path/to/fotor-source
git -C /absolute/path/to/fotor-source rev-parse HEAD
FOTOR_SOURCE=/absolute/path/to/fotor-source/plugins/dsh/fotor
```

**Done when:** `FOTOR_SOURCE` contains [the bridge manifest example](bridge-package.example.json), [the reviewed lock example](bridge-package-lock.example.json), [the full configuration example](configuration.example.yml) and the complete package resources.

## 3. Install the external bridge

The example filenames are inert. Copy them to the new external directory as npm's `package.json` and `package-lock.json`. Their private package name retains the original acceptance identity; it is not the Fotor Bundle name or an npm publication.

```bash
mkdir "$FOTOR_BRIDGE"
cp "$FOTOR_SOURCE/bridge-package.example.json" "$FOTOR_BRIDGE/package.json"
cp "$FOTOR_SOURCE/bridge-package-lock.example.json" "$FOTOR_BRIDGE/package-lock.json"
sha256sum "$FOTOR_BRIDGE/package-lock.json"
```

Require this lock SHA-256:

```text
da720cef01409be0f0952876bcc12b4f7e57ff6341c78168735f689bbf8fde27
```

If the directory already exists, inspect its non-credential manifest, lock and installed version before proceeding. Reuse an exact matching installation; stop on a conflict. Do not run `npm ci` in an unrelated directory: it replaces that directory's `node_modules`.

Use separate empty npm configuration files and a bridge-local cache. The lock already records the dependency graph and registry integrity values; `npm ci` verifies downloaded content without resolving a new graph.

```bash
touch "$FOTOR_BRIDGE/npmrc" "$FOTOR_BRIDGE/npm-globalrc"
"$FOTOR_NODE" "$FOTOR_NPM_CLI" ci --prefix "$FOTOR_BRIDGE" --userconfig "$FOTOR_BRIDGE/npmrc" --globalconfig "$FOTOR_BRIDGE/npm-globalrc" --cache "$FOTOR_BRIDGE/npm-cache" --ignore-scripts --no-audit --no-fund
"$FOTOR_NODE" "$FOTOR_NPM_CLI" ls --prefix "$FOTOR_BRIDGE" --depth=0
test -f "$FOTOR_BRIDGE/node_modules/mcp-remote/dist/client.js"
test -f "$FOTOR_BRIDGE/node_modules/mcp-remote/dist/proxy.js"
sha256sum "$FOTOR_BRIDGE/package-lock.json"
```

**Done when:** installation succeeds, `npm ls` reports `mcp-remote@0.15.1`, both entry files exist and the lock hash is unchanged. Keep these files outside Fotor. Connection requests use them directly, without `npx`, downloads or dependency updates.

## 4. Install the Bundle and resolve its actual path

Choose one source. A complete local package works without a public release:

```bash
dsh plugin --profile "$FOTOR_PROFILE" add "$FOTOR_SOURCE" --ignore-scripts
```

For an available public test revision:

```bash
dsh plugin --profile "$FOTOR_PROFILE" add "git+https://github.com/fotor-ai/fotor-agent-plugins.git#release/test&path:/plugins/dsh/fotor" --ignore-scripts
```

For production, use the production README's command after confirming that `main` contains DeepSeek Harness. Keep the entire Git source quoted so the shell does not interpret `&`. Do not add both local and Git sources.

DeepSeek Harness forwards package commands to its profile's package manager. Use its native dependency and root queries, then resolve the installed entry:

```bash
dsh plugin --profile "$FOTOR_PROFILE" why fotor
dsh plugin --profile "$FOTOR_PROFILE" root
FOTOR_INSTALLED="$(realpath "$(dsh plugin --profile "$FOTOR_PROFILE" root)/fotor")"
cat "$FOTOR_INSTALLED/package.json"
test -d "$FOTOR_INSTALLED/skills"
```

Read the recorded dependency spec and resolved revision in the selected profile's package manifest/lock when verifying a Git source. `why` alone does not prove the resolved commit. Keep the installed version, source and `FOTOR_INSTALLED` together for later updates.

If using a dedicated pnpm store for a rehearsal, select it on the first `add` with `--store-dir /absolute/path/to/task-pnpm-store`, and pass that same option to later `update` and `remove`. Existing profiles must retain their existing store; a conflicting store is not repaired by deleting caches.

**Done when:** one installed `fotor` resolves to the intended package/version and source. Its `cordis.patch.yml` still disables both components, so installation has not scanned skills, launched the bridge or started OAuth.

## 5. Apply complete configuration and load skills

Copy the example outside the installed directory. Keep an existing override unchanged until its ownership and intended settings are understood.

```bash
cp -n "$FOTOR_INSTALLED/configuration.example.yml" "$FOTOR_PATCH"
```

Edit that external YAML file. Shell variables in this guide are not automatically expanded inside YAML: put the resolved literal values into every field.

| YAML field | Required value |
| --- | --- |
| Skill `dshHome` / `agentsHome` | Effective `DSH_HOME` / `DSH_AGENTS_HOME` for this launch; use the selected client's actual locations. |
| Skill `customSkillDirs` | One absolute directory: the resolved `FOTOR_INSTALLED` followed by `/skills`. |
| Skill `providerName`, `includeDefaultRoots`, `watch` | Keep `fotor`, `false`, `false`; scan only this installed package. |
| MCP `command` | Literal `FOTOR_NODE`. |
| First MCP `args` entry | Literal `FOTOR_BRIDGE` followed by `/node_modules/mcp-remote/dist/proxy.js`. |
| MCP URL, transport and resource | Retain the production values from the example. |
| Callback port and host | Select a free port and retain the example's loopback host. Use exactly the same pair for the standalone client. |
| MCP `env.MCP_REMOTE_CONFIG_DIR` | Literal `FOTOR_AUTH`; DeepSeek Harness must pass it to the bridge child. |
| MCP `cwd` | Literal `FOTOR_WORKSPACE`. |
| Component states | Skills enabled; MCP disabled until step 6 succeeds. Keep reconnect disabled and the supplied timeouts. |

Use the two complete rows as a native overlay. For a new isolated profile with no provider credentials, check composition before launching:

```bash
dsh --profile "$FOTOR_PROFILE" --patch "$FOTOR_PATCH" --dump-config
```

In an existing profile, inspect only the Fotor component settings through native controls. A full dump can include unrelated private settings; do not request, paste or commit it. The host's own dump may contain expressions, which are not Fotor configuration or instructions to execute.

Launch with the same overlay:

```bash
dsh --profile "$FOTOR_PROFILE" --patch "$FOTOR_PATCH"
```

An alternative is to merge these rows into that profile's external `cordis.patch.yml`, preserving unrelated rows. Choose one loading method and use it on subsequent launches. DeepSeek Harness replaces complete `config` objects; partial field overrides do not deep-merge.

For interactive use, the user configures any model-provider account through DeepSeek Harness's native settings, separately from Fotor OAuth. In Web, select the intended workspace. Use the actual skill menu to select `/fotor-connect`, asking it to explain the connection steps without Fotor OAuth or MCP calls. Check that it reads resources under `FOTOR_INSTALLED`. An installation-only rehearsal can stop at configuration and resource-path checks; it does not establish model-driven skill execution.

**Done when:** the effective configuration has one Fotor provider, its installed skills are discoverable, and the MCP component remains disabled. If the Shell sandbox hides a temporary package path, use DeepSeek Harness's native `read` tool with the skill's supplied base directory; a failed `bash ls` does not prove the package is missing.

## 6. Authorize, then enable the connection

Follow [first authorization](references/authentication.md#first-authorization). Run its standalone client command using the Node, bridge, Fotor credential directory, endpoint/resource and callback values selected above. Keep stdin open through actual tool discovery. Confirm that the opener uses the user's daily ordinary browser; preserve its website login and browser data. If its routing is unclear, resolve it before starting authorization instead of changing the system browser default.

The user completes login/consent and chooses Continue, Wait or Cancel. After Continue, require client-confirmed connection and actual tool discovery. Then stop the standalone client and verify its callback listener has closed. Change only the external override's `fotor-mcp.disabled` to `false` and restart DeepSeek Harness with the same overlay. Do not run authorization client and proxy concurrently.

**Done when:** DeepSeek Harness exposes the configured Fotor tools and a real read-only model query succeeds. Installation, a browser success page, or an exit code alone does not establish this.

## 7. Use, update or remove Fotor

### First requests

| Intent | Example request |
| --- | --- |
| MCP only | “Use existing Fotor authorization to query image models. Do not authorize again or open a website; stop if authorization is needed.” |
| Website connection | “Connect to Fotor and introduce its main features. Use my confirmed daily browser and preserve its existing login state.” |
| Explicit skill | Select `/fotor-connect` from the actual skill menu, then give the intended request. |
| Other capabilities | Use the [package README](README.md) and the selected skill's workflow. A capability listing does not perform media operations. |

For website access, prepare the confirmed browser/profile before requesting a link. Follow [website access](references/authentication.md#website-access): open the original production handoff once, use native single-command approval if required, and wait for the user's actual page/login result. Manual opening is a separate fallback. An already signed-in page does not prove a new login.

### Update or change source

```bash
dsh plugin --profile "$FOTOR_PROFILE" update fotor --ignore-scripts
dsh plugin --profile "$FOTOR_PROFILE" why fotor
FOTOR_INSTALLED="$(realpath "$(dsh plugin --profile "$FOTOR_PROFILE" root)/fotor")"
```

Pass the same explicit `--store-dir` if one was selected at installation. Check the new source revision, version and installed files, refresh `customSkillDirs` if the resolved directory changed, and start a new DeepSeek Harness process. The bridge version, external authorization store and security arguments remain unchanged.

If the native update does not advance, keep that result and use native removal/addition only as a separately identified reinstall. Source changes require recording the old source, stopping the Fotor components, removing only their override rows, and reinstalling the intended source in the same profile. Preserve unrelated configuration and credentials.

### Remove

Stop the Fotor proxy, disable both components and remove only their external override rows before native removal:

```bash
dsh plugin --profile "$FOTOR_PROFILE" remove fotor --config.ignore-scripts=true
```

Use the original profile and store. This baseline's `remove` rejects `--ignore-scripts`; use the form above. Preserve the external bridge and Fotor credential directory for later reuse unless their deletion is explicitly requested. Local deletion does not revoke server-side consent. Daily website login remains unchanged.

Verify that the profile no longer registers Fotor as a dependency or Bundle and that its effective configuration has neither Fotor component. Directory installation can use a link to the source directory. In the installation rehearsal with pnpm `11.28.5`, native removal removed registration but left that unused link; native `unlink fotor` reported “Nothing to unlink.” Report such residue separately, preserve the original source directory, and do not claim all installed filesystem entries were removed. A dedicated rehearsal directory can be deleted during its scoped cleanup.

## Troubleshooting and evidence boundaries

| Observation | Next action |
| --- | --- |
| Bridge missing or wrong version | Return to step 3; inspect the selected external directory. Package installation cannot install it implicitly. |
| Skills missing / wrong base directory | Resolve the installed package again, inspect the effective full override and use native `read`. Keep one Fotor provider. |
| `ERR_PNPM_UNEXPECTED_STORE` | Reuse the store selected on the original native installation; do not recreate the cache or change global settings. |
| Generic Git-package message asks to enable a build script | Inspect the actual failure. Fotor has no build/lifecycle script; do not enable unrelated builds to hide a source, permission or store error. |
| `needsAuth`, `discoveryState` error or “Start a new authorization flow” | Stop the failed call and use [controlled recovery](references/authentication.md#bounded-recovery-and-controlled-reauthorization). Preserve the store and security arguments. |
| Browser command blocked | Request the host's approval for that one opener command, or provide the unused handoff for manual opening in the confirmed browser. |
| Website link expired | Follow the bounded recovery in the authentication reference; do not reopen a consumed link. |

Report package installation, skills, bridge authorization, actual MCP calls, website login, restart reuse and controlled recovery separately. Full DeepSeek Harness support remains pending real successful controlled-reauthorization recovery. Following this guide does not erase that gate or establish expiry refresh, Desktop, headless, ACP or media acceptance.
