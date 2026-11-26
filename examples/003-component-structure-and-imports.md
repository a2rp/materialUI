# Component Structure and Imports - Example

## Goal

Build an account card as a reusable React component. The example demonstrates direct Material UI imports, custom props, composition with children, and an anchor rendered with Button styling.

## Prerequisites

- A React project with the packages from Chapter 002 installed.
- A working development command for the project.

## File structure

Create the component folder and files:

```text
src/
  App.jsx
  components/
    AccountCard.jsx
```

## 1. Create a reusable component

Add `src/components/AccountCard.jsx`:

```jsx
import Button from '@mui/material/Button';
import Chip from '@mui/material/Chip';
import Paper from '@mui/material/Paper';
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';

export default function AccountCard({
  name,
  email,
  status,
  profileHref,
  onContact,
}) {
  const statusColor = status === 'Active' ? 'success' : 'default';

  return (
    <Paper
      component="section"
      aria-labelledby="account-name"
      sx={{ maxWidth: 520, p: 3 }}
    >
      <Stack spacing={1.5}>
        <Typography id="account-name" variant="h5" component="h2">
          {name}
        </Typography>

        <Typography color="text.secondary">{email}</Typography>

        <Chip
          label={status}
          color={statusColor}
          size="small"
          sx={{ alignSelf: 'flex-start' }}
        />

        <Stack direction="row" spacing={1}>
          <Button
            component="a"
            href={profileHref}
            variant="outlined"
          >
            View profile
          </Button>

          <Button variant="contained" onClick={onContact}>
            Contact
          </Button>
        </Stack>
      </Stack>
    </Paper>
  );
}
```

AccountCard's props are its public interface. The component does not fetch account data or decide what Contact should do; those responsibilities belong to its caller.

## 2. Compose it in the application

Replace `src/App.jsx`:

```jsx
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import AccountCard from './components/AccountCard';

export default function App() {
  function contactAccount() {
    console.info('Contact action selected');
  }

  return (
    <Stack spacing={2} sx={{ p: 3 }}>
      <Typography variant="h4" component="h1">
        Team
      </Typography>

      <AccountCard
        name="Avery Morgan"
        email="avery@example.test"
        status="Active"
        profileHref="/team/avery-morgan"
        onContact={contactAccount}
      />
    </Stack>
  );
}
```

The example address uses the reserved .test domain and is placeholder content. The local route path is illustrative; connect it to the routing system in your actual application.

## 3. Run the project

Use the development command declared in the app's package manifest. For a Vite starter:

```sh
npm run dev
```

## Expected result

The page shows a Team heading and a card with a name, email, status chip, and two controls. View profile renders as an anchor because its purpose is navigation. Contact remains a button because it triggers an action. Selecting Contact runs the callback supplied by App.

## What to notice

- Direct imports identify the public component paths.
- AccountCard accepts only the values and behavior it needs.
- The caller owns account data and the contact callback.
- Material UI components are composed through normal React children.
- Paper renders a semantic section and associates its heading through aria-labelledby.
- The component prop changes the Button root for the navigation control.
- Props are passed explicitly; component internals are not accessed by inspecting generated DOM.

## Variations

1. Add an optional subtitle prop and render it only when present.
2. Add a disabled contact state and pass it from App.
3. Add a second account card using different values to demonstrate reusability.
4. Replace the local href with your router's Link component after checking the router and Button integration.

## Common mistakes

- **Using a button for navigation:** use a link target so browser link behavior is preserved.
- **Using an anchor for an action:** an action that changes state or submits data should use a button.
- **Importing from a private implementation path:** use the documented component path.
- **Hard-coding application data in a reusable component:** pass changing data through props.
- **Dropping an event callback:** ensure the parent passes the callback and the child assigns it to the correct event prop.
- **Forgetting heading hierarchy:** choose a semantic heading level that fits the surrounding page.

## Practice check

Add a second AccountCard instance. Explain which values are props, which value selects the Chip color, and why the profile control is an anchor while the contact control is a button.

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
      <td><a href="002-project-setup-and-installation.md">002 - Project setup and installation</a></td>
      <td><a href="../lessons/003-component-structure-and-imports.md">Lesson</a></td>
      <td><a href="../qnas/003-component-structure-and-imports.md">Q&amp;A</a></td>
      <td><a href="004-themes-themeprovider-and-cssbaseline.md">004 - Themes, ThemeProvider, and CssBaseline</a></td>
    </tr>
  </tbody>
</table>
