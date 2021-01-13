---
title: 'Tasks und Anwendungen für die Nachrichtenreplikation: Azure Service Bus | Microsoft-Dokumentation'
description: Dieser Artikel enthält eine Übersicht über die Erstellung von Tasks und Anwendungen für die Nachrichtenreplikation mit Azure Functions.
ms.topic: article
ms.date: 12/12/2020
ms.openlocfilehash: 4db151f54a2ad236ba937b005ba6a1fd3edd5967
ms.sourcegitcommit: ad677fdb81f1a2a83ce72fa4f8a3a871f712599f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/17/2020
ms.locfileid: "97663452"
---
# <a name="message-replication-tasks-and-applications"></a>Tasks und Anwendungen für die Nachrichtenreplikation

Wie im Artikel [Nachrichtenreplikation und regionsübergreifender Verbund](service-bus-federation-overview.md) erläutert, basiert die Replikation von Nachrichtensequenzen zwischen Service Bus-Entitätspaaren sowie zwischen Service Bus und anderen Nachrichtenquellen und -zielen in der Regel auf Azure Functions.

[Azure Functions](../azure-functions/functions-overview.md) ist eine skalierbare und zuverlässige Ausführungsumgebung zum Konfigurieren und Ausführen serverloser Anwendungen. Hierzu zählen auch Tasks für [Nachrichtenreplikation und Verbund](service-bus-federation-overview.md).

In dieser Übersicht erfahren Sie mehr über die integrierten Funktionen von Azure Functions für solche Anwendungen, über Codeblöcke, die Sie für Transformationsaufgaben übernehmen und ändern können, sowie darüber, wie Sie eine Azure Functions-Anwendung so konfigurieren, dass sie ideal in Service Bus und andere Azure-Messagingdienste integriert werden kann. Bei vielen Details wird in diesem Artikel auf die Dokumentation von Azure Functions verwiesen.

[!INCLUDE [messaging-replicator-functions](../../includes/messaging-replicator-functions.md)]
