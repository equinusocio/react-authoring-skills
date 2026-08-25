# React components

Apply whenever creating or editing React components.

## Shape

- Components are always **named `const` arrow functions** (export named or not as needed).

```tsx
const MyComponent = () => (
  // ...
)

const MyComponent = () => {
  // ...
}
```

- Every component is typed with **`React.FC<>`** or **`FC<>`** (match the project’s import pattern), with props passed to the generic:

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
