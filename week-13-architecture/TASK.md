# Week 13: Architecture & Code Organization
> **Month 4 | Week 13 of 16** | **Difficulty:** Advanced

---

## Goal
Structure your code like a production app — feature folders, custom hooks, error boundaries.

---

## Tasks

### Day 1: Feature-Based Folder Structure
- [ ] Reorganize your project:
  ```
  src/
    features/
      notes/
        components/
          NoteCard.tsx
          NoteList.tsx
          NoteForm.tsx
        hooks/
          useNotes.ts
          useNoteMutations.ts
        services/
          notesApi.ts
        types.ts
        index.ts          # Public exports
      auth/
        components/
          LoginForm.tsx
          SignupForm.tsx
        hooks/
          useAuth.ts
        services/
          authApi.ts
        context/
          AuthProvider.tsx
        types.ts
        index.ts
    shared/
      components/
        Button.tsx
        Loading.tsx
        ErrorState.tsx
      hooks/
        useNetworkStatus.ts
      utils/
        storage.ts
        formatters.ts
    app/
      _layout.tsx
      index.tsx
      (tabs)/
  ```
- [ ] Import from feature index: `import { NoteCard } from '@/features/notes'`
- [ ] Verify: Imports are clean, no circular dependencies

### Day 2: Custom Hooks Extraction
- [ ] Extract reusable logic into hooks:
  ```typescript
  // shared/hooks/useDebounce.ts
  export function useDebounce<T>(value: T, delay: number): T {
    const [debouncedValue, setDebouncedValue] = useState(value);
  
    useEffect(() => {
      const timer = setTimeout(() => setDebouncedValue(value), delay);
      return () => clearTimeout(timer);
    }, [value, delay]);
  
    return debouncedValue;
  }
  
  // shared/hooks/useRefreshOnFocus.ts
  export function useRefreshOnFocus(refetch: () => void) {
    const isFocused = useIsFocused();
    useEffect(() => {
      if (isFocused) refetch();
    }, [isFocused]);
  }
  ```
- [ ] Build 3 custom hooks for your app
- [ ] Verify: Hooks are reusable, well-tested

### Day 3: Error Boundaries
- [ ] Implement global error handling:
  ```typescript
  // shared/components/ErrorHandler.tsx
  class ErrorHandler extends React.Component<Props, State> {
    state = { hasError: false, error: null };
  
    static getDerivedStateFromError(error: Error) {
      return { hasError: true, error };
    }
  
    componentDidCatch(error: Error, errorInfo: React.ErrorInfo) {
      // Log to crash reporting
      Sentry.captureException(error, { extra: errorInfo });
    }
  
    render() {
      if (this.state.hasError) {
        return <ErrorScreen error={this.state.error} onRetry={() => this.setState({ hasError: false })} />;
      }
      return this.props.children;
    }
  }
  ```
- [ ] Wrap app in ErrorHandler
- [ ] Add error boundaries per feature
- [ ] Verify: Crashes show user-friendly error, not white screen

### Day 4: TypeScript Strictness
- [ ] Enable strict mode in `tsconfig.json`:
  ```json
  {
    "compilerOptions": {
      "strict": true,
      "noUncheckedIndexedAccess": true,
      "noUnusedLocals": true,
      "noUnusedParameters": true
    }
  }
  ```
- [ ] Fix all TypeScript errors
- [ ] Add proper types to all props and state
- [ ] Verify: `npx tsc --noEmit` passes with zero errors

### Day 5: Code Quality
- [ ] Set up ESLint + Prettier:
  ```bash
  npm install --save-dev eslint prettier eslint-config-expo
  ```
- [ ] Run linter: `npx eslint .`
- [ ] Fix all warnings and errors
- [ ] Add pre-commit hook:
  ```bash
  npm install --save-dev husky lint-staged
  npx husky init
  ```
  ```json
  // package.json
  "lint-staged": {
    "*.{ts,tsx}": ["eslint --fix", "prettier --write"]
  }
  ```
- [ ] Verify: No lint errors, consistent formatting

---

## Architecture Principles

| Principle | Implementation |
|-----------|---------------|
| Feature isolation | Each feature in its own folder |
| Shared code | Common components in `shared/` |
| Barrel exports | `index.ts` exports public API |
| Custom hooks | Extract reusable logic |
| Error boundaries | Per-feature error handling |
| TypeScript strict | No `any`, proper types |

---

## Verification

```bash
npx tsc --noEmit        # TypeScript passes
npx eslint .            # No lint errors
npx jest --coverage     # Tests pass, coverage > 60%
npx expo start --clear  # App runs
```
