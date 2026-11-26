# Component Composition, Slots, and Customization: Settings Panel Example

This example builds a small settings panel that demonstrates several customization layers without replacing Material UI's built-in behavior. It includes composed children, a reusable styled button, a theme default, a one-off sx rule, a Tooltip slot configuration, a ref-forwarding wrapper, and a link rendered through the component prop.

## What you will build

- A settings card assembled from Material UI children.
- A one-off styled Button using sx.
- A reusable QuietButton built with styled.
- A shared theme default and root style override.
- A Tooltip configured through slots and slotProps.
- A custom Button wrapper that forwards refs and props.
- A navigation link rendered with Button's component prop.

## Requirements

In an existing React application, install Material UI and its default styling engine:

```bash
npm install @mui/material @emotion/react @emotion/styled
```

## `src/App.jsx`

Replace the starter component with this file:

```jsx
import { forwardRef } from "react";
import {
  Button,
  Card,
  CardActions,
  CardContent,
  Fade,
  Stack,
  ThemeProvider,
  Tooltip,
  Typography,
} from "@mui/material";
import { createTheme, styled } from "@mui/material/styles";

const appTheme = createTheme({
  components: {
    MuiButton: {
      defaultProps: {
        disableElevation: true,
      },
      styleOverrides: {
        root: {
          borderRadius: 10,
          textTransform: "none",
        },
      },
    },
  },
});

const QuietButton = styled(Button)(({ theme }) => ({
  color: theme.palette.text.primary,
  borderColor: theme.palette.divider,
  "&:hover": {
    borderColor: theme.palette.text.secondary,
    backgroundColor: theme.palette.action.hover,
  },
}));

const CompactAction = forwardRef(function CompactAction(
  { size = "small", ...props },
  ref,
) {
  return <Button ref={ref} size={size} {...props} />;
});

export default function App() {
  return (
    <ThemeProvider theme={appTheme}>
      <Stack spacing={3} sx={{ maxWidth: 640, mx: "auto", p: 3 }}>
        <Typography component="h1" variant="h4" fontWeight={700}>
          Account settings
        </Typography>

        <Card variant="outlined">
          <CardContent>
            <Typography variant="h6" gutterBottom>
              Notification preferences
            </Typography>
            <Typography color="text.secondary">
              Choose how project updates reach your team.
            </Typography>
          </CardContent>
          <CardActions sx={{ flexWrap: "wrap", gap: 1, px: 2, pb: 2 }}>
            <Button
              variant="contained"
              sx={{
                px: 3,
                "&:focus-visible": {
                  outline: "2px solid",
                  outlineOffset: 2,
                },
              }}
            >
              Save preferences
            </Button>

            <QuietButton variant="outlined">
              Cancel
            </QuietButton>

            <Tooltip
              describeChild
              title="Opens the notification settings"
              slots={{ transition: Fade }}
              slotProps={{
                popper: { placement: "bottom" },
                tooltip: { sx: { maxWidth: 240 } },
              }}
            >
              <CompactAction
                aria-label="Open notification settings"
                component="a"
                href="#notification-settings"
              >
                More settings
              </CompactAction>
            </Tooltip>

            <Button component="a" href="#help">
              Read help
            </Button>
          </CardActions>
        </Card>
      </Stack>
    </ThemeProvider>
  );
}
```

## `src/main.jsx`

Use the Vite entry point to mount the panel:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

## How the example works

### Children composition

Card contains CardContent and CardActions. Each component owns its own part of the layout, so the application can change content without rebuilding the card's internal behavior.

### Root component semantics

The help and settings controls use component="a" with a local fragment destination. They remain links for navigation. Replace the fragments with real internal destinations or a router link in the application.

### Slots and slotProps

Tooltip uses the Fade transition through slots. Its popper placement and tooltip width are configured with slotProps, which preserves the built-in Popper and Tooltip components.

### One-off and reusable styling

The Save preferences Button has an sx rule for this single instance. QuietButton uses styled because the appearance is reusable. Both styles read from theme values where appropriate.

### Theme defaults

The ThemeProvider supplies disableElevation and the shared root styles to Buttons in the panel. A caller can still override default props where the component API allows it. The theme keeps application-wide decisions in one place.

### Ref forwarding

CompactAction forwards the ref and remaining props to Button. Tooltip can therefore attach the DOM ref and event handlers it needs. A wrapper that drops those values can break the composition.

## Try these changes

1. Change the Tooltip popper placement and observe the result.
2. Remove the one-off focus-visible rule and compare the browser's keyboard focus treatment.
3. Add a different application-wide Button default to appTheme.
4. Change QuietButton's hover style by using another theme token.
5. Replace the fragment destinations with routes in the application.
6. Remove ref forwarding from CompactAction temporarily and inspect the warning from Tooltip.

## Common mistakes

- Replacing an internal slot when slotProps can configure the existing component.
- Using old componentsProps or slot-specific props instead of slots and slotProps.
- Dropping ref, className, event handlers, or accessibility attributes in a wrapper.
- Styling a one-off button through a global theme rule.
- Using a root element that does not match the control's semantic purpose.
- Targeting generated hash classes instead of global utility classes.
- Removing keyboard focus visibility while customizing hover or ripple styles.
- Adding a global theme override that affects unrelated buttons.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous example</th>
      <th>Chapter lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Next example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="033-date-and-time-pickers.md">033 - Date and Time Pickers</a></td>
      <td><a href="../lessons/034-component-composition-slots-and-customization.md">Lesson</a></td>
      <td><a href="../qnas/034-component-composition-slots-and-customization.md">Q&amp;A</a></td>
      <td><a href="035-typescript-with-material-ui.md">035 - TypeScript with Material UI</a></td>
    </tr>
  </tbody>
</table>
