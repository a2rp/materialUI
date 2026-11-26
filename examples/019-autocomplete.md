# Autocomplete - Example

## Goal

Create a project assignment form with a searchable owner field, multiple member selection, and a free-solo skills field.

## Prerequisites

- A React application with Material UI and Emotion installed.
- Material UI v9 for the primitive `getOptionValue` mapping used below.
- Familiarity with React state and Autocomplete's separate value and input text states.

## Example: project assignment

Save this as `App.jsx` and render `<App />` from the existing application entry point.

```jsx
import { useState } from 'react';
import Autocomplete from '@mui/material/Autocomplete';
import Button from '@mui/material/Button';
import Container from '@mui/material/Container';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import TextField from '@mui/material/TextField';
import Typography from '@mui/material/Typography';

const people = [
  {
    id: 'person-ada',
    name: 'Ada Lovelace',
    team: 'Research',
    active: true,
  },
  {
    id: 'person-grace',
    name: 'Grace Hopper',
    team: 'Engineering',
    active: true,
  },
  {
    id: 'person-katherine',
    name: 'Katherine Johnson',
    team: 'Data',
    active: true,
  },
  {
    id: 'person-dorothy',
    name: 'Dorothy Vaughan',
    team: 'Research',
    active: false,
  },
];

const skillOptions = [
  'Accessibility',
  'Design systems',
  'React',
  'TypeScript',
];

export default function App() {
  const [ownerId, setOwnerId] = useState(null);
  const [ownerQuery, setOwnerQuery] = useState('');
  const [members, setMembers] = useState([people[1]]);
  const [skills, setSkills] = useState(['React']);
  const [message, setMessage] = useState('');

  const selectedOwner =
    people.find((person) => person.id === ownerId) || null;

  function handleSubmit(event) {
    event.preventDefault();

    const ownerName = selectedOwner ? selectedOwner.name : 'no owner';

    setMessage(
      'Assignment prepared with ' +
      ownerName +
      ', ' +
      members.length +
      ' members, and ' +
      skills.length +
      ' skills.'
    );
  }

  return (
    <Container component="main" maxWidth="sm" sx={{ py: { xs: 3, sm: 5 } }}>
      <Paper
        component="form"
        variant="outlined"
        onSubmit={handleSubmit}
        aria-labelledby="assignment-heading"
        sx={{ p: { xs: 2, sm: 3 } }}
      >
        <Stack spacing={2.5}>
          <Stack spacing={0.5}>
            <Typography id="assignment-heading" variant="h1" component="h1">
              Project assignment
            </Typography>
            <Typography color="text.secondary">
              Find an owner, add team members, and describe the required skills.
            </Typography>
          </Stack>

          <Autocomplete
            options={people}
            value={ownerId}
            inputValue={ownerQuery}
            onChange={(event, nextOwnerId) => {
              setOwnerId(nextOwnerId);
              setMessage('');
            }}
            onInputChange={(event, nextQuery) => {
              setOwnerQuery(nextQuery);
            }}
            getOptionLabel={(person) => person.name}
            getOptionValue={(person) => person.id}
            getOptionKey={(person) => person.id}
            getOptionDisabled={(person) => !person.active}
            noOptionsText="No matching people"
            renderOption={(props, person) => (
              <li {...props}>
                <Stack spacing={0.25}>
                  <Typography variant="body1">{person.name}</Typography>
                  <Typography variant="caption" color="text.secondary">
                    {person.team}
                    {!person.active ? ' � Inactive' : ''}
                  </Typography>
                </Stack>
              </li>
            )}
            renderInput={(params) => (
              <TextField
                {...params}
                label="Project owner"
                helperText="Search by name. Inactive people cannot be selected."
              />
            )}
            fullWidth
          />

          <Autocomplete
            multiple
            options={people.filter((person) => person.active)}
            value={members}
            onChange={(event, nextMembers) => {
              setMembers(nextMembers);
              setMessage('');
            }}
            getOptionLabel={(person) => person.name}
            getOptionKey={(person) => person.id}
            isOptionEqualToValue={(option, value) => option.id === value.id}
            renderInput={(params) => (
              <TextField
                {...params}
                label="Project members"
                helperText="Choose one or more active members."
              />
            )}
            fullWidth
          />

          <Autocomplete
            multiple
            freeSolo
            options={skillOptions}
            value={skills}
            onChange={(event, nextSkills) => {
              setSkills(nextSkills);
              setMessage('');
            }}
            renderInput={(params) => (
              <TextField
                {...params}
                label="Required skills"
                helperText="Choose suggestions or type a new skill and press Enter."
              />
            )}
            fullWidth
          />

          <Button type="submit" variant="contained">
            Prepare assignment
          </Button>

          <Typography role="status" aria-live="polite" color="text.secondary">
            {message}
          </Typography>
        </Stack>
      </Paper>
    </Container>
  );
}
```

The people and skills are illustrative local data. The form does not contact a server or create a real project assignment.

## What the example demonstrates

- The owner field stores a primitive person ID using Material UI v9's `getOptionValue`.
- The owner's selected value and typed search text use separate state.
- Option rows show a person's name, team, and active state.
- Inactive people appear in the suggestion list but cannot be selected.
- The members field allows multiple object values and compares them by stable ID.
- The skills field allows multiple suggestions and arbitrary free-solo text.
- Each input receives the parameters provided through `renderInput`.
- The form has visible labels and helper text for all three fields.

## Expected result

Typing in Project owner filters the people. Inactive people appear disabled. Project members displays selected people as chips. Required skills accepts the predefined choices and new text values. Submitting the form shows a local status message with the selected owner and counts.

## Verification checklist

1. Search for a person's name and select an active owner.
2. Confirm that the owner is stored as an ID while the field displays the person's name.
3. Search for Dorothy Vaughan and confirm the inactive option cannot be selected.
4. Add and remove multiple members and verify the selected chips.
5. Type a new skill and press Enter to add it.
6. Use keyboard navigation to open the suggestions, move between options, select one, and clear the field.
7. Submit the form and confirm the status message reflects the selections.
8. Confirm that the example makes no claim that a project was actually saved.

## Practice extensions

1. Add an asynchronous owner search and handle loading, empty, and error states.
2. Debounce input changes and ignore stale search responses.
3. Add categories with `groupBy` and sort the options by the same category.
4. Replace free-solo skills with a validated create flow if arbitrary skills should not be accepted.
5. Add a selected-value display using `renderValue` and preserve the item props it supplies.
6. Add form validation that explains when an owner or skill is required.

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
      <td><a href="018-form-control-validation-and-accessibility.md">018 - Form control, validation, and accessibility</a></td>
      <td><a href="../lessons/019-autocomplete.md">Lesson</a></td>
      <td><a href="../qnas/019-autocomplete.md">Q&amp;A</a></td>
      <td><a href="020-paper-cards-and-accordions.md">020 - Paper, cards, and accordions</a></td>
    </tr>
  </tbody>
</table>
