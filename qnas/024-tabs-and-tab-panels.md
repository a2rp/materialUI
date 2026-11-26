# Tabs and Tab Panels - Q&A

## 1. What are Material UI Tabs for?

Tabs switch among a manageable set of related views within one page context. Each tab represents one panel. They are not a general replacement for links to separate pages.

## 2. What is the difference between Tabs and Tab?

`Tabs` is the tablist container. It coordinates the selected value and keyboard movement. `Tab` is one selectable control inside the tablist.

## 3. Does MUI provide a TabPanel in the core package?

The core package provides Tabs and Tab. With the standard API, compose the panel markup and its ARIA relationships yourself. The experimental `@mui/lab` package includes TabContext, TabList, and TabPanel helpers.

## 4. How should I control the selected tab?

Keep the value in React state, pass it to `Tabs`, and update it in `onChange`. Each Tab needs a matching unique value. This makes selection explicit and lets your application update other content consistently.

## 5. Can I rely on the index value of each Tab?

MUI uses a child's index as the default value when no explicit value is provided. This works for a static list, but explicit stable values are safer if tabs may be reordered, inserted, or conditionally rendered.

## 6. How do I connect a tab to its panel?

Give each Tab a unique `id` and an `aria-controls` value pointing to its panel. Give the panel a unique `id` and an `aria-labelledby` value pointing back to its Tab. Keep all ids unique on the page.

## 7. Does Tabs add the panel relationships automatically?

The standard `Tabs` and `Tab` components manage tablist and tab semantics and keyboard interaction, but a custom panel still needs its own id, role, and relationship attributes. The Lab helpers provide these relationships for you.

## 8. When should a panel have tabIndex={0}?

Set `tabIndex={0}` when the panel has no focusable content or its first meaningful content is not focusable. It gives keyboard users a way to focus and read the panel. Avoid adding an unnecessary extra focus stop without considering the panel's content flow.

## 9. What is the default keyboard activation behavior?

Tabs uses manual activation by default. Arrow keys move focus between tabs, but the active panel changes only after the user activates the focused tab with Enter or Space. This prevents arrowing through tabs from triggering expensive content changes.

## 10. What does selectionFollowsFocus do?

`selectionFollowsFocus` changes to automatic activation. Moving focus to another tab also selects it and displays its panel. Use it when panel changes are immediate and do not create a slow or disruptive operation.

## 11. Which keyboard keys work in a horizontal tablist?

Tab enters and leaves the tablist. Arrow keys move between tabs. Home and End move to the first and last tabs. Enter or Space activates the focused tab when using the default manual activation model.

## 12. Should tabs be used for route navigation?

Usually not. Use links and your router for distinct pages. If a design uses tabs for route navigation, preserve link semantics and route behavior instead of pretending the pages are in-page tab panels.

## 13. What is the difference between tabs and a button group?

Tabs select one view from a set and show an associated panel. A button group groups related actions or options. If selecting a button changes a filter or formatting option rather than switching between labelled panels, a button group or another control may fit better.

## 14. When should I use the fullWidth variant?

Use `variant="fullWidth"` when a small set of tabs should share the available width, often on a compact view. Check that labels remain readable and controls do not become cramped.

## 15. When should I use the scrollable variant?

Use `variant="scrollable"` when more tabs exist than fit in the available width. Configure `scrollButtons` when appropriate and check swipe, touch, and keyboard access. By default, scroll buttons are hidden on mobile; `allowScrollButtonsMobile` changes that behavior.

## 16. What does centered do?

`centered` centers the tabs when there is room. It is intended for larger views and does not combine with every layout variant. Use it for visual alignment, not to solve overflow.

## 17. Can Tabs be vertical?

Yes. Set `orientation="vertical"` and place the tablist beside the panel. Give the tablist an accessible name and make the layout responsive so both tabs and panel remain usable on small screens.

## 18. How should I treat disabled tabs?

Set `disabled` when an option is genuinely unavailable. Explain unavailable functionality elsewhere if users need to know why. Do not use disabled tabs to hide unfinished or access-restricted content without clear context.

## 19. Should inactive panels remain mounted?

It depends. Unmounting inactive panels can reduce rendered work but resets local component state. Keeping panels mounted can preserve form inputs, but all panel children consume memory and effects may continue while hidden. Choose based on the panel's state and workload.

## 20. What does the experimental Lab API add?

`@mui/lab` provides TabContext, TabList, and TabPanel to help wire up accessible tab relationships. It is marked experimental in the documentation, so check its status and compatibility before making it a central dependency.

## 21. Why does my tab indicator not match the selected content?

The controlled `value` may not match the values assigned to the Tab children, or state may not update in `onChange`. Confirm that values are unique, the selected value exists, and panel visibility uses the same value.

## 22. What should I test before shipping tabs?

Test the tablist name, ids and ARIA relationships, selected state, keyboard behavior, manual or automatic activation, panel focus, narrow layouts, long labels, disabled tabs, and state retention when switching panels.

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
      <td><a href="023-drawers-and-navigation-patterns.md">023 - Drawers and navigation patterns</a></td>
      <td><a href="../lessons/024-tabs-and-tab-panels.md">Lesson</a></td>
      <td><a href="../examples/024-tabs-and-tab-panels.md">Example</a></td>
      <td><a href="025-dialogs-and-modal-workflows.md">025 - Dialogs and modal workflows</a></td>
    </tr>
  </tbody>
</table>
