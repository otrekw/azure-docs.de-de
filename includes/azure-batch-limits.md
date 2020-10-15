---
title: include file
description: include file
services: batch
author: JnHs
ms.service: batch
ms.topic: include
ms.date: 06/03/2020
ms.author: jenhayes
ms.custom: include file
ms.openlocfilehash: 2a35180e4c5ca26d53be135718e345f9657af6a2
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "88655180"
---
| **Ressource** | **Standardlimit** | **Maximales Limit** |
| --- | --- | --- |
| Azure Batch-Konten pro Region und Abonnement | 1-3 |50 |
| Dedizierte Kerne pro Batch-Konto | 90-900 | Support kontaktieren |
| Kerne mit niedriger Priorität pro Batch-Konto | 10-100 | Support kontaktieren |
| **[Aktive](https://docs.microsoft.com/rest/api/batchservice/job/get#jobstate)** Aufträge und Auftragszeitpläne pro Batch-Konto (**abgeschlossene** Aufträge haben keinen Grenzwert) | 100-300 | 1\.000<sup>1</sup> |
| Pools pro Batch-Konto | 20-100 | 500<sup>1</sup> |

<sup>1</sup>Wenden Sie sich an den Azure-Support, wenn Sie eine Heraufsetzung dieses Grenzwerts wünschen.

> [!IMPORTANT]
> Wir ändern die Art und Weise, in der Sie dedizierte Kontingente anfordern und verwalten.  Der Gesamtwert für dedizierte vCPUs ist der aktuell gültige Wert, in Kürze werden wir jedoch dedizierte Kontingente pro VM-Serie einrichten. Kontingente mit niedriger Priorität werden weiterhin basierend auf dem Gesamtgrenzwert erzwungen, nicht pro VM-Serie.

> [!NOTE]
> Die Standardgrenzwerte variieren abhängig vom Abonnementtyp, den Sie zum Erstellen eines Batch-Kontos verwenden. Die angezeigten Kontingente für Kerne gelten für Batch-Konten im Batch-Dienstmodus. [Zeigen Sie die Kontingente in Ihrem Batch-Konto an](../articles/batch/batch-quota-limit.md#view-batch-quotas).

> [!IMPORTANT]
> Damit die Kapazität während der globalen Pandemie besser verwaltet werden kann, wurden die Standardkernkontingente für neue Batch-Konten in einigen Regionen sowie für einige Abonnementtypen in den oben genannten Wertebereichen reduziert, in manchen Fällen sogar auf null Kerne. Wenn Sie ein neues Batch-Konto erstellen, [überprüfen Sie Ihr Kernkontingent](../articles/batch/batch-quota-limit.md#view-batch-quotas) und fordern Sie ggf. eine [Erhöhung des Kernkontingents](../articles/batch/batch-quota-limit.md#increase-a-quota) an. Alternativ können Sie auch Batch-Konten wiederverwenden, die bereits über ein ausreichendes Kontingent verfügen. 
