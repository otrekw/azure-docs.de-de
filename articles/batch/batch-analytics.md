---
title: Azure Batch-Analysen
description: Die Themen in „Batch-Analysen“ enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.
ms.topic: reference
ms.date: 10/08/2020
ms.openlocfilehash: 0d55ecd7f10e267a9cb469dffcdf26c131c8ce41
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "91849510"
---
# <a name="batch-analytics"></a>Batch-Analysen

Die Themen in diesem Abschnitt enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.

Unter [Diagnoseprotokollierung für Azure Batch](./batch-diagnostics.md) finden Sie weitere Informationen zum Aktivieren und Nutzen von Batch-Diagnoseprotokollen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Der Azure Batch-Dienst gibt im Verlauf der Lebensdauer bestimmter Batch-Ressourcen die folgenden Diagnoseprotokollereignisse aus.

### <a name="service-log-events"></a>Dienstprotokollereignisse

- [Erstellung eines Pools](batch-pool-create-event.md)
- [Start des Löschvorgangs eines Pools](batch-pool-delete-start-event.md)
- [Abschluss des Löschvorgangs eines Pools](batch-pool-delete-complete-event.md)
- [Start der Größenänderung eines Pools](batch-pool-resize-start-event.md)
- [Abschluss der Größenänderung eines Pools](batch-pool-resize-complete-event.md)
- [Autoskalierung von Pools](batch-pool-autoscale-event.md)
- [Taskstart](batch-task-start-event.md)
- [Taskabschluss](batch-task-complete-event.md)
- [Taskfehler](batch-task-fail-event.md)
- [Taskzeitplan-Fehler](batch-task-schedule-fail-event.md)
