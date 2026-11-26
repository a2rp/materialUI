# The sx Prop and Theme-Aware Styles

## Learning goals

After this chapter, you should be able to:

- Use the sx prop for local styles that need theme access.
- Distinguish normal CSS values from MUI System's transformed values.
- Write responsive styles and nested selectors.
- Use a theme callback for values that need computation.
- Choose when sx is appropriate and when another styling approach is clearer.

## What sx does

The sx prop is a style object supported by Material UI components and MUI System components such as Box. It accepts normal CSS properties and additional theme-aware properties. MUI System converts the object into CSS using the active theme.

```jsx
import Box from '@mui/material/Box';

<Box
  sx={{
    color: 'primary.main',
    bgcolor: 'background.paper',
    p: 2,
    borderRadius: 1,
  }}
>
  Theme-aware content
</Box>
```

This style uses the palette path for color, the theme spacing scale for padding, and the theme shape scale for border radius. The theme-aware syntax is concise, but values are not all interpreted the same way.

## CSS properties and system transformations

The sx prop accepts CSS properties, but selected keys map values through the theme:

- Palette-aware color properties can refer to paths such as primary.main or text.secondary.
- Spacing properties such as p, px, mt, and margin use the theme spacing scale for numeric values.
- The gap properties use the theme spacing scale.
- borderColor can refer to a palette path.
- borderRadius uses the theme's default radius scale for numeric values.
- boxShadow can refer to a theme shadow index.
- zIndex can refer to a named theme level.
- Typography properties can refer to theme typography values.

Other properties follow their CSS value behavior. For sizing properties, MUI System applies documented transforms; for example, a fractional width between zero and one represents a percentage, while a larger number is treated as a CSS length. Check the relevant system property documentation instead of assuming a number always means pixels or spacing.

Raw CSS values can also be passed where the property accepts them:

```jsx
<Box
  sx={{
    width: 'min(100%, 48rem)',
    border: '1px solid',
    color: 'text.primary',
  }}
/>
```

## Responsive values

A style value can change at theme breakpoints. Use an object keyed by breakpoint name:

```jsx
<Box
  sx={{
    p: { xs: 2, md: 4 },
    display: { xs: 'block', md: 'flex' },
  }}
>
  Responsive content
</Box>
```

A breakpoint's value applies at that breakpoint and larger widths unless overridden later. The responsive values use the active theme's breakpoint configuration. This is generally better than checking viewport width in JavaScript when only the visual style changes.

## Theme callback

Use a callback for the whole sx value when a style needs computed theme information:

```jsx
<Box
  sx={(theme) => ({
    color: theme.palette.primary.main,
    borderColor: theme.palette.divider,
    p: theme.spacing(2),
  })}
/>
```

Current guidance deprecates callback functions as individual property values. Prefer one callback around the whole sx object rather than a callback nested inside one property's value.

Callbacks are not needed for common theme paths. A string such as primary.main is simpler when it directly expresses the desired token.

## Nested selectors and media rules

The sx object can include nested selectors:

```jsx
<Box
  sx={{
    '&:hover': {
      bgcolor: 'primary.dark',
    },
    '& .description': {
      color: 'text.secondary',
    },
  }}
>
  <span className="description">Details</span>
</Box>
```

The ampersand refers to the current component selector. Keep nested selectors local and understandable. For complex styles shared across many components, a reusable styled component or theme override may be clearer.

The sx prop also supports media rules and responsive values. Prefer breakpoint values for ordinary responsive layout. Use a custom media query when the requirement does not match the theme's named breakpoints.

## Combining styles

An sx prop can accept an object, callback, or an array of style values in supported APIs. Arrays are useful when a component needs base styles plus conditional styles. When a custom component accepts an sx prop, preserve and compose the caller's style value instead of discarding it.

Do not spread an arbitrary sx value as though it were always a plain object. It may be a callback or array. Use the documented composition pattern for the installed version.

## Performance and dynamic values

The sx prop is convenient, but it uses runtime styling. For ordinary component styles this is usually an effective tradeoff. In a very large list, or when a CSS value changes to a wide range of values on every render, measure performance before generating a distinct style object for each item.

For highly dynamic values such as live color-picker output, CSS custom properties can keep the structure of the style stable while varying only a value. Validate user-provided style values before applying them. Never treat untrusted strings as safe CSS.

## When to use sx

The sx prop is well suited for:

- A local style on one component.
- A small layout adjustment.
- Responsive styling tied to theme breakpoints.
- Accessing palette, typography, spacing, and other theme values.
- A simple selector that is clearly scoped to the component.

Consider `styled` for a reusable named component with multiple contexts or style variants. Consider theme component overrides for a product-wide default. Use a stylesheet or CSS Modules when the CSS is large, shared with non-MUI elements, or easier to maintain outside JSX.

## Common mistakes

- **Assuming p: 2 is two pixels:** numeric spacing values use the spacing scale.
- **Assuming width: 0.5 is half a pixel:** sizing values have a different documented transform.
- **Using callback values inside individual properties:** use a callback for the whole sx object.
- **Writing long complex styles inline:** move reusable rules to styled or a stylesheet.
- **Targeting private generated classes:** prefer documented slots and local selectors.
- **Passing unvalidated dynamic strings:** validate values before using them as CSS.
- **Using JavaScript for visual-only breakpoints:** responsive CSS values are usually simpler.

## Recap

sx is a local, theme-aware styling API that combines CSS with MUI System transforms. Learn which properties map numeric values through spacing, palette, sizing, or other theme sections. Use responsive values for visual adaptation, one callback for computed theme values, and another styling tool when a style becomes shared or complex.

## Practice task

Build a card with responsive padding, a palette-based heading, and a hover state. Explain why its padding number is a spacing token and why the width value has different numeric rules.

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
      <td><a href="005-theme-tokens-and-design-system.md">005 - Theme tokens and design system</a></td>
      <td><a href="../qnas/006-sx-prop-and-theme-aware-styles.md">Q&amp;A</a></td>
      <td><a href="../examples/006-sx-prop-and-theme-aware-styles.md">Example</a></td>
      <td><a href="007-styled-api-and-reusable-styles.md">007 - The styled API and reusable styles</a></td>
    </tr>
  </tbody>
</table>
