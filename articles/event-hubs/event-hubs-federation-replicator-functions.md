---
title: 'Ereignisreplikationstasks und Anwendungen: Azure Event Hubs | Microsoft-Dokumentation'
description: Dieser Artikel bietet eine Übersicht über das Entwickeln von Ereignisreplikationstasks und -Anwendungen mit Azure Functions.
ms.topic: article
ms.date: 12/01/2020
ms.openlocfilehash: a65815c7da400af8b5b6d46358e6bca6edbd7543
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/29/2021
ms.locfileid: "97663521"
---
# <a name="event-replication-tasks-and-applications-with-azure-functions"></a>Ereignisreplikationstasks und -Anwendungen mit Azure Functions

> [!TIP]
> Verwenden Sie für alle zustandsbehafteten Replikationstasks, bei denen Sie die Nutzlasten ihrer Ereignisse berücksichtigen und diese transformieren, aggregieren, anreichern oder reduzieren müssen, [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) anstelle von Azure Functions.

Wie im Artikel [Ereignisreplikation und regionsübergreifender Verbund](event-hubs-federation-overview.md) erläutert wird, stützt sich die zustandslose Replikation von Ereignisdatenströmen zwischen Paaren von Event Hubs und zwischen Event Hubs und anderen Ereignis-Datenstromquellen und -zielen auf Azure Functions.

[Azure Functions](../azure-functions/functions-overview.md) ist eine skalierbare und zuverlässige Ausführungsumgebung zum Konfigurieren und Ausführen serverloser Anwendungen, einschließlich Ereignisreplikation und Verbundtasks.

In dieser Übersicht erfahren Sie mehr über die integrierten Funktionen von Azure Functions für solche Anwendungen, über Codeblöcke, die Sie für Transformationsaufgaben übernehmen und ändern können, sowie darüber, wie Sie eine Azure Functions-Anwendung so konfigurieren, dass sie ideal in Event Hubs und andere Azure-Messagingdienste integriert werden kann. Bei vielen Details wird in diesem Artikel auf die Dokumentation von Azure Functions verwiesen.

[!INCLUDE [messaging-replicator-functions](../../includes/messaging-replicator-functions.md)]









