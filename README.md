# FanConnect Android MVP

Android app (Kotlin) to choose a fan category, select an already-paired Bluetooth Classic controller, connect via Serial Port Profile (SPP), and send `ON\n` / `OFF\n` power commands.

## Important scope
This is a starter MVP, not a universal adapter for every fan. The Android app works with a controller that exposes Bluetooth Classic SPP and accepts the FanConnect text protocol. Different brands/protocols, BLE-only fans, Wi-Fi APIs, IR remotes, and ordinary unmodified fans need separate integrations or compatible hardware. A phone's Bluetooth cannot directly switch a normal mains-powered fan.

## Build APK
1. Install Android Studio (current stable) and Android SDK Platform 35.
2. Open this folder (`FanConnectMVP`) in Android Studio.
3. Allow Gradle sync. Android Gradle Plugin 8.7.3 and Kotlin 2.0.21 are declared in `build.gradle`.
4. On an Android phone, enable Developer options and USB debugging, connect it to your computer, and press **Run** in Android Studio. Or choose **Build > Build APK(s)** to produce an APK.
5. On the phone, pair your controller from Android Settings > Bluetooth before opening FanConnect.
6. In the app, select fan category, select the paired controller, tap Connect, then tap the power button.

## Controller test firmware (optional)
`firmware/ESP32_FanConnect/ESP32_FanConnect.ino` is a minimal ESP32 Bluetooth Classic SPP test sketch. It advertises as `FanConnect-Controller`; it accepts `ON`, `OFF`, and `STATUS` lines. It toggles GPIO 23 as a **low-voltage logic signal only**. It does not drive a fan or relay directly.

### Hardware safety
Never connect an ESP32, phone, GPIO pin, or breadboard directly to household mains. For a real fan, use a properly rated, enclosed, isolated and certified fan controller/module compatible with the fan motor, installed by a qualified electrician. Do not use this example GPIO sketch as a mains wiring guide.

## Protocol
- App -> controller: UTF-8 `ON\n` or `OFF\n`
- Example controller response: `STATE:ON` / `STATE:OFF`
- Current Android MVP updates the button after sending; it does not yet parse acknowledgements, so the UI cannot prove the fan physically switched.

## Next improvements
- Read and display `STATE:ON/OFF` acknowledgements
- Add BLE GATT and Wi-Fi integrations
- Add vendor-specific APIs and IR support for phones that have an IR blaster
- Add reconnect handling, device whitelist, and tests
