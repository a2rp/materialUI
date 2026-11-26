# MUI X Data Grid Community: Project Tracker Example

This example uses the Community Data Grid to display a small project list. The grid provides column sorting, filtering, row selection, and pagination. The example keeps its row and column definitions stable and gives the grid an explicit height.

## What you will build

- A project grid with name, team, status, and open-task columns.
- Built-in column sorting and filtering.
- A checkbox selection affordance for bulk selection.
- Client-side pagination with a page size below the Community maximum.
- A responsive container that lets the grid use the available width.

## Requirements

In an existing React application, install the Community grid and its Material UI dependencies:

```bash
npm install @mui/x-data-grid @mui/material @emotion/react @emotion/styled
```

If you use Vite to create a new project:

```bash
npm create vite@latest mui-x-project-tracker -- --template react
cd mui-x-project-tracker
npm install
npm install @mui/x-data-grid @mui/material @emotion/react @emotion/styled
npm run dev
```

## `src/App.jsx`

Replace the starter component with this file:

```jsx
import { Box, Typography } from "@mui/material";
import { DataGrid } from "@mui/x-data-grid";

const rows = [
  { id: "atlas", project: "Atlas", team: "Platform", status: "Active", openTasks: 12 },
  { id: "beacon", project: "Beacon", team: "Research", status: "Planning", openTasks: 5 },
  { id: "cobalt", project: "Cobalt", team: "Platform", status: "Blocked", openTasks: 8 },
  { id: "delta", project: "Delta", team: "Growth", status: "Active", openTasks: 3 },
  { id: "ember", project: "Ember", team: "Research", status: "Complete", openTasks: 0 },
  { id: "fjord", project: "Fjord", team: "Growth", status: "Planning", openTasks: 7 },
  { id: "grove", project: "Grove", team: "Platform", status: "Active", openTasks: 2 },
];

const columns = [
  {
    field: "project",
    headerName: "Project",
    width: 180,
  },
  {
    field: "team",
    headerName: "Team",
    flex: 1,
    minWidth: 140,
  },
  {
    field: "status",
    headerName: "Status",
    width: 140,
  },
  {
    field: "openTasks",
    headerName: "Open tasks",
    type: "number",
    width: 130,
  },
];

export default function App() {
  return (
    <Box sx={{ p: { xs: 2, sm: 3 } }}>
      <Typography component="h1" variant="h4" fontWeight={700} sx={{ mb: 2 }}>
        Project tracker
      </Typography>

      <Box sx={{ height: 440, width: "100%" }}>
        <DataGrid
          rows={rows}
          columns={columns}
          checkboxSelection
          pageSizeOptions={[5, 10, 25]}
          initialState={{
            pagination: {
              paginationModel: { page: 0, pageSize: 5 },
            },
          }}
        />
      </Box>
    </Box>
  );
}
```

## `src/main.jsx`

Keep or replace the Vite entry point with:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

## Try the grid

1. Click the Project heading to sort by project name.
2. Open a column menu to filter by a value such as Platform.
3. Use the checkboxes to select rows, or use the header checkbox to select the current set.
4. Move between pages and change the page size.
5. Resize the browser and observe how the flexible Team column uses available space.
6. Remove one row from the sample data and confirm that the remaining grid still renders with unique IDs.

## Change the row identifier

The sample data uses an id field, which the Data Grid recognizes by default. If an API returns another stable identifier, supply a stable getRowId function:

```jsx
function getProjectId(row) {
  return row.projectKey;
}

<DataGrid rows={apiRows} columns={columns} getRowId={getProjectId} />
```

Define the function outside the component or memoize it. Do not create random IDs during rendering; a row's identifier must stay the same across updates.

## Add controlled pagination

Use initialState when you only need to choose the starting page and page size. If the application needs to control pagination later, store the pagination model in React state and pass paginationModel with onPaginationModelChange. Keep the selected page size at or below 100 for the Community plan.

## How the example works

### Stable definitions

Rows and columns are declared outside the component, so their references do not change on every render. This helps the grid avoid unnecessary processing and preserves column state.

### Built-in sorting and filtering

The grid provides sorting and filtering for supported columns. The Project, Team, and Status columns use text values. Open tasks uses the number type so its values are treated as numbers.

### Selection

The checkboxSelection prop displays row checkboxes. The header checkbox provides a way to select the displayed set. Community also provides single-row selection by default. If the product requires modifier-based multi-row selection or range selection, verify that the plan supports it before designing the workflow.

### Pagination

Pagination is built into Community and remains enabled. The initial page size is five rows, and the available choices are all within the Community maximum of 100.

### Layout

The outer Box gives the grid a fixed height and full available width. Data Grid needs a bounded vertical space to calculate its layout. The Team column uses flex to expand, with a minimum width to keep its content readable.

## Common mistakes

- Leaving out a unique id for a row.
- Using a newly generated getRowId function on every render.
- Recreating large row and column arrays on each render.
- Giving the grid no usable height.
- Providing a page-size option above 100 on the Community plan.
- Expecting pagination to be disabled in Community.
- Assuming client-side filtering can see server rows that have not been loaded.
- Building around Pro or Premium features before checking the plan boundary.

## Chapter navigation

<table>
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
      <td><a href="030-tables-and-data-display.md">030 - Tables and Data Display</a></td>
      <td><a href="../lessons/031-mui-x-data-grid-community.md">Lesson</a></td>
      <td><a href="../qnas/031-mui-x-data-grid-community.md">Q&amp;A</a></td>
      <td><a href="032-icons-and-icon-buttons.md">032 - Icons and Icon Buttons</a></td>
    </tr>
  </tbody>
</table>
