# Filesystem layout

Apply when scaffolding or reorganizing React component files and folders.

## File layout

- Each component typically has **its own kebab-case folder** with scoped files, plus co-located subcomponents that belong to the same unit and are exported from the same `index`.
- Include **`index.ts`** that exports the component `.tsx` and any props types as needed.
- The folder may also hold the related CSS (`.css` or `.module.css`) depending on the project. For styling rules, use the `authoring-css` skill when present.

```
/my-component
-- index.ts
-- my-component.module.css
-- my-component.tsx
-- my-component-subcomponent.tsx
```

## Checklist

- [ ] Kebab-case folder for the component
- [ ] `index.ts` exports component (+ props types as needed)
- [ ] Co-located subcomponents exported from the same `index`
- [ ] Scoped CSS beside the component when styles are added (`.css` / `.module.css` per project)
