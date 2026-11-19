# Fotor for omp (oh-my-pi)

<img src="assets/logo.svg" alt="Fotor logo" width="96" />

Fotor plugin **0.12.0** · production channel · [License](LICENSE)

Generate images, videos and audio with Fotor from omp CLI. The six skills and
bundled HTTP MCP service install together. Both channels connect to production.
Selecting a draft uses the model's recommended defaults and may still consume
credits. When choosing a higher quality for you, the agent will first explain
the change and offer a draft instead.

<a id="omp"></a>

## omp (oh-my-pi)

#### Production release

Select a verified `main` checkout that contains `.omp-plugin/marketplace.json` pointing to `plugins/omp/fotor`. Before public release, use only the fixed candidate checkout supplied for review. Keep the checkout available while using this local catalog. Run in your target Git project. If this profile already has a Fotor catalog, package, skill or server from another source, stop. Do not replace or force-reinstall it. For the same known catalog omit the add step.

```bash
omp plugin marketplace add "$source_checkout"
omp plugin install fotor@fotor-ai --scope project
omp plugin list --json
omp skill list --json
```

Use `/skill:fotor-connect` and `/mcp list` in the active session. Check the
source, not just the name: older Fotor catalogs resolve the same plugin ID to
the Claude Code package. The omp package contributes six skills and the native
`fotor:fotor` HTTP server. Preserve the profile's approval policy and use
native OAuth when needed.


Read the [omp authentication and choice guide](skills/fotor-connect/references/authentication.md)
for native OAuth, skill and tool discovery, user preferences and browser scope.
Find the installed version in [the plugin manifest](.claude-plugin/plugin.json).
For project-level removal, verify ownership and use
`omp plugin uninstall fotor@fotor-ai --scope project`. Keep the profile and
credentials. Loading the plugin alone does not verify media delivery.
