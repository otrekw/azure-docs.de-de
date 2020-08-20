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
ms.openlocfilehash: 6880391fb54791fe5f597de2305d24f8c0e47ec6
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88122427"
---
## <a name="health-entity-categories"></a>Gesundheitsentitätskategorien:

Die folgenden Entitätskategorien werden von [Textanalyse für Gesundheit](../../how-tos/text-analytics-for-health.md) zurückgegeben.  Beachten Sie, dass in dieser Containervorschau nur englischer Text unterstützt wird und nur eine einzige Modellversion in jedem Containerimage bereitgestellt wird.

### <a name="named-entity-recognition"></a>Erkennung benannter Entitäten

|Category  |BESCHREIBUNG   |
|----------|--------------|
| Age | Altersangaben Beispiel: *30 Jahre alt*. |
| AdministrativeEvent | Ein administratives Ereignis. |
| BodyStructure | Teile des menschlichen Körpers einschließlich der Organe und anderer Strukturen. Beispiele: *Arm* oder *Herz*. | 
| CareEnvironment | Die Umgebung, in der die Behandlung oder Pflegemaßnahme stattfindet. Beispiel: *Notaufnahme*. | 
| ConditionQualifier | Abstufungen eines Krankheitszustands. Beispiele: *mild*, *schwer wiegend* oder *diffus*. | 
| Diagnosis | Medizinische Bedingungen. Beispiel: *Bluthochdruck* (Hypertonie). | 
| Direction | Genaue Position. Beispiele: *links* oder *vorne*. | 
| Dosage | Höhe einer Medikation. Beispiel: *25 mg*.  | 
| ExaminationName | Eine Untersuchungsmethode. Beispiel: *Röntgen*. | 
| RelationalOperator | Ein Operator, der eine Beziehung zwischen zwei Entitäten definiert. Beispiele: *weniger als* oder `>=`.  | 
| MeasurementUnit | Eine Maßeinheit (z. B. ein Prozentsatz). | 
| MeasurementValue | Der numerische Wert einer Maßeinheit. | 
| FamilyRelation | Eine familiäre Beziehung. Beispiel: *Schwester*.  | 
| Häufigkeit | Häufigkeit.   | 
| Geschlecht | Geschlechter. | 
| Gene | Eine Genentität, z. B. *TP53*.   | 
| HealthcareProfession | Methode zum Verwalten der Medikation. Beispiel: *orale Verabreichung*. | 
| MedicationClass | Medikationsklassen. Beispiel: *antibiotics* (Antibiotika).  | 
| MedicationForm | Eine Form der Medikation. Beispiel: *Kapsel*. | 
| MedicationName  | Generische und Markenmedikationen. Beispiel: *Ibuprofen*. | 
| MedicationRoute | Methode zum Verwalten der Medikation. Beispiel: *orale Verabreichung*. | 
| SymptomOrSign  | Krankheitssymptome. Beispiel: *Halsschmerzen*. | 
| Time | Zeitangaben. Beispiel: *8 Jahre* oder *heute Morgen 2:30 Uhr* |
| TreatmentName  | Name von Behandlungen. Beispiel: *Therapie*. | 
| Variant | Eine genetische Variante der Genentität. | 

### <a name="relation-extraction"></a>Beziehungsextrahierung

Die Beziehungsextrahierung identifiziert sinnvolle Verbindungen zwischen Begriffen, die im Text erwähnt werden. Beispielsweise wird die Beziehung „Zeit der Bedingung“ durch Zuordnen eines Bedingungsnamens zu einem Zeitpunkt gefunden. Mit „Textanalyse für Gesundheit“ können die folgenden Beziehungen identifiziert werden:

|Kategorie  |BESCHREIBUNG   |
|----------|--------------|
| DirectionOfBodyStructure | Informationen zu einer Körperstruktur. |
| DirectionOfCondition | Informationen zu einem Krankheitszustand. |
| DirectionOfExamination | Informationen zu einer Untersuchung. |
| DirectionOfTreatment | Informationen zu einer Behandlung. |
| TimeOfCondition | Der Zeitpunkt, zu dem ein Krankheitszustand eingetreten ist. |
| QualifierOfCondition | Der zugeordnete Qualifizierer eines Krankheitszustands. |
| DosageOfMedication | Eine Dosierung der Medikation. |
| FormOfMedication | Eine Form der Medikation. |
| RouteOfMedication | Eine Art der Verabreichung eines Medikaments. Beispiel: *oral*. |
| FrequencyOfMedication | Die Häufigkeit der Verabreichung einer Medikation. | 
| ValueOfCondition | Ein numerischer Wert, der einem Krankheitszustand zugeordnet ist. |
| UnitOfCondition | Eine Einheit (z. B. Uhrzeit), die einem Krankheitszustand zugeordnet ist. |
| TimeOfMedication | Der Zeitpunkt, zu dem eine Medikation verabreicht wurde. |
| TimeOfTreatment | Der Zeitpunkt, zu dem eine Behandlung erfolgte. | 
| FrequencyOfTreatment | Die Häufigkeit, mit der eine Behandlung erfolgte. |
| ValueOfExamination | Ein numerischer Wert, der einer Untersuchung zugeordnet ist. | 
| UnitOfExamination | Eine Einheit (z. B. ein Prozentsatz), die einer Untersuchung zugeordnet ist. |
| RelationOfExamination | Eine Beziehung zwischen einer Entität und einer Untersuchung. | 
| TimeOfExamination | Der Zeitpunkt, der einer Untersuchung zugeordnet ist. |
| Abkürzung | Eine Abkürzung.  | 
