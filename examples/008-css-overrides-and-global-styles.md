# CSS Overrides and Global Styles - Example

## Goal

Apply styles at three different scopes: a theme-wide Button default, a document-level baseline rule, and a CSS Module rule scoped to one panel.

## Prerequisites

- A React application with Material UI and Emotion installed.
- ThemeProvider and CssBaseline configured at the app root.
- A build tool that supports CSS Modules.

## File structure

```text
src/
  AccountPage.jsx
  theme.js
  AccountPanel.module.css
```

## 1. Add shared theme overrides

Update `src/theme.js`:

```jsx
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
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
    MuiCssBaseline: {
      styleOverrides: (theme) => ({
        body: {
          backgroundColor: theme.palette.background.default,
        },
      }),
    },
  },
});

export default theme;
```

The app root must already pass this theme to ThemeProvider and render one CssBaseline inside that provider.

## 2. Write a scoped CSS Module

Add `src/AccountPanel.module.css`:

```css
.panel {
  max-width: 680px;
  margin: 2rem auto;
  padding: 1.5rem;
}

/* This affects MUI Button slots only inside this panel. */
.panel :global(.MuiButton-root) {
  justify-content: flex-start;
}
```

The local .panel class is scoped by the CSS Modules build tool. The :global selector targets the documented MUI Button root class. This syntax is supported by common CSS Modules implementations; check the syntax expected by your build tool.

## 3. Use a local state style

Add `src/AccountPage.jsx`:

```jsx
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import styles from './AccountPanel.module.css';

export default function AccountPage() {
  return (
    <Box component="main" sx={{ p: 2 }}>
      <Paper className={styles.panel}>
        <Stack spacing={2}>
          <Typography variant="h5" component="h1">
            Account actions
          </Typography>

          <Button variant="outlined">
            Edit contact details
          </Button>

          <Button
            variant="contained"
            disabled
            sx={{
              '&.Mui-disabled': {
                color: 'text.disabled',
              },
            }}
          >
            Export is unavailable
          </Button>
        </Stack>
      </Paper>
    </Box>
  );
}
```

## Run and inspect

Start the React application using the command declared in its package manifest. Inspect the computed styles on both buttons and on the page body.

## Expected result

All Buttons use the theme's text casing and elevation defaults. The body uses the theme background. Buttons inside the account panel align their content to the start. The disabled Button receives the local state-specific sx rule.

## What to notice

- The theme override applies across the application.
- CssBaseline remains a single root-level component.
- The CSS Module rule is limited to one panel.
- The stable MUI Button root class is paired with a local parent selector.
- The disabled state is scoped to the Button instance.
- The sx rule is more local than the theme-wide Button override.

## Optional CSS layer integration

If the application also uses a CSS framework whose utilities need predictable precedence, Material UI v9 supports CSS cascade layers. Configure layer output at the application styling provider and define the layer order in the app's stylesheet according to the framework integration. This is an application-level change and may affect existing overrides; do not enable it only to fix one selector without reviewing the rest of the cascade.

## Common mistakes

- **Using a generated Emotion hash selector:** target the documented .MuiButton-root class.
- **Writing .Mui-disabled alone:** state classes can apply to unrelated components; include the component selector.
- **Expecting the CSS Module to apply globally:** only the explicitly global MUI selector escapes module scoping.
- **Adding another CssBaseline to the page:** keep the baseline at the application root.
- **Putting a one-panel rule in the global theme:** choose the narrowest scope.
- **Adding !important immediately:** inspect computed style, state specificity, and source order first.

## Practice check

Change the panel class selector so it targets a different documented component slot. Explain which rule should affect every Button and which should affect only controls inside the panel.

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
      <td><a href="007-styled-api-and-reusable-styles.md">007 - The styled API and reusable styles</a></td>
      <td><a href="../lessons/008-css-overrides-and-global-styles.md">Lesson</a></td>
      <td><a href="../qnas/008-css-overrides-and-global-styles.md">Q&amp;A</a></td>
      <td><a href="009-responsive-design-and-breakpoints.md">009 - Responsive design and breakpoints</a></td>
    </tr>
  </tbody>
</table>
