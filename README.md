# qa-ai-tools (GitHub Copilot)

A GitHub Copilot plugin marketplace with QA workflow plugins. This is the Copilot version of [qa_ai_tools](https://github.com/atlusine/qa_ai_tools) (Claude Code); the skills are the same, only the packaging differs.

## Structure

```
qa-ai-tools-copilot/
├── .github/
│   └── plugin/
│       └── marketplace.json          # catalog of all plugins
├── plugins/
│   ├── qa-test-design/               # Lesson 2
│   │   ├── plugin.json
│   │   └── skills/
│   │       └── manual-test-cases/
│   │           ├── SKILL.md
│   │           └── references/       # templates, techniques, examples
│   ├── qa-bug-investigation/         # Lesson 3 (later)
│   └── qa-automation/                # Lessons 4–5: skills + custom agents
│       └── com.github.copilot/agents/
└── README.md
```

Plugins use the [Agent Plugins 1.0](https://agent-plugins.org) layout: `plugin.json` at the plugin root, skills in `skills/<name>/SKILL.md`, and Copilot-specific pieces (custom agents, hooks) under `com.github.copilot/`.

## Plugins

| Plugin | Status | What it does |
|--------|--------|--------------|
| `qa-test-design` | ✅ Ready | `manual-test-cases` skill: designs manual test cases from requirements using EP, BVA, decision tables and state transitions, with a traceability matrix |
| `qa-bug-investigation` | 🚧 Lesson 3 | Bug investigation and reporting |
| `qa-automation` | 🚧 Lessons 4–5 | Test automation skills and agents |

Only finished plugins are listed in `marketplace.json`. Add a plugin there when it has content.

## Install

### Copilot CLI

From a Git remote:

```
copilot plugin marketplace add <owner/repo>
copilot plugin install qa-test-design@qa-ai-tools
```

From a local clone (single plugin):

```
copilot plugin install ./plugins/qa-test-design
```

Check it loaded: `copilot plugin list`, then `/skills list` in a session.

### VS Code / Copilot in the IDE (without plugins)

Copy the skill folder into one of the locations Copilot reads skills from:

- **One repository:** `.github/skills/manual-test-cases/`
- **All your projects:** `~/.copilot/skills/manual-test-cases/`

## Usage

Ask Copilot (agent mode) something like:

- "Write test cases for this user story: …"
- "What should I test for JIRA-123?"
- "Give me test cases for the login form as CSV for TestRail"

## Adding a plugin

1. Create `plugins/<name>/plugin.json` (with `"$schema": "https://agent-plugins.org/schemas/1.0.0/plugin.schema.json"`).
2. Add skills under `plugins/<name>/skills/<skill>/SKILL.md` and custom agents under `plugins/<name>/com.github.copilot/agents/<agent>.agent.md`.
3. Register it in `.github/plugin/marketplace.json`.
4. Test locally: `copilot plugin install ./plugins/<name>`, then `/skills list` and `/agent`.
