# Alerts, Snackbars, and Feedback - Example

## Goal

Build a project settings form that shows a persistent inline Alert when validation fails and a temporary Snackbar after saving. The Snackbar includes an Undo action and handles timeout, click-away, and Escape close requests.

## Prerequisites

- An existing React application.
- Material UI and its default Emotion styling packages.

Install the packages if they are not already present:

```sh
npm install @mui/material @emotion/react @emotion/styled
```

## File structure

Add this component to the existing application:

```text
src/
��y��yۧu���@�w^~)�t App.jsx
```

## Example: project name feedback

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Alert from '@mui/material/Alert';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import Paper from '@mui/material/Paper';
import Snackbar from '@mui/material/Snackbar';
import Stack from '@mui/material/Stack';
import TextField from '@mui/material/TextField';
import Typography from '@mui/material/Typography';

export default function App() {
  const [projectName, setProjectName] = useState('Atlas');
  const [savedName, setSavedName] = useState('Atlas');
  const [previousSavedName, setPreviousSavedName] = useState('Atlas');
  const [validationError, setValidationError] = useState('');
  const [snackbarOpen, setSnackbarOpen] = useState(false);
  const [snackbarMessage, setSnackbarMessage] = useState('');
  const [snackbarId, setSnackbarId] = useState(0);
  const [status, setStatus] = useState('No changes have been saved.');

  function handleSubmit(event) {
    event.preventDefault();

    const trimmedName = projectName.trim();

    if (!trimmedName) {
      setValidationError('Enter a project name before saving.');
      return;
    }

    setValidationError('');
    setPreviousSavedName(savedName);
    setSavedName(trimmedName);
    setProjectName(trimmedName);
    setSnackbarMessage('Project name saved.');
    setSnackbarId((currentId) => currentId + 1);
    setSnackbarOpen(true);
    setStatus('Project name saved as ' + trimmedName + '.');
  }

  function handleSnackbarClose(event, reason) {
    if (reason === 'clickaway') {
      return;
    }

    setSnackbarOpen(false);
  }

  function undoSave() {
    setSavedName(previousSavedName);
    setProjectName(previousSavedName);
    setSnackbarOpen(false);
    setStatus('The previous project name was restored.');
  }

  function dismissValidationError() {
    setValidationError('');
  }

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={3}>
        <Box>
          <Typography component="h1" variant="h4" gutterBottom>
            Project settings
          </Typography>
          <Typography color="text.secondary">
            Update the project name and review the save feedback.
          </Typography>
        </Box>

        {validationError && (
          <Alert severity="error" onClose={dismissValidationError}>
            {validationError}
          </Alert>
        )}

        <Paper
          component="form"
          variant="outlined"
          onSubmit={handleSubmit}
          sx={{ p: { xs: 2, sm: 3 } }}
        >
          <Stack spacing={2}>
            <TextField
              label="Project name"
              value={projectName}
              onChange={(event) => setProjectName(event.target.value)}
              error={Boolean(validationError)}
              helperText={validationError || 'Use a short, recognizable name.'}
              fullWidth
            />

            <Button type="submit" variant="contained" sx={{ alignSelf: 'flex-start' }}>
              Save changes
            </Button>
          </Stack>
        </Paper>

        <Paper variant="outlined" sx={{ p: 2 }}>
          <Typography component="h2" variant="h6">
            Saved value
          </Typography>
          <Typography>{savedName}</Typography>
        </Paper>

        <Typography role="status" aria-live="polite" color="text.secondary">
          {status}
        </Typography>

        <Snackbar
          key={snackbarId}
          open={snackbarOpen}
          autoHideDuration={6000}
          onClose={handleSnackbarClose}
          anchorOrigin={{ vertical: 'bottom', horizontal: 'center' }}
          message={snackbarMessage}
          action={
            <Button color="inherit" size="small" onClick={undoSave}>
              Undo
            </Button>
          }
        />
      </Stack>
    </Container>
  );
}
```

## How it works

- Empty input displays a persistent Alert with a close action and field-level helper text.
- Successful submission updates local state and opens a Snackbar.
- The Snackbar stays open for six seconds unless the user activates Undo or presses Escape. Click-away is ignored so the action is not dismissed accidentally.
- Undo restores the previous saved value and updates the status message.
- The Snackbar key changes for each successful save so repeated messages are handled as distinct notifications.
- The save and undo behavior updates local React state only. Connect it to the application's data layer for a real persistence workflow.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. Clear the project name and submit. Confirm the error is shown inline and remains available until dismissed.
3. Enter a valid name and submit. Confirm that the saved value changes and the Snackbar appears.
4. Activate Undo and confirm the previous name is restored.
5. Save again, then test timeout, Escape, and click-away behavior.
6. Use only a keyboard to reach the form, submit it, read the error, and activate Undo.
7. Confirm the status message explains the result without moving focus.

## Variations

1. Add a request delay and disable the Save button while the request is pending.
2. Add a persistent error Alert when saving fails and keep the user's input.
3. Add a second Snackbar action only if it is clear and keyboard reachable.
4. Place the Snackbar above a mobile bottom action bar using an appropriate anchor position.
5. Replace the sample Undo behavior with a real rollback operation.

## Common mistakes

- Automatically hiding validation errors makes them easy to miss.
- A six-second timer is not suitable for critical information.
- Snackbar's auto-hide callback does not set the controlled open state by itself.
- Ignoring click-away should not remove all close paths. Keep timeout, Escape, or a visible control.
- Repeated messages need distinct notification state so an old timer does not close a newer message.
- Snackbar Undo should not promise a server rollback when it only changes local state.

## Practice

Add a warning Alert when the project name exceeds a chosen length. Keep the message visible, explain the limit, and preserve the entered value while the user edits it.

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
      <td><a href="026-popovers-tooltips-and-click-away.md">026 - Popovers, tooltips, and click-away</a></td>
      <td><a href="../lessons/027-alerts-snackbars-and-feedback.md">Lesson</a></td>
      <td><a href="../qnas/027-alerts-snackbars-and-feedback.md">Q&amp;A</a></td>
      <td><a href="028-progress-indicators-and-skeletons.md">028 - Progress indicators and skeletons</a></td>
    </tr>
  </tbody>
</table>
