# Week 14: Platform-Specific Code
> **Month 4 | Week 14 of 16** | **Difficulty:** Advanced

---

## Goal
Handle iOS vs Android differences — platform-specific components and behaviors.

---

## Tasks

### Day 1: Platform API
- [ ] Read: https://reactnative.dev/docs/platform
- [ ] Conditional code:
  ```typescript
  import { Platform, StyleSheet } from 'react-native';
  
  const styles = StyleSheet.create({
    shadow: Platform.select({
      ios: {
        shadowColor: '#000',
        shadowOffset: { width: 0, height: 2 },
        shadowOpacity: 0.1,
        shadowRadius: 4,
      },
      android: {
        elevation: 4,
      },
    }),
  });
  
  // Or inline
  if (Platform.OS === 'ios') {
    // iOS-specific
  } else {
    // Android-specific
  }
  ```
- [ ] Apply platform-specific styles to your app
- [ ] Verify: Styling works correctly on both platforms

### Day 2: Platform-Specific Files
- [ ] Create platform-specific components:
  ```
  components/
    Header.tsx            # Default (shared logic)
    Header.ios.tsx        # iOS-specific
    Header.android.tsx    # Android-specific
  ```
- [ ] Metro bundler automatically picks the right file
- [ ] Use when behavior differs significantly:
  - Back button handling (iOS swipe vs Android hardware)
  - Permission dialogs
  - Typography (SF Pro vs Roboto)
  - Navigation patterns
- [ ] Verify: Correct file loads on each platform

### Day 3: Safe Areas
- [ ] Install: `npx expo install react-native-safe-area-context`
- [ ] Wrap app:
  ```typescript
  import { SafeAreaProvider } from 'react-native-safe-area-context';
  
  // In _layout.tsx
  <SafeAreaProvider>
    <Stack />
  </SafeAreaProvider>
  ```
- [ ] Use SafeAreaView:
  ```typescript
  import { SafeAreaView } from 'react-native-safe-area-context';
  
  <SafeAreaView style={{ flex: 1 }}>
    <Header />
    <Content />
  </SafeAreaView>
  ```
- [ ] Handle notch, home indicator, status bar
- [ ] Verify: Content doesn't overlap system UI

### Day 4: Platform-Specific Behaviors
- [ ] Handle different behaviors:
  ```typescript
  // Back handler (Android hardware back button)
  import { BackHandler } from 'react-native';
  
  useEffect(() => {
    const backHandler = BackHandler.addEventListener('hardwareBackPress', () => {
      router.back();
      return true;
    });
    return () => backHandler.remove();
  }, []);
  
  // Keyboard behavior
  <KeyboardAvoidingView
    behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
  >
  ```
- [ ] Test back button on Android
- [ ] Verify: Behaviors are platform-appropriate

### Day 5: iOS & Android Guidelines
- [ ] Review platform differences:
  | Element | iOS | Android |
  |---------|-----|---------|
  | Back | Swipe from edge | Hardware back button |
  | Fonts | SF Pro | Roboto |
  | Alerts | Native iOS | Material dialogs |
  | Tabs | Bottom bar | Bottom bar (MD3) |
  | Headers | Large title | Material app bar |
  | Touch target | 44pt | 48dp |
- [ ] Adjust your app for both platforms
- [ ] Verify: App feels native on both platforms

---

## Verification

```bash
# Test on iOS
npx expo start --ios

# Test on Android
npx expo start --android

# Check:
# 1. Shadows render on iOS, elevation on Android
# 2. Safe area handles notch correctly
# 3. Back button works on Android
# 4. Keyboard behavior correct on both
# 5. Typography looks native on each platform
# 6. Touch targets are large enough
```
