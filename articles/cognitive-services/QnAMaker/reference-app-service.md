---
title: Dienstkonfiguration – QnA Maker
description: Erfahren Sie, wie und wo Ressourcen konfiguriert werden.
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: reference
ms.date: 02/21/2020
ms.openlocfilehash: 1d54b912d2177a3ccd0cf34d57fc0358af653199
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/09/2020
ms.locfileid: "91776713"
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
|AzureSearchAdminKey|Cognitive Search: wird für QnA-Paarspeicher und Bewerter 1 verwendet|
|AzureSearchName|Cognitive Search: wird für QnA-Paarspeicher und Bewerter 1 verwendet|
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