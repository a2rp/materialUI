# Component Structure and Imports

## Learning goals

After this chapter, you should be able to:

- Import public Material UI components using supported package paths.
- Read common component props and distinguish them from application state.
- Compose components through children and layout components.
- Change a component's root element with its documented component prop.
- Avoid relying on private implementation details or wrappers that break refs.

## Import public components

A Material UI component is a React export. The current Material UI guidance recommends direct component-path imports for better development startup and rebuild performance:

```jsx
import Button from '@mui/material/Button';
import TextField from '@mui/material/TextField';
```

This import style works without a special bundler plugin. Top-level named imports are also part of the public package API and modern production bundlers can tree-shake unused exports. However, importing from package barrels can slow development startup and rebuilds, especially for large icon packages. Prefer the direct paths shown in current MUI examples for predictable development performance.

Do not import from undocumented internal paths such as a nested implementation file. Such paths can change between releases or create duplicate modules. Import from the documented package root or public component path.

Icons use their own package. For example:

```jsx
import DeleteIcon from '@mui/icons-material/Delete';
```

Install the icons package separately before importing it. If the application uses its own icon set, it does not need to install this package.

## Props configure one component instance

Props are values passed from a parent to a component. Material UI props commonly control the component's visual variant, size, color, semantic element, state, and event behavior.

```jsx
import Button from '@mui/material/Button';

function SaveButton({ onSave, disabled = false }) {
  return (
    <Button
      variant="contained"
      color="primary"
      size="medium"
      disabled={disabled}
      onClick={onSave}
    >
      Save
    </Button>
  );
}
```

Here, `variant`, `color`, `size`, and `disabled` configure the Button. The `onClick` prop receives application behavior from its parent. Material UI does not own the save operation; it renders a button and invokes the callback when the user activates it.

Not every component accepts the same props. Check the API for the component rather than assuming that a prop from Button is available on Dialog, TextField, or another component.

## Compose through children

React's standard composition pattern is to pass child elements between a component's opening and closing tags. Material UI uses that same pattern:

```jsx
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

function NoticeCard() {
  return (
    <Paper>
      <Stack spacing={1}>
        <Typography variant="h6">Account settings</Typography>
        <Typography color="text.secondary">
          Review your contact details.
        </Typography>
      </Stack>
    </Paper>
  );
}
```

The components keep their responsibilities small: Paper creates a surface, Stack arranges the content, and Typography applies a text variant. The content itself remains ordinary React children.

For repeated UI, make a component that accepts meaningful props instead of copying the same markup to every page. Keep its public interface smaller than the set of all possible internal styling decisions.

## The component prop and semantic HTML

Many Material UI components let you replace their root element with the `component` prop. This changes the rendered root while retaining the component's styling and behavior that support that usage.

For example, a Button that navigates to another page should render a link:

```jsx
<Button component="a" href="/settings">
  Settings
</Button>
```

An anchor communicates navigation semantics and supports browser link behavior such as opening in a new tab. A normal Button is more appropriate for an action such as submitting a form or opening a dialog. Do not choose an element only for its appearance.

The `component` prop can also receive a router Link component, but the router and Material UI component must agree on required props and refs. Confirm support in that component's API and your router integration. TypeScript can require a generic prop type when a reusable custom component forwards the component prop.

## Slots and slotProps

A slot is a named structural part of a component. Every Material UI component has a root slot, and more complex components can expose interior slots. The documented component API lists available slots.

- `slots` can replace a supported slot component.
- `slotProps` can pass props to supported slots.
- `component` changes the root element on components that support it.

Use these public customization points when the component provides them. Do not target an internal DOM node by guessing its position or class name. Slot details vary by component, so verify the API before using a slot.

## Refs and wrapper components

Some components need a DOM node reference to position a tooltip, focus an input, measure an anchor, or manage an interaction. A custom component used as a child or as the root replacement may need to forward its ref and relevant props to the actual DOM element.

```jsx
import { forwardRef } from 'react';

const ForwardedLink = forwardRef(function ForwardedLink(props, ref) {
  return <a {...props} ref={ref} />;
});
```

Forwarding only the ref is not enough if the wrapper drops other props such as `className`, event handlers, or accessibility attributes. Spread supported props onto the intended element and keep the wrapper's element semantics correct.

Not every component requires a custom child to forward a ref. Check the component API and the warning produced by the actual integration. Avoid adding ref machinery where it is not needed.

## Reading an API surface

When you use a new component, inspect its documented API for:

1. The rendered root and supported semantic alternatives.
2. The props for variant, size, color, state, and behavior.
3. Required children and whether it supports a ref.
4. Available slots and slot props.
5. The classes or state hooks documented for customization.
6. The package that exports the component.

Treat component props as the supported contract. Internal generated markup, private implementation files, and undocumented CSS selectors are not stable contracts.

## Common mistakes

- **Using an HTML element for the wrong job:** use an anchor for navigation and a button for an action.
- **Assuming all components expose the same props:** each component has its own API.
- **Using a guessed deep import:** stick to public package paths.
- **Dropping props in a wrapper:** forward the props and ref expected by the wrapped component.
- **Reaching into generated DOM:** prefer documented slots, slotProps, and styling APIs.
- **Overusing wrapper components:** compose Material UI components directly when a wrapper does not add a useful product-level abstraction.

## Recap

Material UI components are public React exports configured through props and composed with children. Current guidance recommends direct component-path imports for development performance. Use the documented component prop for semantic root changes, slots for supported structural customization, and forward props and refs when a wrapper is used where the target component needs them.

## Practice task

Create a page with a Paper surface, a heading, a description, and a Button that navigates to a settings page. Use direct component-path imports. Explain why the navigation control should render as an anchor, and inspect the Button API before using `component` with a router Link.

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
      <td><a href="002-project-setup-and-installation.md">002 - Project setup and installation</a></td>
      <td><a href="../qnas/003-component-structure-and-imports.md">Q&amp;A</a></td>
      <td><a href="../examples/003-component-structure-and-imports.md">Example</a></td>
      <td><a href="004-themes-themeprovider-and-cssbaseline.md">004 - Themes, ThemeProvider, and CssBaseline</a></td>
    </tr>
  </tbody>
</table>
