# skills

Two Claude Code skills that audit a codebase without rewriting it. Each one reports a short numbered plan and stops. Nothing changes until you pick an item.

| Skill | What it audits |
| --- | --- |
| [`organize`](skills/organize/SKILL.md) | Structure: scattered concepts, shallow folders, pass-through layers, dead files and dependencies, components worth extracting |
| [`design-token`](skills/design-token/SKILL.md) | Styles: raw colors, magic numbers, repeated class chains, copied package CSS |

## Install

```sh
git clone https://github.com/umurkose/skills.git
mkdir -p ~/.claude/skills
cp -r skills/skills/organize skills/skills/design-token ~/.claude/skills/
```

Then restart Claude Code. To use the skills in one project only, copy the two folders into that project's `.claude/skills/` instead.

## Usage

```text
/organize src
/design-token src
```

The skill writes a report and stops. Apply items by number: `apply 1` or `apply 1 2`.

## License

[MIT](LICENSE)
