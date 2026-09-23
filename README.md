# ASV2paedML

Eine einzelne, eigenständige HTML-Seite zum Bereinigen von ASV-Klassen-/Lehrerlisten-Exporten und zum Export in einem für **paedML**-Importe geeigneten Format. Kein Server, keine Installation, keine Abhängigkeiten — einfach im Browser öffnen.

## Warum

ASV-Exporte lassen sich selten unverändert nach paedML importieren: Spaltennamen weichen ab, IDs duplizieren sich, Namen enthalten Sonderzeichen, Doppelnamen oder Namenszusätze, und Benutzernamen überschreiten oft das für paedML Windows 5.x gültige Längenlimit. ASV2paedML nimmt eine CSV/TSV-Datei entgegen, erkennt diese Probleme automatisch, schlägt Korrekturen vor und lässt jede Zeile gezielt manuell bestätigen, bevor exportiert wird.

## Nutzung

1. `ASV2paedML.html` im Browser öffnen (Doppelklick genügt, kein Server nötig).
2. Datei per Drag & Drop auf die Fläche ziehen oder über den Dateiauswahl-Dialog laden.
3. Spaltenüberschriften in der Tabelle anklicken, um sie den Zielfeldern zuzuordnen (Vorname, Name, Klasse, Externe ID, Kennwort, E-Mail-Adresse, Benutzername).
4. Reihenfolge der Spalten per Griff (⠿) am linken Rand der Kopfzeile per Drag & Drop anpassen, nicht benötigte Spalten über das Mülleimer-Symbol ausblenden.
5. Auffälligkeiten im Bereich **„Manueller Abgleich nötig"** oben prüfen, korrigieren (oder Korrekturvorschläge übernehmen) und einzeln bestätigen.
6. Über den Button **„Als CSV exportieren"** die bereinigte, UTF-8-kodierte, semikolon-getrennte Datei herunterladen.

Werden Umlaute/ß nach dem Import falsch angezeigt, in den Optionen unter **Import → Zeichenkodierung** auf „ANSI (Windows-1252)" umstellen und die Datei erneut laden.

## Funktionsübersicht

### Spalten-Mapping
- Zielbezeichnung direkt in der Spaltenüberschrift wählen, kein separates Zuordnungs-Panel nötig.
- Spalten per Drag & Drop neu anordnen.
- Nicht benötigte Spalten ausblenden — sie fließen dann auch nicht in den Export ein.
- Nicht zugeordnete Spalten werden beim Export automatisch weggelassen.

### Automatische Prüfungen (priorisiert)
Pro Zeile wird immer nur der ranghöchste offene Punkt angezeigt; erst nach dessen Behebung erscheint der nächste:

1. **Externe ID** — Dubletten und vom Mehrheitsformat abweichende IDs werden erkannt und gruppiert angezeigt.
2. **Nachnamenszusatz** — erkennt Zusätze wie „von", „van der", „bin" (konfigurierbare Liste).
3. **Doppelname** — zusammengesetzte Vor-/Nachnamen (z. B. „Maja-Sophie") mit Vorschlag zur Kürzung.
4. **Sonderzeichen** — Zeichen außerhalb a–z/A–Z im Namen, inklusive ASCII-Transliterationsvorschlag nach ICAO-Doc-9303-Prinzip (z. B. Ö → „Oe", ß → „ss"). Kann kein Zeichen automatisch übersetzt werden, wird kein (falscher) Vorschlag erzeugt, sondern auf manuelle Anpassung verwiesen.
5. **Benutzername** — prüft auf Leerzeichen, einen Punkt am Zeilenende sowie eine konfigurierbare Maximallänge (Standard: 17 Zeichen + 1 Punkt, passend zu paedML Windows 5.x) und schlägt bei Bedarf einen korrigierten Benutzernamen vor.

Jede dieser Prüfungen lässt sich in den Optionen einzeln deaktivieren.

### Manueller Abgleich
- Betroffene Zeilen werden aus der Haupttabelle herausgezogen und oben gruppiert angezeigt (z. B. „Doppelte Externe ID", „Ungültiger Benutzername …").
- Werte sind direkt in der Liste editierbar; verfügbare Korrekturvorschläge lassen sich per Klick übernehmen.
- Jede Zeile muss einzeln (oder gesammelt über „Alle bestätigen") bestätigt werden, bevor sie zurück in die Haupttabelle und in den Export wandert.
- Zeilen lassen sich zur Löschung vormerken (erscheinen rot durchgestrichen) und erst nach zusätzlicher Bestätigung endgültig entfernen — inklusive Möglichkeit, die Löschung wieder zurückzunehmen.
- Bearbeitungen einer Zeile lassen sich jederzeit verwerfen (Originalwerte wiederherstellen).

### Änderungsprotokoll
Zeigt vor dem Export eine Zusammenfassung aller Abweichungen vom Original: entfernte/umbenannte Spalten, geänderte Spaltenreihenfolge, gelöschte Zeilen und bestätigte Zellenänderungen (inkl. Vorher/Nachher-Wert und CSV-Zeilennummer). Lässt sich als eigene Datei exportieren.

### Referenzlisten
Zwei Listen steuern die automatische Erkennung und lassen sich in den Optionen einsehen, ergänzen und als CSV laden/herunterladen:

- **Sonderzeichen-Anpassung** (`sz-anpassung.csv`) — Zeichen-Ersetzungstabelle für die Benutzernamen-/Namensvorschläge.
- **Nachnamenszusätze** (`nachnamenszusaetze.csv`) — Liste erkannter Namenszusätze.

Eigene Ergänzungen werden nur für die laufende Sitzung gespeichert und gehen beim Neuladen der Seite verloren, sofern sie nicht vorher exportiert werden.

### Weitere Optionen
- **Lehrerliste**: macht „Klasse" zu einem optionalen statt einem Pflichtfeld, für Importe ohne Klassenzuweisung.
- **Zeichenkodierung**: UTF-8 oder ANSI (Windows-1252) beim Einlesen der Datei.
- **Automatische Übernahme aller Vorschläge** (als riskant gekennzeichnet): übernimmt jeden verfügbaren Korrekturvorschlag automatisch — ersetzt nicht die manuelle Prüfung und Bestätigung jeder Zeile vor dem Export.

## Export

- CSV, UTF-8, semikolon-getrennt.
- Nur zugeordnete, nicht ausgeblendete Spalten.
- Nur bestätigte bzw. unauffällige Zeilen (zur Löschung vorgemerkte und noch nicht final bestätigte Zeilen werden nicht mit exportiert).

## Technische Hinweise

- Reines HTML/CSS/JavaScript, keine externen Abhängigkeiten, keine Serverkommunikation — alle Daten bleiben lokal im Browser.
- Einbindung der beiden Referenz-CSVs kann automatisch erfolgen, wenn `sz-anpassung.csv` und `nachnamenszusaetze.csv` im selben Verzeichnis liegen und die Seite über `http(s)://` (nicht `file://`) geöffnet wird — andernfalls greifen die eingebauten Standardlisten.
- Keine Datenpersistenz über einen Seiten-Reload hinaus (außer den beiden Referenzlisten, sofern über die Optionen ergänzt).

## Bekannte Einschränkungen

- Kein Undo/Redo auf Aktionsebene (einzelne Zeilen lassen sich aber jederzeit zurücksetzen).
- Keine Tastatur- oder Touch-Bedienung für Drag-&-Drop-Interaktionen (Spaltenreihenfolge, Datei-Upload-Fläche).
- Für sehr große Dateien (mehrere tausend Zeilen) nicht performance-optimiert.
