# AppBar, Toolbar, and Top Navigation - Example

## Goal

Build a responsive workspace header with desktop navigation links, a compact mobile navigation menu, an account menu, and page sections that match the navigation destinations.

## Prerequisites

- An existing React application.
- Material UI and its default Emotion styling packages.
- The Material UI icon package.

Install the packages from the project folder if needed:

```sh
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
```

## File structure

Add this component to the existing application:

```text
src/
  App.jsx
```

## Example: responsive workspace header

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import AccountCircleOutlinedIcon from '@mui/icons-material/AccountCircleOutlined';
import MenuIcon from '@mui/icons-material/Menu';
import AppBar from '@mui/material/AppBar';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import IconButton from '@mui/material/IconButton';
import Menu from '@mui/material/Menu';
import MenuItem from '@mui/material/MenuItem';
import Toolbar from '@mui/material/Toolbar';
import Typography from '@mui/material/Typography';

const pages = [
  { id: 'overview', label: 'Overview' },
  { id: 'projects', label: 'Projects' },
  { id: 'activity', label: 'Activity' },
];

export default function App() {
  const [navigationAnchor, setNavigationAnchor] = useState(null);
  const [accountAnchor, setAccountAnchor] = useState(null);
  const [status, setStatus] = useState('Workspace overview is ready.');

  const mobileNavigationOpen = Boolean(navigationAnchor);
  const accountMenuOpen = Boolean(accountAnchor);

  function closeNavigation() {
    setNavigationAnchor(null);
  }

  function closeAccountMenu() {
    setAccountAnchor(null);
  }

  function chooseAccountAction(action) {
    setStatus(action + ' selected. Connect this menu to your application action.');
    closeAccountMenu();
  }

  return (
    <>
      <AppBar component="header" position="static">
        <Toolbar disableGutters>
          <Container
            maxWidth="lg"
            sx={{
              display: 'flex',
              alignItems: 'center',
              gap: 1,
            }}
          >
            <Typography
              component="div"
              variant="h6"
              sx={{ flexGrow: { xs: 1, md: 0 }, mr: { md: 3 } }}
            >
              Northstar
            </Typography>

            <Box
              component="nav"
              aria-label="Primary navigation"
              sx={{ display: { xs: 'none', md: 'flex' }, gap: 0.5, flexGrow: 1 }}
            >
              {pages.map((page) => (
                <Button
                  key={page.id}
                  component="a"
                  href={'#' + page.id}
                  color="inherit"
                >
                  {page.label}
                </Button>
              ))}
            </Box>

            <IconButton
              color="inherit"
              aria-label="Open primary navigation"
              aria-haspopup="true"
              aria-controls={
                mobileNavigationOpen ? 'primary-navigation-menu' : undefined
              }
              aria-expanded={mobileNavigationOpen ? 'true' : undefined}
              onClick={(event) => setNavigationAnchor(event.currentTarget)}
              sx={{ display: { xs: 'inline-flex', md: 'none' } }}
            >
              <MenuIcon />
            </IconButton>

            <Menu
              id="primary-navigation-menu"
              anchorEl={navigationAnchor}
              open={mobileNavigationOpen}
              onClose={closeNavigation}
            >
              {pages.map((page) => (
                <MenuItem
                  key={page.id}
                  component="a"
                  href={'#' + page.id}
                  onClick={closeNavigation}
                >
                  {page.label}
                </MenuItem>
              ))}
            </Menu>

            <Button
              color="inherit"
              startIcon={<AccountCircleOutlinedIcon />}
              aria-haspopup="true"
              aria-controls={accountMenuOpen ? 'account-menu' : undefined}
              aria-expanded={accountMenuOpen ? 'true' : undefined}
              onClick={(event) => setAccountAnchor(event.currentTarget)}
            >
              Account
            </Button>

            <Menu
              id="account-menu"
              anchorEl={accountAnchor}
              open={accountMenuOpen}
              onClose={closeAccountMenu}
            >
              <MenuItem onClick={() => chooseAccountAction('Profile')}>
                Profile
              </MenuItem>
              <MenuItem onClick={() => chooseAccountAction('Preferences')}>
                Preferences
              </MenuItem>
              <MenuItem onClick={() => chooseAccountAction('Sign out')}>
                Sign out
              </MenuItem>
            </Menu>
          </Container>
        </Toolbar>
      </AppBar>

      <Container component="main" maxWidth="lg" sx={{ py: { xs: 3, md: 5 } }}>
        <Typography component="h1" variant="h4" gutterBottom>
          Workspace overview
        </Typography>

        <Typography
          role="status"
          aria-live="polite"
          color="text.secondary"
          sx={{ mb: 4 }}
        >
          {status}
        </Typography>

        <StackedSection id="overview" title="Overview">
          <Typography>
            Review the current workspace status and continue with the main task.
          </Typography>
        </StackedSection>

        <StackedSection id="projects" title="Projects">
          <Typography>
            Project pages can be connected to your application's router.
          </Typography>
        </StackedSection>

        <StackedSection id="activity" title="Activity">
          <Typography>
            Recent updates belong in this section of the workspace.
          </Typography>
        </StackedSection>
      </Container>
    </>
  );
}

function StackedSection({ id, title, children }) {
  return (
    <Box
      component="section"
      id={id}
      aria-labelledby={id + '-heading'}
      sx={{ py: 3, scrollMarginTop: 2 }}
    >
      <Typography id={id + '-heading'} component="h2" variant="h5" gutterBottom>
        {title}
      </Typography>
      {children}
    </Box>
  );
}
```

## How it works

- The AppBar explicitly uses `position="static"`, so it occupies normal page space and does not cover the main content.
- Toolbar provides the header row. The Container aligns its contents with the page sections.
- Desktop navigation is rendered as links in a labelled `nav` landmark.
- At small widths, the desktop links are replaced by a button that opens a Menu. Each menu item points to a matching in-page section.
- The Account button opens a separate menu with separate anchor state. Choosing an account action closes the menu and updates the live status message.
- The `StackedSection` helper keeps each destination id, heading, and content together.
- This example uses local state only. Connect destinations and account actions to your router and application logic.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. At a wide viewport, confirm that the primary links appear in the AppBar.
3. At a narrow viewport, confirm that the links are replaced by the menu button.
4. Use the keyboard to open each menu, move through its items, choose an item, and dismiss the menu.
5. Activate a navigation destination and confirm that the matching heading is brought into view.
6. Use browser zoom and confirm that the header controls remain visible and do not cause horizontal scrolling.
7. Confirm that opening the account menu does not accidentally open the navigation menu.

## Fixed AppBar variation

If the design requires a bar that stays at the viewport top, change its position to `fixed` and add a matching spacer after it:

```jsx
<AppBar component="header" position="fixed">
  <Toolbar>
    <Typography component="div" variant="h6">
      Northstar
    </Typography>
  </Toolbar>
</AppBar>
<Toolbar aria-hidden="true" />
<main>{/* Main page content */}</main>
```

The spacer prevents the page from starting behind the fixed bar. Do not add a second top margin of the same height.

## Variations

1. Add a search field with a visible label and a clear keyboard focus state.
2. Replace the local in-page anchors with links from your router integration.
3. Use `position="sticky"` and inspect how an ancestor's overflow setting affects it.
4. Add a selected-page style to the desktop navigation and expose the current page with an appropriate state.
5. Move infrequent account actions to a profile page instead of expanding the AppBar menu.

## Common mistakes

- A fixed AppBar without an offset can hide the first page heading.
- Hidden desktop links must remain available in the small-screen navigation.
- Each independent popup needs separate open state and a correctly associated anchor.
- An icon-only menu button needs an accessible name.
- Navigation destinations should be links. Reserve menu items for the temporary popup interaction.
- Do not leave menu options as pretend application behavior. Replace the sample status update with real account actions when integrating the component.

## Practice

Add a new "Reports" destination and a matching page section. Make it appear in both desktop and mobile navigation, then verify that the two lists stay in sync when the destinations change.

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
      <td><a href="021-lists-dividers-and-list-items.md">021 - Lists, dividers, and list items</a></td>
      <td><a href="../lessons/022-appbar-toolbar-and-top-navigation.md">Lesson</a></td>
      <td><a href="../qnas/022-appbar-toolbar-and-top-navigation.md">Q&amp;A</a></td>
      <td><a href="023-drawers-and-navigation-patterns.md">023 - Drawers and navigation patterns</a></td>
    </tr>
  </tbody>
</table>
