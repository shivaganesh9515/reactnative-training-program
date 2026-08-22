# Week 02: Navigation
> **Month 1 | Week 2 of 16** | **Difficulty:** Beginner-Intermediate

---

## Goal
Master screen navigation — the single biggest paradigm shift from Next.js.

---

## Tasks

### Day 1: Install & Setup Expo Router
- [ ] Read: https://docs.expo.dev/router/introduction/
- [ ] Understand: Expo Router uses file-based routing (like Next.js App Router)
- [ ] Create new project: `npx create-expo-app NavigationApp --template blank-typescript`
- [ ] Install Expo Router: `npx expo install expo-router`
- [ ] Set up file structure:
  ```
  app/
    _layout.tsx       # Root layout
    index.tsx         # Home screen (/)
    profile.tsx       # Profile screen (/profile)
  ```
- [ ] Verify: Tapping between screens works

### Day 2: Stack Navigation
- [ ] Read: https://docs.expo.dev/router/layouts/tabs/
- [ ] Build a stack navigator with 3 screens:
  - Home → Details → Settings
- [ ] Pass params between screens:
  ```typescript
  // Navigate
  router.push({ pathname: '/details', params: { id: '123', title: 'Item' } });
  
  // Receive
  const { id, title } = useLocalSearchParams<{ id: string; title: string }>();
  ```
- [ ] Verify: Can navigate forward and back, params arrive correctly

### Day 3: Tab Navigation
- [ ] Read: https://docs.expo.dev/router/layouts/tabs/
- [ ] Create bottom tab navigation:
  ```typescript
  // app/_layout.tsx
  <Tabs>
    <Tabs.Screen name="index" options={{ title: 'Home', tabBarIcon: ... }} />
    <Tabs.Screen name="search" options={{ title: 'Search', tabBarIcon: ... }} />
    <Tabs.Screen name="profile" options={{ title: 'Profile', tabBarIcon: ... }} />
  </Tabs>
  ```
- [ ] Add icons using `@expo/vector-icons`
- [ ] Verify: Bottom tabs work, icons show, active tab highlights

### Day 4: Dynamic Routes & Deep Linking
- [ ] Create dynamic route: `app/item/[id].tsx`
- [ ] Test deep link: `exp://your-app-url/item/123`
- [ ] Build: A list on Home screen where each item navigates to `/item/[id]`
- [ ] Verify: Tapping different items shows correct detail screen

### Day 5: Navigation Patterns Comparison
- [ ] Build all three patterns in one app:
  1. Stack (push/pop)
  2. Tabs (bottom bar)
  3. Modal (overlay)
- [ ] Understand when to use each:
  ```
  Stack:  Linear flow (signup, checkout)
  Tabs:   Top-level sections (home, search, profile)
  Modal:  Temporary actions (confirm, edit, share)
  ```

---

## Key Concepts

### Next.js vs React Native Navigation

| Concept | Next.js | React Native |
|---------|---------|--------------|
| File routing | `app/page.tsx` | `app/index.tsx` |
| Dynamic route | `[id].tsx` | `[id].tsx` (same!) |
| Params | `useParams()` | `useLocalSearchParams()` |
| Navigate | `<Link>` / `router.push()` | `router.push()` |
| Layout | `layout.tsx` | `_layout.tsx` |
| Back button | Browser back | `router.back()` or swipe |

### Navigation File Structure
```
app/
  _layout.tsx          # Root layout (wraps everything)
  index.tsx            # /
  (tabs)/
    _layout.tsx        # Tab layout
    index.tsx          # / (tabs)
    search.tsx         # /search
  item/
    [id].tsx           # /item/:id
  modal.tsx            # Modal overlay
```

---

## Common Mistakes This Week

1. **Not wrapping in `<Stack>` or `<Tabs>`** — Navigation won't work without layout wrapper.
2. **Wrong file name** — `_layout.tsx` not `layout.tsx` (Expo Router uses underscore prefix).
3. **Using `href`** — That's web. Use `router.push()` in React Native.
4. **Forgetting back navigation** — Always provide a way to go back (header button or swipe).

---

## Verification

```bash
npx expo start --clear

# Test these flows:
# 1. Home → tap item → Details screen shows item data
# 2. Back button/gesture returns to Home
# 3. Bottom tabs switch between sections
# 4. Tab icons display correctly
# 5. Modal opens and closes
# 6. Deep link works (if testing on device)
```

**Done when:** You can navigate between 5+ screens using stack, tabs, and params.

---

## Resources
- Expo Router docs: https://docs.expo.dev/router/
- Navigation patterns: https://reactnavigation.org/docs/getting-started
