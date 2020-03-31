---
title: Grenzwerte, Kontingente und Schwellenwerte in Azure Scheduler
description: Informationen zu Grenzwerten, Kontingenten, Standardwerten und Drosselungsschwellenwerten für Azure Scheduler
services: scheduler
ms.service: scheduler
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: article
ms.date: 08/18/2016
ms.openlocfilehash: 610232dab776648bb3dcc7c301ec292e9acad9fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898526"
---
# <a name="limits-quotas-and-throttle-thresholds-in-azure-scheduler"></a>Grenzwerte, Kontingente und Drosselungsschwellenwerte in Azure Scheduler

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ersetzt den Microsoft Azure Scheduler, der [ eingestellt wird](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Wenn Sie weiterhin mit den Aufträgen arbeiten möchten, die Sie in Scheduler eingerichtet haben, sollten Sie so bald wie möglich [zu Azure Logic Apps migrieren](../scheduler/migrate-from-scheduler-to-logic-apps.md). 
>
> Scheduler ist nicht mehr in der Azure-Portal verfügbar, aber die [REST-API](/rest/api/scheduler) und [Azure Scheduler-PowerShell-Cmdlets](scheduler-powershell-reference.md) sind weiterhin verfügbar, damit Sie Ihre Aufträge und Auftragssammlungen verwalten können.

## <a name="limits-quotas-and-thresholds"></a>Grenzwerte, Kontingente und Schwellenwerte

[!INCLUDE [scheduler-limits-table](../../includes/scheduler-limits-table.md)]

## <a name="x-ms-request-id-header"></a>x-ms-request-id header

Jede Anforderung für den Scheduler-Dienst gibt einen Antwortheader namens **x-ms-request-id**zurück. Dieser Header enthält einen opaken Wert, der die Anforderung eindeutig identifiziert. Wenn also eine Anforderung immer wieder fehlschlägt und Sie bestätigt haben, dass die Anforderung ordnungsgemäß formatiert ist, können Sie den Fehler an Microsoft melden, indem Sie den Antwortheaderwert **x-ms-request id** und die folgenden Details angeben: 

* Den **x-ms-request-id**-Wert
* Die ungefähre Zeit, zu der die Anforderung vorgenommen wurde 
* Die Bezeichner für das Azure-Abonnement, die Auftragssammlung und den Auftrag 
* Die Art des Vorgangs, den die Anforderung versucht hat

## <a name="next-steps"></a>Nächste Schritte

* [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)
* [Pläne und Abrechnung für Microsoft Azure Scheduler](scheduler-plans-billing.md)
* [Azure Scheduler-REST-API – Referenz](/rest/api/scheduler)
* [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)