# Checkboxes, Radio Buttons, and Switches

## Learning goals

After this chapter, you should be able to:

- Choose a checkbox, radio group, or switch for the selection task.
- Connect each control to a clear label.
- Group related controls with a descriptive heading.
- Control values with React state.
- Use indeterminate checkboxes for a parent with partially selected children.
- Make state and helper messages understandable beyond color alone.

## Choose the control based on the decision

| Control | User decision | Typical use |
| --- | --- | --- |
| Checkbox | Select or clear one independent option; multiple choices can be selected | Categories, consent, or a list of optional settings |
| Radio group | Select exactly one option from a visible set | A small number of mutually exclusive choices |
| Switch | Turn one setting on or off | A single preference that takes effect immediately |

Use checkboxes when multiple options are available. Use radio buttons when users need to choose one option and seeing the choices helps them decide. Use a switch for a single on/off setting whose state is clear from its label.

If a radio list becomes long or can be collapsed, a Select can conserve space. If a switch controls an action that requires an explicit form submission, a checkbox may communicate the pending choice more clearly.

## Labels with FormControlLabel

FormControlLabel connects a visible label to a Checkbox, Radio, or Switch:

```jsx
import Checkbox from '@mui/material/Checkbox';
import FormControlLabel from '@mui/material/FormControlLabel';

<FormControlLabel
  control={<Checkbox />}
  label="Send me product updates"
/>
```

Make the label describe the option or setting. A switch labelled "Notifications" is ambiguous if it could mean several types; a label such as "Email me product updates" describes the effect.

When you cannot use FormControlLabel, give the input an accessible name through an appropriate label association or \`aria-label\` on the input slot.

## Checkbox groups

Use FormGroup to organize related independent options. Controlled values can be stored as a set or object in state:

```jsx
import { useState } from 'react';
import Checkbox from '@mui/material/Checkbox';
import FormControlLabel from '@mui/material/FormControlLabel';
import FormGroup from '@mui/material/FormGroup';

function NotificationOptions() {
  const [options, setOptions] = useState({
    product: true,
    security: true,
    billing: false,
  });

  function handleChange(event) {
    const { name, checked } = event.target;
    setOptions((current) => ({
      ...current,
      [name]: checked,
    }));
  }

  return (
    <FormGroup>
      <FormControlLabel
        control={
          <Checkbox
            name="product"
            checked={options.product}
            onChange={handleChange}
          />
        }
        label="Product updates"
      />
      <FormControlLabel
        control={
          <Checkbox
            name="security"
            checked={options.security}
            onChange={handleChange}
          />
        }
        label="Security alerts"
      />
      <FormControlLabel
        control={
          <Checkbox
            name="billing"
            checked={options.billing}
            onChange={handleChange}
          />
        }
        label="Billing notices"
      />
    </FormGroup>
  );
}
```

Each checkbox represents a separate boolean value. Use a \`name\` so the event handler can update the corresponding choice.

## Parent and child checkboxes

A parent checkbox can represent a group of child choices. Its checked state is true when all children are selected, false when none are selected, and indeterminate when only some are selected:

```jsx
const allSelected = selectedCount === items.length;
const someSelected = selectedCount > 0 && !allSelected;

<Checkbox
  checked={allSelected}
  indeterminate={someSelected}
  onChange={toggleAll}
/>
```

Indeterminate is a visual state for a partially selected group. It does not create a third submitted form value. Keep the actual checked value derived from the child selection state.

## RadioGroup for one choice

Use RadioGroup when exactly one value can be selected from a short visible list. Give the group a name and an accessible label:

```jsx
import { useState } from 'react';
import FormControl from '@mui/material/FormControl';
import FormControlLabel from '@mui/material/FormControlLabel';
import FormLabel from '@mui/material/FormLabel';
import Radio from '@mui/material/Radio';
import RadioGroup from '@mui/material/RadioGroup';

function DeliveryChoice() {
  const [delivery, setDelivery] = useState('standard');

  return (
    <FormControl>
      <FormLabel id="delivery-label">Delivery speed</FormLabel>
      <RadioGroup
        aria-labelledby="delivery-label"
        name="delivery"
        value={delivery}
        onChange={(event) => setDelivery(event.target.value)}
      >
        <FormControlLabel
          value="standard"
          control={<Radio />}
          label="Standard"
        />
        <FormControlLabel
          value="express"
          control={<Radio />}
          label="Express"
        />
        <FormControlLabel
          value="pickup"
          control={<Radio />}
          label="Store pickup"
        />
      </RadioGroup>
    </FormControl>
  );
}
```

RadioGroup provides group-level keyboard behavior. Use \`row\` when the controls should be arranged horizontally and enough space is available. The group should have an appropriate default value when the task expects one option to be selected.

## Switch for a single setting

Switch controls one on/off setting. Use a FormControlLabel so the effect is clear:

```jsx
import Switch from '@mui/material/Switch';
import FormControlLabel from '@mui/material/FormControlLabel';

<FormControlLabel
  control={
    <Switch
      checked={emailEnabled}
      onChange={(event) => setEmailEnabled(event.target.checked)}
    />
  }
  label="Email me security alerts"
/>
```

Make the state and effect understandable from the label. For example, "Email me security alerts" conveys what is enabled when the switch is on.

## Default values and controlled state

Use \`defaultChecked\` when the component should manage its own initial checked value. Use \`checked\` and \`onChange\` when React state controls it:

```jsx
<Checkbox defaultChecked />

<Checkbox
  checked={accepted}
  onChange={(event) => setAccepted(event.target.checked)}
/>
```

Do not mix \`checked\` and \`defaultChecked\` on the same control. A controlled checkbox or switch should always receive a boolean value.

## Required, disabled, and helper information

A required checkbox or radio choice should be clearly identified, and form submission still needs appropriate validation. A disabled control is unavailable and should not be the only place where a requirement is described.

Use explanatory text beside a group when users need to know what their choice means. Do not rely on a red tint, a checkmark, or an icon alone to explain an error or selected state.

## Common mistakes

- Using radio buttons when users may select several independent choices.
- Using several switches for a list of options that would be clearer as checkboxes.
- Giving a single switch an ambiguous label such as "Enabled."
- Omitting a visible label or accessible name.
- Forgetting to label the RadioGroup as a group.
- Using \`defaultChecked\` on a component that is controlled by React state.
- Treating indeterminate as an actual third submitted value.
- Using a long radio list when a Select would be easier to scan.
- Communicating a required choice only through color or a disabled state.

## Recap

Checkboxes represent independent choices, RadioGroup represents one choice among several, and Switch represents one on/off setting. Use labels and group names that explain the decision. Control values with boolean or string state as appropriate, and treat indeterminate as a visual summary of child selections.

## Practice task

Build a notification preferences form with two independent checkboxes, a radio group for delivery frequency, and a switch for a single email setting. Add a parent checkbox that selects all the categories, then verify its unchecked, checked, and indeterminate states.

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
      <td><a href="016-selects-menus-and-native-selects.md">016 - Selects, menus, and native selects</a></td>
      <td><a href="../qnas/017-checkboxes-radio-buttons-and-switches.md">Q&amp;A</a></td>
      <td><a href="../examples/017-checkboxes-radio-buttons-and-switches.md">Example</a></td>
      <td><a href="018-form-control-validation-and-accessibility.md">018 - Form control, validation, and accessibility</a></td>
    </tr>
  </tbody>
</table>
