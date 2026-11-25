# skills

Agent skills that clean up a codebase without rewriting it. Each skill audits one kind of decay, reports a short numbered plan, and stops. Nothing changes until you pick an item.

| Skill | What it audits |
| --- | --- |
| [`organize`](skills/organize/SKILL.md) | Structure: scattered concepts, shallow folders, pass-through layers, dead files and dependencies, components worth extracting |
| [`design-token`](skills/design-token/SKILL.md) | Styles: raw colors, magic numbers, repeated class chains, copied package CSS |

## Install

**Claude Code plugin**

```text
/plugin marketplace add umurkose/skills
/plugin install skills@umurkose
```

Plugin skills are namespaced: `/skills:organize`, `/skills:design-token`.

**[skills CLI](https://github.com/vercel-labs/skills)**

```sh
npx skills add umurkose/skills
```

**Manual**

Copy the skill folders into `.claude/skills/` in your project, or into `~/.claude/skills/` to use them everywhere:

```sh
git clone https://github.com/umurkose/skills.git
cp -r skills/skills/organize skills/skills/design-token ~/.claude/skills/
```

## Usage

Point a skill at a folder:

```text
/organize web
/design-token src
```

The skill inspects the folder, writes a report, and stops. Apply items by number:

```text
apply 1
apply 1 2
```

Reports come back in the language you write in.

## Examples

<details>
<summary><code>/organize web</code> on a Next.js app</summary>

````markdown
### organize · web

**Summary:** 10 → 10 folders | 40 → 37 files

---

#### Proposed Changes

**1. Remove three leftovers nothing uses**

`sidebar.tsx` is 0 bytes and imported nowhere. `icon.tsx` is imported nowhere and has no `<symbol>` to render; `zustand` is imported by no file.

- **Delete:** `components/layout/sidebar.tsx`
- **Delete:** `components/ui/icon.tsx`
- **Delete:** the `zustand` dependency in `package.json`

**2. Merge the auth actions into one file**

The two files share 8 of 10 meaningful lines, and the `email`/`password` rules (`z.string().min(8)`) are defined twice. Changing the password rule means editing both.

- **Merge:** `app/auth/login/actions.ts` + `app/auth/signup/actions.ts` → `app/auth/actions.ts`

---

#### Target Folder Structure

```text
web/
├── app/
│   ├── api/
│   └── auth/
│       ├── actions.ts
│       ├── login/
│       │   ├── form.tsx
│       │   └── page.tsx
│       └── signup/
│           ├── form.tsx
│           └── page.tsx
├── components/
│   ├── layout/
│   │   ├── footer.tsx
│   │   ├── header.tsx
│   │   └── theme-toggle.tsx
│   └── ui/              22 files (without icon.tsx)
└── core/
```

**Keep:** `core/`, the page → form → action split (metadata needs a server page, `useActionState` a client form) and the `Form`/`Text` primitives stay as they are.

**Next:** Type `apply 1` (or `apply 1 2`) to execute.
````

</details>

<details>
<summary><code>/design-token web</code> on the same app</summary>

````markdown
### design-token · web

**Summary:** 2 raw colors | 4 magic numbers | 1 repeated pattern

---

#### Proposed Changes

**1. Point the self-referencing font token at Geist**

`--font-sans: var(--font-sans)` in `app/globals.css` references itself, so `font-sans` and `font-heading` resolve to no font and the Geist Sans that `layout.tsx` loads is never used. This is the only item that changes rendering: text moves from the browser's default font to Geist Sans.

- **Replace:** `--font-sans: var(--font-sans)` → `--font-sans: var(--font-geist-sans)` in `app/globals.css`

---

#### Target Token Map

```text
app/globals.css
├── Fonts: --font-sans → --font-geist-sans, --font-mono → --font-geist-mono, --font-heading → --font-sans
├── Colors: shadcn semantic set (:root + .dark), unchanged
├── Radii: --radius and its sm…4xl steps, unchanged
└── Utilities: @utility container, unchanged
```

**Keep:** `app/globals.css` is the single token source, package CSS is imported rather than copied, and one-off values below the 3-place threshold stay local.

**Next:** Type `apply 1` to execute.
````

</details>

## Principles

- **Read-only until asked.** Files change only after you pick items.
- **Evidence over intuition.** Every item cites real paths and counts.
- **Your stack stays.** Folder names, libraries and conventions are kept, and no dependencies are added.
- **Vendor code is not your code.** `node_modules`, build output and generated UI kits are not audited as your code.

## License

[MIT](LICENSE)
