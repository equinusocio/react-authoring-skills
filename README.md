# react-authoring-skills

Personal skills for syntax preferences and best practices when authoring React components and CSS.

## Install

```bash
npx skills add equinusocio/react-authoring-skills
```

Install one skill:

```bash
npx skills add equinusocio/react-authoring-skills --skill react-components
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
| `react-components` | [`skills/react-components`](./skills/react-components) | React component authoring preferences |
| `css` | [`skills/css`](./skills/css) | CSS authoring preferences |

## Layout

```
skills/
  react-components/SKILL.md
  css/SKILL.md
```

Each skill is a folder with a `SKILL.md` (YAML frontmatter + instructions). Add more under `skills/<name>/`.
