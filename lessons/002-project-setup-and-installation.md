# Project Setup and Installation

## Learning goals

After this chapter, you should be able to:

- Add Material UI to an existing React project.
- Explain why the Emotion packages are installed with the default setup.
- Add optional icons or MUI X packages only when needed.
- Confirm which package versions your project installed.
- Diagnose common dependency and import problems.

## Start with a React project

Material UI components are React components. Start with a working React project that has a package manifest and a development server. A Vite, Next.js, or other supported React project can be used. The installation command belongs in the project directory, where the correct `package.json` is located.

A minimal project directory might look like:

```text
my-app/
  package.json
  src/
    App.jsx
    main.jsx
```

Do not install the packages in a parent folder that is not the application. Check the current directory before running package-manager commands.

## Install the core component package

The default Material UI styling engine is Emotion. Install the core component package with the Emotion runtime and styled utility:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

With pnpm or yarn, use the matching package-manager command:

```sh
pnpm add @mui/material @emotion/react @emotion/styled
yarn add @mui/material @emotion/react @emotion/styled
```

React and React DOM are peer dependencies. They should already be installed by a React project, and their versions must satisfy the Material UI version's peer dependency range. If you are creating a new React app, use the setup instructions for your chosen React framework, then add Material UI to that app.

The package manager resolves versions and records them in the package manifest and lockfile. Keep the lockfile under version control so another developer or build environment can reproduce the dependency tree.

## Add optional packages deliberately

Install icons separately if the project uses the Material Design icon components:

```sh
npm install @mui/icons-material
```

MUI X components are separate packages. For example, the Community Data Grid uses:

```sh
npm install @mui/x-data-grid
```

Add only packages that the application needs. The core component package does not include every MUI X feature. Some MUI X packages or feature tiers require a commercial license, so confirm the license before selecting them.

## Check the installed dependency tree

Use npm to inspect the direct package versions and dependency tree:

```sh
npm ls @mui/material @emotion/react @emotion/styled react react-dom
```

If using another package manager, use its equivalent list or why command. Compare the output with the package manifest and lockfile. A successful install should not be followed by unresolved peer dependency warnings that were ignored without investigation.

The current Material UI installation guidance lists compatibility with recent React major versions. The supported range can change. Verify compatibility using the exact installed Material UI release rather than assuming every MUI version works with every React version.

For React 18 and earlier, the current installation guidance describes aligning `react-is` with the version of React in some setups. If prop-type checks report element identification problems, inspect the installed `react-is` version and follow the guidance for the project's React version. Do not add overrides unless the dependency versions actually require them.

## Import a component

Once the package is installed, import a component and render it as part of a React component:

```jsx
import Button from '@mui/material/Button';

export default function App() {
  return <Button variant="contained">Start</Button>;
}
```

Material UI documents both direct component-path imports and named imports from the package root. Follow one consistent style in a project. Direct imports make the component package explicit; root imports can be convenient for several components in one file.

## Styling engine choices

Emotion is the default and simplest supported setup. Replacing it with another styling engine changes package installation and often requires bundler aliases or framework-specific configuration. Do not remove Emotion packages just because the application also uses plain CSS, CSS Modules, or another CSS tool. They can coexist.

If there is a real project constraint that requires another engine, follow the integration guidance for the exact Material UI and framework versions. A configuration that works in a client-only development app may not be appropriate for server rendering.

## Framework integration

A client-rendered React starter usually needs no special Material UI setup beyond installing the packages and importing components. Server-rendered frameworks need additional integration so that generated styles are collected and inserted in the correct order. Use the instructions that match both the framework's major version and router. That integration is covered in the server-rendering chapter.

Do not copy Next.js setup into a Vite project or Pages Router setup into an App Router project. The entry files and rendering lifecycle differ.

## Common installation problems

### "Cannot find module '@mui/material'"

Confirm that the install command ran in the application directory and that the package appears in that app's dependencies. Restart the development server after a dependency install if its module graph is stale.

### Emotion-related module errors

The default engine needs `@emotion/react` and `@emotion/styled` alongside `@mui/material`. Restore missing dependencies with the package manager and avoid mixing lockfiles from different package managers.

### Peer dependency warnings

Read the package names and version ranges in the warning. Confirm that React, React DOM, Material UI, and any MUI X packages are compatible. Do not silence a real mismatch with a force-install flag.

### Works locally but not on another machine

Check that the lockfile is committed and that the project uses the same package manager. A clean dependency install should recreate the dependency versions without relying on packages installed globally.

## Practical checklist

Before using the first component, confirm:

1. The terminal is in the React application's directory.
2. The package manager completed without unresolved dependency conflicts.
3. The expected MUI and Emotion packages are in the dependency tree.
4. The project can import a component from `@mui/material`.
5. The development server uses the project's normal start command.
6. Any server-rendering setup matches the framework version and router.

## Recap

Install `@mui/material` and the default Emotion dependencies in the React application, not an unrelated parent folder. React and React DOM must meet the package's peer requirements. Add icons and MUI X separately when needed. Keep the lockfile, inspect peer warnings, and use framework-specific styling integration for server rendering.

## Practice task

In a React starter project, add the core Material UI packages, inspect the dependency tree, and render one Button. Record the package manager and exact installed versions in your project notes. If the install reports a peer warning, identify the package and version range before proceeding.

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
      <td><a href="001-introduction-to-material-ui-and-mui-ecosystem.md">001 - Introduction to Material UI and the MUI ecosystem</a></td>
      <td><a href="../qnas/002-project-setup-and-installation.md">Q&amp;A</a></td>
      <td><a href="../examples/002-project-setup-and-installation.md">Example</a></td>
      <td><a href="003-component-structure-and-imports.md">003 - Component structure and imports</a></td>
    </tr>
  </tbody>
</table>
