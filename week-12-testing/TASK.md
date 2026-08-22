# Week 12: Testing
> **Month 3 | Week 12 of 16** | **Difficulty:** Advanced

---

## Goal
Write tests that catch bugs — unit, integration, and E2E.

---

## Tasks

### Day 1: Jest Unit Tests
- [ ] Jest comes with Expo — just run `npx jest`
- [ ] Write your first test:
  ```typescript
  // __tests__/utils/storage.test.ts
  import { storage } from '../../utils/storage';
  
  describe('storage', () => {
    it('should save and retrieve data', async () => {
      await storage.set('test-key', { name: 'John' });
      const result = await storage.get('test-key');
      expect(result).toEqual({ name: 'John' });
    });
  
    it('should return null for missing key', async () => {
      const result = await storage.get('nonexistent');
      expect(result).toBeNull();
    });
  });
  ```
- [ ] Test pure functions:
  ```typescript
  // utils/formatters.ts
  export function formatPrice(cents: number): string {
    return `$${(cents / 100).toFixed(2)}`;
  }
  
  // __tests__/utils/formatters.test.ts
  import { formatPrice } from '../../utils/formatters';
  
  describe('formatPrice', () => {
    it('formats cents to dollars', () => {
      expect(formatPrice(1999)).toBe('$19.99');
    });
  
    it('handles zero', () => {
      expect(formatPrice(0)).toBe('$0.00');
    });
  
    it('handles cents', () => {
      expect(formatPrice(50)).toBe('$0.50');
    });
  });
  ```
- [ ] Verify: All tests pass

### Day 2: React Native Testing Library
- [ ] Install: `npm install --save-dev @testing-library/react-native`
- [ ] Test components:
  ```typescript
  // __tests__/components/NoteCard.test.tsx
  import { render, screen, fireEvent } from '@testing-library/react-native';
  import { NoteCard } from '../../components/NoteCard';
  
  describe('NoteCard', () => {
    const mockNote = { id: '1', title: 'Test Note', content: 'Hello' };
  
    it('renders note title', () => {
      render(<NoteCard note={mockNote} onPress={() => {}} />);
      expect(screen.getByText('Test Note')).toBeTruthy();
    });
  
    it('calls onPress when tapped', () => {
      const onPress = jest.fn();
      render(<NoteCard note={mockNote} onPress={onPress} />);
      fireEvent.press(screen.getByText('Test Note'));
      expect(onPress).toHaveBeenCalledWith('1');
    });
  
    it('renders empty state when no notes', () => {
      render(<NoteCard note={null} onPress={() => {}} />);
      expect(screen.getByText('No notes yet')).toBeTruthy();
    });
  });
  ```
- [ ] Use factory pattern for test data:
  ```typescript
  // __tests__/factories.ts
  export const createMockNote = (overrides = {}) => ({
    id: '1',
    title: 'Test Note',
    content: 'Test content',
    createdAt: new Date().toISOString(),
    ...overrides,
  });
  ```
- [ ] Verify: Component tests pass

### Day 3: Mocking
- [ ] Mock modules:
  ```typescript
  // Mock AsyncStorage
  jest.mock('@react-native-async-storage/async-storage', () =>
    require('@react-native-async-storage/async-storage/jest/async-storage-mock')
  );
  
  // Mock API calls
  jest.mock('../../services/api', () => ({
    fetchMovies: jest.fn(),
    searchMovies: jest.fn(),
  }));
  
  // Use mock in test
  const mockFetchMovies = require('../../services/api').fetchMovies;
  mockFetchMovies.mockResolvedValue([{ id: '1', title: 'Test Movie' }]);
  ```
- [ ] Mock navigation:
  ```typescript
  jest.mock('expo-router', () => ({
    useRouter: () => ({ push: jest.fn(), back: jest.fn() }),
    useLocalSearchParams: () => ({ id: '1' }),
  }));
  ```
- [ ] Verify: Mocks work, tests are isolated

### Day 4: E2E Testing with Maestro
- [ ] Install Maestro CLI: https://maestro.mobile.dev/
- [ ] Write first flow:
  ```yaml
  # .maestro/note-app.yaml
  appId: com.myapp
  ---
  - launchApp
  - tapOn: "Create Note"
  - inputText: "My Test Note"
  - tapOn: "Save"
  - assertVisible: "My Test Note"
  - tapOn: "My Test Note"
  - assertVisible: "My Test Note"
  - tapOn: "Delete"
  - assertVisible: "No notes yet"
  ```
- [ ] Run: `maestro test .maestro/note-app.yaml`
- [ ] Build 3 E2E flows:
  1. Create and view note
  2. Edit note
  3. Delete note
- [ ] Verify: E2E flows pass

### Day 5: Test Coverage & CI
- [ ] Run with coverage: `npx jest --coverage`
- [ ] Check coverage report
- [ ] Set up GitHub Actions:
  ```yaml
  # .github/workflows/test.yml
  name: Test
  on: [push, pull_request]
  jobs:
    test:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: 18
        - run: npm install
        - run: npx jest --coverage
  ```
- [ ] Verify: CI runs tests on every push

---

## Testing Pyramid

```
       E2E (Maestro)
      ╱    (few)    ╲
     ╱ Integration    ╲
    ╱   (some)         ╲
   ╱ Unit Tests (many)  ╲
  ╱______________________╲
```

| Type | Count | Speed | What |
|------|-------|-------|------|
| Unit | Many | Fast | Functions, utilities |
| Integration | Some | Medium | Components + hooks |
| E2E | Few | Slow | Full user flows |

---

## Verification

```bash
# Unit tests
npx jest --coverage

# E2E tests
maestro test .maestro/

# Check:
# 1. All unit tests pass
# 2. Coverage > 60%
# 3. E2E flows pass
# 4. No flaky tests
```
