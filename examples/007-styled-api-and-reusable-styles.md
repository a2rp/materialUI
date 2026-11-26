# The styled API and Reusable Styles - Example

## Goal

Create a reusable theme-aware notice card with styled, then use it in two places with different local sx values. This demonstrates the difference between shared base styles and per-instance adjustments.

## Prerequisites

- A React application with Material UI and Emotion installed.
- ThemeProvider configured at the app root.

## File structure

```text
src/
  App.jsx
  components/
    NoticeCard.jsx
```

## 1. Create the reusable styled component

Add `src/components/NoticeCard.jsx`:

```jsx
import Paper from '@mui/material/Paper';
import { styled } from '@mui/material/styles';

const NoticeSurface = styled(Paper)(({ theme }) => ({
  padding: theme.spacing(2),
  borderLeft: '4px solid ' + theme.palette.info.main,
  borderRadius: theme.shape.borderRadius,
  backgroundColor: theme.palette.background.paper,
  transition: theme.transitions.create('box-shadow'),
  '&:hover': {
    boxShadow: theme.shadows[3],
  },
}));

export default function NoticeCard({ title, children }) {
  return (
    <NoticeSurface component="section" aria-label={title}>
      <strong>{title}</strong>
      <div>{children}</div>
    </NoticeSurface>
  );
}
```

The styled surface uses the theme for spacing, palette, shape, transition, and shadow. NoticeCard adds a small product-level interface around that surface. Its children remain regular React content.

## 2. Use it in the application

Update `src/App.jsx`:

```jsx
import Stack from '@mui/material/Stack';
import Typography from '@mui/material/Typography';
import NoticeCard from './components/NoticeCard';

export default function App() {
  return (
    <Stack spacing={2} sx={{ maxWidth: 760, mx: 'auto', p: 3 }}>
      <Typography variant="h4" component="h1">
        Notices
      </Typography>

      <NoticeCard title="Profile incomplete">
        Add a contact number to finish your profile.
      </NoticeCard>

      <NoticeCard
        title="New feature"
        sx={{ bgcolor: 'action.hover', maxWidth: 600 }}
      >
        You can now review your account activity.
      </NoticeCard>
    </Stack>
  );
}
```

There is an important issue in this first version: NoticeCard does not accept or forward sx, so the second call's sx prop is ignored. Update the component to support it.

## 3. Forward caller styles

Update `src/components/NoticeCard.jsx` to accept sx and pass it through to the styled surface:

```jsx
import Paper from '@mui/material/Paper';
import { styled } from '@mui/material/styles';

const NoticeSurface = styled(Paper)(({ theme }) => ({
  padding: theme.spacing(2),
  borderLeft: '4px solid ' + theme.palette.info.main,
  borderRadius: theme.shape.borderRadius,
  backgroundColor: theme.palette.background.paper,
  transition: theme.transitions.create('box-shadow'),
  '&:hover': {
    boxShadow: theme.shadows[3],
  },
}));

export default function NoticeCard({ title, children, sx }) {
  return (
    <NoticeSurface
      component="section"
      aria-label={title}
      sx={sx}
    >
      <strong>{title}</strong>
      <div>{children}</div>
    </NoticeSurface>
  );
}
```

The styled component already accepts sx. The wrapper must expose and forward the value if callers are meant to customize it.

## Run the project

Use the development command declared by the React application. For a Vite starter, this is commonly:

```sh
npm run dev
```

## Expected result

Two notice cards appear with a shared info-colored left border, spacing, surface styling, and hover shadow. The second card also receives its local background and maximum-width adjustments because the wrapper forwards sx.

## What to notice

- The reusable base style is defined once at module scope.
- Shared style values come from the active theme.
- The wrapper's props form its public API.
- sx only reaches the inner styled component because the wrapper forwards it.
- The caller can add local styles without editing the base NoticeSurface definition.
- The section element has a label derived from the visible title.

## Variations

1. Add a tone prop with a limited set of allowed values and choose the border color from the theme palette.
2. Filter the tone prop if it would otherwise be forwarded to a native DOM element.
3. Give the component a default title only if that default has a clear product meaning.
4. Compare the component with a direct Paper plus sx implementation. Keep the wrapper only if reuse or API clarity justifies it.

## Common mistakes

- **Assuming wrapper props are automatically forwarded:** explicitly accept and pass through the props the wrapper supports.
- **Passing sx but dropping it:** style custom components only when they preserve caller customization.
- **Defining NoticeSurface inside NoticeCard:** keep styled components stable at module scope.
- **Creating a wrapper without added value:** use Paper directly if the component is not reused and adds no meaningful API.
- **Passing arbitrary style-only props to DOM:** filter custom props where needed.
- **Using invalid aria-label text:** use a label that identifies the region meaningfully.

## Practice check

Remove the sx prop from the second NoticeCard call and compare its appearance. Then explain which styles are shared by both cards and which are specific to the second instance.

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
      <td><a href="006-sx-prop-and-theme-aware-styles.md">006 - The sx prop and theme-aware styles</a></td>
      <td><a href="../lessons/007-styled-api-and-reusable-styles.md">Lesson</a></td>
      <td><a href="../qnas/007-styled-api-and-reusable-styles.md">Q&amp;A</a></td>
      <td><a href="008-css-overrides-and-global-styles.md">008 - CSS overrides and global styles</a></td>
    </tr>
  </tbody>
</table>
