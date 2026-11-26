# Introduction to Material UI and the MUI Ecosystem - Example

## Goal

Build a small status card in an existing React application. The example shows how Material UI components compose inside JSX and how component props connect presentation to React state.

## Prerequisites

- An existing React project with a development server, such as a Vite React starter.
- React and React DOM installed in that project.
- Material UI and its default Emotion styling dependencies.

Install the packages from the project directory if needed:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

## File structure

Use the application entry point already created by your React starter. The relevant file can be:

```text
src/
  App.jsx
  main.jsx
```

Replace the contents of `src/App.jsx` with this component:

```jsx
import { useState } from 'react';
import {
  Box,
  Button,
  Chip,
  Paper,
  Stack,
  Typography,
} from '@mui/material';

export default function App() {
  const [saved, setSaved] = useState(false);

  function saveDraft() {
    setSaved(true);
  }

  function resetDraft() {
    setSaved(false);
  }

  return (
    <Box
      component="main"
      sx={{
        minHeight: '100vh',
        p: { xs: 2, md: 5 },
        bgcolor: 'background.default',
      }}
    >
      <Paper
        elevation={2}
        sx={{
          maxWidth: 640,
          mx: 'auto',
          p: { xs: 2, sm: 4 },
        }}
      >
        <Stack spacing={2}>
          <Typography variant="h4" component="h1">
            Material UI status card
          </Typography>

          <Typography color="text.secondary">
            This card is built by composing React components and configuring
            them with props.
          </Typography>

          <Chip
            label={saved ? 'Saved' : 'Draft'}
            color={saved ? 'success' : 'default'}
            size="small"
            sx={{ alignSelf: 'flex-start' }}
          />

          <Stack direction={{ xs: 'column', sm: 'row' }} spacing={1}>
            <Button variant="contained" onClick={saveDraft}>
              Save draft
            </Button>
            <Button variant="outlined" onClick={resetDraft}>
              Reset
            </Button>
          </Stack>
        </Stack>
      </Paper>
    </Box>
  );
}
```

The starter's `src/main.jsx` should render `<App />` into its root element. A typical Vite React starter already does this, so leave it unchanged unless your starter uses a different entry point.

## Run the example

From the React project directory, start the development server:

```sh
npm run dev
```

Open the local address printed by the development server. The page should show a centered card with a heading, explanatory text, a Draft chip, and two buttons.

## What to notice

- `App` is a React component. It owns the `saved` state with `useState`.
- `Box` renders a semantic `main` element because its `component` prop is set to `"main"`.
- `Paper` supplies a surface with elevation and theme-aware padding.
- `Stack` arranges children and applies consistent gaps.
- `Typography` uses a theme variant and renders the heading as an `h1`.
- `Chip` changes its label and color based on React state.
- `Button` uses props for its visual variant and click behavior.
- The responsive objects in `sx` use theme breakpoints to adjust padding and layout.

Clicking Save draft changes the chip from Draft to Saved. Clicking Reset returns it to Draft. The UI changes because React state changes; Material UI does not store this draft or send it to a server.

## Variations

1. Add a third button with a text variant and make it clear the card without changing the saved state.
2. Add a small helper message below the chip that explains its current state.
3. Change the maximum width and responsive padding, then check the screen at a narrow viewport.
4. Place the action buttons in reverse order on small screens and decide whether that improves the flow.

## Common mistakes

- **Forgetting to install Emotion:** the default Material UI styling engine needs its packages.
- **Importing from the wrong package:** core components such as Button and Paper come from `@mui/material`.
- **Expecting Save draft to persist data:** the example only updates local component state. Persistence requires application logic and usually a server or browser storage.
- **Using color as the only state indicator:** the Chip also changes its text, so the state remains understandable without relying only on color.
- **Using a visual heading without semantic structure:** the heading sets `component="h1"` so its document meaning is explicit.
- **Copying the file into a non-React project:** this JSX component requires a React build setup.

## Practice check

Before continuing, explain which props control layout, which prop changes the rendered semantic element, and which React state value changes the visible status. Then add a second status and decide how to make its text and visual treatment clear.

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
      <td>Start of course</td>
      <td><a href="../lessons/001-introduction-to-material-ui-and-mui-ecosystem.md">Lesson</a></td>
      <td><a href="../qnas/001-introduction-to-material-ui-and-mui-ecosystem.md">Q&amp;A</a></td>
      <td><a href="002-project-setup-and-installation.md">002 - Project setup and installation</a></td>
    </tr>
  </tbody>
</table>
