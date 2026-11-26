# Paper, Cards, and Accordions - Example

## Goal

Build a project overview page with an outlined Paper section, a card whose main area is a link, a separate card action, and a controlled FAQ with one open panel at a time.

## Prerequisites

- A React application with Material UI and Emotion installed.
- The Material UI icon package installed for the accordion expand icon.
- Familiarity with React state and the Paper, Card, and Accordion components.

## Example: project overview and FAQ

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Accordion from '@mui/material/Accordion';
import AccordionDetails from '@mui/material/AccordionDetails';
import AccordionSummary from '@mui/material/AccordionSummary';
import Button from '@mui/material/Button';
import Card from '@mui/material/Card';
import CardActionArea from '@mui/material/CardActionArea';
import CardActions from '@mui/material/CardActions';
import CardContent from '@mui/material/CardContent';
import Container from '@mui/material/Container';
import ExpandMoreIcon from '@mui/icons-material/ExpandMore';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

const faqItems = [
  {
    id: 'access',
    title: 'Who can access this project?',
    body: 'Project access is managed by its owners. Review the member list before sharing sensitive work.',
  },
  {
    id: 'updates',
    title: 'How are updates shared?',
    body: 'Project updates appear in the activity feed. Notification preferences can be managed separately.',
  },
  {
    id: 'archive',
    title: 'Can I archive the project?',
    body: 'An owner can archive a completed project. Archiving hides it from active work views but preserves its history.',
  },
];

export default function App() {
  const [expandedPanel, setExpandedPanel] = useState(null);
  const [saved, setSaved] = useState(false);

  function handlePanelChange(panelId) {
    return (event, isExpanded) => {
      setExpandedPanel(isExpanded ? panelId : null);
    };
  }

  return (
    <Container component="main" maxWidth="md" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={4}>
        <Paper
          component="section"
          variant="outlined"
          aria-labelledby="overview-heading"
          sx={{ p: { xs: 2, sm: 3 } }}
        >
          <Stack spacing={1}>
            <Typography id="overview-heading" variant="h1" component="h1">
              Project overview
            </Typography>
            <Typography color="text.secondary">
              Review a project or find answers to common questions.
            </Typography>
          </Stack>
        </Paper>

        <Card component="article" variant="outlined">
          <CardActionArea component="a" href="/projects/atlas">
            <CardContent>
              <Typography variant="h2" component="h2" gutterBottom>
                Atlas project
              </Typography>
              <Typography color="text.secondary">
                A workspace for planning research and product delivery.
              </Typography>
            </CardContent>
          </CardActionArea>

          <CardActions>
            <Button onClick={() => setSaved((current) => !current)}>
              {saved ? 'Remove saved project' : 'Save project'}
            </Button>
          </CardActions>
        </Card>

        <Stack
          component="section"
          spacing={1}
          aria-labelledby="faq-heading"
        >
          <Typography id="faq-heading" variant="h2" component="h2">
            Frequently asked questions
          </Typography>

          {faqItems.map((item) => (
            <Accordion
              key={item.id}
              expanded={expandedPanel === item.id}
              onChange={handlePanelChange(item.id)}
              slotProps={{ heading: { component: 'h3' } }}
            >
              <AccordionSummary
                id={item.id + '-summary'}
                aria-controls={item.id + '-content'}
                expandIcon={<ExpandMoreIcon />}
              >
                <Typography component="span">{item.title}</Typography>
              </AccordionSummary>
              <AccordionDetails>
                <Typography>{item.body}</Typography>
              </AccordionDetails>
            </Accordion>
          ))}
        </Stack>

        <Typography role="status" aria-live="polite" color="text.secondary">
          {saved ? 'Atlas project added to your saved list.' : ''}
        </Typography>
      </Stack>
    </Container>
  );
}
```

The project route and FAQ answers are example content. The project route should exist in the host application for the card link to navigate.

## How the example works

- Paper provides a flat outlined surface for the page introduction.
- Card groups information about one project.
- CardActionArea makes the main card content a navigation link.
- CardActions contains a separate button, outside the link region.
- The FAQ uses controlled expansion state, so at most one panel is open.
- The section and heading elements follow a clear page hierarchy.
- Accordion summaries have unique IDs and matching `aria-controls` values.

## Expected result

The page shows an outlined introduction, a project card, and three FAQ rows. Selecting an FAQ summary opens that panel and closes the previously open panel. The project title and description navigate to the project route, while the save action updates a local status message.

## Verification checklist

1. Confirm the project card's main area is a link and the save action is a separate button.
2. Tab to both card interactions and verify their focus states are visible.
3. Open each FAQ panel and confirm that only one remains expanded.
4. Verify every accordion summary has a descriptive label and a unique ID.
5. Confirm the FAQ headings are h3 elements below the h2 section heading.
6. Use keyboard activation to expand and collapse each accordion.
7. Confirm the save message is announced and the page makes no claim that it stored data remotely.

## Practice extensions

1. Change the FAQ state so multiple panels may remain open and compare the interaction.
2. Add a CardHeader with a compact owner summary.
3. Add CardMedia and provide appropriate alternative text for meaningful images.
4. Add a second Paper surface and decide whether it needs an outline or elevation.
5. Make the project route work with the application's router component.
6. Add a long FAQ answer and inspect reading order and scrolling behavior.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous example</th>
      <th>Chapter lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Next example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="019-autocomplete.md">019 - Autocomplete</a></td>
      <td><a href="../lessons/020-paper-cards-and-accordions.md">Lesson</a></td>
      <td><a href="../qnas/020-paper-cards-and-accordions.md">Q&amp;A</a></td>
      <td><a href="021-lists-dividers-and-list-items.md">021 - Lists, dividers, and list items</a></td>
    </tr>
  </tbody>
</table>
