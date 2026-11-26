# Theme Tokens and the Design System

## Learning goals

After this chapter, you should be able to:

- Identify the main design tokens in a Material UI theme.
- Configure palette, typography, spacing, breakpoints, and shape.
- Choose which values belong in a shared theme and which belong in local styles.
- Configure component defaults and variants at the theme level.
- Avoid treating every numeric style value as a spacing unit.

## What a design token is

A design token is a named value used to express a shared design decision. A palette role such as primary, a typography variant such as h6, or a spacing scale is easier to reuse than unrelated hard-coded values across many files.

Material UI's theme provides these named values to components and styling APIs. Components use the active theme for default colors, typography, and other visual behavior. Application styles can refer to the same values, so shared decisions stay consistent.

## Palette

The palette contains semantic color roles. Common roles include primary, secondary, error, warning, info, success, text, background, and divider. A component can use supported color props such as primary or error to select one of these roles.

A basic palette might define:

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
    success: {
      main: '#2e7d32',
    },
  },
});
```

When a custom palette role provides a main color, Material UI can derive related light, dark, and contrast values using its palette utilities. If the brand requires precise values, specify and verify the complete set rather than relying on an assumed shade. Custom colors also need contrast review for the actual foreground and background combinations.

Prefer semantic roles such as primary and error to repeating literal hex values in components. The role describes why a color is used, while the literal value is configured in one place.

## Typography

The typography section defines the font family and text variants such as h1 through h6, subtitle1, body1, body2, button, caption, and overline. Components such as Typography can select a variant.

```jsx
const theme = createTheme({
  typography: {
    fontFamily: 'Arial, sans-serif',
    h1: {
      fontSize: '2.25rem',
      fontWeight: 700,
      lineHeight: 1.15,
    },
    body1: {
      fontSize: '1rem',
      lineHeight: 1.6,
    },
  },
});
```

Typography variants establish a consistent type scale. A visual variant and the rendered HTML heading level are separate decisions. When a heading must be an h1, explicitly use the semantic component appropriate to the page even if its visual variant is different.

## Spacing

The theme spacing scale gives layouts a consistent rhythm. The default scale uses an 8-pixel base unit. A spacing helper can calculate a CSS value:

```jsx
const theme = createTheme();

theme.spacing(1); // "8px" with the default spacing scale
theme.spacing(2); // "16px" with the default spacing scale
```

System spacing props such as padding and margin can interpret numeric values through the theme spacing scale:

```jsx
<Box sx={{ p: 2, mt: 3 }}>
  Theme-scaled padding and top margin
</Box>
```

Do not assume every numeric sx value means spacing. For example, width, opacity, and z-index have their own property-specific behavior. Check the style API for a particular property.

## Breakpoints

Breakpoints define named width ranges for responsive styles. Material UI provides common breakpoint names such as xs, sm, md, lg, and xl. Their values can be customized in the theme. The exact viewport values are configurable, so styles should depend on breakpoint names rather than copying another application's pixel thresholds.

Responsive style values can be supplied as objects:

```jsx
<Box
  sx={{
    p: { xs: 2, md: 4 },
    display: { xs: 'block', md: 'flex' },
  }}
>
  Responsive content
</Box>
```

This keeps visual adaptation in CSS rather than rendering different React trees solely for appearance.

## Shape, elevation, z-index, and transitions

Other theme sections centralize visual behavior:

- Shape settings define shared geometry such as the default border radius.
- Shadows represent elevation levels used by surfaces.
- z-index values coordinate stacking order for menus, drawers, dialogs, and tooltips.
- Transitions define shared timing and easing for animated changes.

Use these shared values where they express a consistent system. Do not assume a theme token determines every component's final layout or stacking in every application.

## Component defaults, variants, and overrides

The theme's components section can configure several kinds of shared behavior:

```jsx
const theme = createTheme({
  components: {
    MuiButton: {
      defaultProps: {
        disableElevation: true,
      },
      styleOverrides: {
        root: {
          borderRadius: 8,
          textTransform: 'none',
        },
      },
    },
  },
});
```

- defaultProps supplies a default prop value when a component instance does not provide one.
- styleOverrides changes styles for component slots.
- variants describes styles that apply when selected props match.

Use theme defaults when they should apply consistently. Use a local sx style when the decision belongs to one instance. Avoid using the theme as a dumping ground for unrelated page-specific layout rules.

## Custom theme values

An application can add custom theme fields when its design system needs additional shared tokens. TypeScript projects also need module augmentation so the compiler understands those fields. Keep runtime theme configuration and type declarations aligned. This course covers the TypeScript aspect later.

## A practical token strategy

1. Identify repeated values in the design.
2. Give values semantic names based on their role.
3. Configure shared palette, typography, spacing, and shape in the theme.
4. Use component defaults only for consistent product-wide behavior.
5. Keep one-off layout choices close to the component or page.
6. Check contrast and responsive behavior in the rendered application.

## Common mistakes

- **Hard-coding the same brand color everywhere:** define a semantic palette role.
- **Using color names for unrelated meanings:** choose clear semantic tokens.
- **Assuming all numeric sx props use spacing:** behavior depends on the CSS property.
- **Treating a visual typography variant as semantic HTML:** choose the correct heading or text element.
- **Setting global overrides for one page:** keep page-specific choices local.
- **Changing breakpoint values without checking all responsive styles:** test the full design after changing the scale.

## Recap

A theme is the shared design vocabulary of an application. Palette roles, typography, spacing, breakpoints, shape, elevation, transitions, and component configuration make repeated choices consistent. Use semantic tokens for shared decisions and keep local layout details near their components.

## Practice task

Create a small theme with primary and success colors, a font family, and a custom button default. Build two screens that use the same Button and Typography variants. Identify which visual decisions are shared and which should remain local.

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
      <td><a href="004-themes-themeprovider-and-cssbaseline.md">004 - Themes, ThemeProvider, and CssBaseline</a></td>
      <td><a href="../qnas/005-theme-tokens-and-design-system.md">Q&amp;A</a></td>
      <td><a href="../examples/005-theme-tokens-and-design-system.md">Example</a></td>
      <td><a href="006-sx-prop-and-theme-aware-styles.md">006 - The sx prop and theme-aware styles</a></td>
    </tr>
  </tbody>
</table>
