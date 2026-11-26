# Autocomplete

## Learning goals

After this chapter, you should be able to:

- Choose Autocomplete for a searchable or free-text option field.
- Render it with a correctly labelled TextField.
- Control selected value and typed input independently.
- Work with object options and stable equality.
- Support multiple values, free solo input, grouping, and remote results.
- Preserve keyboard interaction when customizing options.

## What Autocomplete is for

Autocomplete enhances a text input with a popup of suggestions. Use it when users benefit from searching a list or when a typed value may be added as free text.

Choose it over Select when the options are long, searchable, or asynchronously loaded. Use Select for a short fixed list. Use a dedicated component when the interaction needs more complex data selection or a specialized picker.

## Basic Autocomplete

The `renderInput` callback must render a TextField and pass through the parameters it receives. Those parameters include the wiring required for focus, keyboard behavior, popup state, and accessibility:

```jsx
import Autocomplete from '@mui/material/Autocomplete';
import TextField from '@mui/material/TextField';

const cities = ['Delhi', 'Mumbai', 'Pune', 'Kolkata'];

<Autocomplete
  options={cities}
  renderInput={(params) => (
    <TextField {...params} label="City" />
  )}
/>
```

Do not discard or replace `params` when customizing the input. The field should also have a useful visible label.

## Object options

An option can be an object. Use `getOptionLabel` to choose the text shown in the field and popup:

```jsx
const people = [
  { id: 'person-ada', name: 'Ada Lovelace', team: 'Research' },
  { id: 'person-grace', name: 'Grace Hopper', team: 'Engineering' },
];

<Autocomplete
  options={people}
  getOptionLabel={(person) => person.name}
  renderInput={(params) => (
    <TextField {...params} label="Owner" />
  )}
/>
```

By default, the selected value is the option object. Matching uses strict equality unless you provide another comparison method. If options are recreated or loaded as new objects, use a stable matching rule:

```jsx
<Autocomplete
  options={people}
  value={selectedPerson}
  getOptionLabel={(person) => person.name}
  isOptionEqualToValue={(option, value) => option.id === value.id}
  onChange={(event, nextPerson) => setSelectedPerson(nextPerson)}
  renderInput={(params) => (
    <TextField {...params} label="Owner" />
  )}
/>
```

When labels can repeat, provide `getOptionKey` so each option has a unique key.

## Current v9 option values

Material UI v9 supports `getOptionValue` for applications that want the selected value to be a primitive such as an ID instead of the full option object:

```jsx
<Autocomplete
  options={people}
  getOptionLabel={(person) => person.name}
  getOptionValue={(person) => person.id}
  value={selectedPersonId}
  onChange={(event, nextPersonId) => setSelectedPersonId(nextPersonId)}
  renderInput={(params) => (
    <TextField {...params} label="Owner" />
  )}
/>
```

The value function must return a unique, non-null primitive for every option. Callbacks such as `getOptionLabel` and `renderOption` still receive the original option object.

If the application stores the whole object, leave `getOptionValue` out and use object values with a stable equality comparison where needed. Keep the chosen value model consistent with the rest of the form.

## Selected value and input text are separate

Autocomplete has two related but independent states:

- `value` is the selected option or options.
- `inputValue` is the text currently in the textbox.

Control them separately when the application needs to react to both:

```jsx
const [selectedCity, setSelectedCity] = useState(null);
const [cityText, setCityText] = useState('');

<Autocomplete
  options={cities}
  value={selectedCity}
  inputValue={cityText}
  onChange={(event, nextCity) => setSelectedCity(nextCity)}
  onInputChange={(event, nextText) => setCityText(nextText)}
  renderInput={(params) => (
    <TextField {...params} label="City" />
  )}
/>
```

Do not create a new array or object for the controlled value on every render unless the value actually changed. Keep controlled values stable, for example by storing them in state or memoizing derived values.

## Multiple selections

Set `multiple` when the user may choose several options. The value is an array:

```jsx
<Autocomplete
  multiple
  options={people}
  getOptionLabel={(person) => person.name}
  value={selectedPeople}
  onChange={(event, nextPeople) => setSelectedPeople(nextPeople)}
  renderInput={(params) => (
    <TextField {...params} label="Project members" />
  )}
/>
```

By default, selected options are represented with chips. Use `limitTags` when the field needs to show only a few chips while unfocused.

## Custom selected-value rendering

The `renderValue` prop customizes how selected value or values appear in the input. Use the provided item props when rendering interactive value elements so that focus and removal behavior continue to work:

```jsx
<Autocomplete
  multiple
  options={people}
  getOptionLabel={(person) => person.name}
  renderValue={(selected, getItemProps) =>
    selected.map((person, index) => (
      <Chip
        key={person.id}
        label={person.name}
        {...getItemProps({ index })}
      />
    ))
  }
  renderInput={(params) => (
    <TextField {...params} label="Project members" />
  )}
/>
```

Keep the selected values readable and removable. A custom renderer should preserve the props returned by `getItemProps`.

## Free solo text

Set `freeSolo` when users may enter text that is not one of the suggestions:

```jsx
<Autocomplete
  freeSolo
  options={cities}
  renderInput={(params) => (
    <TextField {...params} label="Search or enter a city" />
  )}
/>
```

In free-solo mode, a value created by typing is a string, even when the options are objects. Handle both types in functions such as `getOptionLabel`:

```jsx
<Autocomplete
  freeSolo
  options={people}
  getOptionLabel={(person) =>
    typeof person === 'string' ? person : person.name
  }
  renderInput={(params) => (
    <TextField {...params} label="Owner or new name" />
  )}
/>
```

If users are creating a new record rather than searching, make that behavior explicit. Treat arbitrary text as untrusted input and validate it before saving.

## Grouped options

Use `groupBy` to group options. Sort the options by the same grouping value so each group appears once:

```jsx
const groupedCities = [
  { name: 'Delhi', region: 'North' },
  { name: 'Mumbai', region: 'West' },
  { name: 'Pune', region: 'West' },
  { name: 'Kolkata', region: 'East' },
];

<Autocomplete
  options={groupedCities}
  groupBy={(city) => city.region}
  getOptionLabel={(city) => city.name}
  renderInput={(params) => (
    <TextField {...params} label="City" />
  )}
/>
```

If duplicate labels exist, still give each option a unique key. Group labels should help users scan rather than duplicate information already present in every option.

## Remote and asynchronous options

For remote search:

1. Control the input text.
2. Debounce requests so each keystroke does not start an unnecessary request.
3. Ignore or cancel stale responses so an older query cannot replace newer results.
4. Use `loading` and `loadingText` while results are pending.
5. Show a helpful empty state when no results match.
6. Keep the field label and status messages available to assistive technology.

When the server already filters results, configure the component not to apply a second local filter. Keep network errors distinguishable from a valid empty result.

## Rendering options and disabled entries

Use `getOptionDisabled` when some options cannot be selected. Use `renderOption` only when the default label is not enough. Spread the provided option props onto the rendered option element because they carry the component's focus and selection behavior:

```jsx
<Autocomplete
  options={people}
  getOptionLabel={(person) => person.name}
  getOptionDisabled={(person) => !person.active}
  renderOption={(props, person) => (
    <li {...props}>
      {person.name}
      {!person.active ? ' (inactive)' : ''}
    </li>
  )}
  renderInput={(params) => (
    <TextField {...params} label="Owner" />
  )}
/>
```

Do not replace the option's keyboard behavior with a clickable non-interactive element.

## Accessibility and keyboard use

Autocomplete manages an input and a list of suggestions. Preserve the input parameters and option props provided by the component. Keep the input labelled, make suggestions distinguishable, and verify keyboard operation for opening the popup, moving through choices, selecting, and clearing.

Use a label that explains what is being selected. If helper text gives important instructions, connect it to the field. Do not rely on color alone to indicate disabled or selected options.

## Common mistakes

- Failing to spread the parameters received by `renderInput`.
- Recreating controlled object or array values on every render.
- Relying on strict object equality after options are reloaded as new objects.
- Forgetting that free-solo input can be a string even when options are objects.
- Assuming the typed `inputValue` is the same as the selected `value`.
- Grouping options without sorting by the same group key.
- Omitting unique keys when option labels repeat.
- Replacing the props supplied to a custom rendered option.
- Failing to handle loading, empty, and server-error states in remote search.
- Using Autocomplete for a short fixed list that Select would express more simply.

## Recap

Autocomplete combines a text input with suggestions. Keep selected value and input text separate when controlling them. Use stable values and matching rules for objects, handle strings in free-solo mode, and preserve the props used for rendering the input and options. For remote options, manage request timing and loading states.

## Practice task

Build an owner field from object options with IDs and names. Control both selected value and typed text, support a multiple-members version, and add a free-solo search field. Verify the labels, keyboard selection, object matching after reloading options, and the empty and loading states.

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
      <td><a href="018-form-control-validation-and-accessibility.md">018 - Form control, validation, and accessibility</a></td>
      <td><a href="../qnas/019-autocomplete.md">Q&amp;A</a></td>
      <td><a href="../examples/019-autocomplete.md">Example</a></td>
      <td><a href="020-paper-cards-and-accordions.md">020 - Paper, cards, and accordions</a></td>
    </tr>
  </tbody>
</table>
