# 042 - Production Design Systems and Maintenance Example

This example turns a small set of product-wide decisions into a maintainable Material UI setup. It includes semantic palette choices, global component defaults, a reusable action component, and a user-controlled light or dark scheme.

The code is written for an existing React and TypeScript application using Vite. It illustrates structure and APIs; it has not been installed, run, or compiled as part of this notes update.

## Goals

By the end, the example will show:

- One place for shared theme decisions.
- An intentional wrapper API for a repeated product action.
- A color-scheme toggle using Material UI's color-scheme context.
- A clear boundary between shared rules and one-off layout styles.
- An adoption checklist for testing and documentation.

## Prerequisites

- An existing React and TypeScript application.
- Node.js and npm installed for that application.
- A project entry point that renders a React component.
- Familiarity with Material UI components, themes, and the `sx` prop.

Install the Material UI packages in the existing application:

~~~sh
npm install @mui/material @emotion/react @emotion/styled
~~~

This example does not use icons, so the icon package is not required.

## File structure

~~~text
src/
  design-system/
    ActionButton.tsx
    theme.ts
  App.tsx
  main.tsx
~~~

## 1. Define the theme

Create `src/design-system/theme.ts`:

~~~ts
import { createTheme } from '@mui/material/styles';

export const theme = createTheme({
  cssVariables: {
    colorSchemeSelector: 'data',
  },
  colorSchemes: {
    light: {
      palette: {
        primary: {
          main: '#155EEF',
        },
        background: {
          default: '#F7F9FC',
          paper: '#FFFFFF',
        },
      },
    },
    dark: {
      palette: {
        primary: {
          main: '#9DB9FF',
        },
        background: {
          default: '#101522',
          paper: '#171E2B',
        },
      },
    },
  },
  shape: {
    borderRadius: 12,
  },
  typography: {
    fontFamily: 'Inter, Arial, sans-serif',
    button: {
      fontWeight: 650,
      textTransform: 'none',
    },
  },
  components: {
    MuiButton: {
      defaultProps: {
        disableElevation: true,
      },
      styleOverrides: {
        root: {
          borderRadius: 10,
          minHeight: 40,
        },
      },
    },
    MuiCard: {
      styleOverrides: {
        root: {
          borderColor: 'var(--mui-palette-divider)',
          borderRadius: 16,
        },
      },
    },
  },
});
~~~

The palette values are grouped by scheme, while the rest of the theme defines decisions shared by both. The button defaults are broad rules for all MUI buttons in this application. If one screen needs a special width or margin, keep that local to its layout instead of expanding the global theme.

The theme uses one `createTheme` argument. When composing optional theme fragments, merge them into one options object first instead of relying on multiple arguments to `createTheme`.

## 2. Add a product-level action component

Create `src/design-system/ActionButton.tsx`:

~~~tsx
import Button, { type ButtonProps } from '@mui/material/Button';

type ActionButtonProps = Omit<ButtonProps, 'variant'> & {
  intent?: 'primary' | 'secondary';
};

export function ActionButton({
  intent = 'primary',
  ...buttonProps
}: ActionButtonProps) {
  const variant = intent === 'primary' ? 'contained' : 'outlined';

  return <Button {...buttonProps} variant={variant} />;
}
~~~

This component adds one product decision: the application uses `intent` to select one of two approved action treatments. The remaining supported MUI button props are available to consumers, including labels, links, disabled state, and size.

Do not add a new wrapper for every color or isolated page style. Add a product-level component when its API and behavior are repeated and worth maintaining.

## 3. Add a color-scheme control

Create `src/ColorModeToggle.tsx`:

~~~tsx
import Button from '@mui/material/Button';
import { useColorScheme } from '@mui/material/styles';

export function ColorModeToggle() {
  const { mode, setMode } = useColorScheme();

  return (
    <Button
      aria-label="Toggle color scheme"
      onClick={() => setMode(mode === 'dark' ? 'light' : 'dark')}
      variant="outlined"
    >
      Toggle color scheme
    </Button>
  );
}
~~~

The label remains stable instead of changing based on the current mode. If the first render does not yet know the active mode, this avoids rendering different button text before and after hydration. If a product needs to display the current mode, account for the initial unresolved value explicitly.

## 4. Compose a screen from the shared system

Create `src/App.tsx`:

~~~tsx
import Alert from '@mui/material/Alert';
import Box from '@mui/material/Box';
import Card from '@mui/material/Card';
import CardContent from '@mui/material/CardContent';
import Container from '@mui/material/Container';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import { ActionButton } from './design-system/ActionButton';
import { ColorModeToggle } from './ColorModeToggle';

export default function App() {
  return (
    <Container maxWidth="md">
      <Stack spacing={3} sx={{ py: 5 }}>
        <Stack
          alignItems={{ xs: 'stretch', sm: 'center' }}
          direction={{ xs: 'column', sm: 'row' }}
          justifyContent="space-between"
          spacing={2}
        >
          <Box>
            <Typography component="p" color="text.secondary" variant="overline">
              Design system
            </Typography>
            <Typography component="h1" variant="h4">
              Product settings
            </Typography>
          </Box>
          <ColorModeToggle />
        </Stack>

        <Alert severity="info">
          Shared tokens and component defaults are applied by the theme.
        </Alert>

        <Card variant="outlined">
          <CardContent>
            <Stack spacing={2}>
              <Typography component="h2" variant="h6">
                Workspace profile
              </Typography>
              <Typography color="text.secondary">
                Update details that appear to your teammates.
              </Typography>
              <Stack direction={{ xs: 'column', sm: 'row' }} spacing={1.5}>
                <ActionButton href="#save">Save changes</ActionButton>
                <ActionButton href="#cancel" intent="secondary">
                  Cancel
                </ActionButton>
              </Stack>
            </Stack>
          </CardContent>
        </Card>
      </Stack>
    </Container>
  );
}
~~~

The shared theme controls the page palette, button defaults, and card border. The screen uses `sx` for its own responsive layout. If another settings page needs the same structure and behavior, consider making that layout a reusable composition rather than duplicating it.

## 5. Mount the theme

Create `src/main.tsx`:

~~~tsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import CssBaseline from '@mui/material/CssBaseline';
import { ThemeProvider } from '@mui/material/styles';
import App from './App';
import { theme } from './design-system/theme';

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <ThemeProvider defaultMode="system" theme={theme}>
      <CssBaseline />
      <App />
    </ThemeProvider>
  </StrictMode>,
);
~~~

The provider makes the theme and color-scheme context available to the screen. `CssBaseline` applies the Material UI baseline. Keep a single top-level provider unless the application has a specific reason for nested themes.

## 6. Start the application

Use the existing Vite scripts from the project root, for example:

~~~sh
npm run dev
~~~

Expected behavior:

- The page starts with the browser's system color scheme.
- The toggle switches between light and dark.
- Buttons share the theme's defaults.
- The card uses the same shape and border treatment as other cards.
- The responsive action row stacks on narrow screens and aligns horizontally on wider screens.

These are expected results from the code, not a claim that this example was run.

## Server-rendered applications

This file structure is for a client-rendered Vite application. For a server-rendered application with a color-scheme toggle, also configure the rendering framework's document root to render `InitColorSchemeScript` as the first child of `body`. Use the same color-scheme selector and default mode as the theme and provider. Follow the framework's own Material UI integration so styles and hydration are configured correctly.

Do not add the script to a client-only page without a need for pre-hydration scheme selection.

## Adoption checklist

Before promoting a local rule into a shared system, review:

1. **Reuse:** Is the need present in more than one real screen?
2. **Meaning:** Does the token or component name communicate its role?
3. **API:** Are props and slots focused, understandable, and typed?
4. **Accessibility:** Are labels, focus, disabled, error, and contrast states covered?
5. **Schemes and sizes:** Does it work in each supported scheme and breakpoint?
6. **Testing:** Which component tests, accessibility checks, and visual reviews are appropriate?
7. **Documentation:** Can a consumer find when and how to use it?
8. **Ownership:** Who reviews changes and responds to feedback?
9. **Release:** Are consumers informed if behavior or appearance changes?

## Variations

- Add semantic palette roles for error, warning, success, text, surfaces, and borders as the product needs them.
- Introduce shared spacing or typography decisions when repeated screens demonstrate a stable scale.
- Add a documented action intent only when its meaning differs from the existing primary and secondary actions.
- Use `theme.applyStyles('dark', styles)` when a specific custom style needs different values in the dark scheme.
- Create a reusable form section only after multiple screens share its layout and behavior.
- For stronger product isolation, move design-system components into a package with its own version and release notes.

## Common mistakes

- Using palette names as semantic roles throughout application code.
- Putting page-specific margin and width rules in the global theme.
- Wrapping a component without adding an actual product rule.
- Turning off interaction feedback globally without checking focus visibility.
- Using a generated CSS class from one browser render as a selector.
- Treating the color toggle as an accessibility substitute for sufficient contrast.
- Forgetting to review the server-rendered first paint when adding dark mode.
- Releasing shared token changes without checking important consuming screens.

## Practice tasks

1. Add a status token for a real application state and identify every component that consumes it.
2. Add an accessible loading state to `ActionButton` without changing its primary and secondary intent contract.
3. Pick one local `sx` rule and decide whether it is truly repeated enough to promote.
4. Write a short change note describing which screens need visual review after changing the primary color.

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
      <td><a href="041-form-library-integration.md">041 - Form library integration example</a></td>
      <td><a href="../lessons/042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance</a></td>
      <td><a href="../qnas/042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance Q&amp;A</a></td>
      <td>End of course</td>
    </tr>
  </tbody>
</table>