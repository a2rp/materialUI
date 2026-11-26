# CSS Overrides and Global Styles

## Learning goals

After this chapter, you should be able to:

- Choose an override strategy based on how broadly a style should apply.
- Target documented component slots and state classes safely.
- Avoid unstable generated class names.
- Apply document-wide styles with GlobalStyles or CssBaseline overrides.
- Understand CSS Modules, style order, and CSS cascade layers.

## Choose the narrowest suitable strategy

Material UI supports several customization scopes:

| Scope | Suitable approach |
| --- | --- |
| One instance | sx |
| Reusable component | styled |
| All instances of a Material UI component in the app | Theme component overrides |
| Document-level or non-MUI global rules | GlobalStyles, CssBaseline overrides, or a stylesheet |

Start with the narrowest scope that meets the requirement. Broad overrides are harder to reason about because they can affect unrelated pages and states.

## Theme component overrides

Use the theme components section when an application-wide rule should apply to a Material UI component. The component key uses the theme name, and styleOverrides keys identify component slots.

```jsx
const theme = createTheme({
  components: {
    MuiButton: {
      styleOverrides: {
        root: {
          textTransform: 'none',
          borderRadius: 8,
        },
      },
    },
  },
});
```

Use the documented slot names from that component's API. The root slot is the component's outermost part. More complex components can expose slots such as input, label, thumb, or paper. Component APIs list their supported slots and class names.

Theme component configuration is useful for consistent defaults and shared variants. It is not a substitute for every page's layout.

## Stable slot classes and state classes

Material UI exposes stable global class names for component slots, such as `.MuiButton-root`. These are different from generated Emotion classes that contain a changing hash. Use documented global classes when writing CSS selectors.

State classes represent conditions such as disabled, selected, focused, expanded, checked, or error. They behave like pseudo-classes for specificity. Target a state class together with the relevant component slot:

```css
.MuiButton-root.Mui-disabled {
  color: #666;
}
```

Avoid styling a generic state class alone:

```css
/* Too broad: can affect unrelated components. */
.Mui-disabled {
  color: #666;
}
```

For a local override, the same documented slot can be targeted from sx:

```jsx
<Button
  disabled
  sx={{
    '&.Mui-disabled': {
      color: 'text.disabled',
    },
  }}
>
  Unavailable
</Button>
```

Do not select an implementation class such as `.css-ae2u5c-MuiButton-root`. The hash can change between builds and releases. Avoid relying on undocumented internal DOM structure.

## GlobalStyles

Use `GlobalStyles` for a small set of document-wide CSS rules:

```jsx
import GlobalStyles from '@mui/material/GlobalStyles';

<GlobalStyles
  styles={(theme) => ({
    body: {
      margin: 0,
      backgroundColor: theme.palette.background.default,
    },
    '::selection': {
      backgroundColor: theme.palette.primary.light,
    },
  })}
/>
```

Keep global selectors limited. A global rule can affect every route and component, so it should represent a genuine document-level requirement. If GlobalStyles is rendered from a component that re-renders often, hoist a static GlobalStyles element when its styles do not change.

## Extend CssBaseline

If an application already uses CssBaseline, global baseline adjustments can be configured through the MuiCssBaseline theme slot:

```jsx
const theme = createTheme({
  components: {
    MuiCssBaseline: {
      styleOverrides: (theme) => ({
        body: {
          backgroundColor: theme.palette.background.default,
        },
      }),
    },
  },
});
```

This keeps baseline-related styles in the same theme setup. Do not render a second CssBaseline just to add one global rule.

## External CSS and CSS Modules

A regular stylesheet can target documented global MUI classes. CSS Modules scope class names locally, so a MUI class may need to be declared global using the CSS Modules syntax supported by the bundler.

```css
/* Example syntax used by CSS Modules implementations that support :global. */
.card :global(.MuiButton-root) {
  justify-content: flex-start;
}
```

The exact CSS Modules configuration depends on the build tool. Keep the component selector and the MUI slot selector together so the rule stays scoped to the intended region.

## Specificity and style order

If a valid CSS rule does not win, inspect the browser's computed styles. Check selector specificity, source order, active state classes, and the styling engine's insertion order. Prefer a documented customization API over increasing specificity blindly.

Avoid `!important` as a first fix. It often hides a selector or order problem and makes later overrides harder. If integrating a CSS framework, use the supported style-order or cascade-layer integration for the installed versions.

Material UI v9 supports CSS cascade layers. For a single-page app, CSS layer output can be enabled through the styling provider. A layer order can then coordinate MUI styles with other CSS frameworks. This is an integration choice for the whole app; enabling layers can change existing style precedence, so review and test affected overrides rather than adding it casually.

## Practical debugging process

1. Inspect the element and computed style in browser developer tools.
2. Identify the documented component slot and its stable class.
3. Check whether the component is in a state such as disabled or selected.
4. Find which rule wins and why.
5. Move the rule to the right scope: local sx, reusable styled component, theme override, or global CSS.
6. Confirm that other component instances remain correct.

## Common mistakes

- **Targeting generated hash classes:** use the stable documented slot class.
- **Styling a state class by itself:** pair the state with the component slot.
- **Using a global override for one screen:** use a local style.
- **Using GlobalStyles for every component detail:** keep global CSS limited to real document-level rules.
- **Adding !important before checking specificity:** inspect the computed cascade first.
- **Enabling CSS layers without checking existing overrides:** layer order changes precedence.
- **Assuming CSS Modules can see MUI classes directly:** use the bundler's global selector support.

## Recap

Use sx for local changes, styled for repeated components, theme overrides for component-wide rules, and global styles for document-level needs. Target documented slot and state classes, never generated hashes. Inspect specificity and style order before escalating a selector, and treat CSS layers as an intentional application-wide integration.

## Practice task

Set a product-wide Button rule in the theme. Then add a local style for one Button's disabled state. Inspect the result and explain why the state selector includes both the Button root and the disabled class.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Chapter example</th>
      <th>Next lesson</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="007-styled-api-and-reusable-styles.md">007 - The styled API and reusable styles</a></td>
      <td><a href="../qnas/008-css-overrides-and-global-styles.md">Q&amp;A</a></td>
      <td><a href="../examples/008-css-overrides-and-global-styles.md">Example</a></td>
      <td><a href="009-responsive-design-and-breakpoints.md">009 - Responsive design and breakpoints</a></td>
    </tr>
  </tbody>
</table>
