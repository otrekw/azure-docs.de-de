---
title: Benannte Entitäten für das Gesundheitswesen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 03/11/2021
ms.author: aahi
ms.openlocfilehash: 805c726d33f2050f6f2797c0689069aa5ec4ee71
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2021
ms.locfileid: "104599320"
---
[Text Analytics for Health](../../how-tos/text-analytics-for-health.md) verarbeitet und extrahiert Informationen aus unstrukturierten medizinischen Daten. Der Dienst erkennt medizinische Begriffe, zeigt diese an, weist ihnen Assertionen zu, leitet semantische Beziehungen zwischen ihnen ab und verknüpft sie mit häufigen medizinischen Ontologien.

Gewusst wie: Verwenden von Text Analytics for Health (Vorschauversion) erkennt medizinische Begriffe in den folgenden Kategorien. In dieser Vorschau wird nur englischsprachiger Text unterstützt, und es steht nur eine einzige Modellversion zur Verfügung.

| Category  | BESCHREIBUNG  |
|---------|---------|
| [ANATOMY](#anatomy) | Begriffe, die Informationen zum Körper, anatomischen Systemen, Körperstellen oder -regionen erfassen. |
 | [DEMOGRAPHICS](#demographics) | Begriffe, die Informationen über Geschlecht und Alter erfassen. |
 | [EXAMINATION](#examinations) | Begriffe, die Informationen zu Diagnoseverfahren und Tests erfassen. |
 | [GENERAL ATTRIBUTES](#general-attributes) | Begriffe, die Informationen zu anderen Begriffen der oben genannten Kategorien erfassen. |
 | [GENOMICS](#genomics) | Begriffe, die Informationen zu Genen und Varianten erfassen. |
 | [HEALTHCARE](#healthcare) | Begriffe, die Informationen zu Verwaltungsereignissen, Pflegeumgebungen und Berufen im Gesundheitswesen erfassen. |
 | [MEDICAL CONDITION](#medical-condition) | Begriffe, die Informationen zu Diagnosen, Symptomen oder Anzeichen erfassen. |
 | [MEDICATION](#medication) | Begriffe, die Informationen zu Arzneiverordnungen einschließlich Name, Klasse, Dosierung und Verabreichungsweg erfassen. |
 | [SOCIAL](#social) | Begriffe, die Informationen über medizinisch relevante soziale Aspekte wie z. B. Verwandschaftsverhältnisse erfassen. |
 | [TREATMENT](#treatment) | Begriffe, die Informationen zu Therapieverfahren erfassen. |

Weitere Informationen und Beispiele finden Sie weiter unten.

## <a name="anatomy"></a>Aufbau

### <a name="entities"></a>Entitäten

**BODY_STRUCTURE**: Körpersysteme, anatomische Bereiche und Körperregionen. Beispielsweise „Arm“, „Knie“, „Bauch“, „Nase“, „Leber“, „Kopf“, „Atemsystem“, „Lymphozyten“.

:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure.png" alt-text="Ein Beispiel für die Körperstrukturentität.":::


:::image type="content" source="../../media/ta-for-health/anatomy-entities-body-structure-2.png" alt-text="Ein erweitertes Beispiel für die Körperstrukturentität.":::

## <a name="demographics"></a>Demografische Informationen

### <a name="entities"></a>Entitäten

**AGE**: Alle Bezeichnungen und Ausdrücke in Bezug auf das Alter von Patienten, Familienmitgliedern und anderen. Beispielsweise „40-jährig“, „51 Jahre“, „3 Monate alt“, „erwachsen“, „älterer“, „jung“, „minderjährig“, „mittleren Alters“.

:::image type="content" source="../../media/ta-for-health/age-entity.png" alt-text="Ein Beispiel für eine Altersentität.":::

:::image type="content" source="../../media/ta-for-health/age-entity-2.png" alt-text="Ein weiteres Beispiel für eine Altersentität.":::


**GENDER**: Benennungen, die das Geschlecht der Person angeben. Beispielsweise „männlich“, „weiblich“, „Frau“, „Herr“, „Dame“.

:::image type="content" source="../../media/ta-for-health/gender-entity.png" alt-text="Ein Beispiel für eine Geschlechtsentität.":::

## <a name="examinations"></a>Untersuchungen

### <a name="entities"></a>Entitäten

**EXAMINATION_NAME**: Diagnoseverfahren und Tests einschließlich Vitalzeichen und Körpermaße. Beispielsweise „MRT“, „EKG“, „HIV-Test“, „Hämoglobin“, „Blutplättchenanzahl“, „Skalensysteme“ wie *Bristol-Stuhlformen-Skala*.

:::image type="content" source="../../media/ta-for-health/exam-name-entities.png" alt-text="Ein Beispiel für eine Untersuchungsentität.":::

:::image type="content" source="../../media/ta-for-health/exam-name-entities-2.png" alt-text="Ein weiteres Beispiel für eine Untersuchungsnamenentität.":::

## <a name="general-attributes"></a>Allgemeine Attribute

### <a name="entities"></a>Entitäten

**DATE**: Vollständiges Datum einer Erkrankung, Untersuchung, Behandlung, Arzneiverordnung oder eines Verwaltungsereignisses.

**DIRECTION**: Begriffe zur lokalen Bestimmung im Zusammenhang mit dem Körperbau, einer Erkrankung, Untersuchung oder Behandlung. Beispiel: links, seitlich, oberhalb von, hinter.

**FREQUENCY**: Beschreibt, wie häufig eine Erkrankung, Untersuchung, Behandlung oder Arzneiverordnung stattfand, stattfindet oder stattfinden sollte.

**MEASUREMENT_VALUE**: Messwert im Zusammenhang mit einer Untersuchung oder einer Erkrankung.

**MEASUREMENT_UNIT**: Einheit des Messwerts im Zusammenhang mit einer Untersuchung oder einer Erkrankung.

**RELATIONAL_OPERATOR**: Ausdrücke für die quantitative Beziehung zwischen einer Entität und zusätzlichen Informationen.

**TIME**: Zeitliche Begriffe zur Angabe des Beginns und/oder der Dauer einer Erkrankung, Untersuchung, Behandlung, Arzneiverordnung oder eines Verwaltungsereignisses. 

## <a name="genomics"></a>Genomics

### <a name="entities"></a>Entitäten

**GENE_OR_PROTEIN**: Alle Erwähnungen von Namen und Symbolen menschlicher Gene sowie von Chromosomen und Chromosomen-/Proteinteilen. Beispiel: „MTRR“, „F2“.

:::image type="content" source="../../media/ta-for-health/genomics-entities.png" alt-text="Ein Beispiel für eine Genentität.":::

**VARIANT**: Alle Erwähnungen von Genvarianten und -mutationen. Beispiel: `c.524C>T`, `(MTRR):r.1462_1557del96`.
  
## <a name="healthcare"></a>Integritätsressourcen

### <a name="entities"></a>Entitäten
  
**ADMINISTRATIVE_EVENT**: Ereignisse in Bezug auf das Gesundheitswesen, aber (teilweise) administrativ. Beispielsweise „Registrierung“, „Zulassung“, „Testversion“, „Studieneintrag“, „Überweisung“, „Entlassung“, „Krankenhauseinweisung“, „Krankenhausaufenthalt“. 

:::image type="content" source="../../media/ta-for-health/healthcare-event-entity.png" alt-text="Ein Beispiel für eine Ereignisentität im Gesundheitswesen.":::

**CARE_ENVIRONMENT**: Eine Umgebung oder ein Ort, wo Patienten versorgt werden. Beispielsweise „Notaufnahme“, „Arztpraxis“, „kardiologische Station“, „Hospiz“, „Krankenhaus“.

:::image type="content" source="../../media/ta-for-health/healthcare-environment-entity.png" alt-text="Screenshot: Beispiel für eine Umgebungsentität im Gesundheitswesen":::

**HEALTHCARE_PROFESSION**: Praktizierender im Gesundheitswesen. Beispielsweise „Zahnarzt“, „Pathologe“, „Neurologe“, „Radiologe“, „Apotheker“, „Ernährungsberater“, „Physiotherapeut“, „Chiropraktiker“.

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity.png" alt-text="Screenshot: Weiteres Beispiel für eine Umgebungsentität im Gesundheitswesen":::

:::image type="content" source="../../media/ta-for-health/healthcare-profession-entity-2.png" alt-text="Ein weiteres Beispiel für eine Umgebungsentität im Gesundheitswesen.":::

## <a name="medical-condition"></a>Medizinischer Zustand

### <a name="entities"></a>Entitäten

**DIAGNOSIS**: Krankheit, Syndrom, Vergiftung. Beispielsweise „Brustkrebs“, „Alzheimer“, „Bluthochdruck“, „Herzinsuffizienz“, „Wirbelsäulenverletzung“.

:::image type="content" source="../../media/ta-for-health/medical-condition-entity.png" alt-text="Ein Beispiel für eine Entität für den medizinischen Zustand.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-entity-2.png" alt-text="Ein weiteres Beispiel für eine Entität für den medizinischen Zustand.":::

**SYMPTOM_OR_SIGN**: Subjektiver oder objektiver Nachweis von Krankheiten oder sonstige Diagnosen. Beispielsweise „Brustschmerzen“, „Kopfschmerzen“, „Schwindel“, „Effloreszenz“, „Dyspnoe“, „gute Darmgeräusche“, „gut genährt“.

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity.png" alt-text="Ein Beispiel für eine Anzeichen- oder Symptomentität für den medizinischen Zustand.":::

:::image type="content" source="../../media/ta-for-health/medical-condition-symptom-entity-2.png" alt-text="Ein weiteres Beispiel für eine Anzeichen- oder Symptomentität für den medizinischen Zustand.":::

**CONDITION_QUALIFIER**: Qualitative Angaben zur Beschreibung eines medizinischen Zustands oder einer Erkrankung. Alle folgenden Unterkategorien gelten als Qualifizierer:

1.  Zeitbezogene Ausdrücke: Dies sind Benennungen, die die Zeitdimension qualitativ beschreiben, z. B. „plötzlich“, „akut“, „chronisch“, „langjährig“. 
2.  Qualitätsausdrücke:  Dabei handelt es sich um Benennungen, die die „Art“ des medizinischen Zustands beschreiben, z. B. „brennen“, „scharf“.
3.  Schweregradausdrücke: „schwerwiegend“, „mild“, „etwas“, „unkontrolliert“.
4.  Ausdehnungsausdrücke: „lokal“, „zentriert“, „diffus“.
5.  Ausstrahlungsausdrücke: „strahlt aus“, „Ausstrahlung“.
6.  Zustandsskala: In einigen Fällen ist ein Zustand durch eine Skala gekennzeichnet, bei der es sich um eine endliche geordnete Liste von Werten handelt. Beispielsweise „Patienten mit Bauchspeicheldrüsenkrebs im Stadium III“.
7.  Zustandsverlauf: Eine Benennung, die sich auf den Verlauf oder Fortschritt eines Zustands bezieht, z. B. „Verbesserung“, „Verschlechterung“, „Überwinden“, „Nachlassen“. 

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis.png" alt-text="Ein Beispiel für ein Zustandsqualifiziererattribut und eine Diagnoseentität.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-2.png" alt-text="Ein weiteres Beispiel für ein Zustandsqualifiziererattribut und eine Diagnoseentität.":::

:::image type="content" source="../../media/ta-for-health/conditional-qualifier-symptom-medication.png" alt-text="Ein Beispiel für ein Zustandsqualifiziererattribut mit Symptom- und Medikamententitäten.":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-diagnosis-3.png" alt-text="Screenshot: Weiteres Beispiel für ein Zustandsqualifiziererattribut mit einer Diagnoseentität":::

:::image type="content" source="../../media/ta-for-health/condition-qualifier-symptom.png" alt-text="Screenshot: Weiteres Beispiel für ein Zustandsqualifiziererattribut mit einer Diagnoseentität":::

## <a name="medication"></a>Medikament

### <a name="entities"></a>Entitäten

**MEDICATION_CLASS**: Eine Gruppe von Medikamenten, die einen ähnlichen Aktionsmechanismus, einen verwandten Aktionsmodus, eine ähnliche chemische Struktur haben und/oder zur Behandlung derselben Krankheit verwenden. Beispielsweise „ACE-Hemmer“, „Opioid“, „Antibiotika“, „Mittel zur Schmerzlinderung“.

:::image type="content" source="../../media/ta-for-health/medication-entities-class.png" alt-text="Ein Beispiel für eine Medikamentenklassenentität.":::

**MEDICATION_NAME**: Erwähnungen von Medikamenten einschließlich urheberrechtlich geschützter Markennamen und Namen, die keine Markennamen sind. Beispiel: Ibuprofen.

:::image type="content" source="../../media/ta-for-health/medication-entities-name.png" alt-text="Ein Beispiel für eine Medikamentennamenentität.":::

**DOSAGE**: Menge der bestellten Medikamente. Beispielsweise „*1.000 ml* Natriumchlorid-Infusionslösung“.

:::image type="content" source="../../media/ta-for-health/medication-dosage.png" alt-text="Ein Beispiel für ein Medikamentdosierungsattribut.":::

**MEDICATION_FORM**: Die Form des Medikaments. Beispielsweise „Lösung“, „Pille“, „Kapsel“, „Tablette“, „Pflaster“, „Gel“, „Paste“, „Schaum“, „Spray“, „Drops“, „Creme“, „Sirup“.

:::image type="content" source="../../media/ta-for-health/medication-form.png" alt-text="Ein Beispiel für ein Medikamentenformattribut.":::

**MEDICATION_ROUTE**: Die Methode der Medikamenteneinnahme. Beispielsweise „oral“, „vaginal“, „IV“, „epidural“, „inhalieren“.

:::image type="content" source="../../media/ta-for-health/medication-route.png" alt-text="Ein Beispiel für ein Attribut für die Methode der Medikamenteneinnahme.":::

## <a name="social"></a>Soziale Netzwerke

### <a name="entities"></a>Entitäten

**FAMILY_RELATION**: Erwähnungen von Familienangehörigen des Patienten. Beispielsweise „Vater“, „Tochter“, „Geschwister“, „Eltern“.

:::image type="content" source="../../media/ta-for-health/family-relation.png" alt-text="Screenshot: Weiteres Beispiel für ein Behandlungszeitattribut":::

## <a name="treatment"></a>Behandlung

### <a name="entities"></a>Entitäten

**TREATMENT_NAME**: Therapeutische Verfahren. Beispielsweise „Knieprothesenchirurgie“, „Knochenmarktransplantation“, „TAVI“, „Diät“.

:::image type="content" source="../../media/ta-for-health/treatment-entities-name.png" alt-text="Ein Beispiel für eine Behandlungsnamenentität.":::
