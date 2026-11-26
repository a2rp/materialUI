# Responsive Design and Breakpoints

## Learning goals

After this chapter, you should be able to:

- Build layouts that adapt from narrow to wide viewports.
- Use Material UI's mobile-first breakpoint names in sx values.
- Create reusable media queries from the theme.
- Decide when CSS should adapt appearance and when JavaScript should change rendered behavior.
- Test layouts across widths instead of targeting one device.

## Responsive design is content-driven

Responsive design adapts content and controls to the available space. It is not a list of fixed layouts for specific phone models. Start with a narrow viewport, identify the content's minimum readable and usable size, then add layout changes when the design needs more room.

Material UI uses a mobile-first approach. A rule at a breakpoint applies at that minimum width and above until a later rule overrides it. The default breakpoints are:

| Name | Default minimum width |
| --- | ---: |
| xs | 0 px |
| sm | 600 px |
| md | 900 px |
| lg | 1200 px |
| xl | 1536 px |

These values can be customized in the theme. Treat names as design-system thresholds, not device categories.

## Responsive sx objects

Use a breakpoint-keyed object for a style that changes by viewport width:

```jsx
<Box
  sx={{
    px: { xs: 2, md: 4 },
    display: { xs: 'block', md: 'flex' },
    gap: { xs: 1, md: 3 },
  }}
>
  Responsive content
</Box>
```

Each value applies at that breakpoint and larger widths. A later breakpoint value replaces the earlier value for that property. This keeps visual changes in CSS rather than branching the React tree.

Responsive values work with supported sx properties and can be combined with theme palette and spacing values. Arrays are also available for responsive values, but object syntax is easier to read and maintain when more than a few breakpoints are used.

## Theme breakpoint helpers

The theme provides helpers for building media queries:

- `theme.breakpoints.up(key)` targets the named breakpoint and above.
- `theme.breakpoints.down(key)` targets below the named breakpoint.
- `theme.breakpoints.only(key)` targets one named range.
- `theme.breakpoints.not(key)` excludes one named range.
- `theme.breakpoints.between(start, end)` targets a range between two breakpoints.

These helpers can be used in styled callbacks and other style APIs:

```jsx
<Box
  sx={(theme) => ({
    [theme.breakpoints.up('md')]: {
      display: 'grid',
      gridTemplateColumns: '1fr 1fr',
    },
  })}
/>
```

Use the sx responsive object for simple property values. Use breakpoint helpers when several styles need to change together or when the style is written outside sx.

## Container, Stack, and Grid

Responsive behavior can come from the layout component as well as from sx:

- Container provides a centered content width with configurable maximum widths.
- Stack arranges items in one direction and can change direction at a breakpoint.
- Grid supports responsive column sizes.
- Box and CSS flex/grid can express custom layouts.

A layout component does not remove the need to check content overflow, long labels, form controls, and touch target spacing at narrow widths.

## Custom breakpoints

An application can replace the default breakpoint values and names:

```jsx
const theme = createTheme({
  breakpoints: {
    values: {
      mobile: 0,
      tablet: 640,
      laptop: 1024,
      desktop: 1280,
    },
  },
});
```

When replacing the breakpoint map, provide all values the theme should support. TypeScript projects must also augment the breakpoint types so the compiler recognizes custom names. Changing names or thresholds can affect every responsive rule in the application, so audit existing sx objects and component APIs.

## useMediaQuery for behavior changes

Use `useMediaQuery` when the React rendering or behavior must change based on a media query:

```jsx
import useMediaQuery from '@mui/material/useMediaQuery';
import { useTheme } from '@mui/material/styles';

function Navigation() {
  const theme = useTheme();
  const compact = useMediaQuery(theme.breakpoints.down('sm'));

  return compact ? <CompactNavigation /> : <WideNavigation />;
}
```

Prefer CSS responsive styles when only appearance changes. JavaScript media queries can introduce server-rendering concerns because the server does not know the browser viewport. Material UI supports SSR configuration for the hook, but matching a media query on the server and client has limitations. Try CSS first; use the hook when changing the React tree is necessary.

## Viewport metadata

A responsive page needs an appropriate viewport declaration in its document head so mobile browsers lay out the page at the device width and preserve user zoom. Many React starters include this already. Confirm it exists in the HTML document rather than adding duplicate viewport tags.

## Container queries

A component sometimes needs to adapt to its own available width rather than the viewport. CSS container queries can address this. MUI System supports container query shorthand in responsive sx values in current versions. Verify browser support and the syntax for the installed version before using it.

## Testing responsive layouts

Test at representative widths around each breakpoint and at widths between them. Check:

- No unexpected horizontal scrolling.
- Text remains readable without clipping.
- Navigation and actions remain reachable.
- Form fields fit and labels remain associated.
- Content order and keyboard flow remain logical.
- Zoom and larger text do not break the layout.

Use browser resizing and real devices when appropriate. A breakpoint preview alone does not reveal every interaction or touch issue.

## Common mistakes

- **Designing only for named devices:** content can appear at any intermediate width.
- **Assuming md means tablet:** breakpoint names are configurable thresholds.
- **Using JavaScript for a color or spacing change:** use responsive CSS values.
- **Using an array with many anonymous breakpoint values:** prefer named object keys.
- **Changing only one breakpoint in a custom map:** provide the complete intended map.
- **Changing the React tree with useMediaQuery during SSR without planning:** CSS media queries are more reliable for visual changes.
- **Forgetting viewport metadata:** check the document head for the responsive viewport setting.

## Recap

Material UI's responsive tools use mobile-first breakpoint thresholds. Use object values in sx for visual changes, theme breakpoint helpers for media queries, and useMediaQuery only when application behavior must change. Customize breakpoints centrally, test widths around every threshold, and keep layouts usable between them.

## Practice task

Build a page whose content is one column on narrow screens and two columns at md and above. Add responsive padding and verify the layout at a width just below md, exactly at md, and just above md.

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
      <td><a href="008-css-overrides-and-global-styles.md">008 - CSS overrides and global styles</a></td>
      <td><a href="../qnas/009-responsive-design-and-breakpoints.md">Q&amp;A</a></td>
      <td><a href="../examples/009-responsive-design-and-breakpoints.md">Example</a></td>
      <td><a href="010-color-schemes-dark-mode-and-css-variables.md">010 - Color schemes, dark mode, and CSS variables</a></td>
    </tr>
  </tbody>
</table>
