# Lists, Dividers, and List Items - Q&A

## 1. What does the List component render by default?

List renders a `ul`. ListItem renders an `li`. Use these defaults for a semantic list of related entries.

## 2. When should I use ListItemButton?

Use it when a row is interactive, such as a navigation link or action. Keep plain informational rows as ListItem without an unnecessary button.

## 3. How do I make a list row a link?

Set `component="a"` and provide an `href`:

```jsx
<ListItemButton component="a" href="/settings/profile">
  <ListItemText primary="Profile" />
</ListItemButton>
```

For client-side routing, use the router link component with its destination prop.

## 4. How do I indicate the current page in a navigation list?

Use `selected` for the visual state and `aria-current="page"` for the current destination:

```jsx
<ListItemButton selected aria-current="page">
  <ListItemText primary="Security" />
</ListItemButton>
```

Selected styling alone does not expose the current page state.

## 5. What does ListItemText provide?

It formats the primary and optional secondary text within a row. Use concise primary text and secondary text for context that helps users choose or understand the row.

## 6. What is ListItemIcon for?

It wraps a leading icon and aligns it with the list text. Keep a text label for the row even when it has an icon.

## 7. How should a secondary action be added?

Use ListItem's `secondaryAction` prop for a separate action such as a menu button or switch. Give an icon-only control an accessible name that identifies its row.

## 8. Should a secondary action be inside ListItemButton?

No. Keep it separate so the row's primary action and the secondary control do not become nested interactive elements.

## 9. What is ListSubheader used for?

It labels a group in a longer list, such as a group of workspace settings. Use headings that make the grouping useful.

## 10. What does the `dense` prop do?

It reduces vertical spacing in the list and descendant items. Make sure compact rows remain readable and easy to activate.

## 11. How does the `divider` prop differ from a Divider component?

`divider` adds a bottom border to a list item or list button. Divider is a standalone separator component that can be horizontal or vertical.

## 12. How should Divider be rendered inside a List?

Use the item's `divider` prop, or render Divider as an `li` with `component="li"`. A default horizontal rule should not be placed directly as a child of a `ul`.

## 13. What is the difference between `variant="inset"` and a full-width Divider?

An inset variant shortens the divider so it aligns with list text rather than running beneath the leading icon or avatar. Use it when the row layout needs that visual alignment.

## 14. What does `orientation="vertical"` do?

It renders a vertical separator. In a flex row, add `flexItem` so the Divider gets the appropriate height.

## 15. Should every Divider be announced by screen readers?

A semantic separator is announced. If the line is purely decorative, use `aria-hidden="true"`. If the Divider wraps content, use an appropriate presentational wrapper so its child content remains accessible without announcing a redundant separator.

## 16. What is the difference between ListItem and ListItemButton?

ListItem is a semantic row container. ListItemButton adds an interactive button behavior and focus handling. Use the one that matches whether the row can be activated.

## 17. How do I align a multi-line row with its avatar?

Set `alignItems="flex-start"` on ListItem so the avatar aligns with the top of the text block rather than its vertical center.

## 18. Can a List contain nested lists?

Yes. Nested lists can represent a hierarchy. Keep the depth modest and make each level's relationship clear with appropriate parent text and subheaders.

## 19. Does ListItemButton's `selected` prop replace `aria-current`?

No. `selected` applies visual selected styling. For a current navigation page, use `aria-current="page"` to expose the meaning.

## 20. What should I check when a list row contains a link and a menu action?

Check that the row link and menu button are separate targets, both have useful names, and keyboard focus moves through them in a sensible order.

## Chapter navigation

<table width="100%">
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
      <td><a href="020-paper-cards-and-accordions.md">020 - Paper, cards, and accordions</a></td>
      <td><a href="../lessons/021-lists-dividers-and-list-items.md">Lesson</a></td>
      <td><a href="../examples/021-lists-dividers-and-list-items.md">Example</a></td>
      <td><a href="022-appbar-toolbar-and-top-navigation.md">022 - AppBar, Toolbar, and top navigation</a></td>
    </tr>
  </tbody>
</table>
