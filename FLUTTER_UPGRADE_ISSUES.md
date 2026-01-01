# Flutter SDK Compatibility Issues

This document tracks issues encountered when building with Flutter 3.27+ (bleeding-edge SDK).

## Completed Fixes

| Package | Old Version | New Version | Notes |
|---------|-------------|-------------|-------|
| `flutter_isolate` | git: chipweinberger/flutter_isolate | git: rmawatson/flutter_isolate | Original repo no longer exists |
| `flex_color_scheme` | ^7.3.1 | ^8.0.0 | Theme type renames (AppBarTheme → AppBarThemeData, etc.) |
| `flutter_slidable` | ^3.1.0 | ^4.0.0 | Deprecated `hashValues` removed |
| `skeletonizer` | ^1.1.1 | ^2.0.0 | New Canvas methods (`clipRSuperellipse`, `drawRSuperellipse`) |
| `audio_waveforms` | ^1.0.5 | ^2.0.0 | API breaking changes (see below) |

### audio_waveforms API Changes

Files modified to remove deprecated parameters:
- `lib/app/layouts/conversation_view/widgets/text_field/text_field_suffix.dart` - Removed `sampleRate` and `bitRate` from `record()`
- `lib/app/layouts/settings/pages/message_view/conversation_panel.dart` - Removed `finishMode` from `startPlayer()` (2 occurrences)
- `lib/app/layouts/conversation_view/widgets/message/attachment/audio_player.dart` - Removed `finishMode` from `startPlayer()`

### Other Fixes
- Created empty `.env` file (required by assets declaration in pubspec.yaml)

## Blocking Issues

### 1. disable_battery_optimization (BlueBubbles Fork)

**Error:**
```
DisableBatteryOptimizationPlugin.java:56: error: cannot find symbol
    public static void registerWith(PluginRegistry.Registrar registrar)
```

**Cause:** Uses deprecated Flutter v1 embedding `Registrar` API, removed in Flutter 3.27+.

**Fix Required:** Fork needs to be updated to use Flutter v2 embedding (`FlutterPlugin` interface).

**Location:** `https://github.com/BlueBubblesApp/Disable-Battery-Optimizations.git`

### 2. Potential Similar Issues

These BlueBubbles forks may have the same `Registrar` issue:
- `gesture_x_detector` (xgesture_flutter fork)
- `secure_application` fork
- `store_checker` fork
- `video_thumbnail` fork
- `maps_launcher` fork
- `local_notifier` fork
- `desktop_webview_auth` fork

## Warnings (Non-blocking)

### Android Gradle Plugin
```
Warning: Flutter support for your project's Android Gradle Plugin version (8.3.1) will soon be dropped.
Please upgrade to at least 8.6.0.
```

### Kotlin Version
```
Warning: Flutter support for your project's Kotlin version (1.9.23) will soon be dropped.
Please upgrade to at least 2.1.0.
```

### open_filex Plugin
```
Package open_filex references dartPluginClass as the default plugin, but the package does not exist.
```
This is a warning only and doesn't block the Android build.

## Next Steps

1. Fork `BlueBubblesApp/Disable-Battery-Optimizations`
2. Update Java code to use `FlutterPlugin` interface instead of `Registrar`
3. Update pubspec.yaml to point to new fork
4. Repeat for any other forks with the same issue
5. Consider upgrading AGP to 8.6.0 and Kotlin to 2.1.0
