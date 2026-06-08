---
author: volles-kaennchen
created: 08-06-2026 20:56:25
updated: 08-06-2026 20:56:25
topic: App-Baukasten
tags:
  - android
  - apk
  - kalenderapp
  - capacitor
---
# 📦 Von der HTML-Datei zur Android APK

> [!abstract] Einleitung 
> Diese Anleitung beschreibt den vollständigen Weg, um eine fertige HTML/JS-App mithilfe von Capacitor in eine installierbare Android-APK zu verwandeln. Voraussetzung ist eine fertige `index.html` im `dist/`-Ordner des Projekts.

---

## 🛠️ Komponenten-Übersicht

### 📌 Was wird gebraucht und warum?

|Software|Rolle|
|---|---|
|**Node.js**|Ermöglicht JavaScript außerhalb des Browsers. Bringt `npm` mit – den Paketmanager zum Installieren von Capacitor.|
|**npm**|Paketmanager von Node.js. Funktioniert wie ein App-Store für Entwickler-Werkzeuge.|
|**Capacitor**|Das Bindeglied. Nimmt die HTML/CSS/JS-Dateien und verpackt sie in ein standardisiertes Android-Projekt. Baut selbst keine APK.|
|**JDK 17**|Java Development Kit. Wird von Android Studio und Gradle benötigt, um das Projekt zu kompilieren.|
|**Android Studio**|Die eigentliche „Fabrik". Besitzt die offiziellen Google-Werkzeuge (Compiler + Android SDK), die aus dem Capacitor-Projekt eine echte, native APK bauen.|
|**VS Code**|Editor und Terminal-Zentrale für alle npm/npx-Befehle während des Projekts.|

> [!tip] Merkhilfe 
> Capacitor verpackt – Android Studio backt. Ohne Android Studio gibt es keine fertige APK-Datei.

---

## 🛠️ Schritt 0 – Voraussetzungen installieren

### 💎 Node.js

Lade Node.js (LTS) von [nodejs.org](https://nodejs.org/) herunter und installiere es.

### 💎 Terminal – Prüfen ob Node.js korrekt installiert ist

```bash
node -v
npm -v
```

### 💎 JDK 17

Lade Eclipse Temurin 17 (Windows x64 Installer) von [adoptium.net](https://adoptium.net/) herunter und installiere es.

### 💎 Android Studio

Lade [Android Studio](https://developer.android.com/studio) herunter und installiere es. Beim ersten Start alle empfohlenen SDK-Komponenten installieren lassen. 

### 💎 Umgebungsvariablen setzen

Öffne: _Systemsteuerung → System → Erweiterte Systemeinstellungen → Umgebungsvariablen_

Unter **Systemvariablen** hinzufügen:

```
JAVA_HOME   →  C:\Users\user1\AppData\Local\Programs\Eclipse Adoptium\jdk-25.0.3.9-hotspot\
ANDROID_HOME  →  C:\Users\user1\AppData\Local\Android\Sdk
```

Zur **Path-Variable** hinzufügen:

```
%ANDROID_HOME%\tools
%ANDROID_HOME%\platform-tools
```

---

## 🛠️ Schritt 1 – Projektstruktur vorbereiten

Capacitor erwartet die Web-Dateien in einem bestimmten Ordner (`dist`). Struktur anlegen:

```
mein-projekt/
├── dist/
│   └── index.html   ← fertige App-Datei hierhin kopieren
├── package.json     ← wird in Schritt 2 erstellt
```

Ordner in VS Code öffnen, Terminal starten (`Strg + ö`).

> [!tip] Ordnername nachträglich ändern
> sollte Capacitor den Ordner nicht finden                                                      (z.B. nach einer Umbenennung), kann der Pfad jederzeit in `capacitor.config.json` angepasst werden.

---

## 🛠️ Schritt 2 – npm-Projekt initialisieren

### 💎 Terminal

```bash
Set-ExecutionPolicy -ExecutionPolicy Bypass -Scope Process
```

```bash
npm init -y
```

---

## 🛠️ Schritt 3 – Capacitor installieren

### 💎 Terminal

```bash
npm install @capacitor/core
npm install --save-dev @capacitor/cli
```

---

## 🛠️ Schritt 4 – Capacitor initialisieren

### 💎 Terminal

```bash
npx cap init
```

### 📌 Abgefragte Werte

```
Name        →  kalender_app
Package ID  →  com.hostname.kalender_app
Web asset directory  →  dist
```

---

## 🛠️ Schritt 5 – Windows Defender Ausschlüsse setzen

_Windows-Sicherheit → Viren- & Bedrohungsschutz → Einstellungen verwalten → Ausschlüsse_

Folgende Ordner hinzufügen:

```
C:\Users\user1\.gradle
C:\Users\user1\AppData\Local\Android\Sdk
C:\Users\user1\AppData\Local\Google\AndroidStudio2026.1.1
C:\Users\user1\Desktop\Kalender_App\android
```

> [!tip] Warum das wichtig ist 
> Ohne diese Ausschlüsse scannt Windows Defender während des Gradle-Builds jeden einzelnen File – das verlangsamt den Build massiv oder bricht ihn ab.

---

## 🛠️ Schritt 6 – Android-Plattform hinzufügen

### 💎 Terminal

```bash
npm install @capacitor/android
npx cap add android
```

Danach wird ein `android/`-Ordner im Projekt erstellt.

> [!warning] Hinweis zur Warnung 
> Die Meldung `sync could not run--missing www directory` kann ignoriert werden – der Sync folgt im nächsten Schritt.

---

## 🛠️ Schritt 7 – Web-Assets synchronisieren

### 💎 Terminal

```bash
npx cap sync android
```

> [!tip] Wann ausführen? 
> Diesen Befehl nach **jeder Änderung** an der `index.html` ausführen, bevor in Android Studio neu gebaut wird.

---

## 🛠️ Schritt 8 – Projekt in Android Studio öffnen

### 💎 Terminal

```bash
npx cap open android
```

Android Studio öffnet sich automatisch mit dem Projekt.

> [!tip] Gradle-Sync abwarten 
> Beim ersten Öffnen läuft ein Gradle-Sync der einige Minuten dauern kann. Warten bis die Statusleiste unten in Android Studio fertig ist.

---

## 🛠️ Schritt 9 – APK in Android Studio bauen

### 📌 Ablauf

```
Menü: Build → Build Bundle(s) / APK(s) → Build APK(s)
```

Nach dem Build erscheint unten rechts ein Toast: _„APK(s) generated"_ → auf **locate** klicken.

### 📌 Speicherort der fertigen APK

```
android/app/build/outputs/apk/debug/app-debug.apk
```

---

## 🛠️ Schritt 10 – APK auf das Android-Gerät installieren

### 📌 Option A – Per USB-Kabel (empfohlen)

**Am Gerät einmalig einrichten:**

1. _Einstellungen → Über das Telefon → 7× auf Buildnummer tippen_ (Developer Mode aktivieren)
2. _Entwickleroptionen → USB-Debugging aktivieren_
3. Handy per USB anschließen und den Schlüssel auf dem Gerät bestätigen

### 💎 Terminal

```bash
adb install android/app/build/outputs/apk/debug/app-debug.apk
```

> [!warning] Bekannte Fehlermeldungen
> 
> ```
> adb.exe: no devices/emulators found
> adb.exe: device unauthorized
> ```
> 
> → USB-Debugging am Gerät prüfen und den Bestätigungsdialog auf dem Bildschirm kontrollieren.

### 💎 Terminal – Nur bei ADB-Verbindungsproblemen

```bash
adb kill-server
adb start-server
adb devices
```

### 📌 Option B – Datei direkt übertragen

`app-debug.apk` per USB oder Cloud auf das Gerät kopieren und direkt installieren. Einmalig _„Installation aus unbekannten Quellen"_ in den Android-Einstellungen erlauben.

---
## 🛠️ App aktualisieren – Ablauf bei Änderungen an der index.html

> [!abstract] Wann gilt das? 
> Immer wenn etwas an der `index.html` verändert wurde – z.B. ein neuer Button, ein zusätzliches Eingabefeld oder eine Designänderung – und die Änderung auch in der installierten APK landen soll.

### 📌 Kurzübersicht

```
1. index.html anpassen & speichern
2. npx cap sync android   ← Änderungen ins Android-Projekt übertragen
3. In Android Studio neu bauen
4. APK per adb installieren
```

### 💎 Terminal – Schritt 2 & 4


```bash
npx cap sync android
```

```bash
adb install android/app/build/outputs/apk/debug/app-debug.apk
```

### 📌 Schritt 3 – In Android Studio

```
Build → Build Bundle(s) / APK(s) → Build APK(s)
```

> [!tip] Android Studio 
> muss nicht neu geöffnet werden Es reicht, den Build erneut anzustoßen. Ein erneutes `npx cap open android` ist nicht nötig, solange Android Studio noch offen ist.

> [!warning] Häufiger Fehler 
> `npx cap sync android` vergessen → Android Studio baut die alte Version ohne die neuen Änderungen. Immer zuerst syncen, dann bauen.
