# Week 03: Lists, Forms & User Input
> **Month 1 | Week 3 of 16** | **Difficulty:** Beginner-Intermediate

---

## Goal
Handle real user interaction — lists of data, form inputs, and keyboard management.

---

## Tasks

### Day 1: FlatList Mastery
- [ ] Read: https://reactnative.dev/docs/flatlist
- [ ] Understand: `ScrollView` loads ALL items (bad for 100+ items). `FlatList` virtualizes (renders only visible).
- [ ] Build a contacts list:
  ```typescript
  <FlatList
    data={contacts}
    keyExtractor={(item) => item.id}
    renderItem={({ item }) => (
      <View style={styles.row}>
        <Text>{item.name}</Text>
        <Text>{item.phone}</Text>
      </View>
    )}
    ItemSeparatorComponent={() => <View style={styles.separator} />}
  />
  ```
- [ ] Add pull-to-refresh: `refreshing` + `onRefresh` props
- [ ] Verify: List scrolls smoothly, 1000+ items don't lag

### Day 2: SectionList
- [ ] Build a grouped list (like phone contacts grouped by letter):
  ```typescript
  <SectionList
    sections={[
      { title: 'A', data: ['Alice', 'Anna'] },
      { title: 'B', data: ['Bob', 'Ben'] },
    ]}
    renderItem={({ item }) => <Text>{item}</Text>}
    renderSectionHeader={({ section }) => <Text>{section.title}</Text>}
    stickySectionHeadersEnabled={true}
  />
  ```
- [ ] Verify: Section headers stick while scrolling

### Day 3: TextInput & Forms
- [ ] Read: https://reactnative.dev/docs/textinput
- [ ] No `<input>` — use `<TextInput>`
- [ ] Key differences:
  ```typescript
  <TextInput
    value={text}
    onChangeText={setText}       // Not onChange
    placeholder="Enter name"
    keyboardType="email-address" // Keyboard type changes
    secureTextEntry={true}       // Password field
    autoCapitalize="none"        // No auto-capitalize
  />
  ```
- [ ] Build a registration form: name, email, password, submit button
- [ ] Verify: Form captures all inputs, keyboard appears correctly

### Day 4: Keyboard Handling
- [ ] Read: https://reactnative.dev/docs/keyboardavoidingview
- [ ] Problem: Keyboard covers inputs on mobile
- [ ] Solution: `KeyboardAvoidingView`
  ```typescript
  <KeyboardAvoidingView
    behavior={Platform.OS === 'ios' ? 'padding' : 'height'}
    style={styles.container}
  >
    <TextInput ... />
  </KeyboardAvoidingView>
  ```
- [ ] Alternative: `KeyboardAwareScrollView` from expo packages
- [ ] Verify: Input fields move up when keyboard appears, no occlusion

### Day 5: Form Validation & State
- [ ] Build form validation:
  - Email must contain @
  - Password min 8 chars
  - Show error messages below fields
- [ ] Disable submit button until form is valid
- [ ] On submit: Log form data to console, clear form
- [ ] Verify: Validation works, errors display, form resets

---

## Key Concepts

### Mobile Input Differences

| Web | React Native |
|-----|--------------|
| `<input>` | `<TextInput>` |
| `<select>` | Picker library or custom modal |
| `<textarea>` | `<TextInput multiline />` |
| `onChange` | `onChangeText` (returns string directly) |
| `type="email"` | `keyboardType="email-address"` |
| `type="password"` | `secureTextEntry={true}` |
| `placeholder` | `placeholder` (same) |

### FlatList vs ScrollView

| Feature | ScrollView | FlatList |
|---------|-----------|----------|
| Renders | ALL children | Only visible items |
| Memory | High for long lists | Low, efficient |
| Use when | Small, fixed content | Dynamic, long lists |
| Performance | Poor at 100+ items | Handles 10,000+ items |

---

## Common Mistakes This Week

1. **Using ScrollView for long lists** — Performance disaster. Always use FlatList for dynamic data.
2. **Missing `keyExtractor`** — React warns, performance drops. Always provide stable keys.
3. **No keyboard avoidance** — Users can't see what they're typing. Always use KeyboardAvoidingView.
4. **Using `onChange` instead of `onChangeText`** — `onChange` returns event, `onChangeText` returns string directly. Use onChangeText.

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. Contact list scrolls smoothly with 1000+ items
# 2. Pull-to-refresh works
# 3. Section headers stick
# 4. Registration form captures all inputs
# 5. Keyboard doesn't cover inputs
# 6. Validation shows errors
# 7. Submit button enables/disables correctly
```

**Done when:** You can build a form that handles keyboard, validates input, and a list that scrolls 1000+ items without lag.
