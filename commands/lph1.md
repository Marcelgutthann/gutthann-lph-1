---
name: lph1
description: Erstellt einen vollständigen LPH 1 Grundlagenermittlungsbericht (HOAI § 33, Gebäude und Innenräume) für ein beliebiges Architekturprojekt. Durchsucht alle Dateien im Projektordner, extrahiert Daten und befüllt das standardisierte HTML-Template.
user-invocable: true
---

# LPH 1 Grundlagenermittlungsbericht — Automatische Generierung

## OUTPUT-KONVENTION (verbindlich)

**WICHTIGSTE REGEL: Es wird ein EINZIGER Ordner namens `Claude/` im Projekt-Root erzeugt. ALLE Outputs landen dort drin. NIRGENDS sonst im Projekt.**

```
<Projekt-Root>/                           <- bleibt unveraendert
├── (die existierenden Projektordner)       (welche Struktur auch immer)
│
└── Claude/                               <- EINZIGER Ordner den wir anlegen
    ├── Projekt_Historie.md                 <- kumulative Wissensbasis
    ├── LPH_1/LPH1_Report_YYYY-MM-DD.html
    ├── LPH_2/LPH2_Report_YYYY-MM-DD.html
    ├── ... bis LPH_8/
    ├── Dashboard/Projekt_Dashboard_YYYY-MM-DD.html
    └── Fachplaner-Analyse/Fachplaner_Analyse_YYYY-MM-DD.md
```

**Dein konkreter Output-Pfad fuer diesen Skill:**
```
<Projekt-Root>/Claude/LPH_1/LPH1_Report_YYYY-MM-DD.html
```

**Pflichten beim Schreiben:**
1. Wenn `<Projekt-Root>/Claude/` nicht existiert: anlegen (mkdir)
2. Wenn `<Projekt-Root>/Claude/LPH_1` nicht existiert: anlegen
3. Datums-Format im Dateinamen: `YYYY-MM-DD` (z.B. 2026-05-27)
4. Bei jedem Aufruf: NEUE Datei mit aktuellem Datum (keine Ueberschreibung)

**NIEMALS schreibst du:**
- In andere Ordner des Projekts (00 Aktennotiz/, 11 Doku/, etc.) - keine Verschmutzung der Projekt-Struktur
- In den Projekt-Root direkt - nur in `Claude/` Unterstruktur
- In `~/.claude/plugins/` - das ist Read-only Plugin-Code
- In versteckte Ordner (.claude/) - nutze ausschliesslich `Claude/` (sichtbar)

---

## WISSENSBASIS NUTZEN

Falls `<Projekt-Root>/Claude/Projekt_Historie.md` existiert:
- ZUERST diese Datei lesen - sie enthaelt chronologisch alle E-Mails, Aktennotizen, Beschluesse
- Spart Zeit + Tokens (statt das ganze Projekt jedes Mal neu zu scannen)

Falls die Datei NICHT existiert:
- Empfehle dem User: "Tipp: Fuer aktuelle Daten zuerst /projekt-historie ausfuehren"
- Trotzdem das Projekt direkt scannen (Fallback)

---


## Template

Suche das Template relativ zum aktuellen Projektordner:
```powershell
Get-ChildItem -Path "." -Recurse -Filter "LPH1_Template.html" | Select-Object -First 1 -ExpandProperty FullName
```
Falls nicht gefunden: Hinweis auf Installation des `lph-1` Plugins geben.

Kopiere es als `LPH1_Report.html` und befülle alle `{{PLATZHALTER}}`.

## Ablauf — 5 Schritte

### Schritt 1: Projektordner durchsuchen
Durchsuche den GESAMTEN Projektordner rekursiv:
- **PDFs**: Aufgabenbeschreibung, B-Plan, FNP, Liegenschaftskarte, Grundbuch
- **DOCX**: Per PowerShell Word COM
- **XLSX**: Per PowerShell Excel COM (Beteiligtenliste)
- **MSG / EML**: E-Mail-Korrespondenz (Aktennotizen, Bauherrn-Anfragen)
- **Bilder**: Logo + Titelbild (Standort, Lageplan)

### Schritt 2: Daten extrahieren
- **Aufgabenstellung**: Was will der Bauherr (Funktion, Qualität, Termin, Kosten)
- **Beteiligte**: Bauherr, Bauherrnvertretung, Akquise-Status der Fachplaner
- **Standort**: B-Plan-Festsetzungen, Erschließung, Topografie, Altlasten
- **Bestandsunterlagen**: Falls vorhanden, vorhandene Pläne, Statik, TGA
- **Konzeptskizzen**: Erste Ideen, Bauherrenwünsche

### Schritt 3: Template kopieren und befüllen
1. Kopiere `LPH1_Template.html` als `LPH1_Report.html`
2. Ersetze ALLE `{{PLATZHALTER}}` mit den extrahierten Daten
3. Befülle Sidebar-Dateiliste mit allen gelesenen Dateien
4. Erstelle die 4 Kapitel:
   - 1.1 Aufgabenstellung
   - 1.2 Leistungs- und Untersuchungsbedarf
   - 1.3 Standort-Entscheidungshilfen
   - 1.4 Zusammenfassung der Ergebnisse

### Schritt 4: Empfehlungen für LPH 2
Erstelle aus den Ergebnissen konkrete Empfehlungen:
- Welche Fachplaner sind zu beauftragen (Priorität, Zeitpunkt)
- Welche Vorab-Untersuchungen sind notwendig (Bodengutachten, Vermessung, etc.)
- Welche Risiken sind bekannt (Schadstoffe, Altlasten, Brandschutz, etc.)

### Schritt 5: Qualitätsprüfung
- Keine `{{PLATZHALTER}}` mehr im Output
- Schreibstil: Sachlich, technisch, Passiv ("ist einzuhalten", "wird geprüft")
- Beteiligte nur indirekt ("seitens AG", "durch Fachplaner")
- Max. 5-8 Sätze pro Absatz

## Design-Regeln (Lessons-Learned — verbindlich)

- **Schwarze Banner-Balken (ch-label, ch-num, ch-title) ALLE einheitlich `#1a1a1a`** — niemals `#333` oder Graustufen
- **Print-CSS PFLICHT** für alle dunklen Elemente:
  `-webkit-print-color-adjust: exact !important; print-color-adjust: exact !important;`
- **Lange Gedankenstriche** (`&mdash;` / —) in ÜBERSCHRIFTEN durch kurze Bindestriche `-` ersetzen
- **Quellen-Block** unter jeder Checkliste in der `.side-col`:
  ```html
  <div class="source-block">
    <strong>QUELLEN</strong>
    • [Dateiname + Datum + Herkunft]
  </div>
  ```

## Inhalts-Regeln

- **Bauherr vs. Projektsteuerer EXAKT klären** — sind unterschiedliche Rollen, NIEMALS vermischen
- **Beteiligtenliste ist Source of Truth** — nur dort aufgeführte Firmen verwenden
- Bei unklarer Datenlage: User fragen, NICHT raten

## Pre-Flight Checklist

- [ ] Schwarze Banner-Balken einheitlich `#1a1a1a`?
- [ ] Print-CSS hat `print-color-adjust:exact !important`?
- [ ] Quellen-Block (`.source-block`) unter jeder Checkliste vorhanden?
- [ ] Lange Gedankenstriche (—) in Überschriften durch "-" ersetzt?
- [ ] Stand-Datum aktuell?
- [ ] Bauherr / Projektsteuerer / Architekt klar getrennt?
- [ ] Beteiligtenliste vollständig + nur dort aufgeführte Firmen verwendet?
- [ ] Empfehlungen für LPH 2 konkret + priorisiert?
- [ ] Editor komplett? (toggleEdit, installEditTools, addRowToTable, saveDoc, printAsPDF per Grep im Output gefunden; Toolbar-Buttons verweisen darauf)
- [ ] Seitenzahlen? (updatePagination im Script; Testdruck: ab Seite 2 durchgehend nummeriert) + Bilder als data-URI eingebettet?

## Platzhalter-Referenz

| Platzhalter | Beschreibung |
|---|---|
| `{{PROJEKT_TITEL}}` | z.B. "NEUBAU GRUNDSCHULE MIT SPORTHALLE" |
| `{{PROJEKT_NR}}` | Projektnummer |
| `{{PROJEKT_ORT}}` | Ort |
| `{{DATUM}}` | Aktuelles Datum TT.MM.JJJJ |
| `{{AG_NAME}}` | Auftraggeber |
| `{{1_1_AUFGABENSTELLUNG_TEXT}}` | Fließtext Aufgabenstellung |
| `{{1_1_ZIELE_LISTE}}` | HTML-Liste Projektziele |
| `{{1_1_ZIELKONFLIKTE_TABELLE}}` | HTML-Tabelle Zielkonflikte |
| `{{1_2_FACHPLANER_BEDARF}}` | Tabelle Fachplaner-Empfehlung |
| `{{1_2_GENEHMIGUNGSBEDARF_TEXT}}` | Fließtext Genehmigungsbedarf |
| `{{1_3_STANDORT_TABELLE}}` | Tabelle Standort-Bewertung |
| `{{1_3_ERSCHLIESSUNG_TEXT}}` | Fließtext Erschließung |
| `{{1_4_ZUSAMMENFASSUNG_TEXT}}` | Fließtext Zusammenfassung |
| `{{1_4_EMPFEHLUNG_LISTE}}` | Empfehlungen für LPH 2 |
| `{{BETEILIGTE_TABELLE}}` | HTML-Tabelle Beteiligte |
| `{{DATEILISTE}}` | `<li>` Elemente für Sidebar |
| `{{CHECKLISTE_1_X}}` | Checkbox-Items je Kapitel |

## EINGEBAUTER BEARBEITUNGSMODUS (Stand 07.07.2026, verbindlich - Referenz: 4007 LPH2_Report_REFERENZ_4007.html)

Jeder generierte Bericht MUSS den kompletten 4007-Bearbeitungsmodus enthalten. Er steckt bereits im
Template (Toolbar + #toast direkt im Body, Editor-CSS im <style>-Block, Editor-JS im <script> am Ende).

PFLICHT beim Generieren:
1. Template 1:1 als Basis uebernehmen und NUR die {{PLATZHALTER}} ersetzen.
2. Toolbar, <style>-Block und <script>-Block NIEMALS entfernen, kuerzen oder neu schreiben.
3. KEINE hardcodierten contenteditable-Attribute in den Inhalt schreiben - applyEditable() im Editor-JS verwaltet das.
4. IDs/Klassen beibehalten, an denen der Editor haengt: #doc, #file-sidebar, #toast, .toolbar,
   #title-img-input, .sec-heading, .chapter-page, .goal-card, .risk-tag, .sum-row, .gap-note.
5. Falls der Editor fehlt oder beschaedigt ist: aus templates/editor_referenz_4007.html wiederherstellen
   (Abschnitte TOOLBAR / CSS / JS an die markierten Stellen einsetzen).

Funktionsumfang (Abnahmekriterien des Users):
- Toolbar: Bearbeiten/Vorschau (toggleEdit), Titelbild aendern (uploadTitleImage), Speichern (saveDoc
  als Download mit dynamischem Dateinamen), PDF speichern (printAsPDF); Shortcuts Strg+E / Strg+S; #toast-Meldungen.
- Bearbeiten-Modus mit FESTEN Buttons (kein Hover-Verstecken): jede tbody-Zeile hat einen x-Loeschbutton;
  unter jeder Tabelle dauerhaft "+ Zeile hinzufuegen | + Spalte | - letzte Spalte | x Tabelle loeschen";
  Abschnitte (.sec-heading), Kapitel (.chapter-page), Zielkarten (.goal-card), Luecken-Bloecke (.gap-note)
  und Sidebar-Eintraege einzeln loeschbar (mit Sicherheitsabfrage), Sidebar mit "+ Eintrag".
- Status-/Risiko-Kaestchen (.risk-tag): Klick oeffnet Menue mit Farbauswahl (hoch/mittel/gering/offen),
  freiem Text und "Status-Tag entfernen".

SEITENZAHLEN (Pflicht): Das Editor-JS enthaelt updatePagination() - automatische Fusszeilen
"Seite N von M" ab Seite 2 bis zum Ende (Seite 1 = Titelseite ohne Nummer), berechnet aus der realen
A4-quer-Druckgeometrie, plus automatische Seitenzahlen im Inhaltsverzeichnis (.toc-page-num).
NIEMALS entfernen; laeuft bei Load, vor jedem Druck und nach Editor-Aenderungen.

BILDER (Pflicht): Alle Bilder werden als data-URI eingebettet, vorher auf max. 1800px verkleinert
(JPEG ~0.82). NIEMALS relative Links auf Original-Fotos (macht 30+ MB PDFs und bricht beim
Verschieben). Foto-Slots (.photo-slot) und jedes <img> sind im Bearbeiten-Modus per Klick tauschbar
(Upload verkleinert automatisch).

WEITERE EDIT-BEREICHE: "+ Absatz" je Abschnitt, "+ Punkt" je Liste, "+ Zielkarte" im Goal-Grid,
Kapitel duplizieren, Foto-Slots hinzufuegen/loeschen mit editierbarer Bildunterschrift.
