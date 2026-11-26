# Box, Container, and Stack Layout

## Learning goals

After this chapter, you should be able to:

- Choose between Box, Container, and Stack based on the layout job.
- Use Box as a theme-aware semantic wrapper.
- Set a readable content width with Container.
- Arrange one-dimensional content with Stack.
- Apply consistent and responsive spacing through the theme.
- Avoid common nested spacing and flex sizing problems.

## Choosing the right primitive

Material UI provides several layout components with different responsibilities:

| Component | Main role | Use it when |
| --- | --- | --- |
| `Box` | Generic theme-aware wrapper | You need a semantic wrapper or local layout styling through `sx` |
| `Container` | Centered page-width boundary | Content needs a readable maximum width and horizontal gutters |
| `Stack` | One-dimensional arrangement | Immediate children should flow in a row or column with shared spacing |

Use the simplest component that communicates the layout intent. A two-dimensional layout with aligned rows and columns belongs in Grid, covered in the next chapter. A surface with elevation or an outlined panel is usually better represented by Paper or a card component than by a layout wrapper alone.

## Box: the general-purpose wrapper

Box renders a `div` by default and gives it access to the theme and the `sx` styling system. Use the `component` prop when a different semantic element fits the content:

```jsx
import Box from '@mui/material/Box';

function ArticleSection() {
  return (
    <Box
      component="section"
      sx={{
        p: 2,
        border: 1,
        borderColor: 'divider',
        borderRadius: 1,
      }}
    >
      A semantic section with theme-aware spacing and colors.
    </Box>
  );
}
```

Box is useful for layout and local styling, but it does not add page width, spacing between children, or an elevated surface by itself. Use Container for the page boundary and Stack for repeated space between items.

The `sx` prop supports theme-aware values. For example, `p: 2` uses the theme spacing scale and `color: 'text.secondary'` reads a theme palette value. The exact CSS value depends on the theme configuration.

## Container: a centered content boundary

Container centers its contents horizontally and limits its maximum width. It is appropriate for page-level content that should not expand indefinitely on wide screens.

```jsx
import Container from '@mui/material/Container';

function HelpPage() {
  return (
    <Container maxWidth="md">
      <h1>Help center</h1>
      <p>Browse answers and guides for your account.</p>
    </Container>
  );
}
```

The `maxWidth` prop accepts breakpoint values such as `xs`, `sm`, `md`, `lg`, and `xl`. It can also be set to `false` to disable the built-in maximum width. The default maximum is `lg`.

Container includes horizontal gutters by default. Use `disableGutters` only when another parent or layout rule already supplies the required horizontal spacing. Avoid nesting multiple Containers unless the page genuinely has separate content boundaries.

The `fixed` prop makes the width follow the minimum width of the active breakpoint. The default behavior is fluid up to the selected maximum. Choose based on how the page should respond as the viewport grows.

The `component` prop can give the root an appropriate semantic role, such as `main`:

```jsx
<Container component="main" maxWidth="lg">
  <h1>Project overview</h1>
</Container>
```

## Stack: one axis with shared spacing

Stack arranges its immediate children in a column by default. Set `direction="row"` to arrange them horizontally:

```jsx
import Stack from '@mui/material/Stack';

<Stack spacing={2}>
  <div>Profile</div>
  <div>Security</div>
  <div>Notifications</div>
</Stack>
```

A numeric `spacing` value is converted with the theme spacing helper. This keeps gaps consistent with the design system:

```jsx
<Stack direction="row" spacing={2}>
  <button type="button">Cancel</button>
  <button type="button">Save</button>
</Stack>
```

Use the native MUI components for production controls. The plain buttons above only keep the layout snippet focused.

You can set flex alignment and justification through `sx`:

```jsx
<Stack
  direction="row"
  spacing={2}
  sx={{
    alignItems: 'center',
    justifyContent: 'space-between',
  }}
>
  <div>Account</div>
  <div>Active</div>
</Stack>
```

Stack primarily solves one-dimensional layout. If items need to line up in both rows and columns, use Grid instead of trying to grow one Stack into a custom grid system.

## Responsive direction and spacing

Responsive values let Stack switch layout at a breakpoint:

```jsx
<Stack
  direction={{ xs: 'column', sm: 'row' }}
  spacing={{ xs: 1, sm: 2 }}
>
  <div>Search settings</div>
  <div>Privacy settings</div>
  <div>Notification settings</div>
</Stack>
```

On extra-small screens, the items are stacked. From the small breakpoint onward, they form a row. The spacing value changes with the direction so the vertical and horizontal gaps suit each layout.

Ensure the row still fits its content. A responsive row with long text may need wrapping, flexible widths, or a different breakpoint.

## Dividers between Stack children

Use the `divider` prop to insert a divider between each immediate child. For a horizontal row, use a vertical divider:

```jsx
import Divider from '@mui/material/Divider';
import Stack from '@mui/material/Stack';

<Stack
  direction="row"
  spacing={2}
  divider={<Divider orientation="vertical" flexItem />}
>
  <div>Overview</div>
  <div>Members</div>
  <div>Settings</div>
</Stack>
```

A divider should help communicate grouping. Do not add one to every Stack automatically, and make sure its orientation matches the layout.

## Flex gap and child margins

By default, Stack spacing is implemented by applying spacing rules between children. Child margins can conflict with those rules. When the layout needs CSS flexbox `gap`, set `useFlexGap`:

```jsx
<Stack
  direction="row"
  spacing={2}
  useFlexGap
  sx={{ flexWrap: 'wrap' }}
>
  <div>Short item</div>
  <div>Another item</div>
  <div>Longer item that can wrap</div>
</Stack>
```

Flexbox gap avoids the nested-child spacing behavior, and works naturally with wrapping. Check browser support for your supported browser range before relying on CSS flexbox gap. If you enable it globally through the theme, verify that the setting is appropriate for every Stack in the application.

## Flexible rows and minimum width

A flex item can keep a minimum width based on its content. This may cause long text to push a neighboring action outside the layout. Set `minWidth: 0` on the relevant flex child when it needs permission to shrink:

```jsx
<Stack direction="row" spacing={2} sx={{ alignItems: 'center' }}>
  <Box sx={{ minWidth: 0, flex: 1 }}>
    A long project description that can wrap within the available row width.
  </Box>
  <div>Action</div>
</Stack>
```

If text is deliberately prevented from wrapping, provide a suitable width and decide how users can access the full value. Do not let overflow obscure adjacent controls.

## A page-level composition

Container, Stack, and Box can be combined when each has a distinct responsibility:

```jsx
import Box from '@mui/material/Box';
import Container from '@mui/material/Container';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

function TeamPage() {
  return (
    <Container component="main" maxWidth="md">
      <Stack spacing={{ xs: 2, sm: 3 }}>
        <Box component="header">
          <Typography variant="h1" component="h1">
            Team
          </Typography>
          <Typography color="text.secondary">
            People with access to this workspace.
          </Typography>
        </Box>

        <Box component="section" aria-labelledby="members-heading">
          <Typography id="members-heading" variant="h2" component="h2">
            Members
          </Typography>
          <Typography>
            Invite a teammate or review their current access.
          </Typography>
        </Box>
      </Stack>
    </Container>
  );
}
```

Container sets the readable page width, Stack defines the vertical rhythm between major parts, and Box supplies semantic grouping where needed.

## Common mistakes

- Using Box when a Container is needed to limit page width.
- Nesting Containers and unintentionally doubling horizontal gutters.
- Using Stack for a layout that must align items along two axes.
- Adding margins to Stack children while also relying on Stack's default spacing rules.
- Assuming Stack spacing automatically makes a horizontal row wrap.
- Forgetting `minWidth: 0` on a flex item that must shrink around long text.
- Adding a divider without matching its orientation to the Stack direction.
- Using arbitrary pixel gaps instead of the theme's spacing scale without a design reason.

## Recap

Box is a flexible theme-aware wrapper. Container establishes a centered content boundary. Stack arranges immediate children along one axis with shared, theme-based spacing. Choose each for its specific purpose, then use responsive values and flex sizing deliberately.

## Practice task

Build a settings page with a centered Container, a semantic main element, a page heading, two vertically spaced sections, and a row of actions. Switch the actions to a column on narrow screens. Check the page with a long label and verify that gutters and gaps remain consistent.

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
      <td><a href="011-typography.md">011 - Typography</a></td>
      <td><a href="../qnas/012-box-container-and-stack-layout.md">Q&amp;A</a></td>
      <td><a href="../examples/012-box-container-and-stack-layout.md">Example</a></td>
      <td><a href="013-grid-layout.md">013 - Grid layout</a></td>
    </tr>
  </tbody>
</table>
