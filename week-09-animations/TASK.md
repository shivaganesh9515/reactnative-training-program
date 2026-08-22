# Week 09: Animations
> **Month 3 | Week 9 of 16** | **Difficulty:** Advanced

---

## Goal
Make your app feel alive — LayoutAnimation, Reanimated, and Gesture Handler.

---

## Tasks

### Day 1: LayoutAnimation (Quick Wins)
- [ ] Read: https://reactnative.dev/docs/layoutanimation
- [ ] Simplest animation in React Native:
  ```typescript
  import { LayoutAnimation, UIManager } from 'react-native';
  
  // Enable on Android
  UIManager.setLayoutAnimationEnabledExperimental?.(true);
  
  function ToggleableSection() {
    const [expanded, setExpanded] = useState(false);
  
    const toggle = () => {
      LayoutAnimation.configureNext(LayoutAnimation.Presets.easeInEaseOut);
      setExpanded(!expanded);
    };
  
    return (
      <View>
        <Button title="Toggle" onPress={toggle} />
        {expanded && <View style={{ height: 200 }}><Text>Content</Text></View>}
      </View>
    );
  }
  ```
- [ ] Apply to: list item add/remove, expandable sections, tab switches
- [ ] Verify: Animations are smooth, not jarring

### Day 2: Reanimated Basics
- [ ] Install: `npx expo install react-native-reanimated`
- [ ] Add babel plugin: `babel.config.js` → `plugins: ['react-native-reanimated/plugin']`
- [ ] Understand shared values:
  ```typescript
  import Animated, { useSharedValue, useAnimatedStyle, withTiming } from 'react-native-reanimated';
  
  function FadeBox() {
    const opacity = useSharedValue(1);
  
    const animatedStyle = useAnimatedStyle(() => ({
      opacity: opacity.value,
    }));
  
    const fadeOut = () => {
      opacity.value = withTiming(0, { duration: 500 });
    };
  
    return (
      <View>
        <Animated.View style={[styles.box, animatedStyle]} />
        <Button title="Fade Out" onPress={fadeOut} />
      </View>
    );
  }
  ```
- [ ] Build: Fade in/out, slide up/down, scale animations
- [ ] Verify: 60fps animations, no jank

### Day 3: Gesture Handler
- [ ] Install: `npx expo install react-native-gesture-handler`
- [ ] Basic gestures:
  ```typescript
  import { GestureDetector, Gesture } from 'react-native-gesture-handler';
  import Animated, { useSharedValue, useAnimatedStyle, withSpring } from 'react-native-reanimated';
  
  function DraggableBox() {
    const translateX = useSharedValue(0);
    const translateY = useSharedValue(0);
  
    const pan = Gesture.Pan()
      .onUpdate((e) => {
        translateX.value = e.translationX;
        translateY.value = e.translationY;
      })
      .onEnd(() => {
        translateX.value = withSpring(0);
        translateY.value = withSpring(0);
      });
  
    return (
      <GestureDetector gesture={pan}>
        <Animated.View style={{ transform: [{ translateX }, { translateY }] }} />
      </GestureDetector>
    );
  }
  ```
- [ ] Build: Swipeable list item (swipe to delete)
- [ ] Verify: Gestures feel native, spring physics work

### Day 4: Common Animation Patterns
- [ ] Build these reusable patterns:
  1. **FadeIn** — Component appears with fade
  2. **SlideIn** — Component slides from bottom
  3. **ScaleOnPress** — Button shrinks on press, springs back
  4. **Staggered list** — Items appear one after another
  ```typescript
  // Staggered list pattern
  {data.map((item, index) => (
    <Animated.View
      key={item.id}
      entering={FadeInDown.delay(index * 100).springify()}
    >
      <ListItem item={item} />
    </Animated.View>
  ))}
  ```
- [ ] Verify: All patterns work smoothly

### Day 5: Performance Check
- [ ] Run animation profiler in Flipper
- [ ] Check: Are animations running at 60fps?
- [ ] Common issues:
  - JS thread animations → Use native driver (Reanimated handles this)
  - Layout thrashing → Batch layout changes
  - Too many animated components → Limit concurrent animations
- [ ] Verify: No dropped frames in profiler

---

## Animation Decision Tree

```
Simple show/hide?    → LayoutAnimation
Smooth transitions?  → Reanimated withTiming
Gesture-based?       → Reanimated + GestureHandler
Complex sequences?   → Reanimated withDelay/withSequence
Icon animation?      → Lottie
```

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. Toggle animations are smooth
# 2. Drag gesture works
# 3. Swipe to delete works
# 4. Staggered list items animate in
# 5. No jank or dropped frames
# 6. Animations work on low-end device (if available)
```
