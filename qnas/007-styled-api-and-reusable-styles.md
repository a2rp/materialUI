# The styled API and Reusable Styles - Q&A

## 1. What does Material UI's styled utility do?

It creates a styled React component using the configured styling engine, with Material UI theme integration. Import it from `@mui/material/styles` when building a Material UI app.

## 2. When should I use styled instead of sx?

Use sx for a one-off component style or a small local adjustment. Use styled when a named component's base styles are reused across multiple places or depend on its props.

## 3. Can a styled component still accept sx?

Material UI's styled utility supports the sx prop by default. Callers can add local adjustments while the shared base style remains defined in the reusable component.

## 4. Should I define a styled component inside a React component?

Usually not. Define it at module scope. Creating a new styled component type on each render can cause unnecessary work and affect component identity.

## 5. How do I read theme values inside styled?

The style callback receives the theme:

```jsx
const Card = styled(Paper)(({ theme }) => ({
  padding: theme.spacing(2),
  color: theme.palette.text.primary,
}));
```

Use the active theme rather than repeating shared literals.

## 6. Is the style object written like sx?

Both APIs use CSS-style property names and can access theme values. sx has MUI System-specific shorthand and transforms, while styled's callback returns a style object for the styled utility. Check the API for a property's value behavior.

## 7. Can a styled component change based on a custom prop?

Yes. The callback can use props. In TypeScript, define the prop type. If the custom prop is only for styling and the component renders a DOM element, ensure it is not forwarded as an invalid HTML attribute.

## 8. What is shouldForwardProp?

It is a filter that decides which props are passed to the rendered component or DOM element. Use it to prevent style-only custom props from leaking to a DOM node. Confirm how the MUI styled API handles its standard props before replacing its forwarding rules.

## 9. Does every custom prop need filtering?

No. Some props are valid on the underlying component, and MUI's styled utility filters some styling props by default. Filter the custom style-only props that would otherwise be passed where they do not belong.

## 10. Can theme overrides target my styled component?

They can when the styled component is configured with the appropriate component name and slot. This makes it part of the theme component configuration contract. Add that integration only when centralized defaults or variants are useful.

## 11. Should I use a component name and slot for every styled component?

No. A simple reusable component can keep its styles local. Theme registration adds a shared API and is useful when the app needs global overrides or variants.

## 12. Can I use Emotion's styled function directly?

You can, but Material UI's styled utility adds theme integration and MUI features. Prefer the utility from Material UI when building a component for a Material UI application unless you need a styling-engine-specific capability.

## 13. Is styled appropriate for global body styles?

No. Use CssBaseline, GlobalStyles, or a global stylesheet for document-wide rules. A styled component is for a component's own rendered output.

## 14. Does styled automatically make a reusable component accessible?

No. Styling does not choose semantics, accessible names, keyboard behavior, or focus handling. Start from an appropriate semantic or Material UI component and preserve its behavior.

## 15. How do I decide whether to extract a style?

Extract when the style is reused or has a useful name and API. Keep a short one-off style next to its component. Avoid creating abstractions that add more complexity than they remove.

## 16. What is the difference between a styled wrapper and composition?

A styled wrapper creates a reusable component with its own base styling. Composition combines existing components as children. They can be used together, but composition alone may be enough when the structure is not reused.

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
      <td><a href="006-sx-prop-and-theme-aware-styles.md">006 - The sx prop and theme-aware styles</a></td>
      <td><a href="../lessons/007-styled-api-and-reusable-styles.md">Lesson</a></td>
      <td><a href="../examples/007-styled-api-and-reusable-styles.md">Example</a></td>
      <td><a href="008-css-overrides-and-global-styles.md">008 - CSS overrides and global styles</a></td>
    </tr>
  </tbody>
</table>
