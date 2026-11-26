# Drawers and Navigation Patterns - Example

## Goal

Build a responsive settings workspace that uses a temporary navigation Drawer on small screens and a permanent Drawer on wider screens. The navigation updates the selected section and closes the mobile Drawer after a destination is chosen.

## Prerequisites

- An existing React application.
- Material UI and its default Emotion styling packages.
- The Material UI icon package.

Install the packages if needed:

```sh
npm install @mui/material @emotion/react @emotion/styled @mui/icons-material
```

## File structure

Add this component to the existing application:

```text
src/
��y��yۧu���@�w^~)�t App.jsx
```

## Example: responsive settings navigation

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import DashboardOutlinedIcon from '@mui/icons-material/DashboardOutlined';
import MenuIcon from '@mui/icons-material/Menu';
import SettingsOutlinedIcon from '@mui/icons-material/SettingsOutlined';
import WorkOutlineIcon from '@mui/icons-material/WorkOutline';
import AppBar from '@mui/material/AppBar';
import Box from '@mui/material/Box';
import Container from '@mui/material/Container';
import Drawer from '@mui/material/Drawer';
import IconButton from '@mui/material/IconButton';
import List from '@mui/material/List';
import ListItem from '@mui/material/ListItem';
import ListItemButton from '@mui/material/ListItemButton';
import ListItemIcon from '@mui/material/ListItemIcon';
import ListItemText from '@mui/material/ListItemText';
import Toolbar from '@mui/material/Toolbar';
import Typography from '@mui/material/Typography';
import useMediaQuery from '@mui/material/useMediaQuery';
import { useTheme } from '@mui/material/styles';

const DRAWER_WIDTH = 264;

const sections = [
  { id: 'overview', label: 'Overview', Icon: DashboardOutlinedIcon },
  { id: 'projects', label: 'Projects', Icon: WorkOutlineIcon },
  { id: 'preferences', label: 'Preferences', Icon: SettingsOutlinedIcon },
];

export default function App() {
  const theme = useTheme();
  const isDesktop = useMediaQuery(theme.breakpoints.up('md'), { noSsr: true });
  const [mobileOpen, setMobileOpen] = useState(false);
  const [currentSection, setCurrentSection] = useState('overview');

  function closeDrawer() {
    setMobileOpen(false);
  }

  function selectSection(sectionId) {
    setCurrentSection(sectionId);

    if (!isDesktop) {
      closeDrawer();
    }
  }

  return (
    <>
      <AppBar component="header" position="fixed">
        <Toolbar>
          {!isDesktop && (
            <IconButton
              edge="start"
              color="inherit"
              aria-label="Open navigation"
              aria-controls={mobileOpen ? 'settings-navigation' : undefined}
              aria-expanded={mobileOpen ? 'true' : undefined}
              onClick={() => setMobileOpen(true)}
              sx={{ mr: 1 }}
            >
              <MenuIcon />
            </IconButton>
          )}

          <Typography component="div" variant="h6">
            Settings workspace
          </Typography>
        </Toolbar>
      </AppBar>

      <Toolbar aria-hidden="true" />

      <Box sx={{ display: 'flex', minHeight: 'calc(100vh - 64px)' }}>
        <Drawer
          id="settings-navigation"
          anchor="left"
          variant={isDesktop ? 'permanent' : 'temporary'}
          open={isDesktop || mobileOpen}
          onClose={closeDrawer}
          sx={{
            width: isDesktop ? DRAWER_WIDTH : 0,
            flexShrink: 0,
            '& .MuiDrawer-paper': {
              width: DRAWER_WIDTH,
              boxSizing: 'border-box',
              top: isDesktop ? 64 : 56,
              height: isDesktop ? 'calc(100% - 64px)' : 'calc(100% - 56px)',
            },
          }}
        >
          <Box component="nav" aria-label="Settings navigation" sx={{ pt: 1 }}>
            <List>
              {sections.map(({ id, label, Icon }) => (
                <ListItem key={id} disablePadding>
                  <ListItemButton
                    component="a"
                    href={'#' + id}
                    selected={currentSection === id}
                    aria-current={currentSection === id ? 'page' : undefined}
                    onClick={() => selectSection(id)}
                  >
                    <ListItemIcon>
                      <Icon />
                    </ListItemIcon>
                    <ListItemText primary={label} />
                  </ListItemButton>
                </ListItem>
              ))}
            </List>
          </Box>
        </Drawer>

        <Container
          component="main"
          maxWidth="lg"
          sx={{ flexGrow: 1, minWidth: 0, py: { xs: 3, md: 5 } }}
        >
          <Typography component="h1" variant="h4" gutterBottom>
            Workspace settings
          </Typography>
          <Typography color="text.secondary" sx={{ mb: 4 }}>
            Choose a section from the navigation.
          </Typography>

          <Box
            component="section"
            id="overview"
            aria-labelledby="overview-heading"
            sx={{ py: 3, scrollMarginTop: 2 }}
          >
            <Typography id="overview-heading" component="h2" variant="h5">
              Overview
            </Typography>
            <Typography sx={{ mt: 1 }}>
              Review the main workspace settings and current status.
            </Typography>
          </Box>

          <Box
            component="section"
            id="projects"
            aria-labelledby="projects-heading"
            sx={{ py: 3, scrollMarginTop: 2 }}
          >
            <Typography id="projects-heading" component="h2" variant="h5">
              Projects
            </Typography>
            <Typography sx={{ mt: 1 }}>
              Manage project access and workspace organization.
            </Typography>
          </Box>

          <Box
            component="section"
            id="preferences"
            aria-labelledby="preferences-heading"
            sx={{ py: 3, scrollMarginTop: 2 }}
          >
            <Typography id="preferences-heading" component="h2" variant="h5">
              Preferences
            </Typography>
            <Typography sx={{ mt: 1 }}>
              Update personal display and notification preferences.
            </Typography>
          </Box>
        </Container>
      </Box>
    </>
  );
}
```

## How it works

- The AppBar is fixed, so a Toolbar-sized element after it offsets the page content.
- The Drawer variant changes at the theme's `md` breakpoint.
- On wide screens the permanent Drawer stays visible. On smaller screens an AppBar button opens the temporary Drawer.
- `useMediaQuery` uses `noSsr: true` here because this sample targets a client-rendered app. For a server-rendered application, choose the SSR behavior deliberately or use a CSS-driven layout to avoid an initial mismatch.
- The Drawer navigation uses links that target matching page sections. The current destination is styled with `selected` and announced with `aria-current="page"`.
- Selecting a section closes the temporary Drawer while leaving permanent desktop navigation in place.
- The temporary Drawer keeps its built-in backdrop and Escape close behavior.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. At a wide viewport, confirm the navigation remains visible next to the content.
3. At a narrow viewport, open the navigation from the AppBar button.
4. Select a section and confirm the page moves to it and the temporary Drawer closes.
5. Open the mobile Drawer and dismiss it with Escape and by clicking the backdrop.
6. Use Tab and Shift+Tab to check that focus moves through the trigger, navigation links, and page content.
7. Resize across the `md` breakpoint and confirm that the selected section and content remain available.

## Variations

1. Add a fourth settings section and keep the navigation and content section ids aligned.
2. Replace the fixed AppBar with a static AppBar and remove its Toolbar offset.
3. Try a persistent Drawer on wide screens when users need to change the content width as it opens and closes.
4. Add a nested list for related settings, following the list semantics from the previous chapter.
5. For a server-rendered app, configure the media query behavior using the application's rendering setup.

## Common mistakes

- A fixed AppBar without a matching offset can hide the page heading.
- A permanent Drawer should not be presented as a dismissible modal.
- A temporary Drawer should close after navigation and remain dismissible with Escape and the backdrop.
- Changing the selected visual style without updating current-page state can leave the navigation out of sync with the content.
- Do not hide destinations on mobile without providing another way to reach them.
- Do not set `noSsr` blindly in an application that needs server-rendered responsive markup.

## Practice

Add a "Security" section to the settings workspace. Add its link to the Drawer, mark it current when selected, and verify its target and heading work at both wide and narrow viewport sizes.

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
      <td><a href="022-appbar-toolbar-and-top-navigation.md">022 - AppBar, Toolbar, and top navigation</a></td>
      <td><a href="../lessons/023-drawers-and-navigation-patterns.md">Lesson</a></td>
      <td><a href="../qnas/023-drawers-and-navigation-patterns.md">Q&amp;A</a></td>
      <td><a href="024-tabs-and-tab-panels.md">024 - Tabs and tab panels</a></td>
    </tr>
  </tbody>
</table>
