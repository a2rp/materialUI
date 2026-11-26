# Text Fields and Input Components

## Learning goals

After this chapter, you should be able to:

- Use TextField for common labelled text input patterns.
- Choose between controlled and uncontrolled values.
- Provide labels, helper text, and error messages.
- Distinguish the native HTML input from the Material UI input component.
- Use multiline fields and common input attributes.
- Recognize when the lower-level form components are needed.

## TextField as a convenience component

TextField combines the common pieces of a form control: FormControl, InputLabel, an input variant, and FormHelperText. It is a convenient starting point for most text fields.

```jsx
import TextField from '@mui/material/TextField';

<TextField
  id="email"
  label="Email address"
  type="email"
  helperText="Use the address associated with your account."
  fullWidth
/>
```

TextField supports three visual variants:

- \`outlined\`: the default bordered style.
- \`filled\`: a filled background style.
- \`standard\`: an underline style that remains supported.

Choose a consistent variant for related fields. The visual variant should not change the field's label, value, or behavior.

## Labels, IDs, and helper text

Give each field a clear label. Provide a stable unique \`id\` so the label and helper text can be connected to the input, including before client rendering completes.

```jsx
<TextField
  id="display-name"
  name="displayName"
  label="Display name"
  helperText="This name is visible to your teammates."
/>
```

A placeholder can show an example format, but it does not replace the label. Labels remain visible after users enter a value and provide the purpose of the field.

Use \`name\` when the value participates in form submission or a form library. Choose names that are stable and meaningful.

## Controlled and uncontrolled fields

A controlled field receives its value from React state and reports changes through \`onChange\`:

```jsx
import { useState } from 'react';
import TextField from '@mui/material/TextField';

function DisplayNameField() {
  const [displayName, setDisplayName] = useState('');

  return (
    <TextField
      id="display-name"
      name="displayName"
      label="Display name"
      value={displayName}
      onChange={(event) => setDisplayName(event.target.value)}
    />
  );
}
```

The input value is read from \`event.target.value\`. Keep the value type consistent across renders; for an empty text field, initialize the state to an empty string instead of switching between \`undefined\` and a string.

An uncontrolled field stores its current value in the DOM. Use \`defaultValue\` for an initial value when React does not need to update it on each keystroke:

```jsx
<TextField
  id="team-name"
  name="teamName"
  label="Team name"
  defaultValue="Design"
/>
```

Use \`value\` for controlled input and \`defaultValue\` for uncontrolled input. Do not pass both for the same field.

## Input types and native attributes

The \`type\` prop selects a native input type, such as \`email\`, \`password\`, \`search\`, or \`date\`:

```jsx
<TextField
  id="account-email"
  label="Email address"
  type="email"
  autoComplete="email"
/>
```

Native HTML attributes are passed through \`slotProps.htmlInput\`:

```jsx
<TextField
  id="quantity"
  label="Quantity"
  type="number"
  slotProps={{
    htmlInput: {
      min: 1,
      max: 20,
      step: 1,
    },
  }}
/>
```

Use a number input only when increment and decrement controls fit the task. Numeric inputs can permit signs, decimal points, or other values that may not match an application's rules. For identifiers such as postal codes, phone numbers, or account numbers, use a text input with appropriate input guidance instead of \`type="number"\`.

The \`inputMode\` attribute can hint which virtual keyboard is helpful on touch devices. It does not validate or convert the entered value.

## Native input slot versus Material UI input slot

TextField exposes multiple slots:

- \`slotProps.htmlInput\` passes attributes to the native \`<input>\` or \`<textarea>\`.
- \`slotProps.input\` passes props to the Material UI Input, FilledInput, or OutlinedInput component.
- \`slotProps.inputLabel\` configures the InputLabel component.
- \`slotProps.formHelperText\` configures the FormHelperText component.

```jsx
<TextField
  id="meeting-time"
  label="Meeting time"
  type="time"
  slotProps={{
    inputLabel: { shrink: true },
    htmlInput: { step: 300 },
  }}
/>
```

These slots control different layers. Use the native HTML slot for native input attributes such as \`min\`, \`max\`, \`step\`, \`autoComplete\`, and \`inputMode\`. Use the MUI input slot when configuring the input component itself, such as its adornments.

## Multiline fields

Set \`multiline\` to render an expanding text area. Use \`minRows\` and \`maxRows\` to bound its height:

```jsx
<TextField
  id="project-description"
  label="Project description"
  multiline
  minRows={3}
  maxRows={8}
  fullWidth
/>
```

Use a single-line input for concise values and a multiline field for longer content. If the field has a known fixed height, the \`rows\` prop can set its row count.

## Error and required states

Use the \`error\` prop to apply an error state and provide a message that explains how to correct the value:

```jsx
<TextField
  id="work-email"
  label="Work email"
  type="email"
  error={emailError}
  helperText={emailError ? 'Enter a valid work email address.' : 'We use this for account updates.'}
/>
```

The visual error state alone does not describe what is wrong. Pair it with clear text and validate the value in the application. The browser's \`required\` attribute can express that a value is required, but applications still need appropriate submission and error handling.

```jsx
<TextField
  id="organization"
  label="Organization"
  required
  helperText="Required"
/>
```

Keep helper text concise. It should explain the format, requirement, or error in a way users can act on.

## Full width, size, disabled, and read-only

- \`fullWidth\` makes the field use the width of its containing block.
- \`size="small"\` provides a more compact control than \`medium\`.
- \`disabled\` prevents interaction and removes the field from normal form submission.
- \`slotProps.htmlInput.readOnly\` makes the value uneditable while remaining focusable.

A disabled and a read-only field have different meanings. Use disabled when the control is unavailable. Use read-only when users need to inspect and copy a value but must not change it.

## Input adornments

Use InputAdornment to add context or an action at the start or end of a field. It is passed through the MUI input slot:

```jsx
import InputAdornment from '@mui/material/InputAdornment';
import TextField from '@mui/material/TextField';

<TextField
  id="budget"
  label="Monthly budget"
  slotProps={{
    input: {
      startAdornment: (
        <InputAdornment position="start">$</InputAdornment>
      ),
    },
  }}
/>
```

An adornment should clarify the value or provide a meaningful action. Do not use it to replace the label, and make interactive adornments keyboard accessible.

## When to use the lower-level components

TextField covers the common field pattern. Use FormControl, InputLabel, an input component, and FormHelperText directly when the composition needs greater control than TextField exposes.

Common input components include:

- \`Input\`: the standard underline input.
- \`FilledInput\`: an input with a filled surface.
- \`OutlinedInput\`: an input with an outline.
- \`InputBase\`: a lower-level input foundation used when designing a custom field.

When composing lower-level pieces, preserve the label association, error announcement, focus behavior, and helper text relationship yourself.

## Common mistakes

- Using placeholder text as the only label.
- Reusing the same \`id\` for multiple fields.
- Setting both \`value\` and \`defaultValue\`.
- Passing a native attribute to \`slotProps.input\` instead of \`slotProps.htmlInput\`.
- Using \`type="number"\` for identifiers that may contain leading zeros.
- Showing an error color without a useful error message.
- Treating disabled and read-only as the same state.
- Using a helper message that is not connected to a stable field ID in custom compositions.
- Assuming TextField covers every advanced input composition.

## Recap

TextField is a convenience wrapper for common form fields. Provide a visible label, a stable ID, and useful helper or error text. Keep controlled and uncontrolled patterns separate. Use \`slotProps.htmlInput\` for native attributes and \`slotProps.input\` for the Material UI input component. Compose the lower-level pieces when a specialized field needs more control.

## Practice task

Build a profile form with a controlled display name, an uncontrolled note field, an email field with helper text, and a multiline description. Add a required state and an error message. Verify every field has a visible label and unique ID, then inspect the native input attributes.

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
      <td><a href="014-buttons-icon-buttons-and-button-groups.md">014 - Buttons, icon buttons, and button groups</a></td>
      <td><a href="../qnas/015-text-fields-and-input-components.md">Q&amp;A</a></td>
      <td><a href="../examples/015-text-fields-and-input-components.md">Example</a></td>
      <td><a href="016-selects-menus-and-native-selects.md">016 - Selects, menus, and native selects</a></td>
    </tr>
  </tbody>
</table>
