# Text Fields and Input Components - Example

## Goal

Create a profile form that demonstrates controlled fields, an uncontrolled field, multiline text, native input attributes, and visible validation feedback.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Familiarity with React state and form submission handlers.
- The TextField and input slot concepts from the matching lesson.

## Example: profile form

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import TextField from '@mui/material/TextField';
import Typography from '@mui/material/Typography';

export default function App() {
  const [profile, setProfile] = useState({
    displayName: '',
    email: '',
    phone: '',
  });
  const [emailError, setEmailError] = useState('');
  const [status, setStatus] = useState('');

  function handleChange(event) {
    const { name, value } = event.target;

    setProfile((currentProfile) => ({
      ...currentProfile,
      [name]: value,
    }));

    if (name === 'email') {
      setEmailError('');
    }
  }

  function handleSubmit(event) {
    event.preventDefault();

    if (!profile.email.trim()) {
      setEmailError('Enter your email address.');
      setStatus('');
      return;
    }

    if (!profile.email.includes('@')) {
      setEmailError('Enter an email address that includes @.');
      setStatus('');
      return;
    }

    const formData = new FormData(event.currentTarget);
    const note = String(formData.get('note') || '');

    setEmailError('');
    setStatus(
      'Profile saved for ' +
      (profile.displayName.trim() || 'your account') +
      '. Note length: ' +
      note.length +
      ' characters.'
    );
  }

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Paper
        component="form"
        variant="outlined"
        noValidate
        onSubmit={handleSubmit}
        aria-labelledby="profile-heading"
        sx={{ p: { xs: 2, sm: 3 } }}
      >
        <Stack spacing={2.5}>
          <Typography id="profile-heading" variant="h1" component="h1">
            Edit profile
          </Typography>

          <TextField
            id="display-name"
            name="displayName"
            label="Display name"
            value={profile.displayName}
            onChange={handleChange}
            helperText="This name is visible to your teammates."
            slotProps={{ htmlInput: { maxLength: 80 } }}
            fullWidth
          />

          <TextField
            id="profile-email"
            name="email"
            label="Email address"
            type="email"
            autoComplete="email"
            value={profile.email}
            onChange={handleChange}
            error={Boolean(emailError)}
            helperText={emailError || 'Use the address associated with your account.'}
            required
            fullWidth
          />

          <TextField
            id="profile-phone"
            name="phone"
            label="Phone number"
            type="tel"
            autoComplete="tel"
            value={profile.phone}
            onChange={handleChange}
            helperText="Include a country code if needed."
            fullWidth
          />

          <TextField
            id="profile-bio"
            name="bio"
            label="About you"
            multiline
            minRows={3}
            maxRows={6}
            defaultValue=""
            helperText="A short description for your profile."
            fullWidth
          />

          <TextField
            id="profile-note"
            name="note"
            label="Private note"
            multiline
            minRows={2}
            defaultValue="Visible only to you."
            helperText="This field is uncontrolled in the example."
            fullWidth
          />

          <Button type="submit" variant="contained">
            Save profile
          </Button>

          <Typography role="status" aria-live="polite" color="text.secondary">
            {status}
          </Typography>
        </Stack>
      </Paper>
    </Container>
  );
}
```

The email check is intentionally minimal for this teaching example. Production applications should use validation rules appropriate to the product and validate submitted data on the server as well.

## What the example demonstrates

- The display name, email, and phone fields are controlled by React state.
- The private note is initialized with `defaultValue` and remains uncontrolled.
- The description and note fields use multiline text areas.
- Each TextField has a unique ID and visible label.
- The email field shows an error state and corrective helper text after invalid submission.
- The display name's native input has a maximum character count through `slotProps.htmlInput`.
- The form uses `noValidate` so its custom submit handler can display the example's message.
- A polite status region communicates the successful submission result.

## Expected result

The page shows a labelled profile form. Submitting it with an empty or malformed email displays an error message below the email field. Submitting an email that includes `@` displays a status message with the entered name and note length. No data is sent to a server.

## Verification checklist

1. Confirm each visible field label remains available after typing a value.
2. Submit with an empty email and verify the field enters an error state.
3. Enter an email without `@` and confirm corrective helper text appears.
4. Enter a sample email and submit to see the success status.
5. Type beyond the display name limit and confirm the native input stops accepting additional characters.
6. Confirm the multiline fields grow within their row limits.
7. Inspect the form and confirm the controlled fields use `value` while the private note uses `defaultValue`.
8. Use keyboard navigation to move through the fields and submit button.

## Practice extensions

1. Add validation for the display name and describe the requirement next to the field.
2. Replace the minimal email check with the application's chosen validation approach.
3. Turn the private note into a controlled field and compare the state handling.
4. Add a helper message that describes the maximum length before users reach it.
5. Move the fields into a reusable component while keeping IDs, names, and labels unique.

## Common mistakes

- Using placeholder text as the field's only label.
- Supplying both value and defaultValue to the same input or changing between controlled and uncontrolled modes.
- Passing native input attributes to the wrong TextField slot.

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
      <td><a href="014-buttons-icon-buttons-and-button-groups.md">014 - Buttons, icon buttons, and button groups</a></td>
      <td><a href="../lessons/015-text-fields-and-input-components.md">Lesson</a></td>
      <td><a href="../qnas/015-text-fields-and-input-components.md">Q&amp;A</a></td>
      <td><a href="016-selects-menus-and-native-selects.md">016 - Selects, menus, and native selects</a></td>
    </tr>
  </tbody>
</table>
