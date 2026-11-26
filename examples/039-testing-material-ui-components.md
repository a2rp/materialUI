# Testing Material UI Components - Example

This example tests a project settings form and confirmation dialog through accessible roles, labels, and realistic user interactions. The test code is illustrative course material; it should be run inside a project with its own configured test runner.

## What this example demonstrates

- Form fields are found by their accessible labels.
- Validation is checked through invalid state and visible error text.
- user-event simulates typing and clicking.
- Async save feedback is awaited.
- A shared wrapper supplies the Material UI theme.
- A dialog rendered in a portal is queried through the document.

## Assumptions

The example uses Jest, JSDOM, React Testing Library, user-event v14, and jest-dom matchers.

Install the test packages:

~~~bash
npm install --save-dev jest jest-environment-jsdom
npm install --save-dev @testing-library/react @testing-library/dom
npm install --save-dev @testing-library/user-event @testing-library/jest-dom
~~~

Configure Jest to use JSDOM and load a setup file:

~~~js
// jest.config.js
module.exports = {
  testEnvironment: "jsdom",
  setupFilesAfterEnv: ["<rootDir>/src/test/setup.js"],
};
~~~

Create **src/test/setup.js**:

~~~js
import "@testing-library/jest-dom";
~~~

Adjust module syntax and configuration to match the project's build tooling.

## 1. Build a form with observable behavior

Create **src/ProjectSettingsForm.jsx**:

~~~jsx
import * as React from "react";
import Alert from "@mui/material/Alert";
import Box from "@mui/material/Box";
import Button from "@mui/material/Button";
import Stack from "@mui/material/Stack";
import TextField from "@mui/material/TextField";

export default function ProjectSettingsForm({ saveProject }) {
  const [name, setName] = React.useState("");
  const [error, setError] = React.useState("");
  const [saving, setSaving] = React.useState(false);
  const [status, setStatus] = React.useState("");

  async function handleSubmit(event) {
    event.preventDefault();
    setStatus("");

    const trimmedName = name.trim();
    if (!trimmedName) {
      setError("Enter a project name.");
      return;
    }

    setError("");
    setSaving(true);

    try {
      await saveProject(trimmedName);
      setStatus("Project saved");
    } catch {
      setError("Unable to save project.");
    } finally {
      setSaving(false);
    }
  }

  return (
    <Box component="form" onSubmit={handleSubmit} noValidate>
      <Stack spacing={2} sx={{ maxWidth: 420 }}>
        <TextField
          id="project-name"
          label="Project name"
          value={name}
          onChange={(event) => setName(event.target.value)}
          error={Boolean(error)}
          helperText={error || "Use a name your team will recognize."}
        />

        <Button type="submit" variant="contained" disabled={saving}>
          {saving ? "Saving..." : "Save project"}
        </Button>

        {status && <Alert severity="success">{status}</Alert>}
      </Stack>
    </Box>
  );
}
~~~

The form exposes a label, field value, error text, disabled pending state, and success message. Tests can check each of these without knowing the component's internal state structure.

## 2. Build a dialog that renders in a portal

Create **src/DeleteProjectAction.jsx**:

~~~jsx
import * as React from "react";
import Button from "@mui/material/Button";
import Dialog from "@mui/material/Dialog";
import DialogActions from "@mui/material/DialogActions";
import DialogContent from "@mui/material/DialogContent";
import DialogTitle from "@mui/material/DialogTitle";

export default function DeleteProjectAction() {
  const [open, setOpen] = React.useState(false);

  return (
    <>
      <Button onClick={() => setOpen(true)}>Delete project</Button>

      <Dialog
        open={open}
        onClose={() => setOpen(false)}
        aria-labelledby="delete-project-title"
        aria-describedby="delete-project-description"
      >
        <DialogTitle id="delete-project-title">
          Delete project?
        </DialogTitle>
        <DialogContent id="delete-project-description">
          This action will remove the project from your workspace.
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setOpen(false)}>Cancel</Button>
          <Button color="error" onClick={() => setOpen(false)}>
            Confirm delete
          </Button>
        </DialogActions>
      </Dialog>
    </>
  );
}
~~~

The dialog renders outside the component's local DOM subtree. The test will use screen queries, which search the document, and within to target buttons inside the dialog.

## 3. Create a themed render helper

Create **src/test/test-utils.jsx**:

~~~jsx
import { render } from "@testing-library/react";
import { createTheme, ThemeProvider } from "@mui/material/styles";

const testTheme = createTheme({
  palette: {
    primary: { main: "#3157b7" },
  },
});

function Providers({ children }) {
  return (
    <ThemeProvider theme={testTheme}>
      {children}
    </ThemeProvider>
  );
}

function renderWithTheme(ui, options) {
  return render(ui, {
    wrapper: Providers,
    ...options,
  });
}

export * from "@testing-library/react";
export { renderWithTheme };
~~~

Add only the providers the component actually needs. For a component that uses routing, localization, or application context, extend this wrapper with the smallest realistic provider configuration.

## 4. Test validation and saving

Create **src/ProjectSettingsForm.test.jsx**:

~~~jsx
import userEvent from "@testing-library/user-event";
import { renderWithTheme, screen } from "./test/test-utils";
import ProjectSettingsForm from "./ProjectSettingsForm";

test("shows a clear error when the project name is empty", async () => {
  const user = userEvent.setup();

  renderWithTheme(
    <ProjectSettingsForm saveProject={jest.fn()} />,
  );

  await user.click(
    screen.getByRole("button", { name: "Save project" }),
  );

  expect(
    screen.getByRole("textbox", { name: "Project name" }),
  ).toHaveAttribute("aria-invalid", "true");

  expect(screen.getByText("Enter a project name."))
    .toBeInTheDocument();
});

test("saves the trimmed name and announces success", async () => {
  const user = userEvent.setup();
  const saveProject = jest.fn().mockResolvedValue(undefined);

  renderWithTheme(
    <ProjectSettingsForm saveProject={saveProject} />,
  );

  await user.type(
    screen.getByRole("textbox", { name: "Project name" }),
    "  Design system  ",
  );
  await user.click(
    screen.getByRole("button", { name: "Save project" }),
  );

  expect(await screen.findByRole("alert"))
    .toHaveTextContent("Project saved");
  expect(saveProject).toHaveBeenCalledWith("Design system");
});
~~~

The test checks the field's accessible name and invalid state, then checks visible feedback. It does not inspect the TextField's private component instance or generated class names.

## 5. Test a portal dialog

Add a separate test file **src/DeleteProjectAction.test.jsx**:

~~~jsx
import userEvent from "@testing-library/user-event";
import {
  renderWithTheme,
  screen,
  within,
} from "./test/test-utils";
import DeleteProjectAction from "./DeleteProjectAction";

test("opens the confirmation dialog and lets the user cancel", async () => {
  const user = userEvent.setup();

  renderWithTheme(<DeleteProjectAction />);

  await user.click(
    screen.getByRole("button", { name: "Delete project" }),
  );

  const dialog = await screen.findByRole("dialog", {
    name: "Delete project?",
  });

  expect(dialog).toHaveTextContent(
    "This action will remove the project from your workspace.",
  );

  await user.click(
    within(dialog).getByRole("button", { name: "Cancel" }),
  );

  expect(screen.queryByRole("dialog"))
    .not.toBeInTheDocument();
});
~~~

The dialog's accessible name comes from its title. The test queries it from the document, then scopes the Cancel button lookup to that dialog.

## 6. Add an asynchronous failure case

A rejected save should give the user a visible explanation. A separate test can verify that behavior:

~~~jsx
test("shows an error when saving fails", async () => {
  const user = userEvent.setup();
  const saveProject = jest.fn().mockRejectedValue(
    new Error("Network unavailable"),
  );

  renderWithTheme(
    <ProjectSettingsForm saveProject={saveProject} />,
  );

  await user.type(
    screen.getByRole("textbox", { name: "Project name" }),
    "Design system",
  );
  await user.click(
    screen.getByRole("button", { name: "Save project" }),
  );

  expect(await screen.findByText("Unable to save project."))
    .toBeInTheDocument();
});
~~~

The component intentionally presents a user-facing message rather than exposing a raw technical exception. The test verifies the interaction contract and leaves logging or error reporting to the application's error-handling layer.

## Run the example in a configured project

Use the command configured for the project's test runner. For Jest, a focused run can look like this:

~~~bash
npx jest src/ProjectSettingsForm.test.jsx src/DeleteProjectAction.test.jsx
~~~

This command is shown as an example and has not been run in the notes repository. Component tests cannot verify real browser layout, visual contrast, or popover placement; use browser-level checks for those questions.

## Common mistakes

- Querying generated Emotion classes or private DOM structure instead of user-facing roles and labels.
- Forgetting to await asynchronous user interactions and resulting feedback.
- Treating a passing DOM test as proof of visual layout, browser behavior, or assistive-technology usability.
- Leaving portals, timers, or shared state behind between tests.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="038-server-side-rendering-and-nextjs.md">038 - Server-side rendering and Next.js</a></td><td><a href="../lessons/039-testing-material-ui-components.md">Lesson</a></td><td><a href="../qnas/039-testing-material-ui-components.md">Q&amp;A</a></td><td><a href="040-performance-bundle-size-and-tree-shaking.md">040 - Performance, bundle size, and tree shaking</a></td></tr></tbody>
</table>
