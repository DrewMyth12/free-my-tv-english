# Free My TV English

An English-only edition of [Free My TV](https://github.com/simbasjungles/free-my-tv), adapted and built for Android 11 (API 30) televisions such as the TCL C635. The weather widget is off by default. This is a community fork; see the upstream project and MIT license for attribution.

## Download

Open **Releases** and download `FreeMyTV.apk`. The GitHub Actions build also keeps its APK artifact for 30 days.

## Install on a Google TV / Android TV

You need a computer and TV on the same Wi-Fi network, and Android platform-tools (`adb`) on the computer.

1. On the TV, open **Settings → System → About** and press **Android TV OS build** seven times to enable Developer options.
2. Open **Settings → System → Developer options**, enable **USB debugging** (on some TCL firmware this is called Network debugging), and note the TV's IP under **Settings → Network & Internet**.
3. On the computer, connect and install the downloaded APK:

   ```sh
   adb connect TV_IP:5555
   adb install FreeMyTV.apk
   ```

4. Accept the debugging authorization prompt on the TV if shown. Open **Free My TV English** from the Apps screen to inspect its settings.

Installing the APK alone does not switch your TV's home screen or turn off other apps. Activating **Home Screen → Free My TV** is a separate choice inside the app and asks Android's on-device ADB authorization first.

## What activation changes

When you choose Free My TV mode, the app:

- selects Free My TV as the home screen and its screensaver;
- turns off (does not uninstall) only the optional packages listed below, if present and not owning a saved Wi-Fi network:
  - Google Play Games and YouTube Music;
  - TCL MagiConnect, Miracast, TCL Channel components, messaging/account, store demo, hotel menu, repair guide/manual and logging components.
- keeps the stock home and screensaver packages installed and enabled, and protects Google setup/recovery, Play services, Play Store, Android Settings/System UI, keyboard, WebView, TV/input/audio/remote/pairing services, and this app.

The exact package list is in [`HomeMode.java`](app/src/io/freemytv/HomeMode.java) (`DEBLOAT` and `PROTECTED`). If a package is not recognized, it is left alone. No root is used and no apps are deleted. TCL firmware varies, so review the list before activating.

## Roll back

Open **Free My TV English** from the Apps screen and choose **Home Screen → Original**. This restores the previously selected home app and screensaver and re-enables apps this app turned off. If you only installed it and never activated it, uninstall it from **Settings → Apps**.

## Build from source

Requirements: JDK 17+, Android SDK command-line tools, `platforms;android-36`, and `build-tools;36.0.0` (or newer).

```sh
sdkmanager 'platforms;android-36' 'build-tools;36.0.0'
./build.sh
```

The APK is `FreeMyTV.apk`. The build creates a local signing key if needed. Keep `freemytv.keystore` private and backed up if you build your own updates; do not publish it. GitHub Actions builds a fresh installable APK on every push and publishes it as a 30-day artifact. Since the public workflow cannot safely retain a private signing key, a later CI-built APK may not update-install over an earlier CI build; uninstall the older build first (this removes its settings).

## Edition details

- Android 11 / API 30 minimum; English only.
- Weather and crypto widgets off by default; no TMDB key or account is bundled.
- No credentials are required. Your TV settings stay on the device.
- Source is licensed under MIT. See [`LICENSE`](LICENSE) and the upstream [Free My TV](https://github.com/simbasjungles/free-my-tv) project.
