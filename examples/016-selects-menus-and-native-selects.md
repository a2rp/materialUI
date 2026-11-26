# Selects, Menus, and Native Selects - Example

## Goal

Build a preferences screen with a custom single Select, a native time-zone select, a multi-select field, and an account action menu.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Familiarity with React state.
- The Select, NativeSelect, and Menu concepts from the matching lesson.

## Example: notification preferences

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import FormControl from '@mui/material/FormControl';
import FormHelperText from '@mui/material/FormHelperText';
import InputLabel from '@mui/material/InputLabel';
import Menu from '@mui/material/Menu';
import MenuItem from '@mui/material/MenuItem';
import NativeSelect from '@mui/material/NativeSelect';
import Stack from '@mui/material/Stack';
import TextField from '@mui/material/TextField';
import Typography from '@mui/material/Typography';
import Select from '@mui/material/Select';

const notificationOptions = [
  { value: 'product', label: 'Product updates' },
  { value: 'security', label: 'Security alerts' },
  { value: 'billing', label: 'Billing notices' },
];

export default function App() {
  const [region, setRegion] = useState('');
  const [timezone, setTimezone] = useState('utc');
  const [notifications, setNotifications] = useState(['security']);
  const [menuAnchor, setMenuAnchor] = useState(null);
  const [message, setMessage] = useState('');

  const menuOpen = Boolean(menuAnchor);

  function handleNotificationsChange(event) {
    const nextValue = event.target.value;
    setNotifications(
      typeof nextValue === 'string' ? nextValue.split(',') : nextValue
    );
  }

  function closeMenu() {
    setMenuAnchor(null);
  }

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={3}>
        <Stack
          direction={{ xs: 'column', sm: 'row' }}
          spacing={2}
          sx={{
            alignItems: { xs: 'stretch', sm: 'center' },
            justifyContent: 'space-between',
          }}
        >
          <Box component="header">
            <Typography variant="h1" component="h1">
              Preferences
            </Typography>
            <Typography color="text.secondary">
              Choose your region, time zone, and notifications.
            </Typography>
          </Box>

          <Button
            aria-haspopup="true"
            aria-expanded={menuOpen ? 'true' : undefined}
            aria-controls={menuOpen ? 'preferences-menu' : undefined}
            onClick={(event) => setMenuAnchor(event.currentTarget)}
          >
            Account actions
          </Button>

          <Menu
            id="preferences-menu"
            anchorEl={menuAnchor}
            open={menuOpen}
            onClose={closeMenu}
          >
            <MenuItem
              onClick={() => {
                setMessage('Profile action selected.');
                closeMenu();
              }}
            >
              View profile
            </MenuItem>
            <MenuItem
              onClick={() => {
                setMessage('Help action selected.');
                closeMenu();
              }}
            >
              Get help
            </MenuItem>
          </Menu>
        </Stack>

        <Box
          component="form"
          onSubmit={(event) => {
            event.preventDefault();
            setMessage(
              'Preferences saved for ' +
              region +
              ' with ' +
              notifications.length +
              ' notification types.'
            );
          }}
        >
          <Stack spacing={2.5}>
            <TextField
              id="region-select"
              name="region"
              label="Region"
              value={region}
              onChange={(event) => setRegion(event.target.value)}
              helperText="Choose the region used for regional settings."
              select
              fullWidth
            >
              <MenuItem value="north">North</MenuItem>
              <MenuItem value="central">Central</MenuItem>
              <MenuItem value="south">South</MenuItem>
            </TextField>

            <FormControl fullWidth>
              <InputLabel
                variant="standard"
                htmlFor="timezone-select"
              >
                Time zone
              </InputLabel>
              <NativeSelect
                value={timezone}
                onChange={(event) => setTimezone(event.target.value)}
                inputProps={{
                  id: 'timezone-select',
                  name: 'timezone',
                }}
              >
                <option value="utc">UTC</option>
                <option value="europe">Europe</option>
                <option value="pacific">Pacific</option>
              </NativeSelect>
              <FormHelperText>
                This example uses simplified time-zone choices.
              </FormHelperText>
            </FormControl>

            <FormControl fullWidth>
              <InputLabel id="notifications-label">
                Notification types
              </InputLabel>
              <Select
                id="notifications-select"
                labelId="notifications-label"
                label="Notification types"
                name="notifications"
                multiple
                value={notifications}
                onChange={handleNotificationsChange}
                renderValue={(selected) =>
                  selected
                    .map((value) => {
                      const option = notificationOptions.find(
                        (item) => item.value === value
                      );
                      return option ? option.label : value;
                    })
                    .join(', ')
                }
              >
                {notificationOptions.map((option) => (
                  <MenuItem key={option.value} value={option.value}>
                    {option.label}
                  </MenuItem>
                ))}
              </Select>
              <FormHelperText>
                Choose one or more types of messages.
              </FormHelperText>
            </FormControl>

            <Button type="submit" variant="contained">
              Save preferences
            </Button>

            <Typography role="status" aria-live="polite" color="text.secondary">
              {message}
            </Typography>
          </Stack>
        </Box>
      </Stack>
    </Container>
  );
}
```

The preferences and menu actions are local demonstrations. They do not access an account service or persist values after the page is reloaded.

## How the example works

- The Region field uses TextField's `select` mode for a custom popup.
- The Time zone field uses NativeSelect and native `option` elements.
- The Notification types field uses a custom multiple Select with an array value.
- The Account actions button anchors a Menu to the clicked button.
- Choosing a menu item updates a status message and closes the Menu.
- The preference values are controlled by React state.
- Each field has a visible label and stable identifier.

## Expected result

The page shows three preference fields and an Account actions menu. Selecting a region, time zone, or notification types updates the displayed value. Saving updates the status message. The action menu opens below its trigger and closes after an action.

## Verification checklist

1. Confirm the Region field has a visible label and selected value.
2. Change the Time zone using the platform-native control.
3. Choose multiple notification types and confirm they appear in the selected value.
4. Open the Account actions menu with the keyboard and activate an item.
5. Confirm the menu closes and the status message updates.
6. Submit preferences and check that the feedback contains the selected region and count.
7. Verify the NativeSelect label is linked to its native select ID.
8. Resize the viewport and confirm the title and menu trigger do not overflow.

## Practice extensions

1. Replace the sample time-zone options with valid zones used by the application.
2. Add a disabled or unavailable option and explain why it cannot be selected.
3. Add a ListSubheader to group a longer custom Select list.
4. Add helper text for the multi-select and connect it to the control with an accessible description.
5. Compare the custom Select and NativeSelect interaction on a mobile device.
6. Replace the local save handler with a real persistence flow and report errors clearly.

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
      <td><a href="015-text-fields-and-input-components.md">015 - Text fields and input components</a></td>
      <td><a href="../lessons/016-selects-menus-and-native-selects.md">Lesson</a></td>
      <td><a href="../qnas/016-selects-menus-and-native-selects.md">Q&amp;A</a></td>
      <td><a href="017-checkboxes-radio-buttons-and-switches.md">017 - Checkboxes, radio buttons, and switches</a></td>
    </tr>
  </tbody>
</table>
