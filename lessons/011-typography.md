# Typography

## Learning goals

After this chapter, you should be able to:

- Choose a Material UI typography variant for a clear content hierarchy.
- Keep visual styling separate from the semantic HTML element.
- Customize the typography scale in the theme.
- Use responsive font sizes, line height, and text wrapping intentionally.
- Check text contrast, readability, and heading order.

## Why typography needs a system

Typography gives an interface a readable structure. A consistent system helps people scan titles, section headings, body copy, labels, and supporting details. Material UI provides the \`Typography\` component and theme typography tokens so that the same roles can be reused throughout an application.

Use typography variants to communicate a text role, not to make every individual line visually unique. For example, use \`h1\` for the page's main heading, \`h2\` for a major section, \`body1\` for primary reading text, and \`caption\` for short supporting information.

## The Typography component

Import the component directly when using it:

```jsx
import Typography from '@mui/material/Typography';
```

The \`variant\` prop selects a named style from the theme:

```jsx
<Typography variant="h1">Account settings</Typography>
<Typography variant="h2">Profile</Typography>
<Typography variant="body1">Manage your personal information.</Typography>
<Typography variant="caption">Last updated today</Typography>
```

Material UI supplies these common variants:

| Variant group | Variants | Typical role |
| --- | --- | --- |
| Headings | \`h1\` through \`h6\` | Page and section titles |
| Subtitles | \`subtitle1\`, \`subtitle2\` | Short supporting headings |
| Body | \`body1\`, \`body2\` | Paragraphs, descriptions, and secondary copy |
| Utility | \`button\`, \`caption\`, \`overline\` | Control labels and compact supporting text |
| Inherited | \`inherit\` | Inherit the surrounding font settings |

The default variant is \`body1\`. Prefer the variants that match the role of the content. A button's visible label should normally come from its component rather than an unrelated typography variant.

## Visual variant and semantic element are separate

A Typography variant controls styling. The rendered HTML element controls document semantics. These are related by default, but they are not the same choice.

For example, a visually large page title may need to render as an \`h1\`:

```jsx
<Typography variant="h1" component="h1">
  Account settings
</Typography>
```

If the page already has an \`h1\`, a visually large subsection title can use a lower semantic heading:

```jsx
<Typography variant="h2" component="h2">
  Personal information
</Typography>
```

The default mapping uses heading elements for \`h1\` through \`h6\`, maps \`subtitle1\` and \`subtitle2\` to \`h6\`, and maps body variants to paragraphs. Use the \`component\` prop when an individual instance needs a different semantic element.

Do not select a heading only because its font size looks right. Preserve a sensible heading order for assistive technology and page navigation. If styling and semantic level need different values, set both \`variant\` and \`component\` explicitly.

An application can customize the default mapping through the theme's \`MuiTypography.defaultProps.variantMapping\`. Apply a global mapping only when the same semantic rule is suitable across the application.

## Customize the theme typography

Typography tokens live in the theme's \`typography\` object. They can set a shared font family, adjust variants, and define a scale used by all components that read the theme.

```jsx
import { createTheme, ThemeProvider } from '@mui/material/styles';
import CssBaseline from '@mui/material/CssBaseline';
import Typography from '@mui/material/Typography';

const theme = createTheme({
  typography: {
    fontFamily: [
      'Inter',
      'Arial',
      'sans-serif',
    ].join(','),
    h1: {
      fontSize: '2.5rem',
      fontWeight: 700,
      lineHeight: 1.15,
      letterSpacing: '-0.02em',
    },
    h2: {
      fontSize: '2rem',
      fontWeight: 650,
      lineHeight: 1.2,
    },
    body1: {
      fontSize: '1rem',
      lineHeight: 1.6,
    },
    body2: {
      fontSize: '0.875rem',
      lineHeight: 1.5,
    },
  },
});

function SettingsPage() {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      <main>
        <Typography variant="h1" component="h1">
          Account settings
        </Typography>
        <Typography variant="body1">
          Review your profile and notification preferences.
        </Typography>
      </main>
    </ThemeProvider>
  );
}
```

The chosen font must actually be available to the browser. Listing a font family in the theme does not download or bundle font files. If the design uses a web font, load it using the application's chosen asset strategy and include reliable fallback fonts.

Variant values can use CSS units and properties. Relative units such as \`rem\` make the scale respond to the root font size. Avoid shrinking the browser's root font size just to simplify calculations; users may have changed it for readability.

## Responsive typography

A type scale may need to adapt to the available viewport. You can add breakpoint styles directly to a theme variant:

```jsx
import { createTheme } from '@mui/material/styles';

const theme = createTheme({
  typography: {
    h1: {
      fontSize: '2rem',
      lineHeight: 1.2,
      '@media (min-width:900px)': {
        fontSize: '3rem',
      },
    },
  },
});
```

The \`responsiveFontSizes\` helper can adjust theme typography across breakpoints:

```jsx
import {
  createTheme,
  responsiveFontSizes,
} from '@mui/material/styles';

let theme = createTheme({
  typography: {
    h1: {
      fontSize: '2.25rem',
    },
  },
});

theme = responsiveFontSizes(theme);
```

Use one responsive strategy at a time for each variant. Explicit breakpoint rules are useful when the design needs exact sizes. The helper is convenient when a consistent scale adjustment is enough. Review the resulting sizes at narrow and wide widths rather than assuming the type fits.

## Common text layout properties

Typography accepts common text props and the \`sx\` styling system:

- \`align\` sets text alignment, for example \`left\`, \`center\`, \`right\`, or \`justify\`.
- \`gutterBottom\` adds a bottom margin for common heading spacing.
- \`noWrap\` prevents wrapping and truncates overflow with an ellipsis when the element has a constrained width.
- \`paragraph\` is a legacy convenience for paragraph spacing. Prefer a semantic element and explicit spacing in new code.
- \`sx\` applies a local style using theme-aware values.

```jsx
<Typography
  variant="body2"
  color="text.secondary"
  sx={{ maxWidth: 60, overflowWrap: 'anywhere' }}
>
  A long identifier can wrap without widening the layout.
</Typography>
```

Use \`noWrap\` only when truncation is an acceptable way to present the content. For content that must remain fully available, allow wrapping or provide another accessible way to read it. Ellipsis does not make hidden text available to assistive technology in every context.

## Custom variants

A project can add a custom typography variant when it represents a repeated design role that does not fit the built-in scale. For example, a poster style may be useful for a branded landing page:

```jsx
const theme = createTheme({
  typography: {
    poster: {
      fontSize: '3.5rem',
      lineHeight: 1.05,
      fontWeight: 800,
      letterSpacing: '-0.03em',
    },
  },
});
```

A custom variant is used by Typography when the theme provides its style. Decide separately what semantic HTML element it should render as. In TypeScript projects, augment the relevant Material UI types when adding a custom variant so that the new name is accepted by the compiler. Type augmentation is covered in the TypeScript chapter.

Avoid adding a custom variant for a one-off spacing or color adjustment. Use the existing variant with \`sx\` for isolated cases; reserve theme variants for roles that recur in the design system.

## Readability and accessibility

Check the following when defining a typography system:

1. Use a logical heading hierarchy and do not choose heading tags only for their appearance.
2. Maintain sufficient contrast between text and its background.
3. Keep body text large enough to read comfortably and allow browser zoom and user font preferences.
4. Use line heights that make paragraphs easy to follow.
5. Do not rely on color alone to distinguish important or error text.
6. Ensure that text remains visible at narrow widths and with increased text size.
7. Use labels and descriptions that explain controls; small text style does not replace accessible labeling.

Typography cannot correct unclear content. Keep headings descriptive, paragraphs concise, and instructions close to the control or task they explain.

## Common mistakes

- Using \`h1\` for every large-looking title, creating a misleading document outline.
- Assuming \`variant="h2"\` always renders an \`h2\` element without checking the mapping or \`component\` prop.
- Choosing a tiny \`caption\` variant for information users must read to complete a task.
- Setting \`noWrap\` on important content without a way to inspect the full text.
- Listing a font in \`fontFamily\` without ensuring it is loaded or available.
- Adding many custom variants that duplicate existing roles and make the system hard to maintain.
- Hardcoding a breakpoint font size without checking the design at intermediate widths.

## Recap

Use \`Typography\` variants to keep visual text roles consistent. Choose the HTML element deliberately because a variant and its semantic element are separate. Set shared font and variant rules in the theme, use responsive sizing where the design requires it, and check the rendered page for hierarchy, contrast, wrapping, and readability.

## Practice task

Create a page heading, two section headings, a paragraph, and supporting metadata using the theme typography scale. Inspect the rendered HTML elements, adjust the heading semantics if needed, then resize the viewport and verify that long text wraps and remains readable.

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
      <td><a href="010-color-schemes-dark-mode-and-css-variables.md">010 - Color schemes, dark mode, and CSS variables</a></td>
      <td><a href="../qnas/011-typography.md">Q&amp;A</a></td>
      <td><a href="../examples/011-typography.md">Example</a></td>
      <td><a href="012-box-container-and-stack-layout.md">012 - Box, Container, and Stack layout</a></td>
    </tr>
  </tbody>
</table>
