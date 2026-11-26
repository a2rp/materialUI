# Accessibility and Keyboard Interaction: Keyboard-First Project Settings Example

This example brings accessible names, state, focus, and feedback together in one small project settings screen. It includes a skip link, a labelled form field, a toggle IconButton, an expandable details section, a polite status region, and a confirmation dialog.

## What you will build

- A keyboard-visible skip link to the main content.
- A form with a persistent label, required state, and corrective error text.
- A favorite toggle with aria-pressed.
- A details disclosure with aria-expanded and aria-controls.
- A status message announced without moving focus.
- A labelled delete dialog with a cancel, confirm, and undo path.

## Requirements

Install Material UI, its default styling engine, and the icon package:

```bash
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
```

This example uses the current theme-level focusVisible option. Confirm that your installed Material UI version supports it; if not, use the focus styling approach supported by that version.

## `src/App.jsx`

Replace the starter component with:

```jsx
import { useState } from "react";
import {
  Box,
  Button,
  Container,
  Dialog,
  DialogActions,
  DialogContent,
  DialogContentText,
  DialogTitle,
  IconButton,
  Link,
  Paper,
  Stack,
  TextField,
  ThemeProvider,
  Tooltip,
  Typography,
} from "@mui/material";
import { createTheme } from "@mui/material/styles";
import FavoriteIcon from "@mui/icons-material/Favorite";
import FavoriteBorderIcon from "@mui/icons-material/FavoriteBorder";

const theme = createTheme({
  focusVisible: true,
});

export default function App() {
  const [projectName, setProjectName] = useState("Atlas");
  const [favorite, setFavorite] = useState(false);
  const [detailsOpen, setDetailsOpen] = useState(false);
  const [deleteDialogOpen, setDeleteDialogOpen] = useState(false);
  const [deleted, setDeleted] = useState(false);
  const [error, setError] = useState("");
  const [statusMessage, setStatusMessage] = useState("");

  function handleSubmit(event) {
    event.preventDefault();

    const normalizedName = projectName.trim();
    if (!normalizedName) {
      setError("Enter a project name before saving.");
      setStatusMessage("The project name is required.");
      return;
    }

    setProjectName(normalizedName);
    setError("");
    setStatusMessage("Project settings saved.");
  }

  function confirmDelete() {
    setDeleteDialogOpen(false);
    setDeleted(true);
    setStatusMessage("Project removed. Use Undo to restore it.");
  }

  function restoreProject() {
    setDeleted(false);
    setStatusMessage("Project restored.");
  }

  return (
    <ThemeProvider theme={theme}>
      <Container maxWidth="sm" sx={{ py: 5 }}>
        <Link
          href="#main-content"
          sx={{
            position: "absolute",
            left: -10000,
            top: 8,
            zIndex: 1500,
            p: 1,
            bgcolor: "background.paper",
            "&:focus": { left: 8 },
          }}
        >
          Skip to main content
        </Link>

        <Box component="main" id="main-content" tabIndex={-1}>
          <Typography component="h1" variant="h4" fontWeight={700} sx={{ mb: 2 }}>
            Project settings
          </Typography>

          {deleted ? (
            <Paper variant="outlined" sx={{ p: 2 }}>
              <Typography sx={{ mb: 1 }}>Atlas was removed.</Typography>
              <Button onClick={restoreProject}>Undo remove</Button>
            </Paper>
          ) : (
            <Paper variant="outlined" sx={{ p: 3 }}>
              <Stack spacing={3}>
                <Stack
                  direction="row"
                  alignItems="center"
                  justifyContent="space-between"
                  spacing={2}
                >
                  <Box>
                    <Typography component="h2" variant="h6">
                      {projectName}
                    </Typography>
                    <Typography color="text.secondary">
                      Platform team project
                    </Typography>
                  </Box>

                  <Tooltip
                    describeChild
                    title="Adds or removes this project from favorites"
                  >
                    <IconButton
                      aria-label="Favorite project"
                      aria-pressed={favorite}
                      onClick={() => setFavorite((current) => !current)}
                    >
                      {favorite ? <FavoriteIcon /> : <FavoriteBorderIcon />}
                    </IconButton>
                  </Tooltip>
                </Stack>

                <Button
                  aria-expanded={detailsOpen}
                  aria-controls="project-details"
                  onClick={() => setDetailsOpen((current) => !current)}
                >
                  {detailsOpen ? "Hide project details" : "Show project details"}
                </Button>

                <Box
                  id="project-details"
                  hidden={!detailsOpen}
                  sx={{ p: 2, bgcolor: "action.hover", borderRadius: 1 }}
                >
                  <Typography>
                    The project is owned by the Platform team and is currently active.
                  </Typography>
                </Box>

                <Box component="form" noValidate onSubmit={handleSubmit}>
                  <Stack spacing={2}>
                    <TextField
                      label="Project name"
                      value={projectName}
                      required
                      error={Boolean(error)}
                      helperText={error || "Enter the name shown to your team."}
                      onChange={(event) => {
                        setProjectName(event.target.value);
                        if (error) setError("");
                      }}
                      fullWidth
                    />

                    <Stack direction={{ xs: "column", sm: "row" }} spacing={1}>
                      <Button type="submit" variant="contained">
                        Save settings
                      </Button>
                      <Button
                        type="button"
                        color="error"
                        onClick={() => setDeleteDialogOpen(true)}
                      >
                        Delete project
                      </Button>
                    </Stack>
                  </Stack>
                </Box>
              </Stack>
            </Paper>
          )}

          <Box role="status" aria-live="polite" sx={{ minHeight: 28, mt: 2 }}>
            {statusMessage}
          </Box>
        </Box>

        <Dialog
          open={deleteDialogOpen}
          onClose={() => setDeleteDialogOpen(false)}
          aria-labelledby="delete-project-title"
          aria-describedby="delete-project-description"
        >
          <DialogTitle id="delete-project-title">Delete {projectName}?</DialogTitle>
          <DialogContent>
            <DialogContentText id="delete-project-description">
              This removes the project from the current view. You can undo it afterward.
            </DialogContentText>
          </DialogContent>
          <DialogActions>
            <Button onClick={() => setDeleteDialogOpen(false)}>Cancel</Button>
            <Button color="error" onClick={confirmDelete} autoFocus>
              Delete project
            </Button>
          </DialogActions>
        </Dialog>
      </Container>
    </ThemeProvider>
  );
}
```

## `src/main.jsx`

Mount the application with React StrictMode:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

## Keyboard walkthrough

1. Press Tab from the browser address bar to reach the skip link. Press Enter to move to main content.
2. Continue with Tab to the favorite control. Its accessible name identifies the action, and its pressed state changes when activated.
3. Activate Show project details. The expanded state changes, and the controlled details become visible.
4. Move to the form field, edit the value, and submit with Enter. The status region announces the result.
5. Activate Delete project, then press Escape to close the dialog or choose Delete project to confirm.
6. After deletion, use Undo remove to restore the project.

## How the example works

### Names and descriptions

The TextField uses a persistent label. The IconButton has an aria-label, while Tooltip uses describeChild to add information without replacing that name.

### State

The favorite control exposes aria-pressed. The disclosure button exposes aria-expanded and points to its content through aria-controls. Both attributes follow React state rather than styling alone.

### Form feedback

The form uses noValidate so its submit handler can show a connected helper message. The field remains labelled and marked required. The message explains what value is needed.

### Status announcement

The status region uses a polite live announcement. Saving and deletion feedback updates without forcing focus away from the current control.

### Dialog focus

Dialog is connected to a title and description. Material UI manages focus while it is open and restores focus after closing. The confirm action is focused initially, and the cancel or Escape path remains available.

## Try these changes

1. Clear the Project name field and submit. Confirm that the error text explains what to enter.
2. Toggle Favorite project and inspect its pressed state with assistive technology.
3. Open the details section and confirm that aria-expanded matches visibility.
4. Open the delete dialog and close it with Escape. Confirm focus returns to Delete project.
5. Delete the project, then restore it with Undo remove.
6. Zoom the page and verify that the skip link and focus ring remain visible.

## Common mistakes

- Removing the skip link from the keyboard order when it receives focus.
- Replacing the input label with placeholder text.
- Styling the favorite state without exposing aria-pressed.
- Updating disclosure visibility without updating aria-expanded.
- Moving focus to a status message after every save.
- Omitting the dialog title or description relationship.
- Disabling modal focus management without an alternative.
- Announcing the same message repeatedly when nothing changed.
- Relying on color or icon shape alone to communicate a state.

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
      <td><a href="035-typescript-with-material-ui.md">035 - TypeScript with Material UI</a></td>
      <td><a href="../lessons/036-accessibility-and-keyboard-interaction.md">Lesson</a></td>
      <td><a href="../qnas/036-accessibility-and-keyboard-interaction.md">Q&amp;A</a></td>
      <td><a href="037-localization-rtl-and-internationalization.md">037 - Localization, RTL, and Internationalization</a></td>
    </tr>
  </tbody>
</table>
