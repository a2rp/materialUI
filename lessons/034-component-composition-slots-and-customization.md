# Component Composition, Slots, and Customization

Material UI components are designed to be combined, adapted, and styled. Composition lets an application keep semantic behavior while changing the rendered element or supplying a custom child. Slots expose important internal pieces of a component. Styling tools let you choose a one-off adjustment, a reusable component style, or a shared theme rule.

This chapter gives a practical order for these choices and explains how to avoid replacing behavior accidentally.

## Learning goals

After this chapter, you should be able to:

- Choose between children, the component prop, slots, and slotProps.
- Keep refs and required props working when composing custom components.
- Use sx for local styles and styled for reusable styles.
- Set shared defaults, variants, and overrides through the theme.
- Target stable utility classes instead of generated class names.
- Keep customizations narrow enough to maintain.

## Compose with children

Use normal React composition when the component's content is meant to be supplied by the application:

```jsx
import { Card, CardActions, CardContent, Typography, Button } from "@mui/material";

<Card>
  <CardContent>
    <Typography variant="h6">Release notes</Typography>
    <Typography color="text.secondary">
      Review the changes before publishing.
    </Typography>
  </CardContent>
  <CardActions>
    <Button>Review changes</Button>
  </CardActions>
</Card>
```

Prefer this direct structure when the component already exposes the content and behavior you need. A custom wrapper should forward the props and refs that its parent component relies on.

## Change the root with component

Many Material UI components allow the root element to be changed with the component prop. This is useful when a control needs different HTML semantics or when it should use a router link:

```jsx
<Button component="a" href="/reports">
  Open reports
</Button>
```

Use an anchor or router link for navigation and a button for an action. Changing the root element does not remove the need to provide the correct props for that element. In a custom component, forward the ref and any props Material UI passes through.

## Slots and slotProps

A slot is a named internal part of a component. The slots prop replaces a slot's component. The slotProps prop configures a slot without replacing its component.

```jsx
import Fade from "@mui/material/Fade";
import IconButton from "@mui/material/IconButton";
import MoreVertIcon from "@mui/icons-material/MoreVert";
import Tooltip from "@mui/material/Tooltip";

<Tooltip
  title="Opens the project menu"
  slots={{ transition: Fade }}
  slotProps={{
    popper: { placement: "bottom-end" },
    tooltip: { sx: { maxWidth: 240 } },
  }}
>
  <IconButton aria-label="Open project menu">
    <MoreVertIcon />
  </IconButton>
</Tooltip>
```

Use slotProps for properties such as placement, className, event handlers, and styling on the existing internal component. Use slots when the internal component itself needs to be replaced. A custom slot must preserve the props, ref, and behavior required by the parent.

The library is standardizing on slots and slotProps. Older APIs such as components, componentsProps, and many slot-specific component or props names are deprecated or removed in current versions. Prefer the current API for new code and follow the migration guide when updating older projects.

## Styling layers

Choose the narrowest styling tool that fits the need:

1. Use sx for a one-off change on one component instance.
2. Use styled to create a reusable styled component.
3. Use theme defaults, variants, or overrides for rules shared across the application.
4. Use global CSS only when the other tools do not fit the requirement.

### One-off styles with sx

The sx prop can read values from the theme and can target stable inner utility classes:

```jsx
import Button from "@mui/material/Button";

<Button
  variant="contained"
  sx={{
    px: 3,
    borderRadius: 2,
    "& .MuiButton-startIcon": {
      mr: 1,
    },
  }}
>
  Publish
</Button>
```

Use global utility classes such as MuiButton-startIcon for component slots. Generated class names often include a build-specific hash and should not be used as durable selectors.

### Reusable styles with styled

Use styled when a style belongs to a reusable component rather than one usage:

```jsx
import Button from "@mui/material/Button";
import { styled } from "@mui/material/styles";

const QuietButton = styled(Button)(({ theme }) => ({
  borderRadius: theme.shape.borderRadius * 2,
  textTransform: "none",
  "&:hover": {
    backgroundColor: theme.palette.action.hover,
  },
}));
```

Base reusable styles on theme values when possible. This helps the component adapt when the palette, spacing, shape, or typography changes.

## Shared theme customization

Theme configuration can set default props, styles, and prop-based variants for a component. Theme component customization is not tree-shakable, so use a reusable component for heavy customizations:

```jsx
import { createTheme, ThemeProvider } from "@mui/material/styles";

import Button from "@mui/material/Button";

const theme = createTheme({
  components: {
    MuiButton: {
      defaultProps: {
        disableElevation: true,
      },
      styleOverrides: {
        root: {
          borderRadius: 10,
          textTransform: "none",
          variants: [
            {
              props: { variant: "outlined", color: "primary" },
              style: { borderWidth: 2 },
            },
          ],
        },
      },
    },
  },
});

function App() {
  return (
    <ThemeProvider theme={theme}>
      <Button variant="outlined">Save draft</Button>
    </ThemeProvider>
  );
}
```

Use defaultProps for a consistent default that a caller may still override. Use styleOverrides for shared slot styles. Use variants when a shared style should apply only when component props match. Keep theme rules focused: a global override affects every instance unless a more specific style takes precedence.

## Target component states carefully

Interactive states such as hover, focus-visible, disabled, selected, and expanded can have higher CSS specificity than a basic root rule. Target the correct utility state class and preserve the focus treatment.

```jsx
import IconButton from "@mui/material/IconButton";
import SettingsIcon from "@mui/icons-material/Settings";

<IconButton
  sx={{
    "&.Mui-focusVisible": {
      outline: "2px solid",
      outlineOffset: 2,
    },
    "&:hover": {
      bgcolor: "action.hover",
    },
  }}
  aria-label="Open settings"
>
  <SettingsIcon />
</IconButton>
```

Do not remove focus styling while changing ripple behavior. Do not use visual state alone when the interaction also needs an accessible state such as aria-pressed or aria-expanded.

## Wrap components safely

A custom wrapper can add defaults or application-specific behavior. Forward the ref and remaining props so the wrapped Material UI component can still receive accessibility attributes, event handlers, and styling:

```jsx
import { forwardRef } from "react";
import IconButton from "@mui/material/IconButton";

const SmallActionButton = forwardRef(function SmallActionButton(
  { size = "small", ...props },
  ref,
) {
  return <IconButton ref={ref} size={size} {...props} />;
});
```

A parent such as Tooltip may need a DOM ref on its child. A function component that drops the ref or props can trigger warnings or lose interaction behavior. When a parent depends on a particular child type, check whether wrapping it also requires preserving that component's composition metadata.

## Avoid unnecessary customization

Before overriding a component, check whether a built-in prop, variant, palette token, typography setting, spacing value, or slot already solves the need. Prefer stable public APIs over selectors tied to private DOM details.

Avoid global changes that make unrelated controls harder to use. Test customizations in hover, focus, disabled, dark, and responsive states, and verify that text and focus remain legible.

## Common mistakes

- Changing a root element without changing its semantics or required props.
- Replacing a slot when slotProps would preserve the built-in behavior.
- Using old components or componentsProps APIs in new code.
- Dropping refs, className, event handlers, or aria props in a wrapper.
- Targeting generated hash class names instead of stable utility classes.
- Using sx for a style that should be shared across many components.
- Adding a global theme override for a single exceptional instance.
- Overriding a component's internal structure with private DOM assumptions.
- Removing focus visibility during visual customization.

## Recap

- Compose components through children when the content structure is the customization point.
- Use component to select a supported root element and slots or slotProps to customize documented internal parts.
- Choose sx for local styles, styled for repeated component styling, and theme overrides for shared defaults.
- Preserve semantics, refs, accessibility behavior, and stable public APIs when wrapping components.

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
      <td><a href="033-date-and-time-pickers.md">033 - Date and Time Pickers</a></td>
      <td><a href="../qnas/034-component-composition-slots-and-customization.md">Q&amp;A</a></td>
      <td><a href="../examples/034-component-composition-slots-and-customization.md">Example</a></td>
      <td><a href="035-typescript-with-material-ui.md">035 - TypeScript with Material UI</a></td>
    </tr>
  </tbody>
</table>
