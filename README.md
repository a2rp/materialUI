# Material UI Practical Notes

A structured, hands-on learning guide for Material UI (MUI), the React component library. These notes cover project setup, theming, styling, responsive layouts, accessible components, MUI X, testing, and production integration.

## Scope and version

This course focuses on the Material UI component library published as `@mui/material` and its companion MUI packages. The current stable Material UI documentation identifies v9 as the recommended production version as of 2026-11-26. Examples use the v9 API where version-specific behavior matters. Check the version installed in your project before copying an example because APIs and peer dependency ranges can change.

Material UI, MUI System, MUI X, and Base UI are related but distinct packages. This course focuses on Material UI and introduces selected MUI X packages where they provide separate components such as the Data Grid and date/time pickers.

## Learning outcomes

By following the roadmap, you will learn to:

- Install Material UI in an existing React project and understand its peer dependencies.
- Build interfaces from Material UI components and compose them into reusable patterns.
- Customize a shared theme for colors, typography, spacing, breakpoints, component defaults, and dark mode.
- Apply component styles with `sx`, `styled`, theme overrides, and CSS.
- Create responsive layouts and interactive forms with accessible controls.
- Use navigation, feedback, overlay, and data display components.
- Distinguish Material UI core components from separately installed MUI X packages and their licensing tiers.
- Integrate Material UI with TypeScript, testing tools, server rendering, and production workflows.

## Prerequisites

- Basic JavaScript and React knowledge, including JSX, props, state, and composition.
- Familiarity with HTML, CSS, and responsive design fundamentals.
- Node.js and a package manager such as npm, pnpm, or yarn.
- TypeScript is useful for the TypeScript chapter, but the earlier chapters can be followed with JavaScript.

## Quick guides

- [Material UI summary](materialUI-summary.md)
- [Material UI keywords and concepts](materialUI-keywords.md)

## Getting started

Material UI is added to an existing React project. The default styling engine uses Emotion:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

Install the icon package only when your application needs Material UI's icon set:

```sh
npm install @mui/icons-material
```

The application must already have compatible `react` and `react-dom` peer dependencies. Follow the installation requirements for the exact Material UI version used by your project. MUI X components are separate packages and are introduced in their chapters.

## How to use these notes

1. Read the lesson for a chapter.
2. Use its Q&A file to review decisions, common mistakes, and edge cases.
3. Work through the matching example and adapt it in a small React project.
4. Keep the installed package versions and framework setup in mind when applying version-sensitive guidance.

## Repository structure

- `lessons/` contains the chapter explanations.
- `qnas/` contains chapter questions and answers.
- `examples/` contains hands-on examples and practice tasks.
- `materialUI-summary.md` is a topic-wide review.
- `materialUI-keywords.md` is an index of Material UI terms and APIs.
- `LICENSE` contains the repository license.

## Course roadmap

| No. | Lesson | Q&A | Example |
| ---: | --- | --- | --- |
| 001 | [Introduction to Material UI and the MUI ecosystem](lessons/001-introduction-to-material-ui-and-mui-ecosystem.md) | [Open Q&A](qnas/001-introduction-to-material-ui-and-mui-ecosystem.md) | [Open example](examples/001-introduction-to-material-ui-and-mui-ecosystem.md) |
| 002 | [Project setup and installation](lessons/002-project-setup-and-installation.md) | [Open Q&A](qnas/002-project-setup-and-installation.md) | [Open example](examples/002-project-setup-and-installation.md) |
| 003 | [Component structure and imports](lessons/003-component-structure-and-imports.md) | [Open Q&A](qnas/003-component-structure-and-imports.md) | [Open example](examples/003-component-structure-and-imports.md) |
| 004 | [Themes, ThemeProvider, and CssBaseline](lessons/004-themes-themeprovider-and-cssbaseline.md) | [Open Q&A](qnas/004-themes-themeprovider-and-cssbaseline.md) | [Open example](examples/004-themes-themeprovider-and-cssbaseline.md) |
| 005 | [Theme tokens and design system](lessons/005-theme-tokens-and-design-system.md) | [Open Q&A](qnas/005-theme-tokens-and-design-system.md) | [Open example](examples/005-theme-tokens-and-design-system.md) |
| 006 | [The sx prop and theme-aware styles](lessons/006-sx-prop-and-theme-aware-styles.md) | [Open Q&A](qnas/006-sx-prop-and-theme-aware-styles.md) | [Open example](examples/006-sx-prop-and-theme-aware-styles.md) |
| 007 | [The styled API and reusable styles](lessons/007-styled-api-and-reusable-styles.md) | [Open Q&A](qnas/007-styled-api-and-reusable-styles.md) | [Open example](examples/007-styled-api-and-reusable-styles.md) |
| 008 | [CSS overrides and global styles](lessons/008-css-overrides-and-global-styles.md) | [Open Q&A](qnas/008-css-overrides-and-global-styles.md) | [Open example](examples/008-css-overrides-and-global-styles.md) |
| 009 | [Responsive design and breakpoints](lessons/009-responsive-design-and-breakpoints.md) | [Open Q&A](qnas/009-responsive-design-and-breakpoints.md) | [Open example](examples/009-responsive-design-and-breakpoints.md) |
| 010 | [Color schemes, dark mode, and CSS variables](lessons/010-color-schemes-dark-mode-and-css-variables.md) | [Open Q&A](qnas/010-color-schemes-dark-mode-and-css-variables.md) | [Open example](examples/010-color-schemes-dark-mode-and-css-variables.md) |
| 011 | [Typography](lessons/011-typography.md) | [Open Q&A](qnas/011-typography.md) | [Open example](examples/011-typography.md) |
| 012 | [Box, Container, and Stack layout](lessons/012-box-container-and-stack-layout.md) | [Open Q&A](qnas/012-box-container-and-stack-layout.md) | [Open example](examples/012-box-container-and-stack-layout.md) |
| 013 | [Grid layout](lessons/013-grid-layout.md) | [Open Q&A](qnas/013-grid-layout.md) | [Open example](examples/013-grid-layout.md) |
| 014 | [Buttons, icon buttons, and button groups](lessons/014-buttons-icon-buttons-and-button-groups.md) | [Open Q&A](qnas/014-buttons-icon-buttons-and-button-groups.md) | [Open example](examples/014-buttons-icon-buttons-and-button-groups.md) |
| 015 | [Text fields and input components](lessons/015-text-fields-and-input-components.md) | [Open Q&A](qnas/015-text-fields-and-input-components.md) | [Open example](examples/015-text-fields-and-input-components.md) |
| 016 | [Selects, menus, and native selects](lessons/016-selects-menus-and-native-selects.md) | [Open Q&A](qnas/016-selects-menus-and-native-selects.md) | [Open example](examples/016-selects-menus-and-native-selects.md) |
| 017 | [Checkboxes, radio buttons, and switches](lessons/017-checkboxes-radio-buttons-and-switches.md) | [Open Q&A](qnas/017-checkboxes-radio-buttons-and-switches.md) | [Open example](examples/017-checkboxes-radio-buttons-and-switches.md) |
| 018 | [Form control, validation, and accessibility](lessons/018-form-control-validation-and-accessibility.md) | [Open Q&A](qnas/018-form-control-validation-and-accessibility.md) | [Open example](examples/018-form-control-validation-and-accessibility.md) |
| 019 | [Autocomplete](lessons/019-autocomplete.md) | [Open Q&A](qnas/019-autocomplete.md) | [Open example](examples/019-autocomplete.md) |
| 020 | [Paper, cards, and accordions](lessons/020-paper-cards-and-accordions.md) | [Open Q&A](qnas/020-paper-cards-and-accordions.md) | [Open example](examples/020-paper-cards-and-accordions.md) |
| 021 | [Lists, dividers, and list items](lessons/021-lists-dividers-and-list-items.md) | [Open Q&A](qnas/021-lists-dividers-and-list-items.md) | [Open example](examples/021-lists-dividers-and-list-items.md) |
| 022 | [AppBar, Toolbar, and top navigation](lessons/022-appbar-toolbar-and-top-navigation.md) | [Open Q&A](qnas/022-appbar-toolbar-and-top-navigation.md) | [Open example](examples/022-appbar-toolbar-and-top-navigation.md) |
| 023 | [Drawers and navigation patterns](lessons/023-drawers-and-navigation-patterns.md) | [Open Q&A](qnas/023-drawers-and-navigation-patterns.md) | [Open example](examples/023-drawers-and-navigation-patterns.md) |
| 024 | [Tabs and tab panels](lessons/024-tabs-and-tab-panels.md) | [Open Q&A](qnas/024-tabs-and-tab-panels.md) | [Open example](examples/024-tabs-and-tab-panels.md) |
| 025 | [Dialogs and modal workflows](lessons/025-dialogs-and-modal-workflows.md) | [Open Q&A](qnas/025-dialogs-and-modal-workflows.md) | [Open example](examples/025-dialogs-and-modal-workflows.md) |
| 026 | [Popovers, tooltips, and click-away behavior](lessons/026-popovers-tooltips-and-click-away.md) | [Open Q&A](qnas/026-popovers-tooltips-and-click-away.md) | [Open example](examples/026-popovers-tooltips-and-click-away.md) |
| 027 | [Alerts, snackbars, and feedback](lessons/027-alerts-snackbars-and-feedback.md) | [Open Q&A](qnas/027-alerts-snackbars-and-feedback.md) | [Open example](examples/027-alerts-snackbars-and-feedback.md) |
| 028 | [Progress indicators and skeletons](lessons/028-progress-indicators-and-skeletons.md) | [Open Q&A](qnas/028-progress-indicators-and-skeletons.md) | [Open example](examples/028-progress-indicators-and-skeletons.md) |
| 029 | [Chips, avatars, and badges](lessons/029-chips-avatars-and-badges.md) | [Open Q&A](qnas/029-chips-avatars-and-badges.md) | [Open example](examples/029-chips-avatars-and-badges.md) |
| 030 | [Tables and data display](lessons/030-tables-and-data-display.md) | [Open Q&A](qnas/030-tables-and-data-display.md) | [Open example](examples/030-tables-and-data-display.md) |
| 031 | [MUI X Data Grid Community](lessons/031-mui-x-data-grid-community.md) | [Open Q&A](qnas/031-mui-x-data-grid-community.md) | [Open example](examples/031-mui-x-data-grid-community.md) |
| 032 | [Icons and icon buttons](lessons/032-icons-and-icon-buttons.md) | [Open Q&A](qnas/032-icons-and-icon-buttons.md) | [Open example](examples/032-icons-and-icon-buttons.md) |
| 033 | [Date and time pickers](lessons/033-date-and-time-pickers.md) | [Open Q&A](qnas/033-date-and-time-pickers.md) | [Open example](examples/033-date-and-time-pickers.md) |
| 034 | [Component composition, slots, and customization](lessons/034-component-composition-slots-and-customization.md) | [Open Q&A](qnas/034-component-composition-slots-and-customization.md) | [Open example](examples/034-component-composition-slots-and-customization.md) |
| 035 | [TypeScript with Material UI](lessons/035-typescript-with-material-ui.md) | [Open Q&A](qnas/035-typescript-with-material-ui.md) | [Open example](examples/035-typescript-with-material-ui.md) |
| 036 | [Accessibility and keyboard interaction](lessons/036-accessibility-and-keyboard-interaction.md) | [Open Q&A](qnas/036-accessibility-and-keyboard-interaction.md) | [Open example](examples/036-accessibility-and-keyboard-interaction.md) |
| 037 | [Localization, RTL, and internationalization](lessons/037-localization-rtl-and-internationalization.md) | [Open Q&A](qnas/037-localization-rtl-and-internationalization.md) | [Open example](examples/037-localization-rtl-and-internationalization.md) |
| 038 | [Server-side rendering and Next.js](lessons/038-server-side-rendering-and-nextjs.md) | [Open Q&A](qnas/038-server-side-rendering-and-nextjs.md) | [Open example](examples/038-server-side-rendering-and-nextjs.md) |
| 039 | [Testing Material UI components](lessons/039-testing-material-ui-components.md) | [Open Q&A](qnas/039-testing-material-ui-components.md) | [Open example](examples/039-testing-material-ui-components.md) |
| 040 | [Performance, bundle size, and tree shaking](lessons/040-performance-bundle-size-and-tree-shaking.md) | [Open Q&A](qnas/040-performance-bundle-size-and-tree-shaking.md) | [Open example](examples/040-performance-bundle-size-and-tree-shaking.md) |
| 041 | [Form library integration](lessons/041-form-library-integration.md) | [Open Q&A](qnas/041-form-library-integration.md) | [Open example](examples/041-form-library-integration.md) |
| 042 | [Production design systems and maintenance](lessons/042-production-design-system-and-maintenance.md) | [Open Q&A](qnas/042-production-design-system-and-maintenance.md) | [Open example](examples/042-production-design-system-and-maintenance.md) |

## Course status

The roadmap is established. Chapter lessons, Q&A, and examples are being completed in order, with each artifact committed and pushed separately.

## License

This repository is distributed under the license in [LICENSE](LICENSE).
