# ABAP-Beispiel: Steuerzeichen im Druckprogramm aufbauen

Ausgangspunkt für eigene Implementierungen, ab ABAP 7.40. Nicht ungeprüft produktiv einsetzen: Konditionsarten (PR00, Steuer), Partnerrollen und Mengeneinheiten an das eigene System anpassen und im Testsystem prüfen.

Die Klasse liefert eine String-Tabelle. Sie wird an die Formularschnittstelle übergeben und im Formular als wiederholtes, einzeiliges Textfeld ausgegeben (ohne automatischen Umbruch).

```abap
CLASS lcl_xinv DEFINITION FINAL.
  PUBLIC SECTION.
    TYPES tt_lines TYPE STANDARD TABLE OF string WITH EMPTY KEY.
    TYPES tt_vbrp  TYPE STANDARD TABLE OF vbrp   WITH EMPTY KEY.

    METHODS build
      IMPORTING is_vbrk         TYPE vbrk
                it_vbrp         TYPE tt_vbrp
      RETURNING VALUE(rt_lines) TYPE tt_lines.

  PRIVATE SECTION.
    TYPES: BEGIN OF ty_vat,
             percent TYPE p LENGTH 5 DECIMALS 2,
             basis   TYPE p LENGTH 15 DECIMALS 2,
             amount  TYPE p LENGTH 15 DECIMALS 2,
           END OF ty_vat.
    DATA mt_lines TYPE tt_lines.
    DATA mt_vat   TYPE SORTED TABLE OF ty_vat WITH UNIQUE KEY percent.

    METHODS xi   IMPORTING iv_name TYPE string iv_value TYPE string.
    METHODS amt  IMPORTING iv TYPE p RETURNING VALUE(rv) TYPE string.
    METHODS qty  IMPORTING iv TYPE p RETURNING VALUE(rv) TYPE string.
    METHODS dat  IMPORTING iv TYPE d RETURNING VALUE(rv) TYPE string.
    METHODS txt  IMPORTING iv TYPE csequence RETURNING VALUE(rv) TYPE string.
    METHODS unit IMPORTING iv TYPE vrkme RETURNING VALUE(rv) TYPE string.
ENDCLASS.

CLASS lcl_xinv IMPLEMENTATION.

  METHOD build.
    CLEAR: mt_lines, mt_vat.

    " ---- Kopf ----------------------------------------------------------
    xi( iv_name = `DOCUMENT_TYPE` iv_value = `380` ).   " Gutschrift-Fakturaarten: 381
    xi( iv_name = `VOUCHER_ID`    iv_value = |{ is_vbrk-vbeln ALPHA = OUT }| ).
    xi( iv_name = `VOUCHER_DATE`  iv_value = dat( is_vbrk-fkdat ) ).
    xi( iv_name = `CURRENCY`      iv_value = CONV #( is_vbrk-waerk ) ).
    xi( iv_name = `BUYER_ID`      iv_value = |{ is_vbrk-kunrg ALPHA = OUT }| ).
    xi( iv_name = `BUYER_TAX_ID`  iv_value = CONV #( is_vbrk-stceg ) ).
    " ... Verkäufer (T001/ADRC), Käufer (VBPA RE -> ADRC/ADR6), Kontakt,
    "     Bank (T012K/TIBAN), BT-10, Zahlungsbedingung (T052U) analog

    " ---- Positionen ----------------------------------------------------
    LOOP AT it_vbrp INTO DATA(ls_p).
      " Preis: S/4HANA PRCD_ELEMENTS, ECC KONV (gleiche Felder)
      SELECT SINGLE kbetr, kpein FROM prcd_elements
        WHERE knumv = @is_vbrk-knumv AND kposn = @ls_p-posnr
          AND kschl = 'PR00' AND kinak = @space
        INTO @DATA(ls_price).

      " Steuersatz aus der Steuerkondition (KOAID 'D')
      " ECC/KONV: Prozent in Promille -> durch 10 teilen. S/4: im System prüfen.
      SELECT SINGLE kbetr FROM prcd_elements
        WHERE knumv = @is_vbrk-knumv AND kposn = @ls_p-posnr
          AND koaid = 'D' AND kinak = @space
        INTO @DATA(lv_rate).
      DATA(lv_pct) = CONV ty_vat-percent( lv_rate ).

      DATA(lv_cat) = COND string( WHEN lv_pct > 0 THEN `S` ELSE `AE` ). " 0 %: Kategorie fachlich klären

      APPEND |@@xr table=ITEM_DETAILS| &&
             |\|ITEM_LINE_ID={ ls_p-posnr ALPHA = OUT }| &&
             |\|ITEM_SELLER_ID={ txt( |{ ls_p-matnr ALPHA = OUT }| ) }| &&
             |\|ITEM_NAME={ txt( ls_p-arktx ) }| &&
             |\|ITEM_QUANTITY={ qty( ls_p-fkimg ) }| &&
             |\|ITEM_PRICE_BASE_QUANTITY={ ls_price-kpein }| &&
             |\|ITEM_UNIT={ unit( ls_p-vrkme ) }| &&
             |\|ITEM_GROSS_PRICE={ amt( ls_price-kbetr ) }| &&
             |\|ITEM_NET_PRICE={ amt( ls_price-kbetr ) }| &&
             |\|ITEM_TOTAL_AMOUNT={ amt( ls_p-netwr ) }| &&
             |\|ITEM_VAT_PERCENT={ lv_pct DECIMALS = 0 }| &&
             |\|ITEM_VAT_CATEGORY={ lv_cat }@@|
        TO mt_lines.

      " Steuer je Satz sammeln
      READ TABLE mt_vat ASSIGNING FIELD-SYMBOL(<ls_vat>) WITH TABLE KEY percent = lv_pct.
      IF sy-subrc <> 0.
        INSERT VALUE #( percent = lv_pct ) INTO TABLE mt_vat ASSIGNING <ls_vat>.
      ENDIF.
      <ls_vat>-basis = <ls_vat>-basis + ls_p-netwr.
    ENDLOOP.

    " ---- Steuersätze (Steuer pro Satz runden, nicht pro Position) -------
    LOOP AT mt_vat ASSIGNING <ls_vat>.
      <ls_vat>-amount = round( val = <ls_vat>-basis * <ls_vat>-percent / 100 dec = 2 ).
      APPEND |@@xr table=VAT\|VAT_PERCENT={ <ls_vat>-percent DECIMALS = 0 }| &&
             |\|BASIS_AMOUNT={ amt( <ls_vat>-basis ) }| &&
             |\|VAT_AMOUNT={ amt( <ls_vat>-amount ) }| &&
             |\|VAT_CATEGORY={ COND string( WHEN <ls_vat>-percent > 0 THEN `S` ELSE `AE` ) }@@|
        TO mt_lines.
    ENDLOOP.

    " ---- Summen --------------------------------------------------------
    DATA(lv_gross) = CONV ty_vat-basis( is_vbrk-netwr + is_vbrk-mwsbk ).
    xi( iv_name = `LINE_TOTAL_AMOUNT`  iv_value = amt( is_vbrk-netwr ) ).
    xi( iv_name = `ALLOWANCE_AMOUNT`   iv_value = `0,00` ).
    xi( iv_name = `CHARGE_AMOUNT`      iv_value = `0,00` ).
    xi( iv_name = `NET_AMOUNT`         iv_value = amt( is_vbrk-netwr ) ).
    xi( iv_name = `TAX_AMOUNT`         iv_value = amt( is_vbrk-mwsbk ) ).
    xi( iv_name = `GROSS_AMOUNT`       iv_value = amt( lv_gross ) ).
    xi( iv_name = `DUE_PAYABLE_AMOUNT` iv_value = amt( lv_gross ) ).

    rt_lines = mt_lines.
  ENDMETHOD.

  METHOD xi.
    IF iv_value IS NOT INITIAL.
      APPEND |@@xi { iv_name }={ txt( iv_value ) }@@| TO mt_lines.
    ENDIF.
  ENDMETHOD.

  METHOD amt.   " 3803.5 -> 3803,50 (ohne Tausenderpunkt)
    rv = |{ iv DECIMALS = 2 NUMBER = RAW }|.
    REPLACE '.' IN rv WITH ','.
  ENDMETHOD.

  METHOD qty.   " 2 -> 2,0000
    rv = |{ iv DECIMALS = 4 NUMBER = RAW }|.
    REPLACE '.' IN rv WITH ','.
  ENDMETHOD.

  METHOD dat.   " 20261028 -> 28.10.2026
    rv = |{ iv+6(2) }.{ iv+4(2) }.{ iv(4) }|.
  ENDMETHOD.

  METHOD txt.   " Trennzeichen der Steuerzeichen neutralisieren
    rv = iv.
    REPLACE ALL OCCURRENCES OF '|'  IN rv WITH '/'.
    REPLACE ALL OCCURRENCES OF '@@' IN rv WITH '@'.
  ENDMETHOD.

  METHOD unit.  " UN/ECE Rec. 20 - besser als Z-Tabelle pflegen
    rv = SWITCH #( iv WHEN 'ST' THEN `H87` WHEN 'H'  THEN `HUR`
                      WHEN 'L'  THEN `LTR` WHEN 'KG' THEN `KGM`
                      WHEN 'M'  THEN `MTR` ELSE `C62` ).
  ENDMETHOD.

ENDCLASS.
```

## Hinweise

- **Sprachabhängige Mengeneinheiten:** `VRKME` ist das interne Format. Je nach Anmeldesprache sieht man außen andere Kürzel (z. B. PC statt ST). Das Mapping deshalb auf das interne Format beziehen.
- **Steuer pro Satz runden:** Die Steuer wird je Steuersatz aus der Summe berechnet und kaufmännisch gerundet. Weicht `VBRK-MWSBK` davon um Cent ab, die Rundungslogik im Preisschema prüfen.
- **ECC statt S/4HANA:** `PRCD_ELEMENTS` durch `KONV` ersetzen. Dort stehen Prozentkonditionen in Promille (19 % = 190,00).
- **Ausgabe im Formular:** Die Tabelle als Teilformular mit einem Textfeld pro Zeile ausgeben, Feldbreite so, dass nicht umbrochen wird; ggf. kleine Schrift oder ausgeblendete Ausgabe, wie für PDFMAILER-Steuerzeichen üblich.
