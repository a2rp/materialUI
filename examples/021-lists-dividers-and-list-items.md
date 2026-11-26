# Lists, Dividers, and List Items - Example

## Goal

Build an accessible mail-style interface with a folder navigation list, a nested project list, selectable messages, secondary star actions, and dividers between groups.

## Prerequisites

- An existing React application.
- Material UI and its default Emotion styling packages.
- The Material UI icon package.

From the project folder, install the packages if they are not already present:

```sh
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
```

## File structure

Use the component in the existing application:

```text
src/
5�@ App.jsx
```

## Example: mail folders and messages

Save this as `App.jsx` and render `<App />` from the application's existing entry point.

```jsx
import { Fragment, useState } from 'react';
import ArchiveOutlinedIcon from '@mui/icons-material/ArchiveOutlined';
import ChevronRightIcon from '@mui/icons-material/ChevronRight';
import ExpandMoreIcon from '@mui/icons-material/ExpandMore';
import DraftsOutlinedIcon from '@mui/icons-material/DraftsOutlined';
import FolderOutlinedIcon from '@mui/icons-material/FolderOutlined';
import InboxOutlinedIcon from '@mui/icons-material/InboxOutlined';
import StarIcon from '@mui/icons-material/Star';
import StarBorderIcon from '@mui/icons-material/StarBorder';
import Box from '@mui/material/Box';
import Container from '@mui/material/Container';
import Divider from '@mui/material/Divider';
import IconButton from '@mui/material/IconButton';
import List from '@mui/material/List';
import ListItem from '@mui/material/ListItem';
import ListItemButton from '@mui/material/ListItemButton';
import ListItemIcon from '@mui/material/ListItemIcon';
import ListItemText from '@mui/material/ListItemText';
import ListSubheader from '@mui/material/ListSubheader';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

const folders = [
  { id: 'Inbox', label: 'Inbox', Icon: InboxOutlinedIcon, count: 3 },
  { id: 'Drafts', label: 'Drafts', Icon: DraftsOutlinedIcon, count: 1 },
  { id: 'Archive', label: 'Archive', Icon: ArchiveOutlinedIcon, count: 2 },
];

const messages = [
  {
    id: 'kickoff',
    folder: 'Inbox',
    sender: 'Maya Chen',
    subject: 'Project kickoff notes',
    preview: 'The agenda and decisions from today are ready to review.',
    time: '9:42 AM',
    unread: true,
  },
  {
    id: 'design',
    folder: 'Inbox',
    sender: 'Noah Patel',
    subject: 'Updated design review',
    preview: 'I added the latest screen flows and accessibility notes.',
    time: 'Yesterday',
    unread: true,
  },
  {
    id: 'release',
    folder: 'Inbox',
    sender: 'Ari Morgan',
    subject: 'Release checklist',
    preview: 'The final checks are grouped by owner in the document.',
    time: 'Monday',
    unread: false,
  },
  {
    id: 'draft',
    folder: 'Drafts',
    sender: 'Me',
    subject: 'Weekly project update',
    preview: 'A draft summary of completed work and next steps.',
    time: 'Today',
    unread: false,
  },
  {
    id: 'archive',
    folder: 'Archive',
    sender: 'Maya Chen',
    subject: 'Previous sprint summary',
    preview: 'A short recap of the last sprint and its follow-up items.',
    time: 'Oct 12',
    unread: false,
  },
];

export default function App() {
  const [selectedFolder, setSelectedFolder] = useState('Inbox');
  const [selectedMessage, setSelectedMessage] = useState('kickoff');
  const [projectsOpen, setProjectsOpen] = useState(true);
  const [starredIds, setStarredIds] = useState(() => new Set());
  const [status, setStatus] = useState('Choose a message to preview it.');

  const visibleMessages = messages.filter(
    (message) => message.folder === selectedFolder,
  );

  function chooseFolder(folderId) {
    setSelectedFolder(folderId);
    const firstMessage = messages.find((message) => message.folder === folderId);
    setSelectedMessage(firstMessage ? firstMessage.id : null);
    setStatus('Showing ' + folderId.toLowerCase() + ' messages.');
  }

  function toggleStar(message) {
    setStarredIds((currentIds) => {
      const nextIds = new Set(currentIds);

      if (nextIds.has(message.id)) {
        nextIds.delete(message.id);
      } else {
        nextIds.add(message.id);
      }

      return nextIds;
    });
  }

  return (
    <Container component="main" maxWidth="lg" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={3}>
        <Box>
          <Typography component="h1" variant="h4" gutterBottom>
            Project mail
          </Typography>
          <Typography color="text.secondary">
            Browse folders and select a message to preview.
          </Typography>
        </Box>

        <Box
          sx={{
            display: 'grid',
            gridTemplateColumns: { xs: '1fr', md: '240px minmax(0, 1fr)' },
            gap: 2,
            alignItems: 'start',
          }}
        >
          <Paper
            component="nav"
            aria-label="Mail folders"
            variant="outlined"
            sx={{ py: 1 }}
          >
            <List
              aria-label="Folders and project lists"
              subheader={
                <ListSubheader component="li" disableSticky>
                  Mailboxes
                </ListSubheader>
              }
            >
              {folders.map(({ id, label, Icon, count }) => (
                <ListItem key={id} disablePadding>
                  <ListItemButton
                    component="button"
                    type="button"
                    selected={selectedFolder === id}
                    onClick={() => chooseFolder(id)}
                  >
                    <ListItemIcon>
                      <Icon fontSize="small" />
                    </ListItemIcon>
                    <ListItemText primary={label} />
                    <Typography
                      component="span"
                      variant="body2"
                      color="text.secondary"
                    >
                      {count}
                    </Typography>
                  </ListItemButton>
                </ListItem>
              ))}

              <Divider component="li" sx={{ my: 1 }} />

              <ListSubheader component="li" disableSticky>
                Collections
              </ListSubheader>

              <ListItem disablePadding sx={{ display: 'block' }}>
                <ListItemButton
                  component="button"
                  type="button"
                  aria-expanded={projectsOpen}
                  onClick={() => setProjectsOpen((open) => !open)}
                >
                  <ListItemIcon>
                    {projectsOpen ? (
                      <ExpandMoreIcon fontSize="small" />
                    ) : (
                      <ChevronRightIcon fontSize="small" />
                    )}
                  </ListItemIcon>
                  <ListItemText primary="Projects" />
                </ListItemButton>

                {projectsOpen && (
                  <List
                    component="ul"
                    aria-label="Project folders"
                    disablePadding
                    sx={{ pl: 3 }}
                  >
                    {['Website refresh', 'Research'].map((project) => (
                      <ListItem key={project} disablePadding>
                        <ListItemButton
                          component="button"
                          type="button"
                          onClick={() =>
                            setStatus('Project selected: ' + project + '.')
                          }
                        >
                          <ListItemIcon>
                            <FolderOutlinedIcon fontSize="small" />
                          </ListItemIcon>
                          <ListItemText primary={project} />
                        </ListItemButton>
                      </ListItem>
                    ))}
                  </List>
                )}
              </ListItem>
            </List>
          </Paper>

          <Paper
            component="section"
            aria-labelledby="message-list-heading"
            variant="outlined"
            sx={{ minWidth: 0, overflow: 'hidden' }}
          >
            <Stack
              direction="row"
              justifyContent="space-between"
              alignItems="baseline"
              sx={{ px: 2, pt: 2 }}
            >
              <Typography id="message-list-heading" component="h2" variant="h6">
                {selectedFolder}
              </Typography>
              <Typography variant="body2" color="text.secondary">
                {visibleMessages.length} messages
              </Typography>
            </Stack>

            <List dense aria-label={selectedFolder + ' messages'} disablePadding>
              {visibleMessages.map((message, index) => {
                const isStarred = starredIds.has(message.id);

                return (
                  <Fragment key={message.id}>
                    <ListItem
                      disablePadding
                      secondaryAction={
                        <IconButton
                          edge="end"
                          aria-label={
                            (isStarred ? 'Remove star from ' : 'Star ') +
                            message.subject
                          }
                          aria-pressed={isStarred}
                          onClick={() => toggleStar(message)}
                        >
                          {isStarred ? (
                            <StarIcon color="warning" />
                          ) : (
                            <StarBorderIcon />
                          )}
                        </IconButton>
                      }
                    >
                      <ListItemButton
                        component="button"
                        type="button"
                        selected={selectedMessage === message.id}
                        onClick={() => {
                          setSelectedMessage(message.id);
                          setStatus('Previewing: ' + message.subject);
                        }}
                        sx={{ textAlign: 'left', py: 1.5 }}
                      >
                        <ListItemText
                          primary={
                            <Typography
                              component="span"
                              variant="body2"
                              fontWeight={message.unread ? 700 : 400}
                            >
                              {message.sender} - {message.time}
                            </Typography>
                          }
                          secondary={
                            <Stack component="span" spacing={0.25}>
                              <Typography
                                component="span"
                                variant="body2"
                                color="text.primary"
                                fontWeight={message.unread ? 600 : 400}
                                noWrap
                              >
                                {message.subject}
                              </Typography>
                              <Typography
                                component="span"
                                variant="body2"
                                color="text.secondary"
                                noWrap
                              >
                                {message.preview}
                              </Typography>
                            </Stack>
                          }
                        />
                      </ListItemButton>
                    </ListItem>

                    {index < visibleMessages.length - 1 && (
                      <Divider component="li" />
                    )}
                  </Fragment>
                );
              })}
            </List>

            <Divider />

            <Typography
              role="status"
              aria-live="polite"
              color="text.secondary"
              sx={{ px: 2, py: 1.5 }}
            >
              {status}
            </Typography>
          </Paper>
        </Box>
      </Stack>
    </Container>
  );
}
```

## What to notice

- `List` groups related rows. Its default list semantics support assistive technology.
- `ListItem` provides the list-row structure. `ListItemButton` makes a row interactive and keyboard-operable.
- The nested project list sits inside its parent `ListItem`, preserving valid list nesting.
- The `selected` prop communicates the current folder or message visually. The interface also keeps the selected item in React state.
- `secondaryAction` places the star button outside the row button, so the two actions do not become nested interactive controls.
- The star button exposes its state with `aria-pressed` and has a message-specific accessible name.
- `Divider component="li"` keeps a separator inside a list as a list item. The standalone divider separates the message list from the status area.
- `dense` is suitable for compact rows when the surrounding layout still leaves enough room for touch and pointer use.

## Run and inspect

1. Start the existing React development server with `npm run dev`.
2. Resize the page to a narrow viewport and confirm the folder navigation moves above the message panel.
3. Use Tab, Shift+Tab, Enter, and Space to navigate and activate folders, the Projects control, messages, and star buttons.
4. Select a different folder and confirm the list and message count update.
5. Expand and collapse Projects. Check that `aria-expanded` changes with the visible nested list.
6. Star a message, then activate the same button again to remove the star.
7. Inspect the accessibility tree to confirm the navigation, named lists, message section heading, selected buttons, and live status are understandable.

## Variations

1. Add a `ListSubheader` for unread messages and another for earlier messages.
2. Add an avatar with `ListItemAvatar` for each sender. Give meaningful images useful alternative text.
3. Add a compact mode toggle that changes the `dense` prop.
4. Replace folder selection with links rendered through your application's router. Keep the row's accessible name and focus indicator.
5. Add a contextual menu as a separate secondary action. Do not place a button inside another button.

## Common mistakes

- Putting a button or link inside `ListItemButton` creates nested interactive controls. Keep secondary actions as siblings through `secondaryAction`.
- A divider inside a list should use `component="li"` to preserve valid list markup.
- A selected style alone does not manage application state. Store the selected item and update it in event handlers.
- Icon-only actions need an accessible name that describes the affected item and action.
- Do not add `role="menu"` to an ordinary navigation list. Menus have different keyboard behavior and should represent an actual menu interaction.

## Practice

Add a list of notifications with a primary action and a separate dismiss button. Keep the controls independently keyboard-operable and announce a useful status after a notification is dismissed.

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
      <td><a href="020-paper-cards-and-accordions.md">020 - Paper, cards, and accordions</a></td>
      <td><a href="../lessons/021-lists-dividers-and-list-items.md">Lesson</a></td>
      <td><a href="../qnas/021-lists-dividers-and-list-items.md">Q&amp;A</a></td>
      <td><a href="022-appbar-toolbar-and-top-navigation.md">022 - AppBar, Toolbar, and top navigation</a></td>
    </tr>
  </tbody>
</table>
