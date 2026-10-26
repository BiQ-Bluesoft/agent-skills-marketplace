# Bluesoft Agent Skills

A public **agent plugin marketplace** built on the [Agent Plugins 1.0](https://github.com/agentplugins/agent-plugins-spec/blob/main/spec/1.0.0.md)
standard. It hosts plugins (skills, agents, MCP servers) maintained by Bluesoft, installable
in GitHub Copilot CLI, VS Code, Claude Code, and Codex.

## Install this marketplace

All clients read the same [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) catalog.

**GitHub Copilot CLI**

```shell
copilot plugin marketplace add BiQ-Bluesoft/agent-skills-marketplace
copilot plugin install xperience-by-kentico@bluesoft-agent-skills
```

**VS Code** ([agent plugins are in preview](https://code.visualstudio.com/docs/copilot/customization/agent-plugins) — enable `chat.plugins.enabled`)

```jsonc
"chat.plugins.marketplaces": ["BiQ-Bluesoft/agent-skills-marketplace"]
```

Then open the **Extensions** view, search `@agentPlugins`, and install.

**Claude Code**

```shell
/plugin marketplace add BiQ-Bluesoft/agent-skills-marketplace
/plugin install xperience-by-kentico@bluesoft-agent-skills
```

**Codex** — a workspace admin imports it once via ChatGPT **Admin > Plugins > Add > Import
marketplace**, entering `https://github.com/BiQ-Bluesoft/agent-skills-marketplace`. See
[Plugin management](https://developers.openai.com/codex/enterprise/plugin-management). After
that, everyone installs via `/plugins`.

## Available plugins

| Plugin | Skill | Description |
|--------|-------|-------------|
| `xperience-by-kentico` | `xbyk-api-lookup` | Look up Xperience by Kentico (XbyK) API types, methods, interfaces, and namespaces from the official API reference. |

## Repository structure

```
.claude-plugin/marketplace.json    # Marketplace catalog — every client reads this
plugins/
  xperience-by-kentico/
    plugin.json                    # Agent Plugins 1.0 manifest (Copilot CLI, VS Code, Codex)
    .claude-plugin/plugin.json     # Claude Code manifest (same metadata, Claude's required layout)
    skills/xbyk-api-lookup/SKILL.md
```

Claude Code doesn't yet auto-detect the Agent Plugins 1.0 root manifest, so each plugin
ships both manifests to stay portable.

## Adding a new plugin

1. Create `plugins/<plugin-name>/plugin.json` (Agent Plugins 1.0 format, see
   `plugins/xperience-by-kentico/plugin.json`) and a matching
   `plugins/<plugin-name>/.claude-plugin/plugin.json`. Keep both in sync when bumping versions.
2. Add a skill at `plugins/<plugin-name>/skills/<skill-name>/SKILL.md`. Copilot-specific
   extras (agents, hooks, slash commands, LSP servers) go under `com.github.copilot/`.
3. Register the plugin in `.claude-plugin/marketplace.json` (`name`, `description`,
   `version`, `source`).
4. Test locally:
   ```shell
   copilot plugin marketplace add .
   copilot plugin install <plugin-name>@bluesoft-agent-skills
   copilot plugin list
   ```
5. Open a PR. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Publishing / pushing to GitHub

```shell
git branch -M main
git push -u origin main
```

If the remote already has unrelated commits, reconcile first (`git pull --rebase
--allow-unrelated-histories origin main`) or force-push to replace its contents.

## Reference

- [Agent Plugins 1.0 spec](https://github.com/agentplugins/agent-plugins-spec/blob/main/spec/1.0.0.md)
- [Copilot CLI plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace) · [CLI reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference)
- [Agent plugins in VS Code](https://code.visualstudio.com/docs/copilot/customization/agent-plugins)
- [Claude Code marketplaces](https://code.claude.com/docs/en/plugin-marketplaces) · [installing plugins](https://code.claude.com/docs/en/discover-plugins)
- [Codex plugins](https://developers.openai.com/codex/plugins) · [workspace import](https://developers.openai.com/codex/enterprise/plugin-management)

## License

[MIT](LICENSE)
