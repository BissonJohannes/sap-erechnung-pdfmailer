---
name: sap-erechnung-pdfmailer
description: Erstellt aus SAP-Rechnungsdaten (VBRK/VBRP, IDoc, OData) E-Rechnungsdaten nach EN 16931 als PDFMAILER-Steuerzeichen (@@xi/@@xr) oder als XRechnung/ZUGFeRD-XML (CII/UBL), mit Feld-Mapping, ABAP-Beispiel und Pflichtfeldprüfung. Nutzen bei E-Rechnung, XRechnung, ZUGFeRD oder Steuerzeichen aus SAP.
---

# SAP E-Rechnung mit PDFMAILER

Erstellt und strukturiert E-Rechnungsdaten (XRechnung, ZUGFeRD, Peppol BIS) aus SAP-Rohdaten.

Standardweg: Das SAP-Formular (Adobe Forms, Smart Forms oder SAPscript) gibt **PDFMAILER-Steuerzeichen** mit aus. PDFMAILER liest sie beim Druck und erzeugt daraus die E-Rechnung. Direktes XML nur, wenn der Nutzer es ausdrücklich verlangt.

## Mitgelieferte Dateien

- `references/steuerzeichen-referenz.md`: alle Steuerzeichen-Parameter mit BT-Nummer, Format, Pflicht in XRechnung und PDFMAILER-Version. **Vor jeder Ausgabe lesen** und die Parameternamen exakt so verwenden.
- `references/abap-beispiel.md`: ABAP-Klasse, die die Steuerzeichen im Druckprogramm aufbaut. Als Ausgangspunkt verwenden, wenn ABAP-Code gewünscht ist.
- `examples/testrechnung-90012345-steuerzeichen.txt`: vollständiges, rechnerisch geprüftes Beispiel (eine Rechnung, vier Positionen, 19 %). Als Muster für Aufbau und Formate nutzen.

## Aufgaben

1. SAP-Daten einlesen: Tabellenauszüge (VBRK, VBRP, VBPA, VBKD, ADRC, ADR6, T001, T012K, T052U, KONV bzw. PRCD_ELEMENTS), IDoc INVOIC02, OData oder Screenshots.
2. Felder auf EN-16931-Business-Terms (BT/BG) mappen.
3. Ausgabe erzeugen:
   - **Steuerzeichen** (Standard): `@@xi PARAMETER=Wert@@` für Einzelwerte, `@@xr table=NAME|P1=…|P2=…@@` für Tabellen (ITEM_DETAILS, VAT, ALLOWANCE_CHARGE, PAYMENT_TERMS_TABLE, ATTACH …).
   - **XML** (nur auf Wunsch): CII D16B oder UBL 2.1, CustomizationID `urn:cen.eu:en16931:2017#compliant#urn:xeinkauf.de:kosit:xrechnung_3.0`.
4. Bei Bedarf ABAP-Code liefern (Basis: `references/abap-beispiel.md`).

## Kern-Mapping SAP → Steuerzeichen

| BT | Parameter | SAP-Quelle |
|---|---|---|
| BT-1 | VOUCHER_ID | VBRK-VBELN (ALPHA = OUT) |
| BT-2 | VOUCHER_DATE | VBRK-FKDAT |
| BT-3 | DOCUMENT_TYPE | VBRK-FKART → 380 Rechnung, 381 Gutschrift |
| BT-5 | CURRENCY | VBRK-WAERK |
| BT-10 | ROUTING_ID | Leitweg-ID (öffentlicher Auftraggeber) bzw. vom Kunden vorgegebene Käuferreferenz |
| BT-13 | BUYER_ORDER_ID | VBKD-BSTKD |
| BT-16 | DESPATCH_ADVICE_REFERENCE_ID | VBRP-VGBEL |
| BT-72 | DELIVERY_DATE | VBRP-FBUDA bzw. LIKP-WADAT_IST |
| BT-27 bis 40 | SELLER_* | VBRK-BUKRS → T001-ADRNR → ADRC |
| BT-31 | SELLER_TAX_ID | T001-STCEG |
| BT-32 | SELLER_TAX_REG_ID | T001Z bzw. Standardtext |
| BT-34 | SELLER_ENDPOINT_ID(_TYPE) | ADR6 zur Firmenadresse, Typ EM |
| BT-41 bis 43 | SELLER_CONTACT_* | VBPA (Sachbearbeiter) → ADRP/ADR2/ADR6 |
| BT-84 bis 86 | SELLER_IBAN, SELLER_BIC, SELLER_PAYMENT_ACCOUNT_NAME | T012K/TIBAN, BNKA |
| BT-44 bis 55 | BUYER_* | VBPA Rolle RE → ADRC |
| BT-46 | BUYER_ID | VBRK-KUNRG |
| BT-48 | BUYER_TAX_ID | VBRK-STCEG |
| BT-49 | BUYER_ENDPOINT_ID(_TYPE) | ADR6 zum RE-Partner |
| BT-9 / BT-20 | PAYMENT_TERMS_DUE_DATE / PAYMENT_TERMS_DESCRIPTION | VBRK-ZTERM → T052/T052U |
| BT-81 | PAYMENT_MEANS_TYPE_CODE | VBRK-ZLSCH → 58 SEPA-Überweisung, 59 SEPA-Lastschrift |
| BT-106 bis 115 | LINE_TOTAL_AMOUNT, NET_AMOUNT, TAX_AMOUNT, GROSS_AMOUNT, DUE_PAYABLE_AMOUNT | Summe VBRP-NETWR, VBRK-NETWR, VBRK-MWSBK |
| BG-23 | @@xr table=VAT | Steuerkonditionen (KOAID 'D') je Satz und Kategorie |
| BG-25 | @@xr table=ITEM_DETAILS | VBRP + Preiskondition (KBETR/KPEIN) |
| BT-130 | ITEM_UNIT | VBRP-VRKME → eigene Mappingtabelle auf UN/ECE Rec. 20 |

## Regeln

- Nur gültige Syntax für XRechnung, das ZUGFeRD-Profil (EN 16931/Comfort, Extended) bzw. exakt die Steuerzeichen-Syntax aus der Referenz.
- Jedes Steuerzeichen steht auf **einer** Zeile. Im Formular darf es nicht umbrechen, sonst kann PDFMAILER es nicht lesen.
- Rechtsrahmen Deutschland: Empfangspflicht seit 01.01.2025; Ausstellungspflicht B2B ab 01.01.2027 (Vorjahresumsatz über 800.000 €), ab 01.01.2028 für alle. § 14 UStG: Leistungsdatum oder Leistungszeitraum ist Pflicht.
- XRechnung-Pflichtfelder immer befüllen: BT-10, BT-34, BT-49, BT-41/42/43, BT-9 oder BT-20, Zahlungsanweisungen (BG-16), USt-IdNr. oder Steuernummer des Verkäufers.
- Formate: Beträge mit Dezimalkomma, ohne Tausenderpunkt (3803,50); Mengen mit vier Nachkommastellen (2,0000); Datum TT.MM.JJJJ; IBAN ohne Leerzeichen; Länder ISO 3166 Alpha-2; Währung ISO 4217.
- Mengeneinheiten nie ungeprüft aus T006-ISOCODE übernehmen (z. B. ist PCE ungültig), sondern auf Rec. 20 mappen (H87/C62, HUR, LTR, KGM, MTR …).
- Steuerkategorie: S bei 19 % und 7 %; AE (Reverse Charge), K (innergemeinschaftliche Lieferung), E/Z jeweils mit VAT_EXEPT_REASON_TEXT.
- ECC: KONV-KBETR steht bei Prozentkonditionen in Promille (19 % = 190,00). S/4HANA: Belegung von PRCD_ELEMENTS-KBETR im System prüfen.
- Texte (ARKTX, Langtexte) bereinigen: `|` und `@@` ersetzen, sonst brechen die Steuerzeichen.
- Rechenprüfung vor der Ausgabe: Summe der Positionen = BT-106; Steuer je Satz = Basis × Satz / 100, kaufmännisch gerundet; BT-112 = BT-109 + BT-110; BT-115 = BT-112 − BT-113.
- Wenn möglich gegen den KoSIT-Validator (XRechnung-Konfiguration) prüfen. Ist das nicht möglich, offen sagen, dass nur Pflichtfelder und Summen geprüft wurden.
- Für Tests nur fiktive oder anonymisierte Daten verwenden.

## Ausgabe

1. Kurzes Mapping (Tabelle) der verwendeten Felder.
2. Steuerzeichen bzw. XML als sauberer Code-Block.
3. Bei Bedarf ABAP-Snippet als Code-Block.
4. Liste der gefundenen Probleme: fehlende Pflichtfelder, Rundung, Codelisten, offene Annahmen (z. B. welche Käuferreferenz in BT-10 steht).
