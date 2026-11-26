# TypeScript with Material UI: Q&A

This Q&A accompanies [035 - TypeScript with Material UI](../lessons/035-typescript-with-material-ui.md). It explains how to preserve type safety when wrapping components, styling with sx, forwarding refs, and extending the theme.

## Component props and events

### 1. Should I copy a Material UI component's props into my wrapper type?

Usually not. Reuse the public prop type, such as ButtonProps, and omit only the props your wrapper owns or narrows. This keeps the wrapper aligned with the installed Material UI version.

### 2. Why might a wrapper omit children?

A wrapper can own its content through another required prop, such as label. Omitting children prevents callers from passing two competing sources of content. If the wrapper is a layout component, use ReactNode for children instead.

### 3. Should I type every event parameter manually?

Let TypeScript infer event types from a typed component when possible. Explicitly type callbacks that form part of your own component API, especially when exposing a simpler value such as a string rather than a Material UI event.

### 4. When should I use ReactNode?

Use ReactNode when a prop accepts renderable content such as text, fragments, or elements. Use a narrower type when the component requires one specific kind of child.

### 5. How do I type a component that accepts a custom Material UI color?

Augment the palette interfaces to add the theme color and augment the component's color override interface to allow that value on its color prop. Also provide the color tokens in the runtime theme.

### 6. Why does adding a color to createTheme not make TypeScript accept it?

Runtime configuration and compile-time declarations are separate. The theme can contain the color at runtime while the component's TypeScript union still rejects it. Module augmentation updates the type contract.

## Theme augmentation

### 7. What is module augmentation?

It extends an existing TypeScript module declaration with application-specific interfaces. Material UI uses it for custom palette colors, component variants, and theme variables.

### 8. Where should augmentation declarations live?

A .d.ts file in a directory included by tsconfig is a common choice. It must be part of the TypeScript program that compiles the components using the new types.

### 9. Why does my augmentation appear to have no effect?

Check that the declaration file is included, that the module specifier exactly matches the package path being augmented, and that the dev server or TypeScript language service has reloaded the project.

### 10. What is the difference between Palette and PaletteOptions?

Palette describes the fully created theme, while PaletteOptions describes the optional configuration accepted by createTheme. A custom color normally needs an entry in both interfaces.

### 11. Do I need to augment a component's color overrides too?

Yes, if the custom color should be accepted by a component prop such as Button color. Augment that component's PropsColorOverrides interface in addition to Palette and PaletteOptions.

### 12. How do I add a custom Button variant?

Augment ButtonPropsVariantOverrides so TypeScript accepts the variant name, then define the matching runtime style in the theme. The declaration alone does not create the visual style.

### 13. Where do theme variants go in current Material UI?

They belong under the relevant slot in styleOverrides, commonly styleOverrides.root. Examples from older versions may show a different structure, so check the installed version's current documentation.

### 14. How do I add an application-specific theme variable?

Augment both Theme and ThemeOptions. Theme represents the final theme available to components, while ThemeOptions describes the configuration that createTheme accepts.

## sx, styled, and polymorphic components

### 15. What type should an sx prop use?

Use SxProps with the application's Theme type, typically SxProps<Theme>. This allows callers to pass theme-aware objects, callbacks, or arrays.

### 16. Why can merging sx produce a type error?

SxProps can itself be an array or callback. Normalize the caller's value before adding it to another sx array, and avoid nesting an SxProps value as if it were a single style object.

### 17. Does the component prop work with TypeScript?

Built-in Material UI components support polymorphic root elements and infer many matching attributes. A custom generic wrapper can require more type work, especially when its ref type changes with the root element.

### 18. Why can a forwarded ref type be wrong?

The ref must match the actual rendered root. A wrapper fixed to a button can use an HTMLButtonElement ref. A wrapper that also permits rendering an anchor needs a polymorphic or more constrained type strategy.

### 19. Why might styled(Button) lose some Button prop typing?

The generic component surface can be difficult for TypeScript to preserve through styled. If the result should retain exactly the original Button API, the MUI TypeScript guide may recommend casting the styled result to typeof Button. Do not use a cast to claim unsupported behavior.

### 20. Should I use any to fix a Material UI type error?

No. any hides the mismatch instead of explaining it. Check whether the value type, module augmentation, component root, or wrapper prop contract is incorrect, then use the narrow type that matches the implementation.

## Practical troubleshooting

### 21. Should module augmentation change runtime behavior?

No. It changes TypeScript's understanding only. Add the corresponding runtime theme configuration or component implementation separately.

### 22. How can I debug a custom theme color that compiles but renders incorrectly?

Check both sides: the module augmentation must describe the color, and createTheme must supply valid palette tokens such as main, light, dark, and contrastText. Also confirm the component supports the custom color prop.

### 23. How do I type a custom component intended to be themeable like a built-in component?

Define explicit props and owner state, define named slots with styled, and use the theme APIs to consume default props and styles. This is useful when building a reusable component library; a single-application component may not need that added integration.

### 24. How should I approach a type mismatch after upgrading Material UI?

Check the API and migration guidance for the installed version. Types can change with component APIs, and older examples may refer to deprecated props or different polymorphic and slot types.

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
      <td><a href="034-component-composition-slots-and-customization.md">034 - Component Composition, Slots, and Customization</a></td>
      <td><a href="../lessons/035-typescript-with-material-ui.md">Lesson</a></td>
      <td><a href="../examples/035-typescript-with-material-ui.md">Example</a></td>
      <td><a href="036-accessibility-and-keyboard-interaction.md">036 - Accessibility and Keyboard Interaction</a></td>
    </tr>
  </tbody>
</table>
