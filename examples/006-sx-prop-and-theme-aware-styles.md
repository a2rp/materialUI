# The sx Prop and Theme-Aware Styles - Example

## Goal

Create responsive cards using the sx prop. The example uses palette paths, spacing transformations, breakpoints, nested selectors, and a theme callback.

## Prerequisites

- A React application with Material UI installed.
- ThemeProvider and CssBaseline configured.
- A development server command in the app's package manifest.

## File structure

Use the application's existing App file:

```text
src/
  App.jsx
```

## Build the responsive card layout

Replace `src/App.jsx`:

```jsx
import Box from '@mui/material/Box';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

const cards = [
  {
    title: 'Theme values',
    description: 'Use palette paths and spacing tokens from the active theme.',
  },
  {
    title: 'Responsive styles',
    description: 'Change layout at named breakpoints without conditional React.',
  },
];

export default function App() {
  return (
    <Box
      component="main"
      sx={{
        minHeight: '100vh',
        bgcolor: 'background.default',
        p: { xs: 2, md: 5 },
      }}
    >
      <Stack spacing={3} sx={{ maxWidth: 960, mx: 'auto' }}>
        <Typography variant="h4" component="h1">
          Theme-aware cards
        </Typography>

        <Box
          sx={{
            display: 'grid',
            gridTemplateColumns: {
              xs: '1fr',
              md: 'repeat(2, minmax(0, 1fr))',
            },
            gap: 2,
          }}
        >
          {cards.map((card) => (
            <Paper
              key={card.title}
              sx={{
                p: { xs: 2, sm: 3 },
                color: 'text.primary',
                bgcolor: 'background.paper',
                borderRadius: 2,
                boxShadow: 1,
                transition: 'box-shadow 160ms ease',
                '&:hover': {
                  boxShadow: 4,
                },
                '& .card-description': {
                  color: 'text.secondary',
                },
              }}
            >
              <Stack spacing={1}>
                <Typography variant="h6">{card.title}</Typography>
                <Typography className="card-description">
                  {card.description}
                </Typography>
              </Stack>
            </Paper>
          ))}
        </Box>
      </Stack>
    </Box>
  );
}
```

## Add a computed theme value

When a style needs calculation, use a callback for the entire sx object:

```jsx
<Box
  sx={(theme) => ({
    border: '1px solid ' + theme.palette.divider,
    color: theme.palette.primary.main,
    p: theme.spacing(2),
  })}
/>
```

This callback is optional for simple theme paths. The cards use string paths and responsive values because those styles do not need additional computation.

## Run the example

Start the React development server with the command configured by the starter. For a Vite app, this is usually:

```sh
npm run dev
```

Resize the viewport. The cards should stack at narrow widths and display in two columns at the md breakpoint and above.

## What to notice

- p and gap are spacing-aware system values.
- Palette string paths use the active theme.
- Grid columns change at a theme breakpoint.
- The hover selector targets the current Paper component.
- The child selector is scoped to the card.
- The computed example uses one callback around the whole sx object.

## Variations

1. Change gap from 2 to 3 and compare the theme spacing.
2. Add a border color from the palette divider token.
3. Add a responsive change to the heading size.
4. Try a fractional width on one card and compare it with a numeric padding value. These properties use different transforms.
5. Move the card styles into a reusable styled component and decide whether the extra abstraction helps.

## Common mistakes

- **Treating every number as a spacing value:** transformations depend on the sx key.
- **Writing a callback inside one property:** use the callback around the complete sx object.
- **Using an unscoped descendant selector:** keep the selector rooted at the current component.
- **Using media queries in React for a visual-only change:** use responsive styles when component behavior does not need to change.
- **Animating every property:** keep transitions limited to visual properties that should change.
- **Passing arbitrary user text into CSS:** validate dynamic style values.

## Practice check

Explain how the card's p value differs from its width value, which theme setting controls the responsive threshold, and what the hover selector refers to.

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
      <td><a href="005-theme-tokens-and-design-system.md">005 - Theme tokens and design system</a></td>
      <td><a href="../lessons/006-sx-prop-and-theme-aware-styles.md">Lesson</a></td>
      <td><a href="../qnas/006-sx-prop-and-theme-aware-styles.md">Q&amp;A</a></td>
      <td><a href="007-styled-api-and-reusable-styles.md">007 - The styled API and reusable styles</a></td>
    </tr>
  </tbody>
</table>
