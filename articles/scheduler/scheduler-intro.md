---
title: Was ist Azure Scheduler?
description: Erstellen, Planen und Ausführen automatisierter Aufträge, die Dienste innerhalb und außerhalb von Azure aufrufen
services: scheduler
ms.service: scheduler
ms.suite: infrastructure-services
author: derek1ee
ms.author: deli
ms.reviewer: klam, estfan
ms.topic: conceptual
ms.date: 02/17/2020
ms.openlocfilehash: e851da0013cf3a9ff6bb1a0fc1c073b5b796c54d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "78898538"
---
# <a name="what-is-azure-scheduler"></a>Was ist Azure Scheduler?

> [!IMPORTANT]
> [Azure Logic Apps](../logic-apps/logic-apps-overview.md) ersetzt den Microsoft Azure Scheduler, der [ eingestellt wird](../scheduler/migrate-from-scheduler-to-logic-apps.md#retire-date). Wenn Sie weiterhin mit den Aufträgen arbeiten möchten, die Sie in Scheduler eingerichtet haben, sollten Sie so bald wie möglich [zu Azure Logic Apps migrieren](../scheduler/migrate-from-scheduler-to-logic-apps.md). 
>
> Scheduler ist nicht mehr in der Azure-Portal verfügbar, aber die [REST-API](/rest/api/scheduler) und [Azure Scheduler-PowerShell-Cmdlets](scheduler-powershell-reference.md) sind weiterhin verfügbar, damit Sie Ihre Aufträge und Auftragssammlungen verwalten können.

[Azure Scheduler](https://azure.microsoft.com/services/scheduler/) unterstützt Sie durch deklarative Beschreibung von Aktionen beim Erstellen von [Aufträgen](../scheduler/scheduler-concepts-terms.md), die in der Cloud ausgeführt werden. Anschließend werden diese Aktionen vom Dienst automatisch geplant und ausgeführt. Sie können beispielsweise Dienste inner- und außerhalb von Azure aufrufen (z. B. HTTP- oder HTTPS-Endpunkte) und zudem Nachrichten in Azure Storage-Warteschlangen sowie Azure Service Bus-Warteschlangen oder -Themen veröffentlichen. Sie können Aufträge sofort oder zu einem späteren Zeitpunkt ausführen. [Komplexe Zeitpläne und erweiterte Serien](../scheduler/scheduler-advanced-complexity.md) werden von Scheduler problemlos unterstützt. Scheduler legt fest, wann Aufträge ausgeführt werden, speichert einen Verlauf der Auftragsergebnisse, den Sie überprüfen können, und plant die auszuführenden Workloads dann auf vorhersehbare und zuverlässige Weise.

Andere Azure-Planungsfunktionen nutzen Scheduler ebenfalls im Hintergrund, beispielsweise der Dienst [Azure WebJobs](../app-service/webjobs-create.md), bei dem es sich um eine [Web-Apps](https://azure.microsoft.com/services/app-service/web/)-Funktion von Azure App Service handelt. Sie können die Kommunikation für diese Aktionen mithilfe der [Scheduler-REST-API](https://docs.microsoft.com/rest/api/scheduler/) verwalten.

Im Folgenden sind einige Szenarien aufgeführt, in denen Scheduler hilfreich sein kann:

* Ausführen wiederkehrender App-Aktionen: Ein Beispiel hierfür ist die regelmäßige Erfassung von Daten aus Twitter in einem Feed.

* Ausführen täglicher Wartungsaufgaben: Beispiele hierfür sind die tägliche Bereinigung von Protokollen, die Erstellung von Sicherungen und die Ausführung anderer Wartungsaufgaben.

  Ein Administrator kann beispielsweise planen, dass seine Datenbank in den kommenden neun Monaten täglich um 1:00 Uhr gesichert wird.

Der Scheduler-Dienst kann zwar zum Erstellen, Verwalten und Ausführen von geplanten Workloads verwendet werden, er hostet aber weder Workloads noch führt er Code aus. Der Dienst *ruft* lediglich an anderer Stelle (beispielsweise in Azure, lokal oder bei einem anderen Anbieter) gehostete(n) Dienste oder Code auf. Scheduler kann den Aufruf über HTTP, HTTPS, eine Storage-Warteschlange, eine Service Bus-Warteschlange oder ein Service Bus-Thema ausführen.

Zum Erstellen, Planen, Verwalten, Aktualisieren oder Löschen von Aufträgen und [Auftragssammlungen](../scheduler/scheduler-concepts-terms.md) können Sie Code, die [Scheduler-REST-API](https://docs.microsoft.com/rest/api/scheduler/) oder die [Azure Scheduler-PowerShell-Cmdlets](scheduler-powershell-reference.md) verwenden.

## <a name="next-steps"></a>Nächste Schritte

* [Konzepte, Terminologie und Entitätshierarchie für Azure Scheduler](scheduler-concepts-terms.md)
* [Pläne und Abrechnung für Microsoft Azure Scheduler](scheduler-plans-billing.md)
* [Erstellen komplexer Zeitpläne und erweiterter Serien mit Azure Scheduler](scheduler-advanced-complexity.md)
* [Azure Scheduler-REST-API – Referenz](/rest/api/scheduler)
* [Azure Scheduler – PowerShell-Cmdlets-Referenz](scheduler-powershell-reference.md)
