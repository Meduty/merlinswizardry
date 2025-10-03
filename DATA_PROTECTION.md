# Data Protection & Privacy Information
## Votery - Lottery of Choice App

**Version:** 1.0.2  
**Last Updated:** October 2024  
**App Package:** com.merlinswizardry.votery

---

## Executive Summary

**Votery** is a local, offline, pass-and-play group decision-making game. The app is designed with **privacy-first principles** and operates entirely on-device with **no network connectivity** in production builds. All user data remains local to the device and is never transmitted to external servers.

### Quick Privacy Facts
- ✅ **100% Offline** - No internet connectivity required or used
- ✅ **Local Storage Only** - All data stored on device
- ✅ **No User Accounts** - No registration or authentication
- ✅ **No Analytics** - No tracking or telemetry
- ✅ **No Ads** - No advertising or third-party ad networks
- ✅ **Minimal Permissions** - Only INTERNET permission for development/debug builds
- ✅ **No PII Collection** - Only user-entered names (locally stored)

---

## 1. Data Collection & Storage

### 1.1 Data Stored Locally on Device

The app stores the following data types **exclusively on the user's device** using local storage mechanisms:

#### A. Player Names
- **Type:** User-entered text strings
- **Purpose:** Display player names during gameplay
- **Storage:** Hive database (local NoSQL database)
- **Retention:** Stored until manually deleted by user
- **Location:** `/data/data/com.merlinswizardry.votery/` on Android
- **Sensitivity:** Low - User-chosen display names only (not verified identities)
- **File:** `lib/models/inventory_models.dart` - PlayerInventoryItem
- **Cleanup:** Automatically removes entries older than 6 months

#### B. Option Labels
- **Type:** User-entered text strings
- **Purpose:** Display voting options during gameplay
- **Storage:** Hive database (local NoSQL database)
- **Retention:** Stored until manually deleted by user
- **Sensitivity:** Low - User-defined decision options
- **File:** `lib/models/inventory_models.dart` - OptionInventoryItem
- **Cleanup:** Automatically removes entries older than 6 months

#### C. Game History
- **Type:** Game session metadata
- **Data Stored:**
  - Player names used in game
  - Option labels used in game
  - Winner selection
  - Game mode used (Weighted/Ranked)
  - Voting results (aggregated scores)
  - Session timestamps
  - Game duration
- **Storage:** Hive database (local NoSQL database)
- **Purpose:** Display user statistics and last winner information
- **Retention:** Limited to most recent 100 game sessions
- **File:** `lib/models/game_history_models.dart` - GameSession
- **Cleanup:** Automatically prunes to 100 most recent sessions

#### D. User Statistics
- **Type:** Aggregated gameplay statistics
- **Data Stored:**
  - Total games played
  - Total decisions made
  - Favorite game mode
  - Last played timestamp
  - Last winner information
- **Storage:** Hive database (local NoSQL database)
- **Purpose:** Display gameplay statistics to user
- **File:** `lib/models/game_history_models.dart` - UserStats

#### E. App Settings/Preferences
- **Type:** User preferences
- **Data Stored:**
  - Animation speed preferences
  - Game rule toggles (negative votes, anonymous ballots, etc.)
  - UI preferences (drawing view type)
  - Timer settings
- **Storage:** SharedPreferences (Android key-value storage)
- **Purpose:** Persist user preferences across app sessions
- **File:** `lib/services/settings_service.dart`
- **Sensitivity:** None - purely UI/gameplay preferences

### 1.2 Data NOT Collected

The app explicitly does **NOT** collect, store, or transmit:
- ❌ Personal Identifiable Information (PII) beyond user-entered names
- ❌ Email addresses
- ❌ Phone numbers
- ❌ Location data
- ❌ Device identifiers (IMEI, Android ID, etc.)
- ❌ Contacts or contact lists
- ❌ Photos or media files
- ❌ Browsing history
- ❌ Usage analytics or telemetry
- ❌ Crash reports
- ❌ Advertising IDs
- ❌ Financial information
- ❌ Health data
- ❌ Biometric data

---

## 2. Network Activity & Permissions

### 2.1 Android Permissions

**Production Build (Release):**
- **INTERNET permission:** Not used in production
  - Present in debug/profile manifests only for Flutter hot reload functionality
  - NOT present in release APK
  - See: `android/app/src/debug/AndroidManifest.xml` (debug only)
  - See: `android/app/src/profile/AndroidManifest.xml` (profile only)

**Other Permissions:**
- None - The app requests **no additional permissions**

### 2.2 Network Connectivity

**Production Release:**
- ✅ **Zero network calls** - The app makes no HTTP/HTTPS requests
- ✅ **No web sockets** - No real-time connections
- ✅ **No API calls** - No external service integrations
- ✅ **Fully offline** - Can operate in airplane mode

**Google Fonts Note:**
- The `google_fonts` package is used for typography
- **Important:** Fonts are bundled with the app in the assets folder
- No runtime font downloads occur
- All fonts are pre-loaded at build time
- See: `pubspec.yaml` - fonts section with local Cinzel font files

---

## 3. Third-Party Dependencies Analysis

### 3.1 Core Dependencies

All dependencies have been reviewed for data protection implications:

#### A. **flutter_riverpod** (v2.4.9)
- **Purpose:** State management framework
- **Privacy Impact:** None - purely local state management
- **Network Activity:** None
- **Data Collection:** None

#### B. **go_router** (v16.2.1)
- **Purpose:** Navigation and routing
- **Privacy Impact:** None - local navigation only
- **Network Activity:** None
- **Data Collection:** None

#### C. **hive** (v2.2.3) & **hive_flutter** (v1.1.0)
- **Purpose:** Local NoSQL database
- **Privacy Impact:** All data stored locally on device
- **Network Activity:** None
- **Data Collection:** None
- **Storage Location:** `/data/data/com.merlinswizardry.votery/`
- **Encryption:** Not encrypted by default (stored in app's private directory)

#### D. **shared_preferences** (v2.2.2)
- **Purpose:** Key-value storage for app settings
- **Privacy Impact:** All data stored locally on device
- **Network Activity:** None
- **Data Collection:** None
- **Storage Location:** Android SharedPreferences (app-private)

#### E. **uuid** (v4.2.2)
- **Purpose:** Generate unique identifiers for game entities
- **Privacy Impact:** None - random UUIDs generated locally
- **Network Activity:** None
- **Data Collection:** None
- **Note:** UUIDs are for internal game object identification only

#### F. **google_fonts** (v6.1.0)
- **Purpose:** Typography and font rendering
- **Privacy Impact:** **IMPORTANT - Fonts are bundled, not downloaded**
- **Network Activity:** None in this app - all fonts pre-bundled
- **Data Collection:** None
- **Implementation:** Custom Cinzel font family bundled in `assets/fonts/`
- **Note:** While google_fonts *can* download fonts at runtime, this app bundles all fonts locally

#### G. **flutter_native_splash** (v2.3.10)
- **Purpose:** Splash screen management
- **Privacy Impact:** None - UI only
- **Network Activity:** None
- **Data Collection:** None

#### H. **cupertino_icons** (v1.0.8)
- **Purpose:** iOS-style icons
- **Privacy Impact:** None - static assets only
- **Network Activity:** None
- **Data Collection:** None

### 3.2 Development Dependencies (Not in Production)

- **flutter_lints** - Code quality only, not in release build
- **build_runner** - Build-time code generation, not in release
- **hive_generator** - Build-time code generation, not in release
- **flutter_launcher_icons** - Build-time icon generation, not in release

---

## 4. Data Processing & Purpose

### 4.1 How Data is Used

| Data Type | Purpose | Processing | Sharing |
|-----------|---------|------------|---------|
| Player Names | Display during gameplay, populate suggestions | Local only | None |
| Option Labels | Display during voting, populate suggestions | Local only | None |
| Game History | Show statistics, last winner | Local aggregation | None |
| Vote Results | Calculate winner, show results | Local computation | None |
| Settings | Customize user experience | Local storage | None |

### 4.2 Data Retention

- **Player/Option Inventory:** Automatically cleaned after 6 months of inactivity
- **Game History:** Limited to 100 most recent sessions
- **Settings:** Persist indefinitely until user resets or uninstalls
- **Active Game State:** Cleared when app is closed or game ends

### 4.3 Data Deletion

Users can delete their data through:
1. **Settings → Reset to Defaults** - Clears all settings
2. **Inventory Management** - Delete individual players/options
3. **App Uninstall** - Removes all app data (standard Android behavior)
4. **Clear App Data** - Android system settings → Apps → Votery → Clear Data

**Note:** There is currently no single "Delete All Data" button, but users can:
- Reset settings via Settings page
- Clear individual inventory items
- Use Android's "Clear Data" feature

---

## 5. Data Security

### 5.1 Security Measures

- **App Sandbox:** Data stored in Android app-private directory (only accessible by app)
- **No Network Exposure:** Zero attack surface from network vulnerabilities
- **No Cloud Storage:** No synchronization with external services
- **Standard Android Permissions:** App data protected by Android OS permissions
- **No Third-Party SDKs:** No analytics, ads, or tracking SDKs that could leak data

### 5.2 Potential Security Considerations

- **Physical Device Access:** Anyone with physical access to unlocked device can access app
- **No User Authentication:** App has no login/password protection
- **No Data Encryption:** Local storage not encrypted (relies on Android's app sandboxing)
- **Backup Implications:** If user enables Android backup, app data may be included in backups

### 5.3 Recommendations for Sensitive Use Cases

If users plan to use the app with sensitive information:
- Disable Android cloud backups for this app
- Use device-level encryption (enabled by default on modern Android)
- Clear app data after sensitive voting sessions
- Consider using generic placeholder names instead of real names

---

## 6. Child Privacy (COPPA Compliance)

### 6.1 Age Restrictions
- App is suitable for all ages
- No age verification required
- No special child-directed features

### 6.2 Children's Data
- App does not knowingly collect data from children
- No registration or account creation
- Only user-entered names (which may or may not be from children)
- Parents should supervise if children use the app

---

## 7. GDPR Considerations

### 7.1 Data Controller
- **Users are the data controllers** - they enter and control all data
- **Developer has no access** to user data
- **No data processing agreements** needed (no data leaves device)

### 7.2 User Rights

Under GDPR, users have the following rights (all supported):

| Right | How to Exercise |
|-------|-----------------|
| Right to Access | All data visible in app UI |
| Right to Rectification | Edit names/options directly in app |
| Right to Erasure | Delete individual items or uninstall app |
| Right to Data Portability | No export feature currently, but could be added |
| Right to Object | N/A - no automated decision making |
| Right to Restriction | Stop using app or clear data |

### 7.3 Data Protection Impact Assessment (DPIA)

**Assessment:** DPIA not required because:
- No systematic monitoring
- No large-scale processing of sensitive data
- No profiling or automated decision-making
- All processing is local to user's device
- Minimal privacy risk

---

## 8. Google Play Data Safety Requirements

### 8.1 Data Safety Section Answers

For Google Play Console Data Safety section:

**Does your app collect or share any of the required user data types?**
- **Answer: NO**

**Reasoning:**
- User-entered names are not verified PII
- All data is local, never transmitted
- No sharing with third parties
- No data leaves the device

**If required to declare (conservative approach):**

**Data Collected:**
- **Personal Info → Name**
  - **Why:** User enters player names for display during gameplay
  - **Collection:** Required for app functionality
  - **Sharing:** Not shared with third parties
  - **Location:** Stored on device only
  - **Deletion:** User can delete via app or uninstall
  - **Encryption in Transit:** N/A - no transmission
  - **Encryption at Rest:** App-sandboxed storage

**App Activity:**
- **App interactions**
  - **Why:** Game history stored for statistics display
  - **Collection:** Optional (can play without history)
  - **Sharing:** Not shared
  - **Deletion:** Auto-cleanup after 100 sessions

### 8.2 Third-Party Data Sharing
- **None** - Zero third-party data sharing

### 8.3 Data Collection Declaration
Conservative recommendation:
- Declare "Name" as user-provided (optional in description)
- Declare "App interactions" as game history
- Mark all as "Not shared with third parties"
- Mark all as "Stored on device only"

---

## 9. Privacy Policy

### 9.1 Privacy Policy Requirement

**Google Play requires a privacy policy** even for apps that don't collect data. Below is a draft:

---

## DRAFT PRIVACY POLICY for Votery

**Last Updated:** October 2024

### Introduction
Votery ("we," "our," or "the app") is committed to protecting your privacy. This Privacy Policy explains how the app handles information.

### Information Collection
Votery is designed to operate entirely offline on your device. We do not collect, store, transmit, or share any personal information with our servers or third parties.

**Information Stored Locally:**
- Player names you enter (for gameplay display)
- Option labels you create (for voting)
- Game history and statistics (for your reference)
- App settings and preferences

All information is stored only on your device and never leaves your device.

### Information Sharing
We do not share, sell, or transmit any information to third parties. All data remains on your device.

### Third-Party Services
The app does not use any third-party services, analytics, or advertising networks.

### Children's Privacy
The app does not knowingly collect information from children or adults. It is suitable for all ages.

### Data Deletion
You can delete data by:
- Using the app's built-in deletion features
- Clearing app data through Android settings
- Uninstalling the app

### Changes to Privacy Policy
We may update this policy from time to time. Continued use of the app constitutes acceptance of changes.

### Contact
For questions about privacy, contact: [Your email or contact method]

---

### 9.2 Where to Host Privacy Policy

Options:
1. Create GitHub Pages: `https://[username].github.io/lottery-of-choice/privacy`
2. Add to repository: `docs/privacy.html`
3. Use simple hosting service (Google Sites, etc.)

---

## 10. Compliance Checklist

### For Google Play Release:

- [x] **No excessive permissions** - Only INTERNET for debug (removed in release)
- [x] **Data Safety questionnaire** - Can answer "No data collected" or declare conservatively
- [x] **Privacy Policy** - Draft provided above (needs to be hosted)
- [x] **No malware/deceptive behavior** - Clean app, no hidden functionality
- [x] **No sensitive permissions** - No location, camera, contacts, etc.
- [x] **Appropriate content rating** - All ages suitable
- [x] **Local storage only** - No cloud sync or external transmission
- [x] **COPPA compliant** - No child-directed data collection
- [x] **GDPR ready** - Users control all data, can delete anytime

### Additional Recommendations:

- [ ] **Host privacy policy** - Required by Google Play
- [ ] **Add "Clear All Data" button** - Currently can only clear settings or uninstall
- [ ] **Add data export feature** - For GDPR data portability (optional but good practice)
- [ ] **Document backup behavior** - Inform users if they enable Android backups
- [ ] **Consider data encryption** - Optional but adds security layer

---

## 11. Technical Implementation Details

### 11.1 Storage Mechanisms

**Hive Database:**
- **Location:** `/data/data/com.merlinswizardry.votery/`
- **Files:** 
  - `players_inventory.hive` - Player names and metadata
  - `options_inventory.hive` - Option labels and metadata
  - `game_history.hive` - Game session records
  - `user_stats.hive` - Aggregated statistics
- **Access:** App-private, protected by Android sandbox
- **Implementation:** `lib/services/hive_inventory_service.dart`

**SharedPreferences:**
- **Purpose:** App settings only (no personal data)
- **Implementation:** `lib/services/settings_service.dart`
- **Keys stored:** Animation speeds, game rule toggles, UI preferences

### 11.2 Data Flow

```
User Input (names, options)
    ↓
In-Memory State (Riverpod)
    ↓
Hive Local Database (on device)
    ↓
[No external transmission]
```

### 11.3 Code References

Key files for data handling:
- `lib/models/inventory_models.dart` - Data models for players/options
- `lib/models/game_history_models.dart` - Game history data models
- `lib/services/hive_inventory_service.dart` - Local storage service
- `lib/services/game_history_service.dart` - Game history management
- `lib/services/settings_service.dart` - Settings persistence
- `lib/providers/` - State management (in-memory only)

---

## 12. Testing & Verification

### 12.1 Network Activity Verification

To verify no network activity:
1. Build release APK: `flutter build apk --release`
2. Install on test device
3. Enable airplane mode
4. Use the app - should work fully offline
5. Use network monitoring tools (e.g., Android Studio Network Profiler)
6. Verify zero network requests

### 12.2 Permission Verification

```bash
# Check permissions in release APK
aapt dump permissions app-release.apk

# Should show: No permissions (INTERNET removed in release)
```

### 12.3 Data Storage Verification

```bash
# On rooted device or emulator
adb shell
cd /data/data/com.merlinswizardry.votery
ls -la

# Should show:
# - shared_prefs/ (settings)
# - files/ (Hive databases)
# - No network caches or external data
```

---

## 13. Future Considerations

### 13.1 Potential Features (Maintain Privacy)

If adding features, maintain privacy:
- **Export/Import:** Local files only (JSON/CSV export)
- **Themes/Customization:** Store locally in SharedPreferences
- **Additional Statistics:** Keep all aggregation local
- **Sharing Results:** Use Android Share Intent (user controls destination)

### 13.2 Features to Avoid (Privacy Risk)

Do NOT add:
- Cloud synchronization
- Online multiplayer (would require servers)
- Social media integration
- Analytics/crash reporting
- Push notifications (require Firebase)
- User accounts/authentication
- Leaderboards

---

## 14. Summary for Google Play Listing

**Recommended Description Privacy Addendum:**

> **Privacy & Data:**
> Votery is a fully offline app. All data stays on your device—we never collect, transmit, or share your information. No internet connection required. No ads. No tracking. Your privacy is guaranteed.

**Data Safety Section (Conservative):**
- Data collected: App interactions (game history), Optional user-provided names
- Data shared: None
- Data security: Stored on device only, protected by app sandbox
- Data deletion: Delete via app features or uninstall

---

## 15. Contact & Responsibility

**Developer Responsibility:**
- Ensure privacy policy is accurate and updated
- Monitor dependency updates for security issues
- Maintain offline-first architecture
- Do not add tracking or analytics without disclosure

**User Responsibility:**
- Understand app stores data locally
- Manage device security (screen lock, etc.)
- Clear data if device is shared
- Disable backups if needed for sensitive use

---

## Document Maintenance

**Version History:**
- v1.0 (October 2024) - Initial data protection documentation

**Review Schedule:**
- Review before each major release
- Review when adding new dependencies
- Review if privacy laws change

---

## Conclusion

Votery is designed with privacy as a core principle. All data remains local to the user's device, and the app operates entirely offline. This architecture minimizes privacy risks and ensures users maintain full control over their data.

**Key Takeaways:**
1. ✅ **No data leaves the device** - Complete offline operation
2. ✅ **No third-party tracking** - Zero analytics or telemetry
3. ✅ **Minimal data collection** - Only user-entered names and game results
4. ✅ **User control** - Users can delete data anytime
5. ✅ **No sensitive permissions** - INTERNET only in debug builds
6. ✅ **GDPR/COPPA friendly** - Minimal compliance burden
7. ✅ **Google Play ready** - Meets all store requirements

For Google Play release, the main action items are:
1. Host the privacy policy (use draft above)
2. Complete Data Safety questionnaire conservatively
3. Consider adding a "Clear All Data" button for user convenience

---

**Document Status:** ✅ Complete and ready for Google Play release reference
