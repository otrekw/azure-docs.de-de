---
title: Aus dem ITSM-Produkt mit dem Log Analytics-Arbeitsbereich synchronisierte Daten
description: In diesem Artikel finden Sie eine Übersicht über die Daten, die von Ihrem ITSM-Produkt mit dem Log Analytics-Arbeitsbereich synchronisiert werden.
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/29/2020
ms.custom: references_regions
ms.openlocfilehash: 83994c1b6e150342a777a079d79d6a594d30c3ff
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2021
ms.locfileid: "102041602"
---
# <a name="data-synced-from-your-itsm-product"></a>Aus dem ITSM-Produkt synchronisierte Daten

Vorfälle und Änderungsanforderungen aus dem ITSM-Produkt werden mit Ihrem Log Analytics-Arbeitsbereich gemäß der Verbindungskonfiguration (im Feld „Daten synchronisieren“) synchronisiert:
* [ServiceNow](./itsmc-connections-servicenow.md)
* [System Center Service Manager](./itsmc-connections-scsm.md)
* [Cherwell](./itsmc-connections-cherwell.md)
* [Provance](./itsmc-connections-provance.md)

## <a name="synced-data"></a>Synchronisierte Daten

Der folgende Abschnitt enthält Beispiele für Daten, die vom ITSM-Connector gesammelt werden.

Die Felder in **ServiceDesk_CL** variieren abhängig vom Arbeitselementtyp, den Sie in Log Analytics importieren. Im Folgenden finden Sie eine Liste der Felder für zwei Arbeitselementtypen:

**Arbeitselement**: **Incidents**  
ServiceDeskWorkItemType_s="Incident"

**Fields**

- ServiceDeskConnectionName
- Service Desk-ID
- State
- Dringlichkeit
- Auswirkung
- Priority
- Eskalation
- Erstellt von
- Gelöst von
- Geschlossen von
- `Source`
- Zugewiesen zu
- Category
- Titel
- BESCHREIBUNG
- Erstellt am
- Geschlossen am
- Gelöst am
- Datum der letzten Änderung
- Computer

**Arbeitselement**: **Änderungsanforderungen**

ServiceDeskWorkItemType_s="ChangeRequest"

**Fields**
- ServiceDeskConnectionName
- Service Desk-ID
- Erstellt von
- Geschlossen von
- `Source`
- Zugewiesen zu
- Titel
- type
- Category
- State
- Eskalation
- Konfliktstatus
- Dringlichkeit
- Priority
- Risiko
- Auswirkung
- Zugewiesen zu
- Erstellt am
- Geschlossen am
- Datum der letzten Änderung
- Anforderungsdatum
- Geplantes Startdatum
- Geplantes Enddatum
- Startdatum der Arbeit
- Enddatum der Arbeit
- BESCHREIBUNG
- Computer

## <a name="servicenow-example"></a>ServiceNow-Beispiel 
### <a name="output-data-for-a-servicenow-incident"></a>Ausgabedaten für einen ServiceNow-Incident

| Log Analytics-Feld | ServiceNow-Feld |
|:--- |:--- |
| ServiceDeskId_s| Number |
| IncidentState_s | State |
| Urgency_s |Dringlichkeit |
| Impact_s |Auswirkung|
| Priority_s | Priority |
| CreatedBy_s | Geöffnet von |
| ResolvedBy_s | Gelöst von|
| ClosedBy_s  | Geschlossen von |
| Source_s| Kontakttyp |
| AssignedTo_s | Zugewiesen zu  |
| Category_s | Category |
| Title_s|  Kurze Beschreibung |
| Description_s|  Notizen |
| CreatedDate_t|  Geöffnet |
| ClosedDate_t| closed|
| ResolvedDate_t|Gelöst|
| Computer  | Konfigurationselement |

### <a name="output-data-for-a-servicenow-change-request"></a>Ausgabedaten für eine ServiceNow-Änderungsanforderung

| Log Analytics | ServiceNow-Feld |
|:--- |:--- |
| ServiceDeskId_s| Number |
| CreatedBy_s | Angefordert von |
| ClosedBy_s | Geschlossen von |
| AssignedTo_s | Zugewiesen zu  |
| Title_s|  Kurze Beschreibung |
| Type_s|  type |
| Category_s|  Category |
| CRState_s|  State|
| Urgency_s|  Dringlichkeit |
| Priority_s| Priority|
| Risk_s| Risiko|
| Impact_s| Auswirkung|
| RequestedDate_t  | Datum der Anforderung |
| ClosedDate_t | Geschlossen am |
| PlannedStartDate_t  | Geplantes Startdatum |
| PlannedEndDate_t  | Geplantes Enddatum |
| WorkStartDate_t  | Tatsächliches Startdatum |
| WorkEndDate_t | Tatsächliches Enddatum|
| Description_s | BESCHREIBUNG |
| Computer  | Konfigurationselement |

## <a name="next-steps"></a>Nächste Schritte

* [Problembehandlung in ITSM-Connector](./itsmc-resync-servicenow.md)
