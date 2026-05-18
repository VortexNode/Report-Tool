# Changelog – VIOCON Report Tool

Alle wichtigen Änderungen und Neuerungen werden hier dokumentiert.

---

## Version 2.2 – Mai 2026

### Neu
- **Lade-Bildschirm**: Das Tool zeigt sofort nach dem Start einen VIOCON-Splash-Screen mit animiertem Fortschrittsbalken. Alle schweren Module (pandas, matplotlib, reportlab usw.) werden im Hintergrund geladen – der Balken zeigt den Fortschritt in Echtzeit an. Das Hauptfenster öffnet sich erst, wenn alles vollständig geladen ist.

### Verbessert
- **UAC-Admin-Modus**: Die EXE fordert beim Start automatisch Administrator-Rechte an (Windows-UAC-Prompt), was die Kompatibilität mit Windows-Firmenrechnern erhöht.
- **Kleinere EXE**: Durch gezielte Modul-Bündelung von ~290 MB auf ~92 MB reduziert – schnellere Verteilung und kürzere Entpack-Zeit beim Start.
- **Antivirus-Kompatibilität**: UPX-Komprimierung deaktiviert, um Fehlalarme von Antivirensoftware zu vermeiden.
- **Portabilität**: Zuverlässiger Betrieb auf anderen Windows-11-Rechnern ohne Python-Installation sichergestellt.

---

## Version 2.1 – 2025

### Neu
- **Standard-Modus**: LIS-Report und Flotten-Report können jetzt in einem einzigen Schritt gemeinsam erstellt werden (3 Excel-Dateien + 1 PDF).
- **Einstellungen-Dialog**: Über das Hamburger-Menü (☰) kann ein Standard-Speicherpfad für Reports festgelegt werden – dieser wird bei jedem Speichern automatisch vorausgewählt.
- **Automatische Update-Prüfung**: Beim Start wird im Hintergrund geprüft, ob eine neuere Version verfügbar ist. Bei einem Update erscheint ein Hinweis-Dialog mit direktem Link zum Download.
- **Datei-Typ-Erkennung**: Hochgeladene Dateien werden automatisch klassifiziert (Ladevorgänge, Ladestationen, Monta-Rechnung, Flotten-Vorgänge). Falsch zugeordnete Dateien werden mit Warnung markiert.

### Verbessert
- Fortschrittsbalken zeigt den Stand der Report-Generierung an.
- Validierungsmeldungen bei fehlenden oder nicht erkannten Dateien.
- Info-Panel zeigt erkannte Metadaten (Kunde, Monat, Zeitraum, Rechnungsnummer, Brutto-Korrektur) vor der Verarbeitung an.
- Warn-Karten bei Auffälligkeiten in den Quelldaten.

---

## Version 2.0 – 2025

### Neu – Komplette Neugestaltung

- **Neues UI-Design**: Vollständig neu entwickelte Oberfläche mit dunklem Theme und VIOCON CI-Farben (Orange #ef7d00 / Dunkelgrau).
- **Schritt-basierter Workflow**: 4-Schritte-Führung (Modus wählen → Dateien auswählen → Verarbeitung starten → Ergebnis speichern) für eine intuitive, fehlerfreie Bedienung.
- **Drag & Drop**: Dateien können direkt in das Fenster gezogen werden, ohne den Datei-Dialog zu öffnen.
- **LIS-Report**: Neue Auswertung der Ladeinfrastruktur aus Ladevorgängen- und Ladestationen-Excel-Dateien.
- **Flotten-Report**: Flotten-Buchungsnachweis aus Ladevorgängen (Excel) und Monta-Rechnung (PDF).
- **Hintergrund-Verarbeitung**: Report-Generierung läuft in einem eigenen Thread – die Benutzeroberfläche bleibt während der Verarbeitung voll reaktionsfähig.
- **Log-Panel**: Aufklappbare Ausgabe-Konsole mit farbiger Statusanzeige (grün = Erfolg, orange = Warnung, rot = Fehler).
- **Hamburger-Menü**: Einheitlicher Zugang zu allen App-Optionen über das ☰-Symbol im Header.
- **Mehrfach-Reports**: Im Standard-Modus werden LIS- und Flotten-Report parallel erstellt und beide als separate PDFs zum Speichern angeboten.

---

## Version 1.0 – 2024

### Erstveröffentlichung

- Grundlegende Report-Generierung für LIS (Ladeinfrastruktur-Auswertung).
- Manuelle Dateiauswahl über den Windows-Dateidialog.
- Einfache Ergebnisausgabe als PDF.
- Keine Python-Installation auf dem Zielrechner erforderlich (eigenständige EXE).
