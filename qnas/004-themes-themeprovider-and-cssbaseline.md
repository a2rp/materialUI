# Themes, ThemeProvider, and CssBaseline - Q&A

## 1. What is a Material UI theme?

It is shared configuration for design values and component behavior. It can define colors, typography, spacing, breakpoints, shape, shadows, transitions, and component defaults.

## 2. Do I have to create a theme before using Material UI?

No. Components can use the library's default theme. Create a custom theme when the application needs shared design values or consistent component configuration.

## 3. What does createTheme do?

It accepts theme options and returns a complete theme object with defaults filled in. Pass the result to ThemeProvider.

## 4. What does ThemeProvider do?

It provides a theme through React context to descendant components. A component must be inside the provider to use that provider's custom theme.

## 5. Where should ThemeProvider go?

Usually near the React application root, above components that need the theme. This gives the app one clear theme boundary. An intentional nested provider can customize a subtree.

## 6. What happens if I nest ThemeProvider components?

Descendants use the nearest provider's theme. Nesting can be useful for a deliberate region with a different theme, but accidental nesting can produce inconsistent styles between siblings.

## 7. What does useTheme return?

It returns the active theme from the nearest ThemeProvider. Use it in component logic that needs to read a theme value. Styling APIs usually receive the theme without needing this hook.

## 8. Can the theme be defined inside App?

It can, but creating a new theme on every render is wasteful and can cause unnecessary work. Define a stable theme module outside the component, or memoize it when options intentionally depend on changing values.

## 9. What does CssBaseline do?

It adds global baseline styles that reduce differences from browser defaults, including the default body margin and consistent base behavior. It gives the document a starting point; it does not design the full page.

## 10. Should CssBaseline be included once or on every page?

Usually once near the application root inside the theme provider. It contributes global styles, so rendering it in every route is generally unnecessary.

## 11. Should CssBaseline be inside or outside ThemeProvider?

Put it inside when it should use the app's theme. A common root structure is ThemeProvider, then CssBaseline, then the application.

## 12. Does CssBaseline replace my CSS reset or page styles?

It provides Material UI's baseline styles, but it does not replace all product CSS. You still define page layout, content spacing, component-specific styles, and any additional global rules the application needs.

## 13. Why do some components look like the default theme?

The component may be outside the intended provider, may be under a nested provider, or may use theme values not customized by the app. Inspect the React tree and active theme boundary.

## 14. Can one page use two themes?

Yes, by intentionally nesting providers around separate subtrees. Make sure the design calls for a theme boundary and understand that each provider affects only its descendants.

## 15. Does React StrictMode provide theme context?

No. StrictMode enables development checks for React behavior. It does not replace ThemeProvider or CssBaseline.

## 16. Should all spacing values be put into the theme?

Shared spacing scale decisions belong in the theme. A one-off layout adjustment can remain local to the component. Keep the theme focused on values and rules that should be shared.

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
      <td><a href="003-component-structure-and-imports.md">003 - Component structure and imports</a></td>
      <td><a href="../lessons/004-themes-themeprovider-and-cssbaseline.md">Lesson</a></td>
      <td><a href="../examples/004-themes-themeprovider-and-cssbaseline.md">Example</a></td>
      <td><a href="005-theme-tokens-and-design-system.md">005 - Theme tokens and design system</a></td>
    </tr>
  </tbody>
</table>
