# Week 06: API Integration & Error Handling
> **Month 2 | Week 6 of 16** | **Difficulty:** Intermediate

---

## Goal
Connect to real APIs, handle errors gracefully, and work offline.

---

## Tasks

### Day 1: REST API Integration
- [ ] Use a free API: https://developer.themoviedb.org/docs/getting-started (TMDB)
- [ ] Build API service layer:
  ```typescript
  // services/api.ts
  const BASE_URL = 'https://api.themoviedb.org/3';
  const API_KEY = 'your-key';
  
  export async function fetchMovies(page = 1) {
    const response = await fetch(`${BASE_URL}/movie/popular?page=${page}&api_key=${API_KEY}`);
    if (!response.ok) throw new Error('Failed to fetch movies');
    return response.json();
  }
  
  export async function searchMovies(query: string) {
    const response = await fetch(`${BASE_URL}/search/movie?query=${query}&api_key=${API_KEY}`);
    if (!response.ok) throw new Error('Failed to search');
    return response.json();
  }
  ```
- [ ] Connect to TanStack Query from Week 05
- [ ] Verify: Movies load from API

### Day 2: Error Handling Patterns
- [ ] Implement error boundaries:
  ```typescript
  // components/ErrorBoundary.tsx
  class ErrorBoundary extends React.Component<Props, State> {
    state = { hasError: false, error: null };
  
    static getDerivedStateFromError(error) {
      return { hasError: true, error };
    }
  
    render() {
      if (this.state.hasError) {
        return <ErrorScreen error={this.state.error} onRetry={() => this.setState({ hasError: false })} />;
      }
      return this.props.children;
    }
  }
  ```
- [ ] Build error states for every screen:
  - Loading spinner
  - Error message with retry button
  - Empty state (no results)
  - Offline state
- [ ] Verify: Every screen handles all 4 states

### Day 3: Offline Detection
- [ ] Install: `npx expo install @react-native-community/netinfo`
- [ ] Create network hook:
  ```typescript
  import NetInfo from '@react-native-community/netinfo';
  
  function useNetworkStatus() {
    const [isConnected, setIsConnected] = useState(true);
  
    useEffect(() => {
      const unsubscribe = NetInfo.addEventListener(state => {
        setIsConnected(state.isConnected ?? true);
      });
      return () => unsubscribe();
    }, []);
  
    return isConnected;
  }
  ```
- [ ] Show offline banner when disconnected
- [ ] Cache last successful API response
- [ ] Verify: App shows offline state, cached data still visible

### Day 4: Image Loading & Caching
- [ ] Use Expo Image for optimized loading:
  ```typescript
  import { Image } from 'expo-image';
  
  <Image
    source={{ uri: 'https://example.com/photo.jpg' }}
    style={{ width: 200, height: 300 }}
    contentFit="cover"
    transition={300}
    placeholder={blurhash}
  />
  ```
- [ ] Add loading placeholders and error states for images
- [ ] Implement lazy loading for lists with images
- [ ] Verify: Images load smoothly, placeholders show, errors handled

### Day 5: Build Movie Search App
- [ ] Combine everything:
  - Search movies (API)
  - Display results (FlatList)
  - Movie detail screen
  - Favorite movies (local storage)
  - Loading/error/empty states
  - Offline detection
  - Pull-to-refresh
- [ ] Verify: Complete app with all states handled

---

## Error Handling Checklist

| Scenario | Solution |
|----------|----------|
| Network error | Show retry button |
| API rate limit | Show "try again later" |
| Invalid response | Validate data shape |
| Image load fail | Show placeholder |
| No internet | Show cached data + offline banner |
| Crash | Error boundary catches it |

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. Movies load from API
# 2. Search returns results
# 3. Turn off phone WiFi → offline banner shows
# 4. Turn WiFi back → data refreshes
# 5. Invalid API key → error state, not crash
# 6. Images load with placeholders
# 7. Pull-to-refresh works
# 8. Empty search shows "no results" state
```
