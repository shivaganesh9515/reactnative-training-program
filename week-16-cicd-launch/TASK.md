# Week 16: CI/CD, Monitoring & Launch
> **Month 4 | Week 16 of 16** | **Difficulty:** Advanced

---

## Goal
Automate your pipeline, monitor production, and launch with confidence.

---

## Tasks

### Day 1: GitHub Actions CI
- [ ] Create CI workflow:
  ```yaml
  # .github/workflows/ci.yml
  name: CI
  on: [push, pull_request]
  
  jobs:
    test:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: 18
            cache: 'npm'
        - run: npm ci
        - run: npx tsc --noEmit
        - run: npx eslint .
        - run: npx jest --coverage
  
    build:
      needs: test
      if: github.ref == 'refs/heads/main'
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - uses: actions/setup-node@v4
          with:
            node-version: 18
        - run: npm ci
        - run: npx eas build --platform ios --non-interactive
        - run: npx eas build --platform android --non-interactive
  ```
- [ ] Verify: CI runs on every push

### Day 2: Crash Reporting (Sentry)
- [ ] Install: `npx expo install sentry-expo`
- [ ] Set up:
  ```typescript
  // app/_layout.tsx
  import * as Sentry from 'sentry-expo';
  
  Sentry.init({
    dsn: 'your-dsn',
    enableInExpoDevelopment: false,
    tracesSampleRate: 1.0,
  });
  ```
- [ ] Add error boundaries with Sentry
- [ ] Verify: Crashes appear in Sentry dashboard

### Day 3: Analytics
- [ ] Install analytics (choose one):
  - `expo-analytics` (simple)
  - Mixpanel (product analytics)
  - Amplitude (user behavior)
- [ ] Track key events:
  ```typescript
  import { track } from '@mixpanel/mixpanel-react';
  
  // Screen views
  track('Screen Viewed', { screen: 'Home' });
  
  // User actions
  track('Note Created', { length: content.length });
  
  // Errors
  track('API Error', { endpoint: '/movies', status: 500 });
  ```
- [ ] Verify: Events appear in analytics dashboard

### Day 4: Beta Testing
- [ ] iOS TestFlight:
  ```bash
  eas submit --platform ios --profile preview
  ```
  - Add internal testers (up to 100)
  - Add external testers (up to 10,000, requires Apple review)
- [ ] Android Internal Testing:
  ```bash
  eas submit --platform android --profile preview
  ```
  - Add testers by email
- [ ] Create feedback form (Google Form or similar)
- [ ] Verify: Testers can install and provide feedback

### Day 5: Launch Checklist & Handoff
- [ ] Complete launch checklist:
  ```
  PRE-LAUNCH
  - [ ] All tests pass
  - [ ] No console.log in production
  - [ ] Error tracking configured
  - [ ] Analytics tracking key events
  - [ ] App icon and splash screen
  - [ ] Screenshots for both stores
  - [ ] Privacy policy live
  - [ ] Support page/FAQ ready
  
  LAUNCH
  - [ ] Production builds submitted
  - [ ] Store listings complete
  - [ ] Beta testing feedback addressed
  - [ ] Team notified
  - [ ] Social media posts scheduled
  
  POST-LAUNCH
  - [ ] Monitor crash reports
  - [ ] Monitor analytics
  - [ ] Respond to reviews
  - [ ] Plan v1.1 based on feedback
  ```
- [ ] Hand off to team:
  - Document architecture decisions
  - Write onboarding guide
  - Set up monitoring dashboards
  - Schedule weekly review meetings
- [ ] Verify: App is live and monitored

---

## Post-Launch Monitoring

| Metric | Tool | Alert Threshold |
|--------|------|-----------------|
| Crashes | Sentry | >1% crash rate |
| API errors | Sentry/Logs | >5% error rate |
| Load time | Analytics | >3 seconds |
| User retention | Analytics | <20% day-7 |
| App reviews | Store Console | <4 stars average |

---

## Program Complete

### What You've Built
1. Note-taking app (Month 1)
2. Movie tracker app (Month 2)
3. Chat UI clone (Month 3)
4. Production app on stores (Month 4)

### Skills Acquired
- [ ] React Native fundamentals
- [ ] Navigation (Stack, Tabs, Deep links)
- [ ] State management (Zustand, TanStack Query)
- [ ] API integration & error handling
- [ ] Native features (camera, location, notifications)
- [ ] Local storage & security
- [ ] Animations (Reanimated, Gesture Handler)
- [ ] Performance optimization
- [ ] Authentication (OAuth, JWT)
- [ ] Testing (Unit, Integration, E2E)
- [ ] Architecture & code organization
- [ ] Platform-specific code
- [ ] App store deployment
- [ ] CI/CD & monitoring

### Next Steps
- [ ] Start contributing to production codebase
- [ ] Mentor the next batch of juniors
- [ ] Deep dive into your specialty area
- [ ] Stay current with React Native updates
