# Changelog

## Unreleased

## v1.59.6.5 Closed Beta
- Changed quick-job truck ownership prompt suppression from a timer-based cooldown to a movement-based latch.
- Quick-job and quick-drive trucks now remain suppressed until stable out-of-job movement is detected, preventing stale quick-job trucks from prompting as owned trucks later.
- Added diagnostics logging around quick-job truck prompt suppression and clearing.
- Fixed quick-job completion no longer applying the automatic HOS-only reset after the quick-job toggle was hidden.
- Fixed Convoy quick-job completion handling so gameplay events use the same multiplayer server game-time source as telemetry snapshots.
- Prevented quick-job HOS reset markers from mixing local raw game time with Convoy server time, which could create massive fake rest gaps after job completion.
- Fixed Convoy HOS handling when multiplayer server time becomes available after initially reporting offset 0.
- The app now treats the local-to-server Convoy clock jump as a time-source transition and excludes that artificial offset from HOS math, preventing massive fake rest or duty gaps.


## v1.59.6.4 Closed Beta
- Fixed Settings > ATS Plugin install/repair failing to find the bundled `gdc_eld_plugin.dll` after installation by keeping a copy in the app install folder and searching the installed app root.

## v1.59.6.3 Closed Beta
- Simplified DVIR checklist reporting by removing the per-item severity picker, using the note fields for defect details, and relying on the Vehicle safe to operate toggle for drive-blocking defects.
- Hardened quick-job and Convoy truck tracking so temporary quick-job trucks stop triggering owned-truck baseline prompts, while real no-job driving in the same truck can still clear the suppression and allow normal prompting.
- Added a persistent in-screen Settings gear on the home/tablet shell so Settings stays one tap away even when the header is visually out of the way.
- Unified odometer, fuel, and gameplay diagnostics under the app diagnostics logger instead of scattering separate debug sidecar files.
- Quieted routine odometer diagnostics by rolling ordinary accepted samples into periodic summary entries while still logging rejects, source changes, and other important acceptance events individually.
- Added richer diagnostics logging around Convoy mode toggles, ATS plugin install/repair and rebuild flows, and diagnostic bundle export requests/completions.
- Expanded diagnostic bundles to include a consistent SQLite database backup alongside the existing runtime snapshots, summaries, settings, and app logs.
- Fixed updater packaging so release builds no longer ship `PUT_RELEASE_URL_HERE` placeholder manifest URLs.
- Added a hosted GitHub release URL fallback for updater config generation and placeholder recovery.
- Improved updater errors when a published update manifest still contains placeholder installer metadata.
- Added Cloudflare R2 release publishing scripts for updater manifests, installers, and SHA256 files.
- Added a one-command build-and-publish flow for R2-backed closed beta releases.
- Added publish-time manifest URL, installer size, SHA256, and public URL checks before completing an R2 release.
- Changed release installer output to use a stable filename: `GDC_Companion_App_Setup.exe`.
- Updated generated update manifests to point at the stable installer filename while keeping version checks in manifest metadata.
- Wrote generated updater JSON files as UTF-8 without BOM to avoid manifest parsing/display gremlins.
- Fixed update checks incorrectly reporting an available update when the installed app and manifest had the same numeric version but different release suffixes such as `-public`.
- Added updater version comparison tests to keep release/channel labels from affecting update ordering.
- Removed release-channel suffixes such as `-public` from generated app and update manifest versions.
- Kept updater release channel tracking in the manifest `channel` field instead of mixing it into the semantic version.
- Updated installer staging metadata to record the selected update channel instead of hardcoding `public`.

## v1.59.6.2 Closed Beta
- Fixed quick-job completion resets so they reset drive, shift, break, cycle, and recap displays even when completion telemetry arrives after the job market clears.
- Hardened Nuitka and installer packaging so default HOS warning sounds, app icons, and jurisdiction data are validated, copied into the runtime payload when missing, and resolved correctly from bundled resource roots.
- Updated GitHub release publishing to generate release notes from the changelog, update notes on existing releases, accept additional assets, and find stable installer filenames when versioned setup names are not present.
- Added release-channel support to the GitHub publisher so public, closed-beta, and internal releases get normalized channel tags, channel-aware titles/notes, prerelease handling, and tag-specific download URLs.

## v1.59.6.1 Closed Beta
- Fixed quick-job HOS handling so quick jobs keep live motion/status updates and recap math moving while still excluding quick-job truck, fuel, and IFTA career stats
- Preserved quick-job job context when telemetry reports the job market as quick-job even if `job_active` temporarily reports false
- Hardened Stats / Expenses mileage calculations to ignore quick-job stats rows, use current odometer/baseline context for tracked miles, and keep profile lifetime truck rows from being skewed by poisoned odometer samples
- Added a Stats / Expenses Repair Mileage action that clears clearly bad odometer samples while preserving income, expenses, fuel, fines, repairs, and incident records
- Added an in-app Check for Updates button in Settings backed by a standalone updater bootstrapper that checks a hosted manifest, verifies installer SHA256, and launches the installer
- Updated the full release build to produce the main Nuitka runtime, updater bootstrapper, installer payload, installer, hash file, and update manifest in one flow
- Updated installer staging and Inno packaging to include the updater payload, updater config, app icon, update shortcut, AppUserModelID metadata, and hosted update-manifest URLs
- Added installer icon assets and release packaging improvements so staged payloads copy the app icons and updater assets into the final installer layout
- Updated release build documentation for the hosted updater manifest, updater install location, and full tester-ready build flow

## v1.59.6 Closed Beta
- Skipped `v1.59.5`; this release carries the planned `v1.59.5` closed-beta changes plus the follow-up Convoy, trip-history, and quick-job hardening work
- Added explicit Convoy 10-hour rest actions for both Off Duty and Sleeper Berth flows, keeping rest markers separate from normal manual duty changes
- Stabilized Convoy HOS timing against server-time offsets, stale raw game-time samples, placeholder zero times, observed time gaps, and legacy quarantine freezes
- Removed temporary Convoy/HOS debug log writers and ELD debug fields after validating the timing fixes
- Improved Convoy quick-job HOS handling with lifecycle cleanup, quick-job exclusion controls, automatic driving-status correction, and prompt suppression for temporary quick-job trucks
- Fixed team-driving and co-driver HOS progression edge cases, including startup placeholder resets, resting defaults, and local co-driver clock progression
- Fixed Trips / Jobs split duplicate handling and manual merge summaries so merged trip stats, visible markers, and friendly load labels remain consistent
- Added trailer display and facility metadata support while removing trailer plate display from the dashboard
- Fixed jurisdiction overlay active-path handling so the overlay follows the selected route correctly
- Changed career-owned records so DVIRs, diagnostics, day mileage, fuel stats, trips, truck tracking, and stats views follow the active ATS/ETS career profile instead of splitting across driver changes
- Added two-driver career assignment handling for solo/team profiles, including first-run local driver creation, active driver selection, and safer team-driver assignment sync
- Restored separate driver HOS clocks while team driving is enabled, while keeping normal solo HOS scoped to the career profile
- Added HOS gap quarantine handling so non-rest save/reload or offline gaps do not inflate shift/cycle usage, while trusted rest gaps can still complete long breaks and resets
- Updated IFTA mileage and settlement data to use profile/fleet scope across driver and truck changes, including migration and aggregation of legacy driver/truck-scoped rows
- Hardened active-truck fuel summaries with stable lifetime MPG guards and career/truck-owned fuel stat recovery across driver changes
- Improved Fuel / IFTA event editing so price, gallons, jurisdiction, and odometer edits persist, refresh the selected row, and resync stats for the event's actual truck
- Changed recent and quarter fuel event lists to sort newest in-game day/time first, with timestamp and event-id fallback ordering
- Hardened global hotkeys on Windows and Linux with focused-window fallback, listener restart/reconnect support, richer registration diagnostics, and safer manual duty hotkey routing
- Changed IFTA distance and settlement reporting to use a profile/driver fleet ledger while preserving each fuel receipt's historical truck identity for audit display
- Added in-game day/time labels for fuel events and fuel price prompts, plus a Truck column in the recent fuel events table
- Added migration handling that merges legacy per-truck IFTA distance rows into fleet-level jurisdiction totals
- Added a Nuitka public release build script that packages app resources and copies the prebuilt `gdc_eld_plugin.dll` into the release output
- Hardened the Nuitka release script so it detects a usable Python interpreter or honors `GDC_PYTHON` instead of assuming `python` resolves correctly
- Added installer payload staging scripts and documentation for public Windows installer packaging
- Added public/dev build-channel gating so developer UI only appears in internal builds or when explicitly enabled
- Added shared resource-root helpers so icons, warning sounds, jurisdiction data, and plugin patch sources resolve correctly from source, PyInstaller, and Nuitka builds
- Refactored raw database row handling behind storage-layer records for fuel/IFTA, jurisdiction samples, logbook, trips, diagnostics, profiles, and telemetry flows
- Added normalized runtime snapshot domain models for driver, truck, fuel, jurisdiction, HOS, trip, and telemetry UI rendering
- Routed dashboard, runtime diagnostics, Fuel / IFTA, and concept-shell connection displays through normalized runtime snapshots instead of direct mutable runtime-state reads
- Added a Settings diagnostic bundle export that packages app logs, settings, runtime snapshots, and recent telemetry/domain records without copying the full SQLite database
- Added lightweight runtime-event storage used by diagnostic exports for recent app/runtime troubleshooting context
- Added market-aware Stats / Expenses income charting using plugin job-market, cargo ID, and cargo-name telemetry fields
- Changed the hidden Raven extras unlock codes to manufacturer-themed codes
- Changed truck ownership/naming baselines to be scoped to the ATS/ETS career profile instead of the active driver, with migration for existing driver-scoped rows
- Updated PyInstaller packaging to support public/closed-beta build labels, hide the console for public builds, and limit public data files to shipped runtime assets
- Bumped the app version from `v1.59.4` to `v1.59.6`

## v1.59.5 Closed Beta
- Skipped; superseded by `v1.59.6`

## v1.59.4 Closed Beta
- Renamed the active app class and runtime identity to `GDCCompanionApp` while preserving legacy `FunctionalConceptFourteenApp` compatibility imports
- Added Trips / Jobs history storage and UI support, including trip/job summaries, navigation wiring, and Screens menu access from compact/tablet layouts
- Fixed Trips / Jobs navigation so the tab remains reachable from the left Screens rail, header menu, and bottom Screens menu variants
- Added app branding/icon support with bundled default and secret Raven icons, Windows AppUserModelID setup, Tk window icon application, and PyInstaller executable icon wiring
- Added hidden App icon settings that only appear after the secret Raven icon theme is unlocked
- Generalized hidden unlock codes into reusable special-code definitions while keeping the existing corvid/Raven codes working for sounds and icon unlocks
- Locked Dev Tools behind the `RAVENWRENCH` developer special code and hid the tab, menu entry, and home DEV overlay shortcut until unlocked
- Removed the native plugin INI sidecar dependency so the plugin runs with built-in defaults and installers clean up legacy `gdc_eld_plugin_config.ini` files
- Fixed between-job fuel inventory blending so price-unknown refuels use the current tank average provisionally and later ledger edits/deletes adjust inventory gallons and value
- Added a hidden backend mod compatibility scanner that can read active mods from `profile.sii` / `game.log.txt` and evaluate advisory rules
- Added an ATS Plugin settings panel for detecting the ATS install, checking plugin status, and installing or repairing the bundled `gdc_eld_plugin.dll`
- Bumped the app version from `v1.59.3` to `v1.59.4`

## v1.59.3 Closed Beta
- Added a Stats / Expenses tab with per-truck and all-truck scopes, income/expense/net-profit summaries, per-mile metrics, driver score, charts, filters, and recent event review
- Added the `stats_events` and `truck_tracking_baselines` database tables for per-driver/per-truck financial, incident, and mileage tracking
- Added manual stats event entry, edit, and delete support for income, expenses, repairs, tolls, fines, incidents, and adjustments
- Added truck ownership odometer baseline prompts plus active/stored/sold/archived truck tracking so per-mile stats can follow a truck across sessions
- Mirrored finalized refuel ledger entries into stats events and added fuel price prompts so fuel purchases can contribute to expense and profit calculations
- Added native plugin gameplay-event telemetry for fines, tolls, ferry/train fees, job cancellation penalties, job delivery income, and cargo damage, with debug logging for raw gameplay events
- Added native plugin truck/trailer wear telemetry and app-side damage delta detection for inferred repairs and possible accidents, including repair-cost prompts
- Added rotating application logs under the user Documents app folder, Tk/thread exception logging, and Open Logs Folder buttons in runtime/dev tools
- Made stopped-command global hotkeys configurable in Settings with validation, duplicate checks, Ctrl/Shift modifiers, save/reset controls, and registration diagnostics
- Updated plugin telemetry payloads to include `truck_odometer_km`, odometer validity, nested truck damage fields, and trailer damage fields
- Added a small `check_stats_events.py` diagnostic helper for inspecting the local stats table
- Bumped the app version from `v1.59.2` to `v1.59.3`
- Added a dedicated Stats / Expenses Profile Lifetime view that summarizes lifetime income, expenses, net profit, tracked trucks, events, incidents, and per-truck comparison rows across every truck for the active ATS profile and driver
- Added an In-Cab Help dialog and wired the Home User Manual tile to open the read-only driver quick-reference workflow
- Split large UI code into `gdc_companion_app.ui` modules while keeping the app imports working through the new UI package structure
- Split HOS, jurisdiction, and team-driving domain logic into `gdc_companion_app.domain`, then moved overlay and stats tracking into `gdc_companion_app.telemetry`
- Split the legacy `common.py` surface into focused config, protocol, service, storage, and utility modules with explicit imports instead of wildcard/common-barrel dependencies
- Split the monolithic app core into `gdc_companion_app.app` mixins and moved ELD service / overlay window logic into focused telemetry modules while preserving compatibility imports
- Split concept UI layers into named modules such as `DeviceShellApp`, `DashboardScrollLayoutApp`, `HomeDashboardApp`, and `DropdownStatusFlowApp` while retaining legacy `FunctionalConcept*` aliases
- Added team-driving configuration storage, active/co-driver selection, driver swap planning, service-level swap event writing, and a Co-Driver / Team Driving dialog from the Home tile
- Added DVIR tables, domain helpers, roadside export summary data, and a manual DVIR dialog for signed inspection reports, defect tracking, repair certification, and previous-driver review
- Added Volvo VNR Electric truck identity display names and legacy ID aliases
- Changed IFTA tracking to automatically use the current calendar quarter and show the active quarter end date in the Fuel / IFTA UI
- Added a Home quick-job reset toggle that can reset daily HOS limits and the 70-hour cycle after game-handled job completion
- Removed the Overview `Driver Score` and `Worst Cost` cards, leaving the fine-rate metric in the lower overview row
- Added profile-name display support to the runtime/header surfaces and kept the packaged runtime mirror aligned
- Added Settings support for renaming profile/driver display names while preserving telemetry identity links
- Reworked Settings layout for scrollable compact windows, three-column hotkey controls, and side-by-side Identity, Drivers & Teams, Appearance, and Overlay sections
- Improved compact dashboard responsiveness with smaller minimum window sizing, half-width resize behavior, 2x2 HOS dial wrapping, tighter dial/card scaling, and narrow-width identity stacking
- Replaced overflowing notebook tab navigation with a hidden-tab content area and left-side Screens rail, including roadside lock-state disabling for non-roadside screens
- Added a bottom-bar Screens menu path for concept tablet/home layouts so screen navigation remains reachable when the header is cramped
- Added native plugin companion backoff handling so optional companion telemetry failures are retried less aggressively
- Added bundled HOS warning sound packs, per-threshold warning sound selectors, custom WAV support, corvid unlock codes, test buttons, and WAV volume control in Settings
- Moved HOS warning sound selection into a compact dedicated Settings panel while keeping ELD tab warning-sound controls focused on enable/test shortcuts
- Changed settings persistence from LocalAppData JSON to user-visible `settings.xml` under the app Documents folder, with one-time migration from legacy `settings.json`
- Updated direct settings lookups for ATS/profile discovery so they can read the new XML settings file while still falling back to legacy JSON

## v1.59.2 Closed Beta
- Moved stopped-command global hotkeys from F5-F12 to Num1-Num8 and updated the in-app/overlay hotkey hints to match
- Added Windows hotkey registration diagnostics, failed-hotkey reporting, and a Tk window hook so hotkey actions can be queued from `WM_HOTKEY` messages
- Guarded live HOS calculations against ATS crash/save reload rollbacks by ignoring future duty points, clamping future status starts, and rejecting implausible game-time jumps
- Added a live HOS history sanitization window with a synthetic boundary point so old or poisoned duty history cannot create negative status time or absurd cycle-used values
- Capped impossible cycle-used display values at the 70-hour limit so bad history reports as a violation instead of leaking multi-thousand-hour totals into the UI
- Added fuel telemetry debug breadcrumbs in the app documents `debug/fuel_debug.jsonl` file for fuel deltas, refuel signals, pending-refuel finalization, and fallback refuel logging
- Captured raw refuel amount telemetry and pending refuel state in debug breadcrumbs to help diagnose missed, duplicate, or suspicious fuel ledger events
- Reworked plugin and fallback refuel detection to accumulate a pending refuel window from min/max tank levels before writing one de-duped fuel stats and ledger entry
- Stored the refuel detection source plus start/max/end tank levels in finalized fuel ledger metadata to make disputed fuel events easier to audit

## v1.59.1 Closed Beta

- Removed the obsolete duplicate `GDC_Companion_App.spec` so `main.spec` is the single active PyInstaller build spec
- Updated `main.spec` to name packaged executables with the app version and `closed-beta` build label from `src/gdc_companion_app/version.py`
- Prioritized plugin/socket telemetry over the RenCloud shared-memory fallback whenever recent plugin snapshots are available
- Corrected RenCloud fuel shared-memory reads for fuel amount, range, and warning state, while avoiding bogus odometer samples from that feed
- Added plausibility checks for incoming odometer and fuel telemetry so bad samples cannot create false fuel, IFTA, or ledger deltas
- Prevented duplicate fallback refuel events from being logged at the same odometer and gallon amount
- Preserved live accumulated fuel-consumption totals when syncing refuel ledger totals, keeping lifetime MPG from being overwritten by purchase-ledger data
- Removed the leftover `RAW WORLD` debug print block from telemetry snapshot handling
- Fixed a day certification upsert mismatch in `common.py` by correcting the SQL placeholder count for `day_certifications`
- Removed a duplicate `open_manual_status_entry()` method definition from `app_core.py`
- Hardened fuel and IFTA editor focus checks so Tk focus lookup failures no longer raise `KeyError` or `tk.TclError`
- Added a guarded runtime refresh helper so UI refresh callbacks reuse the existing runtime-state apply path without crashing the Tk event loop
- Fixed the `app_core.py` runtime refresh helper definition so the app starts cleanly again, correcting the method indentation and matching the current `_apply_runtime_state()` signature
- Updated `main.spec` so PyInstaller explicitly bundles the `gdc_companion_app` modules plus the native plugin patch/source/include folders and `data` assets required by the packaged app
- Rebuilt the PyInstaller executable successfully from the updated `main.spec`, producing a fresh packaged app in `dist/main.exe`
- Updated `common.py` and `jurisdiction.py` to resolve bundled resources more reliably in PyInstaller builds, including `_MEIPASS`/executable search paths and broader native plugin source lookup for packaged runs

## v1.59.0 Closed Beta

- Adopted the new repository versioning format: `game_version.mod_version`
- Promoted the modular Python companion app as the active codebase under `src/gdc_companion_app`
- Reworked the repository into clear top-level areas for the active app, native plugin, data, docs, examples, scripts, tools, and archives
- Removed generated build output and cache directories from the active repository layout
- Preserved historical single-file app snapshots and older bundles under `archive/`
- Kept root-level compatibility launch/build wrappers so existing workflows are less likely to break during the cleanup
- Centralized the active app version label so legacy copied constants do not override the new `v1.59.0` label
- Added repository architecture notes, migration notes, and a manual regression checklist focused on the known-good behavior list
