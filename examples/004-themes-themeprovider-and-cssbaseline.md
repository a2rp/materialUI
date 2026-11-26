# Themes, ThemeProvider, and CssBaseline - Example

## Goal

Create one application theme, provide it to the React tree, and apply CssBaseline once at the root. Then render a screen whose components use the theme.

## Prerequisites

- An existing React project.
- Material UI and Emotion installed.
- A React entry file such as `src/main.jsx` or the equivalent in your starter.

## File structure

```text
src/
  App.jsx
  main.jsx
  theme.js
```

## 1. Create the theme module

Add `src/theme.js`:

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
  },
  typography: {
    fontFamily: 'Arial, sans-serif',
  },
  shape: {
    borderRadius: 8,
  },
});

export default theme;
```

The theme is a stable module value. The next chapter explains these theme tokens in more detail.

## 2. Provide it at the application root

Update `src/main.jsx` to wrap the rendered application:

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import CssBaseline from '@mui/material/CssBaseline';
import { ThemeProvider } from '@mui/material/styles';
import App from './App';
import theme from './theme';

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <App />
    </ThemeProvider>
  </StrictMode>,
);
```

If the starter's root element ID or entry path differs, keep its existing values and change only the relevant imports and wrapper structure.

## 3. Render themed components

Add or update `src/App.jsx`:

```jsx
import Button from '@mui/material/Button';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

export default function App() {
  return (
    <Paper
      component="main"
      elevation={0}
      sx={{ maxWidth: 560, mx: 'auto', mt: 6, p: 3 }}
    >
      <Stack spacing={2}>
        <Typography variant="h4" component="h1">
          Theme is available
        </Typography>
        <Typography color="text.secondary">
          The heading and button use the active Material UI theme.
        </Typography>
        <Button variant="contained" color="secondary">
          Continue
        </Button>
      </Stack>
    </Paper>
  );
}
```

## 4. Run the app

Use the development command in the project manifest. For a Vite project, the usual command is:

```sh
npm run dev
```

## Expected result

The page displays a centered surface, a heading, supporting text, and a purple contained button. The theme provider supplies the configured palette and typography. CssBaseline removes the browser's default body margin and applies baseline document styles.

## What to notice

- ThemeProvider is above App in the React tree.
- CssBaseline is rendered once within ThemeProvider.
- Descendant components receive the custom theme through context.
- The page still defines its own width, spacing, and content hierarchy.
- StrictMode checks React behavior in development; it does not provide theme values.
- A component rendered outside ThemeProvider would use the default theme instead.

## Variations

1. Change the primary and secondary palette values, then inspect the Button.
2. Remove CssBaseline and compare the document's default body margin.
3. Move App outside ThemeProvider temporarily and identify which custom styles change.
4. Add a nested provider around a deliberately separate section and observe which subtree receives it.

## Common mistakes

- **Importing the wrong ThemeProvider:** use the provider exported from Material UI's styles package.
- **Putting the provider inside App's child subtree:** components outside it cannot read the custom theme.
- **Creating CssBaseline on every route:** keep it at the application boundary.
- **Expecting CssBaseline to lay out the page:** define layout in components and styles.
- **Using the wrong root element ID:** retain the ID supplied by the React starter.
- **Recreating theme on every render:** keep theme options in a stable module unless they truly depend on changing runtime values.

## Practice check

Move one themed component above ThemeProvider and compare it with the same component below the provider. Explain what the provider changes and what CssBaseline changes.

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
      <td><a href="003-component-structure-and-imports.md">003 - Component structure and imports</a></td>
      <td><a href="../lessons/004-themes-themeprovider-and-cssbaseline.md">Lesson</a></td>
      <td><a href="../qnas/004-themes-themeprovider-and-cssbaseline.md">Q&amp;A</a></td>
      <td><a href="005-theme-tokens-and-design-system.md">005 - Theme tokens and design system</a></td>
    </tr>
  </tbody>
</table>
