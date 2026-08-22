# Week 04: Styling, Theming & Dark Mode
> **Month 1 | Week 4 of 16** | **Difficulty:** Intermediate

---

## Goal
Master mobile styling — responsive design, theming, and dark mode support.

---

## Tasks

### Day 1: Responsive Design
- [ ] Read: https://reactnative.dev/docs/dimensions
- [ ] No media queries — use `useWindowDimensions`:
  ```typescript
  import { useWindowDimensions } from 'react-native';
  
  const { width, height } = useWindowDimensions();
  const isSmallScreen = width < 375;
  ```
- [ ] Build: A layout that adapts between phone and tablet
- [ ] Use percentage widths where appropriate:
  ```typescript
  <View style={{ width: '80%', alignSelf: 'center' }}>
  ```
- [ ] Verify: Layout works on iPhone SE, iPhone 14, iPad

### Day 2: Dark Mode Implementation
- [ ] Create a theme context:
  ```typescript
  type Theme = 'light' | 'dark';
  
  const ThemeContext = createContext<{ theme: Theme; toggle: () => void }>(...);
  
  // Provider
  const [theme, setTheme] = useState<Theme>('light');
  const toggle = () => setTheme(t => t === 'light' ? 'dark' : 'light');
  ```
- [ ] Create theme tokens:
  ```typescript
  const themes = {
    light: { bg: '#ffffff', text: '#000000', card: '#f5f5f5' },
    dark: { bg: '#121212', text: '#ffffff', card: '#1e1e1e' },
  };
  ```
- [ ] Apply theme to all screens via context
- [ ] Verify: Toggle switches all screens between light/dark

### Day 3: Custom Fonts & Icons
- [ ] Install Expo fonts: `npx expo install expo-font`
- [ ] Load custom font:
  ```typescript
  import { useFonts } from 'expo-font';
  
  const [loaded] = useFonts({
    'Inter-Regular': require('./assets/fonts/Inter-Regular.ttf'),
    'Inter-Bold': require('./assets/fonts/Inter-Bold.ttf'),
  });
  ```
- [ ] Install icons: `npx expo install @expo/vector-icons`
- [ ] Use icons: `<Ionicons name="heart" size={24} color="red" />`
- [ ] Verify: Custom fonts and icons render correctly

### Day 4: Common UI Patterns
- [ ] Build reusable components:
  1. **Card** — Rounded corners, shadow, padding
  2. **Avatar** — Circular image with fallback initials
  3. **Badge** — Small notification counter
  4. **Divider** — Horizontal line separator
- [ ] Use `Platform` for platform-specific styling:
  ```typescript
  import { Platform } from 'react-native';
  
  const styles = StyleSheet.create({
    shadow: Platform.select({
      ios: { shadowColor: '#000', shadowOffset: { width: 0, height: 2 } },
      android: { elevation: 4 },
    }),
  });
  ```
- [ ] Verify: Components look correct on iOS and Android

### Day 5: Month 1 Project — Note-Taking App
- [ ] Combine everything from Weeks 1-4
- [ ] Build a note-taking app with:
  - Home screen with list of notes (FlatList)
  - Create/edit note screen (TextInput, validation)
  - Note detail screen
  - Dark mode toggle
  - Custom fonts and icons
  - Responsive layout
- [ ] Verify: All features work, app runs on physical device

---

## Key Concepts

### Styling Rules

| Rule | Why |
|------|-----|
| No CSS cascade | Styles are isolated, predictable |
| camelCase only | `backgroundColor` not `background-color` |
| Numbers for pixels | `fontSize: 16` not `fontSize: '16px'` |
| `StyleSheet.create()` | Better than inline objects (optimization) |
| Platform.select() | Different styles for iOS/Android |

### Shadow Pattern
```typescript
// iOS
shadowColor: '#000',
shadowOffset: { width: 0, height: 2 },
shadowOpacity: 0.1,
shadowRadius: 4,

// Android
elevation: 4,
```

---

## Month 1 Project: Note-Taking App

### Requirements
- [ ] Home screen with FlatList of notes
- [ ] Create new note (title + content)
- [ ] Edit existing note
- [ ] Delete note (swipe or long press)
- [ ] Dark mode toggle
- [ ] Notes persist in AsyncStorage
- [ ] Responsive layout
- [ ] Proper error handling

### File Structure
```
app/
  _layout.tsx
  index.tsx          # Home - list of notes
  create.tsx         # Create new note
  edit/[id].tsx      # Edit note
components/
  NoteCard.tsx
  EmptyState.tsx
  ThemeToggle.tsx
context/
  ThemeContext.tsx
  NotesContext.tsx
utils/
  storage.ts         # AsyncStorage helpers
```

### Verification
```bash
npx expo start --clear

# Test:
# 1. Create a note → appears in list
# 2. Edit a note → changes save
# 3. Delete a note → removed from list
# 4. Dark mode toggle → all screens change
# 5. Close and reopen app → notes still there
# 6. Works on iOS and Android
```

---

**Done when:** Note-taking app is complete and verified on both platforms.
