# Dialogs and Modal Workflows - Example

## Goal

Build a project card with an archive or restore confirmation dialog. The dialog explains the consequence, offers clear actions, and returns focus to the same project control after the workflow finishes.

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

## Example: archive confirmation

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Alert from '@mui/material/Alert';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import Dialog from '@mui/material/Dialog';
import DialogActions from '@mui/material/DialogActions';
import DialogContent from '@mui/material/DialogContent';
import DialogContentText from '@mui/material/DialogContentText';
import DialogTitle from '@mui/material/DialogTitle';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

export default function App() {
  const [archived, setArchived] = useState(false);
  const [dialogOpen, setDialogOpen] = useState(false);
  const [pendingAction, setPendingAction] = useState(null);
  const [status, setStatus] = useState('The project is active.');

  function openConfirmation() {
    setPendingAction(archived ? 'restore' : 'archive');
    setDialogOpen(true);
  }

  function closeConfirmation() {
    setDialogOpen(false);
  }

  function confirmAction() {
    const nextArchived = pendingAction === 'archive';
    setArchived(nextArchived);
    setStatus(
      nextArchived
        ? 'Northstar research was archived in this local example.'
        : 'Northstar research was restored in this local example.',
    );
    setDialogOpen(false);
  }

  const actionLabel = archived ? 'Restore project' : 'Archive project';
  const dialogTitle = archived ? 'Restore this project?' : 'Archive this project?';
  const dialogDescription = archived
    ? 'Northstar research will return to the active project list.'
    : 'Northstar research will be hidden from active work. You can restore it later.';

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={3}>
        <Box>
          <Typography component="h1" variant="h4" gutterBottom>
            Projects
          </Typography>
          <Typography color="text.secondary">
            Manage the status of a workspace project.
          </Typography>
        </Box>

        <Paper component="article" variant="outlined" sx={{ p: 3 }}>
          <Stack spacing={2}>
            <Box>
              <Typography component="h2" variant="h6">
                Northstar research
              </Typography>
              <Typography color="text.secondary">
                {archived ? 'Archived project' : 'Active project'}
              </Typography>
            </Box>

            <Button variant="outlined" onClick={openConfirmation}>
              {actionLabel}
            </Button>
          </Stack>
        </Paper>

        <Alert role="status" aria-live="polite" severity="info">
          {status}
        </Alert>
      </Stack>

      <Dialog
        open={dialogOpen}
        onClose={closeConfirmation}
        aria-labelledby="project-confirmation-title"
        aria-describedby="project-confirmation-description"
        fullWidth
        maxWidth="xs"
      >
        <DialogTitle id="project-confirmation-title">{dialogTitle}</DialogTitle>

        <DialogContent>
          <DialogContentText id="project-confirmation-description">
            {dialogDescription}
          </DialogContentText>
        </DialogContent>

        <DialogActions>
          <Button onClick={closeConfirmation}>Cancel</Button>
          <Button variant="contained" onClick={confirmAction}>
            {archived ? 'Restore project' : 'Archive project'}
          </Button>
        </DialogActions>
      </Dialog>
    </Container>
  );
}
```

## How it works

- The project action opens the dialog and stores which operation is pending.
- Dialog is controlled with `dialogOpen`. The close handler supports both Escape and backdrop dismissal.
- The title and description ids are connected with `aria-labelledby` and `aria-describedby`.
- Cancel closes the dialog without changing the project state.
- Confirm updates the local example state, closes the dialog, and announces the result.
- The same project action button remains in the page after the change. Its label changes to the opposite operation, so focus restoration has a stable target.
- This example changes local React state only. Connect the confirmation handler to the application's data layer before using it in a real product.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. Activate "Archive project" and review the title, description, and available actions.
3. Press Escape or choose Cancel. Confirm that the project state does not change.
4. Open the dialog again and confirm the action.
5. Confirm the status message updates, the action button label changes, and focus returns to that button.
6. Use browser zoom and a narrow viewport to check that the dialog fits and the actions remain available.
7. Navigate with a keyboard and verify that focus stays inside the open dialog.

## Variations

1. Add an asynchronous request to the confirmation action. Disable repeated submission and show progress while it is pending.
2. If the request fails, keep the dialog open and explain how the user can retry.
3. Add a dialog form for changing a project name and preserve its field value after validation errors.
4. Use `fullScreen` on a small viewport when the task needs a full-screen flow.
5. Add an audit message that records who archived the project and when.

## Common mistakes

- Closing the dialog visually without changing the state passed to `open` leaves its control state out of sync.
- A generic title such as "Confirm" does not explain the pending action.
- Closing on confirm before completing an asynchronous request can hide errors from the user.
- A dialog that is not labelled or described is harder to understand with assistive technology.
- Removing the trigger from the page can leave focus without a useful destination. Provide a logical focus target after the action.
- A demo's local state update is not a server-side archive operation.

## Practice

Add a confirmation dialog for deleting a saved search. Explain what will be deleted, keep Cancel easy to find, and move focus to the next saved search after deletion if the original trigger is removed.

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
      <td><a href="024-tabs-and-tab-panels.md">024 - Tabs and tab panels</a></td>
      <td><a href="../lessons/025-dialogs-and-modal-workflows.md">Lesson</a></td>
      <td><a href="../qnas/025-dialogs-and-modal-workflows.md">Q&amp;A</a></td>
      <td><a href="026-popovers-tooltips-and-click-away.md">026 - Popovers, tooltips, and click-away</a></td>
    </tr>
  </tbody>
</table>
