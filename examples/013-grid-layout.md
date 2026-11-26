# Grid Layout - Example

## Goal

Build a responsive workspace dashboard with four summary cards, a main activity area, and a sidebar. Use Grid for the two-dimensional page layout and Stack for vertical content inside each area.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Material UI's current Grid component with the \`size\` API.
- Familiarity with the Grid lesson and the earlier Box, Container, Stack, and Typography lessons.

## Example: responsive workspace dashboard

Save this as \`App.jsx\` and render \`<App />\` from the existing application entry point.

```jsx
import Grid from '@mui/material/Grid';
import Box from '@mui/material/Box';
import Container from '@mui/material/Container';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

const metrics = [
  { label: 'Active projects', value: '12', detail: 'Across 4 teams' },
  { label: 'Open tasks', value: '38', detail: '7 due this week' },
  { label: 'Team members', value: '16', detail: '2 invited' },
  { label: 'Updates', value: '24', detail: 'Since your last visit' },
];

const activity = [
  {
    id: 'activity-design',
    title: 'Design review completed',
    detail: 'The onboarding flow is ready for feedback.',
  },
  {
    id: 'activity-release',
    title: 'Release checklist updated',
    detail: 'Three items still need an owner.',
  },
  {
    id: 'activity-invite',
    title: 'New member invited',
    detail: 'An invitation is waiting for acceptance.',
  },
];

function MetricCard({ label, value, detail }) {
  return (
    <Paper
      component="article"
      variant="outlined"
      sx={{ height: '100%', p: 2.5 }}
    >
      <Stack spacing={1}>
        <Typography variant="body2" color="text.secondary">
          {label}
        </Typography>
        <Typography variant="h3" component="p">
          {value}
        </Typography>
        <Typography variant="body2">{detail}</Typography>
      </Stack>
    </Paper>
  );
}

function ActivityItem({ item }) {
  return (
    <Box component="li" sx={{ listStyle: 'none' }}>
      <Stack spacing={0.5}>
        <Typography variant="subtitle1" component="h3">
          {item.title}
        </Typography>
        <Typography variant="body2" color="text.secondary">
          {item.detail}
        </Typography>
      </Stack>
    </Box>
  );
}

export default function App() {
  return (
    <Container component="main" maxWidth="lg" sx={{ py: { xs: 3, md: 5 } }}>
      <Stack spacing={4}>
        <Box component="header">
          <Typography variant="h1" component="h1" gutterBottom>
            Workspace overview
          </Typography>
          <Typography color="text.secondary">
            A quick view of current projects and team activity.
          </Typography>
        </Box>

        <Grid
          container
          rowSpacing={{ xs: 2, md: 3 }}
          columnSpacing={{ xs: 2, md: 3 }}
        >
          {metrics.map((metric) => (
            <Grid key={metric.label} size={{ xs: 12, sm: 6, md: 3 }}>
              <MetricCard {...metric} />
            </Grid>
          ))}
        </Grid>

        <Grid container spacing={{ xs: 2, md: 3 }}>
          <Grid size={{ xs: 12, md: 8 }}>
            <Paper
              component="section"
              variant="outlined"
              aria-labelledby="activity-heading"
              sx={{ height: '100%', p: { xs: 2, sm: 3 } }}
            >
              <Stack spacing={3}>
                <Typography id="activity-heading" variant="h2" component="h2">
                  Recent activity
                </Typography>
                <Stack
                  component="ul"
                  spacing={2.5}
                  useFlexGap
                  sx={{ m: 0, p: 0 }}
                >
                  {activity.map((item) => (
                    <ActivityItem key={item.id} item={item} />
                  ))}
                </Stack>
              </Stack>
            </Paper>
          </Grid>

          <Grid size={{ xs: 12, md: 4 }}>
            <Paper
              component="aside"
              variant="outlined"
              aria-labelledby="attention-heading"
              sx={{ height: '100%', p: { xs: 2, sm: 3 } }}
            >
              <Stack spacing={2}>
                <Typography
                  id="attention-heading"
                  variant="h2"
                  component="h2"
                >
                  Needs attention
                </Typography>
                <Typography>
                  Seven tasks are due this week. Review the owners and adjust
                  priorities if the schedule has changed.
                </Typography>
              </Stack>
            </Paper>
          </Grid>
        </Grid>
      </Stack>
    </Container>
  );
}
```

All counts and activity entries are illustrative sample data.

## How the layout works

- The page-level Container provides a centered maximum width and gutters.
- The first Grid container creates a responsive summary-card layout.
- Each summary card uses all columns at xs, half at sm, and one quarter at md.
- The second Grid container divides the page into an 8-column activity area and a 4-column sidebar at md.
- Both lower sections use all columns at narrow widths.
- \`rowSpacing\` and \`columnSpacing\` keep the card gaps consistent while allowing different responsive values.
- Stack handles vertical spacing inside cards and sections.

## Expected result

At narrow widths, the summary cards and lower sections stack vertically. At medium widths, the summary cards form four columns and the activity section sits beside the attention panel. The cards use consistent gaps and remain inside the page Container.

## Verification checklist

1. Confirm that the file imports \`Grid\` from \`@mui/material/Grid\`.
2. Confirm there is no legacy \`item\`, \`xs\`, or \`sm\` prop on Grid children.
3. At a narrow width, verify that summary cards and lower sections stack.
4. At a medium width, verify that summary cards form four columns and the main panel is wider than the sidebar.
5. Check that text wraps and no panel creates horizontal page overflow.
6. Inspect the rendered headings and named sections.
7. Confirm that spacing is consistent between rows and columns.

## Practice extensions

1. Add a chart placeholder area and decide how many columns it should occupy.
2. Change the card widths at a large breakpoint and observe the wrap behavior.
3. Add an action aligned to the right of the page title using a Grid row or Stack.
4. Try a custom \`columns\` value and update all item sizes to match.
5. Use CSS Grid for a variation that requires an item to span two rows, then compare which primitive expresses the layout more clearly.

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
      <td><a href="012-box-container-and-stack-layout.md">012 - Box, Container, and Stack layout</a></td>
      <td><a href="../lessons/013-grid-layout.md">Lesson</a></td>
      <td><a href="../qnas/013-grid-layout.md">Q&amp;A</a></td>
      <td><a href="014-buttons-icon-buttons-and-button-groups.md">014 - Buttons, icon buttons, and button groups</a></td>
    </tr>
  </tbody>
</table>
