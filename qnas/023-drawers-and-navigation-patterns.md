# Drawers and Navigation Patterns - Q&A

## 1. What is a Material UI Drawer?

Drawer is a side surface for navigation or related application controls. Depending on its variant, it can overlay the page, sit beside and resize content, or remain permanently in the page layout.

## 2. What is the default Drawer variant?

The default is `temporary`. Set `variant` explicitly to `temporary`, `persistent`, or `permanent` so the intended layout and dismissal behavior are clear.

## 3. When should I use a temporary Drawer?

Use it for navigation that opens on demand, especially on small screens. It appears above the page with a backdrop and can normally close through Escape, backdrop interaction, or a destination selection.

## 4. When should I use a permanent Drawer?

Use it when navigation should always remain visible and there is room for both the navigation and page content. It is common in desktop layouts. It is not an overlay that users dismiss.

## 5. How does a persistent Drawer differ from a permanent one?

A persistent Drawer can be opened or closed and shares the page surface with the content. When it opens, the layout changes to make room. A permanent Drawer stays pinned in the layout and does not have a close interaction.

## 6. Does Drawer close itself when its button is clicked?

No. In a controlled temporary Drawer, store the open state in React. The `onClose` callback reports a request to close; update the state to false. The open button should set it to true.

## 7. What are the usual ways to close a temporary Drawer?

Keep the standard close behavior for Escape and backdrop interaction, then close it after a destination is selected. This supports keyboard, pointer, and touch users and returns them to the main page context.

## 8. How do I close a Drawer after client-side navigation?

In the destination's selection handler, update the route through the router and set the temporary Drawer state to false. Do not assume that route changes automatically unmount or close the Drawer.

## 9. How should I choose a Drawer width?

Choose a width that fits navigation labels without consuming too much of the content area. Set width on both the Drawer layout slot and its Paper surface when necessary, and use `boxSizing: 'border-box'` so padding is included in that width.

## 10. How do I make a Drawer responsive?

Use CSS breakpoints when the same content can remain mounted and only the layout changes. Use `useMediaQuery` when the React tree needs to change between temporary and permanent behavior. In both cases, preserve the same destinations and clear open/close behavior.

## 11. What should I consider about useMediaQuery with server rendering?

The server does not know the browser viewport by default. The hook can render with a default match and then resolve the real query on the client, causing an extra render and possible markup changes. Use the documented SSR options or choose a CSS-driven layout that avoids changing the rendered tree.

## 12. Can I use one Drawer component for both mobile and desktop?

Yes. Set the variant based on the viewport and control `open` so the permanent layout remains visible while the temporary layout follows mobile state. Make sure the mobile trigger appears only when needed and that the close callback affects the temporary case.

## 13. Why does a Drawer appear under my AppBar?

The Drawer and AppBar are separate components. A fixed AppBar may overlap the Drawer content unless the Drawer contents include a matching Toolbar-sized offset or the layout is deliberately arranged below it.

## 14. How do I keep page content beside a permanent Drawer?

Use a flex or grid shell with the Drawer and main content as siblings. Give the main region `flexGrow: 1` and `minWidth: 0` where needed, and reserve the Drawer width in the layout. Avoid arbitrary margins that do not match the actual Drawer width.

## 15. What does the anchor prop do?

`anchor` selects the side where the Drawer appears: left, right, top, or bottom. Side anchors are typical for navigation. Check the resulting placement and reading order when supporting right-to-left languages.

## 16. Is SwipeableDrawer always better on touch screens?

No. Swipe gestures can be convenient, but they add rendering and interaction work and can conflict with horizontal gestures in the page. Keep an explicit button to open navigation and test supported touch devices before choosing the swipeable variant.

## 17. What is a mini Drawer?

A mini Drawer is a narrow, persistent navigation rail that can expand to show more labels or content. It works when users need frequent access to sections and the remaining content still has enough space. Icon-only destinations need accessible names.

## 18. Should a Drawer use ListItemButton or clickable text?

Use semantic links for destinations and buttons for controls. ListItemButton can provide a styled interactive row, but render it through a link or router link for navigation. Keep the selected destination visually clear and expose the current page state where appropriate.

## 19. What landmark should contain Drawer navigation?

Place the destinations in a semantic `nav` landmark and provide a label when it helps distinguish that navigation from other navigation regions. Do not assume the Drawer surface alone communicates its purpose.

## 20. Can I disable the backdrop or Escape close behavior?

These behaviors are important for a temporary modal Drawer. Keep them enabled unless a specific interaction requires otherwise and a usable alternative is provided. Hiding the backdrop can make it harder to understand that the page is blocked or to dismiss the surface.

## 21. How do I make an icon-only menu trigger accessible?

Use an IconButton with a concise `aria-label` such as `aria-label="Open navigation"`. Keep a visible focus style and make its expanded state reflect whether the temporary Drawer is open.

## 22. What should I test in a responsive Drawer layout?

Test each breakpoint, keyboard focus and dismissal, route selection, focus restoration, large text, browser zoom, horizontal overflow, AppBar offsets, and right-to-left behavior if supported. Check that the selected destination matches the current page.

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
      <td><a href="022-appbar-toolbar-and-top-navigation.md">022 - AppBar, Toolbar, and top navigation</a></td>
      <td><a href="../lessons/023-drawers-and-navigation-patterns.md">Lesson</a></td>
      <td><a href="../examples/023-drawers-and-navigation-patterns.md">Example</a></td>
      <td><a href="024-tabs-and-tab-panels.md">024 - Tabs and tab panels</a></td>
    </tr>
  </tbody>
</table>
