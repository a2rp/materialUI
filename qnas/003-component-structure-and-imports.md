# Component Structure and Imports - Q&A

## 1. What is a Material UI component?

It is a React component exported from a Material UI package. You render it in JSX, pass it props, compose it with children, and connect it to application state and behavior.

## 2. How should I import a component?

Current Material UI examples commonly use a direct component path:

```jsx
import Button from '@mui/material/Button';
```

The package root also exposes named exports, but direct imports can improve development startup and rebuild speed by avoiding large barrel imports. Follow the current guidance for the installed version.

## 3. Does using a direct import guarantee the production bundle is smaller?

It avoids loading through the package barrel and can improve development performance. Modern production bundlers can tree-shake unused exports from supported named imports, so the production impact depends on the bundler and build. Do not make bundle-size claims without inspecting the built output.

## 4. Can I import from an undocumented internal file?

Avoid it. Internal paths are not a stable public API and may change or cause duplicate modules. Use the documented package root or the documented component path.

## 5. Are Material UI icons included in the core package?

No. The icon components are published separately in `@mui/icons-material`. Install that package if you use it, then import only the icons the app needs.

## 6. What do variant, color, and size props do?

They configure a component instance using the choices supported by that component. Their accepted values differ by component. Check its API instead of assuming values transfer between Button, Chip, Typography, or another component.

## 7. Does an onClick prop make a Button save data?

No. The prop connects a click event to a callback supplied by the application. The callback must implement the save operation, update state, or call a service.

## 8. What is component composition?

Composition is building a larger interface by nesting smaller components and passing children. For example, a Paper can contain a Stack, which contains Typography and Button components. This follows the normal React children model.

## 9. What does the component prop do?

On components that support it, component replaces the rendered root element or component type while preserving the component's supported behavior and styling. For example, a Button used for navigation can render an anchor with an href. Confirm support in the specific component API.

## 10. Should navigation use Button with an onClick handler?

For ordinary navigation, use link semantics: an anchor, or a router Link supplied through a supported component prop. Use a button for an action such as saving, submitting, opening a dialog, or toggling a setting. Correct semantics provide expected browser and keyboard behavior.

## 11. What is the difference between slots and slotProps?

Slots are replaceable structural parts exposed by a component. The slots prop can replace a supported part, while slotProps passes properties to that part. Check the component API because not every component exposes the same slots.

## 12. When does a custom wrapper need to forward a ref?

When the Material UI component uses its child or root as a DOM target for an interaction, such as positioning, focus, or measurement. The component API or a runtime warning can identify this requirement. The wrapper should forward the ref to the actual DOM element.

## 13. Is forwarding the ref enough?

Not always. The wrapper may also need to preserve className, event handlers, accessibility attributes, and other supported props. Dropping these can break styling, interaction, or accessible behavior.

## 14. Can I style a component by selecting its current nested DOM structure?

Avoid relying on undocumented DOM structure. Prefer the public sx/styled API, documented global classes, component slots, and supported slotProps. Implementation markup may change between releases.

## 15. What is the difference between props and state?

Props are inputs supplied by a parent. State is data owned by a component or another state manager that can change over time. A Button's disabled prop might come from parent props or local state, but the Button itself should not own an unrelated application workflow.

## 16. Should I wrap every MUI component in my own component?

No. A wrapper is useful when it adds a stable product-level interface, behavior, or shared policy. A wrapper that only renames a Button or hides all its props can make the API harder to use.

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
      <td><a href="002-project-setup-and-installation.md">002 - Project setup and installation</a></td>
      <td><a href="../lessons/003-component-structure-and-imports.md">Lesson</a></td>
      <td><a href="../examples/003-component-structure-and-imports.md">Example</a></td>
      <td><a href="004-themes-themeprovider-and-cssbaseline.md">004 - Themes, ThemeProvider, and CssBaseline</a></td>
    </tr>
  </tbody>
</table>
