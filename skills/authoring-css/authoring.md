# CSS authoring

Apply whenever creating or editing stylesheets, CSS modules, or component styles.

## Class names

- Every class selector uses **PascalCase** in the form `.MyClass`.

```css
.MyClass {
  /* ... */
}
```

- Outermost wrapper of a component: if it has a class, that class **matches the component name**.

Example: `stack.tsx` (`const Stack = () => {}`) → root class `.Stack`.

```css
.Stack {
  /* ... */
}
```

- CSS modules: **do not** prefix child classes with the component name (e.g. `.Stack_Content`). Name the element only; nest under the root class:

```css
.Stack {
  /* ... */

  & .Content {
    /* ... */
  }
}
```

## Modern CSS and targets

- Prefer **modern CSS** and syntax that matches **Baseline** (last 2 evergreen browser versions) or the project’s **browserslist**.
- When target is unclear: **ask — do not assume**.
- Prefer modern selectors where they help: `:has()`, `:where()`, `:is()`, etc.

## Nesting

- Always use **native CSS nesting** and prefer the use of & to target parent element. DO not nest everything inside the parent class, but related attributes or variants of the same element

```css
.MyComponentClass {
  color: red;

  &[data-attr="true"] {
    color: blue;
  }
}

.Content {
  color: red;

  &:disabled {
    color: blue;
  }

  .MyComponentClass[data-attr="true"] &{
    color: cyan;
  }
}
```

## Vendor prefixes

- Do **not** write vendor prefixes except for properties autoprefixer cannot cover (e.g. certain `user-select` cases).

## Longhand vs shorthand

- Prefer **longhand** over shorthand where practical.
- Avoid shorthand with **5+ values** but if needed: add a comment explaining what those values are.

```css
.Card {
  /* name | duration | easing | delay | iteration | direction | fill-mode */
  animation: fade-in 200ms ease-out 0s 1 normal forwards;
}
```

## Avoid useless resets

- Do **not** add useless declarations such as `min-inline-size: 0` or `min-block-size: 0` unless they serve a real purpose. Agents tend to sprinkle them everywhere — skip that habit.

## Colors

- Prefer design tokens (`var(--…)`) when they exist.
- When inserting **hardcoded** colors (not tokens), prefer HDR formats **OKLCH** and **OKLAB** — especially for **gradients** (better interpolation).
- When deriving colors or changing transparency from a variable or a hardcoded color: **do not** use `color-mix()`; use **relative colors**. Use color-mix() only to create new color from the combination of two colors.

```css
color: oklch(from var(--my-color) l calc(c + 0.2) h / 20%);
```

## Motion and `@property`

- Prefer animations and transitions on **performant** properties (`transform` and similar compositor-friendly props) when there is an alternative to `opacity` / `filter`.
- Prefer **`@property`** to animate custom-property values.
- When a component needs `@property` registrations: create **`my-component.props.css`** beside the component (kebab name matching the component file), register the props there, and **import** that file from the component’s `.css` / `.module.css`.
- Defaults that **cannot** be set inside `@property` (e.g. `var(...)`) go on the component **root** class.

```css
/* accent-badge.props.css */
@property --accent-angle {
  syntax: "<angle>";
  inherits: false;
  initial-value: 0deg;
}
```

```css
/* accent-badge.module.css */
@import "./accent-badge.props.css";

.AccentBadge {
  --accent-color: var(--color-brand);
}
```

## Checklist

- [ ] Classes `.PascalCase` (`.MyClass`)
- [ ] Root class matches component name (`.Stack` for `Stack`)
- [ ] Module children: element name only (`.Content`), nested — no `Component_` prefix
- [ ] Modern CSS / Baseline or project browserslist; ask when unsure
- [ ] Modern selectors (`:has`, `:is`, `:where`, …) when useful
- [ ] Native nesting always
- [ ] No prefixes autoprefixer can add
- [ ] Longhand preferred; 5+ value shorthands commented
- [ ] No useless `min-*-size: 0`
- [ ] Hardcoded colors: OKLCH/OKLAB (esp. gradients); derive/alpha via relative colors — no `color-mix()`
- [ ] Motion on performant props when possible; `@property` + `*.props.css` when animating custom props; `var()` defaults on root class
