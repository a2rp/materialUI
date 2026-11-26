# Theme Tokens and the Design System - Example

## Goal

Create a small shared design system in the Material UI theme and use its palette, typography, spacing, responsive breakpoints, and Button defaults in a screen.

## Prerequisites

- A React application with Material UI and Emotion installed.
- ThemeProvider and CssBaseline configured at the application root as in Chapter 004.

## File structure

```text
src/
  App.jsx
  theme.js
```

## 1. Configure shared tokens

Update `src/theme.js`:

```jsx
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  palette: {
    primary: {
      main: '#1565c0',
    },
    secondary: {
      main: '#7b1fa2',
    },
    success: {
      main: '#2e7d32',
    },
    background: {
      default: '#f5f7fa',
      paper: '#ffffff',
    },
  },
  typography: {
    fontFamily: 'Arial, sans-serif',
    h1: {
      fontSize: '2.25rem',
      fontWeight: 700,
      lineHeight: 1.15,
    },
    body1: {
      lineHeight: 1.6,
    },
  },
  shape: {
    borderRadius: 8,
  },
  components: {
    MuiButton: {
      defaultProps: {
        disableElevation: true,
      },
      styleOverrides: {
        root: {
          textTransform: 'none',
        },
      },
    },
  },
});

export default theme;
```

The palette colors and shared button rules are examples. Check foreground/background contrast for the design you use. Do not assume a generated contrast color is correct for every custom brand combination.

## 2. Use the tokens in a page

Update `src/App.jsx`:

```jsx
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

export default function App() {
  return (
    <Box
      component="main"
      sx={{
        minHeight: '100vh',
        bgcolor: 'background.default',
        p: { xs: 2, md: 5 },
      }}
    >
      <Paper
        sx={{
          maxWidth: 720,
          mx: 'auto',
          p: { xs: 2, sm: 4 },
          borderRadius: 2,
        }}
      >
        <Stack spacing={2}>
          <Typography variant="h1" color="primary">
            Design tokens in use
          </Typography>

          <Typography variant="body1">
            The theme keeps shared visual choices consistent.
          </Typography>

          <Box
            sx={{
              display: 'flex',
              flexDirection: { xs: 'column', sm: 'row' },
              gap: 1,
            }}
          >
            <Button variant="contained" color="primary">
              Primary action
            </Button>
            <Button variant="outlined" color="secondary">
              Secondary action
            </Button>
          </Box>
        </Stack>
      </Paper>
    </Box>
  );
}
```

The root provider must use this theme module for these values to take effect.

## Run and inspect

Start the React application's development server using its normal command. Inspect the page at a narrow and a wide viewport. The app should keep a readable card width, use responsive padding, and stack its actions vertically on a narrow screen.

## What to notice

- The theme maps the semantic primary and secondary roles to concrete colors.
- Typography uses a shared font family and a configured h1 variant.
- The page uses palette roles instead of repeating color literals.
- Numeric padding and gap values use the theme spacing system.
- The display direction changes at the sm breakpoint.
- The theme provides a shared Button default, while the page still chooses each button's variant and color.
- Component defaults do not determine the page's full layout.

## Variations

1. Change the palette's primary color and see which components update.
2. Add an error role and use it for an error message.
3. Define one shared shape rule and compare it with a local border-radius choice.
4. Add another page with the same Button to see which theme rules are reusable.
5. Use a contrast checker on the chosen text and background colors before shipping.

## Common mistakes

- **Hard-coding a brand value in every component:** prefer a semantic token for repeated values.
- **Assuming every numeric sx property means pixels or spacing:** behavior depends on the property.
- **Using a typography variant as the only semantic decision:** verify the rendered heading element too.
- **Putting page-specific layout in MuiButton overrides:** keep each rule at the level where it belongs.
- **Assuming palette generation guarantees contrast:** review the actual colors and states.
- **Expecting theme changes without the root provider:** confirm ThemeProvider imports this theme.

## Practice check

Change the primary token once and identify which parts of the page update. Then choose one local spacing value and explain why it belongs in the page rather than the global theme.

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
      <td><a href="004-themes-themeprovider-and-cssbaseline.md">004 - Themes, ThemeProvider, and CssBaseline</a></td>
      <td><a href="../lessons/005-theme-tokens-and-design-system.md">Lesson</a></td>
      <td><a href="../qnas/005-theme-tokens-and-design-system.md">Q&amp;A</a></td>
      <td><a href="006-sx-prop-and-theme-aware-styles.md">006 - The sx prop and theme-aware styles</a></td>
    </tr>
  </tbody>
</table>
