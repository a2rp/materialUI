# Paper, Cards, and Accordions

## Learning goals

After this chapter, you should be able to:

- Choose Paper for a surface, Card for a focused topic, and Accordion for collapsible details.
- Use elevation and outlines to show surface hierarchy.
- Build a card with content, a main action area, and separate supplemental actions.
- Control an accordion when application state needs to own expansion.
- Preserve semantic headings and accessible expansion behavior.

## Choose a surface by purpose

| Component | Main purpose |
| --- | --- |
| \`Paper\` | A generic surface with theme elevation or an outline |
| \`Card\` | A compact surface for one subject and its related actions |
| \`Accordion\` | A section that users can expand or collapse |

A Box is a general wrapper. Paper adds a surface treatment. Card adds a content structure. Accordion adds expandable behavior. Choose the component whose purpose matches the interface.

## Paper surfaces

Paper represents a surface that sits above or apart from its background. Its \`elevation\` prop uses the theme's shadow scale from 0 to 24:

```jsx
import Paper from '@mui/material/Paper';

<Paper elevation={2} sx={{ p: 3 }}>
  A raised surface for related content.
</Paper>
```

Use a small set of elevation levels to create hierarchy. Avoid giving every surface a different shadow, which makes the interface feel noisy.

Set \`variant="outlined"\` for a flat border without elevation:

```jsx
<Paper variant="outlined" sx={{ p: 2 }}>
  An outlined panel that does not rely on a shadow.
</Paper>
```

Paper renders a div by default. Use the \`component\` prop when the content needs a semantic element:

```jsx
<Paper component="section" aria-labelledby="summary-heading">
  <h2 id="summary-heading">Summary</h2>
  ...
</Paper>
```

Paper is best for a surface treatment. If you only need spacing or semantic grouping, Box or Container may be a clearer choice.

## Card structure

Card groups content and actions about one subject. It can contain a header, media, content, and actions:

```jsx
import Card from '@mui/material/Card';
import CardContent from '@mui/material/CardContent';
import CardActions from '@mui/material/CardActions';
import Typography from '@mui/material/Typography';

<Card component="article" variant="outlined">
  <CardContent>
    <Typography variant="h2" component="h2">
      Workspace plan
    </Typography>
    <Typography color="text.secondary">
      Review member limits and billing settings.
    </Typography>
  </CardContent>
  <CardActions>
    <Button>View plan</Button>
    <Button>Manage billing</Button>
  </CardActions>
</Card>
```

Use CardHeader for a common title and optional avatar or action. Use CardMedia for an image or video, and provide appropriate alternative text or mark purely decorative media accordingly. CardContent provides consistent internal spacing.

Cards are entry points to more detailed information, not a reason to nest every control inside a box. Keep the content focused on one topic.

## CardActionArea and supplemental actions

Use CardActionArea when the card's main content is one interactive destination or action:

```jsx
<Card component="article">
  <CardActionArea component="a" href="/projects/atlas">
    <CardContent>
      <Typography variant="h2" component="h2">
        Atlas project
      </Typography>
      <Typography color="text.secondary">
        Review project activity and assigned tasks.
      </Typography>
    </CardContent>
  </CardActionArea>
  <CardActions>
    <Button>More options</Button>
  </CardActions>
</Card>
```

Keep supplemental actions outside CardActionArea. Do not place one button or link inside another interactive region. That creates confusing keyboard and click behavior.

When the card itself is not a single destination, use Card for layout and place actions separately without making the entire surface look clickable.

## Accordions

An Accordion groups a summary with content that can be shown or hidden. It consists of Accordion, AccordionSummary, and AccordionDetails, with optional AccordionActions:

```jsx
import Accordion from '@mui/material/Accordion';
import AccordionDetails from '@mui/material/AccordionDetails';
import AccordionSummary from '@mui/material/AccordionSummary';
import Typography from '@mui/material/Typography';

<Accordion>
  <AccordionSummary
    id="billing-summary"
    aria-controls="billing-content"
  >
    <Typography component="span">Billing details</Typography>
  </AccordionSummary>
  <AccordionDetails>
    <Typography>
      Review the billing contact and payment schedule.
    </Typography>
  </AccordionDetails>
</Accordion>
```

The summary is the interactive control. Give it a clear name that explains what content expands. Material UI uses the summary's ID and \`aria-controls\` to connect the control and content.

The default heading level may not match every page. In current v9, use \`slotProps.heading.component\` to set the heading element that fits the document hierarchy:

```jsx
<Accordion slotProps={{ heading: { component: 'h3' } }}>
  ...
</Accordion>
```

Choose heading levels in sequence. Do not skip levels simply to get a particular font size.

## Controlled and uncontrolled expansion

An uncontrolled Accordion manages its own expansion. Set \`defaultExpanded\` for an initial open state.

A controlled Accordion receives its state from React and reports changes through \`onChange\`:

```jsx
const [expanded, setExpanded] = useState(false);

<Accordion
  expanded={expanded}
  onChange={(event, nextExpanded) => setExpanded(nextExpanded)}
>
  <AccordionSummary id="access-summary" aria-controls="access-content">
    Access settings
  </AccordionSummary>
  <AccordionDetails>
    ...
  </AccordionDetails>
</Accordion>
```

Use controlled expansion when another part of the application needs to know which panel is open, or when panels must coordinate.

## Only one accordion open at a time

Store the expanded panel's ID to allow only one panel open at a time:

```jsx
const [expandedPanel, setExpandedPanel] = useState(null);

function handlePanelChange(panelId) {
  return (event, isExpanded) => {
    setExpandedPanel(isExpanded ? panelId : null);
  };
}

<Accordion
  expanded={expandedPanel === 'profile'}
  onChange={handlePanelChange('profile')}
>
  ...
</Accordion>

<Accordion
  expanded={expandedPanel === 'security'}
  onChange={handlePanelChange('security')}
>
  ...
</Accordion>
```

Use this behavior only when the information architecture benefits from a single open panel. Allowing multiple panels to remain open may be easier for users who need to compare details.

## Accordion content and performance

Accordion content is mounted even when collapsed by default. This supports server rendering and keeps content available in the document. If a panel contains a large component tree or many expensive panels, you can unmount collapsed content with the transition slot:

```jsx
<Accordion slotProps={{ transition: { unmountOnExit: true } }}>
  ...
</Accordion>
```

Unmounting can discard local component state and may affect server-rendered content. Use it only when the performance tradeoff is intentional.

## Common mistakes

- Using Paper when only a generic wrapper is needed.
- Using elevation to decorate every panel instead of establishing a clear hierarchy.
- Making a card's entire body clickable while also nesting buttons inside the action area.
- Omitting semantic structure because a Card or Paper renders a div by default.
- Using an Accordion for content that should remain visible or is needed to compare several sections.
- Giving a summary a vague label such as "Details."
- Skipping heading levels to achieve a visual size.
- Disabling Accordion content mounting without considering local state and rendering behavior.
- Forgetting that CardActionArea should not wrap supplemental actions.

## Recap

Paper provides a theme surface, Card groups one subject and its related actions, and Accordion reveals or hides details. Use semantic root elements, keep interactive regions separate, and choose controlled expansion only when the surrounding application needs to own that state.

## Practice task

Create a project overview card with one main link and one separate action. Add an FAQ with three accessible accordions. Set heading levels to match the page and decide whether more than one panel should be open at a time.

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
      <td><a href="019-autocomplete.md">019 - Autocomplete</a></td>
      <td><a href="../qnas/020-paper-cards-and-accordions.md">Q&amp;A</a></td>
      <td><a href="../examples/020-paper-cards-and-accordions.md">Example</a></td>
      <td><a href="021-lists-dividers-and-list-items.md">021 - Lists, dividers, and list items</a></td>
    </tr>
  </tbody>
</table>
