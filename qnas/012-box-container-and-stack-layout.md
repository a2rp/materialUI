# Box, Container, and Stack Layout - Q&A

## 1. What is the difference between Box, Container, and Stack?

- `Box` is a generic theme-aware wrapper with access to `sx`.
- `Container` centers content and limits its maximum width.
- `Stack` arranges its immediate children in one direction with consistent spacing.

Choose based on the layout responsibility instead of using one component for every wrapper.

## 2. What element does Box render?

Box renders a `div` by default. Set `component` when the content needs another element:

```jsx
<Box component="section" aria-labelledby="details-heading">
  ...
</Box>
```

## 3. Does Box add spacing between its children?

No. Box is a general wrapper. Use Stack for a one-dimensional group that needs shared spacing, or define a suitable layout rule in `sx`.

## 4. What is Container for?

Container creates a centered content area with a maximum width and horizontal gutters. It is commonly used around page content to keep lines from becoming too wide on large screens.

## 5. What does the Container `maxWidth` prop do?

It sets the maximum width using a breakpoint value such as `sm`, `md`, or `lg`. The default is `lg`. Set it to `false` to disable the built-in maximum width.

## 6. What do Container gutters do?

Gutters provide horizontal padding at the sides of the Container. Use `disableGutters` only when another layout layer supplies the needed spacing. Nested Containers can unintentionally compound the padding.

## 7. What is the difference between fluid and fixed Container behavior?

By default, Container grows with the viewport until it reaches its selected maximum. With `fixed`, its maximum width matches the minimum width of the active breakpoint. Use fixed widths only when that behavior fits the design.

## 8. What direction does Stack use by default?

Stack uses a vertical column. Set `direction="row"` for a horizontal arrangement:

```jsx
<Stack direction="row" spacing={2}>
  <Button>Cancel</Button>
  <Button>Save</Button>
</Stack>
```

## 9. How is Stack spacing calculated?

A numeric `spacing` value is passed through the theme spacing helper. This keeps gaps consistent with the design system:

```jsx
<Stack spacing={2}>
  <Typography>First item</Typography>
  <Typography>Second item</Typography>
</Stack>
```

The actual CSS distance depends on the theme spacing configuration.

## 10. Can Stack direction and spacing change at breakpoints?

Yes. Responsive object values can change both:

```jsx
<Stack
  direction={{ xs: 'column', sm: 'row' }}
  spacing={{ xs: 1, sm: 2 }}
>
  ...
</Stack>
```

Inspect the layout at intermediate sizes, especially if the row includes long text or controls.

## 11. When should I choose Grid instead of Stack?

Use Stack for a one-dimensional row or column. Use Grid when items need a two-dimensional arrangement, such as aligned columns across multiple rows.

## 12. What does Stack's `divider` prop do?

It places a divider element between immediate children. For a horizontal Stack, a vertical Divider is commonly used with `flexItem`. Match the divider orientation to the direction of the layout.

## 13. Why can a child's margin conflict with Stack spacing?

Stack's default spacing implementation applies rules between children. A child's own margins can produce unexpected gaps. Use `useFlexGap` to use CSS flexbox `gap` when appropriate, and check browser support for the project's supported browsers.

## 14. Does a horizontal Stack automatically wrap?

No. Add `flexWrap: 'wrap'` through `sx` if the items should wrap, and consider `useFlexGap` for consistent row and column gaps.

## 15. What does `minWidth: 0` solve in a flex row?

Flex items can retain a minimum size based on their content. If a long string or label pushes other content out of the row, setting `minWidth: 0` on the flexible child allows it to shrink and wrap.

## 16. How should I combine the three components?

Give each a distinct job. For example, use Container to establish page width, Stack for vertical spacing, and Box for semantic sections or local layout. Avoid extra wrappers that do not contribute to structure or behavior.

## 17. How can I make a page wrapper semantic?

Use the `component` prop on Container or Box:

```jsx
<Container component="main" maxWidth="md">
  <Box component="section" aria-labelledby="overview-heading">
    ...
  </Box>
</Container>
```

Use landmarks and section labels that reflect the actual content structure.

## 18. What should I check when a layout breaks at a narrow width?

Check container gutters, Stack direction, flex child sizing, long text wrapping, and whether controls can move to a column. Avoid hiding essential content just to make the row fit.

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
      <td><a href="011-typography.md">011 - Typography</a></td>
      <td><a href="../lessons/012-box-container-and-stack-layout.md">Lesson</a></td>
      <td><a href="../examples/012-box-container-and-stack-layout.md">Example</a></td>
      <td><a href="013-grid-layout.md">013 - Grid layout</a></td>
    </tr>
  </tbody>
</table>
