# Home Decor Handicraft — Stock Register (Android App)

This is a complete Flutter project for a simple Android stock register app:
Product Image + Name + Quantity + Cost Price, with instant search, auto stock
value calculation, and JSON backup/restore. Everything is stored locally on
the phone in a SQLite database (survives app close / phone restart).

I could not compile the actual APK inside the chat sandbox (it has no
Android/Flutter SDK or internet access to Google's servers). Below are two
ways to turn this code into a real, installable APK. **Option A needs no
installation on your computer at all.**

---

## Option A — Build the APK in the cloud with GitHub Actions (recommended, no installs)

1. Create a free account at https://github.com if you don't have one.
2. Create a new **empty** repository (e.g. `home-decor-handicraft`).
3. Upload every file/folder from this project into that repository
   (GitHub's web "Add file → Upload files" works fine — just drag the whole
   folder in, including the hidden `.github` folder).
4. Go to the repository's **Actions** tab. A workflow called
   "Build Android APK" will run automatically (or click "Run workflow" if it
   doesn't start).
5. Wait 3–5 minutes for it to finish (green checkmark).
6. Open the finished run → scroll to **Artifacts** → download
   `home-decor-handicraft-apk` (a zip containing `app-release.apk`).
7. Transfer that APK to your Android phone (via USB, WhatsApp to yourself,
   Google Drive, email — any method) and tap it to install.
   - Your phone may ask you to allow "install from this source" the first
     time — allow it, this is normal for any app installed outside the Play
     Store.

You now have a real, standalone Android app with its own icon named
**Home Decor Handicraft** in your app drawer.

If you ever want to update the app, just change the code in the repository
and push again — a new APK will be built automatically.

---

## Option B — Build locally with Android Studio

1. Install **Android Studio** (free): https://developer.android.com/studio
   During setup, let it install the Android SDK.
2. Install **Flutter**: https://docs.flutter.dev/get-started/install
3. Open a terminal in this project's folder and run:
   ```
   flutter create .
   ```
   This generates the standard `android/` (and `ios/`) folders that Flutter
   needs, matched to your installed Flutter version. It will NOT overwrite
   the `lib/` folder or `pubspec.yaml` already in this project.
4. Install dependencies:
   ```
   flutter pub get
   ```
5. Generate the app icon:
   ```
   flutter pub run flutter_launcher_icons
   ```
6. Open `android/app/src/main/AndroidManifest.xml` and confirm
   `android:label="Home Decor Handicraft"` (already set by flutter create
   using the project name — edit it if it shows a different label).
7. Connect your Android phone via USB (enable "Developer options" → "USB
   debugging" on the phone), then run:
   ```
   flutter build apk --release
   ```
8. The APK will appear at:
   ```
   build/app/outputs/flutter-apk/app-release.apk
   ```
   Copy this file to your phone and install it, or run
   `flutter install` to push it directly over USB.

To build an AAB for the Play Store instead:
```
flutter build appbundle --release
```

---

## Project structure

```
lib/
  main.dart                     - app entry, bottom navigation (Home/Products/Settings)
  models/product.dart           - Product data model + stock value calculation
  db/database_helper.dart       - SQLite (sqflite) database, CRUD, search, totals
  utils/formatters.dart         - ₹ Indian Rupee number formatting
  widgets/product_card.dart     - product list card (image, qty, cost, value, edit/delete)
  screens/home_screen.dart      - search bar + totals (products/quantity/stock value)
  screens/products_screen.dart  - full product list with edit/delete
  screens/product_form_screen.dart - add/edit product with camera/gallery image picker
  screens/settings_screen.dart  - export/import JSON backup (includes images as base64)
assets/icon/icon.png             - app icon source (used by flutter_launcher_icons)
.github/workflows/build_apk.yml  - cloud build workflow (Option A)
```

## What's deliberately NOT included

Per the spec: no selling price, stock in/out, low-stock alerts, suppliers,
customers, GST, invoicing, payroll, CRM, charts, or accounting features.
This is strictly: image, name, quantity, cost price, and stock value.

## Notes

- All input is validated: quantity must be a non-negative whole number, cost
  price must be a non-negative number, and product name is required.
- Delete always asks for confirmation.
- Images are compressed on capture (max 1024px, 70% quality) to save phone
  storage, and stored in the app's private local storage — not shared
  anywhere except inside a backup file you explicitly export.
- Search matches any word/substring within the product name, instantly as
  you type (e.g. searching "Vase" finds "Brass Flower Vase").
