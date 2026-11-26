# AppBar, Toolbar, and Top Navigation

## Learning goals

After this chapter, you should be able to:

- Choose an AppBar position that fits the screen and page behavior.
- Use Toolbar as the horizontal layout container inside an AppBar.
- Build a clear top header with a title, primary navigation, and actions.
- Add a popup Menu with appropriate open and close behavior.
- Prevent fixed app bars from covering page content.
- Keep navigation usable at narrow viewport widths and with a keyboard.

## AppBar and Toolbar roles

An AppBar provides a top-level surface for information and actions related to the current screen. It often contains the product name, current page title, primary navigation, and a small number of useful actions.

Toolbar is the horizontal flex layout normally placed inside AppBar. It gives its children a shared row and aligns them vertically. Use spacing and flex rules to keep the title, navigation, and actions in a predictable order.

```jsx
import AppBar from '@mui/material/AppBar';
import Toolbar from '@mui/material/Toolbar';
import Typography from '@mui/material/Typography';

<AppBar position="static">
  <Toolbar>
    <Typography component="h1" variant="h6">
      Project workspace
    </Typography>
  </Toolbar>
</AppBar>
```

Specify `position` intentionally. The AppBar default is `fixed`, which removes it from normal document flow and can cover the top of the page. A static bar takes up space in normal flow and is a simple default for a page header.

## Position choices

| Position | Behavior | Use it when |
| --- | --- | --- |
| `static` | Remains in normal document flow and scrolls with the page. | The header should take up its normal space. |
| `sticky` | Starts in normal flow, then sticks when it reaches its scroll boundary. | A header should remain visible after scrolling to it. |
| `fixed` | Stays relative to the viewport and is removed from normal flow. | Persistent screen-level controls are necessary. Add an offset for the content. |
| `absolute` | Is positioned relative to its containing block and removed from normal flow. | The bar intentionally overlays a specific region. |

Sticky positioning depends on the surrounding scroll container and can fall back to static in unsupported contexts. Fixed and absolute bars can obscure content, focus targets, or anchor destinations. Prefer static or sticky unless the screen requires an always-visible bar.

## Toolbar sizing and spacing

Toolbar is a flex container. Use its flex properties to assign room to the title and navigation:

```jsx
<Toolbar>
  <Typography sx={{ flexGrow: 1 }} variant="h6" component="div">
    Workspace
  </Typography>
  <Button color="inherit">Help</Button>
</Toolbar>
```

Useful adjustments include:

- `variant="dense"` for a shorter toolbar in compact layouts.
- `disableGutters` when the parent layout already controls horizontal spacing.
- `sx` flex properties such as `flexGrow`, `gap`, and responsive display values.
- A responsive Container inside Toolbar when the app bar contents should align with the page grid.

A dense toolbar is a visual sizing choice. It should not make controls too small to use comfortably.

## Top navigation and popup menus

Use links for navigation to a different page or section. A persistent set of top-level destinations can be represented by links or buttons inside a labelled `nav` landmark. Keep labels short and include only the most useful destinations.

Use `Menu` and `MenuItem` for a temporary list of choices or commands opened by a control. A popup menu is not a general replacement for site navigation. The trigger should be a real button, and the menu should close after a selection or when dismissed.

Material UI provides AppBar, Toolbar, and Menu primitives. It does not provide a complete, opinionated top-navigation information architecture. Your application still decides which destinations appear, how routing works, and how navigation changes on small screens.

## Fixed bar content offset

A fixed AppBar does not reserve layout space. Content can begin underneath it. One simple pattern is to place a second Toolbar after the AppBar. This spacer uses the same toolbar sizing so the following content starts below the fixed bar:

```jsx
<>
  <AppBar position="fixed">
    <Toolbar>
      <Typography component="div" variant="h6">
        Workspace
      </Typography>
    </Toolbar>
  </AppBar>
  <Toolbar aria-hidden="true" />
  <main>{/* Page content */}</main>
</>
```

Another option is to add top padding or margin based on the theme's toolbar mixin. Use the same responsive sizing as the actual toolbar. Avoid inserting both a spacer and equivalent top padding, which would create a double gap.

## Accessibility and responsive behavior

- Give each navigation landmark an accessible label when the page has more than one navigation region.
- Use a heading for the page or product title where the heading fits the document structure.
- Give icon-only buttons an accessible name, for example `aria-label="Open navigation"`.
- Keep visible keyboard focus. Do not remove focus outlines without providing a clear replacement.
- Connect a popup trigger to its menu using a stable id and `aria-controls` when appropriate. Expose open state with `aria-expanded`.
- On small screens, reduce or rearrange links instead of letting controls overflow. A compact menu or Drawer can hold secondary destinations.
- Avoid placing every action in the app bar. Put page-specific or infrequent actions in the page content or a contextual surface.
- Check that fixed headers do not hide focused controls or anchor targets. Use scroll margin on target sections where needed.

## Common mistakes

### Forgetting the fixed-position offset

If the page starts under a fixed AppBar, add one matching offset and verify it at every toolbar breakpoint.

### Treating every navigation item as a menu command

Persistent destinations are usually links in a labelled navigation region. Use Menu for temporary choices that follow menu keyboard behavior.

### Adding too many controls

A crowded toolbar makes the screen hierarchy unclear. Keep the page identity and primary actions visible, then move secondary actions elsewhere.

### Assuming color alone communicates meaning

Use readable text, sufficient contrast, and a clear focus state. The color prop and visual theme do not replace accessible names or content structure.

### Hiding navigation without an alternative

Responsive styles can hide desktop links, but the same destinations must remain available through a usable small-screen pattern.

## Practical guidance

1. Start with `position="static"` for ordinary page headers.
2. Choose fixed or sticky only when keeping controls visible helps the user.
3. Use Toolbar for alignment, not as a substitute for page layout.
4. Keep top navigation limited to the main destinations.
5. Give each menu trigger a clear name, and keep popup state tied to the correct anchor element.
6. Test at narrow widths, with keyboard input, and with browser zoom.

## Recap

AppBar establishes a screen-level header surface. Toolbar arranges its contents. Position determines whether the bar participates in document flow, and that choice affects page layout. Persistent destinations belong in navigation links, while temporary choices can use a popup Menu. Fixed bars need a deliberate content offset and careful focus behavior.

## Practice

Create an AppBar for a small project dashboard. Include a page title, two desktop navigation links, and a mobile menu button. Use static positioning first, then try sticky positioning. Verify that the links remain available at narrow widths and that every menu action is keyboard accessible.

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
      <td><a href="021-lists-dividers-and-list-items.md">021 - Lists, dividers, and list items</a></td>
      <td><a href="../qnas/022-appbar-toolbar-and-top-navigation.md">Q&amp;A</a></td>
      <td><a href="../examples/022-appbar-toolbar-and-top-navigation.md">Example</a></td>
      <td><a href="023-drawers-and-navigation-patterns.md">023 - Drawers and navigation patterns</a></td>
    </tr>
  </tbody>
</table>
