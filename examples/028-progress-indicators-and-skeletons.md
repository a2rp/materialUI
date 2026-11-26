# Progress Indicators and Skeletons - Example

## Goal

Build a report panel that demonstrates loading, successful results, an empty result, and a failed request. Use a linear progress bar and matching Skeleton placeholders while loading.

## Prerequisites

- An existing React application.
- Material UI and its default Emotion styling packages.

Install the packages if they are not already present:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

## File structure

Add the component to the existing application:

```text
src/
  App.jsx
```

## Example: report loading states

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Alert from '@mui/material/Alert';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import LinearProgress from '@mui/material/LinearProgress';
import Paper from '@mui/material/Paper';
import Skeleton from '@mui/material/Skeleton';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

const sampleReports = [
  {
    id: 'weekly',
    title: 'Weekly activity',
    detail: 'A summary of project activity for the current week.',
  },
  {
    id: 'quality',
    title: 'Quality review',
    detail: 'Open review items grouped by project and owner.',
  },
  {
    id: 'delivery',
    title: 'Delivery status',
    detail: 'Progress across active delivery milestones.',
  },
];

export default function App() {
  const [loadState, setLoadState] = useState('idle');
  const [reports, setReports] = useState(null);
  const [status, setStatus] = useState('Choose an option to load reports.');

  const loading = loadState === 'loading';

  function simulateRequest(resultType) {
    if (loading) {
      return;
    }

    setLoadState('loading');
    setStatus('Loading reports.');

    window.setTimeout(() => {
      if (resultType === 'success') {
        setReports(sampleReports);
        setLoadState('complete');
        setStatus(sampleReports.length + ' reports loaded.');
      } else if (resultType === 'empty') {
        setReports([]);
        setLoadState('complete');
        setStatus('The request completed. No reports were found.');
      } else {
        setReports(null);
        setLoadState('error');
        setStatus('The report request failed.');
      }
    }, 1000);
  }

  function retryRequest() {
    simulateRequest('success');
  }

  return (
    <Container component="main" maxWidth="md" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={3}>
        <Box>
          <Typography component="h1" variant="h4" gutterBottom>
            Reports
          </Typography>
          <Typography color="text.secondary">
            The buttons below simulate different request outcomes.
          </Typography>
        </Box>

        <Stack direction="row" spacing={1} flexWrap="wrap">
          <Button
            variant="contained"
            disabled={loading}
            onClick={() => simulateRequest('success')}
          >
            Load reports
          </Button>
          <Button
            variant="outlined"
            disabled={loading}
            onClick={() => simulateRequest('empty')}
          >
            Simulate empty result
          </Button>
          <Button
            variant="outlined"
            disabled={loading}
            onClick={() => simulateRequest('error')}
          >
            Simulate error
          </Button>
        </Stack>

        <Typography role="status" aria-live="polite" color="text.secondary">
          {status}
        </Typography>

        <Paper
          component="section"
          aria-labelledby="report-list-heading"
          aria-describedby={loading ? 'reports-progress' : undefined}
          aria-busy={loading}
          variant="outlined"
        >
          <Typography
            id="report-list-heading"
            component="h2"
            variant="h6"
            sx={{ px: 2, pt: 2 }}
          >
            Available reports
          </Typography>

          {loading && (
            <>
              <LinearProgress
                id="reports-progress"
                aria-label="Loading reports"
              />
              <Stack aria-hidden="true" spacing={1} sx={{ p: 2 }}>
                {[1, 2, 3].map((item) => (
                  <Paper key={item} variant="outlined" sx={{ p: 2 }}>
                    <Skeleton variant="text" width="45%" />
                    <Skeleton variant="text" width="80%" />
                  </Paper>
                ))}
              </Stack>
            </>
          )}

          {!loading && loadState === 'error' && (
            <Alert
              severity="error"
              action={
                <Button color="inherit" size="small" onClick={retryRequest}>
                  Retry
                </Button>
              }
              sx={{ m: 2 }}
            >
              Reports could not be loaded. Try again.
            </Alert>
          )}

          {!loading && loadState === 'complete' && reports?.length === 0 && (
            <Alert severity="info" role="status" sx={{ m: 2 }}>
              No reports are available for this view.
            </Alert>
          )}

          {!loading && reports?.length > 0 && (
            <Stack spacing={1} sx={{ p: 2 }}>
              {reports.map((report) => (
                <Paper key={report.id} variant="outlined" sx={{ p: 2 }}>
                  <Typography component="h3" variant="subtitle1">
                    {report.title}
                  </Typography>
                  <Typography color="text.secondary">
                    {report.detail}
                  </Typography>
                </Paper>
              ))}
            </Stack>
          )}

          {loadState === 'idle' && (
            <Typography color="text.secondary" sx={{ p: 2 }}>
              No request has been made yet.
            </Typography>
          )}
        </Paper>
      </Stack>
    </Container>
  );
}
```

## How it works

- The action buttons simulate a one-second request using a timer. Replace this with the application's real data request when integrating the pattern.
- While the request is pending, the report region is marked busy and points to the progress indicator.
- The indeterminate LinearProgress communicates that work is happening without inventing a percentage.
- Skeleton rows are hidden from assistive technology because the live status outside the busy region already announces the loading state.
- After completion, the panel switches to real sample data, an explicit empty state, or a recoverable error.
- The error Alert includes a Retry action. The request state is cleared before retrying.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. Select Load reports and observe the progress bar and skeleton rows.
3. Wait for the sample report cards to replace the loading state.
4. Choose Simulate empty result and confirm the page shows an intentional empty message.
5. Choose Simulate error and activate Retry.
6. Use a screen reader or accessibility tree to inspect the loading status, busy region, labelled progress bar, and final state.
7. Confirm the page does not leave Skeleton placeholders after success, empty results, or failure.

## Variations

1. Replace the one-second timer with a real asynchronous request and handle cancellation when the request supports it.
2. Add determinate progress for a file upload with a measured value from the upload process.
3. Add a CircularProgress beside a button while one item is being refreshed.
4. Add an empty-state action that helps users create or find a report.
5. Disable Skeleton animation when shimmer is distracting or conflicts with a reduced-motion setting.

## Common mistakes

- Showing fake percentage progress for a request whose completion amount is unknown.
- Leaving the region marked busy after the request has finished.
- Treating an empty response as a loading state.
- Showing an endless skeleton after a request fails.
- Announcing each decorative Skeleton line separately.
- Calling the timer a real server request. This example simulates the delay and response locally.

## Practice

Add a fourth sample report. Then add a determinate upload panel that uses a progress value from an actual operation or a clearly labelled local demonstration. Do not reuse the indeterminate report indicator as if it represented a measured percentage.

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
      <td><a href="027-alerts-snackbars-and-feedback.md">027 - Alerts, snackbars, and feedback</a></td>
      <td><a href="../lessons/028-progress-indicators-and-skeletons.md">Lesson</a></td>
      <td><a href="../qnas/028-progress-indicators-and-skeletons.md">Q&amp;A</a></td>
      <td><a href="029-chips-avatars-and-badges.md">029 - Chips, avatars, and badges</a></td>
    </tr>
  </tbody>
</table>
