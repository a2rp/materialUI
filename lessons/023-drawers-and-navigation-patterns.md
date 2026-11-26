# Drawers and Navigation Patterns

## Learning goals

After this chapter, you should be able to:

- Choose among temporary, persistent, and permanent Drawer variants.
- Build a responsive navigation pattern that changes with available screen space.
- Control Drawer open and close state.
- Integrate a Drawer with an AppBar and page content.
- Keep navigation semantics, keyboard behavior, and focus handling intact.
- Understand the tradeoffs of swipeable and mini drawers.

## What a Drawer is for

A Drawer is a side surface that holds navigation or related app controls. It can be an overlay that opens on demand, a panel that shifts content, or a permanent part of the desktop layout.

A Drawer is not just a visual container. Choose its variant according to how people use the screen and how much room navigation needs. Use the List and ListItemButton patterns from the previous chapter for its destinations.

## Drawer variants

| Variant | Behavior | Common use |
| --- | --- | --- |
| `temporary` | Opens above the page with a backdrop. It can close from the backdrop, Escape, or a destination selection. | Mobile navigation or an occasional side panel. |
| `persistent` | Opens and closes beside the content. The page layout adapts to the Drawer width. | A workspace where people frequently show and hide navigation. |
| `permanent` | Always visible and pinned into the page layout. | Desktop navigation that remains available. |

The default variant is temporary. Set it explicitly so the intended interaction is clear.

```jsx
<Drawer
  variant="temporary"
  open={mobileOpen}
  onClose={closeDrawer}
>
  {/* Navigation content */}
</Drawer>
```

Temporary Drawers use modal behavior, including a backdrop and focus management. Escape and backdrop dismissal should normally remain enabled. The `onClose` callback reports a close request; keep the open state in your component so the Drawer responds.

A persistent Drawer remains part of the layout when open and changes the room available to the main content. A permanent Drawer is not controlled as a modal overlay and cannot be dismissed by a backdrop.

## Responsive navigation

A common pattern uses a temporary Drawer on small screens and a permanent Drawer on larger screens. CSS breakpoints are suitable when only appearance changes. Use `useMediaQuery` when React must switch the rendered Drawer variant or interaction model.

```jsx
import { useTheme } from '@mui/material/styles';
import useMediaQuery from '@mui/material/useMediaQuery';

function ResponsiveLayout() {
  const theme = useTheme();
  const isWideScreen = useMediaQuery(theme.breakpoints.up('md'));

  return (
    <Drawer
      variant={isWideScreen ? 'permanent' : 'temporary'}
      open={isWideScreen || mobileOpen}
      onClose={closeDrawer}
    >
      {/* Shared navigation */}
    </Drawer>
  );
}
```

In a real component, define `mobileOpen`, `closeDrawer`, and a menu button that opens the Drawer. The menu button should be available only when the temporary version is in use.

Prefer CSS media queries when the same elements can remain mounted and only their layout changes. A media query hook can cause a server-rendered page to render twice while the client determines the actual screen width. For server-rendered applications, consider the `useMediaQuery` SSR options or a layout that avoids a server/client markup mismatch.

Do not use viewport width as the only rule for information architecture. Check whether the navigation labels, hierarchy, and expected tasks make sense at each size.

## Drawer width and paper

The Drawer surface is rendered as a Paper. Set a useful width and `boxSizing` so padding does not unexpectedly make it wider than intended:

```jsx
const DRAWER_WIDTH = 280;

<Drawer
  variant="permanent"
  sx={{
    width: DRAWER_WIDTH,
    flexShrink: 0,
    '& .MuiDrawer-paper': {
      width: DRAWER_WIDTH,
      boxSizing: 'border-box',
    },
  }}
>
  {/* Navigation */}
</Drawer>
```

A fixed width is common for a desktop navigation rail. At smaller widths, a temporary Drawer may use a comfortable width that leaves part of the page context visible, while still fitting narrow devices. Avoid choosing a width that forces horizontal scrolling.

## Integrating with AppBar and content

The layout must account for the Drawer and the AppBar as separate surfaces. For a permanent Drawer, a flex or grid page shell can place the navigation beside the main content. For a fixed AppBar, a Toolbar-sized offset may be needed inside the Drawer and above the page content.

```jsx
<>
  <AppBar position="fixed">
    <Toolbar>
      <Typography component="div" variant="h6">
        Workspace
      </Typography>
    </Toolbar>
  </AppBar>

  <Box sx={{ display: 'flex' }}>
    <Drawer variant="permanent">
      <Toolbar aria-hidden="true" />
      {/* Navigation list */}
    </Drawer>

    <Box component="main" sx={{ flexGrow: 1, minWidth: 0 }}>
      <Toolbar aria-hidden="true" />
      {/* Page content */}
    </Box>
  </Box>
</>
```

The example shows the alignment pattern, not a complete responsive shell. Use one deliberate offset for each fixed surface. Do not add duplicate spacing that leaves a large blank area.

## Navigation content

Use a semantic navigation landmark and a labelled list for destinations:

```jsx
<Box component="nav" aria-label="Primary navigation">
  <List>
    <ListItem disablePadding>
      <ListItemButton component="a" href="/overview" selected>
        <ListItemText primary="Overview" />
      </ListItemButton>
    </ListItem>
  </List>
</Box>
```

The `href` in an application should be connected to the actual router or route. Use `selected` to style the current destination, and make sure the current page is also communicated semantically where appropriate. Do not mark a destination selected only because it is being hovered or focused.

For a temporary Drawer, close it when a destination is chosen. This matters because selecting a route may not automatically dismiss the Drawer in a client-side application.

## Anchors and direction

The `anchor` prop selects the side from which the Drawer appears: `left`, `right`, `top`, or `bottom`. Side drawers are typical for navigation. Top and bottom drawers may suit filters or compact task surfaces.

For right-to-left layouts, verify the anchor and navigation order in the complete page. Directionality affects placement and reading flow; do not assume that mirroring CSS alone provides correct behavior.

## Swipeable Drawer

`SwipeableDrawer` adds touch gestures to a temporary navigation surface. Swiping can be useful on touch devices, but it adds interaction and rendering work. It can also conflict with controls that use horizontal gestures. Keep a visible button as an alternative to swiping, and test on the devices and browsers you support.

## Mini and persistent patterns

A mini Drawer keeps a narrow navigation rail visible and expands when requested. It can work for frequently used sections when there is enough room for the main content. Provide accessible names for icon-only destinations and a clear expanded state for the control.

Use a persistent Drawer only when users benefit from changing the content width as navigation opens and closes. It is usually a poor fit for a very narrow screen because the Drawer and content compete for the same limited width.

## Accessibility checklist

- Use a labelled `nav` landmark for navigation links.
- Use buttons for controls that open or close the Drawer, and links for destinations.
- Give an icon-only menu button a clear accessible name.
- Keep Escape and backdrop dismissal for temporary Drawers unless there is a strong interaction reason to change them.
- Preserve visible focus and allow keyboard users to move through destinations.
- Close temporary navigation after a route is selected.
- Keep the page heading and main content discoverable after the Drawer closes.
- Test narrow widths, large text, zoom, right-to-left layout when supported, and screen reader announcements.

## Common mistakes

### Using a temporary Drawer on every screen size

A modal overlay can hide too much context on a wide screen. A permanent or persistent pattern may be more appropriate there.

### Leaving a fixed AppBar or Drawer without layout space

Overlay surfaces can cover content. Add matching offsets and inspect the first heading, focused controls, and anchor targets.

### Forgetting that the temporary Drawer is controlled

Calling a close callback does not update state by itself. Store the open state and set it to false when the Drawer requests closure.

### Using clickable text without link semantics

Navigation changes location. Use links or router link components, not generic clickable text.

### Switching variants without considering server rendering

A client-only viewport measurement may not match server markup. Choose an SSR strategy when using a media query hook in a server-rendered app.

## Practical guidance

1. Start with permanent navigation on wide desktop screens and temporary navigation on small screens.
2. Use persistent navigation only when its content-width behavior fits the work pattern.
3. Keep the temporary Drawer easy to open and close by pointer, touch, and keyboard.
4. Choose a width that supports labels without squeezing the content area.
5. Review the actual responsive result rather than treating a breakpoint as a complete design solution.

## Recap

Drawer variants represent different layout and interaction models. Temporary is modal and overlays the page; persistent adjusts the page layout; permanent stays visible in the layout. Responsive navigation often uses temporary and permanent variants at different widths. Give navigation semantic structure, keep close behavior reliable, and make sure bars and side panels do not hide content.

## Practice

Create a responsive settings layout with four destinations. Use a permanent Drawer on wide screens and a temporary Drawer on small screens. Add a selected destination, close the mobile Drawer after selection, and check the page with keyboard navigation.

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
      <td><a href="022-appbar-toolbar-and-top-navigation.md">022 - AppBar, Toolbar, and top navigation</a></td>
      <td><a href="../qnas/023-drawers-and-navigation-patterns.md">Q&amp;A</a></td>
      <td><a href="../examples/023-drawers-and-navigation-patterns.md">Example</a></td>
      <td><a href="024-tabs-and-tab-panels.md">024 - Tabs and tab panels</a></td>
    </tr>
  </tbody>
</table>
