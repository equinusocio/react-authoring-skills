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

For styling conventions, use the `css` skill when present.

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

## Dynamic `style`

- When the component manipulates the `style` object: destructure it from props, build a `dynamicStyle` const typed as `React.CSSProperties`, then pass `dynamicStyle` to the element.
- Decide `useMemo` (or not) for that object when inline style identity would cause excess re-renders.

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
    // or ...style and the end to allow consumer overwrite
  }

  return <div style={dynamicStyle} {...otherProps} />
}
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
- [ ] Prefer project tools over custom/extra scripting
- [ ] Performance considered (memo / Suspense / etc. when warranted)
- [ ] No inline callbacks in JSX — named handlers in body
- [ ] Manipulated `style` → `dynamicStyle: React.CSSProperties` (+ memo when needed)
