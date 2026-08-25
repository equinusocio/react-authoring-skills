# React components

Apply whenever creating or editing React components.

## Shape

- Components are always **named `const` arrow functions** (export named or not as needed).

```tsx
const MyComponent = () => (
  // ...
)

export const MyComponent = () => {
  // ...
}
```

- Every component is typed with **`React.FC<>`** or **`FC<>`** (match the project’s import pattern) when they have props, with props passed to the generic. Otherwhise just `React.FC` or `FC`

```tsx
const MyComponent: React.FC<MyComponentProps> = () => (
  // ...
)
```

## Props type

- Every component has a **`ComponentNameProps`** type. Export it when the component is reused elsewhere or callers need to infer the original props; otherwise export is optional.
- Prefer props that **extend the HTML (or component) props of the outermost wrapper** — the element/component that receives the props spread.
- Use **`ComponentPropsWithRef`** / **`ComponentPropsWithoutRef`** as appropriate. In React 19, **`ref` is a normal prop** (no `forwardRef` required for that reason alone).

```tsx
export type MyComponentProps = React.ComponentPropsWithRef<'div'> & {
  // ...
}

export type MyComponentProps = React.ComponentPropsWithRef<typeof OtherComponent> & {
  // ...
}
```

Use `ComponentPropsWithoutRef` when the wrapper must not accept `ref`.

## Destructuring and spread

- Destructure props. Prefer spreading the rest onto the wrapper for props not handled directly.
- Place the spread so it either **preserves defaults** or **lets callers override** — choose deliberately.

```tsx
type MyComponentProps = {
  prop1: string
  prop2?: string
}

const MyComponent: React.FC<MyComponentProps> = ({
  prop1,
  ...otherProps
}) => <div data-prop={prop1} {...otherProps} />
```

## Default values

- Prefer **default values in the parameter list** (including when combined with spread):

```tsx
type MyComponentProps = {
  prop1?: string
  prop2?: string
}

const MyComponent: React.FC<MyComponentProps> = ({
  prop1 = 'default',
  ...otherProps
}) => <div data-prop={prop1} {...otherProps} />
```

## Markup branching

- Prefer **`&&`** when a branch returns `null` (render nothing).
- Prefer a **ternary** when both branches render something — **never nest** ternaries.

```tsx
return (
  <div>
    {condition && <p />}
    {condition2 ? <p /> : <figure />}
  </div>
)
```

## Nullish coalescing

- Prefer **nullish coalescing** (`??`) where possible.

```tsx
const myConst = condition ?? condition2
```

## CSS imports

- CSS modules: import as `styles`.
- Plain CSS: side-effect import (no binding).

```tsx
import styles from './my-component.module.css'

const MyComponent: React.FC = () => <div className={styles.MyClass} />
```

```tsx
import './my-component.css'

const MyComponent: React.FC = () => <div className="MyComponent" />
```

For styling conventions, use the `authoring-css` skill when present.

## Prefer project tools

- Avoid custom code or excessive scripting when project tools already cover the need and can shrink the code.

## Performance

- Keep React code performant for re-renders, loading, and data fetching.
- Evaluate when to use `useMemo`, `useCallback`, `React.memo`, `useOptimistic`, `Suspense`, and similar — apply them when they reduce real cost, not by default everywhere. Prioritize performant UX (reactiveness) and optimistic loadings.

## Event handlers

- Never write callback functions inline in the markup.
- Declare them in the component body as **named arrow functions** with relevant memoizing and deps when necessary.

```tsx
const MyComponent: React.FC<MyComponentProps> = ({
  prop1 = 'default',
  ...otherProps
}) => {
  const handleClick = () => {}

  return <div onClick={handleClick} {...otherProps} />
}
```

## className on the outer wrapper

- If the outermost wrapper gets a CSS class: destructure `className` from props and apply it on that element.
- If the project has a class-merge utility (`clsx`, `cn`, etc.), use it. Otherwise **do not** destructure `className` — let it pass through the spread.

```tsx
const MyComponent: React.FC<MyComponentProps> = ({
  className,
  ...otherProps
}) => <div className={clsx(styles.MyComponent, className)} {...otherProps} />
```

## Dynamic `style` and custom attributes

- Prefer controlling CSS via **custom HTML attributes** (`data-*`) and **`dynamicStyle`**.
- When the component manipulates `style`: destructure it from props, build `dynamicStyle` as `React.CSSProperties`, pass it to the element.
- Decide `useMemo` (or not) when inline style identity would cause excess re-renders.
- Place `...style` first or last deliberately (defaults vs consumer overwrite).

```tsx
const MyComponent: React.FC<MyComponentProps> = ({
  style,
  amount,
  full,
  ...otherProps
}) => {
  const dynamicStyle: React.CSSProperties = {
    ...style,
    ...(amount && !full && { '--vui-bleed-amount': `var(--space-${amount})` }),
    // or ...style at the end to allow consumer overwrite
  }

  // [data-prop] is then used in css to customize style
  return <div style={dynamicStyle} data-prop={prop1} {...otherProps} />
}
```

## `data-*` attribute values

- Custom HTML attributes (`data-*`) always receive the strings **`"true"`** or **`"false"`**.
- Do **not** toggle attribute presence with booleans (`<div {...(bool && { "data-prop": bool })} />`).

```tsx
// data-prop becomes [data-prop="true"] or [data-prop="false"].
<div style={dynamicStyle} data-prop={prop1} {...otherProps} />
```

Folder and file placement: see [`filesystem.md`](filesystem.md).

## Checklist

- [ ] `const` named arrow function
- [ ] `React.FC` / `FC` with props generic
- [ ] `ComponentNameProps` (+ export if reuse/inference)
- [ ] Extends `ComponentPropsWithRef` / `WithoutRef` of the outer wrapper when spreading
- [ ] Destructure + residual spread; spread order intentional
- [ ] Defaults in param list when possible
- [ ] Markup: `&&` for null branch; flat ternary otherwise
- [ ] Prefer `??` where applicable
- [ ] CSS modules → `styles` import; plain CSS → side-effect import
- [ ] Outer wrapper `className`: merge with project util, else leave on spread
- [ ] Prefer project tools over custom/extra scripting
- [ ] Performance considered (memo / Suspense / etc. when warranted)
- [ ] No inline callbacks in JSX — named handlers in body
- [ ] Prefer `data-*` + `dynamicStyle: React.CSSProperties` (+ memo when needed)
- [ ] `data-*` values are `"true"` / `"false"` strings, not booleans
