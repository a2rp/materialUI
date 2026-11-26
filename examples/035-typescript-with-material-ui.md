# TypeScript with Material UI: Custom Theme and Project Form Example

This example adds a typed brand color and a soft Button variant, then uses them in a controlled project form. It includes the module augmentation file, runtime theme, form component, and React entry point.

## What you will build

- A custom brand palette color with TypeScript declarations.
- A custom soft Button variant with matching runtime styles.
- A typed form callback.
- A controlled TextField with validation feedback.
- A list of submitted project names.

## Requirements

Start from a React TypeScript project. Install Material UI and its default styling engine if they are not already present:

```bash
npm install @mui/material @emotion/react @emotion/styled
```

## `src/mui.d.ts`

Add the module augmentations in a declaration file included by tsconfig:

```ts
import "@mui/material/styles";
import "@mui/material/Button";

declare module "@mui/material/styles" {
  interface Palette {
    brand: Palette["primary"];
  }

  interface PaletteOptions {
    brand?: PaletteOptions["primary"];
  }
}

declare module "@mui/material/Button" {
  interface ButtonPropsColorOverrides {
    brand: true;
  }

  interface ButtonPropsVariantOverrides {
    soft: true;
  }
}
```

These declarations tell TypeScript that the theme and Button support the new names. They do not add runtime colors or styles by themselves.

## `src/theme.ts`

Create the matching runtime theme:

```ts
import { createTheme } from "@mui/material/styles";

export const theme = createTheme({
  palette: {
    brand: {
      main: "#2457C5",
      light: "#E8EEFC",
      dark: "#173B8A",
      contrastText: "#FFFFFF",
    },
  },
  components: {
    MuiButton: {
      defaultProps: {
        disableElevation: true,
      },
      styleOverrides: {
        root: {
          textTransform: "none",
          variants: [
            {
              props: { variant: "soft", color: "brand" },
              style: {
                backgroundColor: "#E8EEFC",
                color: "#173B8A",
              },
            },
          ],
        },
      },
    },
  },
});
```

## `src/App.tsx`

The form owns its text value, validates before calling its typed callback, and displays submitted projects:

```tsx
import { useState, type FormEvent } from "react";
import {
  Alert,
  Box,
  Button,
  Container,
  Paper,
  Stack,
  TextField,
  Typography,
} from "@mui/material";
import { ThemeProvider } from "@mui/material/styles";
import { theme } from "./theme";

type ProjectFormProps = {
  onCreateProject: (projectName: string) => void;
};

function ProjectForm({ onCreateProject }: ProjectFormProps) {
  const [projectName, setProjectName] = useState("");
  const [error, setError] = useState("");

  function handleSubmit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    const normalizedName = projectName.trim();
    if (!normalizedName) {
      setError("Enter a project name.");
      return;
    }

    onCreateProject(normalizedName);
    setProjectName("");
    setError("");
  }

  return (
    <Box component="form" onSubmit={handleSubmit}>
      <Stack spacing={2}>
        <TextField
          label="Project name"
          value={projectName}
          onChange={(event) => {
            setProjectName(event.target.value);
            if (error) setError("");
          }}
          error={Boolean(error)}
          helperText={error || "Use a short name that identifies the project."}
          fullWidth
        />

        <Button
          type="submit"
          color="brand"
          variant="soft"
          disabled={!projectName.trim()}
        >
          Create project
        </Button>
      </Stack>
    </Box>
  );
}

export default function App() {
  const [projects, setProjects] = useState<string[]>([]);

  function addProject(projectName: string) {
    setProjects((current) => [...current, projectName]);
  }

  return (
    <ThemeProvider theme={theme}>
      <Container maxWidth="sm" sx={{ py: 5 }}>
        <Stack spacing={3}>
          <Box>
            <Typography component="h1" variant="h4" fontWeight={700}>
              Project setup
            </Typography>
            <Typography color="text.secondary">
              Create a project using the typed brand theme.
            </Typography>
          </Box>

          <Paper variant="outlined" sx={{ p: 3 }}>
            <ProjectForm onCreateProject={addProject} />
          </Paper>

          {projects.length > 0 ? (
            <Box component="section" aria-labelledby="projects-heading">
              <Typography id="projects-heading" component="h2" variant="h6">
                Created projects
              </Typography>
              <Stack component="ul" spacing={1} sx={{ pl: 3 }}>
                {projects.map((project, index) => (
                  <Typography component="li" key={project + "-" + index}>
                    {project}
                  </Typography>
                ))}
              </Stack>
            </Box>
          ) : (
            <Alert severity="info">No projects created yet.</Alert>
          )}
        </Stack>
      </Container>
    </ThemeProvider>
  );
}
```

The list combines each name with its position only for this append-only demonstration, where duplicate names are allowed. In a real application, use a stable unique project ID from the data layer.

## `src/main.tsx`

Mount the application with React StrictMode:

```tsx
import { StrictMode } from "react";
import { createRoot } from "react-dom/client";
import App from "./App";

createRoot(document.getElementById("root")!).render(
  <StrictMode>
    <App />
  </StrictMode>,
);
```

## How the example works

### Declaration and runtime theme

The declaration file extends the Palette, PaletteOptions, and Button prop types. The theme then supplies brand color tokens and the style rule for the soft variant. Both layers are required: types alone do not create runtime styles, and runtime values alone do not update TypeScript's accepted prop names.

### Controlled form

ProjectForm owns the field value and exposes a callback that accepts a string. It does not expose a Material UI ChangeEvent to its parent. The form trims whitespace and refuses to submit an empty value.

### Inferred event type

TextField's onChange callback provides an inferred event type. The form submit handler uses FormEvent<HTMLFormElement> because it is a form-level event that belongs to the component's own function signature.

### Theme provider

ThemeProvider wraps the controls that use the custom theme. Components outside that provider continue to use their nearest provider or the default theme.

## Try these changes

1. Change the brand palette tokens and observe the custom Button.
2. Remove the ButtonPropsVariantOverrides declaration and inspect the type error.
3. Keep the declaration but remove the variant style and observe that the type still compiles without the intended appearance.
4. Change the form callback to accept a structured object and update the parent handler.
5. Add a .d.ts file outside the configured tsconfig include path and see why the augmentation is not applied.

## Common mistakes

- Defining a custom color in the theme without augmenting Palette and PaletteOptions.
- Augmenting the Button prop but forgetting to add the runtime palette color.
- Declaring a custom variant without adding its theme style.
- Putting mui.d.ts somewhere TypeScript does not include.
- Using any to suppress an incorrect component contract.
- Using array indexes as persistent IDs in application data.
- Forgetting to wrap the components with ThemeProvider.
- Confusing the TypeScript declaration with runtime behavior.

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
      <td><a href="034-component-composition-slots-and-customization.md">034 - Component Composition, Slots, and Customization</a></td>
      <td><a href="../lessons/035-typescript-with-material-ui.md">Lesson</a></td>
      <td><a href="../qnas/035-typescript-with-material-ui.md">Q&amp;A</a></td>
      <td><a href="036-accessibility-and-keyboard-interaction.md">036 - Accessibility and Keyboard Interaction</a></td>
    </tr>
  </tbody>
</table>
