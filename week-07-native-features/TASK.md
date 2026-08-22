# Week 07: Native Features
> **Month 2 | Week 7 of 16** | **Difficulty:** Intermediate-Advanced

---

## Goal
Access device hardware — camera, location, notifications.

---

## Tasks

### Day 1: Camera & Image Picker
- [ ] Install: `npx expo install expo-image-picker`
- [ ] Request permissions:
  ```typescript
  import * as ImagePicker from 'expo-image-picker';
  
  const result = await ImagePicker.requestCameraPermissionsAsync();
  if (!result.granted) {
    alert('Camera permission is required');
    return;
  }
  ```
- [ ] Pick image from gallery:
  ```typescript
  const result = await ImagePicker.launchImageLibraryAsync({
    mediaTypes: ['images'],
    allowsEditing: true,
    aspect: [1, 1],
    quality: 0.8,
  });
  
  if (!result.canceled) {
    setImage(result.assets[0].uri);
  }
  ```
- [ ] Take photo with camera
- [ ] Verify: Can pick/take photos and display them

### Day 2: Geolocation
- [ ] Install: `npx expo install expo-location`
- [ ] Get current location:
  ```typescript
  import * as Location from 'expo-location';
  
  const { status } = await Location.requestForegroundPermissionsAsync();
  if (status !== 'granted') {
    alert('Location permission required');
    return;
  }
  
  const location = await Location.getCurrentPositionAsync({});
  const { latitude, longitude } = location.coords;
  ```
- [ ] Reverse geocode (lat/lng → address):
  ```typescript
  const [address] = await Location.reverseGeocodeAsync({ latitude, longitude });
  ```
- [ ] Verify: Can get current location and address

### Day 3: Push Notifications
- [ ] Install: `npx expo install expo-notifications`
- [ ] Register for notifications:
  ```typescript
  const { status } = await Notifications.requestPermissionsAsync();
  const token = await Notifications.getExpoPushTokenAsync();
  console.log(token.data); // Send to your backend
  ```
- [ ] Schedule local notification:
  ```typescript
  await Notifications.scheduleNotificationAsync({
    content: { title: 'Reminder', body: 'Check out new movies!' },
    trigger: { seconds: 5 },
  });
  ```
- [ ] Handle notification tap (deep link)
- [ ] Verify: Notifications appear, tapping opens correct screen

### Day 4: Haptics & Audio
- [ ] Install: `npx expo install expo-haptics`
- [ ] Add haptic feedback:
  ```typescript
  import * as Haptics from 'expo-haptics';
  
  // Light feedback on tap
  Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light);
  
  // Success notification
  Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);
  ```
- [ ] Verify: Taps feel responsive with haptic feedback

### Day 5: Build Feature-Rich App
- [ ] Combine native features into one app:
  - Profile photo (camera/gallery)
  - Location display
  - Notification reminders
  - Haptic feedback on actions
- [ ] Verify: All native features work on physical device

---

## Permissions Pattern

Every native feature requires:
1. **Request permission** before using
2. **Handle denial** gracefully (explain why needed)
3. **Provide fallback** if denied

```typescript
async function getPermission(requestFn, feature) {
  const { status } = await requestFn();
  if (status === 'granted') return true;
  
  Alert.alert(
    `${feature} Permission`,
    `${feature} is needed for this feature. Please enable in Settings.`,
    [{ text: 'Cancel' }, { text: 'Open Settings', onPress: Linking.openSettings }]
  );
  return false;
}
```

---

## Verification

```bash
# MUST test on physical device (not simulator)
npx expo start

# Test:
# 1. Camera opens, photo taken
# 2. Gallery picker works
# 3. Location coordinates displayed
# 4. Address shown from coordinates
# 5. Push notification appears
# 6. Tapping notification opens app
# 7. Haptics felt on button press
```

**Note:** Some features require a development build, not Expo Go. If something doesn't work in Expo Go, that's expected — note it and move on.
