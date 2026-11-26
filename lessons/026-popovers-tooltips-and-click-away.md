# Popovers, Tooltips, and Click-Away

## Learning goals

After this chapter, you should be able to:

- Choose Tooltip, Popover, or Popper based on the interaction.
- Position a Popover relative to its trigger.
- Control a popup's open state and dismissal behavior.
- Use Tooltip without hiding essential information from touch or keyboard users.
- Detect outside interactions with ClickAwayListener when a popup does not provide that behavior.
- Keep popup content and focus behavior accessible.

## Choose the right surface

| Component | Best for | Modal behavior |
| --- | --- | --- |
| `Tooltip` | A short label or description attached to a focusable control. | No. It does not block the page. |
| `Popover` | A compact interactive surface anchored to a control. | Yes. It is built on Modal, blocks page scrolling, and closes on click-away by default. |
| `Popper` | A positioned surface that should not use modal behavior. | No. Add dismissal and accessibility behavior that the interaction needs. |
| `ClickAwayListener` | Detecting an interaction outside one wrapped element. | No. It is a utility, not a popup surface. |

Use a dialog for an interruptive task, a menu for a list of choices or commands, and a Drawer for side navigation. These surfaces have different interaction and focus behavior.

## Tooltip

Tooltip displays short information when a user hovers over, focuses, or touches its trigger. Its child must be a single element that can receive the necessary event handlers and ref.

```jsx
<Tooltip title="Delete report">
  <IconButton aria-label="Delete report">
    <DeleteIcon />
  </IconButton>
</Tooltip>
```

By default, Tooltip uses its title as the accessible label for its child. If the trigger already has visible text or an `aria-label` and the tooltip provides extra information, use `describeChild`:

```jsx
<Tooltip describeChild title="Does not add a duplicate report">
  <Button>Add report</Button>
</Tooltip>
```

Keep tooltip content brief. Do not use a tooltip as the only way to access instructions, error details, or important content. Tooltips are not intended to contain interactive controls.

On touch devices, Tooltip typically appears after a long press. This is not a substitute for information that should always be visible. For a disabled control, wrap it in an element that can receive pointer events; explain disabled state in visible text when it matters.

## Popover

Popover places content near a trigger. It is useful for small interactive content that needs more space than a tooltip, such as a short contextual form or a preview. It uses Modal behavior, including scroll lock, focus management, backdrop handling, and click-away dismissal.

Store the trigger element and pass it through `anchorEl`. Set `open` from state and handle `onClose`. Use `anchorOrigin` and `transformOrigin` to align the surface:

```jsx
const [anchorEl, setAnchorEl] = useState(null);
const open = Boolean(anchorEl);

<Button
  aria-describedby={open ? 'report-popover' : undefined}
  onClick={(event) => setAnchorEl(event.currentTarget)}
>
  Show details
</Button>

<Popover
  id="report-popover"
  open={open}
  anchorEl={anchorEl}
  onClose={() => setAnchorEl(null)}
  anchorOrigin={{ vertical: 'bottom', horizontal: 'left' }}
  transformOrigin={{ vertical: 'top', horizontal: 'left' }}
>
  <Box sx={{ p: 2 }}>Short contextual details</Box>
</Popover>
```

The anchor origin selects the point on the trigger. The transform origin selects the point on the popup. Their values determine how the two surfaces meet. The popup may reposition to stay within the viewport, so check it near screen edges.

Popover can also be positioned from explicit coordinates or a virtual element. Use those options for specialized cases such as a text selection. For an ordinary trigger, an element anchor is easier to keep synchronized with the interface.

## Popper

Popper positions content relative to an anchor but does not provide the modal behavior of Popover. It does not block scrolling or automatically dismiss itself on click-away. This makes it useful for non-modal suggestions and custom anchored content.

```jsx
<Popper open={open} anchorEl={anchorEl} placement="bottom-start">
  <Paper elevation={4} sx={{ p: 2 }}>
    Suggested actions
  </Paper>
</Popper>
```

Add the interaction behavior your surface needs. A custom Popper may require an explicit close button, Escape handling, focus behavior, and a ClickAwayListener. Do not add menu or dialog roles unless the content follows that pattern's keyboard and semantic expectations.

## ClickAwayListener

ClickAwayListener detects a click or touch outside its one child element. It is useful when a Popper or custom panel should close after an outside interaction:

```jsx
<ClickAwayListener onClickAway={handleClose}>
  <Box>
    <Button onClick={handleToggle}>Toggle details</Button>
    {open && <Paper>Custom non-modal content</Paper>}
  </Box>
</ClickAwayListener>
```

The child must be able to hold a ref. By default, the listener responds after the click or touch completes. The `mouseEvent` and `touchEvent` props can change it to listen to the start of an interaction. Leading-event behavior ignores scrollbar interactions.

Portaled elements are usually treated as part of the same React tree. The `disableReactTree` option changes click-away handling to use the DOM tree only. Test portal behavior before changing this setting.

ClickAwayListener adds event handling to its child. If the child is not interactive, adding `role="presentation"` can prevent assistive technology from treating it like a control.

## Positioning and portals

Popover and Popper use a portal by default, so the popup may be rendered elsewhere in the DOM while remaining positioned relative to its anchor. Portals help avoid clipping from ancestor overflow, but can affect stacking, CSS inheritance, and click-away logic.

Use a stable anchor element. If the trigger disappears or moves while the popup is open, update or close the popup. Check stacking order against dialogs, Drawers, and other overlays rather than increasing z-index without understanding the layer order.

## Accessibility and interaction

- Make the trigger a real button or link with a useful accessible name.
- Ensure a Tooltip can be activated by focus as well as pointer interaction.
- Use `describeChild` when a Tooltip supplements an existing accessible name.
- Keep important information available without hovering.
- Give interactive Popover content a clear heading or relationship to its trigger.
- Keep keyboard focus behavior suitable for the surface. A modal Popover manages focus as a modal; a non-modal Popper needs deliberate focus behavior.
- Include a visible way to close long-lived or interactive content. Click-away should not be the only way to dismiss it.
- Avoid overlapping popups or opening several independent modal surfaces at once.

## Common mistakes

### Using Tooltip for interactive content

A Tooltip is a short descriptive label, not a container for buttons, links, or forms. Use Popover, Menu, or Dialog depending on the task.

### Making a Tooltip the only label for an icon

The trigger needs a reliable accessible name. Use an IconButton's `aria-label` for the action and use `describeChild` when the tooltip adds context.

### Using Popover when the page should remain interactive

Popover blocks page scrolling and uses modal behavior. Use Popper with explicit interaction management when the page should remain interactive.

### Forgetting to close a custom Popper

Popper does not dismiss itself on outside interaction. Add a close control, Escape handling, and ClickAwayListener as appropriate.

### Assuming visual placement is fixed

Popups can reposition near viewport edges. Test all placements and avoid content that becomes clipped or difficult to reach.

### Wrapping a non-interactive heading in ClickAwayListener without considering its handler

ClickAwayListener attaches event handling to its child. Use a suitable wrapper and presentation role where required, and test with screen readers.

## Practical guidance

1. Use Tooltip for a short label or description, not a task flow.
2. Use Popover when the anchored content is interactive and should block background interaction.
3. Use Popper for positioned content that should remain non-modal.
4. Use ClickAwayListener only when the popup needs custom outside-click dismissal.
5. Keep the trigger stable and ensure each popup has a clear open and close path.
6. Test keyboard, touch, screen edges, portals, and stacking with other surfaces.

## Recap

Tooltip adds a short label or description to a trigger. Popover is an anchored modal surface with built-in dismissal and focus behavior. Popper positions non-modal content and needs custom interaction handling. ClickAwayListener detects outside interactions for a single wrapped child. Choose by behavior, then check labels, focus, and dismissal.

## Practice

Build a filter button that opens a small set of controls. Decide whether it should be a Popover or a non-modal Popper. Add an explicit close path, make the trigger's purpose clear, and verify that the surface works with a keyboard and touch input.

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
      <td><a href="025-dialogs-and-modal-workflows.md">025 - Dialogs and modal workflows</a></td>
      <td><a href="../qnas/026-popovers-tooltips-and-click-away.md">Q&amp;A</a></td>
      <td><a href="../examples/026-popovers-tooltips-and-click-away.md">Example</a></td>
      <td><a href="027-alerts-snackbars-and-feedback.md">027 - Alerts, snackbars, and feedback</a></td>
    </tr>
  </tbody>
</table>
