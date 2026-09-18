# SideQuest — mobile app

Not scaffolded yet. Flutter needs to run natively on the host (emulator/simulator
and USB device access don't work well inside Docker), so once the Flutter SDK
is installed locally, generate the app from this directory with:

```bash
flutter create --org de.sakurasi --project-name sidequest .
```

This will produce the standard Flutter project layout (`lib/`, `android/`,
`ios/`, `pubspec.yaml`, its own `.gitignore`, etc.) in place, pointed at the
`de.sakurasi.sidequest` bundle ID / applicationId agreed on for this project.
