---
title: 'Metrics Advisor-Clientbibliotheken: REST-API'
titleSuffix: Azure Cognitive Services
description: In dieser Schnellstartanleitung erfahren Sie, wie Sie Ihre Anwendungen mit der Metrics Advisor-API von Azure Cognitive Services verbinden.
services: cognitive-services
author: mrbullwinkle
manager: nitinme
ms.service: applied-ai-services
ms.subservice: metrics-advisor
ms.topic: quickstart
ms.date: 07/06/2021
ms.author: mbullwin
zone_pivot_groups: programming-languages-metrics-monitor
ms.openlocfilehash: 7f7f9c69de17779f483ce4e4155a8953f9c9da91
ms.sourcegitcommit: 192444210a0bd040008ef01babd140b23a95541b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2021
ms.locfileid: "114341805"
---
# <a name="quickstart-use-the-client-libraries-or-rest-apis-to-customize-your-solution"></a>Schnellstart: Verwenden der Clientbibliotheken oder REST-APIs zum Anpassen Ihrer Lösung

Hier finden Sie Informationen zu den ersten Schritten mit der Metrics Advisor-REST-API und den Clientbibliotheken. Führen Sie die nachfolgenden Schritte zum Installieren des Pakets aus, und testen Sie den Beispielcode für grundlegende Aufgaben.

Metrics Advisor ermöglicht Folgendes:

* Hinzufügen eines Datenfeeds aus einer Datenquelle
* Überprüfen des Erfassungsstatus
* Konfigurieren von Erkennung und Warnungen 
* Abfragen der Ergebnisse der Anomalieerkennung
* Diagnostizieren von Anomalien


::: zone pivot="programming-language-csharp"

[!INCLUDE [REST API quickstart](../includes/quickstarts/csharp.md)]

::: zone-end

::: zone pivot="programming-language-java"

[!INCLUDE [REST API quickstart](../includes/quickstarts/java.md)]

::: zone-end

::: zone pivot="programming-language-javascript"

[!INCLUDE [REST API quickstart](../includes/quickstarts/javascript.md)]

::: zone-end

::: zone pivot="programming-language-python"

[!INCLUDE [REST API quickstart](../includes/quickstarts/python.md)]

::: zone-end

::: zone pivot="programming-language-rest-api"

[!INCLUDE [REST API quickstart](../includes/quickstarts/rest-api.md)]

::: zone-end

## <a name="clean-up-resources"></a>Bereinigen von Ressourcen

Wenn Sie ein Cognitive Services-Abonnement bereinigen und entfernen möchten, können Sie die Ressource oder die Ressourcengruppe löschen. Wenn Sie die Ressourcengruppe löschen, werden auch alle anderen Ressourcen gelöscht, die ihr zugeordnet sind.

* [Portal](../../../cognitive-services/cognitive-services-apis-create-account.md#clean-up-resources)
* [Azure-Befehlszeilenschnittstelle](../../../cognitive-services/cognitive-services-apis-create-account-cli.md#clean-up-resources)

## <a name="next-steps"></a>Nächste Schritte

- [Verwenden des Webportals](web-portal.md)
- [Durchführen des Onboardings für Ihre Datenfeeds](../how-tos/onboard-your-data.md)
    - [Verwalten von Datenfeeds](../how-tos/manage-data-feeds.md)
    - [Konfigurationen für unterschiedliche Datenquellen](../data-feeds-from-different-sources.md)
- [Konfigurieren von Metriken und Optimieren der Erkennungskonfiguration](../how-tos/configure-metrics.md)
- [Anpassen der Anomalieerkennung anhand von Feedback](../how-tos/anomaly-feedback.md)