# Fotor for Cline

<p align="center"><img src="assets/logo.svg" width="72" alt="Fotor logo" /></p>

<p align="center">
  <img src="https://img.shields.io/static/v1?label=version&amp;message=0.14.0-test.3&amp;color=087f8c&amp;style=flat-square" height="20" alt="Version" />
  <img src="https://img.shields.io/static/v1?label=channel&amp;message=test&amp;color=a56800&amp;style=flat-square" height="20" alt="Release channel" />
  <img src="https://img.shields.io/static/v1?label=license&amp;message=Apache-2.0&amp;color=57606a&amp;style=flat-square" height="20" alt="Apache-2.0 license" />
</p>

Create images, videos, and audio by describing what you want in Cline.
The six skills also cover reference uploads, available models and your own
Fotor credit balance.

| Field | Value |
| --- | --- |
| Plugin | `fotor` |
| Version | `0.14.0-test.3` |
| Release channel | `test` |
| Client baseline | Cline Desktop 0.0.50 · Cline CLI 3.0.75 |
| Remote service | `https://mcp.fotor.com/mcp` |

<a id="cline"></a>

## Cline Desktop / CLI

### Install from Git with Cline

On the Hub host used by CLI and Desktop, install the six skills once. If
Fotor is already installed, check its source in `cline config plugins --json`
and its skills in `cline config skills --json` first. Also check for an
existing portable copy at `~/.agents/plugins/fotor/`. Keep one active Fotor
skill source per Hub.

```bash
cline plugin install "https://github.com/fotor-ai/fotor-agent-plugins.git@release/test"
```

Open a new session. Check that `cline config plugins --json` includes
`fotor` and `cline config skills --json` lists its six `fotor-*` skills.
The installed files, including each skill's references, live under Cline's
plugin store. Continue with the connection steps for your client.

**Changing sources.** Retain the old source and check for local edits. For
a native Git installation, confirm that `cline config plugins --json` shows
exactly one `fotor` and that its path belongs to the source you intend to
replace. Run `cline plugin uninstall fotor` before installing another ref.
For a portable copy, check ownership and compare it to its retained package.
Move the verified directory outside `~/.agents/plugins/` and keep it for
rollback. Resolve modifications or use by another client before moving it.
If the new install fails, inspect the active sources. Restore the verified
portable backup to its original location or reinstall the previous native Git
commit, then check the Fotor skill paths. Leave the registered `fotor` MCP
service and its grant in place.

<details>
<summary>Alternative installation from a verified local checkout</summary>

### Get the Cline package

Open the collection's [`release/test`](https://github.com/fotor-ai/fotor-agent-plugins/tree/release/test) on GitHub. Copy the full 40-character **commit** shown on GitHub. In Bash
on the Hub host, set `public_sha` to that value and `checkout` to a new
absolute directory outside `~/.agents/plugins/`. Keep this checkout for
verification and recovery.

```bash
(
  set -eu
  umask 022
  test -n "$public_sha"
  test ! -e "$checkout" && test ! -L "$checkout" || exit 1
  git clone --single-branch --branch release/test \
    https://github.com/fotor-ai/fotor-agent-plugins.git "$checkout"
  test "$(git -C "$checkout" rev-parse HEAD)" = "$public_sha"
  git -C "$checkout" checkout --detach "$public_sha"
)
```

The Cline package is `plugins/cline/fotor/` in this checkout. Check that its
`plugin.json` names `fotor` and version **0.14.0-test.3** (test channel).

### Install or update the six skills

CLI and Desktop use one copy at `~/.agents/plugins/fotor/` on the same Hub.

- **First installation.** Check for another portable package or ordinary skill
  supplying the same names. Follow the [verified installation steps](#verified-installation-and-rollback)
  with the target directory absent.
- **Existing installation.** Locate the verified package used for the current
  install. Follow the [replacement and rollback steps](#verified-installation-and-rollback) to
  compare every file and its permissions, stage the new package outside the
  discovery directory and retain the previous copy. Resolve any local edits
  or ownership conflict before switching.

</details>

### Connect from the CLI

Run `cline config mcp --json` locally. Reuse the enabled `fotor` service at
`https://mcp.fotor.com/mcp` and its grant. Only for a new registration, open the interactive wizard:

```bash
cline mcp add fotor https://mcp.fotor.com/mcp --transport http
```

Select **Remote (HTTP)** and **OAuth**. Leave the client ID empty for dynamic
registration. If a registered service needs a fresh grant, open `cline mcp`
and choose **Authorize OAuth** for that entry. A fresh Fotor MCP sign-in expires the previous MCP login for
the same account. Confirm before continuing and complete consent in your
browser. Start `cline` in your project. In **Settings → Skills**, look for six
enabled Fotor skills. Open `fotor-connect` for a native installation or
`fotor:fotor-connect` for a portable copy, then ask for image models.

### Connect from Desktop

Open your project on the local Hub used by the CLI. In **Customize → Installed**, confirm
Fotor and its six skills. Inspect the existing `fotor` entry in MCP settings.
If Desktop is the first client used to register the service, select **Add MCP
Server → Remote → Streamable HTTP**, enter `fotor` and `https://mcp.fotor.com/mcp`, then
authorize using the native OAuth action with your consent. Start a new
conversation and ask “Which image models does Fotor offer?”

Package replacement preserves Cline's separately managed registration, native
grant, tool approvals and timeouts. Use Cline's plugin controls for the installed source. [Cline authentication guide](skills/fotor-connect/references/authentication.md)

## Verified installation and rollback

The following copy and rollback procedure applies to the portable installation.
Run these steps on the Hub host in the same Bash terminal as the download.
They use Git, tar and a SHA-256 utility (`sha256sum` or `shasum`).

- For a **first install**, leave `previous_package` and `rollback_from` unset.
- To **replace Fotor**, set `previous_package` to the absolute path of the
  previously installed package in its retained, verified checkout or export.
  Check its recorded commit or inventory. Use an independent source outside
  the Hub's plugin directory. Leave `rollback_from` unset.
- To **undo a completed replacement**, set `rollback_from` to the recovery
  directory printed by that replacement and run the block again. It verifies
  the current install and the saved copy against their respective inventories.

Switch when other sessions are finished using Fotor. Keep the printed recovery
directory. It holds file inventories and the previous package outside plugin
discovery. The lock coordinates simultaneous switches and checks that staging
and installation share a filesystem.

<details>
<summary>Run the verified copy and switch</summary>

```bash
(
  set -eo pipefail
  [[ "$HOME" == /* ]] || exit 1
  umask 022
  registry="$HOME/.agents/plugins"
  target="$registry/fotor"
  [[ ! -L "$registry" ]] || exit 1
  mkdir -p "$registry"
  work=$(mktemp -d "$HOME/.agents/fotor-switch.XXXXXXXX")
  printf 'Recovery directory: %s\n' "$work"
  mkdir "$work/incoming" "$work/previous"
  lock="$registry/.fotor-install.lock"
  printf '%s\n' "$work" > "$work/.fotor-install.lock"
  ln "$work/.fotor-install.lock" "$registry/" || exit 1

  finish() {
    local result=$?
    if ((result != 0)) && [[ -d "$work/previous/fotor" && ! -e "$target" && ! -L "$target" ]]; then
      mv -n "$work/previous/fotor" "$registry/" || printf 'Previous package retained in %s\n' "$work/previous" >&2
    fi
    if [[ "$lock" -ef "$work/.fotor-install.lock" ]]; then rm "$lock"; fi
  }
  trap finish EXIT
  trap 'exit 1' HUP INT TERM

  inventory() (
    [[ -d "$1" && ! -L "$1" ]] || exit 1
    cd "$1" || exit 1
    invalid=$(find . ! -type d ! \( -type f -perm 0644 \) -print) || exit 1
    [[ -z "$invalid" ]] || { printf 'Unexpected type or permissions: %s\n' "$invalid" >&2; exit 1; }
    find . -type d -print | LC_ALL=C sort || exit 1
    if command -v sha256sum >/dev/null; then set -- sha256sum; else set -- shasum -a 256; fi
    find . -type f -exec "$@" {} + | LC_ALL=C sort
  )
  verify() {
    inventory "$1" > "$work/observed" && cmp "$2" "$work/observed"
  }

  if [[ -n "$rollback_from" ]]; then
    cp -Rp "$rollback_from/previous/fotor" "$work/incoming/"
    cp "$rollback_from/previous.sha256" "$work/new.sha256"
    cp "$rollback_from/new.sha256" "$work/previous.sha256"
  else
    [[ "$checkout" == /* && -n "$public_sha" ]] || exit 1
    test "$(git -C "$checkout" rev-parse HEAD)" = "$public_sha"
    mkdir "$work/incoming/fotor"
    git -c tar.umask=0022 -C "$checkout" archive "$public_sha:plugins/cline/fotor" |
      tar -xf - -C "$work/incoming/fotor"
    inventory "$checkout/plugins/cline/fotor" > "$work/new.sha256"
    if [[ -n "$previous_package" ]]; then
      [[ ! "$previous_package" -ef "$target" ]] || exit 1
      inventory "$previous_package" > "$work/previous.sha256"
    fi
  fi

  test -f "$work/incoming/fotor/plugin.json"
  verify "$work/incoming/fotor" "$work/new.sha256"
  if [[ -f "$work/previous.sha256" ]]; then
    verify "$target" "$work/previous.sha256"
    mv -n "$target" "$work/previous/"
    verify "$work/previous/fotor" "$work/previous.sha256"
  else
    [[ ! -e "$target" && ! -L "$target" ]] || { echo 'Fotor already exists' >&2; exit 1; }
  fi
  mv -n "$work/incoming/fotor" "$registry/"
  test ! -e "$work/incoming/fotor"
  verify "$target" "$work/new.sha256"
)
```

</details>

If the switch fails after moving the old directory, the block restores it
when the destination is free. If another writer has occupied the destination,
keep both copies and inspect the reported recovery directory. An interrupted
process can leave its lock there and in the registry. Check the owning process
and the recorded path before cleaning up.

## Account and data

Cline reads the [portable manifest](plugin.json) and skills on the Hub host.
The [MCP settings example](mcp.example.json) documents the separate service
registration. Cline manages OAuth, discovers the current tools and applies
your approval settings. Fotor receives the prompts, parameters and reference
media needed for operations you request. Media generation and processing can
use your Fotor credits. [Service workflow](skills/fotor-connect/references/mcp-integration.md)

## License

[Apache License 2.0](LICENSE)
