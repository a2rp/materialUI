# Form Control, Validation, and Accessibility - Example

## Goal

Create a small registration form that shows field-level errors, an error summary, a grouped radio choice, and an accessible success status.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Familiarity with React state, refs, and form submission.
- The form control and accessibility patterns from the matching lesson.

## Example: registration form

Save this as \`App.jsx\` and render \`<App />\` from the existing application entry point.

```jsx
import { useRef, useState } from 'react';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import FormControl from '@mui/material/FormControl';
import FormControlLabel from '@mui/material/FormControlLabel';
import FormLabel from '@mui/material/FormLabel';
import Paper from '@mui/material/Paper';
import Radio from '@mui/material/Radio';
import RadioGroup from '@mui/material/RadioGroup';
import Stack from '@mui/material/Stack';
import TextField from '@mui/material/TextField';
import Typography from '@mui/material/Typography';

const initialValues = {
  fullName: '',
  email: '',
  delivery: '',
};

export default function App() {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});
  const [status, setStatus] = useState('');
  const nameRef = useRef(null);
  const emailRef = useRef(null);

  function handleTextChange(event) {
    const { name, value } = event.target;

    setValues((current) => ({
      ...current,
      [name]: value,
    }));

    setErrors((current) => ({
      ...current,
      [name]: '',
    }));
    setStatus('');
  }

  function handleDeliveryChange(event) {
    const delivery = event.target.value;

    setValues((current) => ({
      ...current,
      delivery,
    }));

    setErrors((current) => ({
      ...current,
      delivery: '',
    }));
    setStatus('');
  }

  function handleSubmit(event) {
    event.preventDefault();

    const nextErrors = {};

    if (!values.fullName.trim()) {
      nextErrors.fullName = 'Enter your full name.';
    }

    if (!values.email.trim()) {
      nextErrors.email = 'Enter your email address.';
    } else if (!values.email.includes('@')) {
      nextErrors.email = 'Enter an email address that includes @.';
    }

    if (!values.delivery) {
      nextErrors.delivery = 'Choose a delivery option.';
    }

    setErrors(nextErrors);
    setStatus('');

    if (Object.keys(nextErrors).length > 0) {
      if (nextErrors.fullName) {
        nameRef.current?.focus();
      } else if (nextErrors.email) {
        emailRef.current?.focus();
      }
      return;
    }

    setStatus('Registration details are ready to submit.');
  }

  const errorMessages = Object.values(errors).filter(Boolean);

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Paper
        component="form"
        variant="outlined"
        noValidate
        onSubmit={handleSubmit}
        aria-labelledby="registration-heading"
        sx={{ p: { xs: 2, sm: 3 } }}
      >
        <Stack spacing={2.5}>
          <Box>
            <Typography id="registration-heading" variant="h1" component="h1">
              Create an account
            </Typography>
            <Typography color="text.secondary">
              Enter your details and choose how you would like to receive updates.
            </Typography>
          </Box>

          {errorMessages.length > 0 && (
            <Box role="alert">
              <Typography variant="subtitle1" component="h2">
                Review these fields:
              </Typography>
              <Box component="ul" sx={{ mt: 0.5, mb: 0 }}>
                {errorMessages.map((error) => (
                  <li key={error}>{error}</li>
                ))}
              </Box>
            </Box>
          )}

          <TextField
            inputRef={nameRef}
            id="full-name"
            name="fullName"
            label="Full name"
            value={values.fullName}
            onChange={handleTextChange}
            error={Boolean(errors.fullName)}
            helperText={errors.fullName || 'Use the name shown on your profile.'}
            autoComplete="name"
            required
            fullWidth
          />

          <TextField
            inputRef={emailRef}
            id="registration-email"
            name="email"
            label="Email address"
            type="email"
            value={values.email}
            onChange={handleTextChange}
            error={Boolean(errors.email)}
            helperText={errors.email || 'Use an address you can access.'}
            autoComplete="email"
            required
            fullWidth
          />

          <FormControl
            component="fieldset"
            error={Boolean(errors.delivery)}
          >
            <FormLabel
              id="delivery-label"
              component="legend"
            >
              Delivery preference
            </FormLabel>
            <RadioGroup
              aria-labelledby="delivery-label"
              aria-describedby="delivery-helper"
              name="delivery"
              value={values.delivery}
              onChange={handleDeliveryChange}
            >
              <FormControlLabel
                value="email"
                control={<Radio />}
                label="Email updates"
              />
              <FormControlLabel
                value="digest"
                control={<Radio />}
                label="Weekly digest"
              />
            </RadioGroup>
            <Typography
              id="delivery-helper"
              variant="caption"
              color={errors.delivery ? 'error' : 'text.secondary'}
            >
              {errors.delivery || 'Choose one option.'}
            </Typography>
          </FormControl>

          <Button type="submit" variant="contained">
            Continue
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

The email check is intentionally minimal for this teaching example. A production registration flow needs product-appropriate client validation, server-side validation, privacy handling, and secure submission.

## How the example works

- The form uses \`noValidate\` so submission reaches the custom validation handler.
- Name and email are controlled TextFields with unique IDs and visible labels.
- Field-level helper text changes to corrective messages when a value is invalid.
- The radio options share one controlled value and a descriptive group label.
- \`aria-describedby\` connects the radio group to its helper or error text.
- An error summary announces problems after submission.
- The first invalid text field receives focus when present.
- A polite status announces that valid details are ready to submit.
- The sample does not send account data to a server.

## Expected result

Submitting an empty form displays a summary and field-level messages. The full name field receives focus first. After entering a name, an email containing \`@\`, and a delivery choice, submitting displays a success status.

## Verification checklist

1. Submit the empty form and confirm all three errors are visible.
2. Confirm focus moves to the first invalid text input.
3. Correct a field and verify its error clears while typing.
4. Select a delivery option and confirm the group has a single selected value.
5. Inspect that the radio group is labelled and described by its helper text.
6. Submit valid sample values and confirm the success status is announced politely.
7. Use a screen reader or accessibility tree to inspect labels and error text.
8. Confirm that the form does not claim data was saved or transmitted.

## Practice extensions

1. Replace the simple email check with the validation logic used by the application.
2. Link each error summary item to the invalid field.
3. Add a password field and describe its requirements before submission.
4. Handle server-side validation responses and map them to field messages.
5. Provide a clear retry path when the server rejects the submission.
6. Test the form with keyboard-only interaction and enlarged text.

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
      <td><a href="017-checkboxes-radio-buttons-and-switches.md">017 - Checkboxes, radio buttons, and switches</a></td>
      <td><a href="../lessons/018-form-control-validation-and-accessibility.md">Lesson</a></td>
      <td><a href="../qnas/018-form-control-validation-and-accessibility.md">Q&amp;A</a></td>
      <td><a href="019-autocomplete.md">019 - Autocomplete</a></td>
    </tr>
  </tbody>
</table>
