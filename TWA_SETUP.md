# PlushPal Android Wrap (PWABuilder + TWA)

Prepared in this workspace at:
`/home/parthabot/.openclaw/workspace/plushpal-twa`

## What was added

- `manifest.webmanifest`
- `sw.js`
- PWA tags + service worker registration in `index.html`
- App icons:
  - `assets/img/icon-192.png`
  - `assets/img/icon-512.png`

## 1) Deploy updated web app

Deploy this modified site to your domain first (e.g. `https://www.plushpal.app/`).

## 2) Validate PWA quickly

In Chrome DevTools -> Application:
- Manifest should load
- Service worker should register
- Site should be installable

## 3) Generate Android package (PWABuilder)

1. Open https://www.pwabuilder.com/
2. Enter your deployed URL
3. Choose **Android** -> **Trusted Web Activity (TWA)**
4. Set package id (example: `app.plushpal.android`)
5. Download generated Android Studio project

## 4) Configure Digital Asset Links (required)

Create `https://YOUR_DOMAIN/.well-known/assetlinks.json`:

```json
[
  {
    "relation": ["delegate_permission/common.handle_all_urls"],
    "target": {
      "namespace": "android_app",
      "package_name": "YOUR_PACKAGE_NAME",
      "sha256_cert_fingerprints": [
        "YOUR_SHA256_FINGERPRINT"
      ]
    }
  }
]
```

Get SHA-256 from your signing keystore:

```bash
keytool -list -v -keystore your-release-key.jks -alias your_alias
```

## 5) Build release AAB

In Android Studio:
- Build -> Generate Signed Bundle / APK
- Choose Android App Bundle (AAB)
- Upload to Play Console internal testing first

## Important note about Bluetooth

Even with TWA, Web Bluetooth on Android can be device/browser dependent.
Test pairing + reconnect flows on at least:
- Pixel (latest Chrome)
- Samsung device (latest Chrome)

If BLE reliability is poor, native Kotlin BLE app is the fallback.
