# GDC Companion App Repository

Active repository version: `v1.59.6`

This repository is now organized around the active modular companion app and the native ATS telemetry plugin, while preserving the historical one-file app snapshots and older bundles in archive areas instead of leaving them mixed into the root.

## Active areas

- `src/gdc_companion_app/` — active Python companion app package
- `main.py` — source checkout launcher that preserves the familiar root launch path
- `native/plugin/` — active native telemetry plugin project
- `data/jurisdiction/` — jurisdiction atlas data used by resolver code
- `docs/` — current architecture, protocol, compliance, and validation notes
- `examples/protocol/` — protocol message examples
- `scripts/` — build, install, and validation helpers
- `archive/` — preserved historical snapshots, bundles, and experiments

## Launch the companion app from source

```powershell
python main.py
```

Or install it as a package and run:

```powershell
python -m gdc_companion_app
```

## Build the native plugin

Canonical scripts live under `scripts/plugin/`.

Compatibility wrappers remain at the repository root:

- `build_and_install_plugin.cmd`
- `build_and_install_plugin.ps1`
- `build_and_install_plugin_updated.cmd`
- `build_and_install_plugin_updated.ps1`

## Stability priorities preserved in this rework

- keep the current modular Python app as the active baseline
- keep the resolver behavior tied to truck world coordinates and strong live atlas sources
- avoid silently deleting legacy material; move it into archive areas instead
- reduce repository fragility by separating active code, plugin code, docs, data, scripts, and historical artifacts

See also:

- `VERSIONING.md`
- `docs/architecture/repository_layout.md`
- `docs/architecture/application_modules.md`
- `docs/architecture/migration_plan.md`
- `docs/validation/manual_regression_checklist.md`
