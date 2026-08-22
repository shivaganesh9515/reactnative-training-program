# Week 05: State Management
> **Month 2 | Week 5 of 16** | **Difficulty:** Intermediate

---

## Goal
Master state management patterns that scale — Context, Zustand, and server state.

---

## Tasks

### Day 1: Context API + useReducer
- [ ] Review: Context you used in Week 04 (ThemeContext) — now add complex state
- [ ] Build a notes context with full CRUD:
  ```typescript
  type Action =
    | { type: 'ADD'; note: Note }
    | { type: 'UPDATE'; id: string; note: Partial<Note> }
    | { type: 'DELETE'; id: string };
  
  function notesReducer(state: Note[], action: Action): Note[] {
    switch (action.type) {
      case 'ADD': return [...state, action.note];
      case 'UPDATE': return state.map(n => n.id === action.id ? { ...n, ...action.note } : n);
      case 'DELETE': return state.filter(n => n.id !== action.id);
    }
  }
  ```
- [ ] Wrap app in provider, consume in screens
- [ ] Verify: CRUD operations work across screens

### Day 2: Zustand (Recommended for Most Cases)
- [ ] Install: `npm install zustand`
- [ ] Read: https://docs.pmnd.rs/zustand/getting-started/introduction
- [ ] Refactor your NotesContext to Zustand:
  ```typescript
  import { create } from 'zustand';
  
  interface NotesStore {
    notes: Note[];
    addNote: (note: Note) => void;
    updateNote: (id: string, note: Partial<Note>) => void;
    deleteNote: (id: string) => void;
  }
  
  export const useNotesStore = create<NotesStore>((set) => ({
    notes: [],
    addNote: (note) => set((state) => ({ notes: [...state.notes, note] })),
    updateNote: (id, note) => set((state) => ({
      notes: state.notes.map(n => n.id === id ? { ...n, ...note } : n)
    })),
    deleteNote: (id) => set((state) => ({
      notes: state.notes.filter(n => n.id !== id)
    })),
  }));
  ```
- [ ] Use in components: `const { notes, addNote } = useNotesStore();`
- [ ] Verify: Same functionality, much cleaner code

### Day 3: TanStack Query (Server State)
- [ ] Install: `npm install @tanstack/react-query`
- [ ] Understand the split:
  ```
  Client state: UI state, form state, theme → Zustand/Context
  Server state: API data, cache, sync → TanStack Query
  ```
- [ ] Set up QueryClient provider in `_layout.tsx`
- [ ] Fetch data:
  ```typescript
  import { useQuery } from '@tanstack/react-query';
  
  function MovieList() {
    const { data, isLoading, error } = useQuery({
      queryKey: ['movies'],
      queryFn: () => fetch('https://api.example.com/movies').then(r => r.json()),
    });
  
    if (isLoading) return <ActivityIndicator />;
    if (error) return <Text>Error loading movies</Text>;
  
    return <FlatList data={data} renderItem={...} />;
  }
  ```
- [ ] Verify: Data fetches, loading states show, errors handled

### Day 4: Mutations with TanStack Query
- [ ] Add, update, delete via API:
  ```typescript
  import { useMutation, useQueryClient } from '@tanstack/react-query';
  
  function AddMovie() {
    const queryClient = useQueryClient();
    
    const mutation = useMutation({
      mutationFn: (newMovie) => fetch('/api/movies', { method: 'POST', body: JSON.stringify(newMovie) }),
      onSuccess: () => {
        queryClient.invalidateQueries({ queryKey: ['movies'] });
      },
    });
  
    return <Button onPress={() => mutation.mutate({ title: 'New Movie' })} />;
  }
  ```
- [ ] Add optimistic updates (update UI before server confirms)
- [ ] Verify: Mutations work, cache updates, loading states

### Day 5: State Management Decision Framework
- [ ] Build a cheat sheet for your team:
  | State Type | Tool | Example |
  |-----------|------|---------|
  | UI state (modals, toggles) | `useState` | isOpen, activeTab |
  | Complex shared state | Zustand | cart, notes, user preferences |
  | Server data | TanStack Query | API responses, cached data |
  | Form state | React Hook Form | Multi-step forms |
  | Theme/auth | Context | Used by entire app |
- [ ] Refactor Week 04 project to use Zustand instead of Context
- [ ] Verify: All state works, no prop drilling

---

## Key Concepts

### State Management Hierarchy
```
useState          → Simple, local, one component
useReducer        → Complex local state with actions
Context           → Theme, auth, simple global state
Zustand           → Complex global state (recommended)
TanStack Query    → Server data, caching, sync
```

### Anti-Patterns
- Passing state through 3+ levels → Use Zustand or Context
- Storing API data in useState → Use TanStack Query
- One giant global store → Split by feature

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. Notes CRUD works (add, edit, delete)
# 2. State persists when navigating between screens
# 3. API data loads with loading indicator
# 4. Error states display correctly
# 5. Mutations update the UI
# 6. No unnecessary re-renders (add console.log to check)
```
