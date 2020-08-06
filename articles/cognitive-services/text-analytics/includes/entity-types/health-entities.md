---
title: Benannte Entitäten für das Gesundheitswesen
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 07/28/2020
ms.author: aahi
ms.openlocfilehash: 4ac65e85e05f408b8d2f37a1d6845dc9e28e2bab
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87373068"
---
## <a name="health-entity-categories"></a>Gesundheitsentitätskategorien:

Die folgenden Entitätskategorien werden von [Textanalyse für Gesundheit](../../how-tos/text-analytics-for-health.md) zurückgegeben.  Beachten Sie, dass in dieser Containervorschau nur englischer Text unterstützt wird und nur eine einzige Modellversion in jedem Containerimage bereitgestellt wird.

### <a name="named-entity-recognition"></a>Erkennung benannter Entitäten

|Category  |BESCHREIBUNG   |
|----------|--------------|
| AGE | Altersangaben |
| BODY_STRUCTURE | Teile des menschlichen Körpers einschließlich der Organe und anderer Strukturen. | 
| CONDITION_QUALIFIER | Bedingungsebenen wie *mild*, *extended* (erweitert) oder *diffuse* (diffus). | 
| DIAGNOSIS | Medizinische Bedingungen. Beispiel: *hypertension* (Hypertonie). | 
| DIRECTION | Richtungen wie *left* (links) oder *anterior*. | 
| DOSAGE | Höhe einer Medikation.  | 
| EXAMINATION_NAME | Eine Untersuchungsmethode. | 
| EXAMINATION_RELATION | Ein Zusammenhang einer Maßeinheit und einer Untersuchung.  | 
| EXAMINATION_UNIT | Eine Maßeinheit für eine Untersuchung. | 
| EXAMINATION_UNIT | Der Wert der Maßeinheit für eine Untersuchung. | 
| FAMILY_RELATION | Eine familiäre Beziehung, z. b. *sister* (Schwester).  | 
| FREQUENCY (Häufigkeit) | Häufigkeit.   | 
| GENDER | Geschlechter. | 
| GENE | Eine Genentität, z. B. *TP53*.   | 
| MEDICATION_CLASS | Medikationsklassen. Beispiel: *antibiotics* (Antibiotika).  | 
| MEDICATION_NAME  | Generische und Markenmedikationen.| 
| ROUTE_OR_MODE  | Methode zum Verwalten der Medikation. | 
| SYMPTOM_OR_SIGN  | Krankheitssymptome. | 
| TIME  | Zeitangaben. Beispiel: „8 years“ (8 Jahre) oder „2:30AM this morning“ (heute Morgen 2:30 Uhr) |
| TREATMENT_NAME  | Name von Behandlungen. | 
| VARIANT  | Eine genetische Variante der Genentität | 

### <a name="relation-extraction"></a>Beziehungsextrahierung

Die Beziehungsextrahierung identifiziert sinnvolle Verbindungen zwischen Begriffen, die im Text erwähnt werden. Beispielsweise wird die Beziehung „Zeit der Bedingung“ durch Zuordnen eines Bedingungsnamens zu einem Zeitpunkt gefunden. Mit „Textanalyse für Gesundheit“ können die folgenden Beziehungen identifiziert werden:

* DIRECTION_OF_BODY_STRUCTURE  
* TIME_OF_CONDITION
* QUALIFIER_OF_CONDITION  
* DOSAGE_OF_MEDICATION 
* FORM_OF_MEDICATION  
* ROUTE_OR_MODE_OF_MEDICATION   
* STRENGTH_OF_MEDICATION 
* ADMINISTRATION_RATE_OF_MEDICATION   
* FREQUENCY_OF_MEDICATION 
* TIME_OF_MEDICATION 
* TIME_OF_TREATMENT 
* FREQUENCY_OF_TREATMENT  
* VALUE_OF_EXAMINATION
* UNIT_OF_EXAMINATION 
* RELATION_OF_EXAMINATION 
* TIME_OF_EXAMINATION  
* ABBREVIATION 
