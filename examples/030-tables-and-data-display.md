# Tables and Data Display: Search, Sorting, and Pagination Example

This example builds a searchable project table with sortable columns and client-side pagination. It uses semantic table markup, stable row keys, a caption, numeric alignment, an empty-result message, and horizontal scrolling for narrow screens.

## What you will build

- A project table with name, team, status, and open-task columns.
- A text search across project names, teams, and statuses.
- Sortable headings that update the displayed rows.
- Pagination over the sorted, filtered results.
- An explicit message when a search returns no rows.

## Requirements

Create a React application with Vite and install Material UI:

```bash
npm create vite@latest mui-project-table -- --template react
cd mui-project-table
npm install
npm install @mui/material @emotion/react @emotion/styled
npm run dev
```

Replace `src/App.jsx` with this component.

## `src/App.jsx`

```jsx
import { useMemo, useState } from "react";
import {
  Paper,
  Table,
  TableBody,
  TableCell,
  TableContainer,
  TablePagination,
  TableHead,
  TableRow,
  TableSortLabel,
  TextField,
} from "@mui/material";

const projects = [
  { id: "atlas", name: "Atlas", team: "Platform", status: "Active", openTasks: 12 },
  { id: "beacon", name: "Beacon", team: "Research", status: "Planning", openTasks: 5 },
  { id: "cobalt", name: "Cobalt", team: "Platform", status: "Blocked", openTasks: 8 },
  { id: "delta", name: "Delta", team: "Growth", status: "Active", openTasks: 3 },
  { id: "ember", name: "Ember", team: "Research", status: "Complete", openTasks: 0 },
  { id: "fjord", name: "Fjord", team: "Growth", status: "Planning", openTasks: 7 },
  { id: "grove", name: "Grove", team: "Platform", status: "Active", openTasks: 2 },
];

const columns = [
  { id: "name", label: "Project", numeric: false },
  { id: "team", label: "Team", numeric: false },
  { id: "status", label: "Status", numeric: false },
  { id: "openTasks", label: "Open tasks", numeric: true },
];

function compareValues(left, right) {
  if (typeof left === "number" && typeof right === "number") {
    return left - right;
  }

  return String(left).localeCompare(String(right), undefined, {
    numeric: true,
    sensitivity: "base",
  });
}

function descendingComparator(a, b, orderBy) {
  return compareValues(b[orderBy], a[orderBy]);
}

function getComparator(order, orderBy) {
  return order === "desc"
    ? (a, b) => descendingComparator(a, b, orderBy)
    : (a, b) => -descendingComparator(a, b, orderBy);
}

function stableSort(array, comparator) {
  const indexedRows = array.map((row, index) => [row, index]);

  indexedRows.sort((a, b) => {
    const result = comparator(a[0], b[0]);
    return result === 0 ? a[1] - b[1] : result;
  });

  return indexedRows.map(([row]) => row);
}

export default function App() {
  const [query, setQuery] = useState("");
  const [orderBy, setOrderBy] = useState("name");
  const [order, setOrder] = useState("asc");
  const [page, setPage] = useState(0);
  const [rowsPerPage, setRowsPerPage] = useState(5);

  const filteredRows = useMemo(() => {
    const normalizedQuery = query.trim().toLowerCase();
    if (!normalizedQuery) return projects;

    return projects.filter((project) =>
      [project.name, project.team, project.status].some((value) =>
        value.toLowerCase().includes(normalizedQuery),
      ),
    );
  }, [query]);

  const sortedRows = useMemo(
    () => stableSort(filteredRows, getComparator(order, orderBy)),
    [filteredRows, order, orderBy],
  );

  const pageRows = sortedRows.slice(
    page * rowsPerPage,
    page * rowsPerPage + rowsPerPage,
  );

  function requestSort(property) {
    const nextOrder =
      property === orderBy && order === "asc" ? "desc" : "asc";
    setOrder(nextOrder);
    setOrderBy(property);
    setPage(0);
  }

  function changeQuery(event) {
    setQuery(event.target.value);
    setPage(0);
  }

  function changeRowsPerPage(event) {
    setRowsPerPage(Number(event.target.value));
    setPage(0);
  }

  return (
    <Paper sx={{ p: { xs: 2, sm: 3 } }}>
      <TextField
        label="Search projects"
        value={query}
        onChange={changeQuery}
        size="small"
        fullWidth
        sx={{ mb: 2, maxWidth: 420 }}
      />

      <TableContainer sx={{ maxHeight: 440, overflowX: "auto" }}>
        <Table stickyHeader size="small" sx={{ minWidth: 650 }}>
          <caption>Project teams, status, and number of open tasks</caption>
          <TableHead>
            <TableRow>
              {columns.map((column) => {
                const active = orderBy === column.id;

                return (
                  <TableCell
                    key={column.id}
                    component="th"
                    scope="col"
                    align={column.numeric ? "right" : "left"}
                    sortDirection={active ? order : false}
                  >
                    <TableSortLabel
                      active={active}
                      direction={active ? order : "asc"}
                      onClick={() => requestSort(column.id)}
                    >
                      {column.label}
                    </TableSortLabel>
                  </TableCell>
                );
              })}
            </TableRow>
          </TableHead>

          <TableBody>
            {pageRows.length === 0 ? (
              <TableRow>
                <TableCell colSpan={columns.length}>
                  No projects match this search.
                </TableCell>
              </TableRow>
            ) : (
              pageRows.map((project) => (
                <TableRow hover key={project.id}>
                  <TableCell component="th" scope="row">
                    {project.name}
                  </TableCell>
                  <TableCell>{project.team}</TableCell>
                  <TableCell>{project.status}</TableCell>
                  <TableCell align="right">{project.openTasks}</TableCell>
                </TableRow>
              ))
            )}
          </TableBody>
        </Table>
      </TableContainer>

      <TablePagination
        component="div"
        count={sortedRows.length}
        page={page}
        rowsPerPage={rowsPerPage}
        rowsPerPageOptions={[5, 10, 25]}
        onPageChange={(_event, nextPage) => setPage(nextPage)}
        onRowsPerPageChange={changeRowsPerPage}
      />
    </Paper>
  );
}
```

## `src/main.jsx`

Use the Vite entry point to mount the table:

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

## How it works

### Search before sorting and pagination

The search creates a filtered array first. The comparator sorts that filtered array, and pagination slices the sorted result last. This order ensures a match can move between pages when the sort changes and that the page count describes the filtered rows.

### Stable sorting

The comparator handles numeric values numerically and strings with locale-aware comparison. The stable sort keeps the original order when two values compare equally. This avoids unnecessary row movement for equal values.

### Accessible sort state

Each heading uses a TableSortLabel inside a TableCell. The active cell receives sortDirection, and the label receives the active sort direction. The heading text remains visible, so meaning does not depend on the arrow icon alone.

### Responsive layout

The table has a minimum width so column labels and values remain readable. TableContainer allows horizontal and vertical scrolling. On a small screen, users can scroll the table instead of having every value compressed into unreadable columns.

### Empty results

When search matches no projects, the table keeps its heading and displays a message spanning all columns. This distinguishes an empty result from a loading or rendering failure.

## Try these changes

1. Search for Platform to show only Platform projects.
2. Sort by Open tasks, then click it again to reverse the order.
3. Search for a value that is not in the sample data to see the empty state.
4. Change the initial rowsPerPage value to 10 and compare the pagination controls.
5. Add more project records and check that page count and search remain correct.
6. Try a long team name to see how the horizontal container behaves.

## Common mistakes

- Sorting after slicing rows into pages.
- Passing the unfiltered row count to TablePagination.
- Forgetting to reset the page after a search or page-size change.
- Comparing numeric fields as strings.
- Using an array index as a row key.
- Omitting a caption or accessible table name.
- Assuming hover styling makes a row interactive.
- Removing column labels on mobile without preserving their meaning.

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
      <td><a href="029-chips-avatars-and-badges.md">029 - Chips, Avatars, and Badges</a></td>
      <td><a href="../lessons/030-tables-and-data-display.md">Lesson</a></td>
      <td><a href="../qnas/030-tables-and-data-display.md">Q&amp;A</a></td>
      <td><a href="031-mui-x-data-grid-community.md">031 - MUI X Data Grid Community</a></td>
    </tr>
  </tbody>
</table>
