# Week 10: Performance Optimization
> **Month 3 | Week 10 of 16** | **Difficulty:** Advanced

---

## Goal
Make your app fast — memoization, FlatList optimization, profiling.

---

## Tasks

### Day 1: React.memo & useCallback
- [ ] Read: https://reactnative.dev/docs/optimizing-flatlist-performance
- [ ] Understand when re-renders happen:
  ```
  Parent re-renders → ALL children re-render (even if props didn't change)
  ```
- [ ] Fix with memo:
  ```typescript
  // WITHOUT memo: re-renders every time parent updates
  function ListItem({ item }) {
    return <View><Text>{item.title}</Text></View>;
  }
  
  // WITH memo: only re-renders when props change
  const ListItem = React.memo(({ item }) => {
    return <View><Text>{item.title}</Text></View>;
  });
  ```
- [ ] Fix inline functions:
  ```typescript
  // BAD: new function reference every render
  <ListItem onPress={() => handlePress(item.id)} />
  
  // GOOD: stable function reference
  const handlePress = useCallback((id: string) => {
    router.push(`/item/${id}`);
  }, []);
  
  <ListItem item={item} onPress={handlePress} />
  ```
- [ ] Verify: Use React DevTools profiler to confirm fewer re-renders

### Day 2: FlatList Optimization
- [ ] Apply ALL optimizations:
  ```typescript
  <FlatList
    data={items}
    renderItem={renderItem}
    keyExtractor={(item) => item.id}
    getItemLayout={(_, index) => ({
      length: ITEM_HEIGHT,
      offset: ITEM_HEIGHT * index,
      index,
    })}
    windowSize={5}              // Render 5 screens worth
    maxToRenderPerBatch={10}    // Render 10 items per batch
    updateCellsBatchingPeriod={50} // Batch every 50ms
    removeClippedSubviews={true}   // Remove off-screen views
    initialNumToRender={10}     // Initial render count
  />
  ```
- [ ] Understand `getItemLayout`: Tells FlatList exact heights, avoids measurement
- [ ] Verify: List scrolls at 60fps with 10,000+ items

### Day 3: Image Optimization
- [ ] Use Expo Image (better than React Native Image):
  ```typescript
  import { Image } from 'expo-image';
  
  <Image
    source={{ uri: url }}
    style={{ width: 100, height: 100 }}
    contentFit="cover"
    transition={300}
    cachePolicy="memory-disk"  // Cache to disk
  />
  ```
- [ ] Implement lazy loading for list images
- [ ] Use proper image sizes (don't load 4K for 100px thumbnail)
- [ ] Verify: Images load fast, no memory spikes

### Day 4: Flipper Profiling
- [ ] Install Flipper desktop app
- [ ] Connect to your app
- [ ] Use these tools:
  - **React DevTools** — Component re-renders
  - **Performance** — FPS, memory, CPU
  - **Network** — API request timing
  - **Layout** — View hierarchy
- [ ] Find and fix 3 performance issues in your app
- [ ] Verify: Improved metrics after fixes

### Day 5: Performance Audit
- [ ] Run through this checklist:
  - [ ] No ScrollView for long lists
  - [ ] FlatList has `getItemLayout` and `keyExtractor`
  - [ ] List items are memoized
  - [ ] No inline functions in renderItem
  - [ ] Images use Expo Image with caching
  - [ ] No unnecessary re-renders (profiler check)
  - [ ] Bundle size under 5MB
  - [ ] App launches in under 3 seconds
- [ ] Document findings and fixes
- [ ] Verify: All checks pass

---

## Performance Checklist

| Area | Check | Tool |
|------|-------|------|
| Re-renders | Components don't re-render unnecessarily | React DevTools |
| Lists | FlatList handles 10k+ items smoothly | Manual test |
| Memory | No memory leaks or spikes | Flipper Memory |
| Images | Cached, proper sizes, lazy loaded | Network tab |
| Bundle | Under 5MB, code-split where possible | `npx expo export` |
| Launch | Under 3 seconds to interactive | Manual timing |

---

## Common Performance Killers

| Problem | Fix |
|---------|-----|
| ScrollView with 100+ items | Use FlatList |
| Inline renderItem | useCallback + memo |
| Large images | Resize, cache, lazy load |
| Console.log in prod | Strip logs |
| Too many re-renders | Memo, useCallback, useMemo |
| Unnecessary state updates | Split state, use refs |

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. React DevTools shows fewer re-renders after memo
# 2. FlatList scrolls at 60fps with 10k items
# 3. Images load fast with placeholders
# 4. App launches in <3 seconds
# 5. Memory stays stable during use
# 6. No jank during animations
```
