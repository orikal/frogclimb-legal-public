# App Store Connect — checklist for FrogClimb (`com.frogjump.game`)

Use this checklist alongside Apple’s current documentation. Apple changes review guidelines and privacy questionnaires over time—verify against [App Store Review Guidelines](https://developer.apple.com/app-store/review/guidelines/) and [App privacy details](https://developer.apple.com/help/app-store-connect/reference/app-privacy-details).

**Replace placeholders** in the legal Markdown files before linking them in App Store Connect.

---

## 1. Metadata URLs (required / commonly required)

| Field | Typical requirement | Suggested source |
|------|----------------------|------------------|
| **Privacy Policy URL** | Public, stable | Link to `privacy-policy-en.md` on GitHub **or** GitHub Pages HTML |
| **Support URL** | Public help/contact | Your website, or a `support.md` page in this repo published the same way |
| **Marketing URL** | Optional | Your landing page |

Apple does not host your legal text for you—the URL must resolve for reviewers.

---

## 2. App Privacy questionnaire (nutrition labels)

Answer based on **what leaves the user’s device to you or your third-party partners** as implemented in your shipping build.

The following reflects the **FrogClimb codebase** reviewed for this repository: **no developer-operated backend**, **local persistence** (AsyncStorage), **optional Game Center score submit on iOS**, **StoreKit / in-app purchases via `expo-iap`**, **accelerometer only on-device for tilt controls**, **no integrated third-party analytics SDK**, **no integrated ad SDK** (UI may reference ads; reward paths may be mocked—disclose accurately for the binary you submit).

### 2.1 Data **not** collected by you (typical for local-only gameplay state)

Gameplay progress stored **only on-device** (scores, settings, inventory, cosmetics) and **never transmitted** to your servers is commonly disclosed as **not collected** by the developer, because it is not sent off-device. Confirm this remains true in your release build.

### 2.2 Purchases (StoreKit / `expo-iap`)

Apple processes payments. Many apps still disclose purchase-related activity consistent with Apple’s categories. A conservative approach:

- If you **do not** send purchase events to your own servers or third-party analytics: you may be able to select **“Data Not Collected”** for developer collection, **or** disclose purchases as processed by Apple under the relevant category—**confirm the exact questionnaire wording** for your submission month.

If you add analytics that logs purchases, update labels and the Privacy Policy.

### 2.3 Apple Game Center (leaderboards / authentication)

If your build uses Game Center and submits scores:

- Data may be processed by **Apple** as a platform service. Apple’s documentation and the questionnaire evolve; reviewers expect consistency between labels, the Privacy Policy, and behavior.

Practical guidance:

- Mention Game Center clearly in the Privacy Policy (done in `privacy-policy-en.md`).
- In the questionnaire, if asked about **Gameplay Content** / **Identifiers** linked to the user through Game Center, answer consistently with Apple’s definitions for the version of the form you see.

### 2.4 Device motion (tilt)

If motion data is **processed only on-device** for controls and **not sent** to you or partners: typically **not collected** off-device.

Ensure `NSMotionUsageDescription` is present in the iOS build when tilt is available (configured in the app’s `app.json` for FrogClimb).

### 2.5 Tracking (App Tracking Transparency)

If you **do not** track users across apps/websites for advertising (and do not call ATT prompts), you generally do **not** need an `NSUserTrackingUsageDescription`.

If you add ad SDKs that use IDFA or cross-app tracking, you must update disclosures and may need ATT.

### 2.6 Ads / rewarded video

If the shipped binary **does not** load third-party ads, do **not** declare ad network data collection. If you later integrate AdMob/AppLovin/etc., update **Privacy Policy**, **labels**, and add any required SKAdNetwork entries and ATT flows.

---

## 3. Export compliance / encryption

Expo/React Native apps use HTTPS and standard OS cryptography. Answer Apple’s export compliance questions truthfully (often you use encryption only for exempt purposes like HTTPS). See Apple’s wizard text at submission time.

---

## 4. In-app purchases

- Create consumable IAP IDs matching your app configuration (see `powerUpProducts.ts` in the game repo).  
- Attach screenshots demonstrating the purchase flow if reviewers request.  
- Ensure restore/finish-transaction behavior matches StoreKit rules (`expo-iap`).

---

## 5. Game Center

- Leaderboard IDs must exist in App Store Connect and match the app.  
- Provide a test account or clear reviewer notes if authentication is tricky.  
- If Game Center is optional, state that in Review Notes.

---

## 6. Content rights & age rating

- Confirm you own or license all 3D models, textures, audio, and fonts.  
- Complete the age rating questionnaire honestly (mild cartoon gameplay vs hazards).  
- If you are **not** targeting children under 13 as a primary audience, avoid claiming features you do not implement (accounts, chat, etc.).

---

## 7. Review notes (suggested template)

Paste and adapt:

> FrogClimb is an offline climbing game. Progress is stored locally. IAP are consumable power-ups via StoreKit. Optional Game Center submits best height score on game over when the player is signed in. Tilt controls use on-device motion only; motion is not sent to our servers. There is no developer login. [If ads are mocked:] Rewarded ad buttons may appear but the submitted build does not include a third-party ad SDK—labels reflect no ad data collection.

---

## 8. After you change the app

Any new SDK (analytics, ads, social login, cloud save, push notifications) requires:

- Privacy Policy update  
- App Privacy answers update  
- Purpose strings (`Info.plist`) as required

---

*This checklist is operational guidance only—not legal advice.*
