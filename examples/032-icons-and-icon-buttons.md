# Icons and Icon Buttons: Accessible Project Actions Example

This example builds a compact project card with icon-only controls for favoriting, opening details, and deleting a project. Every icon button has an accessible name. Tooltips add descriptions, and deletion requires confirmation.

## What you will build

- A toggle IconButton with a pressed state.
- An IconButton rendered as a link for navigation.
- A destructive IconButton that opens a confirmation dialog.
- Tooltips that describe controls without replacing their accessible names.
- An undo action after the sample project is removed.

## Requirements

In an existing React application, install Material UI, its default styling engine, and the Material SVG icon package:

```bash
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
```

## `src/App.jsx`

Replace the starter component with this file:

```jsx
import { useState } from "react";
import {
  Alert,
  Box,
  Button,
  Container,
  Dialog,
  DialogActions,
  DialogContent,
  DialogContentText,
  DialogTitle,
  IconButton,
  Paper,
  Stack,
  Tooltip,
  Typography,
} from "@mui/material";
import DeleteOutlineIcon from "@mui/icons-material/DeleteOutline";
import FavoriteIcon from "@mui/icons-material/Favorite";
import FavoriteBorderIcon from "@mui/icons-material/FavoriteBorder";
import OpenInNewIcon from "@mui/icons-material/OpenInNew";

export default function App() {
  const [favorite, setFavorite] = useState(false);
  const [deleteDialogOpen, setDeleteDialogOpen] = useState(false);
  const [deleted, setDeleted] = useState(false);

  function confirmDelete() {
    setDeleteDialogOpen(false);
    setDeleted(true);
  }

  function restoreProject() {
    setDeleted(false);
  }

  return (
    <Container maxWidth="sm" sx={{ py: 5 }}>
      <Typography component="h1" variant="h4" fontWeight={700} sx={{ mb: 2 }}>
        Project actions
      </Typography>

      {deleted ? (
        <Alert
          severity="success"
          action={
            <Button color="inherit" size="small" onClick={restoreProject}>
              Undo
            </Button>
          }
        >
          Atlas was removed. You can restore it now.
        </Alert>
      ) : (
        <Paper variant="outlined" sx={{ p: 2 }}>
          <Stack
            direction={{ xs: "column", sm: "row" }}
            alignItems={{ xs: "flex-start", sm: "center" }}
            spacing={2}
          >
            <Box sx={{ flexGrow: 1 }}>
              <Typography id="project-name" variant="h6" fontWeight={600}>
                Atlas
              </Typography>
              <Typography color="text.secondary">
                Platform team · Active
              </Typography>
            </Box>

            <Stack direction="row" spacing={0.5} aria-label="Project actions">
              <Tooltip
                describeChild
                title={favorite ? "Remove from favorites" : "Add to favorites"}
              >
                <IconButton
                  aria-label="Favorite project"
                  aria-pressed={favorite}
                  color={favorite ? "primary" : "default"}
                  onClick={() => setFavorite((current) => !current)}
                >
                  {favorite ? <FavoriteIcon /> : <FavoriteBorderIcon />}
                </IconButton>
              </Tooltip>

              <Tooltip describeChild title="Opens the project details">
                <IconButton
                  component="a"
                  href="#project-details"
                  aria-label="Open Atlas project details"
                >
                  <OpenInNewIcon />
                </IconButton>
              </Tooltip>

              <Tooltip describeChild title="Opens a confirmation before deletion">
                <IconButton
                  aria-label="Delete project"
                  color="error"
                  onClick={() => setDeleteDialogOpen(true)}
                >
                  <DeleteOutlineIcon />
                </IconButton>
              </Tooltip>
            </Stack>
          </Stack>
        </Paper>
      )}

      <Dialog
        open={deleteDialogOpen}
        onClose={() => setDeleteDialogOpen(false)}
        aria-labelledby="delete-project-title"
        aria-describedby="delete-project-description"
      >
        <DialogTitle id="delete-project-title">Delete Atlas?</DialogTitle>
        <DialogContent>
          <DialogContentText id="delete-project-description">
            The project will be removed from this sample view. You can undo the
            change after confirming.
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
  );
}
```

The link points to a local fragment for demonstration. Replace it with the application's router link or a valid project path when integrating the card.

## `src/main.jsx`

Use the Vite entry point to mount the application:

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

## How it works

### Favoriting

The favorite IconButton keeps its selected value in React state. aria-pressed announces its toggle state, and the icon changes to match. The button has a stable action name; assistive technology can announce whether it is pressed.

### Navigation

The details control is an anchor rendered through IconButton. It has a destination-oriented accessible name and uses link behavior, rather than pretending that navigation is a button action.

### Destructive action

The delete IconButton opens a dialog. The user must confirm before the sample removes the project. After deletion, an Undo button restores the item. Real applications should connect this state transition to their data layer.

### Tooltips and accessible names

Each button has an aria-label. Tooltip uses describeChild to provide extra information without replacing that name. The tooltip explains how the action behaves, while the button label identifies the action.

## Try these changes

1. Activate Favorite project with a mouse, then with the keyboard. Confirm that its pressed state changes.
2. Focus the details control and inspect its accessible name and link behavior.
3. Open the delete dialog, choose Cancel, and confirm that the card remains.
4. Open the dialog again, confirm deletion, then choose Undo.
5. Remove describeChild from one Tooltip and compare whether it supplies a label or description.
6. Disable a control only when the action is unavailable, and retain an obvious keyboard focus indicator.

## Common mistakes

- Omitting aria-label from an icon-only button.
- Using button semantics for navigation.
- Relying on a tooltip as the only explanation of a high-impact action.
- Using aria-pressed without synchronizing it with state.
- Communicating a toggle state only through color or a different glyph.
- Removing focus styling when disabling ripple effects.
- Skipping confirmation and recovery for destructive actions.

## Chapter navigation

<table>
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
      <td><a href="031-mui-x-data-grid-community.md">031 - MUI X Data Grid Community</a></td>
      <td><a href="../lessons/032-icons-and-icon-buttons.md">Lesson</a></td>
      <td><a href="../qnas/032-icons-and-icon-buttons.md">Q&amp;A</a></td>
      <td><a href="033-date-and-time-pickers.md">033 - Date and Time Pickers</a></td>
    </tr>
  </tbody>
</table>
