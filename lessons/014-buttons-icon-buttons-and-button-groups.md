# Buttons, Icon Buttons, and Button Groups

## Learning goals

After this chapter, you should be able to:

- Choose an emphasis level with Button variants.
- Use buttons for actions and links for navigation.
- Add icons without losing a clear accessible name.
- Group related buttons with ButtonGroup.
- Use loading and disabled states without creating ambiguous feedback.
- Choose a selection component when a group represents a selected value.

## Choosing an action control

Buttons let users perform an action or make a choice. Their label should make the result clear. Use a link for navigation and a button for an action.

Material UI provides \`Button\`, \`IconButton\`, \`ButtonGroup\`, and other specialized controls. Add only the control package the application needs. The Material UI icon set is a separate package.

## Button variants and emphasis

Button has three standard variants:

| Variant | Visual emphasis | Typical use |
| --- | --- | --- |
| \`contained\` | Highest emphasis with a filled surface | The primary action in a group or screen |
| \`outlined\` | Medium emphasis with a border | An important alternative action |
| \`text\` | Lowest emphasis | A secondary action or a low emphasis action in a card |

```jsx
import Button from '@mui/material/Button';

<Button variant="contained">Save changes</Button>
<Button variant="outlined">Preview</Button>
<Button variant="text">Cancel</Button>
```

Use emphasis to show priority. A page or dialog usually has one primary action. If every action is filled, users may not know which is most important.

Buttons can also use theme palette colors such as \`primary\`, \`secondary\`, \`success\`, \`warning\`, and \`error\`, when those meanings fit the action. Color should not be the only signal of an outcome.

## Button size and width

Use \`size="small"\`, \`"medium"\`, or \`"large"\` to choose a supported control size. Use \`fullWidth\` when the button should fill its available container:

```jsx
<Button size="small" variant="outlined">
  Details
</Button>

<Button fullWidth variant="contained">
  Continue
</Button>
```

Full width is useful in some narrow layouts and forms. It is not a replacement for deciding how a group should wrap or align.

## Button actions and navigation

Use \`onClick\` for an action:

```jsx
<Button
  variant="contained"
  onClick={() => {
    console.info('Changes saved');
  }}
>
  Save
</Button>
```

When the control navigates to a location, use an anchor. Providing \`href\` makes Button render as a link:

```jsx
<Button href="/account/security" variant="outlined">
  Security settings
</Button>
```

For client-side routing, use your routing library's link component through the \`component\` prop and provide a meaningful destination. Do not make a non-navigation action look like a link, or use a button solely to navigate when an anchor is appropriate.

## Icons with text buttons

An icon can reinforce a label. Keep the text because it explains the action:

```jsx
import Button from '@mui/material/Button';
import SaveIcon from '@mui/icons-material/Save';

<Button variant="contained" startIcon={<SaveIcon />}>
  Save changes
</Button>
```

Use \`startIcon\` and \`endIcon\` to position an icon before or after the label. The icon package is separate from \`@mui/material\`.

Avoid adding decorative icons that do not clarify the action. If the icon itself repeats the visible label, consider hiding the icon from assistive technology.

## IconButton

Use IconButton for an action represented by an icon alone, such as opening a menu or dismissing a notification. An icon-only control needs an accessible name:

```jsx
import IconButton from '@mui/material/IconButton';
import MenuIcon from '@mui/icons-material/Menu';

<IconButton aria-label="Open navigation">
  <MenuIcon />
</IconButton>
```

The label should describe the action, not merely the icon shape. If the control's meaning changes with state, update the accessible name to reflect the current action or state as appropriate.

Use \`size\` to adjust the control's visual size and \`color\` when a palette meaning is clear. A tooltip can provide a visible hint, but it should not replace the accessible name on an icon-only button.

## Disabled and loading states

Use \`disabled\` when an action is unavailable. Explain the requirement when users need to know why they cannot act.

For supported current versions, Button's \`loading\` prop shows progress and disables interaction while work is pending:

```jsx
<Button
  variant="contained"
  loading={isSaving}
  onClick={saveChanges}
>
  Save changes
</Button>
```

The value should be a boolean or \`null\`. Keep the action's label meaningful while it loads. Show a separate status message if users need more detail than a progress indicator can convey.

## ButtonGroup

ButtonGroup visually groups related buttons and shares properties such as variant, color, and size:

```jsx
import Button from '@mui/material/Button';
import ButtonGroup from '@mui/material/ButtonGroup';

<ButtonGroup
  variant="outlined"
  aria-label="Report time range"
>
  <Button>Week</Button>
  <Button>Month</Button>
  <Button>Year</Button>
</ButtonGroup>
```

The buttons should be immediate children of ButtonGroup. Groups can be horizontal or vertical through \`orientation\`. Provide an accessible label that describes the group.

ButtonGroup is best for related actions or navigation choices that remain separate actions. If buttons represent a mutually exclusive selected value, use a selection control such as ToggleButtonGroup, radio buttons, or a select rather than treating an action group as state.

## Common mistakes

- Using a button for navigation without a destination, or using a link for an action.
- Giving icon-only controls no accessible name.
- Using several contained buttons for equally ranked actions.
- Relying on color alone to communicate success, danger, or selection.
- Disabling an action without explaining the requirement when that reason matters.
- Treating a ButtonGroup as a selected-value control.
- Using an icon from the icon package without installing that separate package.
- Removing focus feedback when customizing the ripple without adding a visible focus style.

## Recap

Use Button variants to show action priority. Use anchors for navigation and buttons for actions. Give icon-only controls an accessible name. Use ButtonGroup for related actions, and choose a selection component when the interface represents a selected value. Loading and disabled states should make the action's availability clear.

## Practice task

Build a form action row with one primary action, one secondary action, and a cancel action. Add an icon to the primary action and create an icon-only menu button with an accessible label. Then create a labelled group of related view links and decide whether it represents navigation or a selected state.

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
      <td><a href="013-grid-layout.md">013 - Grid layout</a></td>
      <td><a href="../qnas/014-buttons-icon-buttons-and-button-groups.md">Q&amp;A</a></td>
      <td><a href="../examples/014-buttons-icon-buttons-and-button-groups.md">Example</a></td>
      <td><a href="015-text-fields-and-input-components.md">015 - Text fields and input components</a></td>
    </tr>
  </tbody>
</table>
