# Tables and Data Display

Material UI's Table components provide a React interface over native HTML table elements. Use them when information is naturally organized into rows and columns and users need to compare values across records.

This chapter covers semantic structure, sorting, pagination, responsive behavior, empty states, and how to choose between a standard Table and a data-grid component.

## Learning goals

After this chapter, you should be able to:

- Build a table with accessible header, body, and row structure.
- Render records with stable React keys and aligned values.
- Add sorting and pagination without losing semantic table markup.
- Handle narrow screens and empty data states.
- Decide when a standard Table is enough and when a data grid is a better fit.

## Table components and HTML semantics

The common Material UI table components map to familiar HTML elements:

| Material UI component | Typical HTML element | Purpose |
| --- | --- | --- |
| TableContainer | A scrolling container | Holds a table and can provide horizontal overflow. |
| Table | table | Defines the tabular structure. |
| TableHead | thead | Groups column headings. |
| TableBody | tbody | Groups data rows. |
| TableFooter | tfoot | Groups footer rows when needed. |
| TableRow | tr | Represents a header, data, or footer row. |
| TableCell | th or td | Represents a heading or data cell. |
| TablePagination | A pagination control | Changes which rows are displayed. |

A TableCell inside TableHead renders as a header cell by default. Use scope="col" for column headings and scope="row" for a row heading when it helps communicate relationships. A caption gives the table a concise description that is available to assistive technology.

## Build a semantic table

Start with a clear header row and render one TableRow for each record. Use a stable key from the record rather than the array index.

```jsx
import {
  Paper,
  Table,
  TableBody,
  TableCell,
  TableContainer,
  TableHead,
  TableRow,
} from "@mui/material";

const projects = [
  { id: "atlas", name: "Atlas", owner: "Maya Chen", status: "Active" },
  { id: "beacon", name: "Beacon", owner: "Omar Khan", status: "Planning" },
];

export function ProjectTable() {
  return (
    <TableContainer component={Paper}>
      <Table aria-label="Project overview">
        <caption>Projects, owners, and current status</caption>
        <TableHead>
          <TableRow>
            <TableCell scope="col">Project</TableCell>
            <TableCell scope="col">Owner</TableCell>
            <TableCell scope="col">Status</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {projects.map((project) => (
            <TableRow key={project.id} hover>
              <TableCell component="th" scope="row">
                {project.name}
              </TableCell>
              <TableCell>{project.owner}</TableCell>
              <TableCell>{project.status}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

The caption explains the table's subject. The first data cell is a row heading, which can make it easier to associate each value with its record. The hover prop is only a visual aid; it does not make a row interactive.

## Control density and alignment

The size prop can create a denser table. The padding prop supports normal, checkbox, or none depending on the layout. Align numbers and short values consistently, often to the right, while keeping descriptive text left aligned.

Use sx to set column widths or prevent one long value from taking over the table. Avoid shrinking text until it is difficult to read. If the table has many columns, consider whether users need every field at once.

## Add sorting

TableSortLabel provides a button-like control for a sortable heading. Keep sort state in React and sort the full record collection before slicing it for pagination. Apply aria-sort through the heading cell's sortDirection prop so the current sort is exposed.

```jsx
import { useState } from "react";
import { TableCell, TableSortLabel } from "@mui/material";

function SortableHeading({ id, label, orderBy, order, onSort }) {
  const active = orderBy === id;

  return (
    <TableCell scope="col" sortDirection={active ? order : false}>
      <TableSortLabel
        active={active}
        direction={active ? order : "asc"}
        onClick={() => onSort(id)}
      >
        {label}
      </TableSortLabel>
    </TableCell>
  );
}

function toggleSort(id, setOrderBy, setOrder) {
  setOrder((current) =>
    id === current.id && current.direction === "asc" ? "desc" : "asc",
  );
  setOrderBy(id);
}
```

TableSortLabel handles the sort affordance. Your application still needs to compare values, update the sort key and direction, and keep that state consistent when data changes. Do not use an arrow icon by itself as the only indication that a column can be sorted.

## Add pagination

TablePagination uses a zero-based page value. Its count is the total number of rows available, while rowsPerPage controls the current slice.

```jsx
const [page, setPage] = useState(0);
const [rowsPerPage, setRowsPerPage] = useState(5);

const pageRows = sortedRows.slice(
  page * rowsPerPage,
  page * rowsPerPage + rowsPerPage,
);

<TablePagination
  component="div"
  count={sortedRows.length}
  page={page}
  rowsPerPage={rowsPerPage}
  rowsPerPageOptions={[5, 10, 25]}
  onPageChange={(_event, nextPage) => setPage(nextPage)}
  onRowsPerPageChange={(event) => {
    setRowsPerPage(Number(event.target.value));
    setPage(0);
  }}
/>
```

Reset to page zero when the page size changes. Also reset the page when a search or filter reduces the number of matching rows; otherwise the current page might be beyond the end of the filtered result.

## Responsive tables

Tables need enough width to keep their columns readable. A common approach is to let TableContainer scroll horizontally:

```jsx
<TableContainer sx={{ overflowX: "auto" }}>
  <Table sx={{ minWidth: 650 }}>
    {/* table head and body */}
  </Table>
</TableContainer>
```

For smaller screens, choose intentionally between horizontal scrolling, hiding low-priority columns, or presenting each record as a card. Preserve labels and relationships if you change the layout. Do not make users guess what a value means after its column heading disappears.

The stickyHeader prop can keep a heading visible while a long table scrolls inside a constrained container. Check the container height and stacking behavior so the heading does not cover other content.

## Empty, loading, and error states

A table should explain why it has no rows. Distinguish between:

- No records exist yet.
- A filter returned no matches.
- Data is loading.
- Loading failed.

Do not render a blank body for every case. Keep the headings when helpful and place a clear message in a cell spanning the appropriate number of columns, or show a nearby status message. If the table is refreshing, expose the loading state to assistive technology and avoid announcing every cell again unnecessarily.

## Choosing Table or Data Grid

Use the standard Table components when you need semantic rows and columns with a modest amount of custom behavior. They work well for reports, settings, summaries, and straightforward record lists.

Consider MUI X Data Grid when you need capabilities such as column virtualization, extensive editing, column menus, or advanced filtering at scale. A grid has a richer interaction model and requires careful keyboard and accessibility behavior. Do not choose it solely because the data happens to be displayed in rows.

## Common mistakes

- Leaving out a useful caption or accessible table name.
- Rendering headings as ordinary data cells.
- Using the array index as a key for changing data.
- Paginating before sorting, which sorts only the current page.
- Forgetting to reset the page after changing page size or filters.
- Making a row look interactive with hover but providing no action.
- Hiding a column on mobile without preserving its label and meaning.
- Using color alone to show status or sort direction.
- Rebuilding a complex data grid from basic Table components when a dedicated grid fits better.

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
      <td><a href="029-chips-avatars-and-badges.md">029 - Chips, Avatars, and Badges</a></td>
      <td><a href="../qnas/030-tables-and-data-display.md">Q&amp;A</a></td>
      <td><a href="../examples/030-tables-and-data-display.md">Example</a></td>
      <td><a href="031-mui-x-data-grid-community.md">031 - MUI X Data Grid Community</a></td>
    </tr>
  </tbody>
</table>
