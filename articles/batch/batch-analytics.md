---
title: Analysen in Azure Batch | Microsoft-Dokumentation
description: Referenz zu Analysen in Azure Batch.
services: batch
author: ju-shim
manager: gwallace
ms.assetid: ''
ms.service: batch
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: big-compute
ms.date: 04/20/2017
ms.author: jushiman
ms.openlocfilehash: 39a8bfb6a48bf55ae9f2ec36f7716959e6ada9dd
ms.sourcegitcommit: dbcc4569fde1bebb9df0a3ab6d4d3ff7f806d486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/15/2020
ms.locfileid: "76027386"
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