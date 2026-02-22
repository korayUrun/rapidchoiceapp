# RapidChoice – Project Overview

RapidChoice is a **multi‑tool decision‑making app** built with SwiftUI. It helps indecisive people
make quick, random choices in a playful way using:

🌐 **Official Website:** [https://rapidchoiceapp.vercel.app](https://rapidchoiceapp.vercel.app)

1. **Decision Wheels** – custom wheels with weighted options for complex decisions.
2. **Dice Roller** – roll one or two dice with realistic 3D animations.
3. **Coin Flip** – smooth 3D coin flips for instant tiebreakers.
4. **Settings** – fine‑tune theme, haptics and language.

The app focuses on delightful details: gradients, glassmorphism, smooth animations, haptic
feedback and confetti effects, while keeping all user data on device.

---

## 📱 Screenshots

<p align="center">
  <img src="ss/wheel.png" alt="Wheel Screenshot" width="250"/>
  <img src="ss/dices.png" alt="Dices Screenshot" width="250"/>
  <img src="ss/coin.png" alt="Coin Screenshot" width="250"/>
</p>

## 📱 Core Features

### 1. Decision Wheels

- Create multiple decision wheels (categories).
- Add, edit and delete options with **custom weights (voting power)**.
- Rename wheels after creation.
- Weighted random selection algorithm.
- Physics‑based spinning animation.
- Clear results with confetti celebrations.
- Delete confirmations to prevent mistakes.
- Adaptive dashboard grid (2–4 columns depending on device).

### 2. Dice Roller

- Roll **1–2 dice** at the same time.
- Realistic 3D dice using SceneKit.
- True 3D cube with all faces visible during rotation.
- Tap, press the CTA button **or shake the device** to roll.
- In‑context info sheet explaining controls.
- Haptic feedback that respects Settings toggles.

### 3. Coin Flip

- Simple heads/tails decision tool.
- 3D rotation using `rotation3DEffect`.
- Tap the coin or use the button to flip.
- Random result generation with clear result text.
- Haptic feedback on start and result.
- Fully localized interface and VoiceOver‑friendly labels.
- Adaptive colors for light/dark mode.

### 4. Settings

- Theme selection: **Auto / Light / Dark**.
- Language picker: **English, Turkish, Spanish, Simplified Chinese**.
- Haptic feedback toggle.
- Sound effects toggle (UI in place, audio wiring pending).
- "Rate RapidChoice" link plus lightweight About section.
- Persistent settings across launches via `SettingsManager`.

### General

- Tab‑based navigation.
- Persistent storage with error handling.
- Swipe‑to‑delete gestures.
- Real‑time percentage calculations.
- iPad and landscape optimization.
- Inline validation for wheel/category creation and option inputs.
- Localized copy for 4 languages with cached bundle lookup.
- Shared info sheet and shake detector utilities reused across tools.

---

## 🏗️ Architecture

**Pattern:** Clean MVVM (Model–View–ViewModel) with tab‑based navigation.

```
RapidChoice/
├── Models/       # Data entities (Category, WheelOption, DiceRoll, AppSettings)
├── Views/        # SwiftUI views (wheels, dice, coin, settings, shared)
├── ViewModels/   # Business logic (CategoryViewModel, DiceViewModel, SettingsViewModel)
├── Managers/     # Services (PersistenceManager, SettingsManager)
├── Extensions/   # Utilities (Color+Hex, LocalizationHelper, ShakeDetector)
└── Styles/       # UI components (ScaleButtonStyle)
```

### Key Files

**Models**

- `RapidChoice/Models/Category.swift` – wheel entity with name, options and metadata.
- `RapidChoice/Models/WheelOption.swift` – individual choices with weight / probability.
- `RapidChoice/Models/DiceRoll.swift` – roll result with timestamp and dice values.
- `RapidChoice/Models/AppSettings.swift` – user preferences and configuration.

**ViewModels**

- `RapidChoice/ViewModels/CategoryViewModel.swift` – wheel CRUD operations.
- `RapidChoice/ViewModels/DiceViewModel.swift` – dice rolling logic and history.
- `RapidChoice/ViewModels/SettingsViewModel.swift` – settings management.

**Managers**

- `RapidChoice/Managers/PersistenceManager.swift` – category storage with error handling.
- `RapidChoice/Managers/SettingsManager.swift` – settings persistence.

**Views**

- `RapidChoice/ContentView.swift` – tab navigation container.
- `RapidChoice/SpinChoiceApp.swift` – app entry point (main scene).
- `RapidChoice/Views/CategoriesListView.swift` – wheels dashboard with adaptive grid.
- `RapidChoice/Views/WheelSpinnerView.swift` – core spinning feature.
- `RapidChoice/Views/AddCategoryView.swift` – create new wheels.
- `RapidChoice/Views/EditCategoryView.swift` – rename existing wheels.
- `RapidChoice/Views/AddOptionView.swift` – add options with weights.
- `RapidChoice/Views/OptionsManagementView.swift` – edit options.
- `RapidChoice/Views/DiceRollerView.swift` – main dice interface.
- `RapidChoice/Views/SceneKitDiceView.swift` – 3D dice rendering.
- `RapidChoice/Views/CoinFlipView.swift` – coin flipping interface.
- `RapidChoice/Views/SettingsView.swift` – configuration interface.
- `RapidChoice/Views/ConfettiView.swift` – celebration animation.
- `RapidChoice/Views/EmptyStateView.swift` – reusable empty states.
- `RapidChoice/Views/InfoSheetView.swift` – reusable tips/FAQ sheet.

**Extensions & Utilities**

- `RapidChoice/Extensions/Color+Hex.swift` – hex color support.
- `RapidChoice/Extensions/LocalizationHelper.swift` – localized string loading with caching.
- `RapidChoice/Extensions/ShakeDetector.swift` – motion handling bridge for shake‑to‑roll.
- `RapidChoice/Styles/ScaleButtonStyle.swift` – common press animation style.

---

## 🎨 Design System

- **Gradients:** vibrant linear gradients throughout.
- **Colors:** 8‑color palette with high contrast.
- **Animations:** spring physics, easing curves and haptic feedback.
- **Typography:** SF Pro with a clear hierarchy.
- **Effects:** glassmorphism, shadows and subtle blurs.

---

## 🔧 Technical Highlights

### Wheel Spinning Algorithm

1. **Weighted random selection** using a cumulative weight distribution.
2. **Angle calculation:** segments positioned counter‑clockwise from 0°.
3. **Pointer alignment:** fixed pointer at 270° (top), wheel rotates to align winner.
4. **Multi‑revolution:** 5–7 full spins before stopping.
5. **Verification:** post‑spin check ensures the correct winner.

### 3D Dice Implementation (SceneKit)

1. **True 3D geometry:** `SCNBox` with 6 textured faces generated via Core Graphics.
2. **Material mapping:** custom dice textures (1–6 dots) per cube face.
3. **Physics‑style animation:** combined rotations on X/Y/Z during roll.
4. **Face alignment:** precise Euler angles for the final orientation.
5. **Camera setup:** fixed camera with ambient + directional lighting.

### Coin Flip Animation

1. 3D rotation via `rotation3DEffect` on the Y‑axis.
2. Random spin count (5–7 full rotations, ~1800–2520 degrees).
3. 1‑second animation using an ease‑out curve.
4. Dual triggers: tap the coin directly or use the button.
5. Haptic feedback on start and success.

### High‑Level User Flow

**Tab 1 – Wheels**

1. Empty state encourages creating a wheel.
2. Add category name + ≥2 options (inline validation + AddOption sheet).
3. Adaptive grid lists wheels; tap to spin, long‑press to rename or delete.
4. OptionsManagementView manages option CRUD with confirmation dialogs.

**Tab 2 – Dice**

1. Select single or double dice via DiceCountSelector.
2. Tap the dice, shake the device, or press the CTA button to roll.
3. SceneKit renders results; Info sheet explains controls; haptics respect settings.

**Tab 3 – Coin Flip**

1. Tap coin or button to animate rotation.
2. Result text and VoiceOver‑friendly labels update immediately.

**Tab 4 – Settings**

1. Adjust theme and language.
2. Toggle haptics and (future) sound effects.
3. Access rate prompt and About metadata.

---

## 🚀 Getting Started (Development)

1. Open `RapidChoice.xcodeproj` in Xcode.
2. Use Xcode 15 or later, targeting iOS 17 (or current deployment target in the project).
3. Select a simulator or a physical device.
4. Build & run.

Data is stored locally using `UserDefaults` via `PersistenceManager` and `SettingsManager`; there
is no custom backend.

---

## 🌍 Localization

RapidChoice ships with four fully localized languages:

- English (`en.lproj`)
- Turkish (`tr.lproj`)
- Spanish (`es.lproj`)
- Simplified Chinese (`zh-Hans.lproj`)

`LocalizationHelper` caches `Bundle` lookups for performance and keeps string access centralized.

---

## 🔐 Privacy & Safety

- RapidChoice is **not** a gambling app; results have no monetary value.
- No user accounts; all decision content stays on device.
- No third‑party analytics or ad SDKs.
- Motion sensors are only used for shake‑to‑roll; no access to contacts, photos, camera or
  microphone.

For the full legal text used on the App Store, see the dedicated Privacy Policy page in the
showcase site.

---

## ⚠️ Known Limitations

1. **Sound effects toggle is UI‑only** – audio plumbing not implemented yet.
2. **UserDefaults persistence** – fine for light data but no iCloud/backup or conflict handling.
3. **No data export/sharing** – wheels and roll history stay on device.
4. **Dice count capped at two** – UI, logic, and SceneKit scene assume max 2 dice.
5. **Deletes are permanent** – confirmations exist but there is no undo/restore queue.

---

_Last updated: February 17, 2026_
_Originally created: November 5–6, 2025_
