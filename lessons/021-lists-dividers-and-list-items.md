# Lists, Dividers, and List Items

## Learning goals

After this chapter, you should be able to:

- Represent repeated content with List and ListItem.
- Make list rows interactive with ListItemButton.
- Add primary and secondary text, icons, and a separate secondary action.
- Use ListSubheader and nested lists to organize items.
- Separate content with dividers without breaking list semantics.
- Preserve accessible names and state for selected rows.

## List structure

Material UI's List renders a \`ul\` by default, and ListItem renders an \`li\`. Use them to present related rows such as navigation, settings, conversations, or messages:

```jsx
import List from '@mui/material/List';
import ListItem from '@mui/material/ListItem';
import ListItemText from '@mui/material/ListItemText';

<List>
  <ListItem>
    <ListItemText
      primary="Account"
      secondary="Email and profile information"
    />
  </ListItem>
  <ListItem>
    <ListItemText
      primary="Security"
      secondary="Password and sign-in options"
    />
  </ListItem>
</List>
```

Use ListItem for content that is not an action. Do not make every row clickable when the row has no meaningful action.

## Interactive rows with ListItemButton

Use ListItemButton when the row navigates or performs an action. For navigation, render it as an anchor with a destination:

```jsx
import ListItemButton from '@mui/material/ListItemButton';

<List component="nav" aria-label="Account settings">
  <ListItemButton component="a" href="/account/profile">
    <ListItemText primary="Profile" />
  </ListItemButton>
  <ListItemButton component="a" href="/account/security">
    <ListItemText primary="Security" />
  </ListItemButton>
</List>
```

The component prop controls the rendered element. Use a button element for an action, and an anchor for navigation. For a client-side router, pass its link component through \`component\` and supply the route.

A \`selected\` prop styles the active row. For a current navigation destination, also expose the current page state with \`aria-current="page"\`:

```jsx
<ListItemButton
  component="a"
  href="/account/security"
  selected
  aria-current="page"
>
  <ListItemText primary="Security" />
</ListItemButton>
```

Selected styling alone does not explain the navigation state to assistive technology. Use a semantic state such as \`aria-current\` where it applies.

## Icons, avatars, and secondary text

Use ListItemIcon for a leading icon and ListItemAvatar for an avatar. Use ListItemText for primary and secondary content:

```jsx
import ListItemAvatar from '@mui/material/ListItemAvatar';
import ListItemIcon from '@mui/material/ListItemIcon';
import ListItemText from '@mui/material/ListItemText';

<ListItemButton>
  <ListItemIcon>
    <SettingsIcon />
  </ListItemIcon>
  <ListItemText
    primary="Preferences"
    secondary="Language, date, and display options"
  />
</ListItemButton>
```

Keep a meaningful text label even when an icon is present. If an icon is decorative because the adjacent text already explains the row, hide it from assistive technology when appropriate.

For long text, align list items to the start rather than centering an avatar against several lines:

```jsx
<ListItem alignItems="flex-start">
  <ListItemAvatar>
    <Avatar alt="Ada Lovelace" src="/images/ada.jpg" />
  </ListItemAvatar>
  <ListItemText
    primary="Project review"
    secondary="Ada shared a detailed review with notes on accessibility and responsive behavior."
  />
</ListItem>
```

Provide alternative text for meaningful avatars or images. If an avatar is decorative and the name is already adjacent, use an empty alt value.

## Secondary actions

A ListItem can provide a separate secondary action, such as a menu or switch. Keep it as a separate target from a primary row action:

```jsx
import IconButton from '@mui/material/IconButton';
import ListItem from '@mui/material/ListItem';
import ListItemButton from '@mui/material/ListItemButton';
import ListItemText from '@mui/material/ListItemText';

<ListItem
  secondaryAction={
    <IconButton edge="end" aria-label="More options for Security">
      <MoreVertIcon />
    </IconButton>
  }
  disablePadding
>
  <ListItemButton>
    <ListItemText primary="Security" secondary="Password and sign-in options" />
  </ListItemButton>
</ListItem>
```

Give icon-only secondary actions an accessible name that includes enough context to distinguish the row. Keep the secondary action outside the ListItemButton's interactive area.

## Dense and gutterless lists

Use \`dense\` to create a more compact list. It can be set on List and is available to descendant items. Use \`disablePadding\` on List when the surrounding surface already manages outer spacing. Use \`disableGutters\` on an item when its parent defines the horizontal alignment.

Compact spacing should still leave enough room for the content and targets. Check the result on touch devices and with enlarged text.

## List subheaders and nested lists

Use ListSubheader to label groups within a longer list:

```jsx
import ListSubheader from '@mui/material/ListSubheader';

<List
  subheader={
    <ListSubheader component="div" id="settings-subheader">
      Workspace
    </ListSubheader>
  }
>
  <ListItemButton>
    <ListItemText primary="Projects" />
  </ListItemButton>
  <ListItemButton>
    <ListItemText primary="Members" />
  </ListItemButton>
</List>
```

For nested navigation, place another List inside the parent item and label the relationship. Keep nesting shallow enough that users can understand where they are.

## Dividers between items

Divider provides a visual separator. Use it outside the List for content sections. Inside a List, use a list-compatible element or the item's \`divider\` prop:

```jsx
<List>
  <ListItem divider>
    <ListItemText primary="Inbox" />
  </ListItem>
  <ListItem>
    <ListItemText primary="Drafts" />
  </ListItem>
</List>
```

If you need a standalone Divider between list children, set \`component="li"\` so the list contains valid list children:

```jsx
<List>
  <ListItem>
    <ListItemText primary="Inbox" />
  </ListItem>
  <Divider component="li" />
  <ListItem>
    <ListItemText primary="Drafts" />
  </ListItem>
</List>
```

A horizontal Divider renders as a separator. Set \`aria-hidden="true"\` when the line is purely decorative. When the Divider contains text or another element, use a non-semantic wrapper and presentational role so the separator does not add unnecessary noise to screen readers.

## Vertical dividers

Set \`orientation="vertical"\` in a flex row. Use \`flexItem\` so the divider receives the appropriate height:

```jsx
import Divider from '@mui/material/Divider';
import Stack from '@mui/material/Stack';

<Stack direction="row" spacing={2} sx={{ alignItems: 'center' }}>
  <Typography>Recent</Typography>
  <Divider orientation="vertical" flexItem />
  <Typography>Archived</Typography>
</Stack>
```

Vertical dividers render as a div with separator semantics. Use them sparingly; spacing alone may separate items clearly.

## Common mistakes

- Rendering an action row as a plain ListItem without an interactive control.
- Using an anchor without a destination or a button for navigation.
- Relying only on selected styling to indicate the current page.
- Nesting a secondary icon button inside a ListItemButton.
- Using an icon without a text label or accessible name.
- Placing a default Divider element directly as a child of a ul.
- Announcing purely decorative separators unnecessarily.
- Using very dense rows that are hard to read or operate.
- Creating many nested list levels without a clear information hierarchy.

## Recap

List and ListItem provide a semantic vertical structure. Use ListItemButton for actions and links, ListItemText for clear labels and supporting copy, and ListSubheader for groups. Keep secondary actions separate, communicate selected state semantically, and make divider elements valid within a list.

## Practice task

Build a settings navigation list with icons, primary and secondary text, and a current page. Add a separate secondary action to one row. Group a longer list with subheaders, then verify the list's DOM structure, keyboard access, active page state, and separator semantics.

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
      <td><a href="020-paper-cards-and-accordions.md">020 - Paper, cards, and accordions</a></td>
      <td><a href="../qnas/021-lists-dividers-and-list-items.md">Q&amp;A</a></td>
      <td><a href="../examples/021-lists-dividers-and-list-items.md">Example</a></td>
      <td><a href="022-appbar-toolbar-and-top-navigation.md">022 - AppBar, Toolbar, and top navigation</a></td>
    </tr>
  </tbody>
</table>
