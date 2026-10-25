# Roadmap

The project is already scaffolded (Vite, React, TypeScript, React Router, Vitest, React Testing Library) with the template demo content removed, and `npm test` runs.

## Phase 1 — Shell and Home Page

- Create `src/main.tsx` that renders `<BrowserRouter><App /></BrowserRouter>` and imports `src/styles.css`
- Create `src/App.tsx` that renders the `Navbar` and a `<Routes>` block with:
  - `/` showing `HomePage`
  - `/pantry` showing a placeholder heading "The Pantry" (replaced in Phase 2)
- Create `src/components/Navbar.tsx` with:
  - The brand text "Prompt Pantry"
  - `NavLink` links to Home (`/`) and Pantry (`/pantry`)
- Create `src/pages/HomePage.tsx` with:
  - A hero section with the tagline: *"Stock your shelf. Share a prompt."*
  - A brief welcoming paragraph about the pantry
  - A link styled as a button that goes to `/pantry`
- Create `src/styles.css` with base styles for the page layout, navbar and hero
- Set the `<title>` in `index.html` to "Prompt Pantry"
- Write tests in `src/App.test.tsx`, rendering `App` inside a `MemoryRouter`:
  - At `/`, the tagline text is visible
  - The navbar has a "Home" link and a "Pantry" link

## Phase 2 — Pantry Board

- Create `src/types.ts` exporting a `Prompt` type:
  - Fields: `id: string`, `title: string`, `text: string`, `tag: string`, `createdAt: Date`
- Create `src/data/seedPrompts.ts` exporting `seedPrompts: Prompt[]` with 4 seed prompts (generic developer prompts across the tags `debugging`, `refactor`, `testing` and `review`)
- Create `src/components/PromptCard.tsx`:
  - Props: `prompt: Prompt`
  - Shows the title, the tag as a badge, the creation date (formatted with `toLocaleDateString`) and the prompt text
- Create `src/pages/PantryPage.tsx`:
  - Props: `prompts: Prompt[]`
  - A heading: "The Pantry"
  - One `PromptCard` per prompt
  - When `prompts` is empty, show the text "Your shelf is empty. Add the first prompt."
- Update `src/App.tsx`:
  - Keep the prompts in state with `useState<Prompt[]>(seedPrompts)`
  - Replace the placeholder at `/pantry` with `PantryPage`, passing the prompts
- Add card and badge styles to `src/styles.css`
- Write validation tests in `src/pages/PantryPage.test.tsx`:
  - The "The Pantry" heading is visible
  - The title of at least one seed prompt is visible
  - One card is rendered for each prompt passed in
  - An empty list shows the empty-shelf message

## Phase 3 — Add Prompt Form

- Create `src/components/AddPromptForm.tsx`:
  - Props: `onAdd: (input: { title: string; text: string; tag: string }) => void`
  - A controlled form with a "Title" text input, a "Tag" select (`debugging`, `refactor`, `testing`, `review`) and a "Prompt text" textarea, each with a proper label
  - A submit button labelled "Add prompt"
  - Validation on submit: if the trimmed title is empty show "Title is required."; if the trimmed text is empty show "Prompt text is required." Show each message with `role="alert"` and do not call `onAdd`
  - On a valid submit, call `onAdd` and reset the form
- Update `src/App.tsx`:
  - Add an `addPrompt` function that builds a `Prompt` with `id` from `crypto.randomUUID()` and `createdAt` set to `new Date()`, and puts it at the top of the list
  - Pass `addPrompt` to `PantryPage`
- Update `src/pages/PantryPage.tsx` to take an `onAdd` prop and render `AddPromptForm` below the cards
- Add form and error styles to `src/styles.css`
- Write validation tests in `src/App.test.tsx`, rendering `App` inside a `MemoryRouter` at `/pantry`:
  - Submitting a valid title and text adds a card with that title to the page
  - Submitting with an empty title shows "Title is required." and adds no card
  - Submitting with empty prompt text shows "Prompt text is required." and adds no card
  - After a valid submit, the form fields are empty
