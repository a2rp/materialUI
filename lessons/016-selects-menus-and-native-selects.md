# Selects, Menus, and Native Selects

## Learning goals

After this chapter, you should be able to:

- Choose between Select, NativeSelect, and Menu.
- Associate a visible label with a select control.
- Handle a selected value with React state.
- Render a multi-select when users need several values.
- Build a menu that opens from a trigger and closes after an action.
- Choose native controls when platform behavior is useful.

## Select fields and action menus solve different problems

A Select is a form field for choosing a value from a known list. A Menu is a temporary list of commands or destinations opened from another control. They may both display a popup list, but they have different roles.

Use a Select when the chosen option becomes a field value. Use a Menu for actions such as opening a profile page, changing an account command, or invoking a task. Do not use a menu to imitate a form field if Select already fits the interaction.

Material UI provides a custom Select, a native Select, and TextField's select mode. For richer search, async loading, or creatable options, use Autocomplete, covered in its own chapter.

## A custom Select

The custom Select renders a Material UI popup with MenuItem choices. A label needs to be connected to the Select with `labelId`. For the outlined variant, pass the label to the Select as well so the outline and floating label line up:

```jsx
import { useState } from 'react';
import FormControl from '@mui/material/FormControl';
import InputLabel from '@mui/material/InputLabel';
import MenuItem from '@mui/material/MenuItem';
import Select from '@mui/material/Select';

function RegionField() {
  const [region, setRegion] = useState('');

  return (
    <FormControl fullWidth>
      <InputLabel id="region-label">Region</InputLabel>
      <Select
        id="region"
        labelId="region-label"
        label="Region"
        name="region"
        value={region}
        onChange={(event) => setRegion(event.target.value)}
      >
        <MenuItem value="north">North</MenuItem>
        <MenuItem value="central">Central</MenuItem>
        <MenuItem value="south">South</MenuItem>
      </Select>
    </FormControl>
  );
}
```

The selected value is controlled by React state. Give the Select an accessible name through the label association or an appropriate `aria-label`. Add helper text and associate it with the control when users need extra instructions.

## TextField select mode

TextField can wrap a Select field when a simple labelled field is enough:

```jsx
import TextField from '@mui/material/TextField';
import MenuItem from '@mui/material/MenuItem';

<TextField
  id="language"
  name="language"
  label="Language"
  value={language}
  onChange={(event) => setLanguage(event.target.value)}
  select
>
  <MenuItem value="en">English</MenuItem>
  <MenuItem value="fr">French</MenuItem>
  <MenuItem value="es">Spanish</MenuItem>
</TextField>
```

TextField creates the field structure and connects the label. Use the direct FormControl, InputLabel, and Select composition when you need more control over the form elements.

## Select value and controlled state

A controlled Select receives its current value from state. Update the value in `onChange`:

```jsx
const [status, setStatus] = useState('draft');

<Select
  value={status}
  onChange={(event) => setStatus(event.target.value)}
>
  <MenuItem value="draft">Draft</MenuItem>
  <MenuItem value="published">Published</MenuItem>
</Select>
```

Use values that are stable and meaningful in application state. The displayed label can be human-readable while the value is a short identifier.

## Placeholder and empty values

Select does not have a `placeholder` prop like a text input. One option is to include an empty value and customize what the empty state displays:

```jsx
<Select
  value={region}
  displayEmpty
  renderValue={(value) => value || 'Choose a region'}
  inputProps={{ 'aria-label': 'Region' }}
>
  <MenuItem value="" disabled>
    Choose a region
  </MenuItem>
  <MenuItem value="north">North</MenuItem>
  <MenuItem value="south">South</MenuItem>
</Select>
```

A visible InputLabel is usually clearer for a form field. If you use an empty display value, make sure it is understandable and does not look like a selected choice.

## Multiple selection

Set `multiple` to allow more than one choice. The controlled value is an array:

```jsx
const [notifications, setNotifications] = useState([]);

<Select
  multiple
  value={notifications}
  onChange={(event) => setNotifications(event.target.value)}
>
  <MenuItem value="product">Product updates</MenuItem>
  <MenuItem value="security">Security alerts</MenuItem>
  <MenuItem value="billing">Billing notices</MenuItem>
</Select>
```

A multi-select is useful when users need to choose several values from a relatively short list. If the list is long or users need to search, use Autocomplete or a dedicated selection flow.

## Group options

Group related choices with ListSubheader. A native select can use HTML `optgroup`:

```jsx
import ListSubheader from '@mui/material/ListSubheader';

<Select value={value} onChange={handleChange}>
  <ListSubheader>Workspace</ListSubheader>
  <MenuItem value="projects">Projects</MenuItem>
  <MenuItem value="people">People</MenuItem>
  <ListSubheader>Account</ListSubheader>
  <MenuItem value="profile">Profile</MenuItem>
  <MenuItem value="security">Security</MenuItem>
</Select>
```

Keep groups short and make headings useful. Grouping should reduce scanning effort rather than add decoration.

## NativeSelect

NativeSelect renders a platform-native HTML select control. It can offer familiar browser or device behavior, especially on mobile:

```jsx
import FormControl from '@mui/material/FormControl';
import InputLabel from '@mui/material/InputLabel';
import NativeSelect from '@mui/material/NativeSelect';

<FormControl fullWidth>
  <InputLabel htmlFor="timezone">Time zone</InputLabel>
  <NativeSelect
    id="timezone"
    name="timezone"
    defaultValue="utc"
  >
    <option value="utc">UTC</option>
    <option value="europe">Europe</option>
    <option value="pacific">Pacific</option>
  </NativeSelect>
</FormControl>
```

Associate the InputLabel with the native select using the matching `htmlFor` and `id`. Native options use HTML `option` elements rather than MenuItem.

Choose NativeSelect when its simpler platform behavior is useful. Choose the custom Select when the interface needs Material UI's popup rendering and richer composition. Native controls can look or behave differently across platforms, which is often intentional.

## Menus for commands

Menu displays a temporary list of commands anchored to a trigger. Keep the anchor element in state, open the menu from the trigger, and close it when a choice is activated:

```jsx
import { useState } from 'react';
import Button from '@mui/material/Button';
import Menu from '@mui/material/Menu';
import MenuItem from '@mui/material/MenuItem';

function AccountActions() {
  const [anchorEl, setAnchorEl] = useState(null);
  const open = Boolean(anchorEl);

  function closeMenu() {
    setAnchorEl(null);
  }

  return (
    <>
      <Button
        aria-haspopup="true"
        aria-expanded={open ? 'true' : undefined}
        onClick={(event) => setAnchorEl(event.currentTarget)}
      >
        Account actions
      </Button>

      <Menu
        anchorEl={anchorEl}
        open={open}
        onClose={closeMenu}
      >
        <MenuItem onClick={closeMenu}>Profile</MenuItem>
        <MenuItem onClick={closeMenu}>Preferences</MenuItem>
        <MenuItem onClick={closeMenu}>Sign out</MenuItem>
      </Menu>
    </>
  );
}
```

Menu handles positioning and keyboard focus behavior for its items. Keep actions available to keyboard users, use clear item labels, and close the menu after an action. For a menu button, the trigger should communicate that it opens a menu.

## Control the popup state

Select and Menu can be controlled by state when the application needs to coordinate their open and closed behavior. Use `open` with the corresponding change or close handlers. Use an uncontrolled initial-open prop only when the application does not need to own the state.

Avoid changing focus behavior casually. Menus use focus management so users can navigate choices with the keyboard. If you compose a custom list instead of Menu, you become responsible for correct focus behavior.

## Common mistakes

- Using Menu when the user is entering a form value.
- Using Select for long, searchable, async, or creatable lists instead of Autocomplete.
- Omitting a visible label or accessible name.
- Forgetting to match `InputLabel.id` with `Select.labelId`.
- Omitting the outlined Select's `label` prop when using an InputLabel.
- Using `MenuItem` children with NativeSelect instead of native `option` elements.
- Treating Select's empty display as a built-in placeholder.
- Forgetting that the value for a multiple Select is an array.
- Opening a Menu without a meaningful trigger or leaving it open after its action.
- Disabling menu focus behavior without replacing it with a working focus strategy.

## Recap

Select is a form field for choosing a value. Menu is a temporary list of commands. NativeSelect uses the platform's native select element. Provide accessible labels for each, control values through state when needed, and choose the interaction based on list size and behavior.

## Practice task

Create a preferences form with a labelled region Select, a native time zone select, and a multiple Select for notification types. Add an Account actions menu that opens from a button and closes after a choice. Check labels, selected values, keyboard interaction, and the behavior on a mobile viewport.

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
      <td><a href="015-text-fields-and-input-components.md">015 - Text fields and input components</a></td>
      <td><a href="../qnas/016-selects-menus-and-native-selects.md">Q&amp;A</a></td>
      <td><a href="../examples/016-selects-menus-and-native-selects.md">Example</a></td>
      <td><a href="017-checkboxes-radio-buttons-and-switches.md">017 - Checkboxes, radio buttons, and switches</a></td>
    </tr>
  </tbody>
</table>
