# Fotor for Hermes Agent

<p align="center"><img src="assets/logo.svg" width="72" alt="Fotor logo" /></p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.11.1&amp;color=087f8c&amp;style=flat-square" alt="Version" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=production&amp;color=238636&amp;style=flat-square" alt="Release channel" />
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" alt="Apache-2.0 license" /></a>
</p>

Create images, videos, and audio with Fotor in Hermes Agent. Six portable skills
provide the workflows; Hermes manages a separately configured native MCP service
and your Fotor authorization. This package contains instructions and static
resources, with no runtime code, dependencies or installation hooks.

| Field | Value |
| --- | --- |
| Plugin | `fotor` |
| Version | `0.11.1` |
| Release channel | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Implementation baseline | Hermes Agent 0.23.1 |

## Features and verification

Use the matching skill for connection and models, images, video, audio, uploads,
or querying your own Fotor credit balance. Native discovery exposes the actual
service tools subject to your permissions. Media submissions retain the shared
pre-submission credit notice.

Package checks cover files, references, and configuration. Verify installation, MCP authorization and calls, website login, credit links, uploads, and media execution separately in the selected host and environment. A release or model query does not establish successful website login or completed media results.

Source availability, installed content, live CLI/Desktop results and official
catalog inclusion are verified separately. A source snapshot or release badge
does not establish official catalog admission.

<a id="hermes"></a>

## Hermes Agent CLI / Desktop

### Hermes plugin installation

#### Install from the Desktop catalog

In Hermes Agent Desktop, select your profile and open **Capabilities → Plugins**.
Search for `fotor` with the tier filter set to **All** or **Community**.
On the **Fotor** card, check the listed version and click **Install** (**+**).
Enable the plugin if needed, then follow the
[MCP connection steps](#hermes-mcp-registration-and-connection) in the same profile.

#### Production release

Select an available collection source whose version table contains this Hermes package. For a test build use `release/test`; for production use its verified collection Tag. Before publication, use only the candidate branch and commit supplied for review.

Set `public_ref` to that branch or Tag, `source_checkout` to a new directory, and `hermes_profile` to your existing target profile. Download and resolve the full 40-character commit:

```bash
git clone --single-branch --branch "$public_ref" https://github.com/fotor-ai/fotor-agent-plugins.git "$source_checkout"
public_sha=$(git -C "$source_checkout" rev-parse HEAD)
```

Compare that commit and `plugins/hermes/fotor/` with the reviewed source. Confirm plugin version **0.11.1** and release channel **production**. Downloading is not installation; install from the public URL pinned to this commit:

```bash
hermes -p "$hermes_profile" plugins install "https://github.com/fotor-ai/fotor-agent-plugins.git#plugins/hermes/fotor" --ref "$public_sha" --no-enable
hermes -p "$hermes_profile" plugins enable fotor
hermes -p "$hermes_profile" plugins list --user --json
```

Keep native scan and consent checks. Review an existing same-name plugin or skill before replacement. Preserve current profile settings and the separate Fotor MCP configuration and authorization.

Hermes CLI and Desktop can share a profile and installation. Select that same
profile in Desktop; verify the actual running core before acceptance. Current
implementation targets 0.23.1.

#### Official catalog alternative

Use this alternative only when the [official catalog](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugin-catalog)
lists `fotor` with the expected version, public repository, full commit and
`plugins/hermes/fotor` subdirectory. A GitHub Release does not establish listing.
If the entry is absent or does not match, use a verified Git source above.

```bash
hermes -p "$hermes_profile" plugins install fotor --no-enable
hermes -p "$hermes_profile" plugins enable fotor
hermes -p "$hermes_profile" plugins list --user --json
```

Omit `--ref` when installing by name so Hermes records the reviewed catalog origin.

### Hermes MCP registration and connection

The skills plugin does not register MCP. Inspect the target profile first and
stop at an existing Fotor entry or ambiguous skill source. The
[configuration example](mcp.example.json) is documentation,
not an auto-loaded manifest. Add a new entry disabled with native approvals:

```bash
hermes -p "$hermes_profile" config set mcp_servers.fotor '{"url":"https://mcp.fotor.com/mcp","auth":"oauth","enabled":false,"trust":"untrusted"}'
hermes -p "$hermes_profile" mcp login fotor
# After native authorization and connection succeed:
hermes -p "$hermes_profile" config set mcp_servers.fotor.enabled true
```

Reuse valid authorization. When consent is required, the user completes the
native flow in an ordinary external browser. Preserve current approvals and
explicit settings; this package adds no timeout override. Hermes 0.23.1 uses
300 seconds for tool calls by default. Select `fotor:fotor-connect` through the
client's available skill entry or ask Hermes to find the Fotor connection skill.
Follow [Hermes authentication](skills/fotor-connect/references/authentication.md)
for actual tool discovery, bounded authorization, known host conditions and
client-specific browser paths.

### Hermes updates and removal

Compare installed files and recorded configuration before replacing content.
Keep a rollback source and stop on user modifications or ambiguous ownership.
To change a fixed Git commit or switch between Git and catalog sources, remove
only the verified Fotor skills plugin, then follow the selected installation
route above:

```bash
hermes -p "$hermes_profile" plugins remove fotor
```

This is a source/version replacement, not an automatic upgrade. Explicit Git
`--ref` installs stay pinned. For a catalog install, use the read-only
`hermes -p "$hermes_profile" plugins check-updates --json` to inspect the Fotor
entry; it also lists other plugins, which need no changes. When a newer reviewed
catalog version is available, inspect its changes before running
`hermes -p "$hermes_profile" plugins update fotor`. A first catalog installation
and a no-update result do not prove a cross-version upgrade.

Native `plugins disable` / `enable`
control skills, while `mcp_servers.fotor.enabled` controls the separate service.
For this credential-preserving lifecycle, remove the plugin with `plugins remove
fotor` and remove only its native configuration with `config unset
mcp_servers.fotor`. Keep native authorization and unrelated entries. The native
`mcp remove` command also removes OAuth tokens and is not used by this procedure.

Reload the actual shared gateway only when other sessions will not be affected,
then verify the installed version, skills and an approved read-only query from
a new session. CLI and Desktop sharing one installation is not two independent
installations. A local fixed-source reinstall is not a public-channel upgrade.


## Remote service and data use

The skills run through Hermes tools and the separately authorized Fotor service
at `https://mcp.fotor.com/mcp`. When you request an operation, its prompts, parameters and
selected reference media are sent to Fotor as required. Uploads can transmit
files you select; installing the skills alone does not upload files. Media
generation and processing may use your existing Fotor account credits. The
credits skill only queries your own balance.

Website access uses a short-lived handoff link opened at your request: an
ordinary external browser in CLI, or the current session's native preview in
Desktop. Preserve existing browser data and confirm the destination. Native
OAuth credentials and tool approvals remain under Hermes control. The remote
service and its discovered tool list can change independently of the pinned
plugin commit; that pin fixes the instructions and assets, not Fotor's service.

## Workflows

Audio workflows include soundtracks, track creation, song editing, speech,
podcasts and separation. Follow the actual returned model schema and the
[task/result contract](skills/fotor-connect/references/mcp-integration.md#runtime-workflow),
[media preparation](skills/fotor-connect/references/media-upload.md), and
[own-balance queries](skills/fotor-connect/references/credits.md).

## Contents and license

The Agent Plugins v1 `plugin.json` declares the plugin; `skills/` contains six
independent skills, each with four local references. `mcp.example.json` documents
the separate native configuration and is never automatically loaded. The package
includes its logo and [Apache-2.0 license](LICENSE).
