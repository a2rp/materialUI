# Typography - Example

## Goal

Build a small account settings page that uses a shared typography scale, preserves a meaningful heading outline, and remains readable as the viewport changes.

## Prerequisites

- A React application with Material UI and Emotion installed.
- The Material UI theme and Typography basics from the matching lesson.
- A development server configured by the host application.

## Example: a responsive settings page

Add this as \`App.jsx\`. The application's existing entry point should render \`<App />\`.

```jsx
import { useMemo } from 'react';
import {
  createTheme,
  responsiveFontSizes,
  ThemeProvider,
} from '@mui/material/styles';
import CssBaseline from '@mui/material/CssBaseline';
import Box from '@mui/material/Box';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

function createAppTheme() {
  let theme = createTheme({
    typography: {
      fontFamily: [
        'Inter',
        'Arial',
        'sans-serif',
      ].join(','),
      h1: {
        fontSize: '2rem',
        fontWeight: 700,
        lineHeight: 1.15,
        letterSpacing: '-0.02em',
      },
      h2: {
        fontSize: '1.5rem',
        fontWeight: 650,
        lineHeight: 1.25,
      },
      body1: {
        fontSize: '1rem',
        lineHeight: 1.6,
      },
      body2: {
        fontSize: '0.875rem',
        lineHeight: 1.5,
      },
    },
  });

  theme = responsiveFontSizes(theme);
  return theme;
}

function SettingsSection({ id, title, children }) {
  return (
    <Paper
      component="section"
      variant="outlined"
      aria-labelledby={id}
      sx={{ p: 3 }}
    >
      <Stack spacing={1.5}>
        <Typography id={id} variant="h2" component="h2">
          {title}
        </Typography>
        {children}
      </Stack>
    </Paper>
  );
}

export default function App() {
  const theme = useMemo(() => createAppTheme(), []);

  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <Box
        component="main"
        sx={{
          maxWidth: 760,
          mx: 'auto',
          px: { xs: 2, sm: 3 },
          py: { xs: 4, sm: 6 },
        }}
      >
        <Stack spacing={3}>
          <Box component="header">
            <Typography variant="h1" component="h1" gutterBottom>
              Account settings
            </Typography>
            <Typography variant="body1" color="text.secondary">
              Manage your profile and the messages you receive.
            </Typography>
          </Box>

          <SettingsSection id="profile-heading" title="Profile">
            <Typography variant="body1">
              Your profile information helps teammates recognize your account.
            </Typography>
            <Typography variant="body2" color="text.secondary">
              Changes to your display name appear across the workspace.
            </Typography>
          </SettingsSection>

          <SettingsSection
            id="notifications-heading"
            title="Notifications"
          >
            <Typography variant="body1">
              Choose the updates that are useful to you. You can change these
              preferences at any time.
            </Typography>
            <Typography
              variant="body2"
              color="text.secondary"
              sx={{ overflowWrap: 'anywhere' }}
            >
              Example preference key:
              {' '}
              workspace.notifications.weekly-summary
            </Typography>
          </SettingsSection>

          <Typography variant="caption" color="text.secondary">
            Settings are saved to this browser for this example.
          </Typography>
        </Stack>
      </Box>
    </ThemeProvider>
  );
}
```

The sample uses a system font stack so it does not require a web font download. If you replace it with a custom family, make sure that font is loaded by your application.

## What to notice

- The page has one \`h1\` and uses \`h2\` for each major section.
- Each section is a semantic \`section\` labelled by its visible heading.
- The \`h1\` and \`h2\` visual styles come from the shared theme.
- Supporting text uses \`body2\` and the theme's secondary text color.
- \`responsiveFontSizes\` adjusts the theme typography scale for viewport widths.
- The long preference key can wrap rather than widening or clipping the page.
- \`useMemo\` creates the shared theme once for this example component.

## Expected result

The page shows an account settings heading, two outlined sections, readable paragraphs, and a small supporting note. At narrow widths, the content stays within the viewport and the long preference key can break across lines. The heading structure remains visible in the rendered HTML.

## Verification checklist

1. Inspect the page and confirm exactly one page-level \`h1\`.
2. Confirm the Profile and Notifications headings render as \`h2\` elements.
3. Resize the viewport and check that headings and paragraph text remain readable.
4. Increase browser text size and check for overlap or clipped content.
5. Confirm the font family has a usable fallback when the preferred font is unavailable.
6. Review text and background contrast in the rendered page.
7. Check that no essential copy is hidden with truncation.

## Practice extensions

1. Add a third section with a heading and short description. Keep its semantic level consistent with the existing sections.
2. Change the theme's body line height and compare readability for longer paragraphs.
3. Add one local \`sx\` adjustment to a supporting caption without changing the global caption variant.
4. If you introduce a custom typography variant, decide which HTML element it should render as. In a TypeScript project, add the matching type augmentation.
5. Replace the system font stack with a locally available font and confirm the fallback behavior.

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
      <td><a href="010-color-schemes-dark-mode-and-css-variables.md">010 - Color schemes, dark mode, and CSS variables</a></td>
      <td><a href="../lessons/011-typography.md">Lesson</a></td>
      <td><a href="../qnas/011-typography.md">Q&amp;A</a></td>
      <td><a href="012-box-container-and-stack-layout.md">012 - Box, Container, and Stack layout</a></td>
    </tr>
  </tbody>
</table>
