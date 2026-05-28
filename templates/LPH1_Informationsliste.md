# LPH 1 - Grundlagenermittlung - Informationsliste

## Quellen-Heuristik

| Quelle | Wo (relativ zum Projektordner) |
|---|---|
| Aufgabenbeschreibung AG | `01 Grundlagen/Aufgabenbeschreibung*.pdf` oder Bauherr-Anfragen |
| Beteiligtenliste | `00 Aktennotiz/Beteiligte*.xlsx` oder Planfred-Export |
| B-Plan / FNP | `01 Grundlagen/Bebauungsplan*.pdf`, `01 Grundlagen/FNP*.pdf` |
| Liegenschaftskarte / Grundbuch | `01 Grundlagen/Liegenschaft*.pdf`, `Grundbuch*.pdf` |
| Standort-Foto / Ortstermin | `10 Foto/`, `01 Grundlagen/Ortstermin*` |
| Konzeptskizzen | `01 Grundlagen/Skizzen/`, `02 Vorentwurf/Konzept*` |
| Korrespondenz | `13 Mails/`, `00 Aktennotiz/` |
| Vertragsunterlagen | `01 Grundlagen/Vertrag*.pdf`, `01 Grundlagen/Architektenvertrag*.pdf` |
| Gespraechsnotizen | `00 Aktennotiz/*.pdf` oder `*.msg` |

## Such-Patterns (Glob)

| Pattern | Was finden |
|---|---|
| `**/Aufgabenbeschreibung*.pdf` | Aufgabenstellung des AG |
| `**/Beteiligte*.xlsx` | Beteiligtenliste |
| `**/B-Plan*.pdf`, `**/Bebauungsplan*.pdf` | B-Plan |
| `**/FNP*.pdf`, `**/Flaechennutzungsplan*.pdf` | Flaechennutzungsplan |
| `**/Architektenvertrag*.pdf` | Architektenvertrag |
| `**/*Aktennotiz*.pdf`, `**/*Gespraech*.pdf` | Notizen |
| `**/13 Mails/*.msg`, `**/*.eml` | E-Mail-Korrespondenz |

## Pflicht-Kapitel des LPH 1 Berichts (gemaess HOAI Paragraph 33)

| Kapitel | Inhalt | Datenquelle |
|---|---|---|
| 1.1 Aufgabenstellung | Bauherrenwunsch, Projektziele, Zielkonflikte | Aufgabenbeschreibung, Aktennotizen |
| 1.2 Leistungs- und Untersuchungsbedarf | Fachplaner-Empfehlungen, Genehmigungsbedarf, Sondergutachten | Aufgabenbeschreibung + Standort |
| 1.3 Standort-Entscheidungshilfen | B-Plan-Auszug, Erschliessung, Altlasten, Topografie, Denkmalschutz | Liegenschaftsunterlagen |
| 1.4 Zusammenfassung der Ergebnisse | Bewertung der Grundlagen, Empfehlung Naechste Schritte | aus 1.1 bis 1.3 |

## Platzhalter im Template (LPH1_Template.html)

| Platzhalter | Inhalt |
|---|---|
| `{{PROJEKT_TITEL}}` | Projektname (z.B. NEUBAU KINDERGARTEN) |
| `{{PROJEKT_NR}}` | Projektnummer |
| `{{PROJEKT_ORT}}` | Ort |
| `{{DATUM}}` | Aktuelles Datum TT.MM.JJJJ |
| `{{AG_NAME}}` | Auftraggeber |
| `{{TITELBILD_PFAD}}` | Pfad zum Projektbild |
| `{{1_1_AUFGABENSTELLUNG_TEXT}}` | Fliesstext Aufgabenstellung |
| `{{1_1_ZIELE_LISTE}}` | HTML-Liste Projektziele |
| `{{1_1_ZIELKONFLIKTE_TABELLE}}` | HTML-Tabelle Zielkonflikte |
| `{{1_2_FACHPLANER_BEDARF}}` | Tabelle Fachplaner-Empfehlung |
| `{{1_2_GENEHMIGUNGSBEDARF_TEXT}}` | Fliesstext Genehmigungsbedarf |
| `{{1_3_STANDORT_TABELLE}}` | Tabelle Standort-Bewertung |
| `{{1_3_ERSCHLIESSUNG_TEXT}}` | Fliesstext Erschliessung |
| `{{1_4_ZUSAMMENFASSUNG_TEXT}}` | Fliesstext Zusammenfassung |
| `{{1_4_EMPFEHLUNG_LISTE}}` | Empfehlungen fuer LPH 2 |
| `{{BETEILIGTE_TABELLE}}` | HTML-Tabelle Beteiligte |
| `{{ABSCHLUSS_TEXT}}` | Abschlusstext + Freigabevermerk |
| `{{DATEILISTE}}` | `<li>` Elemente fuer Sidebar |
| `{{CHECKLISTE_1_X}}` | Checkbox-Items je Kapitel |

## Risikofaktoren in LPH 1

- **Unklare Aufgabenstellung des AG** -> spaeter teure Aenderungswuensche
- **Fehlende Standort-Pruefung** -> Genehmigungsrisiken erst in LPH 4
- **Unvollstaendige Beteiligtenliste** -> Verzoegerungen in LPH 2/3
- **Fehlende Schadstoffabklaerung bei Bestand** -> Kostentreiber LPH 8
- **Unklare Erschliessung** -> Tiefbau-Risiko in LPH 5
