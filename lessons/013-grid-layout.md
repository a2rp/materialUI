# Grid Layout

## Learning goals

After this chapter, you should be able to:

- Build a responsive column layout with Material UI Grid.
- Set item widths and column counts at breakpoints.
- Control horizontal and vertical spacing.
- Use offsets for intentional alignment.
- Recognize when CSS Grid or Stack is a better fit.
- Avoid mixing current Grid props with the older Grid API.

## What Material UI Grid is for

Material UI's Grid component builds responsive layouts from columns. It uses CSS Flexbox to arrange items in rows and lets each child occupy a chosen number of columns. This works well when the number of columns is known and the layout changes at predictable breakpoints.

Grid is not the same as CSS Grid. It does not provide row spanning or automatic placement. If the design depends on those features, use CSS Grid through \`sx\` or a stylesheet.

Grid is also different from MUI X Data Grid. The former arranges page content; the latter displays and manages tabular data.

## Use the current Grid API

For current Material UI, import Grid directly:

```jsx
import Grid from '@mui/material/Grid';
```

A Grid container wraps Grid items. Every Grid is an item, so do not add an \`item\` prop. Set the item's width with \`size\`:

```jsx
<Grid container spacing={2}>
  <Grid size={8}>
    Main content
  </Grid>
  <Grid size={4}>
    Sidebar
  </Grid>
</Grid>
```

With the default 12-column layout, \`size={8}\` occupies eight columns and \`size={4}\` occupies four. The component calculates a fluid width relative to its grid container.

Older code may use \`GridLegacy\` with \`item\`, \`xs\`, or \`sm\` props. Those are not the current Grid API. For a v9 course, use \`Grid\` with \`size\` and omit \`item\`.

## Responsive item sizes

Provide breakpoint values to let the layout adapt:

```jsx
<Grid container spacing={2}>
  <Grid size={{ xs: 12, md: 8 }}>
    Main content
  </Grid>
  <Grid size={{ xs: 12, md: 4 }}>
    Sidebar
  </Grid>
</Grid>
```

Each item uses all columns at the extra-small breakpoint, then uses an 8/4 split from the medium breakpoint. A value set at a smaller breakpoint continues to apply at wider breakpoints until another value overrides it.

For a repeating card layout, each item can use responsive widths:

```jsx
<Grid container spacing={{ xs: 1, sm: 2 }}>
  <Grid size={{ xs: 12, sm: 6, lg: 4 }}>First card</Grid>
  <Grid size={{ xs: 12, sm: 6, lg: 4 }}>Second card</Grid>
  <Grid size={{ xs: 12, sm: 6, lg: 4 }}>Third card</Grid>
</Grid>
```

At xs each card uses a full row. At sm, two cards fit on a row. At lg, each card occupies four of twelve columns.

## Customize the number of columns

The default column count is 12. You can change it on the container and set item sizes relative to that count:

```jsx
<Grid container columns={{ xs: 4, sm: 8, md: 12 }} spacing={2}>
  <Grid size={{ xs: 2, sm: 4, md: 4 }}>
    Responsive card
  </Grid>
</Grid>
```

At xs the item uses two of four columns, at sm it uses four of eight, and at md it uses four of twelve. The \`size\` and \`columns\` values should be planned together so that widths add up as intended.

## Spacing between items

Use \`spacing\` to set the gap between items. Numeric values use the theme spacing scale:

```jsx
<Grid container spacing={2}>
  <Grid size={6}>First half</Grid>
  <Grid size={6}>Second half</Grid>
</Grid>
```

You can set row and column gaps separately:

```jsx
<Grid
  container
  rowSpacing={3}
  columnSpacing={{ xs: 1, sm: 2, md: 3 }}
>
  <Grid size={6}>First item</Grid>
  <Grid size={6}>Second item</Grid>
  <Grid size={6}>Third item</Grid>
  <Grid size={6}>Fourth item</Grid>
</Grid>
```

Spacing can be responsive. Its actual CSS value depends on the theme spacing configuration.

## Offsets

The \`offset\` prop moves an item to the right by a number of columns, or uses \`auto\` to push it toward the far edge of the row:

```jsx
<Grid container spacing={2}>
  <Grid size={{ xs: 12, md: 4 }} offset={{ md: 2 }}>
    Centered at medium widths
  </Grid>
  <Grid size="auto" offset="auto">
    Row action
  </Grid>
</Grid>
```

Offsets are useful for intentional whitespace. Check that the item and offset fit within the available columns at each breakpoint. Avoid offsets as a substitute for a clear alignment structure.

## Flexible and content-sized items

The \`size\` prop can also use \`auto\` for an item sized to its content, or \`grow\` for an item that grows into available space:

```jsx
<Grid container spacing={2}>
  <Grid size="auto">Status</Grid>
  <Grid size="grow">A flexible description that uses the remaining row space</Grid>
  <Grid size="auto">Action</Grid>
</Grid>
```

Use these forms when the row has content-sized controls and a flexible region. Check the result with long text and narrow widths.

## Aligning Grid contents

Grid uses Flexbox. Use the container's \`sx\` styles for alignment:

```jsx
<Grid
  container
  spacing={2}
  sx={{
    alignItems: 'center',
    justifyContent: 'space-between',
  }}
>
  <Grid size={{ xs: 12, sm: 6 }}>Description</Grid>
  <Grid size={{ xs: 12, sm: 4 }}>Action area</Grid>
</Grid>
```

Grid supports row and row-reverse directions. It is designed to divide horizontal rows into columns. For vertical stacking, use Stack, including inside a Grid item if needed.

## Nested content

A Grid item can contain any suitable React content, including another Grid container. Use nested containers when a subsection needs its own columns. Keep the hierarchy understandable and ensure the inner layout has the intended spacing and widths.

```jsx
<Grid container spacing={2}>
  <Grid size={{ xs: 12, md: 8 }}>
    <Grid container spacing={2}>
      <Grid size={{ xs: 12, sm: 6 }}>First detail</Grid>
      <Grid size={{ xs: 12, sm: 6 }}>Second detail</Grid>
    </Grid>
  </Grid>
  <Grid size={{ xs: 12, md: 4 }}>Related information</Grid>
</Grid>
```

Do not use a Grid container for every wrapper. Use Box or Stack when there is no column layout to express.

## Common mistakes

- Using the legacy \`item\` prop or breakpoint props like \`xs={6}\` with the current Grid.
- Expecting Grid to span rows or automatically place items like CSS Grid.
- Using Grid for a simple vertical list that Stack would express more clearly.
- Forgetting that \`size\` is relative to the container's \`columns\` value.
- Combining item sizes and offsets that exceed the row's available columns.
- Assuming the default spacing value is a fixed pixel amount regardless of the theme.
- Setting a row direction to column instead of using Stack for vertical flow.
- Omitting a narrow breakpoint layout and allowing content to overflow.

## Recap

Grid is a responsive flexbox-based column layout for known column structures. Use \`container\`, responsive \`size\`, and theme-based spacing. Use \`offset\` for deliberate alignment. Choose Stack for one-dimensional vertical flow and CSS Grid when row spanning or auto-placement is required.

## Practice task

Create a responsive product page with a main details area and a related-items sidebar. Use a 12-column layout on wider screens, stack both areas on narrow screens, then add a three-card related-items row with independent row and column spacing. Test long content and verify that sizes and offsets never exceed the configured columns.

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
      <td><a href="012-box-container-and-stack-layout.md">012 - Box, Container, and Stack layout</a></td>
      <td><a href="../qnas/013-grid-layout.md">Q&amp;A</a></td>
      <td><a href="../examples/013-grid-layout.md">Example</a></td>
      <td><a href="014-buttons-icon-buttons-and-button-groups.md">014 - Buttons, icon buttons, and button groups</a></td>
    </tr>
  </tbody>
</table>
