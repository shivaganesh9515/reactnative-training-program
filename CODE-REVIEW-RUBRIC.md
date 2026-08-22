# Code Review Rubric (Reviewer Role)
> Use this when reviewing junior developers' code during training.

---

## Review Principles

1. **Teach, don't criticize** — Every comment should help them learn
2. **Prioritize** — Blocking issues first, improvements second
3. **Be specific** — "This is bad" is useless. "This causes re-renders because..." is useful
4. **Praise good work** — Call out what they did well

---

## Severity Levels

| Level | Meaning | Action |
|-------|---------|--------|
| 🔴 BLOCKING | Breaks functionality, security risk, data loss | Must fix before merge |
| 🟡 IMPORTANT | Performance issue, bad pattern, missing error handling | Should fix this week |
| 🟢 MINOR | Style, naming, minor optimization | Nice to have, no merge block |

---

## Review Checklist

### 1. Functionality (Must Pass)
- [ ] Does it work as expected?
- [ ] Are all edge cases handled?
- [ ] Does it handle errors gracefully?
- [ ] Does it work on both iOS and Android?

### 2. React Native Patterns
- [ ] Uses `<Text>` for all visible text (not bare strings)
- [ ] Uses `<FlatList>` for long lists (not `<ScrollView>`)
- [ ] Styles use `StyleSheet.create()` (not inline objects)
- [ ] No web patterns (`className`, `onClick`, CSS shorthand)
- [ ] Platform-specific code uses `Platform.select()` or file extensions

### 3. Performance
- [ ] List items use `keyExtractor` with stable IDs
- [ ] FlatList has `getItemLayout` for fixed-height items
- [ ] No inline functions in `renderItem` (use `useCallback`)
- [ ] Heavy components wrapped in `React.memo`
- [ ] Images use Expo Image with caching

### 4. State Management
- [ ] Local state uses `useState`/`useReducer`
- [ ] Global state uses Zustand or Context (not prop drilling)
- [ ] Server state uses TanStack Query (not local useState)
- [ ] No unnecessary re-renders

### 5. Code Quality
- [ ] TypeScript types are correct (no `any`)
- [ ] Functions are small and focused
- [ ] No duplicated code
- [ ] Meaningful variable/function names
- 6. Security
- [ ] Tokens in SecureStore (not AsyncStorage)
- [ ] No hardcoded secrets
- [ ] Input validated before use
- [ ] API errors handled

### 7. Testing
- [ ] Critical paths have tests
- [ ] Tests are readable and maintainable
- [ ] No flaky tests (proper mocking)

---

## Common Issues & Fixes

### 🔴 Blocking Issues

**Bare text in View**
```typescript
// BAD - crashes
<View>
  Hello World
</View>

// GOOD
<View>
  <Text>Hello World</Text>
</View>
```

**ScrollView for long lists**
```typescript
// BAD - performance disaster
<ScrollView>
  {items.map(item => <Item key={item.id} item={item} />)}
</ScrollView>

// GOOD
<FlatList
  data={items}
  keyExtractor={(item) => item.id}
  renderItem={({ item }) => <Item item={item} />}
/>
```

**Tokens in AsyncStorage**
```typescript
// BAD - insecure
await AsyncStorage.setItem('token', jwt);

// GOOD - encrypted
await SecureStore.setItemAsync('token', jwt);
```

### 🟡 Important Issues

**Missing error handling**
```typescript
// BAD
const data = await fetch(url).then(r => r.json());

// GOOD
try {
  const response = await fetch(url);
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  const data = await response.json();
} catch (error) {
  // Handle error
}
```

**Inline functions in lists**
```typescript
// BAD - re-renders all items
<FlatList
  renderItem={({ item }) => (
    <TouchableOpacity onPress={() => handlePress(item.id)}>
      <Text>{item.title}</Text>
    </TouchableOpacity>
  )}
/>

// GOOD - stable reference
const handlePress = useCallback((id: string) => {
  router.push(`/item/${id}`);
}, []);

const renderItem = useCallback(({ item }) => (
  <TouchableOpacity onPress={() => handlePress(item.id)}>
    <Text>{item.title}</Text>
  </TouchableOpacity>
), [handlePress]);
```

### 🟢 Minor Issues

**Inconsistent naming**
```typescript
// Inconsistent
function GetUser() { ... }
const user_data = { ... };

// Consistent
function getUser() { ... }
const userData = { ... };
```

---

## Review Comment Templates

### Positive Feedback
```
Great job on [specific thing]! The [pattern/approach] is well implemented.
```

### Blocking Issue
```
🔴 This needs to be fixed before merge.

[What's wrong]
[Why it's a problem]
[How to fix it]

Let me know if you need help!
```

### Improvement Suggestion
```
💡 Nice implementation! One suggestion:

[Current approach] → [Better approach]

This would improve [performance/maintainability/readability].
```

### Question
```
❓ Quick question about [specific line/decision]:

[What you're wondering]

Just want to understand the reasoning.
```

---

## After Review

1. Respond to all comments within 24 hours
2. Re-review fixes promptly
3. Approve when ready (don't nitpick)
4. Document patterns for team reference
5. Give shout-outs for good work

---

*Remember: The goal is to help them grow, not to show you know more.*
