# Week 11: Authentication
> **Month 3 | Week 11 of 16** | **Difficulty:** Advanced

---

## Goal
Implement secure authentication — OAuth, JWT, session management.

---

## Tasks

### Day 1: OAuth with Expo AuthSession
- [ ] Install: `npx expo install expo-auth-session expo-crypto`
- [ ] Set up Google Sign-In:
  ```typescript
  import * as AuthSession from 'expo-auth-session';
  import * as WebBrowser from 'expo-webbrowser';
  
  WebBrowser.maybeCompleteAuthSession();
  
  const discovery = {
    authorizationEndpoint: 'https://accounts.google.com/o/oauth2/v2/auth',
    tokenEndpoint: 'https://oauth2.googleapis.com/token',
  };
  
  const [request, response, promptAsync] = AuthSession.useAuthRequest(
    {
      clientId: 'your-client-id',
      scopes: ['profile', 'email'],
      redirectUri: AuthSession.makeRedirectUri({ scheme: 'myapp' }),
    },
    discovery
  );
  
  useEffect(() => {
    if (response?.type === 'success') {
      const { access_token } = response.params;
      // Send to your backend
    }
  }, [response]);
  ```
- [ ] Verify: OAuth flow opens browser, returns token

### Day 2: JWT Token Management
- [ ] Build token storage:
  ```typescript
  // services/auth.ts
  import * as SecureStore from 'expo-secure-store';
  
  const TOKEN_KEY = 'auth-token';
  const REFRESH_KEY = 'refresh-token';
  
  export const auth = {
    async setTokens(accessToken: string, refreshToken: string) {
      await SecureStore.setItemAsync(TOKEN_KEY, accessToken);
      await SecureStore.setItemAsync(REFRESH_KEY, refreshToken);
    },
  
    async getAccessToken() {
      return SecureStore.getItemAsync(TOKEN_KEY);
    },
  
    async getRefreshToken() {
      return SecureStore.getItemAsync(REFRESH_KEY);
    },
  
    async clearTokens() {
      await SecureStore.deleteItemAsync(TOKEN_KEY);
      await SecureStore.deleteItemAsync(REFRESH_KEY);
    },
  };
  ```
- [ ] Implement token refresh logic
- [ ] Verify: Tokens persist, refresh works

### Day 3: Auth Context & Protected Routes
- [ ] Build auth context:
  ```typescript
  interface AuthState {
    user: User | null;
    isAuthenticated: boolean;
    isLoading: boolean;
  }
  
  function useAuth(): AuthState {
    const [state, setState] = useState<AuthState>({
      user: null,
      isAuthenticated: false,
      isLoading: true,
    });
  
    useEffect(() => {
      checkAuth();
    }, []);
  
    const checkAuth = async () => {
      const token = await auth.getAccessToken();
      if (token) {
        const user = await fetchUser(token);
        setState({ user, isAuthenticated: true, isLoading: false });
      } else {
        setState({ user: null, isAuthenticated: false, isLoading: false });
      }
    };
  
    return state;
  }
  ```
- [ ] Protect routes:
  ```typescript
  function ProtectedRoute({ children }) {
    const { isAuthenticated, isLoading } = useAuth();
  
    if (isLoading) return <ActivityIndicator />;
    if (!isAuthenticated) return <Redirect href="/login" />;
  
    return children;
  }
  ```
- [ ] Verify: Unauthenticated users redirected to login

### Day 4: Secure Token Refresh
- [ ] Implement Axios interceptor (or fetch wrapper):
  ```typescript
  async function authFetch(url: string, options: RequestInit = {}) {
    let token = await auth.getAccessToken();
    
    let response = await fetch(url, {
      ...options,
      headers: { ...options.headers, Authorization: `Bearer ${token}` },
    });
  
    // Token expired → refresh
    if (response.status === 401) {
      const refreshToken = await auth.getRefreshToken();
      const newTokens = await refreshTokens(refreshToken);
      await auth.setTokens(newTokens.accessToken, newTokens.refreshToken);
      
      // Retry request
      token = newTokens.accessToken;
      response = await fetch(url, {
        ...options,
        headers: { ...options.headers, Authorization: `Bearer ${token}` },
      });
    }
  
    return response;
  }
  ```
- [ ] Verify: Expired tokens trigger refresh, no user-facing errors

### Day 5: Logout & Session Handling
- [ ] Implement logout:
  ```typescript
  async function logout() {
    await auth.clearTokens();
    // Clear any cached data
    queryClient.clear();
    // Redirect to login
    router.replace('/login');
  }
  ```
- [ ] Handle app background/foreground (session timeout)
- [ ] Build login screen with Google + email/password options
- [ ] Verify: Full auth flow works end-to-end

---

## Auth Flow Diagram

```
App Launch
    ↓
Check SecureStore for token
    ↓
Token exists? → Yes → Validate with API
                    ↓
              Valid? → Yes → Home screen
                    ↓
              No → Refresh token
                    ↓
              Success? → Yes → Home screen
                    ↓
              No → Login screen
                    
Token exists? → No → Login screen
```

---

## Security Rules

| Rule | Why |
|------|-----|
| Never store tokens in AsyncStorage | Easily stolen |
| Always use SecureStore for tokens | Encrypted |
| Implement token refresh | Short-lived access tokens |
| Clear all data on logout | No stale state |
| Validate tokens on app launch | Handle expired sessions |

---

## Verification

```bash
npx expo start --clear

# Test:
# 1. Google Sign-In flow works
# 2. Tokens stored in SecureStore
# 3. Protected routes redirect to login
# 4. Token refresh happens automatically
# 5. Logout clears all data
# 6. App remembers login across restarts
# 7. Expired token triggers refresh (test with mock)
```
