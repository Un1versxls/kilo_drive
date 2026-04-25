# Expo Build Guide for DRIVE App

## Prerequisites

1. **Expo Account**: Sign up at https://expo.dev
2. **EAS CLI**: Install with `npm install -g eas-cli` (already available)
3. **Credentials** (for production builds):
   - iOS: Apple Developer account ($99/year)
   - Android: Google Play Console account ($25 one-time)
   - RevenueCat API keys (for in-app purchases)
   - Supabase project URL and anon key

## Quick Start - Preview Build

For testing without app store submission:

1. **Configure environment variables in Expo dashboard**:
   - Go to https://expo.dev/accounts/un11xed/projects/drive-productivity
   - Settings → Environment variables
   - Add:
     - `EXPO_PUBLIC_SUPABASE_URL` = your Supabase URL
     - `EXPO_PUBLIC_SUPABASE_ANON_KEY` = your Supabase key
     - `EXPO_PUBLIC_REVENUECAT_TEST_API_KEY` = your RevenueCat test key

2. **Start build**:
   ```bash
   eas build --platform ios --profile preview
   # or
   eas build --platform android --profile preview
   ```

## Production Build for App Store

### Step 1: Create Credentials

**For iOS**:
```bash
eas credentials -p ios
# Follow prompts to create:
# - Distribution Certificate
# - Provisioning Profile
# - Push Notification Key (optional)
```

**For Android**:
```bash
eas credentials -p android
# Follow prompts to create:
# - Keystore
```

### Step 2: Configure RevenueCat (if using)

Add to Expo environment variables:
- `EXPO_PUBLIC_REVENUECAT_IOS_API_KEY`
- `EXPO_PUBLIC_REVENUECAT_ANDROID_API_KEY`

### Step 3: Start Production Build

```bash
# Build for iOS
eas build --platform ios --profile production

# Build for Android
eas build --platform android --profile production
```

### Step 4: Submit to App Stores

After build completes successfully:

```bash
# Submit to App Store
eas submit --platform ios --latest

# Submit to Play Store
eas submit --platform android --latest
```

## Build Profiles Explained

### production
- Full app store build
- Requires all credentials
- No development client
- Optimized for release

### preview
- Test build with development client
- Easier debugging
- Can install via Expo Go
- Good for testing before production

## Important Notes

1. **App Store Connect**: For iOS, you need to create the app in App Store Connect first with bundle ID: `app.rork.i3ycts4qs75xuu5dfjfqx`

2. **Google Play Console**: For Android, create app with package: `app.rork.i3ycts4qs75xuu5dfjfqx`

3. **Build Times**: Usually 5-15 minutes for iOS, 3-10 minutes for Android

4. **App Updates**: 
   - OTA updates work automatically via Expo
   - App store builds needed for: In-app purchases, push notifications, new native modules

## Troubleshooting

**Build fails with credentials error**:
```bash
# Clear and recreate credentials
eas credentials -p ios --clear
eas credentials -p ios
```

**Development client needed**:
```bash
# Install expo-dev-client
npx expo install expo-dev-client

# Or add to your eas.json:
"developmentClient": true
```

**Environment variables not working**:
- Make sure to restart build after adding env vars
- They don't apply to running builds, only new ones

## Current Project Configuration

- **Bundle ID (iOS)**: `app.rork.i3ycts4qs75xuu5dfjfqx`
- **Package (Android)**: `app.rork.i3ycts4qs75xuu5dfjfqx`
- **App Name**: DRIVE
- **Version**: 1.0.0

## App Store Specific Requirements

Based on recent rejection, ensure:
1. ✅ Rate prompting removed (Guideline 2.2)
2. ✅ Clear subscription messaging (Guideline 2.1)
3. ✅ No crashes on sign-in flow (Guideline 2.1)
4. ✅ All features functional without subscriptions

## Support

- Expo docs: https://docs.expo.dev
- EAS docs: https://docs.expo.dev/build-reference/eas-json/
- RevenueCat: https://www.revenuecat.com/docs
