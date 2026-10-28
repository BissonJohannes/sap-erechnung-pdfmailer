# PDFMAILER-Steuerzeichen für E-Rechnungen: Referenz

Grundlage: Steuerzeichen-Liste für PDFMAILER (E-Rechnung). Spalte „Pflicht“ = Pflicht in XRechnung. „ab“ = PDFMAILER-Version, ab der das Steuerzeichen verfügbar ist.

## Syntax

- Einzelwert: `@@xi PARAMETER=Wert@@`
- Tabelle (0..n Zeilen): `@@xr table=TABELLE|PARAM1=Wert|PARAM2=Wert@@`, ein Steuerzeichen pro Tabellenzeile
- Jedes Steuerzeichen auf einer Zeile, ohne Umbruch.
- Betrag: Dezimalkomma, kein Tausenderpunkt (`3803,50`). Menge: `2,0000`. Datum: `TT.MM.JJJJ`. Prozent: `19`.
- In Werten kein `|` und kein `@@`.

Formate: T = Text, C = Code aus Codeliste, B = Betrag, D = Datum, M = Menge, P = Prozentsatz.

## Verkäufer (Rechnungsteller)

| Parameter | BT | Format | Pflicht | ab | Hinweis |
|---|---|---|---|---|---|
| SELLER_NAME | BT-27 | T | ja | 9.0.2 | |
| SELLER_TRADING_NAME | BT-28 | T | | 9.0.2 | Handelsname |
| SELLER_ADDRESS_LINE | BT-35 | T | | 9.0.2 | |
| SELLER_ADDRESS_LINE2 | BT-36 | T | | 9.0.2 | |
| SELLER_ADDRESS_LINE3 | BT-162 | T | | 9.0.2 | |
| SELLER_ADDRESS_ZIP | BT-38 | T | ja | 9.0.2 | |
| SELLER_ADDRESS_CITY | BT-37 | T | ja | 9.0.2 | |
| SELLER_ADDRESS_COUNTRY | BT-40 | C | ja | 9.0.2 | ISO 3166-1 Alpha-2, vorbelegt DE |
| SELLER_ADDRESS_COUNTRY_SUBDIVISION | BT-39 | T | | 10.0.2 | Bundesland |
| SELLER_GLN | BT-29 | T | | 9.0.2 | GLN, Schema 0088 |
| SELLER_ID | BT-29 | T | | 9.0.2 | Lieferantennummer beim Käufer |
| SELLER_TAX_ID | BT-31 | T | * | 9.0.2 | USt-IdNr.; *USt-IdNr. oder Steuernummer ist Pflicht |
| SELLER_TAX_REG_ID | BT-32 | T | * | 9.0.2 | Steuernummer; siehe oben |
| SELLER_ENDPOINT_ID_TYPE | BT-34-1 | C | ja | 9.0.2 | EAS-Codeliste, vorbelegt EM (E-Mail) |
| SELLER_ENDPOINT_ID | BT-34 | T | ja | 9.0.2 | elektronische Adresse |
| SELLER_CONTACT_NAME | BT-41 | T | ja | 9.0.2 | |
| SELLER_CONTACT_PHONE | BT-42 | T | ja | 9.0.2 | |
| SELLER_CONTACT_EMAIL | BT-43 | T | ja | 9.0.2 | |
| SELLER_PAYMENT_ACCOUNT_NAME | BT-85 | T | | 9.0.2 | Kontoinhaber |
| SELLER_IBAN | BT-84 | T | ja | 9.0.2 | ohne Leerzeichen |
| SELLER_BIC | BT-86 | T | ja | 9.0.2 | |
| SELLER_REGULATORY_INFORMATIONS | BT-33 | T | | 9.0.2 | Geschäftsführung, Register, Sitz; je nach Rechtsform rechtlich nötig |

Weitere Bankkonten (Tabelle `SELLER_OTHER_BANKS_DETAILS`, ab 9.0.2):

`@@xr table=SELLER_OTHER_BANKS_DETAILS|SELLER_OTHER_BANKS_ACCOUNT_NAME=|SELLER_OTHER_BANKS_IBAN=|SELLER_OTHER_BANKS_BIC=@@`

## Steuerbevollmächtigter des Verkäufers (ab 9.0.3)

| Parameter | BT | Format |
|---|---|---|
| SELLER_TAXREPRESENTATIVE_NAME | BT-62 | T |
| SELLER_TAXREPRESENTATIVE_ADDRESS_LINE | BT-64 | T |
| SELLER_TAXREPRESENTATIVE_ADDRESS_LINE2 | BT-65 | T |
| SELLER_TAXREPRESENTATIVE_ADDRESS_LINE3 | BT-164 | T |
| SELLER_TAXREPRESENTATIVE_ADDRESS_ZIP | BT-67 | T |
| SELLER_TAXREPRESENTATIVE_ADDRESS_CITY | BT-66 | T |
| SELLER_TAXREPRESENTATIVE_ADDRESS_COUNTRY | BT-69 | C |
| SELLER_TAXREPRESENTATIVE_TAX_ID | BT-63 | T |

## Käufer

| Parameter | BT | Format | Pflicht | ab | Hinweis |
|---|---|---|---|---|---|
| BUYER_NAME | BT-44 | T | ja | 9.0.2 | |
| BUYER_TRADING_NAME | BT-45 | T | | 9.0.2 | |
| BUYER_LEGAL_REGISTRATION_ID | BT-47 | T | | 10.0.4 | Registernummer |
| BUYER_ADDRESS_LINE | BT-50 | T | | 9.0.2 | |
| BUYER_ADDRESS_LINE2 | BT-51 | T | | 9.0.2 | |
| BUYER_ADDRESS_LINE3 | BT-163 | T | | 9.0.2 | |
| BUYER_ADDRESS_ZIP | BT-53 | T | ja | 9.0.2 | |
| BUYER_ADDRESS_CITY | BT-52 | T | ja | 9.0.2 | |
| BUYER_ADDRESS_COUNTRY | BT-55 | C | ja | 9.0.2 | ISO 3166-1 Alpha-2 |
| BUYER_ADDRESS_COUNTRY_SUBDIVISION | BT-54 | T | | 10.0.2 | |
| BUYER_GLN | BT-46 | T | | 9.0.2 | GLN, Schema 0088 |
| BUYER_ID | BT-46 | T | | 9.0.2 | Kundennummer |
| BUYER_TAX_ID | BT-48 | T | | 9.0.2 | USt-IdNr. |
| BUYER_TAX_REGISTRATION_ID | – | T | | 9.0.7 | Steuernummer des Käufers, nur XRechnung (CII) |
| BUYER_ENDPOINT_ID_TYPE | BT-49-1 | C | ja | 9.0.2 | XRechnung: EM; Peppol BIS: Peppol-ID-Schema |
| BUYER_ENDPOINT_ID | BT-49 | T | ja | 9.0.2 | |
| BUYER_CONTACT_NAME | BT-56 | T | | 9.0.2 | |
| BUYER_CONTACT_PHONE | BT-57 | T | | 9.0.2 | |
| BUYER_CONTACT_EMAIL | BT-58 | T | | 9.0.2 | |

## Lieferadresse (Warenempfänger)

| Parameter | BT | Format |
|---|---|---|
| SHIPPING_ADDRESS_NAME | BT-70 | T |
| SHIPPING_ADDRESS_LINE | BT-75 | T |
| SHIPPING_ADDRESS_LINE2 | BT-76 | T |
| SHIPPING_ADDRESS_LINE3 | BT-165 | T |
| SHIPPING_ADDRESS_ZIP | BT-78 | T |
| SHIPPING_ADDRESS_CITY | BT-77 | T |
| SHIPPING_ADDRESS_COUNTRY | BT-80 | C |
| SHIPPING_ADDRESS_GLN | BT-71 | T |
| SHIPPING_ADDRESS_ID | BT-71 | T (nur ZUGFeRD Extended, Comfort) |

Endgültige Lieferadresse (nur ZUGFeRD Extended, ab 10.0.0): ULTIMATE_SHIPPING_ADDRESS_NAME, _LINE, _LINE2, _LINE3, _ZIP, _CITY, _COUNTRY, _GLN, _ID, _TAX_ID, _CONTACT_NAME, _CONTACT_PHONE, _CONTACT_EMAIL (jeweils mit Präfix `ULTIMATE_SHIPPING_ADDRESS`).

Abweichender Rechnungsempfänger (nur ZUGFeRD Extended, ab 9.0.2): INVOICEE_ADDRESS_NAME, _LINE, _LINE2, _LINE3, _ZIP, _CITY, _COUNTRY, _ID, _GLN, _TAX_ID (jeweils mit Präfix `INVOICEE_ADDRESS`).

## Belegdaten

| Parameter | BT | Format | Pflicht | ab | Hinweis |
|---|---|---|---|---|---|
| DOCUMENT_TYPE | BT-3 | C | ja | 9.0.2 | UNTDID 1001: 380 Rechnung, 381 Gutschrift, 384 Korrekturrechnung |
| VOUCHER_ID | BT-1 | T | ja | 9.0.2 | Rechnungsnummer |
| VOUCHER_DATE | BT-2 | D | ja | 9.0.2 | |
| INVOICE_NOTE | BT-22 | T | | 9.0.2 | Freitext |
| INVOICING_PERIOD_START_DATE | BT-73 | D | | 9.0.2 | Leistungszeitraum |
| INVOICING_PERIOD_END_DATE | BT-74 | D | | 9.0.2 | |
| DELIVERY_DATE | BT-72 | D | (§ 14 UStG) | 9.0.2 | Lieferdatum oder Leistungszeitraum ist umsatzsteuerlich Pflicht |
| CURRENCY | BT-5 | C | ja | 9.0.2 | ISO 4217 |
| CURRENCY_VAT | BT-6 | C | | 9.0.3 | Währung der Steuerbuchung |
| VAT_EXEPT_REASON_TEXT | BT-120 | T | * | 9.0.2 | *Pflicht bei Steuerkategorie E |
| DELIVERY_TYPE_CODE | BT-X-145 | C | | 9.0.4 | Incoterms, nur ZUGFeRD Extended |
| VAT_POINT_DATE | BT-7 | D | | 10.0.4 | Steuerfälligkeitsdatum |
| VAT_POINT_DATE_CODE | BT-8 | C | | 10.0.4 | UNTDID 2475 (CII) / 2005 (UBL) |

Weitere Notizen (Tabelle `INVOICE_NOTES`, ab 9.0.3):

`@@xr table=INVOICE_NOTES|INVOICE_NOTES_CONTENT_CODE=|INVOICE_NOTES_CONTENT=|INVOICE_NOTES_SUBJECT_CODE=@@`

INVOICE_NOTES_CONTENT = BT-22, INVOICE_NOTES_SUBJECT_CODE = BT-21 (UNTDID 4451), INVOICE_NOTES_CONTENT_CODE nur ZUGFeRD Extended.

## Referenzen

| Parameter | BT | Format | Pflicht | ab | Hinweis |
|---|---|---|---|---|---|
| ROUTING_ID | BT-10 | T | ja | 9.0.2 | Leitweg-ID bzw. Käuferreferenz |
| BUYER_ORDER_ID | BT-13 | T | | 9.0.2 | Bestellnummer des Käufers |
| BUYER_ORDER_DATE | BT-X-147 | D | | 10.0.1 | nur ZUGFeRD Extended |
| CONTRACT_REFERENCE_ID | BT-12 | T | | 9.0.2 | |
| PROJECT_REFERENCE_ID | BT-11 | T | | 9.0.2 | |
| PROJECT_REFERENCE_NAME | BT-11-0 | T | | 9.0.5 | Pflicht, wenn BT-11 gesetzt (ZUGFeRD) |
| SALES_ORDER_REFERENCE_ID | BT-14 | T | | 9.0.2 | Auftragsnummer |
| SALES_ORDER_REFERENCE_DATE | BT-X-146 | D | | 9.0.3 | nur ZUGFeRD Extended |
| DESPATCH_ADVICE_REFERENCE_ID | BT-16 | T | | 9.0.2 | Lieferavis |
| TENDER_LOT_ID | BT-17 | T | | 9.0.2 | Vergabe- bzw. Losnummer |
| INVOICED_OBJECT_ID | BT-18 | T | | 9.0.2 | |
| INVOICED_OBJECT_ID_SCHEME_ID | BT-18-1 | C | | 10.0.4 | UNTDID 1153 |
| BUYER_ACCOUNTING_REFERENCE | BT-19 | T | | 10.0.2 | |
| PRECEDING_INVOICE_REFERENCE_ID | BT-25 | T | | 9.0.2 | bei Gutschrift/Korrektur |
| PRECEDING_INVOICE_REFERENCE_DATE | BT-26 | D | | 9.0.2 | |
| DELIVERY_NOTE_REFEERENCE_ID | BT-X-202 | T | | 9.0.2 | Lieferscheinnummer, nur ZUGFeRD Extended (Schreibweise wie im Produkt) |
| DELIVERY_NOTE_REFEERENCE_DATE | BT-X-203 | D | | 9.0.2 | nur ZUGFeRD Extended |

Weitere vorhergehende Rechnungen (Tabelle `OTHER_PRECEDING_INVOICES`, nur UBL: XRechnung UBL und Peppol BIS):

`@@xr table=OTHER_PRECEDING_INVOICES|OTHER_PRECEDING_INVOICE_REFERENCE_ID=|OTHER_PRECEDING_INVOICE_REFERENCE_DATE=@@`

## Gesamtbeträge

| Parameter | BT | Format | Pflicht | Hinweis |
|---|---|---|---|---|
| LINE_TOTAL_AMOUNT | BT-106 | B | ja | Summe der Positionsnettobeträge |
| ALLOWANCE_AMOUNT | BT-107 | B | ja | Summe Abschläge, vorbelegt 0 |
| CHARGE_AMOUNT | BT-108 | B | ja | Summe Zuschläge, vorbelegt 0 |
| NET_AMOUNT | BT-109 | B | ja | |
| TAX_AMOUNT | BT-110 | B | ja | |
| TAX_AMOUNT_ACCOUNTING_CURRENCY | BT-111 | B | | ab 9.0.3 |
| GROSS_AMOUNT | BT-112 | B | ja | |
| PREPAID_AMOUNT | BT-113 | B | | |
| DUE_PAYABLE_AMOUNT | BT-115 | B | ja | |

## Zahlungsdaten

| Parameter | BT | Format | Pflicht | Hinweis |
|---|---|---|---|---|
| PAYMENT_REFERENCE | BT-83 | T | | Verwendungszweck |
| PAYMENT_TERMS_DUE_DATE | BT-9 | D | * | *BT-9 oder BT-20 |
| PAYMENT_TERMS_DESCRIPTION | BT-20 | T | ja | |
| PAYMENT_MEANS_TYPE_CODE | BT-81 | C | ja | UNTDID 4461: 58 SEPA-Überweisung, 59 SEPA-Lastschrift, 30 Überweisung |
| PAYMENT_MEANS_TYPE_TEXT | BT-82 | T | | |
| PAYMENT_DIRECTDEBIT_MANDATE_ID | BT-89 | T | | Mandatsreferenz |
| PAYMENT_DIRECTDEBIT_REFERENCE_ID | BT-90 | T | | Gläubiger-ID |
| PAYMENT_DIRECTDEBIT_IBAN | BT-91 | T | | IBAN des Käufers |

Skonto XRechnung (Tabelle `PAYMENT_TERMS_TABLE`):

`@@xr table=PAYMENT_TERMS_TABLE|PAYMENT_TERMS_TABLE_DAYS=14|PAYMENT_TERMS_TABLE_PERCENT=2|PAYMENT_TERMS_TABLE_BASEAMOUNT=4526,17@@`

Skonto ZUGFeRD Extended (Tabelle `PAYMENT_TERMS_TABLE`):

`@@xr table=PAYMENT_TERMS_TABLE|PAYMENT_TERMS_TABLE_DATE=11.11.2026|PAYMENT_TERMS_TABLE_PERCENT=2|PAYMENT_TERMS_TABLE_BASEAMOUNT=4526,17|PAYMENT_TERMS_TABLE_AMOUNT=90,52@@`

## Zu- und Abschläge auf Belegebene (Tabelle `ALLOWANCE_CHARGE`)

`@@xr table=ALLOWANCE_CHARGE|ALLOWANCE_CHARGE_TYPE=|ALLOWANCE_CHARGE_AMOUNT=|ALLOWANCE_CHARGE_REASON=|ALLOWANCE_CHARGE_REASON_CODE=|ALLOWANCE_CHARGE_VAT_CATEGORY=|ALLOWANCE_CHARGE_VAT_PERCENT=|ALLOWANCE_CHARGE_CALCULATION_PERCENT=|ALLOWANCE_CHARGE_BASIS_AMOUNT=@@`

| Parameter | BT | Format | Hinweis |
|---|---|---|---|
| ALLOWANCE_CHARGE_TYPE | – | ALLOWANCE / CHARGE | Abschlag oder Zuschlag |
| ALLOWANCE_CHARGE_AMOUNT | BT-92 / BT-99 | B | |
| ALLOWANCE_CHARGE_REASON | BT-97 / BT-104 | T | |
| ALLOWANCE_CHARGE_REASON_CODE | BT-98 / BT-105 | C | UNTDID 5189 (Abschlag) bzw. 7161 (Zuschlag), ab 10.0.4 |
| ALLOWANCE_CHARGE_CALCULATION_PERCENT | BT-94 / BT-101 | P | ab 10.0.2 |
| ALLOWANCE_CHARGE_BASIS_AMOUNT | BT-93 / BT-100 | B | ab 10.0.2 |
| ALLOWANCE_CHARGE_VAT_CATEGORY | BT-95 / BT-102 | C | UNTDID 5305 |
| ALLOWANCE_CHARGE_VAT_PERCENT | BT-96 / BT-103 | P | |

## Steuersätze (Tabelle `VAT`, Pflicht)

Ein Steuerzeichen je Steuersatz und Steuerkategorie:

`@@xr table=VAT|VAT_PERCENT=19|BASIS_AMOUNT=3803,50|VAT_AMOUNT=722,67|VAT_CATEGORY=S@@`

| Parameter | BT | Format | Hinweis |
|---|---|---|---|
| BASIS_AMOUNT | BT-116 | B | |
| VAT_AMOUNT | BT-117 | B | |
| VAT_CATEGORY | BT-118 | C | UNTDID 5305: S, Z, E, AE, K, G, O, L, M |
| VAT_PERCENT | BT-119 | P | |
| VAT_LINE_TOTAL_BASIS_AMOUNT | BT-X-262 | B | nur ZUGFeRD Extended |

## Anlagen (Tabelle `ATTACH`)

`@@xr table=ATTACH|ATTACH_ID=|ATTACH_NAME=|ATTACH_FILE=|URI=|ATTACH_TYPE=|ATTACH_REFERENCETYPECODE=@@`

| Parameter | BT | Hinweis |
|---|---|---|
| ATTACH_ID | BT-122 | Kennung der Anlage |
| ATTACH_NAME | BT-123 | Beschreibung |
| ATTACH_FILE | BT-125 | Dateipfad der anzuhängenden Datei |
| URI | BT-124 | Link zur externen Datei |
| ATTACH_TYPE | – | Dokumenttyp nach UNTDID 1001 (z. B. 916), XRechnung und Peppol BIS |
| ATTACH_REFERENCETYPECODE | BT-X-32 | XRechnung und Peppol BIS |

## Positionen (Tabelle `ITEM_DETAILS`, Pflicht)

Ein Steuerzeichen je Position. Mindestumfang:

`@@xr table=ITEM_DETAILS|ITEM_LINE_ID=10|ITEM_SELLER_ID=100004711|ITEM_NAME=Hydraulikpumpe HP-250|ITEM_QUANTITY=2,0000|ITEM_PRICE_BASE_QUANTITY=1|ITEM_UNIT=H87|ITEM_GROSS_PRICE=1240,00|ITEM_NET_PRICE=1240,00|ITEM_TOTAL_AMOUNT=2480,00|ITEM_VAT_PERCENT=19|ITEM_VAT_CATEGORY=S@@`

| Parameter | BT | Format | Pflicht | Hinweis |
|---|---|---|---|---|
| ITEM_LINE_ID | BT-126 | T | ja | Positionsnummer |
| ITEM_NAME | BT-153 | T | ja | |
| ITEM_QUANTITY | BT-129 | M | ja | |
| ITEM_PRICE_BASE_QUANTITY | BT-149 | M | ja | Preiseinheit |
| ITEM_UNIT | BT-130 | C | ja | UN/ECE Rec. 20 |
| ITEM_GROSS_PRICE | BT-148 | B | ja | Preis vor Rabatt, netto |
| ITEM_NET_PRICE | BT-146 | B | ja | Preis nach Rabatt, netto |
| ITEM_TOTAL_AMOUNT | BT-131 | B | ja | Positionsnettobetrag |
| ITEM_VAT_PERCENT | BT-152 | P | ja | |
| ITEM_VAT_CATEGORY | BT-151 | C | ja | UNTDID 5305 |
| ITEM_SELLER_ID | BT-155 | T | | eigene Artikelnummer |
| ITEM_BUYER_ID | BT-156 | T | | Artikelnummer des Kunden |
| ITEM_STANDARD_IDENTIFIER | BT-157 | T | | z. B. GTIN |
| ITEM_STANDARD_IDENTIFIER_SCHEME_ID | BT-157-1 | C | | ISO/IEC 6523 (GTIN: 0160) |
| ITEM_DESCRIPTION | BT-154 | T | | |
| ITEM_PRICE_DISCOUNT | BT-147 | B | | Rabatt auf den Preis |
| ITEM_NOTE | BT-127 | T | | |
| ITEM_PERIOD_START_DATE | BT-134 | D | | |
| ITEM_PERIOD_END_DATE | BT-135 | D | | |
| ITEM_DELIVERY_DATE | BT-X-85 | D | | nur ZUGFeRD Extended, ab 10.0.4 |
| ITEM_VAT_EXEPT_REASON_TEXT | BT-X-96 | T | | nur ZUGFeRD Extended, ab 10.0.4 |
| ITEM_ALLOWANCE_AMOUNT | BT-136 | B | | |
| ITEM_ALLOWANCE_REASON | BT-139 | T | | |
| ITEM_ALLOWANCE_REASON_CODE | BT-140 | C | | UNTDID 5189, ab 10.0.4 |
| ITEM_ALLOWANCE_BASIS_AMOUNT | BT-137 | B | | ab 10.0.2 |
| ITEM_ALLOWANCE_CALCULATION_PERCENT | BT-138 | P | | ab 10.0.2 |
| ITEM_CHARGE_AMOUNT | BT-141 | B | | |
| ITEM_CHARGE_REASON | BT-144 | T | | |
| ITEM_CHARGE_REASON_CODE | BT-145 | C | | UNTDID 7161, ab 10.0.4 |
| ITEM_CHARGE_BASIS_AMOUNT | BT-142 | B | | ab 10.0.2 |
| ITEM_CHARGE_CALCULATION_PERCENT | BT-143 | P | | ab 10.0.2 |
| ITEM_OBJECT_IDENTIFIER | BT-128 | T | | |
| ITEM_OBJECT_IDENTIFIER_SCHEME_ID | BT-128-1 | C | | UNTDID 1153, ab 10.0.4 |
| ITEM_ACCOUNTING_ID | BT-133 | T | | Buchungsreferenz, ab 9.0.5 |
| ITEM_ACCOUNTING_CODE | – | C | | Art der Buchungsreferenz, nur ZUGFeRD Extended |
| ITEM_ORIGIN | BT-159 | C | | Herkunftsland, ab 9.0.5 |
| ITEM_REFERENCED_PURCHASE_ORDER_LINE_ID | BT-132 | T | | Bestellposition |
| ITEM_REFERENCED_PURCHASE_ORDER_ID | BT-X-21 | T | | nur ZUGFeRD Extended |
| ITEM_REFERENCED_PURCHASE_ORDER_DATE | BT-X-22-00 | D | | nur ZUGFeRD Extended, ab 10.0.0 |
| ITEM_REFERENCED_DELIVERY_NOTE_ID | BT-X-92 | T | | nur ZUGFeRD Extended |
| ITEM_REFERENCED_DELIVERY_NOTE_LINE_ID | BT-X-93 | T | | nur ZUGFeRD Extended, ab 10.0.4 |
| ITEM_REFERENCED_DELIVERY_NOTE_DATE | BT-X-94 | D | | nur ZUGFeRD Extended |

Positionsattribute (Tabelle `ITEM_ATTR`, ab 9.0.3), Bezug über ITEM_LINE_ID:

`@@xr table=ITEM_ATTR|ITEM_LINE_ID=1|ITEM_ATTR_TYPE=|ITEM_ATTR_DESCRIPTION=Farbe|ITEM_ATTR_VALUE=Rot@@`

ITEM_ATTR_DESCRIPTION = BT-160, ITEM_ATTR_VALUE = BT-161, ITEM_ATTR_TYPE nur ZUGFeRD Extended (UNTDED 6313).

Positionsklassifizierung (Tabelle `ITEM_CLASSIFICATION`, BT-158, ab 10.0.4):

`@@xr table=ITEM_CLASSIFICATION|ITEM_LINE_ID=1|ITEM_CLASSIFICATION_CODE=|ITEM_CLASSIFICATION_SCHEME_ID=|ITEM_CLASSIFICATION_SCHEME_VERSION_ID=@@`

Weitere Positionszuschläge (Tabelle `ITEM_CHARGE`, ab 9.0.5):

`@@xr table=ITEM_CHARGE|ITEM_LINE_ID=1|ITEM_CHARGE_REASON=|ITEM_CHARGE_REASON_CODE=|ITEM_CHARGE_AMOUNT=|ITEM_CHARGE_BASIS_AMOUNT=|ITEM_CHARGE_CALCULATION_PERCENT=@@`

Weitere Positionsabschläge (Tabelle `ITEM_ALLOWANCE`, ab 9.0.5):

`@@xr table=ITEM_ALLOWANCE|ITEM_LINE_ID=1|ITEM_ALLOWANCE_REASON=|ITEM_ALLOWANCE_REASON_CODE=|ITEM_ALLOWANCE_AMOUNT=|ITEM_ALLOWANCE_BASIS_AMOUNT=|ITEM_ALLOWANCE_CALCULATION_PERCENT=@@`
