# AGENTS.md

Personal KDE Plasma light/dark theme auto-switcher. Two standalone bash scripts, no build, no tests, no dependencies beyond KDE tools. This is a single-user personal project — treat it as such (no packaging, CI, or multi-platform concerns).

## Structure & flow

- `auto-knight` — daemon. Started at login via `auto-knight.desktop`. Applies the matching theme once at startup, then watches KDE Night Color' daylight status through `dbus-monitor` and re-applies on every change.
- `toggle-knight` — does the actual switching. Called by `auto-knight` and usable standalone to toggle manually. Decides mode via `is_dark_theme()` (reads current `LookAndFeelPackage`), NOT the daylight status.

`auto-knight` invokes `toggle-knight` as `"$(dirname "$0")/toggle-knight"`, so **both must stay in the same directory**.

## Customization lives in the script, not config

All themes/profiles/brightness are hardcoded variables at the top of `toggle-knight` (see README "Customization"). There is no config file. To change behavior, edit those vars.

- `LIGHT_THEME`/`DARK_THEME` are **duplicated in both `auto-knight` and `toggle-knight`** — keep them in sync; `auto-knight` uses its copies for `should_update_theme()`.
- Several settings are deliberately **disabled by default** (commented `run_step` calls in `set_theme`): GTK, wallpaper, and lockscreen wallpaper. Leave them disabled unless asked; the look-and-feel package already applies its bundled GTK theme.

## Gotchas (verified)

- Shell is **bash**, not POSIX sh (uses arrays, `local -n` namerefs, `${var%.profile}`).
- Live-switching running Konsole sessions requires `konsolerc: [KonsoleWindow] EnableSecuritySensitiveDBusAPI=true`; otherwise only new windows switch.
- `restart_kde()` exists but is commented out from `set_theme`.
- No linter/tests/typecheck — verify by running the scripts (`toggle-knight` toggles your real desktop theme; be cautious).
