# Theme Tokens and the Design System - Q&A

## 1. What is a design token?

A design token is a named value that represents a shared design decision, such as a semantic color, text style, spacing step, border radius, or elevation. Tokens make repeated choices consistent and easier to update.

## 2. Which theme values should an application define first?

Start with values that the design repeats: palette roles, typography, spacing rhythm, breakpoints, and shape. Add component defaults when the same behavior should apply across many instances.

## 3. What is the difference between primary and a literal color value?

Primary is a semantic role. The theme maps that role to a color. Components and styles can then request the primary role without copying the same hex value into many files.

## 4. If I specify only palette.primary.main, are light, dark, and contrastText also generated?

Material UI's palette utilities can derive related values from the main color. The derived values may not match a precise brand specification or contrast requirement. Provide exact values when needed and verify the combinations in the rendered UI.

## 5. Does a semantic color role guarantee sufficient contrast?

No. A role identifies intended meaning, but contrast depends on the foreground and background pair and the displayed text or control. Check the actual combinations, especially for small text and disabled states.

## 6. What is a Typography variant?

A variant is a named text style from the theme, such as h4, body1, or caption. It establishes consistent typography values. The visual variant and the semantic HTML element are separate choices.

## 7. Why set component="h1" on Typography?

It selects the semantic HTML heading level while allowing the visual variant to be configured separately. A page should use a meaningful heading hierarchy rather than selecting heading elements only for their default appearance.

## 8. What does theme.spacing(2) return?

With the default theme spacing scale, it returns a value equivalent to 16 pixels. The scale is configurable, so check the active theme if the app changes spacing.

## 9. Does every numeric sx value use the spacing scale?

No. Only style-system properties that support spacing interpret numbers as spacing units. Width, opacity, z-index, and other properties can have different numeric behavior. Check the API for the specific property.

## 10. What does p: 2 mean in sx?

For the padding system property, 2 is interpreted through the active theme spacing scale. Under the default spacing scale this corresponds to 16 pixels.

## 11. What are breakpoints?

Breakpoints are named viewport thresholds used for responsive styles. Material UI exposes common names such as xs, sm, md, lg, and xl. Their values can be customized in the theme.

## 12. Should I hard-code breakpoint pixel values?

Use the theme's named breakpoints for styles that belong to the design system. Configure the thresholds centrally when the whole application needs different viewport ranges, and test the resulting layouts.

## 13. What is the difference between defaultProps and styleOverrides?

defaultProps supplies default component prop values. styleOverrides changes component slot styles. Variants can define styles selected by matching component props.

## 14. Should I put every component's styles in the theme?

No. Put product-wide, repeated rules in the theme. A one-off style that belongs to one instance or page is easier to understand next to that component.

## 15. Can I add custom values to the theme?

Yes. Custom values can hold application-specific tokens. In TypeScript, augment the theme types as well so the compiler recognizes the new fields.

## 16. Where should dark mode values be configured?

Color-scheme configuration and CSS variable behavior have dedicated APIs. Keep palette roles semantic and handle mode switching with the supported scheme APIs. The later dark-mode chapter explains that setup.

## 17. Why did changing one theme value affect multiple pages?

The theme is shared through ThemeProvider, so every descendant that uses that token can change. This is useful for consistency. If a change should affect one page only, use local styling or a scoped component variant instead.

## 18. How can I check whether a value is already a theme token?

Inspect the theme structure and the component or style API for the installed version. Prefer a documented theme property over inventing a token name or reaching into internal theme fields.

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
      <td><a href="004-themes-themeprovider-and-cssbaseline.md">004 - Themes, ThemeProvider, and CssBaseline</a></td>
      <td><a href="../lessons/005-theme-tokens-and-design-system.md">Lesson</a></td>
      <td><a href="../examples/005-theme-tokens-and-design-system.md">Example</a></td>
      <td><a href="006-sx-prop-and-theme-aware-styles.md">006 - The sx prop and theme-aware styles</a></td>
    </tr>
  </tbody>
</table>
