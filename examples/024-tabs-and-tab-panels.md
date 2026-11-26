# Tabs and Tab Panels - Example

## Goal

Build a controlled account settings page with three tabs and matching content panels. Connect each tab and panel with accessible ids, and keep the panel components mounted while hidden.

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
5�@ App.jsx
```

## Example: account settings tabs

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Box from '@mui/material/Box';
import Container from '@mui/material/Container';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Tab from '@mui/material/Tab';
import Tabs from '@mui/material/Tabs';
import Typography from '@mui/material/Typography';

const panels = [
  {
    value: 'profile',
    label: 'Profile',
    heading: 'Profile details',
    body: 'Manage your name, contact details, and personal information.',
  },
  {
    value: 'security',
    label: 'Security',
    heading: 'Security settings',
    body: 'Review sign-in methods and keep account recovery information current.',
  },
  {
    value: 'notifications',
    label: 'Notifications',
    heading: 'Notification preferences',
    body: 'Choose which workspace updates should be sent to you.',
  },
];

export default function App() {
  const [activeTab, setActiveTab] = useState('profile');

  function handleTabChange(event, nextTab) {
    setActiveTab(nextTab);
  }

  return (
    <Container component="main" maxWidth="md" sx={{ py: { xs: 3, sm: 5 } }}>
      <Typography component="h1" variant="h4" gutterBottom>
        Account settings
      </Typography>
      <Typography color="text.secondary" sx={{ mb: 3 }}>
        Choose a section to review its settings.
      </Typography>

      <Paper variant="outlined">
        <Box sx={{ borderBottom: 1, borderColor: 'divider' }}>
          <Tabs
            value={activeTab}
            onChange={handleTabChange}
            aria-label="Account settings sections"
            variant="scrollable"
            scrollButtons="auto"
            allowScrollButtonsMobile
          >
            {panels.map((panel) => (
              <Tab
                key={panel.value}
                id={'settings-tab-' + panel.value}
                aria-controls={'settings-panel-' + panel.value}
                value={panel.value}
                label={panel.label}
              />
            ))}
          </Tabs>
        </Box>

        {panels.map((panel) => (
          <TabPanel
            key={panel.value}
            value={activeTab}
            panelValue={panel.value}
            id={'settings-panel-' + panel.value}
            labelledBy={'settings-tab-' + panel.value}
          >
            <Stack spacing={1}>
              <Typography component="h2" variant="h6">
                {panel.heading}
              </Typography>
              <Typography color="text.secondary">{panel.body}</Typography>
            </Stack>
          </TabPanel>
        ))}
      </Paper>
    </Container>
  );
}

function TabPanel({ value, panelValue, id, labelledBy, children }) {
  const selected = value === panelValue;

  return (
    <Box
      role="tabpanel"
      id={id}
      aria-labelledby={labelledBy}
      tabIndex={0}
      hidden={!selected}
      sx={{ p: { xs: 2, sm: 3 } }}
    >
      {children}
    </Box>
  );
}
```

## How it works

- The `activeTab` state is the controlled value for `Tabs`.
- Each `Tab` has a stable value and unique id.
- Every tab's `aria-controls` points to its matching panel.
- Every panel's `aria-labelledby` points back to its tab.
- The `hidden` attribute hides inactive panels, but all three panel components stay mounted. This pattern preserves child state, though effects inside hidden panels can still run.
- Tabs uses manual keyboard activation by default. Arrow keys move focus, while Enter or Space selects the focused tab.

## Run and inspect

1. Start the existing development server with `npm run dev`.
2. Click each tab and confirm the matching heading and description are shown.
3. Use Tab to enter the tablist. Use the arrow keys to move focus between tabs.
4. Press Enter or Space to select the focused tab.
5. Inspect the accessibility tree and confirm every tab points to its panel and every panel is labelled by its tab.
6. Reduce the viewport width and confirm the tab row can scroll without clipping its labels.
7. Switch tabs and confirm that any state added to a panel remains available while the panel is hidden.

## Variations

1. Add a short form field to the Profile panel and observe that its value remains when switching tabs.
2. If a panel is expensive or has active effects, conditionally render its children only while selected instead of keeping every panel active.
3. Try `selectionFollowsFocus` and compare how arrow keys affect selection.
4. Change to `orientation="vertical"` and place the tablist beside the panel at wide widths.
5. Add a fourth panel and verify that every tab and panel id remains unique.

## Common mistakes

- Tabs with duplicate values can make the active panel ambiguous.
- Missing or mismatched ids break the accessible tab-to-panel relationship.
- A tab visual indicator does not make unrelated page links into a tab pattern.
- Keeping every panel mounted preserves state but can run hidden effects and consume extra memory.
- Adding custom arrow-key handlers can conflict with the built-in keyboard behavior.
- A scrollable tab row still needs concise labels and a way to reach every tab by keyboard and touch.

## Practice

Add a "Privacy" panel with a stable value and unique ids. Put a small form in it, then decide whether the form should preserve its state when hidden. Verify the keyboard activation behavior and the panel's accessible name.

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
      <td><a href="023-drawers-and-navigation-patterns.md">023 - Drawers and navigation patterns</a></td>
      <td><a href="../lessons/024-tabs-and-tab-panels.md">Lesson</a></td>
      <td><a href="../qnas/024-tabs-and-tab-panels.md">Q&amp;A</a></td>
      <td><a href="025-dialogs-and-modal-workflows.md">025 - Dialogs and modal workflows</a></td>
    </tr>
  </tbody>
</table>
