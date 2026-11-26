# Testing Material UI Components

Test Material UI through the interface your users experience. Prefer accessible roles, labels, and visible text over assumptions about internal component instances, generated class names, or DOM structure.

A good test checks meaningful behavior: a form field accepts input, a dialog opens and closes, a save action reports its result, or a selected tab exposes the right content. Material UI's implementation can change while those user-visible expectations remain stable.

## Learning goals

After this chapter, you should be able to:

- Choose accessible queries for Material UI components.
- Simulate realistic interactions with user-event.
- Test forms, dialogs, asynchronous feedback, and keyboard behavior.
- Set up shared theme and provider wrappers without coupling tests to internals.
- Choose when to use component, integration, browser, or visual tests.
- Avoid brittle snapshots and implementation-detail assertions.

## Test what a person can observe

The first question for each test is: what does a user need to be able to do or perceive?

Prefer checking that:

- A field has a useful accessible label and accepts a value.
- A button action changes the visible state.
- A dialog has a name, presents its content, and closes correctly.
- A loading or success status is communicated.
- A tab, menu, or disclosure exposes the expected content and state.
- Invalid input produces understandable feedback.

Avoid testing which internal React component rendered a node, the exact Material UI class name, or a private wrapper structure. Internal details change more often than the behavior users rely on.

## Query by accessible semantics

Testing Library recommends queries that resemble how a user or assistive technology finds content. A useful default order is:

1. Role and accessible name.
2. Label text for form fields.
3. Visible text or placeholder when it is the user's only meaningful cue.
4. Test ID only when semantic queries do not fit the element.

For example, query the button a user sees instead of locating a particular Material UI Button instance:

~~~jsx
const saveButton = screen.getByRole("button", {
  name: "Save project",
});
~~~

The role query can also check accessible state:

~~~jsx
const favoriteButton = screen.getByRole("button", {
  name: "Favorite project",
  pressed: true,
});
~~~

If a role-and-name query is difficult, examine the accessible name and semantics in the rendered UI. The difficulty may point to a missing label or unclear accessible state.

## Choose get, query, or find

Testing Library has synchronous and asynchronous query families:

- **getBy** expects an element to exist now and throws if it does not.
- **queryBy** returns null when no matching element exists and is useful for absence checks.
- **findBy** returns a promise and retries until an element appears or the timeout expires.
- The corresponding **AllBy** versions return collections.

Use the query that describes the timing and expectation:

~~~jsx
expect(screen.getByRole("heading", { name: "Project settings" }))
  .toBeInTheDocument();

expect(screen.queryByRole("dialog"))
  .not.toBeInTheDocument();

expect(await screen.findByRole("alert"))
  .toHaveTextContent("Settings saved");
~~~

Do not use a synchronous query for content that is expected to appear after an asynchronous update. Do not use a waiting query for content that should already exist.

## Simulate user interaction

Use user-event for ordinary interactions. It models a browser-like interaction sequence more closely than dispatching a single low-level event.

~~~jsx
import userEvent from "@testing-library/user-event";

test("the user can update a project name", async () => {
  const user = userEvent.setup();

  render(<ProjectNameForm />);

  const nameField = screen.getByRole("textbox", {
    name: "Project name",
  });

  await user.clear(nameField);
  await user.type(nameField, "Design system");

  expect(nameField).toHaveValue("Design system");
});
~~~

Create a user-event instance in the test before rendering the component. Await its interaction methods so resulting React updates have completed before assertions.

Use fireEvent only when the interaction is not represented by a supported user-event action or when a low-level event is explicitly what the test needs to exercise.

## Test Material UI form fields

TextField renders an input and associated label and helper text. Query it by the label, then test visible behavior and feedback:

~~~jsx
test("shows a validation message for an empty project name", async () => {
  const user = userEvent.setup();

  render(<ProjectNameForm />);

  await user.click(
    screen.getByRole("button", { name: "Save project" }),
  );

  expect(
    screen.getByRole("textbox", { name: "Project name" }),
  ).toHaveAttribute("aria-invalid", "true");

  expect(screen.getByText("Enter a project name."))
    .toBeInTheDocument();
});
~~~

Use the role that the rendered HTML exposes. A select field, autocomplete, date picker, and custom input can have different accessible interactions. Inspect the actual user-facing interface instead of assuming that every TextField is always a simple textbox.

Avoid assertions that depend on generated helper IDs or class names. Check the accessible relationship and visible error text.

## Test dialogs and portals

Dialogs render content in a portal, but Testing Library's screen queries search the document. Test the dialog as the user opens it and interacts with it:

~~~jsx
test("opens and closes project settings", async () => {
  const user = userEvent.setup();

  render(<ProjectPage />);

  await user.click(
    screen.getByRole("button", { name: "Edit settings" }),
  );

  const dialog = await screen.findByRole("dialog", {
    name: "Project settings",
  });

  expect(dialog).toHaveTextContent("Choose who can edit this project");

  await user.click(
    within(dialog).getByRole("button", { name: "Close" }),
  );

  expect(screen.queryByRole("dialog")).not.toBeInTheDocument();
});
~~~

Import within from Testing Library when scoping queries to the dialog. Depending on its configuration, a dialog may close on Escape or backdrop interaction. Test those behaviors when they are part of the product requirement.

Do not query only inside the render container for portal content; portal nodes are attached elsewhere in the document.

## Test asynchronous feedback

An action can show pending, success, or error feedback after a promise resolves. Use findBy when waiting for an element to appear:

~~~jsx
test("announces that a project was saved", async () => {
  const user = userEvent.setup();

  render(<SaveProjectButton save={async () => undefined} />);

  await user.click(
    screen.getByRole("button", { name: "Save project" }),
  );

  expect(await screen.findByRole("status"))
    .toHaveTextContent("Project saved");
});
~~~

Use waitFor when the assertion itself needs to be retried, such as checking a mock call after an asynchronous action:

~~~jsx
await waitFor(() => {
  expect(saveProject).toHaveBeenCalledWith("project-42");
});
~~~

Keep the callback passed to waitFor focused on assertions. Do not place a user action inside a retried callback; Testing Library may run it more than once.

## Test keyboard and component behavior

Prefer user-event keyboard actions for behavior a keyboard user performs:

~~~jsx
await user.tab();
expect(screen.getByRole("button", { name: "Open menu" }))
  .toHaveFocus();

await user.keyboard("{Enter}");
expect(await screen.findByRole("menu"))
  .toBeInTheDocument();
~~~

Test keyboard behavior that matters for the component and task: focus, activation, dismissal, and returning focus after an overlay closes. Avoid duplicating every internal keyboard detail already guaranteed by the component library unless your application changes that behavior.

## Use a shared render wrapper when needed

Components may need a theme, router, localization provider, or application context. Use a reusable wrapper to supply those dependencies:

~~~jsx
import { render } from "@testing-library/react";
import { ThemeProvider, createTheme } from "@mui/material/styles";

const testTheme = createTheme({
  palette: {
    primary: { main: "#3157b7" },
  },
});

function renderWithTheme(ui, options) {
  function Wrapper({ children }) {
    return (
      <ThemeProvider theme={testTheme}>
        {children}
      </ThemeProvider>
    );
  }

  return render(ui, { wrapper: Wrapper, ...options });
}
~~~

Use the smallest realistic provider setup. Avoid building a complex test framework around a single component. Keep test-specific configuration close enough to understand, and share only stable application setup.

If theme values are part of the behavior, provide the real relevant theme options. A default theme can hide bugs in custom color, spacing, or localization configuration.

## Snapshots and implementation details

Large snapshots of Material UI's generated DOM tend to be noisy. They can fail because wrappers, generated IDs, class names, or markup changed without a user-visible regression. They can also pass while the interface is inaccessible or incorrect.

Prefer focused assertions on roles, accessible names, values, visible text, and state. A small snapshot of stable, meaningful output can be useful in some cases, but it should not be the only evidence of behavior.

Avoid tests that:

- Assert generated CSS class names.
- Depend on exact nested wrapper markup.
- Query private component internals.
- Mock the component under test so thoroughly that no real behavior remains.
- Use a test ID where a role or label gives a stronger user-facing contract.
- Reproduce the entire UI in one enormous snapshot.

## Know what a DOM test environment cannot prove

JSDOM and similar environments do not perform full browser layout or rendering. A passing component test does not prove that:

- A popover is positioned correctly on screen.
- A responsive breakpoint looks right at every viewport.
- Fonts and line breaks match production.
- Animation timing feels correct.
- Color contrast is sufficient.
- Focus behavior matches every browser and assistive technology.

Use browser-level, visual, and accessibility checks for questions that require a real browser. Keep component tests focused on semantic output and interaction logic.

## Test setup and cleanup

Use the testing framework's documented DOM environment and setup file. Load the assertion matchers used by the project, such as the Testing Library DOM matchers, from that setup.

React Testing Library normally cleans up rendered components automatically when the test framework provides the expected after-each hook. If using a runner without automatic cleanup, unmount each render explicitly.

Use fake timers only when the behavior depends on time, such as a debounce or delayed snackbar. Advance the clock deliberately, and restore real timers after the test so one test does not affect another.

## Chapter checklist

Before merging tests for a Material UI interface, confirm that:

- Queries use roles, labels, and visible names where possible.
- Interactions use awaited user-event methods.
- Asynchronous changes use findBy or a focused waitFor assertion.
- Portaled dialogs and menus are queried from the document.
- Tests assert behavior users can observe.
- Theme and provider setup matches the relevant application configuration.
- Snapshots are small and supplemental rather than the main test.
- Browser-only layout and visual behavior receive browser-level checks.

## Chapter navigation

<table style="width:100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="038-server-side-rendering-and-nextjs.md">038 - Server-side rendering and Next.js</a></td><td><a href="../qnas/039-testing-material-ui-components.md">Q&amp;A</a></td><td><a href="../examples/039-testing-material-ui-components.md">Example</a></td><td><a href="040-performance-bundle-size-and-tree-shaking.md">040 - Performance, bundle size, and tree shaking</a></td></tr></tbody>
</table>
