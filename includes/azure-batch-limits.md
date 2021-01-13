---
title: include file
description: include file
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 12/16/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 0a94b122f1cdd598eeac553c8cc784d2a0a5369f
ms.sourcegitcommit: 86acfdc2020e44d121d498f0b1013c4c3903d3f3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97614511"
---
| **Ressource** | **Standardlimit** | **Maximales Limit** |
| --- | --- | --- |
| Azure Batch-Konten pro Region und Abonnement | 1-3 |50 |
| Dedizierte Kerne pro Batch-Konto | 90-900 | Support kontaktieren |
| Kerne mit niedriger Priorität pro Batch-Konto | 10-100 | Support kontaktieren |
| **[Aktive](/rest/api/batchservice/job/get#jobstate)** Aufträge und Auftragszeitpläne pro Batch-Konto (**abgeschlossene** Aufträge haben keinen Grenzwert) | 100-300 | 1\.000<sup>1</sup> |
| Pools pro Batch-Konto | 20-100 | 500<sup>1</sup> |

<sup>1</sup>Wenden Sie sich an den Azure-Support, wenn Sie eine Heraufsetzung dieses Grenzwerts wünschen.

> [!NOTE]
> Die Standardgrenzwerte variieren abhängig vom Abonnementtyp, den Sie zum Erstellen eines Batch-Kontos verwenden. Die angezeigten Kontingente für Kerne gelten für Batch-Konten im Batch-Dienstmodus. [Zeigen Sie die Kontingente in Ihrem Batch-Konto an](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Damit die Kapazität während der globalen Pandemie besser verwaltet werden kann, wurden die Standardkernkontingente für neue Batch-Konten in einigen Regionen sowie für einige Abonnementtypen in den oben genannten Wertebereichen reduziert, in manchen Fällen sogar auf null Kerne. Wenn Sie ein neues Batch-Konto erstellen, [überprüfen Sie Ihr Kernkontingent](../articles/batch/batch-quota-limit.md#view-batch-quotas) und fordern Sie ggf. eine [Erhöhung des Kernkontingents](../articles/batch/batch-quota-limit.md#increase-a-quota) an. Alternativ können Sie auch Batch-Konten wiederverwenden, die bereits über ein ausreichendes Kontingent verfügen.