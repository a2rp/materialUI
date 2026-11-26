# AppBar, Toolbar, and Top Navigation - Q&A

## 1. What is the difference between AppBar and Toolbar?

AppBar is the surface that marks a screen-level header. Toolbar is the flex row normally placed inside it to align a title, navigation controls, and actions. A Toolbar can also be used as a spacer below a fixed AppBar.

## 2. What position should I choose for a normal page header?

Start with `position="static"`. It stays in normal document flow, reserves its height, and scrolls with the page. Choose sticky or fixed only when the screen has a clear reason to keep the bar visible.

## 3. Is AppBar static by default?

No. The API default for `position` is `fixed`. Set the position explicitly so the layout behavior is clear. A fixed bar leaves normal flow, so page content may start behind it.

## 4. What is the difference between sticky and fixed positioning?

A sticky bar starts in normal flow and sticks when it reaches its scroll boundary. A fixed bar is anchored to the viewport and is removed from normal flow immediately. Sticky behavior depends on the scrolling ancestors; fixed positioning needs a content offset.

## 5. How do I stop a fixed AppBar from covering the page?

Add one offset after the bar, such as a second Toolbar with the same sizing, or add responsive top spacing based on the theme's toolbar mixin. Pick one method and verify it at each breakpoint. Avoid applying both methods at once.

## 6. Why might position="sticky" fail to stick?

Sticky positioning is constrained by its scroll container and ancestors. An ancestor with overflow, an unexpected scroll region, or insufficient space can change where it sticks. Check the layout's containing and scrolling elements, or use static positioning if a persistent header is not essential.

## 7. When should I use Toolbar's dense variant?

Use `variant="dense"` when the design needs a shorter toolbar, often in compact or desktop-specific layouts. Check target sizes and text readability after reducing its height. Dense does not mean that controls should become difficult to activate.

## 8. What does disableGutters do?

`disableGutters` removes Toolbar's built-in horizontal padding. Use it when a parent Container or layout already controls the left and right spacing. Without a parent spacing rule, content may sit too close to the viewport edge.

## 9. Should the AppBar contain the page's h1?

It can, if that title is the page's main heading and fits the document hierarchy. Use a heading element through Typography's `component` prop. If the global product title is not the page heading, give the actual page heading a suitable level in the content instead.

## 10. Does AppBar automatically create a navigation landmark?

No. AppBar is a header surface. Add a semantic `nav` element for navigation links and give it an accessible label when the page has multiple navigation regions. A group of actions is not automatically navigation.

## 11. Should top-level destinations be MenuItems?

Usually, persistent destinations should be links inside a labelled navigation region. Menu and MenuItem are intended for temporary choices or commands with menu keyboard behavior. A compact popup can present navigation on small screens when its trigger and destinations remain clear.

## 12. How should a menu trigger announce its state?

Use a real button with a useful accessible name. When relevant, connect it to the popup with `aria-controls` and expose whether it is open with `aria-expanded`. Keep the id stable and ensure the attribute points to the menu it controls.

## 13. How do I open a Material UI Menu?

Store the trigger element in state and pass it as `anchorEl`. Set `open` from that state. Clear the anchor in `onClose` and after a selection so the popup closes. Each independent menu should have its own anchor state.

## 14. Why should every menu close after an item is chosen?

A menu represents a temporary choice surface. Closing after a selection confirms the action and returns focus and interaction to the surrounding screen. If selecting an option should leave a persistent selection visible, reflect that state in the page as well.

## 15. How should I make an AppBar responsive?

Keep the title and most important action visible, then rearrange or move secondary navigation at smaller widths. Use responsive display rules or a compact Menu or Drawer pattern. Do not hide desktop links without making the same destinations available on mobile.

## 16. Should a small-screen navigation popup use Menu or Drawer?

Use Menu for a short set of temporary choices anchored to a button. Use Drawer for a larger navigation area or a persistent side-navigation pattern. The next chapter covers Drawer patterns in more detail.

## 17. Why does changing AppBar color sometimes have no effect in dark mode?

Material UI's color behavior in dark mode depends on the theme and component defaults. The AppBar API provides `enableColorOnDark` for cases where the selected color should apply in dark mode. Check contrast after enabling it; a color token alone does not guarantee readable content.

## 18. What does the AppBar elevation control?

The inherited `elevation` prop controls the shadow depth. A low or zero elevation can make the bar feel integrated with the page; a stronger shadow can separate it from scrolling content. Avoid using shadow as the only signal of hierarchy.

## 19. Can AppBar be used at the bottom of a screen?

Yes. A bottom AppBar can hold a small set of important actions, but it competes with the content viewport and should not duplicate other persistent navigation without a reason. Account for safe areas and ensure it does not cover focused controls or form content.

## 20. What does useScrollTrigger do with an AppBar?

`useScrollTrigger` observes scrolling and returns whether the configured threshold has been crossed. It can drive behaviors such as hiding or elevating a bar. Use it only when the behavior improves the reading experience, and test direction changes, keyboard focus, reduced motion, and scroll containers other than the window.

## 21. How do I make an icon-only AppBar action accessible?

Give its IconButton a concise `aria-label` that describes the action, such as `aria-label="Open account menu"`. A tooltip may provide a visual hint, but it does not replace a reliable accessible name.

## 22. What should I check before shipping a top navigation bar?

Check keyboard focus and activation, small-screen behavior, landmark labels, heading order, color contrast, fixed-bar offsets, anchor targets, and whether every visible action has a clear purpose. Test at browser zoom and confirm the page does not scroll horizontally.

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
      <td><a href="021-lists-dividers-and-list-items.md">021 - Lists, dividers, and list items</a></td>
      <td><a href="../lessons/022-appbar-toolbar-and-top-navigation.md">Lesson</a></td>
      <td><a href="../examples/022-appbar-toolbar-and-top-navigation.md">Example</a></td>
      <td><a href="023-drawers-and-navigation-patterns.md">023 - Drawers and navigation patterns</a></td>
    </tr>
  </tbody>
</table>
