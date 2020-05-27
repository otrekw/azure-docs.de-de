---
title: Azure Batch-Analysen
description: Die Themen in „Batch-Analysen“ enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.
ms.topic: reference
ms.date: 04/20/2017
ms.openlocfilehash: b07d362c3cc2d3b086cf0a82325025604907a393
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83726858"
---
# <a name="batch-analytics"></a>Batch-Analysen
Die Themen in „Batch-Analysen“ enthalten Referenzinformationen zu den Ereignissen und Warnungen, die für Batch-Dienstressourcen verfügbar sind.

Unter [Diagnoseprotokollierung für Azure Batch](https://azure.microsoft.com/documentation/articles/batch-diagnostics/) finden Sie weitere Informationen zum Aktivieren und Nutzen von Batch-Diagnoseprotokollen.

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