# CSS Overrides and Global Styles - Q&A

## 1. Which styling method should I use for one Button?

Use the sx prop for a local, one-instance adjustment. It stays near the component and can read theme values.

## 2. When should I use a theme override?

Use a theme component override when the rule should apply consistently to all instances of a Material UI component or when the app needs shared variants and defaults.

## 3. When should I use GlobalStyles?

Use GlobalStyles for a limited set of app-wide CSS rules, such as a document-level selector or global style that is not specific to one MUI component.

## 4. Can I put global styles in CssBaseline configuration?

Yes. The MuiCssBaseline theme slot supports baseline style overrides. If the app already renders CssBaseline, use its theme configuration instead of rendering another baseline only to add a global rule.

## 5. What is a stable MUI class name?

A stable global class identifies a component slot, for example .MuiButton-root. It is intended for styling and is different from the generated hash class used internally by the styling engine.

## 6. Can I select a generated class like .css-abc123-MuiButton-root?

Avoid it. The hash is an implementation detail and can change between builds. Use the documented global slot class.

## 7. Why should I combine a slot and state class?

State classes such as Mui-disabled or Mui-selected can occur on different components. Pairing the component slot with the state scopes the rule and provides the specificity needed for that state.

## 8. Why does a disabled-state override not apply?

Inspect the computed style. The state rule may have greater specificity, appear later in the cascade, or be supplied through a theme rule with different ordering. Use a selector containing the component slot and the state class when needed.

## 9. Should I target .Mui-disabled globally?

No. It can affect unrelated controls. Pair the state class with the component slot or use the component's sx prop or theme override.

## 10. Do CSS Modules automatically target MUI's global classes?

No. CSS Modules scope local class names. If the bundler supports it, use its global selector syntax for the documented MUI slot class and keep that selector scoped under a local component class.

## 11. What does CSS specificity control?

Specificity helps determine which matching CSS declaration wins within the cascade. Source order, importance, cascade layers, and origin also matter. Inspect computed styles rather than guessing.

## 12. Should I fix every style conflict with !important?

No. First check the selector, component state, source order, theme override, and insertion order. !important can create hard-to-maintain conflicts.

## 13. What are CSS cascade layers?

They are a CSS mechanism for organizing precedence among groups of rules. Material UI v9 can emit styles in layers, which can help integration with CSS frameworks. Enabling layers is a project-level choice that can change the existing cascade.

## 14. Do I need cascade layers for every Material UI app?

No. Use them when the app needs coordinated precedence with other styling systems. A normal app can use the standard styling setup and MUI customization APIs without configuring cascade layers.

## 15. Why might a custom stylesheet load before MUI styles?

Styling libraries insert generated styles at particular positions. The actual result depends on the app's engine and integration. Check the supported insertion-order or CSS-layer configuration for the installed versions.

## 16. Can I override a nested component part?

Yes, when the API exposes a slot or stable global class for it. Use that documented hook. Do not guess the internal element tree from the current rendered markup.

## 17. Should a global style rule contain page layout?

Usually not. Page-specific layout belongs near the page or component. Global rules should be limited to genuine document-wide behavior.

## 18. How do I avoid broad side effects from an override?

Choose the narrowest scope, target the correct component slot, include the correct state when necessary, and check unrelated component instances after changing a shared theme rule.

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
      <td><a href="007-styled-api-and-reusable-styles.md">007 - The styled API and reusable styles</a></td>
      <td><a href="../lessons/008-css-overrides-and-global-styles.md">Lesson</a></td>
      <td><a href="../examples/008-css-overrides-and-global-styles.md">Example</a></td>
      <td><a href="009-responsive-design-and-breakpoints.md">009 - Responsive design and breakpoints</a></td>
    </tr>
  </tbody>
</table>
