# Week 01: React Native Fundamentals
> **Month 1 | Week 1 of 16** | **Difficulty:** Beginner

---

## Goal
Understand how React Native maps from React/Next.js and build your first mobile components.

---

## Tasks

### Day 1: Environment Setup
- [ ] Install Node.js 18+ and npm/yarn
- [ ] Install Expo CLI: `npm install -g expo-cli`
- [ ] Create first project: `npx create-expo-app MyFirstApp --template blank-typescript`
- [ ] Run on phone: Install Expo Go app, scan QR code
- [ ] Verify: App loads on your physical phone

### Day 2: Core Components
- [ ] Read: https://reactnative.dev/docs/components-and-apis
- [ ] Replace `App.tsx` content using these components:
  - `<View>` instead of `<div>`
  - `<Text>` instead of `<p>`, `<h1>`, `<span>`
  - `<Image>` instead of `<img>`
  - `<ScrollView>` instead of `<div>` with overflow
- [ ] Build: A profile card with name, photo, bio, and stats
- [ ] Verify: App renders all elements on phone

### Day 3: StyleSheet Deep Dive
- [ ] Read: https://reactnative.dev/docs/stylesheet
- [ ] Understand: No CSS cascade, no classes, no shorthand like `margin: 10px`
- [ ] Every style is a JS object with camelCase:
  ```typescript
  // WRONG (CSS)
  // background-color: red;
  
  // RIGHT (RN)
  backgroundColor: 'red',
  ```
- [ ] Build: Style the profile card with proper spacing, colors, typography
- [ ] Verify: Card looks good on both iOS and Android (use Expo Go on both)

### Day 4: Flexbox for Mobile
- [ ] Read: https://reactnative.dev/docs/flexbox
- [ ] Key difference: `flexDirection` defaults to `'column'` (not `'row'`)
- [ ] Practice these patterns:
  ```typescript
  // Center content
  { flex: 1, justifyContent: 'center', alignItems: 'center' }
  
  // Row layout
  { flexDirection: 'row', gap: 8 }
  
  // Space between
  { flexDirection: 'row', justifyContent: 'space-between' }
  ```
- [ ] Build: A settings screen with rows of label + value pairs
- [ ] Verify: Layout works on small (iPhone SE) and large (iPad) screens

### Day 5: Pressable and Event Handling
- [ ] Read: https://reactnative.dev/docs/pressable
- [ ] No `onClick` — use `onPress` instead
- [ ] Build: Add buttons to your profile card:
  - "Follow" button that toggles between Follow/Following
  - "Message" button that logs to console
- [ ] Verify: Buttons respond to tap, state updates correctly

---

## Concepts to Internalize

| React (Web) | React Native |
|-------------|--------------|
| `<div>` | `<View>` |
| `<p>`, `<span>`, `<h1>` | `<Text>` |
| `<img>` | `<Image>` |
| `className` | `style` |
| `onClick` | `onPress` |
| CSS files / Tailwind | `StyleSheet.create()` |
| `window` | Does not exist |

---

## Common Mistakes This Week

1. **Using `<Text>` without wrapping** — All visible text MUST be inside `<Text>`.裸 text will crash.
2. **Forgetting `flex: 1`** — Without it, views don't fill space.
3. **Using web CSS shortcuts** — `margin: '10px 20px'` doesn't work. Use `marginTop`, `marginRight` etc.
4. **Inline styles without `StyleSheet`** — Works but hurts performance.

---

## Verification

Run these checks before marking complete:

```bash
# App starts without errors
npx expo start --clear

# Check these on your phone:
# 1. Profile card renders with image, name, bio
# 2. Settings screen has proper rows
# 3. Buttons respond to press
# 4. Layout works on small AND large screens
# 5. No red error screens
```

**Done when:** You can explain the difference between `<View>` and `<div>`, and your profile app runs on a physical device.

---

## Resources
- Official: https://reactnative.dev/docs/getting-started
- Expo: https://docs.expo.dev/
- Snack (try code in browser): https://snack.expo.dev/
