# Project Setup and Installation - Example

## Goal

Add Material UI to an existing React app, render one component, and confirm which packages the application resolved. This example is for a React starter that already has a development script.

## Prerequisites

- Node.js installed.
- An existing React application with `package.json`.
- A terminal opened in that app's directory.
- npm, pnpm, or yarn selected by the project.

## 1. Confirm the application directory

Inspect the current directory and its files. You should see the React app's `package.json` and source folder. If the manifest is missing, move into the correct app or create a React app using the framework's setup process before continuing.

A typical Vite structure includes:

```text
my-app/
  package.json
  package-lock.json
  src/
    App.jsx
    main.jsx
```

The exact starter structure can differ.

## 2. Install Material UI

For a project that uses npm, run:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

For pnpm or yarn, use that project's existing package manager. Do not generate a second lockfile in the same application.

## 3. Check the resolved versions

Run:

```sh
npm ls @mui/material @emotion/react @emotion/styled react react-dom
```

Confirm that the expected packages are installed and that the command does not report unresolved peer conflicts. The output reflects the versions resolved for this specific project; do not copy version numbers from another machine.

## 4. Render a basic component

Replace the application component in `src/App.jsx`:

```jsx
import { Button, Stack, Typography } from '@mui/material';

export default function App() {
  return (
    <Stack spacing={2} sx={{ p: 3 }}>
      <Typography variant="h4" component="h1">
        Material UI is installed
      </Typography>
      <Typography color="text.secondary">
        This page uses components from the Material UI core package.
      </Typography>
      <Button variant="contained">Continue</Button>
    </Stack>
  );
}
```

The React entry point should already render the exported App component. If the starter uses a different component filename, edit the file imported by that entry point instead.

## 5. Start the app

For a Vite starter, run:

```sh
npm run dev
```

For another starter, use the development command declared in its package manifest. Open the local address printed by the server.

## Expected result

The page should show a large heading, a muted explanation, and a contained button with Material UI styling. The Stack component arranges the items vertically and applies spacing. The sx prop adds padding through the MUI styling system.

This is a browser-rendered example of the installed library. The notes repository itself does not include a React application or a package manifest.

## Common mistakes

- **Running install in the wrong directory:** package commands modify the manifest in the current directory.
- **Missing Emotion dependencies:** the default Material UI styling engine needs both Emotion packages.
- **Mixing package managers:** keep one project lockfile and use its corresponding package manager.
- **Overlooking peer warnings:** inspect version ranges instead of bypassing npm's resolver.
- **Expecting a server-rendering integration from this setup:** this example uses a client-rendered starter. Server rendering needs framework-specific setup.
- **Assuming MUI X is installed:** this example installs only Material UI core and Emotion.

## Try next

1. Install `@mui/icons-material` and render an icon only if the app needs it.
2. Change the button variant and compare the visual states.
3. Add a theme provider in the next theming chapter.
4. Remove and reinstall dependencies from the lockfile to confirm the project setup is reproducible.

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
      <td><a href="001-introduction-to-material-ui-and-mui-ecosystem.md">001 - Introduction to Material UI and the MUI ecosystem</a></td>
      <td><a href="../lessons/002-project-setup-and-installation.md">Lesson</a></td>
      <td><a href="../qnas/002-project-setup-and-installation.md">Q&amp;A</a></td>
      <td><a href="003-component-structure-and-imports.md">003 - Component structure and imports</a></td>
    </tr>
  </tbody>
</table>
