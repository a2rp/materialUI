# Icons and Icon Buttons: Q&A

This Q&A accompanies [032 - Icons and Icon Buttons](../lessons/032-icons-and-icon-buttons.md). It focuses on icon packages, accessible names, interaction semantics, tooltips, and custom graphics.

## Icons and installation

### 1. Which package contains the Material SVG icons?

Install @mui/icons-material. It provides Material icon components built on Material UI's SvgIcon component and depends on Material UI.

### 2. Do I need to install an icon font to use these SVG components?

No. The Material SVG icon components render their paths directly. The separate Material UI Icon component uses a font ligature and requires the compatible icon font to be loaded.

### 3. How should I import a Material icon?

Import the icon from its specific package path, such as @mui/icons-material/Delete. This makes the used icons explicit and is the safest pattern for minimizing bundle size.

### 4. Can I import many icons from the package root?

Some bundlers can tree-shake named imports, but a specific import path is the safest option for bundle size. Avoid importing an entire icon collection dynamically when the screen only needs a few known icons.

### 5. How do icon style variants work?

Material icons may have filled, outlined, rounded, sharp, and two-tone variants. The exported component name often includes a suffix such as Outlined or Rounded. Choose the style that fits the rest of the interface and use it consistently.

### 6. When should I use SvgIcon instead of a ready-made icon?

Use SvgIcon when the design needs a custom SVG path or when wrapping an existing vector path in Material UI's sizing and color behavior. Use a ready-made Material icon when it communicates the same concept clearly.

### 7. When is the Icon component appropriate?

Use Icon when the project already loads a compatible ligature icon font. Without that font, the ligature may appear as text. For a self-contained application control, an SVG icon is often more predictable.

## IconButton and interaction

### 8. What is IconButton for?

IconButton is a button whose visual content is an icon. Use it for compact, familiar actions such as edit, delete, close, or opening a menu. Use a text Button when the action needs more explanation or emphasis.

### 9. Does an IconButton need an aria-label?

Yes, when the icon is the only visible content. The name should explain the action, such as “Delete project” rather than merely “Trash.” The decorative icon itself does not need to repeat the button's accessible name.

### 10. Can I put visible text beside an icon in IconButton?

IconButton is intended for icon-only controls. Use Button with startIcon or endIcon when the action includes visible text. Visible text then supplies the accessible name.

### 11. Should navigation use IconButton?

Navigation should use link semantics. Render the control as a link through the component integration used by the application and provide an accessible name that describes its destination or purpose.

### 12. How do I represent a two-state icon action?

Use aria-pressed for a toggle button and keep the state synchronized with the application. A favorite control can switch between pressed and unpressed states. Do not communicate state only by changing the icon's color.

### 13. Is a tooltip enough for an icon button's accessible name?

A Tooltip can supply the label when it is the control's only label. If the IconButton already has an aria-label or visible text, use the Tooltip as a description with describeChild. Avoid accidentally replacing a clear action name with an unrelated tooltip.

### 14. Should a destructive icon action have confirmation?

For a reversible, low-impact action, an IconButton may be enough. For irreversible or high-impact actions, provide clear context and appropriate confirmation or recovery. Do not use a tooltip as the only explanation of data loss.

## Styling and keyboard behavior

### 15. Which sizes and colors can IconButton use?

IconButton supports size and theme color options, along with disabled state and edge alignment. Use these to match the surrounding controls and design system rather than styling each icon button independently.

### 16. Should I remove the ripple effect?

Only if the remaining design still has a visible keyboard focus indicator. Removing ripple styling can also remove default focus-visible styling, so provide an explicit focus treatment when customizing it.

### 17. How large should the clickable target be?

The visual icon can be small while the button has enough padding to be easy to activate. Check spacing and target size on touch screens, and avoid crowding adjacent icon buttons.

### 18. What should a disabled IconButton communicate?

Use disabled state when the action is genuinely unavailable. If users may not understand why it is unavailable, provide an adjacent explanation or another clear status; disabled controls may not receive keyboard focus or pointer events.

### 19. Can IconButton show loading state?

Current Material UI versions provide a loading prop that disables the button and shows a progress indicator. Check the installed version before relying on it, and ensure the loading indicator is appropriately named for assistive technology.

## Accessibility and custom SVG

### 20. When should an SVG icon have its own accessible title?

A standalone informative SVG can use a meaningful title. Inside a button that already has an accessible name, the icon is usually decorative because the button provides the name.

### 21. Why does a custom SvgIcon look clipped or misaligned?

Check that the path coordinates fit the viewBox. A mismatched coordinate system can clip or distort the graphic. Also inspect how the path looks at the sizes used in the interface.

### 22. Can I use emoji as interface icons?

Avoid relying on emoji for consistent controls. Their appearance varies by platform and may not communicate the same action. Use a known SVG icon or a properly loaded icon font, with a clear accessible name.

### 23. Does Tooltip work with any child element?

The child must be able to receive the events and ref that Tooltip uses. A Material UI IconButton is suitable. When wrapping a custom component, make sure it forwards its ref to a DOM element.

## Chapter navigation

<table>
  <thead>
    <tr>
      <th>Previous Q&amp;A</th>
      <th>Chapter lesson</th>
      <th>Chapter example</th>
      <th>Next Q&amp;A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="031-mui-x-data-grid-community.md">031 - MUI X Data Grid Community</a></td>
      <td><a href="../lessons/032-icons-and-icon-buttons.md">Lesson</a></td>
      <td><a href="../examples/032-icons-and-icon-buttons.md">Example</a></td>
      <td><a href="033-date-and-time-pickers.md">033 - Date and Time Pickers</a></td>
    </tr>
  </tbody>
</table>
