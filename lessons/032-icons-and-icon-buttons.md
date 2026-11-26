# Icons and Icon Buttons

Icons communicate familiar actions and visual concepts in a compact space. Material UI supports ready-made Material SVG icons, custom SVG icons through SvgIcon, and font icons through Icon. IconButton provides a focusable control for an action that is represented by an icon.

This chapter explains how to choose and import icons, label icon-only controls, add tooltips, and preserve keyboard and focus behavior.

## Learning goals

After this chapter, you should be able to:

- Install and import Material SVG icons.
- Choose between an SVG icon, a custom SvgIcon, and a font icon.
- Give icon-only controls accessible names.
- Use IconButton for actions and links with the correct semantics.
- Keep icon imports and visual styling maintainable.

## Material SVG icons

Install the icon package:

```bash
npm install @mui/icons-material
```

The package depends on Material UI. Import the specific icons used by the screen:

```jsx
import DeleteIcon from "@mui/icons-material/Delete";
import EditOutlinedIcon from "@mui/icons-material/EditOutlined";
import FavoriteBorderIcon from "@mui/icons-material/FavoriteBorder";
```

A specific icon path makes dependencies visible and is the safest import pattern for minimizing bundle size. Material icon names are exported in PascalCase. Style variants such as outlined, rounded, sharp, and two-tone use corresponding suffixes when available.

Choose icons by their meaning, not only by their appearance. A familiar symbol can still be ambiguous without a visible label, tooltip, or accessible name.

## IconButton for icon-only actions

Use IconButton when an action is represented by an icon rather than visible text. Give it an accessible name that describes the action:

```jsx
import IconButton from "@mui/material/IconButton";
import DeleteIcon from "@mui/icons-material/Delete";

<IconButton aria-label="Delete project" onClick={handleDelete}>
  <DeleteIcon />
</IconButton>
```

The icon is decorative within the named button; the button supplies the accessible name. A bare icon does not explain the action to screen-reader users. Use a regular Button with text when the action needs more explanation or prominence.

IconButton supports sizes, theme colors, disabled state, and edge alignment. Choose a size that fits the surrounding controls and leaves a usable target. Do not remove keyboard focus styling when customizing ripples or outlines.

## Toggle actions

For a two-state action such as favorite or mute, expose its state with aria-pressed and keep the icon and accessible name synchronized with the state:

```jsx
<IconButton
  aria-label={favorite ? "Remove from favorites" : "Add to favorites"}
  aria-pressed={favorite}
  onClick={() => setFavorite((current) => !current)}
>
  {favorite ? <FavoriteIcon /> : <FavoriteBorderIcon />}
</IconButton>
```

If an icon changes with the state, make the state available to assistive technology as well. Do not rely only on a filled or colored icon to indicate whether a toggle is active.

## Tooltips

A Tooltip can explain an icon button to users who hover, focus, or tap it. The button still needs an accessible name. When the button already has a visible label or aria-label, use the Tooltip as an additional description:

```jsx
import IconButton from "@mui/material/IconButton";
import Tooltip from "@mui/material/Tooltip";
import DeleteIcon from "@mui/icons-material/Delete";

<Tooltip describeChild title="Permanently removes this project">
  <IconButton aria-label="Delete project" onClick={handleDelete}>
    <DeleteIcon />
  </IconButton>
</Tooltip>
```

Do not make a tooltip the only visible instruction for a complex or high-impact action. A tooltip is not a replacement for confirmation when an action can cause data loss.

## Links that look like icon buttons

When the control navigates, use link semantics rather than a button action:

```jsx
<IconButton
  component="a"
  href="/projects/atlas"
  aria-label="Open Atlas project"
>
  <OpenInNewIcon />
</IconButton>
```

The label should describe the destination or navigation purpose. Keep browser behaviors such as opening a link in a new tab available when the application requires them.

## Custom SVG icons with SvgIcon

Use SvgIcon to wrap a custom SVG path so it follows Material UI color, size, and theme behavior:

```jsx
import SvgIcon from "@mui/material/SvgIcon";

function CompassIcon(props) {
  return (
    <SvgIcon {...props} viewBox="0 0 24 24">
      <path d="M12 2 4 21l8-4 8 4-8-19zm0 5.6 3.4 8.1-3.4-1.7-3.4 1.7L12 7.6z" />
    </SvgIcon>
  );
}
```

Use a viewBox that matches the coordinate system of the path. If the SVG has multiple paths or a more complex shape, preserve its geometry and verify its alignment at the component's supported sizes. For a semantic standalone graphic, provide an accessible title; inside an IconButton with an accessible name, the icon is usually decorative.

## Font icons

Material UI's Icon component renders a ligature from an icon font. The font must be loaded by the application before the icon name can render correctly. Without the font, users may see the literal ligature text.

Font icons can be appropriate when a project already uses a compatible icon font. For most application controls, SVG icons offer a self-contained asset without a separate font download. Do not assume the font is available just because the Icon component is installed.

## Size, color, and contrast

Use the IconButton size prop or an icon's fontSize prop to fit a consistent visual scale. Use theme colors to communicate meaning, but do not make color the only way to distinguish actions or states. Check icon contrast against its background, including disabled and hover states.

If an icon-only control has a small visual glyph, preserve enough padding around it to make the control easy to target. Keep focus indicators visible, and check that adjacent icon buttons are not crowded.

## Loading and disabled states

A disabled IconButton cannot be activated. Use disabled state only when the action is genuinely unavailable, and make the reason understandable nearby when it may not be obvious.

Recent Material UI versions also provide a loading state for IconButton. It disables interaction while showing a progress indicator. Check the installed version before using version-dependent props, and ensure the loading indicator has an accessible name through the button.

## Common mistakes

- Rendering an icon-only button without an accessible name.
- Using a tooltip as the only accessible name when the trigger already has a label.
- Giving a navigation action button semantics instead of link semantics.
- Using color or a changed glyph as the only indication of a toggle state.
- Importing many icons through a broad barrel without considering bundle impact.
- Loading a font icon without loading its font.
- Removing focus styles when disabling ripple effects.
- Making destructive actions too easy to activate without clear feedback.
- Using an icon whose meaning is not clear in the surrounding context.

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
      <td><a href="031-mui-x-data-grid-community.md">031 - MUI X Data Grid Community</a></td>
      <td><a href="../qnas/032-icons-and-icon-buttons.md">Q&amp;A</a></td>
      <td><a href="../examples/032-icons-and-icon-buttons.md">Example</a></td>
      <td><a href="033-date-and-time-pickers.md">033 - Date and Time Pickers</a></td>
    </tr>
  </tbody>
</table>
