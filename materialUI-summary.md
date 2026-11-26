# Material UI Summary

A topic-wide review of Material UI (MUI), the React component library. The examples and terminology in this course target the stable Material UI v9 API as of 2026-11-26. Always compare version-sensitive code with the version installed in the project.

## Mental model

Material UI components are React components. They receive props, compose into a React tree, and read shared design values from a theme supplied through React context. MUI System translates theme-aware style props into CSS. The default styling engine is Emotion.

A useful model is:

1. React owns application state and component composition.
2. Material UI provides accessible, themeable UI building blocks.
3. The theme defines the shared visual language.
4. Component props configure one instance; theme defaults and overrides define consistent application-wide behavior.
5. CSS and browser layout still determine the final rendered result.

Material UI does not replace React, semantic HTML, CSS knowledge, application state management, or accessibility review.

## Installation and package boundaries

For an existing React project, the default core packages are:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

Add icons only if needed:

```sh
npm install @mui/icons-material
```

React and React DOM are peer dependencies. Install compatible versions and review package requirements when upgrading.

- `@mui/material` contains the core Material UI components.
- `@mui/system` contains styling utilities and system primitives.
- `@mui/icons-material` provides icon components in a separate package.
- MUI X packages are separately installed. The Data Grid Community package and date/time picker package are distinct from core Material UI.
- MUI X Community packages are MIT-licensed. Pro and Premium packages or features require a commercial license. Confirm feature availability before adopting a package.

## Theming

Create a theme once and provide it near the application root. Descendant components can consume its palette, typography, spacing, breakpoints, shape, shadows, transitions, and component configuration.

```tsx
import { CssBaseline } from '@mui/material';
import { createTheme, ThemeProvider } from '@mui/material/styles';

const theme = createTheme({
  cssVariables: true,
  palette: {
    primary: { main: '#3157a4' },
    secondary: { main: '#8b3f7a' },
  },
  typography: {
    fontFamily: 'Arial, sans-serif',
  },
  shape: {
    borderRadius: 8,
  },
});

export function AppRoot() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <App />
    </ThemeProvider>
  );
}
```

`ThemeProvider` provides theme context. `CssBaseline` establishes a consistent baseline. A theme's `components` section can configure component `defaultProps`, `styleOverrides`, and `variants`. Keep shared design decisions in the theme and local one-off adjustments near the component that needs them.

When CSS variables and color schemes are enabled, use supported theme-variable APIs for scheme-dependent styles. Avoid designing dark-mode styles around render-time assumptions that can cause a server/client mismatch or flicker.

## Styling choices

| Need | Suitable tool |
| --- | --- |
| A small local style on one component | `sx` |
| A repeated, named styled component | `styled` |
| App-wide component defaults or consistent variants | Theme `components` configuration |
| A global reset or a small set of global rules | `CssBaseline` or `GlobalStyles` |
| A complex layout or animation | CSS, with MUI components and theme values where useful |

The `sx` prop understands theme values such as spacing, palette colors, and breakpoint keys. Use arrays or responsive objects for styles that change by breakpoint. The `styled` utility is useful for reusable components that need theme access or prop filtering. Keep custom CSS selectors targeted to documented component slots and state classes.

## Layout and responsive design

- Use `Container` for a centered page region with a readable maximum width.
- Use `Stack` for one-dimensional rows or columns with consistent gaps.
- Use `Grid` for responsive grid layouts.
- Use `Box` when a generic wrapper or system style props are useful.
- Use CSS Flexbox or CSS Grid when their native layout model expresses the design more clearly.
- Use theme breakpoints for responsive behavior and test at narrow and wide widths.
- Use `useMediaQuery` only when the React behavior itself must change. Prefer CSS responsive styles for visual-only changes.

MUI spacing values are interpreted using the active theme's spacing scale. Confirm whether a property expects a number, CSS length, or responsive value instead of assuming every numeric prop is a raw pixel count.

## Component selection

- Actions: `Button`, `IconButton`, `ButtonGroup`.
- Text input: `TextField` or lower-level input components.
- Selection: `Select`, `Autocomplete`, `Checkbox`, `RadioGroup`, `Switch`.
- Form structure: `FormControl`, `InputLabel`, `FormHelperText`.
- Content surfaces: `Paper`, `Card`, `Accordion`.
- Navigation: `AppBar`, `Toolbar`, `Drawer`, `Tabs`, `List`.
- Overlays: `Dialog`, `Popover`, `Tooltip`, `Menu`.
- Feedback: `Alert`, `Snackbar`, `CircularProgress`, `LinearProgress`, `Skeleton`.
- Data presentation: `Table`, `Chip`, `Avatar`, `Badge`, and MUI X Data Grid where its richer behavior is justified.

Prefer the highest-level component that fits the interaction. For example, use `TextField` for a conventional labeled text input; use lower-level input pieces when the design or composition requires them.

## Forms

A form control should have a label, a stable value model, clear validation feedback, and a submit path. Use controlled inputs when the application must own and validate the current value. Use `FormControl` and related label/helper components when coordinating lower-level inputs. Provide helpful error text and connect it to the control. Do not use color as the only error signal.

Autocomplete is appropriate for a searchable choice control. Decide whether users may choose only from supplied options or enter free text, and configure option identity and labels accordingly. When a form library is added, keep one clear source of truth for each field and follow the integration library's controlled/uncontrolled conventions.

## Accessibility

- Prefer semantic elements and accessible names.
- Use visible labels for form controls.
- Preserve keyboard operation and visible focus.
- Ensure dialogs manage focus and have an accessible title.
- Give icon-only controls an accessible name.
- Use status text or icons in addition to color for important states.
- Check contrast, zoom, reduced motion, and screen-reader behavior.
- Do not add ARIA attributes when native HTML semantics already provide the needed meaning.

## MUI X

MUI X provides advanced components in separate packages. Data Grid supports tabular interactions beyond a basic semantic table, including features such as sorting, pagination, and virtualization. Start with the Community package when its capabilities are sufficient. Check the exact feature matrix and license for Pro or Premium features; do not assume every feature belongs to the MIT package.

Date and time pickers also require a compatible date adapter and a date library. Keep displayed values, storage format, locale, time zone, and validation rules explicit.

## TypeScript

Use Material UI's exported component and theme types rather than broad untyped objects. Module augmentation extends the theme type when custom fields, palette entries, or component variants are added. Keep runtime theme configuration and compile-time type augmentation aligned.

## Server rendering

Server-rendered applications need the styling integration recommended for their framework and router. The server and client should produce compatible markup and style ordering. For Next.js, use the integration package and entry point that match the Next.js major version and router being used. Avoid copying an integration snippet for a different router or version.

## Testing

Test behavior through user-visible semantics:

- Query controls by role and accessible name.
- Type, click, and navigate with realistic user interactions.
- Assert validation, loading, open/closed state, and resulting application behavior.
- Avoid coupling tests to generated Emotion class names or internal implementation details.
- Add focused tests for custom theme behavior and integration boundaries where they matter.

## Performance and maintainability

- Import components directly from their package entry points in the documented form.
- Avoid importing an entire icon collection or creating unnecessary runtime style work.
- Use code splitting for large, rarely used feature areas where it improves the application.
- Virtualize genuinely large data collections; do not add a Data Grid to a small static table without a need.
- Keep package versions compatible and upgrade intentionally.
- Centralize shared tokens and repeated component defaults in the theme.
- Build small reusable wrappers only when they simplify the product API, not just to rename every MUI component.

## Practical checklist

Before shipping a Material UI screen, check:

1. The package and component are available in the installed version and license tier.
2. The theme provider wraps the relevant component tree.
3. Responsive behavior works at small and large widths.
4. Inputs, overlays, and icon buttons have accessible names and keyboard behavior.
5. Loading, empty, validation, and error states are visible and understandable.
6. Custom styles use theme tokens consistently and avoid brittle selectors.
7. Tests assert behavior instead of generated class names.
8. Server-rendered styles do not flicker or mismatch during hydration.
