# Color Schemes, Dark Mode, and CSS Variables

## Learning goals

After this chapter, you should be able to:

- Distinguish a fixed dark theme from a switchable light/dark color-scheme setup.
- Enable CSS theme variables and configure light/dark palettes.
- Let the browser follow the user's system preference or provide a manual toggle.
- Apply dark-specific styles without branching on runtime palette mode.
- Avoid common hydration flicker and first-render issues.

## Fixed dark theme or switchable schemes

A theme can use a fixed dark palette by setting `palette.mode` to dark. This is appropriate when the entire application should always use dark styling.

If users can switch between light and dark, configure `colorSchemes` and enable `cssVariables`. This creates the scheme-specific CSS variables so the browser can apply a scheme without requiring every component to be rerendered with a different raw color value.

```jsx
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  cssVariables: true,
  colorSchemes: {
    light: true,
    dark: true,
  },
});
```

Then provide this theme through ThemeProvider. The built-in color scheme can follow the system preference. When colorSchemes is configured, the default mode is system unless the provider receives a different defaultMode.

## CSS theme variables

With cssVariables enabled, Material UI generates CSS custom properties from theme values. The default prefix is --mui. A palette token such as primary.main can be emitted as a variable similar to --mui-palette-primary-main.

This lets the browser switch the active values through CSS. The theme exposes a vars tree for styles that need direct access to the generated variables:

```jsx
<Box
  sx={(theme) => ({
    color: theme.vars.palette.text.primary,
    bgcolor: theme.vars.palette.background.paper,
  })}
/>
```

Use theme-aware tokens rather than hard-coding the generated variable names when the style is written in JavaScript. In plain CSS, a CSS variable can be used directly if the app's variable prefix and selector are known.

The vars object is generated and managed by Material UI. Do not provide your own vars field to createTheme.

## Follow the system preference

With light and dark color schemes plus cssVariables enabled, Material UI can use the browser's prefers-color-scheme setting. This is suitable when the application should follow the user's operating-system or browser preference.

System-following CSS variables can work with server rendering without waiting for React to choose a mode because the browser applies the matching CSS media query.

## Add a manual switch

For a user-controlled mode, choose a selector such as class or data and use useColorScheme:

```jsx
import { useColorScheme } from '@mui/material/styles';

function ColorModeSelect() {
  const { mode, setMode } = useColorScheme();

  if (!mode) {
    return null;
  }

  return (
    <label>
      Color mode
      <select
        value={mode}
        onChange={(event) => setMode(event.target.value)}
      >
        <option value="system">System</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
      </select>
    </label>
  );
}
```

The mode can be undefined during the initial render while the provider initializes. Render a stable placeholder or avoid reading mode-dependent content until it is available. In TypeScript, type or narrow the selected value to the supported mode values.

## Choose the scheme selector

The cssVariables configuration supports selectors such as:

- media, which follows the system preference.
- class, which applies a scheme class to the document root.
- data, which applies a scheme data attribute.
- A custom selector pattern.

The selector must match the application's styling setup. For a manual toggle, use a selector that React can update rather than a selector that only follows a media query.

## Style a component for dark mode

Use theme.applyStyles for mode-specific style rules:

```jsx
<Card
  sx={[
    {
      bgcolor: 'background.paper',
      color: 'text.primary',
    },
    (theme) =>
      theme.applyStyles('dark', {
        borderColor: theme.vars.palette.grey[800],
      }),
  ]}
/>
```

Avoid checking theme.palette.mode inside a render-time conditional to choose colors. That can cause a server and client to render different styles before the user's selected mode is known. applyStyles expresses the scheme-specific styles in the generated CSS.

When cssVariables is enabled, applyStyles has a defined specificity relationship with styles outside it. If a dark-mode rule needs an override, use the same mode-aware styling mechanism for the override.

## Server-rendered applications

A server cannot read a browser's saved color scheme preference. In an SSR application with a manual class or data selector, the page can briefly render with the wrong scheme before hydration.

Material UI provides InitColorSchemeScript to set the selector before the React app renders. Place it in the framework's document or root layout before the application content. Its attribute and default mode must match the theme and ThemeProvider configuration. Frameworks may also require an HTML hydration-warning setting for the attribute updated by the script.

For client-only applications, this script is generally unnecessary. Follow the server-rendering guidance for the framework and router used by the project.

## Color scheme and native controls

CSS color-scheme also informs the browser how to render native controls and scrollbars. Material UI theme variables can emit this property based on the active scheme. Avoid globally styling browser scrollbars unless the application has reviewed the accessibility and platform effects.

## Common mistakes

- **Setting only palette.mode when users need a toggle:** use colorSchemes and CSS variables for switchable schemes.
- **Using a media selector and expecting a manual toggle:** media follows the system preference.
- **Reading mode before initialization:** mode can be undefined initially.
- **Branching on theme.palette.mode in rendered styles:** use theme.applyStyles to avoid hydration flicker.
- **Forgetting the root selector script in SSR:** manual schemes may flicker until React hydrates.
- **Mismatching InitColorSchemeScript and ThemeProvider configuration:** keep selector and default mode aligned.
- **Hard-coding generated CSS variable names in components:** use theme.vars paths.

## Recap

A fixed dark theme and a switchable color-scheme system solve different problems. For switchable modes, enable cssVariables, define light and dark colorSchemes, and use useColorScheme when users need a toggle. Use theme.applyStyles for mode-specific CSS. SSR apps with manual selectors need early initialization to avoid flicker.

## Practice task

Add a color-mode selector with System, Light, and Dark options. Confirm that the theme uses CSS variables, test the initial mode and each choice, then document what SSR initialization would be required if the same app were server-rendered.

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
      <td><a href="009-responsive-design-and-breakpoints.md">009 - Responsive design and breakpoints</a></td>
      <td><a href="../qnas/010-color-schemes-dark-mode-and-css-variables.md">Q&amp;A</a></td>
      <td><a href="../examples/010-color-schemes-dark-mode-and-css-variables.md">Example</a></td>
      <td><a href="011-typography.md">011 - Typography</a></td>
    </tr>
  </tbody>
</table>
