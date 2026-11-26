# Responsive Design and Breakpoints - Example

## Goal

Build a dashboard that uses one column on narrow screens and two columns on wider screens. The same React elements remain in the page; CSS values adapt at the theme breakpoints.

## Prerequisites

- A React application with Material UI installed.
- ThemeProvider configured.
- A responsive viewport declaration in the app's HTML document.

## File structure

Use the existing application entry point:

```text
src/
  App.jsx
```

## Build the layout

Replace `src/App.jsx`:

```jsx
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

const items = [
  {
    title: 'Profile',
    detail: 'Keep your contact information up to date.',
  },
  {
    title: 'Security',
    detail: 'Review sign-in and recovery settings.',
  },
  {
    title: 'Notifications',
    detail: 'Choose which updates you receive.',
  },
  {
    title: 'Billing',
    detail: 'Review your current plan and invoices.',
  },
];

export default function App() {
  return (
    <Box
      component="main"
      sx={{
        minHeight: '100vh',
        bgcolor: 'background.default',
        py: { xs: 2, sm: 4, md: 6 },
      }}
    >
      <Container maxWidth="lg">
        <Stack spacing={3}>
          <Stack
            direction={{ xs: 'column', sm: 'row' }}
            justifyContent="space-between"
            alignItems={{ xs: 'stretch', sm: 'center' }}
            spacing={2}
          >
            <Box>
              <Typography variant="h4" component="h1">
                Account settings
              </Typography>
              <Typography color="text.secondary">
                Choose a section to review.
              </Typography>
            </Box>

            <Button variant="contained">Save changes</Button>
          </Stack>

          <Box
            sx={{
              display: 'grid',
              gridTemplateColumns: {
                xs: 'minmax(0, 1fr)',
                md: 'repeat(2, minmax(0, 1fr))',
              },
              gap: { xs: 1.5, md: 2.5 },
            }}
          >
            {items.map((item) => (
              <Paper key={item.title} sx={{ p: { xs: 2, md: 3 } }}>
                <Stack spacing={1}>
                  <Typography variant="h6">{item.title}</Typography>
                  <Typography color="text.secondary">
                    {item.detail}
                  </Typography>
                  <Button sx={{ alignSelf: 'flex-start' }}>
                    Open {item.title}
                  </Button>
                </Stack>
              </Paper>
            ))}
          </Box>
        </Stack>
      </Container>
    </Box>
  );
}
```

## Run and inspect

Start the application with its normal development command. Resize the browser to widths below and above the md breakpoint. Also check an intermediate width where the page is wider than a phone but narrower than the two-column threshold.

## Expected result

On a narrow viewport, the header stacks and the settings cards appear in one column. On wider viewports, the header aligns in a row and the cards use two columns. The page keeps the same content and element order at every width.

## What to notice

- Responsive object values refer to theme breakpoints.
- A breakpoint value applies at that threshold and above.
- Container limits the page width while responsive padding adapts to the viewport.
- CSS Grid handles the card columns without a JavaScript viewport check.
- The action button moves with CSS layout; the React tree does not change.
- Long card content should still be checked for wrapping and overflow.

## Variations

1. Add a third grid column at lg and test the minimum card width.
2. Change the header to keep a row layout at sm and adjust the button size for narrow widths.
3. Add a long translated title and verify it wraps without overlapping the action.
4. Change the theme's breakpoints and see which responsive values use the new thresholds.
5. Add an interaction that truly needs different React behavior and decide whether useMediaQuery is justified.

## Common mistakes

- **Testing only at one phone and one desktop width:** intermediate sizes often reveal layout problems.
- **Assuming breakpoint labels match specific devices:** they are configurable width thresholds.
- **Using useMediaQuery only to change CSS:** use responsive styles for visual layout.
- **Forgetting viewport metadata:** mobile browsers need the correct viewport declaration.
- **Using a fixed card width that overflows:** check narrow screens and long content.
- **Testing only exactly at a breakpoint:** check just below and above it too.

## Practice check

Record what changes at each breakpoint and which values remain constant. Confirm that the same content remains available and in the same logical order when the layout changes.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous example</th>
      <th>Chapter lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Next example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="008-css-overrides-and-global-styles.md">008 - CSS overrides and global styles</a></td>
      <td><a href="../lessons/009-responsive-design-and-breakpoints.md">Lesson</a></td>
      <td><a href="../qnas/009-responsive-design-and-breakpoints.md">Q&amp;A</a></td>
      <td><a href="010-color-schemes-dark-mode-and-css-variables.md">010 - Color schemes, dark mode, and CSS variables</a></td>
    </tr>
  </tbody>
</table>
