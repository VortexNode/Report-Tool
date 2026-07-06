# VIOCON Report Tool

**Automatisierte Erstellung von LIS-Reports und Flotten-Reports aus Excel- und PDF-Quelldaten.**

*Elektromobilität. Einfach. Machen.*

---

## Ankündigungen

<!-- Hier stehen aktuelle Ankündigungen -->

---

## Überblick

Das VIOCON Report Tool ist ein Windows-Desktop-Programm, das aus Rohdaten (Ladevorgänge, Ladestationen, Monta-Rechnungen) vollständige PDF-Reports erstellt. Es ist als eigenständige EXE ausgeliefert – keine Python-Installation oder weitere Software notwendig.

---

## Modi

| Modus | Beschreibung | Benötigte Dateien |
|-------|-------------|-------------------|
| **LIS-Report** | Ladeinfrastruktur-Auswertung | 2× Excel (Ladevorgänge + Ladestationen) · optional: 1× PDF (Monta-Gutschrift) |
| **Flotten-Report** | Flotten-Buchungsnachweis | 1× Excel (Ladevorgänge) · optional: 1× PDF (Monta-Rechnung) |
| **Standard** | LIS + Flotte in einem Schritt | 3× Excel · optional: 1× PDF (Monta-Rechnung), 1× PDF (LIS-Gutschrift) |

---

## Features

- **Drag & Drop** – Dateien direkt ins Fenster ziehen, beliebig viele und jederzeit
- **Automatische Dateierkennung** – Das Tool erkennt selbstständig, welche Datei welchem Typ entspricht
- **Warndialog bei Konflikten** – Bei mehrfachen oder nicht erkannten Dateien: Wahl zwischen „Trotzdem erstellen" und „Abbrechen"
- **Metadaten-Vorschau** – Vor der Verarbeitung werden Kunde, Monat, Zeitraum und Rechnungsnummer angezeigt
- **Hintergrund-Verarbeitung** – Die UI bleibt während der Report-Generierung reaktionsfähig
- **Ladebildschirm** – Animierter Fortschrittsbalken beim Start
- **Original-Rechnung im Report** – Vorhandene Monta-Rechnung/-Gutschrift wird als erste Seite(n) vor den generierten Report gestellt
- **Robuster Excel-Import** – Abweichende Spaltennamen und Zahlenformate (deutsch/englisch) werden automatisch erkannt
- **Automatische Tabellenbreite** – Spalten in den PDF-Tabellen passen sich an lange Texte an, sodass nichts abgeschnitten wird
- **Neuigkeiten-Benachrichtigung** – Popup wenn neue Inhalte auf GitHub verfügbar sind
- **Einstellbarer Speicherpfad** – Standard-Ordner für gespeicherte Reports konfigurierbar

---

## Systemvoraussetzungen

- Windows 10 oder Windows 11 (64-Bit)
- Keine Python-Installation notwendig

---

## Installation & Start

1. `VIOCON_Report_Tool.exe` (und `Unblock.bat`) aus dem `dist/`-Ordner kopieren
2. **Nur beim ersten Mal auf einem neuen PC:** `Unblock.bat` per Rechtsklick → „Als Administrator ausführen" starten – entfernt die Windows-SmartScreen-Sperre einmalig
3. Doppelklick auf die EXE
4. Das Tool startet mit einem Ladebildschirm – das Hauptfenster öffnet sich automatisch

Keine Installation erforderlich. Die EXE ist portabel und läuft von jedem Speicherort.

---

## Bedienung

### Schritt 1 – Modus wählen
Einen der drei Modus-Kacheln anklicken. Der aktive Modus wird orange hervorgehoben.

### Schritt 2 – Dateien auswählen
- Dateien per **Drag & Drop** in die Drop-Zone ziehen, oder
- **„Dateien auswählen"** klicken und Dateien über den Dialog wählen

Die Dateien werden automatisch dem richtigen Typ zugeordnet (farbige Badges). Darunter erscheint ein Info-Panel mit den erkannten Metadaten.

> Wenn eine Datei nicht erkannt wird, erscheint sie mit dem Badge „Unbekannt" und einer Warnung im Log-Panel.

### Schritt 3 – Report erstellen
**„Report erstellen"** klicken. Der Fortschrittsbalken zeigt den aktuellen Stand. Bei einem Fehler erscheint ein Dialog mit Details; das Log-Panel unten enthält den vollständigen Fehler-Trace.

### Schritt 4 – Ergebnis speichern
**„Datei speichern"** klicken. Der Dateiname wird automatisch vorgeschlagen (Kunde + Monat). Der Speicherort kann frei gewählt werden.

Mit **„Neuen Report erstellen"** wird der komplette Workflow zurückgesetzt.

---

## Einstellungen

Über das **☰-Menü** oben rechts → **Einstellungen**:

| Einstellung | Beschreibung |
|-------------|-------------|
| Standard-Ordner Import | Ordner, der beim „Dateien auswählen"-Dialog vorausgewählt wird. Leer lassen = letzter verwendeter Systempfad. |
| Standard-Ordner Export | Ordner, der beim „Report speichern"-Dialog vorausgewählt wird. Leer lassen = letzter verwendeter Systempfad. |

---

## Build (für Entwickler)

**Voraussetzungen:** Python 3.11+, pip

```batch
# Alle Schritte automatisch (Abhängigkeiten installieren + EXE bauen)
build.bat
```

**Manuell:**
```batch
pip install -r requirements.txt
pyinstaller --noconfirm --clean VIOCON_Report_Tool.spec
```

Die fertige EXE liegt in `dist\VIOCON_Report_Tool.exe` (~91 MB).

### Projektstruktur

```
VIOCON_Report_Tool/
│
├── src/                        # Python-Quellcode
│   ├── gui.py                  # Hauptfenster & Einstiegspunkt
│   ├── extractor.py            # Datei-Erkennung & Metadaten-Extraktion
│   ├── viocon_fleet_report.py  # Flotten-Report-Logik
│   └── viocon_lis_report.py    # LIS-Report-Logik
│
├── assets/
│   ├── app/                    # Laufzeit-Assets (Icon, Logo)
│   ├── screenshots/            # App-Screenshots
│   └── design/                 # Design-Vorlagen & Konzepte
│
├── docs/
│   ├── CHANGELOG.md            # Vollständige Versionshistorie
│   └── RELEASE_NOTES.md        # GitHub-Release-Beschreibung (aktuelle Version)
│
├── build/                      # PyInstaller-Zwischendateien (gitignored)
├── dist/                       # Fertige EXE + Unblock.bat (gitignored)
│
├── README.md
├── .gitignore
├── build.bat                   # Build-Skript
├── requirements.txt            # Python-Abhängigkeiten
└── VIOCON_Report_Tool.spec     # PyInstaller Build-Konfiguration
```

---

## Abhängigkeiten

| Paket | Version | Verwendung |
|-------|---------|-----------|
| PySide6 | ≥ 6.6 | GUI-Framework (Qt) |
| pandas | ≥ 2.0 | Datenverarbeitung |
| numpy | ≥ 1.24 | Numerische Berechnungen |
| matplotlib | ≥ 3.7 | Diagramm-Generierung in Reports |
| reportlab | ≥ 4.0 | PDF-Erstellung |
| openpyxl | ≥ 3.1 | Excel-Dateien lesen/schreiben |
| pypdf | ≥ 4.0 | PDF-Dateien lesen (Monta-Rechnung) |

---

## Versionshistorie

Siehe [CHANGELOG.md](https://github.com/VortexStudiosLab/Report-Tool/blob/main/CHANGELOG.md) für alle Änderungen.

Aktuelle Version: **2.9**
