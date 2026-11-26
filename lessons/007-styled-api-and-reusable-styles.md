# The styled API and Reusable Styles

## Learning goals

After this chapter, you should be able to:

- Create a reusable styled component with Material UI's styled utility.
- Use the active theme inside a style definition.
- Choose between sx and styled based on how a style is reused.
- Keep style-only props from leaking to rendered DOM elements.
- Understand that styled components can still use sx at an instance.

## What styled does

Material UI exports a styled utility from `@mui/material/styles`. It builds on the configured styling engine and adds Material UI integration, including access to the default theme and support for MUI theme component configuration when the component is named and configured appropriately.

Use styled when the result should be a reusable component with a consistent base appearance or dynamic styling. Define it at module scope so the styled component is not recreated on every render.

## Create a reusable styled component

```jsx
import Paper from '@mui/material/Paper';
import { styled } from '@mui/material/styles';

const SummaryCard = styled(Paper)(({ theme }) => ({
  padding: theme.spacing(3),
  borderLeft: '4px solid ' + theme.palette.primary.main,
  borderRadius: theme.shape.borderRadius,
  backgroundColor: theme.palette.background.paper,
  transition: theme.transitions.create('box-shadow'),
  '&:hover': {
    boxShadow: theme.shadows[3],
  },
}));

export default SummaryCard;
```

The callback receives the active theme. The style object is ordinary CSS-style configuration, so use CSS property names such as padding and backgroundColor. Theme functions such as spacing and transitions keep the reusable component aligned with shared design values.

## Use the styled component

```jsx
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import SummaryCard from './SummaryCard';

function Overview() {
  return (
    <SummaryCard>
      <Stack spacing={1}>
        <Typography variant="h6">Monthly overview</Typography>
        <Typography color="text.secondary">
          A reusable surface for summary content.
        </Typography>
      </Stack>
    </SummaryCard>
  );
}
```

SummaryCard remains a Paper-based component, so it can contain normal React children and accept the documented Paper props. Do not assume a styled component discards the underlying component's behavior.

## styled and sx serve different use cases

| API | Good fit |
| --- | --- |
| sx | One-off styles for a component instance, short responsive adjustments, local theme-aware values. |
| styled | A reusable named component whose base styling is shared in multiple places. |
| Theme component overrides | Product-wide defaults and rules for Material UI components. |
| Global CSS | Rules for document-level or non-MUI elements that are easier to express globally. |

The APIs can work together. A styled component created with Material UI's styled utility supports the sx prop, so a caller can apply a local adjustment without changing the reusable base:

```jsx
<SummaryCard sx={{ maxWidth: 680, mx: 'auto' }}>
  Content
</SummaryCard>
```

Keep the base component's purpose clear. If callers need arbitrary style controls that change its visual contract, consider a documented variant prop or theme variant rather than a growing collection of unrelated local overrides.

## Dynamic styles from props

A styled callback can use component props to select styles. When adding a custom prop only for styling, prevent it from being forwarded to a DOM node. For example, a custom styled HTML element can filter a prop with shouldForwardProp.

```jsx
import { styled } from '@mui/material/styles';

const StatusPanel = styled('section', {
  shouldForwardProp: (prop) => prop !== 'tone',
})(({ theme, tone = 'info' }) => ({
  padding: theme.spacing(2),
  borderLeft: '4px solid ' + theme.palette[tone].main,
}));
```

Restrict tone to known palette roles in the component's public API. In TypeScript, type the custom prop so callers cannot pass an arbitrary value. Verify which props the underlying styled API filters automatically before adding a custom shouldForwardProp function.

## Theme integration

The Material UI styled utility can be configured with a component name and slot. That setup allows theme component overrides and variants to apply to a custom styled component. Use it when the component is intentionally part of the app's design system and has a stable public name.

For small reusable components, it is fine to keep the styles near the component without integrating them into the global theme. Avoid adding internal-looking names to the theme unless the application needs centralized defaults or overrides for that component.

## Style organization

A useful path is:

1. Use sx for a one-instance adjustment.
2. Extract repeated styles into a styled component.
3. Promote repeated product-wide patterns into theme component configuration.
4. Use global CSS for document-level behavior.

This keeps local decisions local and shared design decisions centralized. It also avoids adding a styled wrapper for a component used in exactly one place with two simple style properties.

## Common mistakes

- **Defining styled components inside render:** this can create a new component type on each render.
- **Using sx for a large repeated style block:** extract it when reuse justifies a named component.
- **Creating styled components for trivial one-off styles:** use sx when it is clearer.
- **Forwarding style-only props to HTML:** filter custom props where required.
- **Reimplementing the underlying component's behavior:** keep the wrapped component and its props unless intentionally changing the API.
- **Using a custom styled utility with the wrong theme provider:** use the utility and provider from the same MUI styling system.

## Recap

Material UI's styled utility creates reusable theme-aware components. Define them outside render, use the active theme for shared values, and use sx for local instance changes. Filter style-only custom props from DOM elements and add theme integration only when the reusable component needs centralized configuration.

## Practice task

Create a styled Paper-based notice panel with a theme palette border and spacing. Use it in two places with different local sx widths. Explain which styles are part of the shared component and which belong to each instance.

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
      <td><a href="006-sx-prop-and-theme-aware-styles.md">006 - The sx prop and theme-aware styles</a></td>
      <td><a href="../qnas/007-styled-api-and-reusable-styles.md">Q&amp;A</a></td>
      <td><a href="../examples/007-styled-api-and-reusable-styles.md">Example</a></td>
      <td><a href="008-css-overrides-and-global-styles.md">008 - CSS overrides and global styles</a></td>
    </tr>
  </tbody>
</table>
