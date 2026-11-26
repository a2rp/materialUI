# Checkboxes, Radio Buttons, and Switches - Example

## Goal

Build a notification preferences form with independent checkbox choices, a select-all checkbox, one radio group, and a single on/off switch.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Familiarity with React state and form submission.
- The checkbox, radio, and switch patterns from the matching lesson.

## Example: notification preferences

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Button from '@mui/material/Button';
import Checkbox from '@mui/material/Checkbox';
import Container from '@mui/material/Container';
import FormControl from '@mui/material/FormControl';
import FormControlLabel from '@mui/material/FormControlLabel';
import FormGroup from '@mui/material/FormGroup';
import FormLabel from '@mui/material/FormLabel';
import Paper from '@mui/material/Paper';
import Radio from '@mui/material/Radio';
import RadioGroup from '@mui/material/RadioGroup';
import Stack from '@mui/material/Stack';
import Switch from '@mui/material/Switch';
import Typography from '@mui/material/Typography';

const initialCategories = {
  product: true,
  security: true,
  billing: false,
};

export default function App() {
  const [categories, setCategories] = useState(initialCategories);
  const [frequency, setFrequency] = useState('weekly');
  const [securityEmail, setSecurityEmail] = useState(true);
  const [message, setMessage] = useState('');

  const values = Object.values(categories);
  const allSelected = values.every(Boolean);
  const someSelected = values.some(Boolean) && !allSelected;

  function handleCategoryChange(event) {
    const { name, checked } = event.target;

    setCategories((current) => ({
      ...current,
      [name]: checked,
    }));
  }

  function handleSelectAll(event) {
    const checked = event.target.checked;

    setCategories((current) =>
      Object.fromEntries(
        Object.keys(current).map((name) => [name, checked])
      )
    );
  }

  function handleSubmit(event) {
    event.preventDefault();

    const selectedCategories = Object.entries(categories)
      .filter((entry) => entry[1])
      .map((entry) => entry[0]);

    setMessage(
      'Saved ' +
      selectedCategories.length +
      ' categories, ' +
      frequency +
      ' summaries, security email ' +
      (securityEmail ? 'on' : 'off') +
      '.'
    );
  }

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Paper
        component="form"
        variant="outlined"
        onSubmit={handleSubmit}
        aria-labelledby="notifications-heading"
        sx={{ p: { xs: 2, sm: 3 } }}
      >
        <Stack spacing={3}>
          <Stack spacing={0.5}>
            <Typography id="notifications-heading" variant="h1" component="h1">
              Notifications
            </Typography>
            <Typography color="text.secondary">
              Choose the updates you want to receive.
            </Typography>
          </Stack>

          <FormControl component="fieldset">
            <FormLabel component="legend" id="category-label">
              Update categories
            </FormLabel>

            <FormControlLabel
              control={
                <Checkbox
                  checked={allSelected}
                  indeterminate={someSelected}
                  onChange={handleSelectAll}
                />
              }
              label="Select all categories"
            />

            <FormGroup aria-labelledby="category-label">
              <FormControlLabel
                control={
                  <Checkbox
                    name="product"
                    checked={categories.product}
                    onChange={handleCategoryChange}
                  />
                }
                label="Product updates"
              />
              <FormControlLabel
                control={
                  <Checkbox
                    name="security"
                    checked={categories.security}
                    onChange={handleCategoryChange}
                  />
                }
                label="Security alerts"
              />
              <FormControlLabel
                control={
                  <Checkbox
                    name="billing"
                    checked={categories.billing}
                    onChange={handleCategoryChange}
                  />
                }
                label="Billing notices"
              />
            </FormGroup>
          </FormControl>

          <FormControl component="fieldset">
            <FormLabel id="frequency-label">
              Summary frequency
            </FormLabel>
            <RadioGroup
              aria-labelledby="frequency-label"
              name="frequency"
              value={frequency}
              onChange={(event) => setFrequency(event.target.value)}
            >
              <FormControlLabel
                value="daily"
                control={<Radio />}
                label="Daily"
              />
              <FormControlLabel
                value="weekly"
                control={<Radio />}
                label="Weekly"
              />
              <FormControlLabel
                value="monthly"
                control={<Radio />}
                label="Monthly"
              />
            </RadioGroup>
          </FormControl>

          <FormGroup>
            <FormControlLabel
              control={
                <Switch
                  checked={securityEmail}
                  onChange={(event) => setSecurityEmail(event.target.checked)}
                  name="securityEmail"
                />
              }
              label="Email me security alerts"
            />
          </FormGroup>

          <Button type="submit" variant="contained">
            Save preferences
          </Button>

          <Typography role="status" aria-live="polite" color="text.secondary">
            {message}
          </Typography>
        </Stack>
      </Paper>
    </Container>
  );
}
```

The form stores its choices in local React state and does not contact a notification service.

## How the selection states work

- Each category checkbox stores an independent boolean value.
- The parent checkbox is checked when every category is checked.
- The parent checkbox is indeterminate when some categories are checked.
- Selecting the parent updates all category values.
- RadioGroup stores one frequency string at a time.
- The switch stores one boolean email preference.
- Submitting the form produces a status message from the current state.

## Expected result

The page initially has Product updates and Security alerts selected. The select-all checkbox appears indeterminate because Billing notices is not selected. Selecting all checks each category. Clearing any category makes the parent checkbox indeterminate. The radio group allows one summary frequency, and the switch changes the security email preference.

## Verification checklist

1. Confirm each checkbox and the switch have a visible descriptive label.
2. Select and clear one category and verify the parent becomes indeterminate.
3. Toggle the parent and confirm that it updates all three categories.
4. Choose each radio option and verify only one can be selected.
5. Toggle the security email switch and confirm the visual state changes.
6. Submit the form and verify the status message reflects current choices.
7. Use only the keyboard to reach and operate all controls.
8. Inspect the group labels and the radio group's accessible name.

## Practice extensions

1. Add a fourth category and confirm the parent state still derives correctly.
2. Add helper text that explains what each notification category contains.
3. Add an error message if the user must select at least one category.
4. Decide whether the switch should save immediately or wait for form submission.
5. Persist the settings using the application's chosen data layer.

## Common mistakes

- Treating a checkbox's boolean checked value like a radio group's selected string.
- Showing a radio or checkbox group without a programmatic group label.
- Using color alone to communicate selection, validation, or disabled state.

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
      <td><a href="016-selects-menus-and-native-selects.md">016 - Selects, menus, and native selects</a></td>
      <td><a href="../lessons/017-checkboxes-radio-buttons-and-switches.md">Lesson</a></td>
      <td><a href="../qnas/017-checkboxes-radio-buttons-and-switches.md">Q&amp;A</a></td>
      <td><a href="018-form-control-validation-and-accessibility.md">018 - Form control, validation, and accessibility</a></td>
    </tr>
  </tbody>
</table>
