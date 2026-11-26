# Material UI Keywords and Concepts

A practical index of Material UI, MUI System, and selected MUI X terminology. This catalog describes concepts used in the course, not JavaScript or TypeScript reserved words. The course targets the stable Material UI v9 API as of 2026-11-26; check installed package versions for version-specific details.

## Product and package names

| Term | Meaning |
| --- | --- |
| Material UI | React component library implementing Material Design and providing a customizable component set. |
| MUI | The umbrella name used for the Material UI ecosystem. |
| MUI System | Styling utilities and layout primitives that power theme-aware styling. |
| MUI X | Separately packaged advanced components such as Data Grid and date/time pickers. |
| Base UI | A separate headless component library; it is not the same package as Material UI. |
| `@mui/material` | Main Material UI component package. |
| `@mui/system` | MUI System package for `sx`, `styled`, and system utilities. |
| `@mui/icons-material` | Material Design icon components, installed separately. |
| `@mui/x-data-grid` | MIT-licensed Community Data Grid package. |
| `@mui/x-data-grid-pro` | Commercial Pro Data Grid package with additional features. |
| `@mui/x-data-grid-premium` | Commercial Premium Data Grid package with the broadest feature set. |
| `@mui/x-date-pickers` | Community date and time picker package. |
| `@mui/lab` | MUI package for components that may be experimental or not yet part of the stable core API; check each component's status. |
| Emotion | Default CSS-in-JS styling engine used by Material UI. |
| Peer dependency | A package that the consuming application must install in a compatible version, such as React. |

## Application and component architecture

| Term | Meaning |
| --- | --- |
| Component | Reusable React UI building block such as `Button`, `TextField`, or `Dialog`. |
| Composition | Combining small components to create a larger interface. |
| Props | Inputs that configure a React component. |
| Controlled component | Component state whose value is supplied and updated by the parent. |
| Uncontrolled component | Component that maintains its own value, optionally starting from a default value. |
| Polymorphic component | Component that can render a different underlying element through a supported prop. |
| `component` prop | Material UI prop commonly used to replace a component's root element or component type. |
| Slot | A named structural part of a component that can be customized. |
| `slots` | Component prop for replacing one or more internal slot components. |
| `slotProps` | Component prop for passing properties to internal slots. |
| `ownerState` | Internal state and props used by some styled component APIs to determine styles. |
| Ref forwarding | Passing a React ref through a component to a DOM node or supported child component. |
| Portal | Rendering a component subtree elsewhere in the DOM, commonly used by overlays. |
| Modal | Overlay interaction that manages focus and blocks interaction with background content. |
| Transition | Visual state change, often used when overlays or content appear and disappear. |

## Theme and design tokens

| Term | Meaning |
| --- | --- |
| Theme | Shared design configuration consumed by components and styling APIs. |
| `createTheme` | Function that creates a Material UI theme with defaults and custom options. |
| `ThemeProvider` | React provider that makes a theme available to descendant components. |
| `useTheme` | Hook that reads the active theme inside a React component. |
| Theme nesting | Providing a more specific theme to a subtree. |
| Design token | Named design value such as a palette color, spacing step, radius, or type style. |
| `palette` | Theme colors and semantic color roles. |
| `primary` | Main brand or action color family. |
| `secondary` | Supporting color family. |
| `error` | Semantic color role for error states. |
| `warning` | Semantic color role for warning states. |
| `info` | Semantic color role for informational states. |
| `success` | Semantic color role for successful states. |
| `contrastText` | Text color chosen to contrast with a palette color. |
| `typography` | Theme configuration for font families, sizes, weights, line heights, and variants. |
| `spacing` | Theme spacing scale and helper used by layout and styling props. |
| `breakpoints` | Named viewport thresholds used for responsive layout rules. |
| `shape` | Theme shape settings such as the default border radius. |
| `shadows` | Theme elevation shadow values. |
| `zIndex` | Theme stacking levels for overlays and fixed UI. |
| `transitions` | Theme transition duration and easing helpers. |
| `components` | Theme section for component defaults, variants, and style overrides. |
| `defaultProps` | Theme configuration that supplies default component property values. |
| `styleOverrides` | Theme configuration for overriding component slot styles. |
| `variants` | Theme-defined style rules selected by component properties. |
| `CssBaseline` | Component that applies a consistent baseline style reset and document defaults. |
| `GlobalStyles` | Component for app-wide CSS rules derived from the theme or constants. |

## Styling APIs and CSS concepts

| Term | Meaning |
| --- | --- |
| `sx` | Theme-aware style prop for concise, local component styling. |
| System property | A style property interpreted by MUI System, such as spacing, palette, or breakpoint-aware values. |
| `styled` | Utility for creating a styled component with theme access and optional prop filtering. |
| Theme callback | Function that receives the theme and returns style values. |
| Responsive value | A style value that changes at named breakpoints or for viewport sizes. |
| `cssVariables` | Theme option that generates CSS custom properties for theme values. |
| CSS custom property | CSS variable written using a name such as `--mui-palette-primary-main`. |
| `theme.vars` | Generated theme-variable access for CSS variable mode; it is managed by MUI. |
| `theme.applyStyles` | Theme helper for defining styles for a named color scheme. |
| `colorSchemes` | Theme configuration for multiple schemes such as light and dark. |
| `colorSchemeSelector` | Configuration controlling how CSS selects the active color scheme. |
| `CssVarsProvider` | Older experimental provider API; current theme-variable guidance uses `ThemeProvider`. |
| Emotion cache | Emotion's style insertion and caching configuration, relevant to rendering integration. |
| CSS layer | CSS cascade layer used to manage style precedence. |
| Specificity | CSS rule precedence determined by selectors and cascade order. |
| State class | Component CSS class representing states such as focused, selected, disabled, or expanded. |
| Slot class | Stable CSS class applied to a named internal component part. |
| Global class name | Predictable class name pattern used to target component slots in CSS. |
| `!important` | CSS priority override; use sparingly because it makes styles harder to maintain. |

## Layout and responsive design

| Term | Meaning |
| --- | --- |
| `Box` | General-purpose wrapper that supports system props and component composition. |
| `Container` | Responsive centered layout container with configurable width behavior. |
| `Stack` | One-dimensional layout component for arranging children with spacing and direction. |
| `Grid` | Responsive grid layout component in the current Material UI API. |
| `spacing` prop | Layout prop that applies theme-based gaps or spacing between children. |
| `direction` prop | Layout prop controlling row or column flow. |
| `breakpoint` | Named responsive threshold such as `sm`, `md`, or `lg`. |
| `useMediaQuery` | Hook for responding to CSS media queries in React logic. |
| Responsive typography | Typography values that adapt to viewport sizes or theme breakpoints. |
| Flexbox | CSS layout model often used through Box, Stack, and related style properties. |
| CSS Grid | Two-dimensional CSS layout model available through MUI System style properties. |

## Common core components

| Component or term | Main use |
| --- | --- |
| `Typography` | Text with theme typography variants. |
| `Button` | Standard text action control. |
| `IconButton` | Compact button intended for an icon action. |
| `ButtonGroup` | Visually grouped related buttons. |
| `TextField` | Common text input with integrated label, helper text, and error presentation. |
| `Input` | Lower-level input component. |
| `FormControl` | Context and layout wrapper for coordinated form elements. |
| `InputLabel` | Label component used with form inputs. |
| `FormHelperText` | Supporting or validation text for form controls. |
| `Select` | Select menu control. |
| `MenuItem` | Selectable menu row, often used with Select or Menu. |
| `Checkbox` | Control for independent yes/no or multi-select choices. |
| `Radio` | Control for choosing one option from a group. |
| `RadioGroup` | Context and behavior for a group of radio options. |
| `Switch` | Control for toggling a setting between on and off. |
| `Autocomplete` | Searchable selection control supporting options and custom filtering. |
| `Paper` | Surface with theme background and elevation treatment. |
| `Card` | Container for related content and actions. |
| `Accordion` | Expandable content section. |
| `List` | Structured list container. |
| `ListItem` | Row within a list. |
| `Divider` | Visual separator between content regions. |
| `AppBar` | Top application bar. |
| `Toolbar` | Content layout area commonly placed inside AppBar. |
| `Drawer` | Side panel for navigation or supplementary content. |
| `Tabs` | Tab selection container. |
| `Tab` | One selectable tab. |
| `Dialog` | Modal dialog surface for focused tasks or decisions. |
| `Popover` | Anchored floating content panel. |
| `Tooltip` | Short contextual description shown near a target. |
| `ClickAwayListener` | Utility that detects clicks outside a child region. |
| `Alert` | Inline feedback message with severity. |
| `Snackbar` | Brief temporary feedback surface. |
| `CircularProgress` | Circular progress indicator. |
| `LinearProgress` | Linear progress indicator. |
| `Skeleton` | Placeholder shape used while content is loading. |
| `Chip` | Compact label, filter, or small action. |
| `Avatar` | Image or initials representing a person or entity. |
| `Badge` | Small status or count marker attached to another element. |
| `Table` | Semantic table building blocks for tabular information. |

## MUI X and data components

| Term | Meaning |
| --- | --- |
| MUI X Community | MIT-licensed baseline set of MUI X components and features. |
| MUI X Pro | Commercial package with additional component capabilities. |
| MUI X Premium | Commercial package with the broadest MUI X feature set. |
| `DataGrid` | MUI X tabular component with sorting, pagination, selection, and other grid features. |
| Column definition | Configuration describing a Data Grid column, including its field and rendering. |
| Row identifier | Stable key used by the Data Grid to identify a row. |
| Server-side pagination | Loading pages of rows from a server rather than holding the full dataset in the browser. |
| Virtualization | Rendering only the visible region of a large list or grid. |
| `DatePicker` | Date selection component from the separate MUI X date picker package. |
| Adapter | Date library integration used by MUI X pickers to parse, format, and manipulate values. |
| License key | Commercial MUI X key required only for features or packages under a commercial license. |

## Accessibility and internationalization

| Term | Meaning |
| --- | --- |
| Accessible name | Text or label that identifies an interactive element to assistive technology. |
| Accessible description | Additional help or context associated with an element. |
| ARIA | Attributes that express roles, states, and relationships for assistive technology when native HTML is insufficient. |
| Keyboard interaction | Using keys such as Tab, Enter, Space, and arrow keys to operate controls. |
| Focus management | Moving, trapping, and restoring keyboard focus in interactive UI. |
| Focus visible | A visible indicator showing which control currently receives keyboard input. |
| Color contrast | Difference between foreground and background luminance that supports text readability. |
| Semantic HTML | Native HTML elements whose meaning and built-in behavior support browsers and assistive technology. |
| Right-to-left (RTL) | Layout direction used by languages written from right to left. |
| Localization | Adapting text, date, number, and component labels to a locale. |
| Internationalization | Designing an application so that it can support multiple languages and regional formats. |
| Locale text | Translated labels and messages supplied to components. |

## TypeScript, rendering, and quality

| Term | Meaning |
| --- | --- |
| Theme augmentation | TypeScript module augmentation used to add custom theme fields or component variants to types. |
| Type narrowing | Refining a TypeScript union based on a check before using a value. |
| Server-side rendering (SSR) | Rendering React markup on the server before the client takes over. |
| Static site generation (SSG) | Producing HTML at build time for pages that can be generated ahead of requests. |
| Hydration | Attaching client-side React behavior to server-rendered HTML. |
| Style flicker | A visible mismatch when server-rendered and client-rendered styles are inserted or selected differently. |
| Testing Library | Testing tools that encourage assertions based on accessible user-facing behavior. |
| User event | Simulated realistic interaction such as typing, clicking, or tabbing. |
| Tree shaking | Build-time removal of unused exports from bundled code. |
| Bundle size | Downloaded JavaScript and CSS volume shipped to a user. |
| Code splitting | Loading parts of an application only when needed. |
| Peer version alignment | Ensuring related React, MUI, and adapter packages satisfy one another's version requirements. |
| Migration | Updating code and configuration when moving between library versions. |

## Distinctions to remember

- A Material UI component is not automatically part of the core `@mui/material` package. Some advanced components require separate MUI X packages.
- MUI X Community is MIT-licensed; some Pro and Premium packages and features require a commercial license.
- `sx` is suited to local one-off styles; `styled` and theme overrides help centralize reusable styling.
- A theme token is shared configuration; a component prop is a per-instance input.
- A component slot is an internal structural part, not a React child slot unless the API defines it that way.
- Material UI does not replace React, HTML, CSS, accessibility decisions, or application state management.
- An API described as experimental or deprecated must be checked against the installed version before use.
