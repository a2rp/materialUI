# Project Setup and Installation - Q&A

## 1. Do I need a React project before installing Material UI?

Yes. Material UI provides React components, so install it inside a React application that has its own package manifest and build setup. If you are creating a new app, create it with your chosen React framework first.

## 2. What is the standard installation command?

For npm, install Material UI and its default Emotion styling dependencies:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

Use the equivalent add command for pnpm or yarn when that is the package manager used by the project.

## 3. Why are Emotion packages installed with Material UI?

Emotion is the default styling engine. The React runtime and styled utility are required for the standard Material UI styling setup. Plain CSS or CSS Modules can still be used in the same project.

## 4. Are React and React DOM included with Material UI?

No. They are peer dependencies. A React application normally already installs them, but their versions must satisfy the peer dependency range of the installed Material UI release.

## 5. How do I know which Material UI version I installed?

Inspect the dependency list and manifest:

```sh
npm ls @mui/material @emotion/react @emotion/styled react react-dom
```

The manifest declares dependency ranges, while the lockfile records the resolved versions. Check both when diagnosing a mismatch.

## 6. Should I use npm, pnpm, or yarn?

Use the package manager already adopted by the project. Avoid creating multiple lockfiles or installing the same dependency tree with different package managers, because that can lead to inconsistent resolutions.

## 7. Do I need to install the icon package?

Only if the project imports Material UI's icon components. The icon package is separate:

```sh
npm install @mui/icons-material
```

A project can also use another icon set or its own SVG assets.

## 8. Are MUI X components included in `@mui/material`?

No. MUI X components are installed separately. For example, the Community Data Grid is published in `@mui/x-data-grid`. Check the package's peer dependencies and feature licensing before adding it.

## 9. Why does npm print a peer dependency warning?

A dependency's declared version range may not match a package already installed in the project. Read the warning and identify the package pair and versions. Resolve the mismatch using compatible versions rather than immediately using force or legacy peer dependency flags.

## 10. What is the `react-is` issue for React 18 and earlier?

Current Material UI installation guidance calls out aligning `react-is` with React in some projects using React 18 or earlier. This helps avoid element identification problems in prop-type checks when versions differ. Confirm the guidance for the exact React and Material UI versions in the app before adding a package override.

## 11. Can I install Material UI in the parent workspace?

Install it in the specific app that imports its components. If a monorepo has multiple applications, add dependencies to the relevant workspace package using that repository's workspace conventions. Installing in an unrelated parent folder can make module resolution confusing.

## 12. Do I need special configuration in Vite?

A normal client-rendered Vite React app usually only needs the core packages installed and components imported. Server rendering, custom styling engines, or special CSS cascade requirements may need additional configuration.

## 13. Do I need special configuration in Next.js?

For server rendering, yes, use the Material UI integration that matches the Next.js major version and router. Pages Router and App Router use different integration entry points. A client-only starter snippet is not a substitute for the server-rendering setup.

## 14. Can I replace Emotion with styled-components?

There are alternative styling-engine integrations, but they require package and bundler configuration and may have framework constraints. Keep Emotion unless there is a concrete requirement to switch, and follow instructions matching the installed Material UI and framework versions.

## 15. Does a successful package installation prove that the app is configured correctly?

No. It confirms that the package manager resolved and installed dependencies. You still need to import and render a component, and server-rendered apps need the appropriate style collection and ordering setup.

## 16. What should I commit after installation?

Commit the application manifest and the lockfile used by its package manager. This keeps declared dependencies and their resolved versions reproducible for teammates and deployment builds.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous Q&amp;A</th>
      <th>Chapter lesson</th>
      <th>Chapter example</th>
      <th>Next Q&amp;A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="001-introduction-to-material-ui-and-mui-ecosystem.md">001 - Introduction to Material UI and the MUI ecosystem</a></td>
      <td><a href="../lessons/002-project-setup-and-installation.md">Lesson</a></td>
      <td><a href="../examples/002-project-setup-and-installation.md">Example</a></td>
      <td><a href="003-component-structure-and-imports.md">003 - Component structure and imports</a></td>
    </tr>
  </tbody>
</table>
