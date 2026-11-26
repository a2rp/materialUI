# Grid Layout - Q&A

## 1. What is Material UI Grid designed to do?

It creates responsive layouts using columns and flexbox. It is useful when the layout has a known column structure that changes at chosen breakpoints.

## 2. How do I import the current Grid component?

Use the direct component import:

```jsx
import Grid from '@mui/material/Grid';
```

Current Grid uses the `size` prop for item width. Older examples may show different imports and props, so check that the code matches the Material UI version in the project.

## 3. How do I create a Grid container?

Add the `container` prop to a Grid:

```jsx
<Grid container spacing={2}>
  <Grid size={6}>First half</Grid>
  <Grid size={6}>Second half</Grid>
</Grid>
```

The nested Grid components are the children in the row layout.

## 4. Should I add `item` to each Grid child?

No. In the current Grid API, every Grid is an item. The old `item` prop is not part of the current API.

## 5. What does `size={6}` mean?

With the default 12 columns, it means that the item occupies six columns, or half of the row. Its rendered width is fluid relative to its container.

## 6. How do I set different widths at breakpoints?

Pass a responsive object to `size`:

```jsx
<Grid size={{ xs: 12, md: 8 }}>Main content</Grid>
<Grid size={{ xs: 12, md: 4 }}>Sidebar</Grid>
```

Each width continues at wider breakpoints until a larger breakpoint sets another value.

## 7. What happens if item sizes add up to more than the column count?

The items may wrap to another row. Check each breakpoint and include offsets when calculating available columns. Do not assume an arrangement will stay in one row if its sizes exceed the configured columns.

## 8. Can I change the total number of columns?

Yes. Set `columns` on the container and set item sizes relative to that number:

```jsx
<Grid container columns={{ xs: 4, md: 12 }}>
  <Grid size={{ xs: 2, md: 4 }}>Responsive width</Grid>
</Grid>
```

Make sure both values are coordinated at every breakpoint.

## 9. What does the `spacing` prop control?

It controls the gap between Grid children. Numeric values use the theme spacing helper, so the computed distance depends on the theme.

## 10. How are `rowSpacing` and `columnSpacing` different?

They set the vertical and horizontal gaps separately. Use them when rows and columns need different spacing. Both can be responsive.

## 11. What does `offset` do?

It moves an item to the right by a number of columns, or uses `auto` to push it toward the far edge. Ensure the item size and offset fit the row at each breakpoint.

## 12. What do `size="auto"` and `size="grow"` mean?

`auto` sizes the item to its content. `grow` lets the item use remaining space. These are useful in mixed rows with fixed-content actions and a flexible text region.

## 13. Is Material UI Grid implemented with CSS Grid?

No. Material UI Grid uses CSS Flexbox. For row spanning or CSS Grid's automatic placement behavior, use the CSS Grid layout system through `sx` or a stylesheet.

## 14. Can Grid items span multiple rows?

No. Grid is designed around column sizing and does not support row spanning. Use CSS Grid when an item must occupy multiple rows.

## 15. Does Grid support vertical column direction?

No. Grid supports row and row-reverse directions. Use Stack for vertical flow, or place a Stack inside a Grid item.

## 16. How is Grid different from MUI X Data Grid?

Material UI Grid arranges page content. MUI X Data Grid displays tabular datasets with table-oriented features. They solve different layout problems and come from different packages.

## 17. When should I choose Stack over Grid?

Choose Stack when the items form one row or one column with consistent spacing. Choose Grid when you need column widths that relate to a configurable column count.

## 18. What should I use when upgrading code with `xs` or `item` props?

Migrate to the current Grid API: import `Grid`, remove `item`, and express breakpoint widths with `size`. Check the installed Material UI version and its migration guidance before changing an existing application.

## 19. How can I align content vertically within a row?

Use flex alignment on the Grid container:

```jsx
<Grid container spacing={2} sx={{ alignItems: 'center' }}>
  ...
</Grid>
```

Use `justifyContent` for main-axis placement when the layout needs it.

## 20. What should I check if a Grid layout overflows?

Check responsive item sizes, total columns, offsets, long unbreakable content, nested container widths, and the selected breakpoint behavior. Also inspect whether CSS Grid would better match the intended layout.

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
      <td><a href="012-box-container-and-stack-layout.md">012 - Box, Container, and Stack layout</a></td>
      <td><a href="../lessons/013-grid-layout.md">Lesson</a></td>
      <td><a href="../examples/013-grid-layout.md">Example</a></td>
      <td><a href="014-buttons-icon-buttons-and-button-groups.md">014 - Buttons, icon buttons, and button groups</a></td>
    </tr>
  </tbody>
</table>
