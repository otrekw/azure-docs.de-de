---
title: Azure Batch-Analysen
description: Die Themen in „Batch-Analysen“ enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: 9d0e5c90c7bade82d76c7dbdfd4276e9671c961c
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2020
ms.locfileid: "85958105"
---
# <a name="batch-analytics"></a>Batch-Analysen
Die Themen in „Batch-Analysen“ enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.

Unter [Diagnoseprotokollierung für Azure Batch](./batch-diagnostics.md) finden Sie weitere Informationen zum Aktivieren und Nutzen von Batch-Diagnoseprotokollen.

## <a name="diagnostic-logs"></a>Diagnoseprotokolle

Der Azure Batch-Dienst gibt im Verlauf der Lebensdauer bestimmter Batch-Ressourcen die folgenden Diagnoseprotokollereignisse aus.

**Dienstprotokollereignisse**
* [Erstellung eines Pools](batch-pool-create-event.md)
* [Start des Löschvorgangs eines Pools](batch-pool-delete-start-event.md)
* [Abschluss des Löschvorgangs eines Pools](batch-pool-delete-complete-event.md)
* [Start der Größenänderung eines Pools](batch-pool-resize-start-event.md)
* [Abschluss der Größenänderung eines Pools](batch-pool-resize-complete-event.md)
* [Taskstart](batch-task-start-event.md)
* [Taskabschluss](batch-task-complete-event.md)
* [Taskfehler](batch-task-fail-event.md)
