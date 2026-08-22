# Week 08: Storage & Persistence
> **Month 2 | Week 8 of 16** | **Difficulty:** Intermediate

---

## Goal
Persist data locally — AsyncStorage, SecureStore, MMKV.

---

## Tasks

### Day 1: AsyncStorage
- [ ] Install: `npx expo install @react-native-async-storage/async-storage`
- [ ] Basic operations:
  ```typescript
  import AsyncStorage from '@react-native-async-storage/async-storage';
  
  // Save
  await AsyncStorage.setItem('user', JSON.stringify({ name: 'John' }));
  
  // Load
  const data = await AsyncStorage.getItem('user');
  const user = data ? JSON.parse(data) : null;
  
  // Delete
  await AsyncStorage.removeItem('user');
  
  // Clear all
  await AsyncStorage.clear();
  ```
- [ ] Build a storage helper:
  ```typescript
  // utils/storage.ts
  export const storage = {
    async get<T>(key: string): Promise<T | null> {
      const data = await AsyncStorage.getItem(key);
      return data ? JSON.parse(data) : null;
    },
    async set<T>(key: string, value: T): Promise<void> {
      await AsyncStorage.setItem(key, JSON.stringify(value));
    },
    async remove(key: string): Promise<void> {
      await AsyncStorage.removeItem(key);
    },
  };
  ```
- [ ] Verify: Data persists after app restart

### Day 2: SecureStore (Sensitive Data)
- [ ] Install: `npx expo install expo-secure-store`
- [ ] Use for tokens, passwords, API keys:
  ```typescript
  import * as SecureStore from 'expo-secure-store';
  
  // Save token
  await SecureStore.setItemAsync('auth-token', token);
  
  // Load token
  const token = await SecureStore.getItemAsync('auth-token');
  
  // Delete token
  await SecureStore.deleteItemAsync('auth-token');
  ```
- [ ] When to use SecureStore vs AsyncStorage:
  | AsyncStorage | SecureStore |
  |-------------|-------------|
  | User preferences | Auth tokens |
  | App settings | API keys |
  | Cached data | Passwords |
  | Notes, drafts | Payment info |
- [ ] Verify: Secure data stored, non-sensitive in AsyncStorage

### Day 3: MMKV (Fast Key-Value)
- [ ] Install: `npm install react-native-mmkv`
- [ ] Why MMKV: 30x faster than AsyncStorage
  ```typescript
  import { MMKV } from 'react-native-mmkv';
  
  const storage = new MMKV();
  
  // Set
  storage.set('user.name', 'John');
  storage.set('user.age', 25); // Numbers work directly
  
  // Get
  const name = storage.getString('user.name'); // string | undefined
  const age = storage.getNumber('user.age');     // number | undefined
  
  // Delete
  storage.delete('user.name');
  
  // Observe changes
  storage.addOnValueChangedListener((key) => {
    console.log(`Key "${key}" changed`);
  });
  ```
- [ ] Use MMKV for app state, Zustand persistence
- [ ] Verify: MMKV reads are noticeably fast

### Day 4: SQLite (Structured Data)
- [ ] Install: `npx expo install expo-sqlite`
- [ ] When to use SQLite:
  - Complex queries (joins, sorting, filtering)
  - Relational data (users, posts, comments)
  - Large datasets (10,000+ records)
- [ ] Basic usage:
  ```typescript
  import * as SQLite from 'expo-sqlite';
  
  const db = await SQLite.openDatabaseAsync('myapp.db');
  
  // Create table
  await db.execAsync(`
    CREATE TABLE IF NOT EXISTS notes (
      id TEXT PRIMARY KEY,
      title TEXT NOT NULL,
      content TEXT,
      created_at DATETIME DEFAULT CURRENT_TIMESTAMP
    );
  `);
  
  // Insert
  await db.runAsync('INSERT INTO notes (id, title, content) VALUES (?, ?, ?)',
    '1', 'My Note', 'Hello world');
  
  // Query
  const notes = await db.getAllAsync('SELECT * FROM notes WHERE title LIKE ?', '%My%');
  ```
- [ ] Verify: CRUD operations work with SQLite

### Day 5: Month 2 Project — Movie Tracker
- [ ] Combine all Month 2 skills:
  - API integration (TMDB)
  - Search movies
  - Save favorites (MMKV)
  - Store auth token (SecureStore)
  - User preferences (AsyncStorage)
  - Offline detection
  - Error handling
  - Loading states
- [ ] Verify: Complete app with persistent data

---

## Storage Decision Tree

```
Need structured queries? → SQLite
Need fast key-value?     → MMKV
Need simple persistence? → AsyncStorage
Need security?           → SecureStore
```

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. Save favorites → close app → reopen → still there
# 2. Auth token stored securely
# 3. Preferences persist
# 4. Clear app data → fresh state
# 5. SQLite queries return correct results
# 6. MMKV is fast (no lag on reads)
```
