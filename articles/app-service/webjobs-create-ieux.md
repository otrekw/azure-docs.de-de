---
title: Ausführen von Hintergrundaufgaben mit WebJobs
description: Erfahren Sie, wie Sie mithilfe von WebJobs Hintergrundaufgaben in Azure App Service ausführen. Wählen Sie aus einer Vielzahl von Skriptformaten aus, und führen Sie sie mit Cron-Ausdrücken aus.
author: ggailey777
ms.assetid: af01771e-54eb-4aea-af5f-f883ff39572b
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: glenga
ms.reviewer: msangapu;suwatch;pbatum;naren.soni
ms.custom: seodec18
zone_pivot_groups: app-service-webjob
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0d49323e2bc3c0522b1fb9ad49ffcc14f476e2dc
ms.sourcegitcommit: 6386854467e74d0745c281cc53621af3bb201920
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2021
ms.locfileid: "102452797"
---
# <a name="run-background-tasks-with-webjobs-in-azure-app-service"></a>Ausführen von Hintergrundaufgaben mit WebJobs in Azure App Service

Das Konzept der Ausführung von [Hintergrundaufgaben](./webjobs-create-ieux-conceptual.md) in Azure wird mit Azure App Service-WebJobs bereitgestellt. Hier erfahren Sie mehr über die Bereitstellung von <abbr title="Dabei handelt es sich um ein Programm oder Skript in derselben Instanz wie eine Web-App, API-App oder mobile App.">WebJobs</abbr> mithilfe des [Azure-Portals](https://portal.azure.com), um eine ausführbare Datei oder ein Skript hochzuladen. 

Zu den drei unterstützten Webaufträgen gehören:

* **Fortlaufende WebJobs:** Diese werden sofort gestartet und in der Regel in einer Endlosschleife ausgeführt.
* **Geplante WebJobs:** Diese werden durch einen Plantrigger gestartet.
* **Manuelle WebJobs:** Diese werden durch einen manuellen Trigger gestartet.

::: zone pivot="webjob-type-continuous"

[!INCLUDE [Continuous](./includes/webjobs-create-ieux-continuous.md)]

::: zone-end

::: zone pivot="webjob-type-scheduled"

[!INCLUDE [Scheduled](./includes/webjobs-create-ieux-scheduled.md)]

::: zone-end

::: zone pivot="webjob-type-manual"

[!INCLUDE [Manual](./includes/webjobs-create-ieux-manual.md)]

::: zone-end
   
## <a name="next-steps"></a><a name="NextSteps"></a> Nächste Schritte

* [Weitere Informationen über Hintergrundaufgaben als WebJobs](./webjobs-create-ieux-conceptual.md)
* [Anzeigen des Protokollverlaufs von WebJobs](./webjobs-create-ieux-view-log.md)

* Verwenden des [WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) zum Vereinfachen vieler Programmieraufgaben

* [Entwickeln und Bereitstellen von WebJobs mit Visual Studio](webjobs-dotnet-deploy-vs.md)
