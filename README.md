# react-authoring-skills

Personal skills for syntax preferences and best practices when authoring React components and CSS.

## Install

```bash
npx skills add equinusocio/react-authoring-skills
```

Install one skill:

```bash
npx skills add equinusocio/react-authoring-skills --skill authoring-react
npx skills add equinusocio/react-authoring-skills --skill authoring-css
```

List skills in this repo:

```bash
npx skills add equinusocio/react-authoring-skills --list
```

Local path (while developing):

```bash
npx skills add ./path/to/react-authoring-skills
```

## Skills

| Skill | Path | Description |
| --- | --- | --- |
| `authoring-react` | [`skills/authoring-react`](./skills/authoring-react) | React + TypeScript authoring conventions (auto-applies when writing React/TS UI) |
| `authoring-css` | [`skills/authoring-css`](./skills/authoring-css) | CSS authoring conventions (auto-applies when writing styles) |

## Layout

```
skills/
  authoring-react/SKILL.md
  authoring-react/authoring.md
  authoring-react/filesystem.md
  authoring-css/SKILL.md
  authoring-css/authoring.md
evals/
  authoring-react/
  authoring-css/
```

Each skill is a folder with a `SKILL.md` (YAML frontmatter + instructions) and optional sibling refs. Add more under `skills/<name>/`.
