# Fotor for Google Antigravity

<img src="assets/logo.svg" alt="Fotor logo" width="96" />

Fotor plugin **0.13.0-test.1** · test channel · [License](LICENSE)

Create and edit images, videos and audio with Fotor in the Antigravity App
and CLI. Six skills and the remote MCP service install together. The
[manifest](plugin.json) identifies the package. Antigravity manages sign-in
and tool approvals for the endpoint in [mcp_config.json](mcp_config.json).

For the first Antigravity collection, the candidate branch is
`release/v0.13.0` and the formal tag is `v0.13.0`. Check that the selected
ref is available before installing.

<a id="antigravity"></a>

## Google Antigravity App / CLI

Choose the [public collection](https://github.com/fotor-ai/fotor-agent-plugins) containing [Fotor **0.13.0-test.1**](README.md)
on the **test** package channel. Set `public_ref` to the published
`release/test` branch for testing, the reviewed `release/v<collection-version>`
branch for candidate review, or the published `v<collection-version>` tag for a formal release.
Use the **collection version** in the branch or tag name.

Set `public_sha` to the full commit you verified for that ref on GitHub (for an
annotated tag, use the commit it points to). Set `checkout` to a new directory.
Run `agy plugin list --json` first. If Fotor is already installed, verify its
source and files before replacing it. Stop if another plugin, workspace skill
or MCP entry already supplies Fotor.

```bash
(
  set -eu
  git clone --single-branch --branch "$public_ref" https://github.com/fotor-ai/fotor-agent-plugins.git "$checkout"
  cd -- "$checkout"
  test "$(git rev-parse HEAD)" = "$public_sha" || exit 1
  git checkout --detach "$public_sha"
  agy plugin validate --strict --json ./plugins/antigravity/fotor
  agy plugin install ./plugins/antigravity/fotor
  agy plugin list --json
)
```

The CLI installs one user-wide package. Open the target project in the CLI
and choose `/fotor:fotor-connect`, or open it in the App and find Fotor under
**Customizations → Installed**. To authorize in the App, choose
**Fotor → Accounts → Sign in**. The **MCP** tab shows the connected service
and its tools. See the [Antigravity authorization guide](skills/fotor-connect/references/authentication.md)
for approvals and recovery. Manage an owned installation with
`agy plugin disable fotor`, `agy plugin enable fotor` or `agy plugin uninstall fotor`.


Fotor receives the prompts, parameters and selected reference media needed
for the operations you request. Generation or processing may use account
credits. The credits skill only queries your own balance. Review the
[service workflow](skills/fotor-connect/references/mcp-integration.md)
for task results, website handoff and bounded recovery.
