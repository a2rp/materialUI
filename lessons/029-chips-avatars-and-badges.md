# Chips, Avatars, and Badges

## Learning goals

After this chapter, you should be able to:

- Use Chip for compact attributes, selections, and actions.
- Choose image, initials, or icon content for Avatar.
- Group participants with AvatarGroup.
- Add Badge counts or dots to a control.
- Preserve keyboard behavior and accessible names.
- Avoid using visual decoration as the only way to convey information.

## Chip

Chip represents a compact piece of information or an action. Common examples include a selected filter, category, contact, or removable input value.

```jsx
<Chip label="Design" />
<Chip label="In review" variant="outlined" />
```

Use `size="small"` when space is limited and the text remains readable. Use color and variants to emphasize the meaning without making every chip visually compete.

### Clickable and deletable chips

Add `onClick` when the chip activates an action. Add `onDelete` when the user can remove the item:

```jsx
<Chip label="Type: report" onDelete={removeFilter} />
<Chip label="Open details" onClick={openDetails} />
```

A clickable chip behaves like a button. A deletable chip supports keyboard interaction while focused, including Backspace or Delete for removal and Escape to blur. Give labels enough context to make the action understandable.

The `clickable` prop controls the visual presentation. It does not create an action handler by itself. For a link, use `component` with an actual anchor or router link and set the clickable appearance as needed.

Avoid chips for a long list of options that needs filtering, grouping, or selection rules. Use a list, checkbox group, or dedicated selection component when that interaction is clearer.

## Avatar

Avatar represents a person, organization, or object. It can render an image, initials, or an icon:

```jsx
<Avatar alt="Maya Chen" src="/images/maya-chen.jpg" />
<Avatar aria-label="Northstar project">N</Avatar>
<Avatar>
  <FolderOutlinedIcon />
</Avatar>
```

When an image fails to load, Avatar can fall back to its children, the first letter of the alt text, or a generic icon. Provide useful children when a specific fallback matters.

Use a meaningful image alternative when the image communicates identity and that identity is not already given in nearby text. If nearby text already names the same person and the image adds no information, avoid announcing the same name twice.

Initials should be recognizable, concise, and stable. A color may help people distinguish avatars, but color alone should not identify a person or convey a role.

## AvatarGroup

AvatarGroup stacks related avatars and limits how many are displayed with `max`:

```jsx
<AvatarGroup max={4}>
  <Avatar alt="Maya Chen" src="/images/maya-chen.jpg" />
  <Avatar alt="Noah Patel" src="/images/noah-patel.jpg" />
  <Avatar alt="Ari Morgan" src="/images/ari-morgan.jpg" />
  <Avatar alt="Sam Rivera" src="/images/sam-rivera.jpg" />
  <Avatar alt="Kai Brooks" src="/images/kai-brooks.jpg" />
</AvatarGroup>
```

Use the group for a meaningful set of participants, not as a decorative pile of anonymous images. If the overflow count matters, make the total understandable in text or a labelled control. Check that the collapsed group still communicates who is present.

## Badge

Badge adds a small count or dot to another element, often an icon button or avatar:

```jsx
<IconButton aria-label="Show 12 unread messages">
  <Badge badgeContent={12} color="secondary">
    <MailOutlineIcon />
  </Badge>
</IconButton>
```

Use `variant="dot"` for a simple state marker when a precise count is not necessary. Use `anchorOrigin` to position the badge, and `overlap="circular"` when it sits on a circular avatar or icon surface.

By default, a badge with content zero is hidden. Set `showZero` when zero is meaningful to the user. The `max` prop abbreviates larger values, with `99+` as the default presentation above that threshold.

### Badge accessibility

The badge visual does not automatically explain its meaning. Give the underlying interactive control an accessible name that includes the count or state, for example "Show 12 unread messages". Keep that name synchronized with the current data. Do not rely on a dot's color alone to communicate unread or alert status.

## Combining the components

Avatar and Badge work together to show a participant's online state or notification count. Chip can include an Avatar for a compact contact or category. Keep each element's purpose clear and avoid stacking too many visual markers on the same item.

```jsx
<Badge
  color="success"
  variant="dot"
  overlap="circular"
  aria-label="Online"
>
  <Avatar alt="Maya Chen" src="/images/maya-chen.jpg" />
</Badge>
```

If the combination is interactive, put the accessible name on the button or link that users activate. A Badge is a visual decoration around content, not a button by itself.

## Semantics and interaction

- Use Chip for compact attributes or actions, not as a substitute for every control.
- Use links for navigation and buttons for actions.
- Keep a visible label for clickable or deletable chips.
- Give icon-only controls a useful accessible name.
- Avoid duplicate accessible names when an Avatar is next to a visible person name.
- Include counts in the accessible name of controls with a Badge.
- Use the correct semantic control when selecting one or more options.

## Common mistakes

### Styling a chip as clickable without an action

`clickable` changes its appearance but does not provide a meaningful action. Use `onClick` or a real link component.

### Using deletion as an unclear action

A chip's delete control removes the represented item. Make the label clear about what will be removed and update the underlying data when it is activated.

### Making Avatar identity depend only on color

Background colors do not identify people for everyone. Use initials, image alternatives, or adjacent names.

### Repeating avatar names in screen reader output

If the same name appears visibly beside the Avatar, a meaningful alt text can cause duplicate announcements. Decide whether the image adds distinct information.

### Showing a Badge without an accessible count

If a badge conveys unread items, include that count in the control's accessible name. Otherwise, assistive technology users may only hear the icon action.

### Using a dot as the only warning

A colored dot has no explicit meaning. Add text, a count, or a status description where the state matters.

## Practical guidance

1. Choose Chip for compact values and lightweight actions.
2. Choose Avatar content that identifies the person or entity clearly.
3. Limit AvatarGroup only when the overflow remains understandable.
4. Put Badge counts on an interactive control's accessible name.
5. Keep icon controls and chip actions keyboard operable.
6. Test labels and fallbacks with images unavailable and with screen readers.

## Recap

Chip represents compact information or an action. Avatar represents a person or entity with an image, initials, or icon. AvatarGroup stacks a related set. Badge adds a count or dot to another element. Use each visual component with the semantic names and interactions people need.

## Practice

Create a project participant row with an AvatarGroup, a clickable team Chip, and an unread-message Badge on an icon button. Add names and counts to the accessible labels, then check the row with keyboard navigation.

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
      <td><a href="028-progress-indicators-and-skeletons.md">028 - Progress indicators and skeletons</a></td>
      <td><a href="../qnas/029-chips-avatars-and-badges.md">Q&amp;A</a></td>
      <td><a href="../examples/029-chips-avatars-and-badges.md">Example</a></td>
      <td><a href="030-tables-and-data-display.md">030 - Tables and data display</a></td>
    </tr>
  </tbody>
</table>
