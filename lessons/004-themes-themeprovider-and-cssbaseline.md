# Themes, ThemeProvider, and CssBaseline

## Learning goals

After this chapter, you should be able to:

- Create a Material UI theme and make it available to an application subtree.
- Place ThemeProvider at a useful point in the React tree.
- Explain what CssBaseline changes and when to include it.
- Read the active theme from a component.
- Recognize why multiple unrelated theme providers can cause inconsistent styling.

## What a theme provides

A theme is a shared object containing visual configuration and component behavior. Material UI components read the active theme to determine defaults such as palette values, typography, spacing, breakpoints, shape, shadows, transitions, and component overrides.

Without a custom provider, components use Material UI's default theme. A custom theme lets an application define its own shared visual rules. This keeps recurring decisions in one place instead of repeating the same values across every screen.

## Create a theme

Use `createTheme` from Material UI's styles package:

```jsx
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: {
      main: '#1565c0',
    },
  },
});

export default theme;
```

`createTheme` fills in defaults for omitted theme options. You can add palette, typography, spacing, breakpoint, component, and other configuration as needed. The theme should be a stable module value rather than recreated on every render of an application component.

## Provide the theme

`ThemeProvider` makes the theme available to descendant components through React context. Put it above the components that need the theme, usually near the application root:

```jsx
import { ThemeProvider } from '@mui/material/styles';
import theme from './theme';

function Root() {
  return (
    <ThemeProvider theme={theme}>
      <App />
    </ThemeProvider>
  );
}
```

A component outside that provider cannot read the custom theme. If an application has multiple React roots, each root needs its own provider. In a normal single-root app, one provider near the entry point creates a clear source of theme configuration.

`ThemeProvider` can be nested for a subtree that needs a different theme. The inner provider takes precedence for its descendants. Nesting is useful for intentional theme boundaries, but accidental nested providers can make siblings look inconsistent.

## Read the theme

Inside a function component, `useTheme` reads the active theme:

```jsx
import { useTheme } from '@mui/material/styles';

function ThemeNote() {
  const theme = useTheme();

  return <p>Primary color: {theme.palette.primary.main}</p>;
}
```

Many Material UI styling APIs receive the theme automatically, so a component often does not need to call this hook. Use `useTheme` when component logic needs to read a theme value. For styles, prefer the theme-aware styling APIs covered in later chapters.

## What CssBaseline does

`CssBaseline` applies a global baseline for document and browser styles. It removes the default body margin and establishes consistent base styling used by Material UI, including box sizing and typography behavior. It helps reduce common differences between browser defaults.

Include it once near the top of the themed application:

```jsx
import CssBaseline from '@mui/material/CssBaseline';

<ThemeProvider theme={theme}>
  <CssBaseline />
  <App />
</ThemeProvider>
```

CssBaseline is not a complete product stylesheet and does not design every element. It does not choose page spacing, layout, content hierarchy, or every semantic element's appearance. Those choices belong to the app's theme and components.

Adding CssBaseline multiple times in nested routes is usually unnecessary because it contributes global styles. Place it at the application boundary unless the app has a specific document structure that requires another approach.

## Provider and baseline order

The theme provider should surround CssBaseline and the app so both are within the theme context. A common setup is:

```jsx
<ThemeProvider theme={theme}>
  <CssBaseline />
  <App />
</ThemeProvider>
```

React StrictMode can also wrap the application for development checks. It does not replace the theme provider or baseline.

## Theme configuration versus local styling

Theme configuration defines application-wide defaults or reusable behavior. For example, a primary palette color should generally live in the theme. A one-off margin for a particular page can live next to that page's component.

Keeping this distinction clear avoids two problems:

- Repeating shared brand values makes updates inconsistent.
- Putting every local layout detail into global component overrides makes the theme difficult to understand.

The following chapters cover theme tokens, component-level customization, and local styling in more detail.

## Common mistakes

- **Provider too low in the tree:** components above it cannot access the custom theme.
- **Different themes in sibling sections by accident:** inspect nested ThemeProvider boundaries.
- **Recreating the theme on each render:** define it outside render or memoize it if its options genuinely depend on changing inputs.
- **Expecting CssBaseline to create the whole design:** it establishes baseline styles; it does not build the page.
- **Including the baseline repeatedly:** put global baseline styles at the application boundary.
- **Mixing unrelated providers:** keep styling and theme context boundaries understandable.

## Recap

`createTheme` creates a theme. `ThemeProvider` supplies that theme to descendants. `useTheme` reads the active theme in component logic. `CssBaseline` establishes global baseline styles and should normally appear once near the themed app root. Shared tokens belong in the theme; page-specific layout remains local.

## Practice task

Create a theme module with a primary color. Add ThemeProvider and one CssBaseline at the React application root. Render a Material UI Button and inspect how it changes when the theme is removed or the provider is moved below it.

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
      <td><a href="003-component-structure-and-imports.md">003 - Component structure and imports</a></td>
      <td><a href="../qnas/004-themes-themeprovider-and-cssbaseline.md">Q&amp;A</a></td>
      <td><a href="../examples/004-themes-themeprovider-and-cssbaseline.md">Example</a></td>
      <td><a href="005-theme-tokens-and-design-system.md">005 - Theme tokens and design system</a></td>
    </tr>
  </tbody>
</table>
