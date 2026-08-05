# Changelog – VIOCON Report Tool

Alle wichtigen Änderungen und Neuerungen werden hier dokumentiert.

---

## Version 3.1 – August 2026

### Bugfixes
- **Flotten-Report: Deutsche Marketplace-Ladevorgänge fälschlich als „Ausland" ausgewiesen**: Enthielt die Rohdaten-Spalte `chargeCountry` den ausgeschriebenen Ländernamen „GERMANY" statt des ISO-Codes „DE", griff der Ländervergleich nicht – die Position wurde als „Ausland GERMANY" mit dem Hinweis „EU-Vorsteuervergütung prüfen" ausgewiesen statt korrekt als abzugsfähige deutsche Vorsteuer. Behoben – „GERMANY"/„DEUTSCHLAND" werden jetzt beim Einlesen auf „DE" normalisiert.

---

## Version 3.0 – August 2026

### Bugfixes
- **Flotten-Report: Falsche USt bei gemischtem Roaming (Reverse Charge + Auslands-MwSt)**: Die selbst zu versteuernde USt für Reverse-Charge-Roaming (§13b UStG) wurde bisher pauschal mit 19 % auf das *gesamte* Roaming berechnet – auch auf den Anteil, der bereits mit einer echten ausländischen MwSt in Rechnung gestellt wurde. Dadurch stimmte die USt-Summe in „GESAMT FÄLLIGER BETRAG" nicht mit den einzeln ausgewiesenen Positionen überein, sobald ein Monat beide Fälle gemischt enthielt. Behoben – die 19 % werden jetzt ausschließlich auf den tatsächlichen Reverse-Charge-Anteil angewendet.
- **Flotten-Report: `kind`-Klassifizierung ohne Rückfallebene**: Enthielt die Excel-Spalte `kind` einen von Monta neu eingeführten, noch nicht erfassten Wert, fiel die betroffene Zeile stillschweigend in die falsche Kategorie (Marketplace statt Roaming/Sponsored). Behoben – `kat_fn()` prüft bei unbekanntem `kind` zusätzlich `transactionName` als zweites Signal.

### Neu
- **Flotten-Report: Abgleich Rohdaten ↔ Monta-Rechnung**: Die Buchungsübersicht nutzt jetzt die aus der Monta-Rechnung (PDF) gelesenen Steuerpositionen (Roaming/Marketplace je Steuersatz) als maßgebliche Quelle, sofern eine Rechnung mitgegeben wurde. Weicht der aus den Rohdaten berechnete Betrag ab, erscheint ein Hinweis in der Buchungshinweis-Spalte; Positionen, die nur in der Rechnung stehen (z. B. weil die Rohdaten sie nicht als Einzeltransaktion enthalten oder falsch klassifiziert haben), werden zusätzlich ergänzt. Ohne Rechnung greift weiterhin die bisherige pauschale Korrektur. Die Kostenstellen- und Detailauswertung bleibt unverändert aus den Rohdaten, da die Rechnung keine Transaktionsdetails liefert.
- **Flotten-Report: Abgleich schon in der Vorschau**: Schritt 2 (Datei-Auswahl) zeigt Abweichungen zwischen Excel-Rohdaten und Monta-Rechnung jetzt bereits vor der Report-Erstellung als Warnung an.

---

## Version 2.9 – Juli 2026

### Bugfixes
- **LIS-Report: Inaktive Ladestationen in „Betreute Ladestationen"**: Die Tabelle zeigte bisher alle Stationen aus der Ladestationen-Datei, unabhängig vom Status. Behoben – es werden nur noch Stationen mit `isActive = WAHR` aufgeführt.
- **LIS-Report: Individuell benannte Preisgruppen anderer Firmenflotten wurden als „Öffentlich (Roaming)" gezählt**: Ladevorgänge mit einer kundenspezifischen Preisgruppe (z. B. „charge@business ...") wurden bisher fälschlich dem öffentlichen Roaming statt der Flotten-Nutzung zugeordnet, sobald sie nicht exakt „charge@company" hießen. Erkennbar am Team-Namen (endet auf „Fleet"). Behoben – solche Ladevorgänge zählen jetzt korrekt als Flotte.
- **Flotten-Report: Rundungsfehler durch nicht berücksichtigte Ladevorgänge an eigenen Stationen**: Die vermeintlich kostenlosen Ladevorgänge an eigenen Stationen (charge@company) wurden in der Buchungsübersicht nur als Kennzahl mit hartkodiertem Betrag „0,00" angezeigt; ihr tatsächlicher (meist geringfügiger) Betrag floss nirgends in die Gesamtsumme ein. Über viele Ladevorgänge hinweg führte das zu Rundungsdifferenzen gegenüber der Monta-Rechnung. Behoben – die realen Beträge werden jetzt angezeigt und in „GESAMT FÄLLIGER BETRAG" mit eingerechnet.
- **LIS-Report: Doppelt vergebene Ladepunkt-Nummern im Balkendiagramm „Nutzung je Ladepunkt"**: Ladepunkte, die nicht (mehr) in der aktiv-gefilterten Ladestationen-Datei stehen (z. B. inzwischen deaktivierte Stationen mit Ladevorgängen im Berichtsmonat), aber im Diagramm durchnummeriert werden mussten, konnten durch die bisherige Positions-basierte Nummerierung zufällig dieselbe Nummer wie eine echte, bereits vergebene Station erhalten. Behoben – solche Ladepunkte erhalten jetzt garantiert eindeutige Nummern direkt nach der höchsten vergebenen Nummer.

---

## Version 2.8 – Juni 2026

### Bugfixes
- **LIS-Report: Fehlende Ladepunkte in der kWh-Tabelle**: Ladepunkte die in den Transaktionsdaten vorhanden sind, aber nicht in der Ladestationen-Datei, wurden bisher stillschweigend aus allen Tabellen und dem Balkendiagramm ausgeschlossen. Behoben – solche Ladepunkte werden jetzt alphabetisch ans Ende der Auswertung angehängt. Die bekannte Reihenfolge aus der Ladestationen-Datei bleibt für die dort erfassten Punkte erhalten.
- **Flotten-Report: `charge@company`-Zeile fehlte in April- und Mai-Reports**: Monta hat zwischen März und April 2026 die Steuerklassifikation für interne Ladevorgänge von `0,00 %` auf den Code `O` (Outside scope) geändert. Das Tool erkannte `O` (und `AE`) bisher nicht und zeigte die Zeile „Ladevorgänge charge@company" in der Buchungsübersicht nicht an. Behoben – beide Codes werden jetzt identisch zu `0,00 %` behandelt.

### Verbessert
- **Splash-Screen für Blueprint-Ersteller**: Der Blueprint-Ersteller hat jetzt ebenfalls den Ladebalken beim Start – schwere Module (pandas, matplotlib, reportlab usw.) werden im Hintergrund geladen, das Hauptfenster öffnet sich erst wenn alles bereit ist.
- **Update-Link korrigiert**: Das Update-Popup öffnet jetzt direkt die Releases-Seite (`/releases`). Der Info-Dialog (☰ → Infos) zeigt weiterhin die allgemeine Projektseite.

---

## Version 2.7 – Juni 2026

### Neu
- **Original-Rechnung im Report**: Eine vorhandene Monta-Rechnung bzw. -Gutschrift (PDF) wird jetzt als erste Seite(n) vor den generierten Report gestellt. Die Monta-Rechnung ist dadurch im Flotten- und Standard-Modus nicht mehr verpflichtend, sondern optional (wie schon zuvor im LIS-Modus).
- **Ladevorgänge charge@company in der Buchungsübersicht**: Der Flotten-Report zeigt in der Buchungsübersicht jetzt eine eigene Zeile „Ladevorgänge charge@company" mit der dazugehörigen kWh-Menge laut Monta-Rechnung – als Vergleichswert zur eigenen Zählung der Stationen.
- **„Nutzer"-Kennzahl im Flotten-Report**: Der Überblick im Flotten-Report zeigt zusätzlich die Anzahl unterschiedlicher Nutzer als KPI-Box.
- **Datei entfernen per Klick**: Im Datei-Auswahl-Dialog kann jede Datei über ein „✕"-Symbol einzeln wieder aus der Liste entfernt werden.
- **Geladene Energie nach Preisgruppe**: Die kWh-Tabelle im LIS-Report gruppiert jetzt nach Preisgruppe (charge@company, charge@work, charge@home, ohne Preisgruppe) statt nach Nutzungsart – für eine genauere Kostenstellen-Zuordnung.

### Verbessert
- **Automatische Tabellenbreite**: In allen Tabellen der PDF-Reports (Buchungsübersicht, Kostenstellenübersicht, Detailnachweis, Roaming-Netzwerke, LIS-Tabellen) wird eine Spalte automatisch verbreitert, wenn ihr Inhalt sonst über den Rand hinauslaufen würde.
- **Robuster Excel-Import**: Spaltennamen und Zahlenformate (deutsch „1.234,56" und englisch „1,234.56") werden jetzt unabhängig von der genauen Schreibweise erkannt und korrekt eingelesen.
- **Kennzahlen ohne Rundungsfehler**: Die Zahlen in den Überblick-KPI-Boxen werden jetzt ohne Zwischenrundung direkt aus den Rohdaten berechnet.
- **Schlankerer Flotten-Überblick**: „Ladekosten netto" und „Vorsteuer" wurden aus dem Überblick des Flotten-Reports entfernt (LIS-Report unverändert).
- **Rechnungsnummer ohne Formatprüfung**: Die Rechnungsnummer aus der Monta-Rechnung wird unverändert übernommen, ohne strenge Format-Prüfung – dadurch entfallen unnötige Warnungen bei abweichenden Formaten.
- **Lesbares Balkendiagramm „Nutzung je Ladepunkt" (LIS-Report)**: Sind die Ladepunkt-Bezeichnungen auf der x-Achse zu lang bzw. zu zahlreich, werden stattdessen nur noch Nummern (1, 2, 3, …) angezeigt. Die Zuordnung der Nummern findet sich in einer neuen Spalte „Nr." in der Tabelle „Betreute Ladestationen".
- **Tortendiagramm „Nutzungsverteilung gesamt" (LIS-Report)**: Wird jetzt – wie die kWh-Tabelle „Geladene Energie in kWh" – nach Preisgruppe (charge@company, charge@work, charge@home, ohne Preisgruppe, ggf. weitere) statt nach Nutzungsart ausgewertet, sodass beide Auswertungen konsistent zueinander sind.

### Entfernt
- **Blueprint-System**: Das Blueprint-Feature (Kundenwünsche als JSON-Datei) wurde aus dem Standard-Tool entfernt.

---

## Version 2.6 – Mai 2026

### Neu
- **Warndialog bei Auffälligkeiten**: Wenn beim Klicken auf „Report erstellen" Probleme erkannt werden (Dateityp mehrfach vorhanden, nicht erkannte Dateien), erscheint ein Dialog mit einer genauen Beschreibung. Der Nutzer kann wählen: **Trotzdem erstellen** (Report wird mit den verfügbaren Dateien erstellt) oder **Abbrechen** (zurück zu Schritt 1, Dateien neu auswählen).

### Bugfixes
- **Datei-Upload nach Pflichtdateien wieder möglich**: Nach dem Hinzufügen der Pflichtdateien wurde Schritt 2 (DropZone + Dateiliste) fälschlicherweise deaktiviert. Weitere Dateien konnten weder per Dialog noch per Drag & Drop hinzugefügt werden. Behoben durch neue Methode `set_files_ready()` die den Inhalt aktiv lässt.

### Verbessert
- **Unbegrenzte Datei-Uploads**: Dateien können jetzt in beliebiger Anzahl hochgeladen werden. Zuvor wurden bei Typkonflikten Dateien lautlos überschrieben.
- **Modus-Karten deutlich besser lesbar**: Beschreibungen in hellem Weiß mit Trennlinie; Hinweistexte in klar lesbarem Hellgrau (#c8cee8, Kontrast ~8:1 statt zuvor ~3:1). Schriftgröße erhöht, Kartenhöhe passt sich automatisch an.
- **Standard-Modus: 5 Dateien**: Die LIS-Gutschrift wird jetzt auch im Standard-Modus erkannt und verarbeitet (3 Excel + 1 PDF Monta-Rechnung · optional: 1 PDF LIS-Gutschrift). Dateianzahl-Anzeige zeigt nun „4–5 Dateien".
- **Umlaute**: Umlautdarstellung in GUI und PDF-Reports erneut geprüft – funktioniert korrekt in allen Bereichen.

---

## Version 2.5 – Mai 2026

### Neu
- **Einstellungen – Import-Ordner**: Im Einstellungs-Dialog kann jetzt zusätzlich zum Export-Ordner auch ein Standard-Ordner für den Daten-Import festgelegt werden. Der Datei-Auswahl-Dialog öffnet sich dann direkt in diesem Ordner.

### Bugfixes
- **Umlaute im Report behoben**: ä, ö, ü und ß wurden in den PDF-Reports nicht korrekt dargestellt. Beide Report-Module registrieren jetzt beim Start Arial aus den Windows-Systemfonts, der vollständige Unicode-Unterstützung bietet. Bei fehlendem Font wird automatisch auf Helvetica zurückgefallen.

---

## Version 2.4 – Mai 2026

### Bugfixes
- **Report-Erstellung dauerhaft repariert**: Parameter werden jetzt in `ReportWorker` explizit namentlich übergeben statt per `**dict` – Key-Name-Abweichungen zwischen Extraktor und Report-Funktion können nie mehr zum Absturz führen.
- **Parameter-Mismatch behoben**: `generate_report()` erhielt fälschlicherweise `logo` statt `logo_path`, `file` statt `filepath` und `rechnungsnr` statt `rnr`.
- **Fehlender stdlib-Import behoben**: `ModuleNotFoundError: No module named 'email'` beim Start auf anderen PCs durch explizites Bündeln der stdlib-Module (`email`, `http`, `urllib`, `ssl`) in der EXE.

### Verbessert
- **EXE vollständig selbstständig**: Qt-Plugins (imageformats, TLS, platforms, styles) werden jetzt explizit in die EXE eingebettet. Das Logo wird auf jedem PC korrekt angezeigt; HTTPS-Update-Check und Windows-TLS funktionieren ohne Zusatzdateien.
- **matplotlib Font-Cache**: Bei gefrorenem EXE-Start wird der Cache-Ordner auf das System-Temp-Verzeichnis umgeleitet – verhindert stillen Absturz wegen read-only `_MEIPASS`.
- **Unblock.bat**: Neue Hilfsdatei in `dist/` – einmalig auf dem Ziel-PC ausführen, um die Windows-SmartScreen-Blockade zu entfernen.
- **Build-Ausgabe**: `build.bat` weist nach dem Build auf `Unblock.bat` hin.

---

## Version 2.3 – Mai 2026

### Neu
- **LIS Gutschrift-Abgleich**: Im LIS-Report-Modus kann optional eine Monta-Gutschrift (PDF) hinzugefügt werden. Das Tool erkennt automatisch das Monta-LIS-Rechnungsformat (inkl. realistischer Dateinamen wie `Kunde LIS-MM-YYYY-invoice-....pdf`), summiert alle kWh-Einträge aus dem Marketplace-Abschnitt und fügt einen Abgleichsabschnitt in den Report ein.

### Verbessert
- **Fehlermeldungen**: Alle Fehler nennen jetzt konkret den Dateinamen, die fehlende Spalte und mögliche Ursachen.
- **EXE-Archivierung**: `build.bat` sichert die vorherige EXE automatisch mit Versionsnummer unter `dist/archive/`.
- **Drag & Drop**: UAC-Admin entfernt (war Ursache für DnD-Blockade durch UIPI); Kompatibilität bleibt durch korrektes Bundling gewährleistet.
- **Balkendiagramm**: Lange Ladepunkt-Namen werden automatisch abgekürzt (LP 1, LP 2, …) mit einer Legende rechts daneben.
- **Kein Korrekturbetrag = kein Fehler**: Wenn in der Monta-Rechnung kein Korrekturbetrag gefunden wird, erscheint jetzt keine Warnung mehr.
- **Header-Unterstrichfehler behoben**: Orangene Trennlinie ist jetzt ein eigenes Widget statt CSS-`border-bottom` – keine Artefakte unter Logo, Trenner oder Versionsnummer.
- **Hinweistexte**: LIS-Modus zeigt „Team-Ladevorgänge", Flotten-Modus „Flotten-Ladevorgänge".
- **Checkmark**: Fortschrittsbalken wird vor `set_done()` gesetzt – korrekte Darstellung.
- **Flotten-Report – Roaming-Netzwerke**: Nur echte externe Roaming-Ladevorgänge (leere `priceGroup`) werden angezeigt.
- **Flotten-Report – Ladearten-Übersicht**: Neuer Abschnitt im Flottenmanager zeigt Aufschlüsselung nach `kind` (charge-sponsored / charge-professional / charge-roaming-emsp).
- **Archivierung**: Backup-EXEs werden mit `_1`, `_2`, `_3`… nummeriert.

---

## Version 2.2 – Mai 2026

### Neu
- **Lade-Bildschirm**: Das Tool zeigt sofort nach dem Start einen VIOCON-Splash-Screen mit animiertem Fortschrittsbalken. Alle schweren Module (pandas, matplotlib, reportlab usw.) werden im Hintergrund geladen – der Balken zeigt den Fortschritt in Echtzeit an. Das Hauptfenster öffnet sich erst, wenn alles vollständig geladen ist.

### Verbessert
- **UAC-Admin entfernt**: Die EXE läuft ohne Administrator-Rechte – war vorher Ursache für Drag-&-Drop-Blockade (UIPI). Kompatibilität bleibt durch korrektes Bundling gewährleistet.
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
