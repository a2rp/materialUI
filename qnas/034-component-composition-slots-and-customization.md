# Component Composition, Slots, and Customization: Q&A

This Q&A accompanies [034 - Component Composition, Slots, and Customization](../lessons/034-component-composition-slots-and-customization.md). It explains how to adapt Material UI components while preserving their semantics, refs, and built-in behavior.

## Composition

### 1. When should I use children?

Use children when the parent component already provides the structure or behavior you need and the application supplies its content. This keeps composition idiomatic and lets each nested component remain explicit.

### 2. What does the component prop change?

It changes the root element or component rendered by a Material UI component. Use it to preserve the Material UI API while choosing appropriate semantics, such as rendering a Button as an anchor for navigation.

### 3. Should navigation controls use button or link semantics?

Use a link for navigation and a button for an action. If a component is rendered as a link, provide its required destination or router props and keep browser navigation behavior available.

### 4. What is a slot?

A slot is a named part of a component's internal structure, such as a root, popper, transition, or icon. The slots prop replaces a slot's component.

### 5. What is slotProps for?

slotProps passes configuration to an existing internal slot. Use it for attributes, event handlers, placement, styling, or other supported props when the built-in slot component should remain in place.

### 6. When should I replace a slot instead of configuring it?

Replace it through slots when the internal component itself needs to change. Use slotProps when the built-in component is correct and only its configuration needs adjustment. Replacing a slot means you may need to preserve its ref, props, and behavior.

### 7. Why are components and componentsProps discouraged?

Material UI is standardizing component customization on slots and slotProps. Older components, componentsProps, and slot-specific Component or Props APIs are deprecated or removed in current versions. Prefer the current API for new code and use migration guidance for older code.

### 8. Can a slot receive both a custom component and custom props?

Yes. Use slots to select the component and slotProps to pass its configuration. The custom slot must support the props and ref that its parent expects.

## Wrappers and refs

### 9. Why must a wrapper forward props?

The parent may pass event handlers, class names, IDs, accessibility attributes, and state props through the wrapper. If the wrapper drops them, the component can lose behavior, styling, or accessibility.

### 10. Why must a custom child forward a ref?

Components such as Tooltip may need a DOM reference to position or manage interaction. A custom function component that does not forward a ref can trigger warnings or fail to work correctly.

### 11. How do I forward a ref through a wrapper?

Use React.forwardRef and attach the received ref to the underlying DOM-capable Material UI component or element. Continue forwarding the remaining props with the spread operator.

### 12. What is muiName and when does it matter?

Some Material UI components inspect child component identity to decide how to compose their children. If a wrapper hides a component that the parent specifically recognizes, the wrapper may need to preserve the relevant muiName metadata. Check the composition guidance for the parent component involved.

## Styling choices

### 13. When should I use sx?

Use sx for a one-off adjustment to a component instance. It supports theme-aware values and nested selectors for stable component utility classes.

### 14. When should I use styled?

Use styled when the same custom styling belongs to a reusable component or when a component needs a named style definition. Reuse theme values for color, spacing, shape, and typography.

### 15. When should I use theme overrides?

Use the theme when a rule should apply consistently across many instances, such as a default prop, shared slot style, or variant based on component props. Keep global rules narrow so they do not create surprises elsewhere.

### 16. Are theme overrides tree-shakable?

No. Theme component customization is not tree-shakable. For heavy or highly specific custom behavior, a reusable component can be a better choice.

### 17. Where do theme variants go?

In current Material UI, variants are configured under the relevant slot within styleOverrides, commonly styleOverrides.root. Each variant matches props and applies a style. Check the installed version's API when following examples from older releases.

### 18. Can sx override a theme rule?

The sx prop has higher specificity than standard theme customization, so it can adjust a particular instance when needed. Prefer resolving the shared design at the theme level when every instance should behave the same.

### 19. Which CSS class names are safe to target?

Use documented global utility classes such as MuiButton-root or MuiButton-startIcon. Generated class names can include hashes and are not stable across builds.

### 20. How do I style a hover, focus, or disabled state?

Target the supported state selector with enough specificity, and preserve keyboard focus visibility. Verify the result with keyboard interaction and high contrast, not only with a pointer hover.

### 21. Should I use global CSS?

Use global CSS when a broad stylesheet rule is the right scope, such as a browser-level reset or shared external integration. For component-specific changes, sx, styled, and the theme usually express the intent more safely.

## Choosing and reviewing customizations

### 22. How do I avoid replacing useful built-in behavior?

Check whether the component already has a prop, variant, theme token, or slot configuration for the need. Replace the component only when configuration cannot achieve the required structure or behavior.

### 23. How do I verify a custom wrapper?

Check that it forwards its ref, className, event handlers, IDs, aria attributes, and other props. Test it inside the parent components that will consume it, such as Tooltip or a list.

### 24. What states should I review after changing styles?

Review hover, focus-visible, disabled, selected, expanded, dark mode, and responsive layouts where they apply. Confirm that content, focus indicators, and control boundaries remain clear.

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
      <td><a href="033-date-and-time-pickers.md">033 - Date and Time Pickers</a></td>
      <td><a href="../lessons/034-component-composition-slots-and-customization.md">Lesson</a></td>
      <td><a href="../examples/034-component-composition-slots-and-customization.md">Example</a></td>
      <td><a href="035-typescript-with-material-ui.md">035 - TypeScript with Material UI</a></td>
    </tr>
  </tbody>
</table>
