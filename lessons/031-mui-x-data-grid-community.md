# MUI X Data Grid Community

MUI X Data Grid Community is a feature-rich React grid for records arranged in rows and columns. It provides a built-in interface for sorting, filtering, pagination, and row selection, so you can build a useful data view without wiring every table interaction yourself.

This chapter focuses on the Community package and its practical boundaries. Use it when a standard HTML-style table would require too much custom behavior, but the dataset and interaction needs still fit the free Community plan.

## Learning goals

After this chapter, you should be able to:

- Install and render the Community Data Grid.
- Define stable columns and rows with unique identifiers.
- Configure a useful initial page size and page-size choices.
- Understand built-in sorting, filtering, and single-row selection.
- Recognize the Community feature limits and choose a paid plan only when a required feature needs it.

## Install the Community package

Install the Community package in a React application that already uses Material UI:

```bash
npm install @mui/x-data-grid
```

The Data Grid depends on React and Material UI. If the project does not already use Material UI with its default Emotion styling engine, install those packages too:

```bash
npm install @mui/material @emotion/react @emotion/styled
```

Import the component from the Community package:

```jsx
import { DataGrid } from "@mui/x-data-grid";
```

Community is MIT-licensed. Pro and Premium are separate commercial packages with additional features. Choose a package based on a needed capability rather than assuming every Data Grid feature is available in Community.

## Define rows and columns

The rows prop is an array of objects. By default, every row must have a unique id property. The columns prop describes which fields to display and how each column should behave.

```jsx
const columns = [
  { field: "name", headerName: "Project", width: 190 },
  { field: "team", headerName: "Team", flex: 1, minWidth: 140 },
  { field: "status", headerName: "Status", width: 140 },
  {
    field: "openTasks",
    headerName: "Open tasks",
    type: "number",
    width: 130,
  },
];

const rows = [
  { id: "atlas", name: "Atlas", team: "Platform", status: "Active", openTasks: 12 },
  { id: "beacon", name: "Beacon", team: "Research", status: "Planning", openTasks: 5 },
];
```

The field value identifies the property to read from each row. If the data uses a different identifier property, provide a getRowId function that returns a stable unique value. Do not generate a new identifier during each render.

Keep the references to columns, rows, and getRowId stable when their contents have not changed. Defining static columns outside the component is a simple approach. Recreating large arrays on every render can make the grid repeat work such as sorting and filtering.

## Render a useful grid

A Data Grid needs a parent with a usable height. A percentage height only works when the parent itself has a defined height. Use a fixed height, a viewport-relative height, or a flex layout with a constrained parent.

```jsx
import { Box } from "@mui/material";
import { DataGrid } from "@mui/x-data-grid";

export function ProjectsGrid() {
  return (
    <Box sx={{ height: 440, width: "100%" }}>
      <DataGrid
        rows={rows}
        columns={columns}
        pageSizeOptions={[5, 10, 25]}
        initialState={{
          pagination: {
            paginationModel: { page: 0, pageSize: 5 },
          },
        }}
      />
    </Box>
  );
}
```

Sorting and column filtering are available through the grid's interface for supported columns. Pagination is enabled by default in the Community plan. The grid can display an empty state when there are no rows.

## Community pagination limits

Community pagination is always enabled and supports a maximum page size of 100 rows. The default page size is 100. Provide pageSizeOptions that fit your screen and product needs; each option must respect the Community limit.

For a fully controlled page model, keep page and pageSize in React state and pass the paginationModel and onPaginationModelChange props. Use initialState for an initial value that the grid can manage on its own. initialState is not a synchronization mechanism for later prop changes.

## Built-in interaction

### Sorting

Users can sort a column by activating its header. Community supports sorting by one column at a time. Repeated activation cycles the sort direction and can return the data to its original order. Sorting is client-side by default when the grid has the full dataset.

### Filtering

Users can filter supported columns through the grid interface. Community supports a single filter criterion at a time. If a product needs multiple simultaneous filter conditions, advanced filtering, or a custom query builder, check the plan limits before designing around those interactions.

### Row selection

Single-row selection is enabled by default. The Community grid also supports checkbox selection, which can expose row checkboxes and a select-all control for bulk workflows. Modifier-based multi-row selection with Ctrl or Cmd and range selection with Shift are Pro features. Keep selection actions clear, and check how selection should behave when users filter or change pages.

### Keyboard and accessibility

The grid implements a composite keyboard interaction model. Users navigate the grid with the keyboard, and focus moves among cells instead of adding every cell to the page's ordinary tab sequence. Preserve the grid's built-in structure when customizing it. Any custom cell renderer that adds buttons or links must have a clear accessible name and a predictable focus order.

## Client-side and server-side data

By default, the grid expects the rows needed for client-side sorting, filtering, and pagination to be available to it. If the server returns only one page, client-side sorting and filtering can operate only on that partial page.

For server-side data, set the relevant mode to server and handle the corresponding model changes by requesting updated data. If pagination, sorting, and filtering all need to cover the complete server dataset, implement all three on the server instead of mixing server pagination with client-only sorting or filtering over one page.

Keep loading state and row count consistent with the response so the grid does not show a page range that contradicts the data source.

## When Community is enough

Community is a good fit for many internal tools and admin views with modest data needs, built-in filtering and sorting, basic row selection, and pages no larger than 100 rows.

Consider Pro or Premium only when the product requires a feature reserved for those plans, such as multi-column sorting, multiple simultaneous filters, column pinning or reordering, or other advanced grid capabilities. Verify the current plan comparison before committing to a feature-dependent design.

## Common mistakes

- Omitting a unique row id or generating a new one on every render.
- Recreating static columns on every render.
- Giving the grid no height or placing it in a parent with unresolved percentage height.
- Expecting a Community page size larger than 100 rows.
- Assuming pagination can be disabled in Community.
- Expecting multiple simultaneous filter criteria or multi-column sorting in Community.
- Paginating on the server while sorting only the currently loaded page.
- Overriding grid keyboard behavior without preserving accessible focus.
- Choosing a paid feature before confirming that the application needs it.

## Chapter navigation

<table>
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
      <td><a href="030-tables-and-data-display.md">030 - Tables and Data Display</a></td>
      <td><a href="../qnas/031-mui-x-data-grid-community.md">Q&amp;A</a></td>
      <td><a href="../examples/031-mui-x-data-grid-community.md">Example</a></td>
      <td><a href="032-icons-and-icon-buttons.md">032 - Icons and Icon Buttons</a></td>
    </tr>
  </tbody>
</table>
