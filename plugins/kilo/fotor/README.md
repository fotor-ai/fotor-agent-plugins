# Fotor for Kilo CLI

<p align="center"><img src="assets/logo.svg" width="72" alt="Fotor logo" /></p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.10.0&amp;color=087f8c&amp;style=flat-square" alt="Version" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=production&amp;color=238636&amp;style=flat-square" alt="Release channel" />
  <a href="LICENSE"><img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" alt="Apache-2.0 license" /></a>
</p>

Use Fotor's shared skills with Kilo CLI through local project skill directories
and a separately configured native MCP service. This source bundle belongs to the
[Fotor collection](https://github.com/fotor-ai/fotor-agent-plugins); verify channel availability before obtaining it.
No runtime plugin, npm dependency, installation hook or bridge is included.

| Field | Value |
| --- | --- |
| Bundle | `fotor` |
| Version | `0.10.0` |
| Release channel | `production` |
| MCP endpoint | `https://mcp.fotor.com/mcp` |
| Host baseline | Kilo CLI 7.8.27 |

## Features and verification

The shared skills cover connection/model queries, images, video, audio, uploads
and the user's own Fotor credit balance. Native discovery exposes the complete
service tool list subject to user permissions. Media submissions retain the
ordinary pre-submission credit notice.

Package checks and prior isolated core simulations do not establish native CLI conversation, production OAuth, website or media acceptance. Verify the actual installed version and each required host flow separately.

Desktop, independent marketplace installation and automatic bundle updates are
outside this support scope. Earlier core API tests remain experimental evidence.
See the [Kilo authentication reference](skills/fotor-connect/references/authentication.md).

<a id="kilo"></a>

## Kilo CLI

### Installation and source

#### Production release

Use the [collection installation guide](https://github.com/fotor-ai/fotor-agent-plugins#kilo) to select an available public source and a new checkout. Collection Tags identify collection versions, not this independently versioned Kilo package. The test source is `release/test`; production uses the verified collection Tag whose version table contains this package. An unpublished candidate uses the exact branch supplied for review.

Confirm version **0.10.0**, release channel **production**, source commit and file inventory. Downloading a source checkout is not installation: perform the two project-scoped steps below. Local development uses a reviewed fixed-source export. Keep existing configuration and grants. Marketplace and Desktop installation are outside this route.

Use a dedicated project and keep the source checkout outside it. Select the
complete `plugins/kilo/fotor/` package from that checkout. Check existing
project/global Fotor services and skill sources first; a same-name conflict stops
installation for review. Preserve unrelated files and user configuration.

1. Copy the complete directories from the package's `skills/` into the project's
   `.kilo/skills/`. Keep all references, licenses and `fotor-version.json` files.
2. Merge this entry into the project's `.kilo/kilo.jsonc`, preserving other keys,
   comments and permissions:

```json
{
  "mcp": {
    "fotor": {
      "type": "remote",
      "url": "https://mcp.fotor.com/mcp",
      "enabled": true,
      "oauth": {},
      "timeout": 300000
    }
  }
}
```

Kilo CLI 7.8.27's named non-interactive `mcp add` command writes global config;
the two steps above deliberately manage only the selected project. Record source
commit, hashes and modes. `MCP.yaml` is future marketplace source, not the runtime
configuration file. This route creates no marketplace receipts.

### Skills and connection

Start Kilo in that project. Verify **0.10.0** from each installed version
marker and compare files with the selected package. Use native `/fotor-connect`
completion or natural-language skill discovery. Read references relative to the
actual skill directory; no package README or manifest is assumed above it.

From the same project, use `kilo mcp list` for native status. Reuse a valid grant;
when authorization is required, use `kilo mcp auth fotor` and complete consent in
the ordinary external browser. Native HTTP/OAuth and complete tool discovery
remain subject to user permissions. The 300000 ms setting covers native waiting;
valid progress can reset it, and explicit user settings remain authoritative.
See the [authentication guide](skills/fotor-connect/references/authentication.md).

### Website, balance and maintenance

Confirm the external browser and its existing login state before requesting a
website handoff. Open the original returned URL once and have the user confirm
the final page. The balance skill queries only the user's own Fotor balance.

For MCP disable/enable, change the owned project entry's `enabled` value; local
skills may remain discoverable. Before updating or removing files, compare them
with the retained install inventory and stop on unexpected edits. Manage the
owned skill directories and Fotor configuration separately, preserving unrelated
resources and native grants. After reinstallation and a full CLI restart, verify
paths, files and one approved read-only query. These checks are manual deployment
and reload checks, not native market installation or automatic bundle upgrades.


## Workflows

Audio workflows cover soundtracks, track creation, song editing, speech, podcasts
and separation. Use the matching installed skill and actual returned model schema.
Consult [task/result handling](skills/fotor-connect/references/mcp-integration.md#runtime-workflow),
[media preparation](skills/fotor-connect/references/media-upload.md) and
[balance queries](skills/fotor-connect/references/credits.md).

## Contents and license

`bundle.json` is Fotor's version metadata. Each skill includes four local
references, a license and a version marker. `MCP.yaml` and deterministic archives
remain future marketplace materials; they are not used by the local CLI setup.
The package includes its Logo and [Apache-2.0 license](LICENSE).
