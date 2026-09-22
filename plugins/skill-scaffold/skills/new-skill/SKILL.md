---
name: new-skill
description: Scaffold a new skill or plugin for the Bluesoft Agent Skills marketplace, generating the correct plugin.json/SKILL.md layout and registering it in marketplace.json. WHEN: "create a new skill", "add a plugin to the marketplace", "scaffold SKILL.md", "publish a skill to bluesoft-agent-skills".
---

# New skill / plugin scaffolder

Use this skill to add a new skill (packaged as an Agent Plugins 1.0 plugin) to this
marketplace repository.

## Steps

1. Ask the user for:
   - A kebab-case plugin name (e.g. `deploy-helper`). This becomes the directory under `plugins/`.
   - A kebab-case skill name (usually the same as the plugin name unless the plugin will
     contain multiple skills).
   - A one-sentence description, written so it front-loads the trigger phrases a user would
     type (used for skill discovery). Follow the `WHEN: "..."` convention used elsewhere in
     this repo.
   - Optional: author name/email, license, keywords, version (default `1.0.0`).

2. Create the plugin manifest at `plugins/<plugin-name>/plugin.json`:

   ```json
   {
     "$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json",
     "name": "<plugin-name>",
     "description": "<short description>",
     "version": "1.0.0",
     "author": { "name": "<author>", "email": "<email>" },
     "license": "MIT",
     "keywords": ["<keyword1>", "<keyword2>"]
   }
   ```

3. Create the skill at `plugins/<plugin-name>/skills/<skill-name>/SKILL.md` with YAML
   frontmatter (`name`, `description`) followed by clear, actionable instructions for the
   agent that will execute the skill. Keep instructions imperative and concrete, and include
   a "WHEN:" trigger list in the description so the skill is discoverable.

4. If the plugin needs Copilot-specific components (custom agents, hooks, slash commands,
   LSP servers), put them under `plugins/<plugin-name>/com.github.copilot/` (for example
   `com.github.copilot/agents/NAME.agent.md`). Portable MCP servers go in a root
   `plugins/<plugin-name>/mcp.json` declaring
   `"$schema": "https://agent-plugins.org/schemas/1.0.0/mcp.schema.json"`.

5. Register the plugin in `.github/plugin/marketplace.json` by appending an entry to the
   top-level `plugins` array:

   ```json
   {
     "name": "<plugin-name>",
     "description": "<short description>",
     "version": "1.0.0",
     "source": "plugins/<plugin-name>"
   }
   ```

6. Validate locally before committing:
   - `copilot plugin install ./plugins/<plugin-name>` to install directly, or
   - `copilot plugin marketplace add .` (from the repo root) followed by
     `copilot plugin install <plugin-name>@bluesoft-agent-skills` to test the full
     marketplace flow.
   - Run `/skills list` and `/agent` in an interactive session to confirm the new
     components loaded.

7. Update `README.md`'s plugin list if the repo maintains one, then commit the new
   `plugins/<plugin-name>/` directory and the `marketplace.json` change together.
