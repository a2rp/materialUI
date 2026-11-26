# Color Schemes, Dark Mode, and CSS Variables - Example

## Goal

Add light and dark color schemes, allow the user to choose System, Light, or Dark mode, and style a component with theme tokens.

## Prerequisites

- A client-rendered React application with Material UI and Emotion installed.
- An application root that can wrap the page with ThemeProvider.
- A server-rendered app should follow the SSR note near the end of this example.

## File structure

```text
src/
  App.jsx
  main.jsx
  theme.js
```

## 1. Configure both schemes

Add `src/theme.js`:

```jsx
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  cssVariables: {
    colorSchemeSelector: 'class',
  },
  colorSchemes: {
    light: {
      palette: {
        primary: {
          main: '#1565c0',
        },
        background: {
          default: '#f4f7fb',
          paper: '#ffffff',
        },
      },
    },
    dark: {
      palette: {
        primary: {
          main: '#90caf9',
        },
        background: {
          default: '#10151d',
          paper: '#1b2430',
        },
      },
    },
  },
});

export default theme;
```

The class selector allows the app to switch schemes manually. The theme provides the light and dark palette values as CSS variables.

## 2. Add the theme provider

Update `src/main.jsx`:

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import CssBaseline from '@mui/material/CssBaseline';
import { ThemeProvider } from '@mui/material/styles';
import App from './App';
import theme from './theme';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <ThemeProvider theme={theme} defaultMode="system">
      <CssBaseline />
      <App />
    </ThemeProvider>
  </StrictMode>,
);
```

Keep the root element ID from the React starter if it differs from root.

## 3. Add a color-mode control

Update `src/App.jsx`:

```jsx
import { useColorScheme } from '@mui/material/styles';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

function ModeSelect() {
  const { mode, setMode } = useColorScheme();

  if (!mode) {
    return null;
  }

  return (
    <label>
      Color mode
      <select
        value={mode}
        onChange={(event) => setMode(event.target.value)}
      >
        <option value="system">System</option>
        <option value="light">Light</option>
        <option value="dark">Dark</option>
      </select>
    </label>
  );
}

export default function App() {
  return (
    <Box
      component="main"
      sx={{
        minHeight: '100vh',
        bgcolor: 'background.default',
        color: 'text.primary',
        p: { xs: 2, md: 5 },
      }}
    >
      <Stack spacing={2} sx={{ maxWidth: 680, mx: 'auto' }}>
        <Typography variant="h4" component="h1">
          Appearance
        </Typography>

        <ModeSelect />

        <Paper
          sx={[
            {
              p: 3,
              border: 1,
              borderColor: 'divider',
            },
            (theme) =>
              theme.applyStyles('dark', {
                borderColor: theme.vars.palette.grey[800],
              }),
          ]}
        >
          <Stack spacing={2}>
            <Typography variant="h6">Theme-aware surface</Typography>
            <Typography color="text.secondary">
              Palette tokens update when the selected color scheme changes.
            </Typography>
            <Button variant="contained">Save preference</Button>
          </Stack>
        </Paper>
      </Stack>
    </Box>
  );
}
```

The control waits until the mode is initialized. The Paper uses shared palette tokens for its normal styles and applyStyles for a dark-specific border value.

## Run and inspect

Start the client-rendered application with its normal development command. Choose System, Light, and Dark from the select control. Also change the operating system's color preference while System is selected.

## Expected result

The page's background, surface, text, and primary action adapt to the active scheme. The same React content remains in the tree while CSS variables change the colors. System follows the browser preference, while Light and Dark select fixed modes.

## What to notice

- The theme defines both schemes and enables CSS variables.
- ThemeProvider makes the theme and color-mode API available.
- useColorScheme supplies mode and setMode.
- mode can be undefined during initialization.
- CSS variable tokens update without requiring each component to calculate raw colors.
- applyStyles is used for a scheme-specific rule instead of checking palette.mode during render.

## Server-rendering note

This example uses a client-rendered React root. For an SSR app with a manual class selector, add InitColorSchemeScript in the framework's document or root layout before the application content. Set its attribute and default mode to match the theme and ThemeProvider. Follow the framework and router-specific setup, and account for the document attribute during hydration.

## Variations

1. Change the selector from class to data and update the SSR initialization attribute if applicable.
2. Add scheme-specific error and success colors.
3. Add a visible text label for the active mode for assistive technology.
4. Remove the System option and set a fixed default if the product does not offer system preference.

## Common mistakes

- **Using a media selector with a manual select:** the browser preference selector does not respond to setMode.
- **Rendering a mode-dependent select before mode is ready:** handle the initial undefined value.
- **Choosing raw colors throughout the app:** use palette tokens where possible.
- **Checking palette.mode for style output:** use applyStyles for mode-specific CSS.
- **Forgetting SSR initialization for manual selectors:** the server cannot read the browser's saved mode.
- **Using an initialization script with a mismatched selector:** align its attribute with the theme.
- **Assuming all visual states are accessible because colors change:** review focus, error, disabled, and contrast states in both schemes.

## Practice check

Set System mode, change the browser preference, then explicitly choose Light and Dark. Explain which selection should follow the system and why the SSR setup needs an initialization script for a manual selector.

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
      <td><a href="009-responsive-design-and-breakpoints.md">009 - Responsive design and breakpoints</a></td>
      <td><a href="../lessons/010-color-schemes-dark-mode-and-css-variables.md">Lesson</a></td>
      <td><a href="../qnas/010-color-schemes-dark-mode-and-css-variables.md">Q&amp;A</a></td>
      <td><a href="011-typography.md">011 - Typography</a></td>
    </tr>
  </tbody>
</table>
