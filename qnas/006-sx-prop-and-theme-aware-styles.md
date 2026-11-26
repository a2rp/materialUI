# The sx Prop and Theme-Aware Styles - Q&A

## 1. What is the sx prop?

It is a styling prop that accepts CSS properties and MUI System style functions. It can read the active theme and apply theme-aware values such as palette paths, spacing, breakpoints, and typography.

## 2. Can I use normal CSS properties in sx?

Yes. The sx prop supports a superset of CSS properties. It also adds MUI-specific transformations for supported keys, so not every value behaves exactly like plain CSS.

## 3. What does color: 'primary.main' mean?

It reads the main color from the primary palette role in the active theme and applies it to the CSS color property.

## 4. What does p: 2 mean?

p is the padding shorthand, and a numeric value is interpreted through the theme spacing scale. With the default scale, 2 corresponds to 16 pixels.

## 5. Does width: 2 use the spacing scale?

No. Width uses a sizing transform, not the spacing transform. Consult the documented behavior for the specific sx key.

## 6. How do I write responsive values?

Pass an object whose keys are theme breakpoint names and whose values are the styles:

```jsx
<Box sx={{ p: { xs: 2, md: 4 } }} />
```

The breakpoint's value applies from that threshold upward until a later value overrides it.

## 7. Should I use useMediaQuery instead of responsive sx values?

Use responsive sx values when only CSS appearance changes. Use useMediaQuery when React behavior or rendered content must change based on a media condition.

## 8. How do I read a computed theme value?

Use an sx callback:

```jsx
<Box sx={(theme) => ({ p: theme.spacing(2) })} />
```

A string theme path is simpler for common values that do not need computation.

## 9. Can each sx property receive its own theme callback?

Current MUI guidance deprecates callbacks nested as individual property values. Use a callback for the entire sx object when needed.

## 10. Can sx use hover and descendant selectors?

Yes. Use nested selectors scoped with the current component selector, such as an ampersand selector for hover or a child class. Keep selectors narrow and understandable.

## 11. Is sx only available on Box?

No. Material UI components support sx, and MUI System components such as Box support it. Custom components need to support and forward sx appropriately if they expose it.

## 12. Can sx be used for a reusable design component?

It can, but a reusable component with multiple styling contexts may be clearer with styled or a shared theme variant. sx is especially useful for local styles.

## 13. Does sx have runtime cost?

It uses runtime styling, so there is some cost. For ordinary UI the convenience is usually reasonable. In very large repeated lists or with rapidly changing style values, profile the real application and consider stable CSS variables or stylesheets.

## 14. How should I handle a color value from user input?

Validate it before using it as a CSS value. Accept only the formats the application expects. Never pass arbitrary untrusted strings into CSS.

## 15. Why might a TypeScript sx object fail when stored in a variable?

TypeScript can widen a literal such as a flex direction string to the broad string type, which no longer satisfies SxProps. Pass the object directly, annotate it with the proper SxProps type, or preserve literal types with an appropriate const assertion.

## 16. Can I combine a caller's sx prop with my own?

Yes, using the supported sx composition pattern. Remember that sx may be an object, callback, or array; do not assume it is always a simple object that can be spread directly.

## 17. Should I use sx to target a component's internal generated class?

Prefer documented component slots and stable styling hooks. Generated implementation classes can change and make styles brittle.

## 18. When should I move styles out of sx?

Move them when the style is long, reused, contains complex selectors, or makes a component's rendering logic difficult to scan. Use styled, theme overrides, or a stylesheet based on where the style belongs.

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
      <td><a href="005-theme-tokens-and-design-system.md">005 - Theme tokens and design system</a></td>
      <td><a href="../lessons/006-sx-prop-and-theme-aware-styles.md">Lesson</a></td>
      <td><a href="../examples/006-sx-prop-and-theme-aware-styles.md">Example</a></td>
      <td><a href="007-styled-api-and-reusable-styles.md">007 - The styled API and reusable styles</a></td>
    </tr>
  </tbody>
</table>
