# Bluesoft Agent Skills

A public [GitHub Copilot CLI](https://docs.github.com/copilot/how-tos/use-copilot-agents/use-copilot-cli)
**plugin marketplace**. It hosts plugins (skills, agents, MCP servers, and other Copilot CLI
customizations) maintained by Bluesoft, packaged so anyone can discover and install them
with a single command.

## Install this marketplace

```shell
copilot plugin marketplace add <owner>/bluesoft-agent-skills-public
```

Replace `<owner>` with the GitHub org/user this repo is pushed under. Once registered, browse
and install plugins from it:

```shell
copilot plugin marketplace browse bluesoft-agent-skills
copilot plugin install skill-scaffold@bluesoft-agent-skills
```

(Or, in an interactive `copilot` session: `/plugin marketplace add <owner>/bluesoft-agent-skills-public`.)

## Repository structure

```
.github/plugin/marketplace.json   # Marketplace manifest (required) — lists all published plugins
plugins/                          # One directory per plugin
  skill-scaffold/                 # Example plugin: scaffolds new skills/plugins for this repo
    plugin.json                   # Agent Plugins 1.0 manifest
    skills/
      new-skill/
        SKILL.md
```

Copilot CLI recognizes this repository as a plugin marketplace purely because
`.github/plugin/marketplace.json` exists. Each entry in its `plugins` array points at a
directory under `plugins/` that contains a `plugin.json` manifest.

## Adding a new plugin

1. Run the bundled `skill-scaffold` plugin's `new-skill` skill to generate the layout, or do
   it by hand:
   - Create `plugins/<plugin-name>/plugin.json` (use the [Agent Plugins 1.0](https://github.com/agentplugins/agent-plugins-spec/blob/main/spec/1.0.0.md)
     format shown in `plugins/skill-scaffold/plugin.json` for portability across clients).
   - Add a skill at `plugins/<plugin-name>/skills/<skill-name>/SKILL.md`.
   - For Copilot-specific extras (custom agents, hooks, slash commands, LSP servers), put
     them under `plugins/<plugin-name>/com.github.copilot/`.
2. Register the plugin in `.github/plugin/marketplace.json` (add an object with `name`,
   `description`, `version`, and `source` to the `plugins` array).
3. Test locally:
   ```shell
   copilot plugin install ./plugins/<plugin-name>
   copilot plugin list
   ```
   Or test the full marketplace flow from the repo root:
   ```shell
   copilot plugin marketplace add .
   copilot plugin install <plugin-name>@bluesoft-agent-skills
   ```
4. Open a PR. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Publishing / pushing to GitHub

This repo is a local git repository with no remote configured yet. To publish it:

```shell
# Create the GitHub repo (choose the right org), then:
git remote add origin https://github.com/<owner>/bluesoft-agent-skills-public.git
git branch -M main
git push -u origin main
```

Once pushed, anyone can install it with `copilot plugin marketplace add <owner>/bluesoft-agent-skills-public`.

## Reference

- [Creating a plugin for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-creating)
- [Creating a plugin marketplace for GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-marketplace)
- [Finding and installing plugins](https://docs.github.com/en/copilot/how-tos/copilot-cli/customize-copilot/plugins-finding-installing)
- [CLI plugin reference (`plugin.json` / `marketplace.json` schemas)](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference)

## License

[MIT](LICENSE)
