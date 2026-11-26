# Form Control, Validation, and Accessibility

## Learning goals

After this chapter, you should be able to:

- Group labels, inputs, and helper text into a coherent form control.
- Mark invalid and required fields with useful text feedback.
- Connect controls and descriptions programmatically.
- Validate at the right time and preserve user-entered values.
- Group related choices with meaningful legends.
- Plan keyboard focus and submission feedback.

## FormControl and TextField

TextField is a convenience wrapper for a common input, label, and helper-text pattern. Use FormControl with lower-level pieces when the composition needs more control.

FormControl provides shared state such as disabled, error, focused, required, and variant to its children. It is intended to contain one input:

```jsx
import FormControl from '@mui/material/FormControl';
import FormHelperText from '@mui/material/FormHelperText';
import FormLabel from '@mui/material/FormLabel';
import Input from '@mui/material/Input';

<FormControl error={hasError} required>
  <FormLabel htmlFor="account-name">Account name</FormLabel>
  <Input
    id="account-name"
    name="accountName"
    aria-describedby="account-name-help"
    aria-invalid={hasError ? 'true' : undefined}
  />
  <FormHelperText id="account-name-help">
    {hasError ? 'Enter a name before continuing.' : 'Shown to your team.'}
  </FormHelperText>
</FormControl>
```

A TextField already connects much of this structure for you. Supply a unique \`id\`, visible \`label\`, and helpful \`helperText\`:

```jsx
<TextField
  id="account-name"
  label="Account name"
  helperText="Shown to your team."
/>
```

Avoid placing multiple inputs inside one FormControl. Give each input its own field wrapper and label.

## Labels, descriptions, and IDs

Every input needs an accessible name. Use a visible label whenever possible. A placeholder is only an example or hint and should not be the only label.

For a custom composition:

- Connect a label to the input with matching \`htmlFor\` and \`id\`.
- Connect explanatory or error text with \`aria-describedby\`.
- Use unique IDs when multiple controls appear on the page.
- Give checkbox, radio, and switch controls a visible label or another accessible name.

TextField's \`id\` helps connect its label and helper text. For custom controls, confirm the actual relationships rather than assuming that nearby text is associated automatically.

## Required fields

The \`required\` prop communicates that a field is required and can enable browser constraint validation. It does not replace application validation or server-side validation.

Show users which fields are required and what format is expected. Do not mark fields required if the product does not require them. If a custom submission flow uses \`noValidate\`, implement the necessary validation and feedback yourself.

```jsx
<TextField
  id="contact-email"
  label="Email address"
  type="email"
  autoComplete="email"
  required
  helperText="Required. Use an address you can access."
/>
```

## Error states and messages

Use the \`error\` prop to display an invalid state and show text that explains the problem and the next step:

```jsx
<TextField
  id="contact-email"
  label="Email address"
  type="email"
  value={email}
  onChange={handleEmailChange}
  error={Boolean(emailError)}
  helperText={emailError || 'Use an address you can access.'}
/>
```

Color and an error icon are not enough on their own. Make the message specific, for example, "Enter an email address in the format name@example.com." Do not erase the user's input when validation fails.

When composing fields manually, connect the error or helper message to the input through \`aria-describedby\`. Add \`aria-invalid="true"\` when the current value is invalid. TextField handles its own error presentation, but custom compositions need to preserve these relationships.

## Choose when validation appears

A useful validation flow generally:

1. Accepts the user's input without interrupting normal typing.
2. Validates when a field is blurred, when the form is submitted, or at another predictable point.
3. Shows a concise message beside the field that needs attention.
4. Keeps the submitted values so users can correct them.
5. Clears or updates the error when the value changes to a valid state.
6. Moves focus to the first invalid field after a failed submit when appropriate.

Avoid showing an error before the user has had a chance to enter a value unless the rule is immediately relevant. For larger forms, summarize submission errors and link each summary item to the invalid field.

## Group related fields and choices

Use a fieldset and legend for related controls that form one question, such as a radio group:

```jsx
<FormControl component="fieldset" error={Boolean(deliveryError)}>
  <FormLabel component="legend" id="delivery-label">
    Delivery speed
  </FormLabel>

  <RadioGroup
    name="delivery"
    aria-labelledby="delivery-label"
    value={delivery}
    onChange={(event) => setDelivery(event.target.value)}
  >
    <FormControlLabel value="standard" control={<Radio />} label="Standard" />
    <FormControlLabel value="express" control={<Radio />} label="Express" />
  </RadioGroup>

  <FormHelperText id="delivery-help">
    {deliveryError || 'Choose one delivery option.'}
  </FormHelperText>
</FormControl>
```

The group label describes the question. Each option has its own label. When a group helper or error message is needed, make sure it is available to the relevant controls.

## Error summaries and status messages

After a failed submission, a form can show a summary near the top as well as field-level messages. Use plain language, identify each field, and provide a way to move to it.

```jsx
{submitError && (
  <Typography role="alert" color="error">
    Review the highlighted fields and correct the issues below.
  </Typography>
)}
```

Use an alert for important errors that need immediate announcement. Use a polite status region for successful completion or background updates:

```jsx
<Typography role="status" aria-live="polite">
  {saved ? 'Your changes have been saved.' : ''}
</Typography>
```

Avoid announcing every keystroke as an alert. Validation feedback should not overwhelm users while they are editing.

## Keyboard and focus behavior

All controls should be reachable by keyboard in a sensible order. Preserve the component's visible focus state. Grouped radio buttons should keep their expected arrow-key behavior; custom groups need equivalent keyboard handling.

After a failed submit, consider moving focus to the first invalid field or to an error summary. Do not move focus on every validation update. Ensure users can identify which field has focus and understand how to correct its value.

## Browser validation and server validation

Native HTML attributes such as \`required\`, \`type\`, \`min\`, and \`max\` can provide helpful browser behavior. Client-side checks improve feedback but can be bypassed. Validate important data again on the server and return errors that can be associated with the relevant fields.

Client validation should mirror the actual product rules and should not imply that data has been saved or accepted until the server confirms it.

## Common mistakes

- Using placeholder text as the only field label.
- Showing a red outline with no explanatory message.
- Reusing an ID and connecting a label to the wrong field.
- Putting several inputs inside one FormControl.
- Assuming helper text is associated in a custom composition without IDs or \`aria-describedby\`.
- Validating in a way that interrupts users before they can finish typing.
- Clearing entered values when submission fails.
- Announcing every changing error as an urgent alert.
- Relying on client-side checks as the only validation.
- Removing keyboard focus styles while customizing controls.
- Failing to identify the first invalid field after a large form submission.

## Recap

FormControl shares state among a single control and its label and helper text. TextField handles common patterns; lower-level compositions require explicit labels and descriptions. Use \`error\` with clear feedback, validate at predictable times, preserve user input, maintain keyboard focus, and validate again on the server.

## Practice task

Create a registration form with a required name, email, password, and radio choice. Add field-level messages and a submission summary. On failure, focus the first invalid field; on success, announce a concise status. Verify label associations, keyboard behavior, and server-validation handling in the application's design.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Chapter example</th>
      <th>Next lesson</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="017-checkboxes-radio-buttons-and-switches.md">017 - Checkboxes, radio buttons, and switches</a></td>
      <td><a href="../qnas/018-form-control-validation-and-accessibility.md">Q&amp;A</a></td>
      <td><a href="../examples/018-form-control-validation-and-accessibility.md">Example</a></td>
      <td><a href="019-autocomplete.md">019 - Autocomplete</a></td>
    </tr>
  </tbody>
</table>
