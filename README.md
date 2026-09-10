# iPA_testing – Vorlage zum Erstellen von iOS-IPAs

Dieses Repo ist eine **Vorlage**, um aus einem Flutter-Projekt per GitHub Actions eine
unsignierte `.ipa` zu bauen, ohne eigenen Mac. Der Build läuft auf einem macOS-Runner von GitHub.

## Verwendung

1. Repo als Vorlage kopieren (oder Workflow-Datei in ein bestehendes Flutter-Projekt legen).
2. Flutter-Projekt ins Repo (mit `ios/`-Ordner und `pubspec.yaml`).
3. Auf GitHub unter **Actions → iOS-ipa-build → Run workflow** starten.
4. Die fertige `FlutterIpaExport.ipa` hängt danach am Release `v1.0`.

## Was der Workflow macht

`.github/workflows/dart.yml`:

- Flutter (stable) installieren
- `flutter pub get`, `pod repo update`
- `flutter build ios --release --no-codesign`
- `Runner.app` in `Payload/` packen und als `.ipa` zippen
- IPA an das Release `v1.0` hängen (überschreibt den vorherigen Build)

## IPA aufs iPhone bringen

Die IPA ist **unsigniert**. Sie lässt sich nicht durch Antippen installieren (auch nicht per
Telegram/AirDrop), sondern nur per Sideloading:

- **Sideloadly** (Windows/Mac, iPhone per USB) oder **AltStore** – signiert mit deiner Apple-ID,
  7 Tage gültig, danach neu signieren
- **TrollStore** – falls auf dem Gerät vorhanden, dauerhaft
- **Xcode** auf einem Mac – Projekt öffnen, mit eigener Apple-ID signieren, direkt aufs iPhone

## Hinweise

- Für andere Frameworks (Capacitor, React Native, Swift) muss der Build-Schritt angepasst werden.
- Für **appetize.io** wird kein IPA gebraucht, sondern ein Simulator-Build
  (`xcodebuild -sdk iphonesimulator`, `.app` als ZIP).
