# Chips, Avatars, and Badges: Participant Directory Example

This example combines Avatar, AvatarGroup, Chip, and Badge in a small participant directory. It includes fallback initials, a compact team preview, selectable category filters, removable active-filter Chips, and an unread notification action.

## What you will build

- A directory of people with initials, names, categories, and unread counts.
- An AvatarGroup preview that limits the number of visible participants.
- Category filter Chips with a pressed state.
- Removable Chips for active filters.
- A notification Badge that marks all notifications as read.

The example uses local sample data and does not require remote images or service credentials.

## Requirements

Create a React application with Vite, then install Material UI and its default styling engine:

```bash
npm create vite@latest mui-participant-directory -- --template react
cd mui-participant-directory
npm install
npm install @mui/material @emotion/react @emotion/styled
npm run dev
```

Replace `src/App.jsx` with the following component. The Badge wraps the notification button. Each participant's unread count is also written as text so the meaning is not carried only by a small visual marker.

## `src/App.jsx`

```jsx
import { useMemo, useState } from "react";
import {
  Avatar,
  AvatarGroup,
  Badge,
  Box,
  Button,
  Chip,
  Container,
  Divider,
  Paper,
  Stack,
  Typography,
} from "@mui/material";

const initialPeople = [
  { id: "maya", name: "Maya Chen", initials: "MC", category: "Design", title: "Product designer", unread: 3 },
  { id: "omar", name: "Omar Khan", initials: "OK", category: "Engineering", title: "Frontend engineer", unread: 0 },
  { id: "priya", name: "Priya Shah", initials: "PS", category: "Product", title: "Product manager", unread: 1 },
  { id: "leo", name: "Leo Martin", initials: "LM", category: "Engineering", title: "Platform engineer", unread: 2 },
  { id: "nina", name: "Nina Brooks", initials: "NB", category: "Design", title: "Content designer", unread: 0 },
];

const categories = ["Design", "Engineering", "Product"];

export default function App() {
  const [people, setPeople] = useState(initialPeople);
  const [activeCategories, setActiveCategories] = useState([]);

  const visiblePeople = useMemo(() => {
    if (activeCategories.length === 0) return people;
    return people.filter((person) => activeCategories.includes(person.category));
  }, [activeCategories, people]);

  const unreadTotal = people.reduce((total, person) => total + person.unread, 0);

  function toggleCategory(category) {
    setActiveCategories((current) =>
      current.includes(category)
        ? current.filter((item) => item !== category)
        : [...current, category],
    );
  }

  function removeCategory(category) {
    setActiveCategories((current) => current.filter((item) => item !== category));
  }

  function markAllRead() {
    setPeople((current) => current.map((person) => ({ ...person, unread: 0 })));
  }

  return (
    <Container maxWidth="md" sx={{ py: 5 }}>
      <Stack spacing={3}>
        <Box
          sx={{
            display: "flex",
            alignItems: "center",
            justifyContent: "space-between",
            gap: 2,
            flexWrap: "wrap",
          }}
        >
          <Box>
            <Typography component="h1" variant="h4" fontWeight={700}>
              Project participants
            </Typography>
            <Typography color="text.secondary">
              Find teammates and review recent activity.
            </Typography>
          </Box>

          <Badge badgeContent={unreadTotal} color="error" max={99}>
            <Button
              variant="outlined"
              onClick={markAllRead}
              aria-label={"Mark all notifications read, " + unreadTotal + " unread"}
            >
              Mark all read
            </Button>
          </Badge>
        </Box>

        <Paper variant="outlined" sx={{ p: 2 }}>
          <Stack
            direction={{ xs: "column", sm: "row" }}
            spacing={2}
            alignItems={{ xs: "flex-start", sm: "center" }}
          >
            <AvatarGroup max={4} aria-label="Project participants">
              {people.map((person) => (
                <Avatar key={person.id} alt={person.name}>
                  {person.initials}
                </Avatar>
              ))}
            </AvatarGroup>
            <Box>
              <Typography fontWeight={600}>{people.length} participants</Typography>
              <Typography variant="body2" color="text.secondary">
                AvatarGroup keeps this team preview compact.
              </Typography>
            </Box>
          </Stack>
        </Paper>

        <Box component="section" aria-labelledby="filter-heading">
          <Typography id="filter-heading" component="h2" variant="h6">
            Filter by category
          </Typography>
          <Stack direction="row" spacing={1} useFlexGap flexWrap="wrap" sx={{ mt: 1 }}>
            {categories.map((category) => {
              const selected = activeCategories.includes(category);
              return (
                <Chip
                  key={category}
                  label={category}
                  clickable
                  color={selected ? "primary" : "default"}
                  variant={selected ? "filled" : "outlined"}
                  aria-pressed={selected}
                  onClick={() => toggleCategory(category)}
                />
              );
            })}
          </Stack>

          {activeCategories.length > 0 && (
            <Box sx={{ mt: 2 }}>
              <Typography variant="body2" color="text.secondary" sx={{ mb: 1 }}>
                Active filters. Remove one to broaden the results.
              </Typography>
              <Stack direction="row" spacing={1} useFlexGap flexWrap="wrap">
                {activeCategories.map((category) => (
                  <Chip
                    key={category}
                    label={category}
                    color="primary"
                    onDelete={() => removeCategory(category)}
                  />
                ))}
              </Stack>
            </Box>
          )}
        </Box>

        <Divider />

        <Box component="section" aria-labelledby="people-heading">
          <Typography id="people-heading" component="h2" variant="h6" sx={{ mb: 1 }}>
            People ({visiblePeople.length})
          </Typography>

          {visiblePeople.length === 0 ? (
            <Paper variant="outlined" sx={{ p: 2 }}>
              <Typography>No participants match these filters.</Typography>
            </Paper>
          ) : (
            <Stack component="ul" spacing={1.5} sx={{ listStyle: "none", p: 0, m: 0 }}>
              {visiblePeople.map((person) => (
                <Paper component="li" variant="outlined" key={person.id} sx={{ p: 2 }}>
                  <Stack
                    direction={{ xs: "column", sm: "row" }}
                    spacing={2}
                    alignItems={{ xs: "flex-start", sm: "center" }}
                  >
                    <Badge
                      badgeContent={person.unread}
                      color="error"
                      max={9}
                      overlap="circular"
                    >
                      <Avatar alt={person.name}>{person.initials}</Avatar>
                    </Badge>

                    <Box sx={{ flexGrow: 1 }}>
                      <Typography fontWeight={600}>{person.name}</Typography>
                      <Typography variant="body2" color="text.secondary">
                        {person.title}
                      </Typography>
                      <Typography variant="caption" color="text.secondary">
                        {person.unread} unread updates
                      </Typography>
                    </Box>

                    <Chip label={person.category} size="small" variant="outlined" />
                  </Stack>
                </Paper>
              ))}
            </Stack>
          )}
        </Box>
      </Stack>
    </Container>
  );
}
```

## `src/main.jsx`

Vite's starter entry file can render the component like this:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

## How it works

### Avatar fallback

Each Avatar receives the participant's name as `alt` and initials as children. Initials remain visible when no image URL is available. A real application can provide `src` while keeping fallback children.

### AvatarGroup overflow

The `max` prop limits the number of visible participants in the summary. The full participant list remains below, so the compact preview is not the only way to identify team members.

### Selectable and removable Chips

Category Chips call `toggleCategory`. Their `aria-pressed` value exposes the selected state. A second row renders active categories as deletable Chips; `onDelete` updates the same state used by the filter.

### Badge and notification action

The top Badge wraps a single Button. Its accessible name includes the action and unread count. Activating the button resets unread values in React state. Participant rows also show unread counts as text.

## Try these changes

1. Select Design and Engineering. The list shows participants in either selected category.
2. Remove Design from the active-filter Chips. The filter and results update together.
3. Select every category, then remove one. Other active selections remain.
4. Activate Mark all read. The Badge count hides at zero and each participant's text count updates.
5. Change AvatarGroup's `max` value and observe the preview.
6. Add an image `src` to one Avatar and keep its initials as fallback content.

## Common mistakes

- Setting `onDelete` without updating the state that renders the Chip.
- Using array indexes as keys for removable participant lists.
- Showing a selected filter visually without exposing its state.
- Treating an Avatar as the only source of a person's name.
- Hiding the full team list behind AvatarGroup overflow.
- Relying on Badge color or a dot alone to communicate an important count.
- Nesting interactive controls inside a Chip or Button.

## Chapter navigation

<table>
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
      <td><a href="028-progress-indicators-and-skeletons.md">028 - Progress indicators and skeletons</a></td>
      <td><a href="../lessons/029-chips-avatars-and-badges.md">Lesson</a></td>
      <td><a href="../qnas/029-chips-avatars-and-badges.md">Q&amp;A</a></td>
      <td><a href="030-tables-and-data-display.md">030 - Tables and data display</a></td>
    </tr>
  </tbody>
</table>
