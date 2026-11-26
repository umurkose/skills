# skills

AI skills that audit a codebase without rewriting it. Each one reports a short numbered plan and stops. Nothing changes until you pick an item.

Each skill is a plain `SKILL.md` file, so it works with any AI agent that supports skills.

| Skill | What it audits |
| --- | --- |
| [`organize`](skills/organize/SKILL.md) | Structure: scattered concepts, shallow folders, pass-through layers, dead files and dependencies, components worth extracting |
| [`design-token`](skills/design-token/SKILL.md) | Styles: raw colors, magic numbers, repeated class chains, copied package CSS |

## Install

```sh
git clone https://github.com/umurkose/skills.git
```

Copy the folders inside `skills/skills/` into the folder your agent loads skills from, then restart the agent.

## Usage

Ask your agent to run a skill on a folder, for example:

```text
/organize src
/design-token src
```

The skill writes a report and stops. Apply items by number: `apply 1` or `apply 1 2`.

## License

[MIT](LICENSE)
