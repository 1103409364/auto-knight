# KDE Plasma automatic night theme switcher

auto-knight is a shell script that automatically switches between light and dark themes in KDE Plasma. It uses the color temperature settings from KDE's Night Color feature to determine when to switch themes.

It is intended to be lightweight.

## How it works

* `auto-knight` is the daemon started at login. It applies the matching theme once at startup and then listens for changes of KDE's Night Color daylight status via `dbus-monitor`, re-applying the theme whenever day turns into night or vice versa.
* `toggle-knight` performs the actual switch. Depending on the target mode it updates:
  * the Plasma global theme (`plasma-apply-lookandfeel`)
  * the Kvantum widget theme (`kvantummanager --set`)
  * the mouse cursor theme (`plasma-apply-cursortheme`)
  * the default Konsole profile (`kwriteconfig6`, affects newly opened windows)
  * the screen brightness (`qdbus`)

  Note: the GTK theme is applied automatically as part of the global theme package, so it is not overridden here. Wallpaper and lockscreen settings are prepared but disabled by default.

  It can also be run standalone to manually toggle between light and dark modes.

## Dependencies

* qdbus: A tool for sending and reading DBus messages and values. This is used to get the current daylight status from KDE's Night Color feature and to set the screen brightness.
* dbus-monitor: A tool for monitoring DBus messages. This is used to detect changes in the daylight status.
* grep: A tool for filtering text. This is used to filter the output of dbus-monitor for messages about the target color temperature.
* plasma-apply-lookandfeel and plasma-apply-cursortheme: KDE tools for applying the global theme and cursor theme.
* kreadconfig6 / kwriteconfig6: KDE config tools, used to read the current theme and set the Konsole profile.
* kvantummanager: Part of the [Kvantum](https://github.com/tsujan/Kvantum) Qt style engine, used to switch the Kvantum theme.

## Installation

To use Auto-Knight, you need to have the above dependencies installed. You can then download the scripts, make them executable and add `auto-knight` as a login script via KDE autostart (see `auto-knight.desktop`). Optionally install `toggle-knight.desktop` to toggle the theme manually from the application launcher.

## Customization

You can customize the light and dark settings by modifying the variables at the top of the `toggle-knight` script (global themes, Kvantum themes, cursor themes, Konsole profiles and brightness values).
