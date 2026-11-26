# Color Schemes, Dark Mode, and CSS Variables - Q&A

## 1. What is the difference between a dark theme and a color-scheme system?

A fixed dark theme always uses dark palette values. A color-scheme system can provide light and dark values and let the browser or user choose the active scheme.

## 2. How do I configure switchable light and dark schemes?

Enable CSS variables and define the available schemes:

```jsx
const theme = createTheme({
  cssVariables: true,
  colorSchemes: {
    light: true,
    dark: true,
  },
});
```

Wrap the application with ThemeProvider using this theme.

## 3. How does the browser choose a mode if I add colorSchemes?

By default, the system preference is used when colorSchemes is configured. The ThemeProvider can set a different defaultMode such as light or dark.

## 4. What does cssVariables: true do?

It generates CSS custom properties for theme values and lets the browser apply scheme-specific styles. The default variable prefix is --mui.

## 5. What is theme.vars?

It is the generated variable access tree when CSS theme variables are enabled. Its paths mirror theme values and resolve to CSS custom properties. Material UI manages this field; do not supply it in createTheme.

## 6. How do I read the active mode in a component?

Use useColorScheme from the Material UI styles package. Its mode may be undefined during initialization, so render a stable placeholder or guard mode-dependent UI until it is available.

## 7. Why is mode undefined at first?

The provider needs to initialize the stored or system-selected mode. This can happen after the initial React render, especially in server-rendered applications.

## 8. Can a media selector support a manual toggle?

No. A media selector follows the browser's system preference. For a manual toggle, choose a class, data, or custom selector that the app can update.

## 9. Should I use theme.palette.mode === 'dark' in my sx callback?

Avoid it for scheme-dependent styling. In SSR, the server may not know the user's selected mode, so runtime branching can create a mismatch or flicker. Use theme.applyStyles('dark', styles) instead.

## 10. What does theme.applyStyles do?

It adds CSS styles for a named scheme, such as dark. The browser can apply those styles based on the scheme selector without choosing colors through a runtime render conditional.

## 11. Do CSS variables work with system dark mode and SSR?

The built-in media-based scheme can be applied by the browser using CSS and can work with SSR without React first selecting the mode. A manual saved choice in SSR needs earlier initialization.

## 12. What is InitColorSchemeScript for?

It updates the document's scheme selector before the React app renders. This can prevent the page from briefly displaying the wrong scheme in a server-rendered app with a manual selector.

## 13. Where should the initialization script go?

Place it in the framework's document or root layout before the application content. Use the attribute and default mode that match the theme and ThemeProvider configuration.

## 14. Does a client-only Vite app need InitColorSchemeScript?

Usually not. There is no server-rendered HTML to flicker before hydration. Use the script when the application's rendering setup and selected scheme require early document initialization.

## 15. Can CSS variables be used in a regular CSS file?

Yes. A stylesheet can reference generated variables with var(), provided the variable exists in the active scope and uses the configured prefix. In component styles, theme.vars paths are generally easier to maintain.

## 16. Should I change cssVarPrefix?

Only if the app has a naming conflict or an explicit integration requirement. The default prefix is fine for most applications.

## 17. Does CSS color-scheme affect only MUI components?

No. The CSS property can also influence native browser controls and scrollbars. Consider how native inputs and browser UI should appear in each mode.

## 18. Why does my custom dark style lose to another rule?

Theme mode styles may have different specificity by design when CSS variables are enabled. If overriding a rule that uses applyStyles, use the same mode-aware mechanism and inspect the computed CSS.

## 19. Can I use a fixed dark theme without a toggle?

Yes. Set palette.mode to dark if the whole app should always use dark palette behavior. You do not need a manual useColorScheme control for a fixed scheme.

## 20. What should I test when adding dark mode?

Test every component state, not just the page background. Check text and control contrast, hover/focus/disabled styles, native inputs, images, overlays, and the initial page load in the selected mode.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous Q&amp;A</th>
      <th>Chapter lesson</th>
      <th>Chapter example</th>
      <th>Next Q&amp;A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="009-responsive-design-and-breakpoints.md">009 - Responsive design and breakpoints</a></td>
      <td><a href="../lessons/010-color-schemes-dark-mode-and-css-variables.md">Lesson</a></td>
      <td><a href="../examples/010-color-schemes-dark-mode-and-css-variables.md">Example</a></td>
      <td><a href="011-typography.md">011 - Typography</a></td>
    </tr>
  </tbody>
</table>
