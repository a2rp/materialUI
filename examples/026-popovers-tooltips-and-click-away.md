# Popovers, Tooltips, and Click-Away - Example

## Goal

Build a small work-filter panel that uses a Tooltip for help, a modal Popover for filter controls, and a non-modal Popper for a quick note that closes on click-away.

## Prerequisites

- An existing React application.
- Material UI and its default Emotion styling packages.
- The Material UI icon package.

Install the packages if needed:

```sh
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
```

## File structure

Add the component to the existing application:

```text
src/
��y��yۧu���@�w^~)�t App.jsx
```

## Example: work filters and quick note

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import InfoOutlinedIcon from '@mui/icons-material/InfoOutlined';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Checkbox from '@mui/material/Checkbox';
import ClickAwayListener from '@mui/material/ClickAwayListener';
import Container from '@mui/material/Container';
import FormControlLabel from '@mui/material/FormControlLabel';
import FormGroup from '@mui/material/FormGroup';
import IconButton from '@mui/material/IconButton';
import Paper from '@mui/material/Paper';
import Popover from '@mui/material/Popover';
import Popper from '@mui/material/Popper';
import Stack from '@mui/material/Stack';
import Tooltip from '@mui/material/Tooltip';
import Typography from '@mui/material/Typography';

const availableFilters = [
  { id: 'assigned', label: 'Assigned to me' },
  { id: 'review', label: 'Needs review' },
];

export default function App() {
  const [filterAnchor, setFilterAnchor] = useState(null);
  const [quickNoteAnchor, setQuickNoteAnchor] = useState(null);
  const [selectedFilters, setSelectedFilters] = useState([]);
  const [status, setStatus] = useState('No filters have been applied.');

  const filterOpen = Boolean(filterAnchor);
  const quickNoteOpen = Boolean(quickNoteAnchor);

  function closeFilterPopover() {
    setFilterAnchor(null);
  }

  function toggleFilter(filterId) {
    setSelectedFilters((currentFilters) =>
      currentFilters.includes(filterId)
        ? currentFilters.filter((item) => item !== filterId)
        : [...currentFilters, filterId],
    );
  }

  function applyFilters() {
    const labels = availableFilters
      .filter((filter) => selectedFilters.includes(filter.id))
      .map((filter) => filter.label);

    setStatus(
      labels.length > 0
        ? 'Applied filters: ' + labels.join(', ') + '.'
        : 'Filters cleared.',
    );
    closeFilterPopover();
  }

  function toggleQuickNote(event) {
    const trigger = event.currentTarget;
    setQuickNoteAnchor((currentAnchor) =>
      currentAnchor ? null : trigger,
    );
  }

  function closeQuickNote() {
    setQuickNoteAnchor(null);
  }

  return (
    <Container component="main" maxWidth="md" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={3}>
        <Box>
          <Typography component="h1" variant="h4" gutterBottom>
            Work items
          </Typography>
          <Typography color="text.secondary">
            Filter the list or open a short contextual note.
          </Typography>
        </Box>

        <Stack direction="row" spacing={1} alignItems="center" flexWrap="wrap">
          <Button
            aria-controls={filterOpen ? 'work-filter-popover' : undefined}
            aria-haspopup="dialog"
            aria-expanded={filterOpen ? 'true' : undefined}
            onClick={(event) => setFilterAnchor(event.currentTarget)}
          >
            Filter
          </Button>

          <Tooltip
            describeChild
            title="Choose filters to narrow the work items shown."
          >
            <IconButton aria-label="About work item filters">
              <InfoOutlinedIcon />
            </IconButton>
          </Tooltip>

          <ClickAwayListener onClickAway={closeQuickNote}>
            <Box>
              <Button
                aria-controls={quickNoteOpen ? 'quick-note' : undefined}
                aria-expanded={quickNoteOpen ? 'true' : undefined}
                aria-describedby={quickNoteOpen ? 'quick-note' : undefined}
                onClick={toggleQuickNote}
              >
                Quick note
              </Button>

              <Popper
                open={quickNoteOpen}
                anchorEl={quickNoteAnchor}
                placement="bottom-start"
              >
                <Paper
                  id="quick-note"
                  role="region"
                  aria-label="Quick note"
                  elevation={4}
                  sx={{ mt: 1, maxWidth: 280, p: 2 }}
                >
                  <Typography variant="body2">
                    Click outside this note or activate Quick note again to close it.
                  </Typography>
                </Paper>
              </Popper>
            </Box>
          </ClickAwayListener>
        </Stack>

        <Popover
          id="work-filter-popover"
          open={filterOpen}
          anchorEl={filterAnchor}
          onClose={closeFilterPopover}
          anchorOrigin={{ vertical: 'bottom', horizontal: 'left' }}
          transformOrigin={{ vertical: 'top', horizontal: 'left' }}
        >
          <Box
            role="dialog"
            aria-labelledby="work-filter-title"
            aria-describedby="work-filter-description"
            sx={{ width: 300, maxWidth: 'calc(100vw - 32px)', p: 2 }}
          >
            <Stack spacing={2}>
              <Box>
                <Typography id="work-filter-title" component="h2" variant="h6">
                  Filter work
                </Typography>
                <Typography
                  id="work-filter-description"
                  variant="body2"
                  color="text.secondary"
                >
                  Choose one or more filters, then apply them.
                </Typography>
              </Box>

              <FormGroup>
                {availableFilters.map((filter) => (
                  <FormControlLabel
                    key={filter.id}
                    control={
                      <Checkbox
                        checked={selectedFilters.includes(filter.id)}
                        onChange={() => toggleFilter(filter.id)}
                      />
                    }
                    label={filter.label}
                  />
                ))}
              </FormGroup>

              <Stack direction="row" spacing={1} justifyContent="flex-end">
                <Button onClick={closeFilterPopover}>Cancel</Button>
                <Button variant="contained" onClick={applyFilters}>
                  Apply
                </Button>
              </Stack>
            </Stack>
          </Box>
        </Popover>

        <Paper variant="outlined" sx={{ p: 2 }}>
          <Typography role="status" aria-live="polite" color="text.secondary">
            {status}
          </Typography>
        </Paper>
      </Stack>
    </Container>
  );
}
```

## How it works

- The Tooltip supplements the Help icon button's accessible name with a short description through `describeChild`.
- The Popover stores the filter trigger element in `filterAnchor`. It uses Modal behavior and closes through its built-in Escape and backdrop handling.
- Filter choices remain in React state while the Popover is open. Apply updates the status and closes the Popover; Cancel closes it without applying a new result.
- The Popper is non-modal. ClickAwayListener wraps its trigger and detects outside interaction, including the portaled panel in the same React tree.
- The quick note has no interactive content, so it is exposed as a labelled region rather than a menu or dialog.
- The status message announces the result of applying or clearing the filters.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. Focus the Help icon with the keyboard and confirm that the Tooltip appears with its description.
3. Open Filter and use the checkboxes, Apply, Cancel, Escape, and backdrop to try each interaction.
4. Confirm that applying filters updates the status message.
5. Open Quick note. Click inside it and confirm it stays open, then click outside and confirm it closes.
6. Activate Quick note again to close it, and verify that its expanded state updates.
7. Resize the viewport and confirm the Popover remains within the screen edges.

## Variations

1. Replace the quick note with a custom search suggestion list and add deliberate keyboard navigation.
2. Use Popper for the filter controls if the rest of the page should remain interactive while they are open. Add focus and dismissal behavior appropriate to that interaction.
3. Add an arrow Tooltip for the Help control.
4. Add a selected-filter summary that can be reviewed without opening the Popover.
5. Try the Popover near each viewport edge and adjust its anchor origins if the placement feels awkward.

## Common mistakes

- Using Tooltip for buttons or other interactive popup content.
- Assuming Popper has Popover's modal focus, backdrop, and scroll behavior.
- Forgetting to close a custom Popper when the user clicks outside.
- Using tooltip text as the only accessible name for an icon button.
- Wrapping multiple sibling elements directly in ClickAwayListener instead of one ref-capable child.
- Assuming a portaled popup is outside the React tree. Test portal behavior before changing `disableReactTree`.
- Treating this local filter example as connected to a data source. Add the actual filtering logic when integrating it.

## Practice

Add a "Created by me" filter and show the active filters as chips below the controls. Let users remove an applied filter without reopening the Popover, and announce each update.

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
      <td><a href="025-dialogs-and-modal-workflows.md">025 - Dialogs and modal workflows</a></td>
      <td><a href="../lessons/026-popovers-tooltips-and-click-away.md">Lesson</a></td>
      <td><a href="../qnas/026-popovers-tooltips-and-click-away.md">Q&amp;A</a></td>
      <td><a href="027-alerts-snackbars-and-feedback.md">027 - Alerts, snackbars, and feedback</a></td>
    </tr>
  </tbody>
</table>
