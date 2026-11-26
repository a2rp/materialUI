# Tabs and Tab Panels

## Learning goals

After this chapter, you should be able to:

- Use Tabs and Tab to switch between related content panels.
- Keep the selected tab controlled by React state.
- Connect each tab to its panel with accessible ids and ARIA attributes.
- Choose a standard, full-width, scrollable, or vertical layout.
- Understand manual and automatic keyboard activation.
- Decide whether a tab pattern or page navigation is the right interaction.

## When to use tabs

Tabs organize a small set of peer views that belong to the same context. Selecting one changes the content panel without navigating to a different page. Examples include profile sections, project details, and settings categories.

Do not use tabs just because a design has several links. If each item navigates to a distinct page or route, use navigation links and the application's router. Tabs imply a tablist, one selected tab, and an associated panel.

## Tabs and Tab components

`Tabs` manages the selected value, visual indicator, and keyboard navigation. `Tab` is an individual control in that tablist. Store the selected value in React state and update it through `onChange`:

```jsx
const [tab, setTab] = useState('profile');

<Tabs
  value={tab}
  onChange={(event, nextTab) => setTab(nextTab)}
  aria-label="Account settings"
>
  <Tab value="profile" label="Profile" />
  <Tab value="security" label="Security" />
  <Tab value="notifications" label="Notifications" />
</Tabs>
```

Values can be strings or numbers. Use stable explicit values when panels are tied to named sections. The default value for a Tab is its child index, but relying on indexes becomes fragile if tabs are reordered or conditionally rendered.

## Connect every Tab to its panel

MUI provides the interactive Tabs and Tab controls. When you compose panels with the standard Material UI API, give each tab and panel matching ids and relationships:

```jsx
<Tabs value={tab} onChange={handleTabChange} aria-label="Account settings">
  <Tab
    id="profile-tab"
    aria-controls="profile-panel"
    value="profile"
    label="Profile"
  />
</Tabs>

<div
  role="tabpanel"
  id="profile-panel"
  aria-labelledby="profile-tab"
  tabIndex={0}
  hidden={tab !== 'profile'}
>
  {tab === 'profile' && <ProfileSettings />}
</div>
```

Use a unique id for each tab and panel. The tab's `aria-controls` points to its panel, and the panel's `aria-labelledby` points back to the tab. The panel should be focusable with `tabIndex={0}` when it has no focusable content or when its first meaningful content is not focusable.

The `hidden` attribute keeps inactive panels out of view and out of the accessibility tree. In the snippet, the inactive component is also not rendered. If you need an inactive panel to preserve local state, keep its children mounted and rely on `hidden` instead; consider the extra memory and any effects that remain active.

## A reusable TabPanel

For several panels, a small wrapper can keep the relationships consistent:

```jsx
function TabPanel({ value, panelValue, id, labelledBy, children }) {
  const selected = value === panelValue;

  return (
    <div
      role="tabpanel"
      id={id}
      aria-labelledby={labelledBy}
      tabIndex={0}
      hidden={!selected}
    >
      {selected ? children : null}
    </div>
  );
}
```

Pass the same selected value to Tabs and each panel. Keep ids stable and unique across the page. If a panel's content starts with a focusable control, consider whether focusing the panel itself is useful for your content flow.

Material UI's experimental `@mui/lab` package also provides TabContext, TabList, and TabPanel utilities that generate the relationships. Use that API only when you are comfortable with its package and stability status.

## Keyboard behavior

Tabs implements manual activation by default:

- Tab moves focus into the tablist and out of it.
- Arrow keys move focus between tabs.
- The focused tab does not become selected until the user activates it, usually with Enter or Space.
- Home and End move to the first and last tabs.

With `selectionFollowsFocus`, selection changes as focus moves. This automatic behavior can be convenient when changing panels is immediate and inexpensive. Manual activation can be easier when loading a panel is slow or changes state. Choose deliberately and test with the keyboard.

Do not add custom arrow-key handlers around the Tabs component unless you are intentionally replacing its behavior. Duplicating keyboard logic can break expected focus movement.

## Layout variants

### Standard

The standard variant uses the available space without forcing every tab to the same width. It works well for a few tabs with short labels.

### Full width

Use `variant="fullWidth"` when a small set of tabs should share the available width evenly. This can work on compact screens, but very long labels may wrap or become hard to scan.

### Scrollable

Use `variant="scrollable"` when there are more tabs than fit comfortably in one row. `scrollButtons="auto"` shows scroll buttons when needed on wider layouts. Buttons are hidden on mobile by default; set `allowScrollButtonsMobile` when persistent visible buttons are needed.

Scrollable tabs are still one tablist. Keep labels concise, provide a visible cue that more tabs exist, and verify touch and keyboard scrolling.

### Vertical

Set `orientation="vertical"` when tabs form a side-by-side list of categories and panels. Give the tablist an accessible name and arrange it beside the panel in a layout that still works at narrow widths.

## Disabled tabs and icons

Use `disabled` only when the destination is unavailable and users cannot activate it. Avoid disabling items just to hide unfinished product areas.

A tab may use text, an icon, or both. Icon-only tabs need an accessible name such as `aria-label`. When space allows, text labels are easier to recognize than unexplained icons.

## Tabs and navigation are different

Tabs are for switching views within one page context. For route navigation, link tabs can be composed with a routing library, but the control must preserve link behavior and page navigation semantics. Do not add a tabpanel for unrelated route links merely to keep the same visual style.

## Common mistakes

### Leaving out the tab-panel relationship

A visual indicator does not tell assistive technology which content belongs to a tab. Connect every tab and panel with unique ids, `aria-controls`, and `aria-labelledby`.

### Reusing tab values

Each tab should have a unique value. Duplicate values make selected state ambiguous.

### Unmounting a panel that needs to retain form state

Conditional rendering removes the inactive panel and its component state. Keep panels mounted when preserving state is required, while accounting for effects and memory.

### Making tabs too numerous or too long

Tabs are intended for a manageable set of peer options. Use a different navigation pattern when labels overflow or the list becomes difficult to scan.

### Enabling automatic activation without checking cost

If every focus change triggers expensive work, arrowing through tabs can feel slow. Keep manual activation or optimize the panel before enabling selection-on-focus.

## Practical guidance

1. Confirm the content belongs to one page context before choosing tabs.
2. Keep the selected value controlled and unique.
3. Label the tablist and link each tab to its panel.
4. Use the built-in keyboard behavior and decide whether activation is manual or automatic.
5. Choose a layout variant that fits without clipping labels.
6. Check whether inactive panel state should be preserved.

## Recap

Tabs provide a coordinated control and panel pattern for related views. React state controls the active value. Accessible composition requires a labelled tablist and explicit tab-to-panel relationships. MUI supports standard, full-width, scrollable, and vertical layouts, and its default keyboard model uses manual activation.

## Practice

Create tabs for "General", "Members", and "Notifications". Associate each tab with its panel, use stable string values, and test the tablist with arrow keys, Enter, and Space. Add a long label and decide whether a scrollable layout is clearer than wrapping it.

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
      <td><a href="023-drawers-and-navigation-patterns.md">023 - Drawers and navigation patterns</a></td>
      <td><a href="../qnas/024-tabs-and-tab-panels.md">Q&amp;A</a></td>
      <td><a href="../examples/024-tabs-and-tab-panels.md">Example</a></td>
      <td><a href="025-dialogs-and-modal-workflows.md">025 - Dialogs and modal workflows</a></td>
    </tr>
  </tbody>
</table>
