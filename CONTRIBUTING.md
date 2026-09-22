# Contributing

Thanks for contributing a plugin to Bluesoft Agent Skills!

## Adding a plugin

1. Create `plugins/<plugin-name>/` with a `plugin.json` manifest. Prefer the
   [Agent Plugins 1.0](https://github.com/agentplugins/agent-plugins-spec/blob/main/spec/1.0.0.md)
   format (declare `"$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json"`)
   so skills and MCP servers stay portable across compatible clients. Use the legacy format
   only if you need custom component paths or Copilot-specific behavior that Agent Plugins
   1.0 doesn't support.
2. Add at least one skill (`skills/<skill-name>/SKILL.md`) or other component. Keep skill
   descriptions front-loaded with the trigger phrases users would type, using a
   `WHEN: "..."` list, so the skill is discoverable.
3. Register the plugin in `.github/plugin/marketplace.json`.
4. Test locally with `copilot plugin install ./plugins/<plugin-name>` and confirm it loads
   via `copilot plugin list` / `/skills list` / `/agent`.

## Plugin naming

- Kebab-case, letters/digits/hyphens only (dots also allowed for Agent Plugins 1.0), max 64
  characters, must start and end with an alphanumeric character.
- Names must be unique within this marketplace.

## Pull requests

- One plugin (or a focused change to an existing plugin) per PR.
- Update `README.md` if you're introducing a notable new plugin.
- Do not commit secrets, tokens, or credentials in plugin code, MCP configs, or hooks.

## Code of conduct

Be respectful and constructive in issues and reviews.
