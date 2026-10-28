# SAP E-Rechnung mit PDFMAILER – Claude-Skill

Ein Skill für [Claude](https://claude.ai), der aus SAP-Rechnungsdaten E-Rechnungsdaten nach EN 16931 erzeugt. Standardweg sind **PDFMAILER-Steuerzeichen** (`@@xi` / `@@xr`), die das SAP-Formular mit ausgibt. PDFMAILER erzeugt daraus XRechnung, ZUGFeRD oder Peppol BIS.

Für Unternehmen, die SAP (ECC oder S/4HANA) zusammen mit PDFMAILER einsetzen.

## Was der Skill kann

- SAP-Daten einlesen: Tabellenauszüge (VBRK, VBRP, VBPA, VBKD, ADRC, T001 …), IDoc INVOIC02, OData oder Screenshots
- Feld-Mapping SAP → BT-Nummer → Steuerzeichen-Parameter erstellen
- Steuerzeichen für eine Rechnung erzeugen, eine Zeile pro Steuerzeichen
- ABAP-Code für das Druckprogramm liefern (Adobe Forms, Smart Forms, SAPscript)
- Auf Wunsch XRechnung- oder ZUGFeRD-XML direkt erzeugen (CII oder UBL)
- Pflichtfelder, Formate und Summen prüfen und Probleme auflisten

## Installation

1. Unter [Releases](../../releases) die Datei `sap-erechnung-pdfmailer.zip` herunterladen, nicht entpacken.
2. In Claude **Customize > Skills** öffnen.
3. Auf **+** klicken, dann **Create skill** und **Upload a skill** wählen.
4. Die ZIP-Datei hochladen und den Skill einschalten.

Voraussetzungen:

- Claude-Konto (Free, Pro, Max, Team oder Enterprise)
- Code-Ausführung aktiviert („Create and edit files“ unter Settings > Capabilities; bei Team/Enterprise durch den Owner)
- PDFMAILER ab Version 9.0.2; einzelne Steuerzeichen ab 10.0.x (siehe Referenz)

Quelle: [Use skills in Claude](https://support.claude.com/en/articles/12512180-use-skills-in-claude). Menünamen können sich ändern.

## Nutzung

Der Skill startet automatisch, wenn eine Anfrage um E-Rechnung, XRechnung, ZUGFeRD oder Steuerzeichen aus SAP geht. Gezielt aufrufen mit `/sap-erechnung-pdfmailer`.

Beispiele:

```text
Hier ein Auszug aus VBRK und VBRP der Rechnung 90012345.
Erzeuge die PDFMAILER-Steuerzeichen für XRechnung.
```

```text
Schreib die ABAP-Klasse, die die Steuerzeichen im Druckprogramm aufbaut, für ECC mit KONV.
```

```text
PDFMAILER meldet BR-DE-15. Hier sind meine Steuerzeichen, was fehlt?
```

Die ausführliche Anleitung mit Workflow, Prüfregeln und typischen Fehlern: [docs/Skill-Anleitung.pdf](docs/Skill-Anleitung.pdf).

## Inhalt des Repositorys

| Pfad | Inhalt |
| --- | --- |
| `skill/sap-erechnung-pdfmailer/SKILL.md` | Anweisungen, Kern-Mapping, Regeln |
| `skill/sap-erechnung-pdfmailer/references/steuerzeichen-referenz.md` | alle Steuerzeichen-Parameter mit BT-Nummer, Format, Pflicht und PDFMAILER-Version |
| `skill/sap-erechnung-pdfmailer/references/abap-beispiel.md` | ABAP-Klasse für das Druckprogramm |
| `skill/sap-erechnung-pdfmailer/examples/` | Steuerzeichen einer vollständigen Beispielrechnung |
| `docs/Skill-Anleitung.pdf` | Anleitung für Anwender |
| `examples/Testrechnung_90012345.pdf` | Testrechnung mit eingebetteten Steuerzeichen für einen Druck über PDFMAILER |
| `examples/Steuerzeichen_Testrechnung_90012345.txt` | dieselben Steuerzeichen zum Abgleich |
| `.github/workflows/release.yml` | baut beim Veröffentlichen eines Releases das Skill-ZIP und hängt es an |

## Testen

1. `examples/Testrechnung_90012345.pdf` über den PDFMAILER-Drucker ausgeben.
2. Die ausgelesenen Steuerzeichen mit `examples/Steuerzeichen_Testrechnung_90012345.txt` abgleichen.
3. Die erzeugte XRechnung mit dem KoSIT-Validator prüfen.

Alle Daten in den Beispielen sind fiktiv.

## Grenzen

- Claude kann die Steuerzeichen nicht selbst durch PDFMAILER schicken; das zeigt erst der Testdruck.
- Das ABAP-Beispiel ist ein Ausgangspunkt und muss im eigenen Testsystem geprüft werden (Konditionsarten, Partnerrollen, Mengeneinheiten).
- Die Ergebnisse ersetzen keine steuerliche oder rechtliche Prüfung.

## Neue Version veröffentlichen

1. Auf GitHub **Releases › Draft a new release** öffnen.
2. Einen neuen Tag anlegen (z. B. `v1.0.1`), Titel und Text aus `CHANGELOG.md` übernehmen.
3. **Publish release** klicken.

Der Workflow packt dann `skill/sap-erechnung-pdfmailer/` als ZIP und hängt es zusammen mit der Anleitung an das Release.

## Lizenz

Noch festzulegen.

## Kontakt

gotomaxx Software GmbH, Hauenstein
