# RAPKP One — v26.3.4 preview

This working copy reconciles the Android app, shared calculation engine, KP rule layer, security behavior, and integration instructions. The rebuilt APK and test evidence are recorded below. This is a **debug-signed evaluation build**, not an owner-signed or store-ready release.

## What the app does

- Uses one deterministic OpenEphem + Skyfield + JPL DE440s calculation core for the hosted API and the paired Termux service. It preserves structured chart facts, KP/Placidus calculations, explicit timezone/DST resolution, transit context, dasha periods, sensitivity information, and the supplied rule-evidence presentation.
- Android can use a configured **HTTPS cloud API first**, with a separate, explicitly authorized paired Termux fallback. If no cloud deployment is configured, select Termux-only mode. Installing the APK does **not** deploy or connect to a cloud server.
- **AI is optional and currently available through direct Termux mode only.** The hosted API in this build performs deterministic calculations but does not expose the Termux AI job endpoint. The app does not send chart data across routes to make an AI call.
- Gemini Gem use is a separate **manual report handoff**: share/copy the handoff or deliberately upload JSON to a Gem. This is not a Gemini API call, an authenticated connector inside a consumer Gem, or an automatic transfer.
- Android profiles, request settings, and saved report history use the native encrypted vault when available. On Android, storage fails closed rather than silently saving personal data in plaintext browser storage. Browser-only use is local browser storage, not a cloud vault.

## Install/update caution — read before installing

- APK: **`native-artifacts/android/RAPKP-Termux-AI-26.3.4.apk`**
- App version/build: **26.3.4 / 2604034**
- Package: **`com.rapkp.one.preview`**
- Signature: Android **debug** certificate, SHA-256 `1bd6d40c49971d6b495de500724ccf84bbeaabf556f46a0f2813d0801151c9c3`.
- Compared with the supplied 26.3.2 preview, the package is the same but the signing certificate differs. **Android will not accept this APK as an in-place update over that copy. Do not uninstall the existing app just to try it; uninstalling can erase its private app data.** A same-key owner-signed build is required for a safe in-place update. This preview is suitable for a clean test device or a device where no app with this package is installed.
- The optional `RAPKP-Termux-AI-26.3.4-unsigned.aab` is an unsigned release bundle; it is not an installable APK and is not owner-signed.

## First-use routes

### Cloud first (when you already have a deployment)

1. In **Cloud, Termux & AI setup**, enter the real HTTPS base URL for a deployed RAPKP API and its token if required.
2. On Android, pair Termux and explicitly enable the fallback checkbox only if you want the same inputs sent to that local service after a network/server failure. Cloud credentials are never forwarded to Termux.
3. Use **Test status** to check an actual response. A saved URL or sent start intent is not shown as connected without a reply.

No hosted service URL, cloud account, DNS name, or server credentials were configured in this build. A public cloud connection has not been tested. The separate cloud deployer must provide its own correctly licensed, verified kernel/data and production controls.

### Termux-only / local fallback

1. Install the official Termux app from a supported source. In the app's connection screen, grant Android's **Run commands in Termux** permission when prompted.
2. Termux requires a user-controlled external-app security setting. The RAPKP app cannot change Termux's private setting; use the in-app guidance to open Termux and review the official permission instructions. **You do not need to type shell commands or manually start the connector.** The RAPKP setup buttons perform the fixed install/re-pair and start requests after permission is granted.
3. The connector installs pinned Python packages into Termux and verifies a local DE440s kernel. The kernel is **not included** in the APK or source bundle. If it is missing, the app asks for explicit one-time consent before downloading about 31.2 MiB directly from JPL; setup checks its pinned size and SHA-256. The file is kept in Termux's private app home.
4. Tap **Install / re-pair** or **Start & connect** in the app. It separately reports the service response, calculation-engine readiness, and AI configuration. Termux:Boot is optional; Android/OEM background restrictions can still stop services.

DE440s is not redistributed by this build. The JPL documentation reviewed does not state a separate file-level redistribution license for the binary. This project makes no claim of commercial redistribution or mirroring rights; obtain authoritative licensing guidance before repackaging it. See `termux-connector/LICENSING.md` and `THIRD-PARTY-NOTICES.md`.

## AI and Gemini Gem

- To use automated AI, switch to direct **Termux-only** mode, configure an OpenAI or Gemini API provider/key in Termux, and affirm the separate AI consent. API charges may apply. Keys stay in Termux's private configuration and are not included in the APK or source bundle.
- Cloud AI jobs are not implemented by this build. A cloud calculation remains complete even when AI is unavailable; no model response is fabricated.
- To use a consumer Gemini Gem, calculate a report, then tap **Copy for Gemini Gem** / **Share Gem handoff** or export JSON and upload it yourself. Review birth details before sharing. The Gem instructions are in `integrations/gemini/GEM_INSTRUCTIONS.md` and are available from the app's integrations area. A static Gem prompt/PDF does not grant authenticated API execution.
- Deterministic calculations produce astronomical/astrological facts. AI may interpret those facts only; it must not invent positions, houses, dates, dashas, transits, or a connection state.

## Method and limits

- The source outline currently accounts for **10 of 21 rule slots as defined** and **0 calibrated outcome rules**. The supplied 10-step structural outline lists fewer steps than its headings claim; the six unlisted definitions and 11 user rules remain unavailable and are not invented.
- The four-check support score is a transparent rule-support measure, **not** an event probability, confidence level, accuracy rate, or guarantee. Binary event judgment remains **NOT DETERMINED**. Horary number lookup is a table mapping, not a horary judgment.
- Numerical comparison fixtures are finite reference checks, not universal error bounds. No personal-event prediction accuracy is established. Medical, legal, financial, and other consequential decisions require real-world professional evidence.
- Supported civil birth/evaluation times require explicit IANA timezones; ambiguous DST folds require a choice, nonexistent local times and unsupported ranges are rejected. The Placidus implementation's supported latitude is strictly inside ±66°.

## Rebuilt evidence (2026-10-06)

- Frontend TypeScript/Vite production build: **passed** (1,754 modules; Node 22.23.3).
- Python: **967 passed, 2 skipped**. The two skipped tests require a local Valkey/Redis server binary. One Starlette/httpx deprecation warning was reported. Ruff checks passed.
- Browser/client tests: **30 Playwright tests passed** across desktop and mobile viewports; **35 storage/request-contract tests passed**; **20 native transport contract tests passed**.
- Android: debug APK and unsigned release AAB built; **7 JVM crypto tests passed**; Android lint **0 errors / 0 warnings**. Gradle printed non-blocking toolchain/`flatDir` and SDK XML-version warnings.
- `npm audit`: **0 known vulnerabilities** after updating the lockfile's vulnerable `source-map-js` transitive dependency. This is a dated scan, not a guarantee of future safety.
- APK content inspection: signature verified; package/version/build matched; web assets matched; Termux plugin and permission present; cleartext restricted to loopback; connector-manifest hash matched; no `.bsp` kernel or provider-key marker found in the APK/connector.
- **Not tested:** physical Android device/emulator, actual Termux RUN_COMMAND/Boot/Keystore behavior, live Termux service, a real hosted cloud connection, or a paid OpenAI/Gemini model call. Mocked and local automated tests do not establish those behaviors.

Machine-readable verification and APK metadata are in `qa/android/26.3.4/`. The APK SHA-256 is `4e93ba48f1e10141764b9dae85a4f2baffed91702ffde4430cad5850782543fb`.

## Security, privacy, licensing

Cloud endpoints must use HTTPS. The Android cleartext exception is limited to the paired loopback service. Termux is a permission-controlled local service; RAPKP exposes fixed setup/start actions, not arbitrary remote shell execution. No API key or release-signing private key is embedded. A debug certificate is used for this preview; configure an owner-controlled persistent keystore for future release signing.

The Termux service/bootstrap has AGPL-3.0-only terms in this build; shared source retains its MIT notice. Swiss Ephemeris/pyswisseph and libephemeris are not dependencies or bundled. DE440s is obtained directly by the user only after consent and is not redistributed by the APK. Review all notices and exact artifact terms before commercial/app-store distribution.
