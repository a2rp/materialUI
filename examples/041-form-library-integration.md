# Form Library Integration - Example

This example connects a project form to React Hook Form and Material UI. It mixes controlled TextField and select controls with a registered native-style TextField input and a controlled checkbox.

## What this example demonstrates

- Complete defaults for every field.
- Controller mapping for controlled values.
- register mapping to TextField's inner input for a compatible field.
- Field validation rendered as Material UI helper text.
- Checkbox state stored as a boolean.
- Async submission, visible success and failure messages, and reset behavior.

## Assumptions

The example uses React Hook Form with Material UI and Emotion:

~~~bash
npm install react-hook-form @mui/material @emotion/react @emotion/styled
~~~

It assumes the application has a working API endpoint for saving projects.

## 1. Build the form

Create **src/ProjectForm.jsx**:

~~~jsx
import * as React from "react";
import { Controller, useForm } from "react-hook-form";

import Alert from "@mui/material/Alert";
import Box from "@mui/material/Box";
import Button from "@mui/material/Button";
import Checkbox from "@mui/material/Checkbox";
import FormControlLabel from "@mui/material/FormControlLabel";
import MenuItem from "@mui/material/MenuItem";
import Stack from "@mui/material/Stack";
import TextField from "@mui/material/TextField";
import Typography from "@mui/material/Typography";

export default function ProjectForm({ project, saveProject }) {
  const [saved, setSaved] = React.useState(false);
  const [submitError, setSubmitError] = React.useState("");

  const {
    control,
    register,
    handleSubmit,
    reset,
    formState: { errors, isSubmitting, isDirty },
  } = useForm({
    mode: "onBlur",
    defaultValues: {
      projectName: "",
      owner: "",
      visibility: "private",
      sendUpdates: false,
    },
  });

  const ownerField = register("owner", {
    required: "Enter an owner.",
  });

  React.useEffect(() => {
    if (!project) {
      return;
    }

    reset({
      projectName: project.name ?? "",
      owner: project.owner ?? "",
      visibility: project.visibility ?? "private",
      sendUpdates: Boolean(project.sendUpdates),
    });

    setSaved(false);
    setSubmitError("");
  }, [project, reset]);

  async function onSubmit(values) {
    setSaved(false);
    setSubmitError("");

    try {
      await saveProject(values);
      setSaved(true);
    } catch {
      setSubmitError("Could not save the project. Try again.");
    }
  }

  function handleReset() {
    reset();
    setSaved(false);
    setSubmitError("");
  }

  return (
    <Box
      component="form"
      onSubmit={handleSubmit(onSubmit)}
      noValidate
      sx={{ maxWidth: 520 }}
    >
      <Stack spacing={2.5}>
        <Typography component="h1" variant="h4">
          Project settings
        </Typography>

        <Controller
          name="projectName"
          control={control}
          rules={{
            required: "Enter a project name.",
            minLength: {
              value: 3,
              message: "Use at least 3 characters.",
            },
          }}
          render={({ field, fieldState }) => {
            const { ref, ...inputProps } = field;

            return (
              <TextField
                {...inputProps}
                inputRef={ref}
                label="Project name"
                error={Boolean(fieldState.error)}
                helperText={
                  fieldState.error?.message ??
                  "Use a name your team will recognize."
                }
                required
              />
            );
          }}
        />

        <TextField
          name={ownerField.name}
          onChange={ownerField.onChange}
          onBlur={ownerField.onBlur}
          inputRef={ownerField.ref}
          label="Project owner"
          error={Boolean(errors.owner)}
          helperText={
            errors.owner?.message ?? "For example, the team or department."
          }
          required
        />

        <Controller
          name="visibility"
          control={control}
          rules={{ required: "Choose a visibility setting." }}
          render={({ field, fieldState }) => {
            const { ref, ...selectProps } = field;

            return (
              <TextField
                {...selectProps}
                inputRef={ref}
                select
                label="Visibility"
                error={Boolean(fieldState.error)}
                helperText={fieldState.error?.message}
              >
                <MenuItem value="private">Private</MenuItem>
                <MenuItem value="team">Team</MenuItem>
                <MenuItem value="public">Public</MenuItem>
              </TextField>
            );
          }}
        />

        <Controller
          name="sendUpdates"
          control={control}
          render={({ field }) => (
            <FormControlLabel
              label="Email me project updates"
              control={
                <Checkbox
                  name={field.name}
                  checked={field.value}
                  onChange={(event) =>
                    field.onChange(event.target.checked)
                  }
                  onBlur={field.onBlur}
                  inputRef={field.ref}
                />
              }
            />
          )}
        />

        {submitError && (
          <Alert severity="error">
            {submitError}
          </Alert>
        )}

        {saved && (
          <Alert severity="success" role="status">
            Project saved.
          </Alert>
        )}

        <Stack direction="row" spacing={2}>
          <Button
            type="submit"
            variant="contained"
            disabled={isSubmitting}
          >
            {isSubmitting ? "Saving..." : "Save project"}
          </Button>

          <Button
            type="button"
            variant="outlined"
            disabled={!isDirty || isSubmitting}
            onClick={handleReset}
          >
            Reset
          </Button>
        </Stack>
      </Stack>
    </Box>
  );
}
~~~

Keep the name, event handlers, and input ref from one registration result so they remain attached to the same field.

## 2. Provide an asynchronous save function

Create **src/api/saveProject.js** or use the application's existing data layer:

~~~js
export async function saveProject(values) {
  const response = await fetch("/api/projects", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify(values),
  });

  if (!response.ok) {
    throw new Error("Project save failed");
  }
}
~~~

The API must validate and authorize the submitted values on the server. The form's client-side rules are for immediate feedback, not a security boundary.

## 3. Connect it to the page

Create or update **src/ProjectSettingsPage.jsx**:

~~~jsx
import ProjectForm from "./ProjectForm";
import { saveProject } from "./api/saveProject";

const existingProject = {
  id: "project-42",
  name: "Design system",
  owner: "Product team",
  visibility: "team",
  sendUpdates: true,
};

export default function ProjectSettingsPage() {
  return (
    <ProjectForm
      project={existingProject}
      saveProject={saveProject}
    />
  );
}
~~~

Pass a stable project object when the selected record changes. The form's effect calls reset with the selected record's values. If the data is loaded asynchronously, render a loading state or pass the loaded record once it is ready.

## Behavior to review

In the running application, confirm that:

- Submitting an empty project name moves focus to the invalid field when supported.
- Validation text appears next to the matching field.
- The owner field is registered once and remains uncontrolled.
- The select submits one of its declared values.
- The checkbox submits a boolean.
- The Save button is disabled while the request is pending.
- Success and server failure messages are visible.
- Reset restores the form's latest default values.
- Switching to another project updates every field without losing value types.

## Chapter navigation

<table style="width:100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="040-performance-bundle-size-and-tree-shaking.md">040 - Performance, bundle size, and tree shaking</a></td><td><a href="../lessons/041-form-library-integration.md">Lesson</a></td><td><a href="../qnas/041-form-library-integration.md">Q&amp;A</a></td><td><a href="042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance</a></td></tr></tbody>
</table>
