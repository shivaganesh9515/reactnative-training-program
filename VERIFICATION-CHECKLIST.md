# Verification Checklist (Examiner Role)
> Use this to verify a junior developer has completed each week's tasks.

---

## How to Use
1. Open the week's `TASK.md`
2. Run through each verification item below
3. Check off items as you verify
4. Note any issues in the Notes section
5. Mark PASS / PASS w/ notes / RETRY

---

## Month 1 Verification

### Week 01: Fundamentals
- [ ] `npx expo start --clear` runs without errors
- [ ] App loads on physical device (Expo Go)
- [ ] Profile card renders with image, name, bio
- [ ] Settings screen has proper rows
- [ ] Buttons respond to press (Follow toggle works)
- [ ] Layout works on small AND large screens
- [ ] No red error screens

**Questions to ask:**
- What's the difference between `<View>` and `<div>`?
- Why can't you use `onClick` in React Native?
- What happens if you put text directly inside a `<View>`?

### Week 02: Navigation
- [ ] 3+ screens navigable (Home → Details → Settings)
- [ ] Params passed between screens correctly
- [ ] Bottom tabs work with icons
- [ ] Back navigation works (button and swipe)
- [ ] Dynamic route `[id].tsx` works
- [ ] Modal opens and closes

**Questions to ask:**
- What's the difference between stack and tab navigation?
- How do you pass data between screens?
- What file does Expo Router use for layouts?

### Week 03: Lists & Forms
- [ ] FlatList renders 1000+ items smoothly
- [ ] Pull-to-refresh works
- [ ] Section headers stick while scrolling
- [ ] Form captures all inputs
- [ ] Keyboard doesn't cover inputs
- [ ] Validation shows errors
- [ ] Submit button enables/disables correctly

**Questions to ask:**
- Why use FlatList instead of ScrollView for long lists?
- What's `getItemLayout` for?
- How do you handle keyboard on mobile?

### Week 04: Styling & Month 1 Project
- [ ] Dark mode toggle works across all screens
- [ ] Custom fonts render correctly
- [ ] Icons display properly
- [ ] Note-taking app has all CRUD operations
- [ ] Notes persist in AsyncStorage
- [ ] Responsive layout works
- [ ] App runs on both iOS and Android

---

## Month 2 Verification

### Week 05: State Management
- [ ] Zustand store works (add, edit, delete)
- [ ] TanStack Query fetches API data
- [ ] Loading states display
- [ ] Error states handle gracefully
- [ ] No prop drilling (state accessed directly)
- [ ] Console.log shows no unnecessary re-renders

### Week 06: API & Error Handling
- [ ] Movies load from TMDB API
- [ ] Search returns results
- [ ] Offline banner shows when disconnected
- [ ] Cached data visible when offline
- [ ] Error states with retry buttons work
- [ ] Images load with placeholders

### Week 07: Native Features
- [ ] Camera opens, photo taken
- [ ] Gallery picker works
- [ ] Location coordinates displayed
- [ ] Push notification appears
- [ ] Tapping notification opens app
- [ ] Haptics felt on button press

### Week 08: Storage & Month 2 Project
- [ ] Data persists after app restart (AsyncStorage)
- [ ] Secure data stored in SecureStore
- [ ] MMKV reads are fast
- [ ] Movie tracker app has all features
- [ ] Favorites save locally
- [ ] Offline detection works

---

## Month 3 Verification

### Week 09: Animations
- [ ] Toggle animations smooth (LayoutAnimation)
- [ ] Drag gesture works (Reanimated)
- [ ] Swipe to delete works
- [ ] Staggered list items animate in
- [ ] No jank or dropped frames

### Week 10: Performance
- [ ] React DevTools shows fewer re-renders after memo
- [ ] FlatList scrolls at 60fps with 10k items
- [ ] Images load fast with caching
- [ ] App launches in <3 seconds
- [ ] Memory stays stable during use

### Week 11: Authentication
- [ ] OAuth flow works (Google Sign-In)
- [ ] Tokens stored in SecureStore
- [ ] Protected routes redirect to login
- [ ] Token refresh happens automatically
- [ ] Logout clears all data
- [ ] App remembers login across restarts

### Week 12: Testing & Month 3 Project
- [ ] Unit tests pass (`npx jest`)
- [ ] Coverage > 60%
- [ ] Component tests work (RNTL)
- [ ] E2E flows pass (Maestro)
- [ ] Chat UI clone has all features

---

## Month 4 Verification

### Week 13: Architecture
- [ ] Feature-based folder structure
- [ ] Custom hooks extracted
- [ ] Error boundaries in place
- [ ] TypeScript strict mode passes (`npx tsc --noEmit`)
- [ ] No lint errors (`npx eslint .`)

### Week 14: Platform-Specific
- [ ] Platform.select styles work
- [ ] Safe areas handle notch correctly
- [ ] Back button works on Android
- [ ] Keyboard behavior correct on both
- [ ] Typography looks native on each platform

### Week 15: Build & Deploy
- [ ] EAS Build configured
- [ ] iOS build completes
- [ ] Android build completes
- [ ] App submitted to stores
- [ ] OTA updates work

### Week 16: CI/CD & Launch
- [ ] GitHub Actions CI runs
- [ ] Sentry crash reporting works
- [ ] Analytics events tracked
- [ ] Beta testing feedback addressed
- [ ] App live on stores

---

## Final Assessment

| Criteria | Status |
|----------|--------|
| All 4 projects complete | |
| All weeks verified | |
| Can build app from scratch | |
| Can navigate independently | |
| Can debug with Flipper | |
| Can write tests | |
| Can deploy to stores | |
| Ready for production work | |

**Verdict:** PASS / PASS w/ notes / RETRY

**Notes:**

---

*Verified by: _____________ Date: _____________*
