# Paper, Cards, and Accordions - Q&A

## 1. What is Paper used for?

Paper is a surface container with a theme-based elevation shadow or an outline. Use it when content needs a surface distinct from the page background.

## 2. What is the difference between Paper and Box?

Box is a general theme-aware wrapper. Paper adds a surface treatment. If the layout needs spacing or semantics but no surface, Box or Container may be clearer.

## 3. What does Paper's `elevation` prop control?

It selects a shadow level from 0 to 24. Use a small set of levels to establish hierarchy rather than assigning a different shadow to every panel.

## 4. How do I create a flat Paper with a border?

Set `variant="outlined"`. This displays a border without the elevation shadow.

## 5. What is Card designed for?

Card groups content and actions about one subject. It can include a header, media, content, and supplemental actions.

## 6. Does Card render a semantic article by default?

No. Its root is a generic element. Use `component="article"` or another suitable element when the content structure calls for it.

## 7. What is CardActionArea for?

It makes a primary region of the card interactive, such as linking to a detail page. Keep supplemental CardActions outside the interactive area.

## 8. Why should buttons not be placed inside CardActionArea?

That nests an interactive control inside another interactive region, creating confusing click and keyboard behavior. Put secondary actions in CardActions outside the main area.

## 9. When should I use CardHeader?

Use it when a card needs a title, optional avatar, or an action in a consistent header layout. For a simpler card, CardContent can contain a title and description.

## 10. When should I use Accordion?

Use it to let users show or hide related details when page space or scanning benefits from collapsed sections. Keep content visible when users need to compare several sections at once.

## 11. What are AccordionSummary and AccordionDetails?

AccordionSummary is the interactive heading that expands or collapses the panel. AccordionDetails contains the panel content. AccordionActions is optional and groups actions in the content.

## 12. How should I label an Accordion?

Give the summary a descriptive name that tells users what the panel contains. Avoid vague labels such as "More" or "Details" when the topic can be named directly.

## 13. How do I connect a summary with its content?

Provide an ID on AccordionSummary and set its `aria-controls` to the panel content ID. Material UI uses this relationship to expose the expanded panel.

## 14. What heading level does Accordion use by default?

The current default is an h3 heading. Use `slotProps.heading.component` to change the semantic heading element when the page structure requires a different level.

## 15. How do I set an Accordion open by default?

Use `defaultExpanded` for an uncontrolled initial state. Use the `expanded` prop with `onChange` when React should own expansion.

## 16. How can I allow only one Accordion to be open at a time?

Store the active panel ID in state and set each Accordion's `expanded` prop by comparing its ID with that state. Clear the ID when the active panel is collapsed.

## 17. Should only one Accordion be open at a time?

It depends on the content. One-open behavior can save space, while multiple open panels can help users compare or reference information.

## 18. Is Accordion content mounted when collapsed?

Yes, by default. Use `slotProps.transition.unmountOnExit` to unmount collapsed content when there is a performance reason. Consider that unmounting can discard local state.

## 19. How do I choose between a card and an accordion?

Use a Card for a focused subject that is visible as an entry point. Use an Accordion when the related content should start collapsed and can be revealed on demand.

## 20. What should I check for accessible surfaces?

Confirm the root has the right semantic element, interactive regions do not nest, card actions have clear names, and accordion headings fit the page hierarchy. Keyboard users should be able to activate and operate the controls.

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
      <td><a href="019-autocomplete.md">019 - Autocomplete</a></td>
      <td><a href="../lessons/020-paper-cards-and-accordions.md">Lesson</a></td>
      <td><a href="../examples/020-paper-cards-and-accordions.md">Example</a></td>
      <td><a href="021-lists-dividers-and-list-items.md">021 - Lists, dividers, and list items</a></td>
    </tr>
  </tbody>
</table>
