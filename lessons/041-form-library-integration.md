# Form Library Integration

A form library manages values, validation, errors, touched state, and submission. Material UI provides the visible controls. Integration is easiest when each field has one clear source of truth and the form library receives the value and events in the shape it expects.

This chapter uses React Hook Form for examples. The same principles apply to other form libraries: decide which fields are native or uncontrolled and which are controlled components, then adapt their value and event APIs deliberately.

## Learning goals

After this chapter, you should be able to:

- Choose between native form registration and controlled field integration.
- Connect React Hook Form to Material UI TextField, Select, and Checkbox.
- Map validation errors to Material UI error and helper text props.
- Provide complete defaults and handle reset or asynchronously loaded data.
- Avoid double registration and duplicate sources of truth.
- Integrate nonstandard values from autocomplete and date picker fields carefully.

## Decide how a field is controlled

React Hook Form can manage native inputs through registration. This is a good fit for inputs that expose a DOM ref and standard input events.

Many component-library controls are controlled: they receive a value and report changes through a callback. React Hook Form provides Controller and useController for these components.

A practical choice:

- Use register for a native input or a component that forwards the actual input ref and standard change events.
- Use Controller or useController for a component that needs a value prop, has a nonstandard callback, or returns a value instead of an event.
- Do not register the same field a second time after Controller has already registered it.

Mixing native and controlled fields in one form is normal.

## Give every field a default value

Use complete default values when calling useForm:

~~~jsx
const form = useForm({
  defaultValues: {
    projectName: "",
    owner: "",
    visibility: "private",
    sendUpdates: false,
  },
});
~~~

A controlled field should not start with undefined and later receive a string, boolean, or object. This can switch the underlying input from uncontrolled to controlled and produce React warnings.

For a field that can be empty, choose a deliberate empty value such as an empty string, false, or null based on that component's value type. Do not pass undefined to Controller's onChange callback.

Default values also provide the baseline for dirty-state comparisons. If data arrives after the form mounts, use reset or the form library's reactive values option instead of expecting a changed defaultValues object to replace the current form values automatically.

## Connect a TextField with Controller

Controller supplies the field value and callbacks. Map its ref to the actual input using TextField's inputRef, rather than attaching the field ref to the outer TextField root:

~~~jsx
import { Controller, useForm } from "react-hook-form";
import Button from "@mui/material/Button";
import Stack from "@mui/material/Stack";
import TextField from "@mui/material/TextField";

function ProjectForm({ saveProject }) {
  const { control, handleSubmit } = useForm({
    defaultValues: {
      projectName: "",
    },
  });

  async function onSubmit(values) {
    await saveProject(values);
  }

  return (
    <form onSubmit={handleSubmit(onSubmit)} noValidate>
      <Stack spacing={2}>
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
                helperText={fieldState.error?.message}
              />
            );
          }}
        />

        <Button type="submit" variant="contained">
          Save project
        </Button>
      </Stack>
    </form>
  );
}
~~~

Connect onChange, onBlur, name, value, and ref. The ref matters when the library should focus an invalid field after submission. Check which ref prop the particular Material UI control exposes.

Map fieldState.error to the control's visible error state and helper text. An error should explain what is wrong or how to fix it, not rely on color alone.

## Register a native input

For a standard native input, register can avoid a controlled value prop. When adapting registration to TextField, forward the ref to its input:

~~~jsx
const ownerRegistration = register("owner", {
  required: "Enter an owner.",
});

<TextField
  name={ownerRegistration.name}
  onChange={ownerRegistration.onChange}
  onBlur={ownerRegistration.onBlur}
  inputRef={ownerRegistration.ref}
  label="Project owner"
  error={Boolean(errors.owner)}
  helperText={errors.owner?.message}
/>
~~~

The TextField root is not the same node as its inner input. If the library needs the input ref for focus or value access, connect the ref to the actual input element.

## Integrate Select fields

A Material UI TextField with select can be controlled through Controller. Keep the current value and callbacks connected, and use a non-undefined default value:

~~~jsx
<Controller
  name="visibility"
  control={control}
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
~~~

The selected value should match the option values. If the select can be cleared, decide whether the form value should be an empty string or null, and validate that value consistently.

## Integrate checkbox values

A checkbox's value is its checked state. Pass the boolean to the form rather than the event object:

~~~jsx
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
          onChange={(event) => field.onChange(event.target.checked)}
          onBlur={field.onBlur}
          inputRef={field.ref}
        />
      }
    />
  )}
/>
~~~

Initialize the field with false. Use the checkbox's checked prop; its value prop has a different meaning and should not be used as the boolean checked state.

## Connect validation and submission state

React Hook Form can validate with field rules or a schema resolver. Use a resolver when validation rules need to be shared, composed, or expressed in a schema.

Choose a validation mode that fits the form. Validating on submit avoids showing errors before a user interacts. Validating on blur or change can provide quicker feedback but may create a noisy experience if every keystroke announces an error.

Use formState for submission and error behavior:

~~~jsx
const {
  control,
  handleSubmit,
  formState: { errors, isSubmitting, isDirty },
} = useForm({
  defaultValues: {
    projectName: "",
    visibility: "private",
  },
});
~~~

Disable duplicate submission while the asynchronous handler is running. Show success and server errors in visible text. A client-side schema does not replace validation and authorization on the server.

## Reset values when records change

Default values are read when the form initializes and are cached by React Hook Form. When loading data after mount or switching to another record, reset the form with the new values:

~~~jsx
React.useEffect(() => {
  if (project) {
    reset({
      projectName: project.name ?? "",
      owner: project.owner ?? "",
      visibility: project.visibility ?? "private",
      sendUpdates: Boolean(project.sendUpdates),
    });
  }
}, [project, reset]);
~~~

Use stable record identity in the surrounding application so a response for an old record does not overwrite a newer selection. Make nullability explicit when converting data into form defaults.

## Integrate controls with nonstandard callbacks

Autocomplete, date pickers, ratings, and range controls may report values through callbacks with signatures that differ from native input events.

With Autocomplete, the selected option is a separate value from the textbox's display text. Store the selected value in the form and map the component's onChange callback to the form field. When options are objects, use a stable identifier for equality if the objects can be recreated.

Date pickers can return a date object or null, and may also expose invalid intermediate values. Choose the form's date representation deliberately, provide a matching default such as null, and decide whether submission should store a date object, ISO value, or date-only string. Avoid converting an invalid or incomplete picker value into a valid submitted date.

For each controlled widget:

1. Identify its actual value type.
2. Map its change callback to the form field.
3. Connect blur and focus ref where the component supports them.
4. Translate validation errors into visible helper text or status.
5. Test clear, keyboard, and invalid states.

## Share form context carefully

For a deep form tree, FormProvider can make form methods available to nested components through useFormContext. Use it when it simplifies component boundaries, but do not register one field in multiple places.

A reusable field wrapper can standardize label, error, helper text, and ref mapping. Keep the wrapper specific enough to preserve each component's real value and event semantics.

## Common mistakes

- Leaving a controlled field's initial value undefined.
- Attaching a form ref to the TextField root instead of its input.
- Calling register again on a field already managed by Controller.
- Passing checkbox event objects instead of checked booleans.
- Treating Autocomplete's input text as the selected option.
- Assuming a date picker only returns a valid date.
- Changing defaultValues after mount without calling reset.
- Showing an error only with red styling and no explanatory text.
- Assuming client validation replaces server validation.
- Maintaining duplicate local state and form-library state for one field.

## Chapter checklist

Before shipping, confirm that:

- Every field has one source of truth and a deliberate default value.
- Native fields use registration only when their ref and event behavior fit.
- Controlled MUI fields connect value, change, blur, name, and input ref appropriately.
- Validation errors are visible and associated with their controls.
- Checkbox and custom picker values use the intended types.
- Asynchronous submission prevents duplicate requests and reports failures.
- Loaded or switched records reset the form with complete values.
- Server-side validation and authorization remain in place.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="040-performance-bundle-size-and-tree-shaking.md">040 - Performance, bundle size, and tree shaking</a></td><td><a href="../qnas/041-form-library-integration.md">Q&amp;A</a></td><td><a href="../examples/041-form-library-integration.md">Example</a></td><td><a href="042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance</a></td></tr></tbody>
</table>
