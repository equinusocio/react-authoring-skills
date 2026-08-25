# react-authoring-skills

Personal skills for syntax preferences and best practices when authoring React components and CSS.

## Install

```bash
npx skills add equinusocio/react-authoring-skills
```

Install one skill:

```bash
npx skills add equinusocio/react-authoring-skills --skill react-ts
npx skills add equinusocio/react-authoring-skills --skill css
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
| `react-ts` | [`skills/react-ts`](./skills/react-ts) | React + TypeScript authoring conventions (load when writing React/TS UI) |
| `css` | [`skills/css`](./skills/css) | CSS authoring conventions (load when writing styles) |

## Layout

```
skills/
  react-ts/SKILL.md
  react-ts/authoring.md
  react-ts/filesystem.md
  css/SKILL.md
  css/authoring.md
evals/
  react-ts/
  css/
```

Each skill is a folder with a `SKILL.md` (YAML frontmatter + instructions) and optional sibling refs. Add more under `skills/<name>/`.
