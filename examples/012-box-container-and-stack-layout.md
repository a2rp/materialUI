# Box, Container, and Stack Layout - Example

## Goal

Create a responsive team directory with a centered page width, semantic sections, one-dimensional lists, and action buttons that move to a vertical layout on small screens.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Familiarity with JSX and component props.
- The Box, Container, and Stack concepts from the matching lesson.

## Example: responsive team directory

Save this component as `App.jsx`. Render `<App />` from the existing application entry point.

```jsx
import Container from '@mui/material/Container';
import Stack from '@mui/material/Stack';
import Box from '@mui/material/Box';
import Button from '@mui/material/Button';
import Typography from '@mui/material/Typography';

const members = [
  {
    id: 'member-ada',
    name: 'Ada Lovelace',
    role: 'Product designer',
    email: 'ada@example.test',
  },
  {
    id: 'member-grace',
    name: 'Grace Hopper',
    role: 'Engineering lead',
    email: 'grace.hopper@example.test',
  },
  {
    id: 'member-katherine',
    name: 'Katherine Johnson',
    role: 'Data analyst',
    email: 'katherine.johnson@example.test',
  },
];

function MemberRow({ member }) {
  return (
    <Box
      component="li"
      sx={{
        listStyle: 'none',
        border: 1,
        borderColor: 'divider',
        borderRadius: 1,
        p: 2,
      }}
    >
      <Stack
        direction={{ xs: 'column', sm: 'row' }}
        spacing={{ xs: 1, sm: 2 }}
        useFlexGap
        sx={{
          alignItems: { xs: 'stretch', sm: 'center' },
          justifyContent: 'space-between',
        }}
      >
        <Box sx={{ minWidth: 0, flex: 1 }}>
          <Typography variant="h6" component="h3">
            {member.name}
          </Typography>
          <Typography variant="body2" color="text.secondary">
            {member.role} - {member.email}
          </Typography>
        </Box>

        <Stack
          direction={{ xs: 'column', sm: 'row' }}
          spacing={1}
          useFlexGap
        >
          <Button variant="outlined" fullWidth>
            View profile
          </Button>
          <Button variant="text" fullWidth>
            More
          </Button>
        </Stack>
      </Stack>
    </Box>
  );
}

export default function App() {
  return (
    <Container component="main" maxWidth="md" sx={{ py: { xs: 3, sm: 5 } }}>
      <Stack spacing={{ xs: 3, sm: 4 }}>
        <Box component="header">
          <Typography variant="h1" component="h1" gutterBottom>
            Team directory
          </Typography>
          <Typography color="text.secondary">
            Review the people who have access to this workspace.
          </Typography>
        </Box>

        <Box component="section" aria-labelledby="members-heading">
          <Stack spacing={2}>
            <Typography id="members-heading" variant="h2" component="h2">
              Members
            </Typography>

            <Stack
              component="ul"
              spacing={1.5}
              useFlexGap
              sx={{ m: 0, p: 0 }}
            >
              {members.map((member) => (
                <MemberRow key={member.id} member={member} />
              ))}
            </Stack>
          </Stack>
        </Box>
      </Stack>
    </Container>
  );
}
```

The example uses fictional `.test` email addresses. It displays them as sample content and does not send messages or access a real team directory.

## How the layout works

- `Container` centers the page and constrains its width.
- The outer `Stack` creates vertical spacing between the header and member section.
- `Box component="main"`, `header`, `section`, and `li` make the content structure explicit.
- The member list is a Stack rendered as a `ul`, with list styles reset in `sx`.
- Each member row switches from a column to a row at the small breakpoint.
- The action buttons use a column on small screens and a row on wider screens.
- `useFlexGap` uses CSS flexbox gap for spacing.
- `minWidth: 0` lets the text region shrink and wrap when the row is narrow.

## Expected result

The directory has a centered heading and three member rows. On a narrow viewport, each row stacks its details above the actions. On a wider viewport, details and actions share a row. Long names and email addresses can wrap without pushing the actions beyond the page boundary.

## Verification checklist

1. Confirm the directory is centered and has comfortable side gutters.
2. Inspect the rendered DOM to confirm the page has a `main`, a named section, and a list of list items.
3. Resize to a narrow viewport and verify that each member row changes to a column.
4. Resize to a wider viewport and verify that member details and actions sit side by side.
5. Check that the email strings do not cause horizontal page overflow.
6. Confirm that the buttons remain reachable and do not overlap the member text.
7. Review the list with the browser's accessibility tree or keyboard navigation.

## Practice extensions

1. Add a role or department field and decide how it should wrap at narrow widths.
2. Add an empty-state message for a directory with no members.
3. Add a responsive page action next to the title using a Stack.
4. Try removing `useFlexGap` and compare the spacing behavior.
5. Change the Container maximum width and inspect how the rows read on a large screen.

## Common mistakes

- Combining Stack spacing with child margins and getting larger or inconsistent gaps.
- Forgetting minWidth zero on a flex child that must shrink around long text.
- Assuming the sample contact details or local actions connect to a real directory or service.

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
      <td><a href="011-typography.md">011 - Typography</a></td>
      <td><a href="../lessons/012-box-container-and-stack-layout.md">Lesson</a></td>
      <td><a href="../qnas/012-box-container-and-stack-layout.md">Q&amp;A</a></td>
      <td><a href="013-grid-layout.md">013 - Grid layout</a></td>
    </tr>
  </tbody>
</table>
