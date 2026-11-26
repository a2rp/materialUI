# Buttons, Icon Buttons, and Button Groups - Example

## Goal

Create a report action panel with a primary save action, a navigation link, a labelled group of related actions, and an icon-only action with an accessible name.

## Prerequisites

- A React application with Material UI and Emotion installed.
- The Material UI icon package installed for the trash icon used in this example.
- Familiarity with React state and event handlers.

## Example: report actions

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import ButtonGroup from '@mui/material/ButtonGroup';
import Container from '@mui/material/Container';
import IconButton from '@mui/material/IconButton';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Tooltip from '@mui/material/Tooltip';
import Typography from '@mui/material/Typography';
import DeleteOutlineIcon from '@mui/icons-material/DeleteOutline';

export default function App() {
  const [isSaving, setIsSaving] = useState(false);
  const [message, setMessage] = useState('');

  function saveReport() {
    setIsSaving(true);
    setMessage('Saving report...');

    window.setTimeout(() => {
      setIsSaving(false);
      setMessage('Report saved.');
    }, 800);
  }

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Paper
        component="section"
        variant="outlined"
        aria-labelledby="report-heading"
        sx={{ p: { xs: 2, sm: 3 } }}
      >
        <Stack spacing={3}>
          <Box>
            <Typography id="report-heading" variant="h1" component="h1">
              Monthly report
            </Typography>
            <Typography color="text.secondary">
              Review the report, save your changes, or choose another action.
            </Typography>
          </Box>

          <Stack
            direction={{ xs: 'column', sm: 'row' }}
            spacing={1.5}
            useFlexGap
            sx={{ alignItems: { xs: 'stretch', sm: 'center' } }}
          >
            <Button
              variant="contained"
              loading={isSaving}
              onClick={saveReport}
            >
              Save report
            </Button>
            <Button href="/reports" variant="outlined">
              All reports
            </Button>
          </Stack>

          <Box>
            <Typography variant="subtitle1" component="h2" gutterBottom>
              More report actions
            </Typography>
            <ButtonGroup
              variant="outlined"
              size="small"
              aria-label="More report actions"
            >
              <Button onClick={() => setMessage('Report link copied.')}>
                Copy link
              </Button>
              <Button onClick={() => setMessage('Download started.')}>
                Download
              </Button>
              <Button onClick={() => setMessage('Print dialog requested.')}>
                Print
              </Button>
            </ButtonGroup>
          </Box>

          <Stack direction="row" spacing={1} sx={{ alignItems: 'center' }}>
            <Tooltip title="Remove this draft report">
              <IconButton
                aria-label="Remove this draft report"
                color="error"
                onClick={() => setMessage('Draft removal requested.')}
              >
                <DeleteOutlineIcon />
              </IconButton>
            </Tooltip>
            <Typography variant="body2" color="text.secondary">
              Draft removal is a demonstration action.
            </Typography>
          </Stack>

          <Typography aria-live="polite" role="status" color="text.secondary">
            {message}
          </Typography>
        </Stack>
      </Paper>
    </Container>
  );
}
```

The report actions are illustrative. Save, download, copy, print, and removal handlers only update a status message; they do not access a server or modify a real report.

## How the controls work

- The contained Save button is the primary action.
- The outlined All reports control uses `href` and represents navigation.
- The report actions are grouped under an accessible group name.
- Each grouped button runs a separate action handler.
- The IconButton has an accessible name that describes its action.
- Tooltip adds a visible hint but does not replace the IconButton's `aria-label`.
- The loading state prevents repeated save interaction while the simulated save is pending.
- The status message uses a polite live region for feedback.
- The action row stacks on narrow screens and becomes horizontal on wider screens.

## Expected result

The panel displays a clear save action, a navigation link, three grouped secondary actions, and a delete icon action. Activating an action updates the status message. While Save report is loading, it displays progress and is disabled.

## Verification checklist

1. Use Tab and Shift+Tab to move through the controls and confirm that focus is visible.
2. Check that the IconButton is announced with its action name by assistive technology.
3. Confirm that All reports is rendered as a link and actions are rendered as buttons.
4. Activate each action and confirm that a status message appears.
5. Activate Save report and verify that it enters and exits the loading state.
6. Resize to a narrow viewport and confirm the primary action row stacks without overflow.
7. Confirm that the ButtonGroup has an accessible label and all actions remain individually available.

## Practice extensions

1. Replace the simulated save handler with the application's actual save operation and keep the loading state tied to its promise.
2. Add a confirmation dialog before removal rather than performing a destructive action immediately.
3. Change the three action buttons to a vertical group when the panel becomes narrow.
4. Add a secondary icon to a text button while keeping the action name visible.
5. Decide how the UI should communicate a save error and how users can retry.

## Common mistakes

- Giving an icon-only button no accessible name or relying on its tooltip as the only label.
- Using a navigation link for an operation or a button for a destination.
- Presenting a sample status update as though it saved or changed data on a server.

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
      <td><a href="013-grid-layout.md">013 - Grid layout</a></td>
      <td><a href="../lessons/014-buttons-icon-buttons-and-button-groups.md">Lesson</a></td>
      <td><a href="../qnas/014-buttons-icon-buttons-and-button-groups.md">Q&amp;A</a></td>
      <td><a href="015-text-fields-and-input-components.md">015 - Text fields and input components</a></td>
    </tr>
  </tbody>
</table>
