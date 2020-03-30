---
title: Dienstkonfiguration – QnA Maker
description: Erfahren Sie, wie und wo Ressourcen konfiguriert werden.
ms.topic: reference
ms.date: 02/21/2020
ms.openlocfilehash: 8ef6fecbfeb119d0c68ec5bc3bbc90ec449dbb7d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2020
ms.locfileid: "77651804"
---
# <a name="service-configuration"></a>Dienstkonfiguration

QnA Maker verwendet verschiedene Azure-Ressourcen (Dienste), einschließlich Cognitive Search, App Service, App Service Plan und Application Insights.

Alle Anpassungen an diesen Einstellungen, die von QnA Maker unterstützt werden, sind unten aufgeführt.

## <a name="app-service"></a>App Service

QnA Maker verwendet den App Service, um die von der [generateAnswer-API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer) verwendete Abfragelaufzeit bereitzustellen.


Diese Einstellungen sind im Azure-Portal für den App Service verfügbar. Die Einstellungen sind durch Auswahl von **Einstellungen** und dann **Konfiguration** verfügbar.

Sie können eine einzelne Einstellung entweder über die Liste der Anwendungseinstellungen festlegen oder mehrere Einstellungen durch Auswahl von **Erweiterte Bearbeitung** ändern.

|Resource|Einstellung|
|--|--|
|AzureSearchAdminKey|Cognitive Search: für QnA-Satzspeicher und Rangfolge #1 verwendet|
|AzureSearchName|Cognitive Search: für QnA-Satzspeicher und Rangfolge #1 verwendet|
|DefaultAnswer|Text der Antwort, wenn kein Treffer gefunden wird|
|UserAppInsightsAppId|Chatprotokoll und Telemetrie|
|UserAppInsightsKey|Chatprotokoll und Telemetrie|
|UserAppInsightsName|Chatprotokoll und Telemetrie|

Erfahren Sie, [wie Sie Ihren Cognitive Search-Dienst](./how-to/set-up-qnamaker-service-azure.md#configure-qna-maker-to-use-different-cognitive-search-resource) für Ihren Dienst ändern.

Sie müssen den Dienst über die Seite **Übersicht** des Azure-Portals **neu starten**, sobald Sie mit den Änderungen fertig sind.

## <a name="qna-maker-service"></a>QnA Maker Service

Der QnA Maker-Dienst stellt die Konfiguration für die folgenden Benutzer bereit, die an einem einzelnen QnA Maker-Dienst und all seinen Wissensdatenbanken zusammenarbeiten können.

Erfahren Sie, [wie Sie Projektmitarbeiter](./how-to/collaborate-knowledge-base.md) zu Ihrem Dienst hinzufügen können.

## <a name="application-insights"></a>Application Insights

Application Insights verfügt über keine speziellen Konfigurationseinstellungen für QnA Maker.

## <a name="app-service-plan"></a>App Service-Plan

App Service-Plan verfügt über keine speziellen Konfigurationseinstellungen für QnA Maker.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie mehr über [Formate](reference-document-format-guidelines.md) für Dokumente und URLs, die Sie in eine Wissensdatenbank importieren möchten.