# Cookie Store - How to Run the Application

## 1. Install the required tools

- Install Flutter and add its `bin` directory to your system PATH.
- Use a Flutter SDK that includes Dart compatible with `^3.13.4`, as required by `pubspec.yaml`.
- For Android, install Android Studio and the Android SDK, then create an emulator or connect an Android phone with USB debugging enabled.
- For iOS, use macOS with Xcode and an iOS simulator or connected iPhone.

Check your setup:

```sh
flutter doctor
```

Resolve any issues reported for the platform you want to use before continuing.

## 2. Open the project

Open the project folder in VS Code or Android Studio. In a terminal, change to the folder containing `pubspec.yaml`:

```sh
cd cookie_store
```

If you are already in the project root, skip this command.

## 3. Install dependencies

```sh
flutter pub get
```

## 4. Configure Supabase

Open [`lib/config/app_config.dart`](lib/config/app_config.dart) and set the project URL and public key for the Supabase backend you will use:

```dart
class AppConfig {
  static const String supabaseUrl = 'https://your-project-id.supabase.co';
  static const String supabaseAnonKey = 'your-supabase-publishable-or-anon-key';
}
```

Use a Supabase publishable or legacy anon public key. Keep service-role and payment secret keys in the backend.

The application reads `AppConfig` directly. A `.env` file is not loaded automatically.

Your Supabase backend must already contain the `products`, `profiles`, `favorites`, and `orders` tables and the `username_available` RPC. Database setup scripts are not included in this repository. If you are using an existing backend, obtain its configuration from the project maintainer.

To test payments, the backend also needs a deployed `create-invoice` Edge Function and a Xendit webhook that updates order status. Match the payment redirect URLs in [`lib/screens/payment_webview_screen.dart`](lib/screens/payment_webview_screen.dart) to those configured by the Edge Function. The current URLs are placeholders.

## 5. Start a device or emulator

Start your Android emulator or iOS simulator, or connect your phone. List the available devices:

```sh
flutter devices
```

## 6. Run the application

If only one target device is available, run:

```sh
flutter run
```
flutter run --dart-define-from-file=.env
flutter build apk --release --dart-define-from-file=.env

## install direct to devices

flutter install --release 

To choose a specific device, replace `DEVICE_ID` with its ID from `flutter devices`:

```sh
flutter run -d DEVICE_ID
```

Wait for the build and installation to finish. The application will open on the selected device.

## 7. Sign in and use the app

Create an account or sign in with an existing account. If email confirmation is enabled, confirm your email before signing in. You can then browse products, add items to your cart, and test checkout once the payment backend is configured.

While `flutter run` is active, press `r` to hot reload, `R` to restart the app, or `q` to stop the session.

## 8. Build and download the Android APK

After completing the setup and Supabase configuration above, run this command from the project root:

```sh
flutter build apk --release
```

When the build finishes, find the APK on your computer at:

```text
build/app/outputs/flutter-apk/app-release.apk
```

This is the file you can copy to an Android phone or share for download. To make it available to others, upload `app-release.apk` to a file-sharing service such as Google Drive and share its download link.

### Install the APK on an Android phone

1. Transfer `app-release.apk` to your phone using USB, or download it from the shared link.
2. Open the APK from your phone's Downloads folder or file manager.
3. If prompted, allow **Install unknown apps** for the browser or file manager you are using.
4. Tap **Install**, then tap **Open** when installation finishes.

The installed app runs without a connected computer. An internet connection is still required for Supabase and payments.

The current Android release configuration uses the debug signing key for testing. Configure a release signing key before publishing the app.

## Troubleshooting

- **`flutter` is not recognized:** Add the Flutter SDK's `bin` directory to PATH, then reopen your terminal.
- **Dart SDK version error:** Use a Flutter SDK whose bundled Dart version satisfies the requirement in `pubspec.yaml`.
- **No devices found:** Start an emulator or connect a phone with USB debugging enabled, then run `flutter devices` again.
- **Authentication or database errors:** Check the values in `AppConfig` and confirm the required backend tables, RPC, and access policies are configured.
- **Checkout fails:** Confirm the `create-invoice` Edge Function, Xendit webhook, and matching payment redirect URLs are configured.
